## basic Linux commands

pwd - print working directory

cd<path> - changes directory to given path
cd /mnt/c/Users/tinot/OneDrive/Desktop

ls - list all files and folders
ls -la - list and detail more info
ls -a - list all including hidden files

touch <file_name> - crate a new file
touch scripting.txt

mkdir <directory name> - make directory
mkdir folder

rm <file_name> - remove a file
rm -r <delete a directory>

find <path> -name "name of file" -- find a specific file
find /mnt/c/Users/tinot/OneDrive/Desktop -name scripting.txt

grep- used to search for specific text inside a file with a lot of text
grep "Linux" scripting.txt

shell scripting
script is text file with a series of commands that Linux shell bash will execute
it follows a structure called shebang(#!/bin/bash)

on Linux follow these to write your script
cd your preferred directory
nano scriptName.sh
write this first
#!/bin/bash
-- this opens a note pad, write all stuff you want
to save and close
ctrl + 0, press enter, ctrl + x

to run it
chmod +x firstScript.sh - this makes it executable
./firstScript.sh - run this script ./ if you already in the folder.

bash syntax
echo "tino" -- use to print stuff on screen just like print("mother fucker in python")
variables
name = "tino"
echo "hello $name"

condition if

if [condition], then
echo "do this"
else
echo "do this sucker"
fi
--fi ends the if condition
