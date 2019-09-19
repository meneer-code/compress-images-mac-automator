# compress-images-mac-automator
Quickly Compress Images with Automator (mac)

## Instructions:

1. Fire up Automator (CMD + SPACE, “automator”, ENTER)
2. File > New, Select “Quick Action”.
3. In the search field search for “run shell script”, drag the item to the right area.
4. Setup like this:
	- “Workflow receives current” = “image files”
	- “in” = “finder”
	- “Shell” = “/bin/bash/”
	- “Pass input” = “as arguments”
5. Paste the following code into the text field below (delete the example code before)
```
for f in "$@"
do

   echo $f | while IFS= read file
   do
filename=$(basename $file)
ext=$(echo ${filename##*.} | tr "[:upper:]" "[:lower:]")
if [ -f $file ]
then
   if ( [ $ext == "png" ] || [ $ext == "jpg" ] || [ $ext == "jpeg" ] )
then
JSON=curl -i --user api:APIKEY --data-binary @$file https://api.tinypng.com/shrink 2>/dev/null  
URL=${JSON/*url\":\"/}
URL=${URL/\"*/}

curl $URL>${file} 2>/dev/null
fi
fi
done

done

afplay /System/Library/Sounds/Submarine.aiff
```
6. [Request a API Key from TinyPNG](https://tinypng.com/developers)
7. Click the Link in the Email TinyPNG sends you.
8. Copy the API Key and replace it in the code fragment where it states APIKEY.
9. File > Save!
10. Name the service how you want it to show up in the context menu. (Example: Compress Images).
11. Done! Right click on an image file in the finder an hit “Compress Images” to let the magic happen!


![Alt text](/automator-tutorial.png "Automator Mac Compress Images")