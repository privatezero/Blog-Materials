# Introduction to a Bash Find Loop

Recently a lot of my work has involved processing legacy digital materials in order to generate/expand their associated metadata. As much of this work is file format specific, I often find myself trying to hunt down all examples of a certain file type spread across thousands of directories representing many terabytes worth of data. To do this I have been using the Bash `find` command. This allows me to generate lists of file paths which then can be looped through to perform actions as needed such as file validation and metadata generation. As I imagine that I am not the only one with vast amounts of legacy metadata to process, I wanted to write up a quick explanation of this process and hopefully save others some time!

For this example, I will be targeting `.wav` files, but this process would work on any type of file. The main caveat with this process it is locating files solely by extension, so any files that have non-conforming extensions will not be discovered and files that have erroneously changed to the target extension will show up as false positives.

A basic example of the find command is `find ~/My-Folder -iname "*.wav"`. This command can be broken down as follows:

* `find` initiates the command.
* `~/My-Folder` is the target directory that will be searched. The search is recursive, meaning that any sub-folders will also be searched. If `.` is used instead of a specific directory, then the current directory will be searched.
* `-iname` tells the find command that you will be searching for case insensitive terms in the filenames. This helps account for people mixing case, such as `.tif` and `.TIF` over the years.
* `"*.wav"` gives the find command the term that you will be searching for. In this case the `*` is a wildcard, so the results will include any files that end in `.wav`.
