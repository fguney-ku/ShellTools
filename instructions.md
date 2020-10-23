# SHELL TOOLS

A simplified summary of (the second part of) [Shell Tools and Scripting] (https://missing.csail.mit.edu/2020/shell-tools/)

---
## Finding how to use commands
* How to find what a command does and what are its different options? 
  google? => [StackOverflow](http://stackoverflow.com/)
  **UNIX predates StackOverflow**
  more built-in ways

* Command with the `-h` or `--help` flags
* `man` command: short for manual
  `man` goes to a manual page (called manpage) for the commands
  in `man rm` search for `-i` flag
  
* Online version of the [Linux manpages](https://man7.org/linux/man-pages/)
* Developers can write a manpage for nonnative commands as part of the installation process.

* Too detailed? How about [TLDR pages](https://tldr.sh/)?
`tldr ffmpeg`
`tldr convert`
`tldr tar`

---
## Finding files
* Finding files or directories. 
`find` command to recursively search for files matching some criteria.

* Some examples:
  * search in the current directory (`.`) with the name "src" (`-name src`) and type dorectory (`-type d`)
  `find . -name src -type d`

  * search by specifying a path (`-path`), for example first in some number of folders followed by "test" folder (`**/test/`) for only python (`*.py`) scripts whose type is file (`-type f`)
  `find . -path '**/test/*.py' -type f`

  * different flags, for example for files in the current directory (`.`) whose modification time (`-mtime`) is last day (`-1`).
  `find . -mtime -1`

  * more search with size, owner, etc.

* Actions over files matching your query
```
# create some files with .tmp and .txt extension
touch a.tmp b.tmp a.txt

# find all the files with .tmp extension
find . -name '*.tmp'

# first find, in every one of these files, execute rm (remove) command on them
find . -name '*.tmp' -exec rm {} \;

# check if they are still there
find . -name '*.tmp'
```
* Find files that match some pattern PATTERN: 
  `find -name '*PATTERN*'` 
  or `-iname` to be case insensitive 

* A friendly alternative: [fd](https://github.com/sharkdp/fd)
colorized output, default regex matching, Unicode support, and a more intuitive syntax.

* Example: the syntax to find a pattern PATTERN is `fd PATTERN`.
`fd ".*.tmp"`

* How about efficiency? 
Compile some sort of index or database for quickly searching. 
`locate` uses a database that is updated using `updatedb`. 

* Difference between `find` and `locate`
`find` can also search for files using attributes such as file size, modification time, or file permissions, while `locate` just uses the file name.
[A more in-depth comparison](https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other)

---
## Finding code
* Search based on file content. 
For example, search for all files that contain some pattern: `grep`

* Search for "normalize" in the file "mot.py"
`grep normalize mot.py`

* Search for "normalize" recursively (`-R`) in the current directory (`.`)
`grep -R normalize .`

* A fast alternative: [`rg`](https://github.com/BurntSushi/ripgrep)
automatically recursive, color coding, ignoring .git folders, using multi CPU support, etc.

* Search for the line `import numpy` in all python files (`*.py`)
`rg 'import numpy' *.py`

* Specify the type as python files (`-t py`) in a directory (`deep_sort/`)
`rg 'import numpy' -t py deep_sort/`

* `-C` for getting **C**ontext, for example 5 lines (`-C 5`),  around the matching line
`rg 'import numpy' -t py deep_sort/ -C 5`

* Print all the lines that do not match the pattern (`--files-without-match`), also include hidden files (`-u`)
`rg -u --files-without-match "import numpy" -t py`

* Statistics (`--stats`) for listing the number of matches and matched lines, etc.
`rg 'import numpy' -t py mot_neural_solver/ -C 5 --stats`

* There are other alternatives: [`ack`](https://beyondgrep.com/), [`ag`](https://github.com/ggreer/the_silver_searcher). 

---
## Finding shell commands
* How to find a specific command you typed at some point?
the up arrow

* `history` command to access shell history

* The last 10 commands
`history 10`

* Use "Ctrl+R" to perform backwards search through your history.
Keep hitting "Ctrl+R" to go through all the matches.

* Using piping (`|`), `grep` and search for patterns in the history
For example, for commands that contain "convert".
`history | grep convert` 


---
## Directory Navigation
* How to quickly navigate directories? 
shell aliases, creating symlinks with `ln -s`

* An example of hell: `ls -R`

* Friendlier solutions: [`tree`](https://linux.die.net/man/1/tree), [`broot`](https://github.com/Canop/broot) or [`nnn`](https://github.com/jarun/nnn)


