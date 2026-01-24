syntax:
DATE/DATE/DATE
-KEYWORD DESCRIPTION FROMDOINGWHAT
-KEYWORD DESCRIPTION FROMDOINGWHAT

PREVIOUSLY:
LINUX LEARNED COMMANDS:
-others before, i hadnt listed
-chmod
-xxd, strings
-base64, -d for decode
-tr (translate. complicated, but can be used for caesar)
-less (scorllable)
-dpkg vs apt: dpkg is low level
-echo $SHELL see what shell im on, echo $- if it contains 'l' then its a login shell
-ln -> link (with -s: symlink) sort of a shortcut

-mount, umount
-df -h -> read disk space. other: fdisk, lblk, blkid
-du -h -> read directory space usage

from overthewire
-ssh [username@]address (-p [port] if specified)
-file [dir] see file types (./* -> all in the directory)
-find [dir] -type [f (file) or d (directory)] -size [something-c] -executable -[and others...] -exec [command to be executed on these files] {} \;-> can see ALL FILES recursively
human readable is usually ASCII (u can grep ASCII)
u can use ! to negate
u can add -exec [command] {} \; the syntax is pretty specific
-cat vs strings: cat shows everything, strings only shows human-readable. means cat will show the giberish
-u can grep lines AROUND the line u get with -A (after), -B (before) and -C (around) NUM, also only search beginning (with caret anchor: "^word", ^ is beginning while $ is end of file) dont forget -i for case insensitive
-wc (wordcount) (with -l it becomes linecount) < filename (redirects file content here)
-stat -c%s filename filesize
[Just write the command name...]
-uniq (print unique lines)
-sort
-mktemp (neat, makes a temporary file (add XXXXs to add randomizer))
-compression types: gzip, tar, bzip2


SYMBOLS
Input Redirection : [cmd] < [filename] -> reads file then redirects content to the command as input
Output Redirection : [cmd] > [filename] -> output of cmd is redirected and written to file (replaces)
Append Output : [cmd] >> [filename] -> same thing but appends only
Pipe : [cmd] | [cmd] -> output of first cmd is redirected as input of the next
Error Reirection : [cmd] 2> [filename] -> redirect errors to logfile
Redirect Output AND Errors : [cmd] &> -> yes
Here (heredoc) : [cmd] << END
consider this
a file 
END -> Input redirection, but you write stuff and it will be treated like its a file's content
Dont forget about wildcards (*) for directories!
