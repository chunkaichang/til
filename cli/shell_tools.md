# Shell Tools

## TLDR man
[TLDR pages](https://tldr.sh/) provides a TLDR version of `man` for each command.


## Globbing

Besides wildcards (`?` and `*`), curly braces (`{}`) are also useful for expanding strings with common patterns.

```
# Create 1.txt, 2.txt, and 3.txt
touch {1..3}.txt

# Remove foo.txt and bar.txt
rm {foo,bar}.txt  

# Remove all txt and python files
rm *{.txt,.py}
```


## Find and Action 

```
# Remove all files with their name ending with .txt
find . -name '*.txt' -exec rm {} \;
```
Note that the semicolon is escaped and the space before it is necessary.

A better approach is to use `find` with `xargs` as following: 
```
find . -name '*.txt' | xargs rm
```
First, the STDOUT of `find` is converted to STDIN by the pipe operator. Next, `xargs` turns STDIN to the arguments of `rm`. `xargs` is necessary for chaining commands that only take arguments as input.
