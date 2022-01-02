## Summary
What's the secret access key for for https://apply.jackfrosttower.com/ ? Practice web app hacking - in particular of one type...

## Steps
1. Get to know the app, the source code, the functionality. What developer expectations do you need to invalidate?

<details>
  <summary>Hint 1 - The objective outline tells you to go to Jack's bathroom for some hints. Here's my summary of that hint...</summary>
There's a bonus training terminal in Jacks bathroom that teaches you about cloud provider metadata IP and endpoint (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instancedata-data-retrieval.html). Go there for practice and learning, else take this hint ;)
</details>

<details>
  <summary>Hint 2</summary>
  Based on my previous hint and assuming you've easily found the only page in the web app that has any functionality, it's pretty clear where and what you'll be hacking. You'll attempt SSRF in the Apply form in order to obtain the AWS Secret Key from the cloud IP/metadata service. The user-supplied target destination, aka injection point, is clearly the "URL to your public NLBI report" field. But how to see the response data?...
</details>

<details>
  <summary>Hint 3</summary>
  After submitting your payloaded form, take a close look at the HTTP Response and see if anything is a direct result of your form submission. Something's fishy here. Look REAL close and you'll know what comes next. Think through what the backend may be doing with your form submission - between the two fields your working with.
</details>

<details>
  <summary>Hint 4</summary>
  The backend appears to be creating and referencing a new image file based on your given name. But you didn't provide an image, right? So where are they getting that "image" content data from? Think through what the developers may have intended. Remember, we know this is SSRF, so part of the web app logic (data retreival) is dependent on user-supplied data.
</details>

<details>
  <summary>Hint 5</summary>
  If using a browser, you'll see your image request call returns 200 Success but doesn't load anything (Firefox claims it "cannot be displayed because it contains errors"). That's because it's not renderable image data...Try using curl to view the raw HTTP artifact response data. You probably won't see this image loading failure in BurpSuite or using curl unless you've changed some default settings etc. 
</details>

<details>
  <summary>Answer</summary>
  Submit the form with:
  
  "Name" = whatever
  
  "URL to your public NLBI report" = "http://169.254.169.254/latest/meta-data/"
  
  Curl your image URL and you'll be able to see the SSRF result. The curl result will display 2 dirs (be careful to not overlook any results), so you now know which dir to append to your SSRF injection. Repeat the SSRF call and the append a few times until you find the AWS secret key.
</details>

<details>
  <summary>The Probable Assumptions of the Developer, Explained</summary>
  It's likely that the developer expected to have the web app make an API call to the NPPD site (http://nppd.northpolechristmastown.com/NLBI/YourReportIdGoesHere) in order to retrieve an image of you and save that result as your name.jpg. The developer forgot to restrict that NPPD URL input field, thus we are able to instead call the metadata service and set that response as our image file. So when we read the image file, we see the SSRF response. We loop over this, submitting the form about 4 times, but each time, updating the SSRF URL based on the previous HTTP Response (which dir to SSRF into next).
</details>
