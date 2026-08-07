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
