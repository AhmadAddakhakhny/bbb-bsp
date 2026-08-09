## File Management

### ls command
1. ls -lF = ll => list with mark for directories and special files
2. ls -lh => list with readable size value

### alias
1. alias $list='ls -laF'

### disk usage
1. du -hs ./dir # list with summary for this dir
2. du -hd 1 ./dir # list with summary for one level of this dir

### dir
1. mkdir -p parent/child # create a directory or more nested
2. mv src-dir1 src-dir2 dest-dir # move multiple directories in one step

### cat & tail
> it shows a file or more than one file at a time concatenate
1. cat file.txt file2.txt
2. tail syslog # by default last 10 lines
3. tail -n 4 syslog
4. tail -f syslog # watching the file without exiting
5. 

### sort
> a nice tool which you pipe the output of a command to sort it numerically then alphabetically
1. cat file.txt | sort    # ascending
2. cat file.txt | sort -r # descending

### echo + text formating (i.e. coloring) ANSI escape codes
> prints outs a text either normal or formatted
1. echo "hello world"
2. echo -e "Hello\nWorld\nHallo\nLeute" # formatted text option, deal with special charachters
> coloring using ANSI escape codes
2. echo -e "\e[38;5;mHello, World\e[0m" 
> \e means -> echo, deal with the following as instruction  
> 38 set foreground text, 5 is setting the color code system, 82 is the color code, \e0m reset the formating  
> P.S. you can make seviral groups of instruction like (foreground, background, bold, italic), and better to do that in grouping which means every instruction is managed by a separate \e group   
3. echo -e "\e[38;5;m \e[48;5;15mHello, World\e[0m"   

### tr - translation
> powerful tool to transform the text from lower to upper case, or delete specific char's (important with REGIX)
1. echo "hello world" | tr 'a-z' 'A-Z'

---
### Redirection – Standard Input, Output and Error
> each process has a process id number and located under /proc  
> each frocess has file decriptor directory /proc/proc-id/fd/  
> inside the /fd/ directory you may find 3 main file 0, 1, 2  
> 0->stdin, 1->stdout, 2->stderr which means when this process wants to have an input or output or output an err, they should be executed through these file decriptors.
> the stdin, stdout, stderr shall be binded with other means not only the terminal
1. stdout
```bash
> , >> 
# input shall be a command output or a file, and the output shall be redirected to a file
echo "hello, world!" > file.txt # overwrite the file
echo "hello, world!" >> file.txt # append to the file
```
2. stdin
> input is a file
```bash
<
# it's not common to be used, but the input MUST be a file.
# use case #1: there are some few commands that accepts only stdin redirection
# tr (transformer cmd) accepts either through piping or stdin
tr 'a-z' 'A-Z' < file.txt
cat file.txt | tr 'a-z' 'A-Z'
# use case #2: it filters some commands outputs
wc < file.name # better than (file name no re-printed)
wc file.name
```
> input is multi lines
```bash
<
# it's being used with commands that accepts files, but you want to send multiline, common to be used with cat
cat << EOF
> Hello
> World
> EOF
Hello
World
# --
cat << EOF > file.txt # redirect the text to file.txt and string delimiter is EOF
> Hello
> World
> EOF
# --
```
> input is one line
```bash
<
# it's commonly used to forward a oneline string (comes from a variable) to a command that accepts only files.
wc -c <<< "one line variable"
3
```
2. stderr
> this shall be applie to > and >>
```bash
<
# you have to exciplicitly add the file fd number '2', as the default is 0 or 1.
ls -l non-existing-dir 2> stderr.txt

# to redirect stdout and stderr in oneline
ls -l non-existing-dir &> stderr.txt # &>
ls -l non-existing-dir > stderr.txt 2>&1 = ls -l non-existing-dir > stderr.txt 2> stderr.txt
```
### balck hole /dev/null
> it's simply not caching or storing the redirect output to it.
```bash
# it's used to implicitly mean I am not intrested of the output result
ls -l non-existing-file > /dev/null
ls -l non-existing-file 2> /dev/null
ls -l non-existing-file &> /dev/null
```
