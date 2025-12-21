
- `grep "ERROR" sample.txt`
  
  gives all the lines that has this

- `grep -c "ERROR" sample.txt`

  gives count of the match

- `find . -name "*.txt" -mtime -1`

    gives all those files thta hve been modified in that last 24 hrs

    Breakdown:
    - `find`: command used to search for files and directories
    - `.`: search will start from wherever you currently are
    - `-name "*.log"`: filter that looks only for files that ends with .log
    - `-mtime`: stands for moififcation time(in days)
    - `-1`: means less than 1 day ago (modified within last 24 hrs)


Shell Script
- It's a text file with linux commands

  
To create a shell script, just copy all the commands in a file with extension .sh  (not required to give it an extensin - it's just for readability)

Now to run it it needs to have an executable permission so for this run:

`chmod +x sample-script.sh`

- `chmod`: change file permission
- `x`: adds execute permission

### Shebang

Now since extension is not required to write a shell script and there are other shell programs like bash, zsh, then how does sytem knows which program has been used to write this shell scripts ?
For this we use `Shebang Statment`

`"#!"` is the Shebang

A shebang (also called a hashbang) is the character sequence at the very top of a script file that tells the os which interpreter to use to run the file.

So a sample bash script would be like:

```
#!/bin/bash

grep "ERROR" sample.log

find . -name "*.log" -mtime -1
```

### Variables

We can use variables to store values that are used at many places in our script to avoid repetition.

We use `$` use a variable.

```
#!/bin/bash

LOG_DIR="/user/logs"

grep "ERROR" $LOG_DIR
```

### Arrays

Arrays are created using round brackets (), and the values are separated by space.

```
FILES=("TEST.txt" "NOTES.txt")
```

Arrays are used using `${FILES[0]}`

### Storing the value of a Command in a variable

```
LOG_FILES=$(find . -name "*.log" -mtime -1)
```

### Loops

Show count of all types of Error like "ERROR", "CRITICAL", "FATAL", in all the available log files.

Here what we can do is:

- Get all the log files
- Use loop to iterate over them
- And then use command to find count of all those Error int hat file

```
!#/bin/bash

ERROR_PATTERNS=(""ERROR" "FATAL" "CRITICAL")

LOG_FILES=$(find . -name "*.log" -mtime -2)

for LOG_FILE in $LOG_FILES; do

  echo "NumbeR of ${ERROR_PATTERNS[0]} logs found in $LOG_FILE"
  grep -c "${ERROR_PATTERNS[0]}" "$LOG_FILE"

  echo "NumbeR of ${ERROR_PATTERNS[1]} logs found in $LOG_FILE"
  grep -c "${ERROR_PATTERNS[1]}" "$LOG_FILE"

  echo "NumbeR of ${ERROR_PATTERNS[2]} logs found in $LOG_FILE"
  grep -c "${ERROR_PATTERNS[2]}" "$LOG_FILE"

done
```

- Iterating over array

  When we do:

  `for PATTERN IN $ERROR_PATTERNS; do`

  here, it will just pick the first element.

  so we have to use: ${ERROR_PATTERNS[@]}

  `@`: it says, to keep all the elements in the array as individual entity

  `*`: it combines all the element into a string

```
ERRORS=("FATAL" "WARNING" "LOG")

for error in ${ERRORS[@]}; do
  echo $error
done
```