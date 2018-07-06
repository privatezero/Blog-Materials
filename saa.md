# Introduction to a Find Loop in Bash

Recently a lot of my work has involved processing legacy digital materials in order to generate/expand upon their associated metadata. As much of this work is file format specific, I often find myself trying to hunt down all examples of a certain file type spread across thousands of directories representing many terabytes worth of data. To do this I have been using the Bash `find` command. This allows me to generate lists of file paths which then can be looped through to perform actions as needed, such as file validation and metadata generation. As I imagine that I am not the only one with vast amounts of legacy files to process, I wanted to write up a quick explanation of this process and hopefully save others some time!

For this example, I will be targeting `.wav` files, but this process would work on any type of file. The main caveat being that this method is locating files solely by extension, so any files that have non-conforming extensions will not be discovered and files that have been erroneously changed to the target extension will show up as false positives.


## Example of `Find`

A basic example of the find command is `find "/My-Folder" -iname "*.wav"`. This command can be broken down as follows:

* `find` initiates the command.
* `~/My-Folder` is the target directory that will be searched. The search is recursive, meaning that any sub-folders will also be searched. If `.` is used instead of a specific directory, then the current directory will be searched. Note that if there are any spaces in your file name you will have to put it in quotes. A quick way to get the 
* `-iname` tells the find command that you will be searching for case insensitive terms in the filenames. This helps account for people mixing case, such as `.tif` and `.TIF` over the years.
* `"*.wav"` gives the find command the term that you will be searching for. In this instance, the `*` is a wildcard, so the results will include any files that end in `.wav`.

Running this command recursively finds all WAV files (or again, more accurately all files that have the .wav extension) in the target directory and will spit out their file paths into your terminal. By itself this can be informative, but it really becomes useful when paired with a loop to process those results.


## Loop Example
In this example I will show how to use the results of the previous command to batch perform the sample preservation action of embedding MD5 checksums into the discovered WAV files using BWF Metaedit ([Download available here](https://mediaarea.net/BWFMetaEdit)).

The entire command is: `find '/My-Folder' -iname "*.wav" | while read wavefile ; do bwfmetaedit --MD5-Embed "$wavefile"; done`, which again can be broken down as follows:

`find '/My-Folder' -iname "*.wav"` is the find command demonstrated previously. This provides the file paths that will be used by the following commands.

`|` This is called a 'pipe' and is what is used in Bash to send the output of one command to another. In this case, we are sending the output of the `find` command into a loop. (This is the vertical bar symbol on the keyboard, not a capital I).

`while read targetfile ; do` This is what starts the loop. What this does is 'read' each chunk of information that is coming from the pipe, in this case file paths, and then assign them to the variable 'targetfile.' The name 'targetfile' is an arbitrary name I chose for this example, and could be replaced with anything such as 'x' or 'myfile' as long as you are consistent in the following steps.

`bwfmetaedit --MD5-Embed --reject-overwrite "$targetfile"` this is the actual preservation action that will be run on every file discovered with the `find` command. The first part `bwfmetaedit --MD5-Embed` is the BWF Metaedit command that embeds a checksum into target WAV files. The second part `"$targetfile"` makes the command run on the variable that was created in the previous step, with the result that BWF Metaedit will be run on all your located WAV files. It is important to not forget the quotation marks around this variable otherwise it won't work on any file names that have spaces in them. Since this variable represents file paths in the loop, it can be used in combination with any command you would want to run on your files!

`; done` ends the loop once it is done reading the output of the `find` command.

Since all that needs to be done to use this command in different contexts is to change the file extension in the initial `find` command and then change the preservation action that is run in the loop, I find that I am using similar constructions on an almost daily basis! By using find loops I am able to save a lot of time (and pain) when dealing with legacy data. I hope this example will help people to do the same! 
