# Summary
In Santa's Courtyard, help Piney Sappington find which files were altered. Practice with exiftool!


## Steps
for i in *; do exiftool $i; done > out.txt
cat out.txt | grep -i modified
cat out.txt | grep -i jack -B 40 | grep "\.docx"
