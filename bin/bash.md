# bash

## bash shebang

```
#!/usr/bin/env bash
```

## common characters

```
” ” or ‘ ‘	Denotes whitespace. Single quotes preserve literal meaning; double quotes allow substitutions.
$	Denotes an expansion (for use with variables, command substitution, arithmetic substitution, etc.)
\	Escape character. Used to remove “specialness” from a special character.
#	Comments. Anything after this character isn’t interpreted.
=	Assignment
[ ] or [[ ]]	Test; evaluates for either true or false
!	Negation
>>, >, <	Input/output redirection
|	Sends the output of one command to the input of another.
* or ?	Globs (aka wildcards). ? is a wildcard for a single character.
```

## conditions

- file-based
- string-based
- arithmetic-based

### file-based

| Condition                        | True if                                                      | Example/explanation                                          |
| -------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ -a existingfile ]              | file ‘existingfile’ exists.                                  | if [ -a tmp.tmp ]; thenrm -f tmp.tmp # *Make sure we’re not bothered by an old temporary file*fi |
| [ -b blockspecialfile ]          | file ‘blockspecialfile’ exists and is block special.         | Block special files are special kernel files found in /dev, mainly used for  ATA devices like hard disks, cd-roms and floppy disks.if [ -b /dev/fd0  ]; thendd if=floppy.img of=/dev/fd0 # *Write an image to a floppy*fi |
| [ -c characterspecialfile ]      | file ‘characterspecialfile’ exists and is character special. | Character special files are special kernel files found in /dev, used for all  kinds of purposes (audio hardware, tty’s, but also /dev/null).if [ -c  /dev/dsp ]; thencat raw.wav > /dev/dsp # *This actually works for certain raw wav files*fi |
| [ -d directory ]                 | file ‘directory’ exists and is a directory.                  | In UNIX-style, directories are a special kind of file.if [ -d ~/.kde ]; thenecho “You seem to be a kde user.”fi |
| [ -e existingfile ]              | file ‘existingfile’ exists.                                  | (same as -a, see that entry for an example)                  |
| [ -f regularfile ]               | file ‘regularfile’ exists and is a regular file.             | A regular file is neither a block or character special file nor a directory.if [ -f ~/.bashrc ]; thensource ~/.bashrcfi |
| [ -g sgidfile ]                  | file ‘sgidfile’ exists and is set-group-ID.                  | When the SGID-bit is set on a directory, all files created in that directory will inherit the group of the directory.if [ -g . ]; thenecho [“Created files are inheriting the group ‘$(ls -ld . \| awk ‘{ print $4 }’)’ from the working directory.”fi](https://acloudguru.com/blog/engineering/the-awk-command-bash-basics) |
| [ -G fileownedbyeffectivegroup ] | file ‘fileownedbyeffectivegroup’ exists and is owned by the effective group ID. | The effective group id is the primary group id of the executing user.if [ ! -G file ]; then # *An exclamation mark inverts the outcome of the condition following it*chgrp $(id -g) file # *Change the group if it’s not the effective one*fi |
| [ -h symboliclink ]              | file ‘symboliclink’ exists and is a symbolic link.           | if [ -h $pathtofile ]; thenpathtofile=$(readlink -e $pathtofile) # *Make sure $pathtofile contains the actual file and not a symlink to it*fi |
| [ -k stickyfile ]                | file ‘stickyfile’ exists and has its sticky bit set.         | The sticky bit has got [quite a history](https://en.wikipedia.org/wiki/Sticky_bit), but is now used to prevent world-writable directories from having their contents deletable by anyone.if [ ! -k /tmp ]; then # *An exclamation mark inverts the outcome of the condition following it*echo “Warning! Anyone can delete and/or rename your files in /tmp!”fi |
| [ -L symboliclink ]              | file ‘symboliclink’ exists and is a symbolic link.           | (same as -h, see that entry for an example)                  |
| [ -N modifiedsincelastread ]     | file ‘modifiedsincelastread’ exists and was modified after the last read. | if [ -N /etc/crontab ]; thenkillall -HUP crond # *SIGHUP makes crond reread all crontabs*fi |
| [ -O fileownedbyeffectiveuser ]  | file ‘fileownedbyeffectiveuser’ exists and is owned by the user executing the script. | if [ -O file ]; thenchmod 600 file # *Makes the file private, which is a bad idea if you don’t own it*fi |
| [ -p namedpipe ]                 | file ‘namedpipe’ exists and is a named pipe.                 | A named pipe is a file in /dev/fd/ that can be read just once. See [my bash tutorial](https://www.linuxtutorialblog.com/post/tutorial-the-best-tips-tricks-for-bash#using-several-ways-of-substitution) for a case in which it’s used.if [ -p $file ]; thencp $file tmp.tmp # *Make sure we’ll be able to read*file=”tmp.tmp”  # *the file as many times as we like*fi |
| [ -r readablefile ]              | file ‘readablefile’ exists and is readable to the script.    | if [-r file ]; thencontent=$(cat file) # *Set $content to the content of the file*fi |
| [ -s nonemptyfile ]              | file ‘nonemptyfile’ exists and has a size of more than 0 bytes. | if [ -s logfile ]; thengzip logfile  # *Backup the old logfile*touch logfile # *before creating a fresh one.*fi |
| [ -S socket ]                    | file ‘socket’ exists and is a socket.                        | A socket file is used for inter-process communication, and features an  interface similar to a network connection.if [ -S  /var/lib/mysql/mysql.sock ]; thenmysql –socket=/var/lib/mysql/mysql.sock # *See [this MySQL tip](https://www.tech-recipes.com/mysql_tips762.html)*fi |
| [ -t openterminal ]              | file descriptor ‘openterminal’ exists and refers to an open terminal. | Virtually everything is done using files on Linux/UNIX, and the terminal is no  exception.if [ -t /dev/pts/3 ]; thenecho -e “nHello there. Message from  terminal $(tty) to you.” > /dev/pts/3 # *Anyone using that terminal will actually see this message!*fi |
| [ -u suidfile ]                  | file ‘suidfile’ exists and is set-user-ID.                   | Setting the suid-bit on a file causes execution of that file to be done with  the credentials of the owner of the file, not of the executing user.if [ -u executable ]; thenecho “Running program executable as user $(ls -l  executable \| awk ‘{ print $3 }’).”fi |
| [ -w writeablefile ]             | file ‘writeablefile’ exists and is writeable to the script.  | if [ -w /dev/hda ]; thengrub-install /dev/hdafi              |
| [ -x executablefile ]            | file ‘executablefile’ exists and is executable for the script. | Note that the execute permission on a directory means that it’s searchable  (you can see which files it contains).if [ -x /root ]; thenecho “You can view the contents of the /root directory.”fi |
| [ newerfile -nt olderfile ]      | file ‘newerfile’ was changed more recently than ‘olderfile’, or if ‘newerfile’ exists and ‘olderfile’ doesn’t. | if [ story.txt1 -nt story.txt ]; thenecho “story.txt1 is newer than story.txt; I suggest continuing with the former.”fi |
| [ olderfile -ot newerfile ]      | file ‘olderfile’ was changed longer ago than ‘newerfile’, or if ‘newerfile’ exists and ‘olderfile’ doesn’t. | if [ /mnt/remote/remotefile -ot localfile ]; thencp -f localfile /mnt/remote/remotefile # *Make sure the remote location has the newest version of the file, too*fi |
| [ same -ef file ]                | file ‘same’ and file ‘file’ refer to the same device/inode number. | if [ /dev/cdrom -ef /dev/dvd ]; thenecho “Your primary cd drive appears to read dvd’s, too.”fi |


### string-based

| Condition                                                  | True if                                                      | Example/explanation                                          |
| ---------------------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ STRING1 == STRING2 ]                                     | STRING1 is equal to STRING2.                                 | if [ “$1” == “moo” ]; thenecho $cow # *Ever tried executing ‘apt-get moo’?*fiNote: you can also use a single “=” instead of a double one. |
| [ STRING1 != STRING2 ]                                     | STRING1 is not equal to STRING2.                             | if [ “$userinput” != “$password” ]; thenecho “Access denied! Wrong password!”exit 1 # *Stops script execution right here*fi |
| [ STRING1 > STRING2 ]                                      | STRING1 sorts after STRING2 in the current locale (lexographically). | The backslash before the angle bracket is there because the bracket needs  to be escaped to be interpreted correctly. As an example we have a basic [bubble sort](https://en.wikipedia.org/wiki/Sorting_algorithm#Bubble_sort):*(Don’t feel ashamed if you don’t understand this, it is a more complex example)*array=( linux tutorial blog )swaps=1while (( swaps > 0 )); doswaps=0for ((  i=0; i < (( ${#array[@]} – 1 )) ; i++ )); doif [ “${array[$i]}” >  “${array[$(( i + 1 ))]}” ]; then # *Here is the sorting condition*tempstring=${array[$i]}array[$i]=${array[$(( i + 1 ))]}array[$(( i + 1 ))]=$tempstring(( swaps=swaps + 1  ))fidonedoneecho ${array[@]} # *Returns “blog linux tutorial”* |
| [ STRING1 < STRING2 ]                                      | STRING1 sorts before STRING2 in the current locale (lexographically). |                                                              |
| [ -n NONEMPTYSTRING ]                                      | NONEMPTYSTRING has a length of more than zero.               | This condition only accepts valid strings, so be sure to quote anything you  give to it.if [ -n “$userinput” ]; thenuserinput=parse($userinput) # *Only parse if the user actually gave some input.*fiNote that you can also omit the “-n”, as brackets with just a string in it behave the same. |
| [ -z EMPTYSTRING ]                                         | EMPTYSTRING is an empty string.                              | This condition also accepts non-string input, like an uninitialized  variable:if [ -z $uninitializedvar ]; thenuninitializedvar=”initialized” # *-z returns true on an uninitialized variable, so we initialize it here.*fi |
| *Double-bracket syntax only:*[[ STRING1 =~ REGEXPATTERN ]] | STRING1 matches REGEXPATTERN.                                | If you are familiar with Regular Expressions, you can use this conditions  to perform a regex match.if [[ “$email” =~  “b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+.[A-Za-z]{2,4}b” ]]; thenecho “$email  contains a valid e-mail address.”fi |

### arithmetic-based

| Condition         | True if                                        | Example/explanation                                          |
| ----------------- | ---------------------------------------------- | ------------------------------------------------------------ |
| [ NUM1 -eq NUM2 ] | NUM1 is **EQ**ual to NUM2.                     | These conditions only accept integer numbers. Strings will be converted to  integer numbers, if possible. Some random examples:if [ $? -eq 0 ]; then # *$? returns the exit status of the previous command*echo  “Previous command ran succesfully.”fiif [ $(ps -p $pid -o ni=) -ne  $(nice) ]; thenecho “Process $pid is running with a non-default nice  value”fiif [ $num -lt 0 ]; thenecho “Negative numbers not allowed;  exiting…”exit 1fi |
| [ NUM1 -ne NUM2 ] | NUM1 is **N**ot **E**qual to NUM2.             |                                                              |
| [ NUM1 -gt NUM2 ] | NUM1 is **G**reater **T**han NUM2.             |                                                              |
| [ NUM1 -ge NUM2 ] | NUM1 is **G**reater than or **E**qual to NUM2. |                                                              |
| [ NUM1 -lt NUM2 ] | NUM1 is **L**ess **T**han NUM2.                |                                                              |
| [ NUM1 -le NUM2 ] | NUM1 is **L**ess than or **E**qual to NUM2.    |                                                              |


## single bracket vs double bracket

The double-bracket syntax serves as an enhanced version of the single-bracket syntax; it mainly has the same features, but also some important differences with it. I will list them here:

```
if [[ "$stringvar" == *string* ]]; then
```

The first difference can be seen in the above example; when comparing strings, the double-bracket syntax features shell globbing. This means that an asterisk (“*”) will expand to literally anything, just as you probably know from normal command-line usage. Therefore, if $stringvar contains the phrase “string” anywhere, the condition will return true. Other forms of shell globbing are allowed, too. If you’d like to match both “String” and “string”, you could use the following syntax:
```
if [[ "$stringvar" == *[sS]tring* ]]; then
```

Note that only general shell globbing is allowed. Bash-specific things like {1..4} or {foo,bar} will not work. Also note that the globbing will not work if you quote the right string. In this case you should leave it unquoted.
The second difference is that word splitting is prevented. Therefore, you could omit placing quotes around string variables and use a condition like the following without problems:

```
if [[ $stringvarwithspaces != foo ]]; then
```

Nevertheless, the quoting string variables remains a good habit, so I recommend just to keep doing it.
The third difference consists of not expanding filenames. I will illustrate this difference using two examples, starting with the old single-bracket situation:

```
if [ -a *.sh ]; then
```

The above condition will return true if there is one single file in the working directory that has a .sh extension. If there are none, it will return false. If there are several .sh files, bash will throw an error and stop executing the script. This is because *.sh is expanded to the files in the working directory. Using double brackets prevents this:
```
if [[ -a *.sh ]]; then
```

The above condition will return true only if there is a file in the working directory called “*.sh”, no matter what other .sh files exist. The asterisk is taken literally, because the double-bracket syntax does not expand filenames.
The fourth difference is the addition of more generally known combining expressions, or, more specific, the operators “&&” and “||”. Example:
```
if [[ $num -eq 3 && "$stringvar" == foo ]]; then
```

The above condition returns true if $num is equal to 3 and $stringvar is equal to “foo”. The -a and -o known from the single-bracket syntax is supported, too.Note that the and operator has precedence over the or operator, meaning that “&&” or “-a” will be evaluated before “||” or “-o”.


