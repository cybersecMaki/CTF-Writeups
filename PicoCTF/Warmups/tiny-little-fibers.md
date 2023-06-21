```
Title: tiny little fibers
Points: 340
Author: John Hammond
Description: "Oh wow, it's another of everyone's favorite. But we like to try and turn the ordinary into extraordinary!"
```

Upon opening up the challenge, we're greeted with the description and a file called "tiny-little-fibers". 
Already, the challenge seems to be hinting to the 'strings' command. 

After downloading the file, the first thing we want to do is check the file type by using the file command.

```
 file tiny-little-fibers
```

The file turns out to be a JPEG image. Could there be hidden files within the image? Let's use binwalk to find out.

```
binwalk -e tiny--little-fibers
```

Unfortunately, there turns out to be no hidden files within the JPEG - Just a copyright. 
Next up, we'll use "strings" to extract the readable characters from the file, hopefully leading to an easy flag.

``` 
strings tiny-little-fibers > fibers.txt | grep "flag"
```

But, nothing to be had, on the surface at least. Let's take a closer look at the output of the strings command to see what we're dealing with.

```
vim fibers.txt
``` 

Here, we can see a file that contains 8326 lines of random characters separated by newlines, and no flag to be seen, so we have to take a closer look at our approach.
Dialing into our trusty tool, the default behavior of "strings" is to find readable strings with four or more characters that end in a new line. 
That sounds great for finding normal strings, but this is a CTF - perhaps the author was banking on us not modifying the default behavior of strings?
After consulting the manual, we try our new command - 

``` 
strings -n 1 tiny-little-fibers > smaller_fibers.txt && vim smaller_fibers.txt
``` 

Taking a new look at our file, we see a number of shorter strings as lines in the file, but using the search function of vim ( /wordtofind ) turns up no results for flag. 
Instead, we take into account the newline characters, and try searching for "f\nl\na\ng". This leads us to the flag we so desperately sought - "flag{22c534c5abea84bf6c1193e263f7259f}". 

Instead of taking your time to painfully type it out, we can go ahead and dump the file into Cyberchef and use the handy "Remove Whitespace" recipe to remove those pesky newline characters. 

Alternatively, for a more terminal-based approach, we can use 

```
tr -d '\n' < smaller_fibers.txt > smallest_fibers.txt
``` 

After this, we can peacefully search for "flag{" to find our prize. Congrats! 
