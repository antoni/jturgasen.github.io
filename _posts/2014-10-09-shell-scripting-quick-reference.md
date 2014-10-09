---
layout: post
title: "Shell Scripting Quick Reference"
description: ""
category: 
tags: ["shell scripting"]
---
{% include JB/setup %}

The shell scripting quick reference I wrote way back in 2003 (!!), now in markdown!

## Misc Notes

* As a best practice variables names should be surrounded in curly braces such as ``${VARIABLE}``
* It is also a best practice to explicitly set your ``$PATH`` at the beginning of the script
* To easily store file modes for backout use **ACL* commands, they can be parsed easier than ``ls -l`` output
* When using strings, quote variables to ensure the word splitting is correct
* Use ``echo`` to debug your code.  ``echo starting this`` then ``echo that completed``, or ``echo ${VAR}``


## Variables

* ``VAR=VALUE`` - Sets the variable VAR to VALUE
* ``VAR="VALUE"`` - Quote the VALUE if you use a string
* ``${VAR:-VALUE}`` - If VAR is set return its value, if not display VALUE**
* ``${VAR:=VALUE}`` - If VAR is set return its value, if not set VAR to VALUE
* ``${VAR:=VALUE}`` - If VAR is set return VALUE, if it is empty return null
* ``${VAR:offset:length}`` - Returns a substring of VAR beginning at *offset*.  *length* is optional
* ``echo $VAR`` - Displays the value of VAR
* ``export $VAR`` - Makes the variable VAR available to all new subprocesses
* ``$1`` - First argument
* ``$2`` - Second argument, $3 is the third argument and so on
* ``$*`` - All arguments
* ``$#`` - Number of arguments
* ``$0`` - Name of the shell script
* ``$$`` - The script's PID
* ``$?`` - Exit or return code from the last command
* ``$@`` - All arguments as separate words
* ``"$@"`` - Each argument individually quoted (ie: "$1" "$2" "$3", etc), **PREFERRED OVER ``$@``**
* ``$!`` - Displays the PID of the last background process that exited

* ``read VAR`` - Reads input from the terminal and sets VAR to that input
* ``read VAR1 VAR2 ...`` - The first word of input is set to VAR1 the second to VAR2 and so on.  If there are multiple words left over when you get to the last variable it's value is set to all of the remaining words

* ``shift`` - Moves all arguments to the left one place (ie: ``$2`` becomes ``$1`` and ``$1`` is removed)
* ``shift number`` - Move all arguments *number* of places to the left (1, 2, 3, etc - an integer)


## case Statements

The basic format of a case statement:

    case variable in
    value1 ) commands... ;;
    value2 ) commands... ;;
    ...
    * ) commands... ;;
    esac

* If variable is equal to value1 then execute the commands up to ``;;``
* If not, compare it to value2 and so on
* If it doesn't match any values it executes the commands after the ``* )``

Another example that uses an **or**:

    case variable in
    value1|value2 ) commands... ;;
    value3 ) commands... ;;
    ...
    * ) commands... ;;
    esac

* If variable is either value1 or value2 then execute the commands up to ``;;``
* If not, compare it to value3 and so on
* If it doesn't match any values it executes the commands after the ``* )``

Another example using **wildcards**:

    case variable in
    value1 ) commands... ;;
    value2* ) commands... ;;
    ...
    * ) commands... ;;
    esac

* If variable is equal to value1 then execute the commands up to the two semicolons
* If not, compare it to value2 and so on
* If value2 is a and variable is aa or abcde it will match
* If it doesn't match any values it executes the commands after the * )


## Loops

The format of a for loop:

    for variable in word1 word2 word3 ... ; do
      commands....
    done

* Loops until there are no more words (ie: if there are 4 words it will loop 4 times and then go on to the next part of the script)
* The value of variable is set to the current value of word each time though the loop

The format of a while loop:

    while command ; do
      commands
    done

* Loops until the exit status of *command* is non-zero

The format of a until loop:

    until command ; do
      commands
    done

* Loops until the exit status of *command* is 0

How to break out of a loop:

* ``break`` - Break out of the current loop and resume execution of the script after the loop
* ``break number`` - Break out of *number* of nested loops and resume executed of the script after the outermost loop
* ``continue`` - Stop execution of the current loop, go to the beginning of the loop and process the next value
* ``continue number`` - Same as above but stop execution of number of embedded loops


## Tests and Comparisons

A simple if then statement that makes a decision based on the exit code of a command:

    if command ; then
      commands...
    fi

* Tests the exit code of command and executes commands if it is 0
* If the exit code of command is non-zero the script continues after the ``fi``

``[ command ]`` - ``[`` is an alias for ``test`` and must be closed with ``]``.  Here is an example:

    if [ test ] ; then
      commands...
    fi

* Tests the exit code of test and executes commands if it is 0
* If the exit code of test is non-zero the script continues after the fi

An ``if / else`` statement:

    if [ test ] ; then
      commands...
    else
      commands...
    fi

An else if (``elif``) statement, you can nest them:

    if [ test1 ] ; then
      commands1...
    elif [ test2 ] ; then
      commands2...
    fi
    fi

If the exit status of ``test1`` is 0 then ``commands1`` is executed and the script continues after the second ``fi``
If the exit status of ``test1`` is non-zero then the ``elif`` test is executed
If the exit status of ``test2`` is 0 then ``commands2`` is executed
If the exit status of ``test2`` is non-zero then nothing is executed and the script continues after the first ``fi``

Chaining commands based on exit code:

* ``command1 && command2`` - If *command1* has an exit code of 0, run *command2*
* ``command1 || command2`` - If *command2* non-zero exit code run *command2*

Integer comparisons:

* ``integer1 -eq integer2`` - Equal to
* ``integer1 -ne integer2`` - Not equal
* ``integer1 -gt integer2`` - Greater than
* ``integer1 -lt integer2`` - Less than
* ``integer1 -le integer2`` - Less than or equal to
* ``integer1 -ge integer2`` - Greater than or equal to

String comparisons:

* ``string1 = string2`` - Equal to
* ``string1 != string2`` - Not equal to
* ``-z string`` - Zero length
* ``-n string`` - Non-zero length

File tests:

* ``-a file`` - File exists (ksh only)
* ``-b file`` - Block device
* ``-c file`` - Character device
* ``-d file`` - Directory
* ``-f file`` - Ordinary file
* ``-g file`` - Has the SGID bit set
* ``-G file`` - If file is owned by the EGID of the process
* ``-k file`` - Sticky bit set
* ``-L file`` - Symbolic link
* ``-O file`` - If the file is owned by the EUID of the process
* ``-p file`` - Named pipe or FIFO
* ``-r file`` - Readable by the process
* ``-s file`` - Non-zero length
* ``-u file`` - Has SUID bit set
* ``-w file`` - Writeable by the process
* ``-x file`` - Executable by the process
* ``file1 -ef file2`` - *file1* and *file2* are linked (symlink or hard link)
* ``file1 -nt file2`` - *file1* is newer than *file2*
* ``file1 -ot file2`` - *file1* is older than *file2*


## Trapping Signals

* ``trap command signal1 signal2 ...`` - Execute *command* if signal(s) are received
* ``trap signal1 signal2 ...`` - Stop trapping signal1, signal2 and so on

Common signals, check manpages for more:

* 0 - Exit from shell
* 1 - Hangup or logout
* 2 - Interrupt (CTRL + C)
* 15 - Software termination via kill


## I/O Redirection

* ``> file`` - Save STDOUT to *file*
* ``>> file`` - Append STDOUT to *file*
* ``< file`` - Read STDIN from *file*
* ``2> file`` - Saves STDERR to *file*
* ``2>&1`` - Redirects STDERR to SDTOUT
* ``> file 2>&1`` - Redirects STDOUT and STDERR to *file*
* ``command1 | command2`` - Takes STDOUT from *command1* and redirects it to STDIN to *command2*
* ``command | tee file`` - Sets the output of command to both the screen and *file*
* ``command > /dev/null`` - Redirects STDOUT to /dev/null
* ``command 2> /dev/null`` - Redirects STDERR to /dev/null
* ``command > /dev/null 2>&1`` - Redirects both STDOUT and STDERR to /dev/null


## Misc Helpful Commands

* ``expr`` - Integer math
* ``wait PID`` - Pause until *PID* is finished executing, if no PID is specified wait for all background jobs to complete
* ``sleep seconds`` - Pause for *seconds* and continue
* ``true`` - Does nothing, always returns an exit status of 0
* ``false`` - Does nothing, always returns an exit status of 1
* ``sort -u`` - Removes duplicate lines
* ``xargs command`` - Allows *command* to read arguments from standard input, eg: ``find . | xargs ls -la``
* ``set -a`` - Exports all shell variables
* ``exec command`` - Replaces the current running process with *command*, used often when starting a new shell so you don't have to exit twice
* ``uniq`` - Used to remove and/or manipulate continuous duplicate lines
* ``rev`` - Reverses the characters on each line in a file (eg: abc becomes cba, etc)
* ``tac`` - Reverse cat, last line first, second to last second, etc
* ``usleep time`` - Sleeps for *time* in microseconds
* ``basename command`` - Strips all leading slashes off command such as $0 or /usr/bin/ls
* ``getopts`` - Parses arguments for shell scripts

* ``cut -ddelimiter -ffields`` - Cuts the specified field(s) from standard input using *delimiter* to delimit each *field*.  Fields can be a single field (3), multiple fields (1,3,5), a range of fields (1-10) or a combination (1,3-4,7).
* ``cut -cnumber`` - Cuts characters *number* from each line.  *number* is the same format as fields above

* ``awk '{print $field }'`` - Displays *field* number using 1 or more spaces as a field delimiter
* ``awk '{print $field1,$field2 }'`` - Same as above but prints two fields separated by spaces, remove the comma to have the fields not separated
* ``awk -Fdelimiter '{print $field }'`` - Prints field number *field* using delimiter as the field delimiter
* ``awk '{print $NF}'`` - Prints the last field in each line (the ``$`` regex)
* ``awk 'length($0) >number'`` - Prints each line that is more than *number* of characters long
* ``awk 'NF > 0'`` - Prints all non-blank lines in a file
* ``awk '{print NF}'`` - Prints the number of fields in each line
* ``awk '/string/{print $1}'`` - Grabs the first field in each line that contains string

* ``grep -i string`` - Case insensitive grep
* ``grep -v string`` - Displays all lines except those that contain *string*
* ``grep -c string`` - Displays the number of lines that contain *string*
* ``grep -n string`` - Precedes each line with it's line number in the file
* ``grep -h string`` - Suppresses the file name from being displayed when searching multiple files
* ``grep -q string`` - Suppresses all output, returns an exit status of 0 if the string is found
* ``egrep 'string1|string2'`` - Greps for lines containing *string1* or *string2*, more strings can be specified, delimit them with a pipe

* ``tr string1 string2`` - Replace all instances of *string1* with *string2*
* ``tr -d string`` - Delete all instances of *string*


## Misc Stuff

* ``#!shell -e`` - Exit script if any command returns a non-zero exit code
* ``#!shell -x`` - Enables debugging
* ``#!shell -n`` - Checks the syntax without running any commands

* ``[characters]`` - Matches single character such as [A-Z] or [1-4] or any characters in the brackets such as [12345] or [aD3Fbr]
* ``[characters,characters]`` - Matches a single character within 2 ranges of (ie: [a-c,e-g]
* ``[!characters]`` - Matches any characters EXCEPT those listed, same format as above
* ``?`` - Matches a single character except period or dot
* ``*`` - Matches zero or more characters except files that begin with a period or dot
* ``\`` - The next character loses it's special meaning to the shell (such as echo \*)
* ``''`` - Single quotes, all characters within single quotes lose their special meaning to the shell
* ``""`` - Double quotes, SOME characters lose their special meaning.  $, ? and * do not
* `` - Backticks or backquotes, replaces the enclosed command with the output of that command
* ``#`` - Comment
* ``exit number`` - Exit the script with a return or exit code of number
* ``:`` - Performs no action, always runs an exit code of 0
* ``name() { commands }`` - Creates a function named name that contains the command or commands commands
* ``function name { commands }`` - Alternate method of creating a function
* ``return number`` - Makes the exit status of a function number
* ``. file`` - Executes the contents of file in the current shell
* ``echo`` - Prints a blank line
* ``eval`` - Has the shell evaluate the line twice, often used with ~user
* ``( ls /dir ) | while read file ; do cp "$file" ../newdir ; done`` - Used to process a list of files where some of the files contain spaces in their name


## bash Specific Stuff

* ``* + - / %`` - Arithmetic operators
* ``((var++))`` - Increments *var* after obtaining it's value (eg: echo $((ONE++)) returns 1)
* ``((++var))`` - Increments *var* before obtaining it's value (eg: echo $((++ONE)) returns 2)
* ``((var--))`` - Decrements *var* after obtaining it's value
* ``((--var))`` - Decrements *var* before obtaining it's value
* ``((var=operation))`` = Sets *var* to the result of the arithmetic operation

The following are character classes, they go between ``[::]`` such as ``[:lower:]``:

* ``alnum`` - Alphanumeric
* ``alpha`` - All letters
* ``digit`` - All numbers
* ``lower`` - Lowercase letters
* ``punct`` - Punctuation symbols
* ``upper`` - Upper case letter

Some bash predefined variables:

* ``EUID`` - Current UID
* ``GROUPS`` - Groups you're a member of, array variable
* ``HOSTNAME`` - The usual
* ``PPID`` - bash's PPID
* ``RANDOM`` - Prints a random number between 0 and 32k when referenced
* ``SHELLOPTS`` - Options passed to the current shell using -o
* ``OPTARG`` - Last command line argument read using getops
* ``OPTIND`` - Points to the array index of the next argument to by processed with getops
* ``UID`` - UID of the current user as set when the shell was started