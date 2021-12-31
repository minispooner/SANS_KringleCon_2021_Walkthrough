## Summary
This challenge gives you some experience using grep.

## Steps
Save the outputs into files so you can print each answer later so you can quickly enter each into the `quizme` prompts.
1. `cat bigscan.gnmap | grep "34.76.1.22" > 1.txt` 
2. `cat bigscan.gnmap | grep "34.77.207.226" > 2.txt`
3. `cat bigscan.gnmap | grep "Status: Up" | wc -l > 3.txt` or `tail -n 1 bigscan.gnmap > 3.txt`
4. You can do multiple ANDs using `\|`, like `grep "test one\|another test\|the last one"`, or you could use a regex pattern like `grep -e "[80,443,8080]/open/tcp"`
5. Do a diff check between count(all UPs) and count(all hosts w at least 1 open port). (A-B=Answer...which was about 409 if I remember correctly)
6. Grep has a couple options to count occurrences per line. I think they were `-n` and `-o`. Use these, along with `sort` and `uniq` to count occurrences and get the greatest count of "open/tcp" per line. I think the answer was 12.
