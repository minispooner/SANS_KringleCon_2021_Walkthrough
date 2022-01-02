## Summary
What's the secret access key for for https://apply.jackfrosttower.com/ ? Practice web app hacking - in particular of one type...

## Steps
1. Get to know the app, the source code, the functionality. N

Hint 1
The objective outline tells you to go to Jack's bathroom for some hints. Here's my summary of that hint...
There's a bonus training terminal in Jacks bathroom that teaches you about cloud provider metadata IP and endpoint (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html). Go there for practice and learning, else take this hint :)

Hint 2
Based on my previous hint and assuming you've easily found the only page in the web app that has any functionality, it's pretty clear where and what you'll be hacking. You'll attempt SSRF to get the AWS Secret Key from the cloud IP/metadata in the Apply form under the "URL to your public NLBI report" field. But how?...

Hint 3
After submitting your payloaded form a few times, you should notice something changing in the HTTP Response each time. Think through what the backend may be doing with your form submission - between the two fields your working with.

Hint 4
The backend appears to be creating and referencing a new image file based on your given name. But you didn't provide an image, right? So where are they getting that "image" content data from? Think through what the developers may have intended. Remember, we know this is SSRF, so part of the web app logic (data retreival) is dependent on user-supplied data.

Hint 5
BurpSuite is tricky with this one. Try curl to view HTTP Responses.

Answer
Submit the form with:
"Name" = whatever
"URL to your public NLBI report" = "http://169.254.169.254/latest/meta-data/"
If using a brower, you'll see your image doesn't load, because it's not renderable image data - it's the meta-data response data! You won't see this img failure in BurpSuite or using curl. Now, curl your image URL and you'll be able to see the SSRF result (the brower attempts to render it and will fail). The curl result will display 2 dirs (careful not to overlook any results), so you now know which dir to append to your SSRF injection. Repeat a few times until you find the AWS secret keys.


The Probable Assumptions of the Developer, Explained
It's likely that the developer expected to have the web app make an API call to the NPPD site (http://nppd.northpolechristmastown.com/NLBI/YourReportIdGoesHere) in order to retrieve an image of you. The developer forgot to restrict that NPPD URL input field, thus we are able to instead call the metadata service and set that response as our image file. So when we read the image file, we see the SSRF response. We loop over this, submitting the form about 4 times, but each time, updating the SSRF URL based on the previous HTTP Response (which dir to SSRF into next).
