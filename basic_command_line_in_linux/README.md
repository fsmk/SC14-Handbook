**BASIC COMMAND LINE IN LINUX**
===============================

Graphical user interfaces (GUIs) are helpful for many tasks, but they are not good for all tasks. Most computers today are not powered by electricity. They instead seem to be powered by the "pumping" motion of the mouse! Computers were supposed to free us from manual labor, but how many times have you performed some task you felt sure the computer should be able to do but you ended up doing the work yourself by tediously working the mouse? Pointing and clicking, pointing and clicking.

So,let your minds explore the new concept of Command Lines. Linux and Unix systems can be made extremely simple using basic command line instructions. And you can have fun with these to. 



Contents
--------

 1. [What Is "The Shell"?][1]
 2. [Navigation][2]
 3. [Looking Around][3]
 4. [A Guided Tour][4]
 5. [Manipulating Files][5]
 6. [Working with Commands][6]
 7. [I/O Redirection][7]
 8. [Expansion][8]
 9. [Permissions][9]
 10. [Job Control][10]
 
----------
## How to use Basic Commands ##
This article will show how to execute commands like changing the working directory, viewing content of a directory, creating and renaming folders, copying, deleting files and folders, and how to launch any application from Command Prompt. Also, it will show how to get help when using the tool.

### Directories: ###

BlockquoteFile and directory paths in UNIX use the forward slash "/" 
to separate directory names in a path.
Examples:

    /                   "root" directory
    /usr                directory usr (sub-directory of / "root" directory)
    /usr/STRIM100       STRIM100 is a subdirectory of /usr
    
### Moving around the file system: ###

File and directory paths in UNIX use the forward slash "/" 
to separate directory names in a path.
Examples:

    pwd               Show the "present working directory", or current directory.
    cd                Change current directory to your HOME directory.
    cd /usr/STRIM100  Change current directory to /usr/STRIM100.
    cd INIT           Change current directory to INIT which is a sub-directory of the current 
                            directory.
    cd ..             Change current directory to the parent directory of the current directory.
    cd $STRMWORK      Change current directory to the directory defined by the environment 
                            variable 'STRMWORK'.
    cd ~bob           Change the current directory to the user bob's home directory (if you have permission).

### Listing Directory Contents: ###

    ls    list a directory
    ls -l    list a directory in long ( detailed ) format
    
       for example:
       
    $ ls -l 
    
    drwxr-xr-x    4 cliff    user        1024 Jun 18 09:40 WAITRON_EARNINGS
    -rw-r--r--    1 cliff    user      767392 Jun  6 14:28 scanlib.tar.gz
    ^ ^  ^  ^     ^   ^       ^           ^      ^    ^      ^
    | |  |  |     |   |       |           |      |    |      |  
    | |  |  |     | owner   group       size   date  time    name 
    | |  |  |     number of links to file or directory contents
    | |  |  permissions for world
    | |  permissions for members of group
    | permissions for owner of file: r = read, w = write, x = execute -=no permission
    type of file: - = normal file, d=directory, l = symbolic link, and others...
    
    ls -a        List the current directory including hidden files. Hidden files start 
                 with "." 
    ls -ld *     List all the file and directory names in the current directory using 
                 long format. Without the "d" option, ls would list the contents 
                 of any sub-directory of the current. With the "d" option, ls 
                 just lists them like regular files. 
                 
### Changing file permissions and attributes: ###

    chmod 755 file       Changes the permissions of file to be rwx for the owner, and rx for the group and the world. (7 = rwx = 111 binary. 5 = r-x = 101 binary)
    
    chgrp user file      Makes file belong to the group user.
    chown cliff file     Makes cliff the owner of file.
    chown -R cliff dir   Makes cliff the owner of dir and everything in its directory tree. 

You must be the owner of the file/directory or be root before you can do any of these things. 

### Moving,renaming and copying files: ###

    cp file1 file2          copy a file
    mv file1 newname        move or rename a file
    mv file1 ~/AAA/         move file1 into sub-directory AAA in your home directory.
    rm file1 [file2 ...]    remove or delete a file
    rm -r dir1 [dir2...]    recursivly remove a directory and its contents BE CAREFUL!
    mkdir dir1 [dir2...]    create directories
    mkdir -p dirpath        create the directory dirpath, including all implied directories in the path.
    rmdir dir1 [dir2...]    remove an empty directory
    
### Viewing and Editing files: ###

    cat filename      Dump a file to the screen in ascii. 
    more filename     Progressively dump a file to the screen: ENTER = one line down 
                      SPACEBAR = page down  q=quit
    less filename     Like more, but you can use Page-Up too. Not on all systems. 
    vi filename       Edit a file using the vi editor. All UNIX systems will have vi in some form. 
    emacs filename    Edit a file using the emacs editor. Not all systems will have emacs. 
    head filename     Show the first few lines of a file.
    head -n  filename Show the first n lines of a file.
    tail filename     Show the last few lines of a file.
    tail -n filename  Show the last n lines of a file.
    
### Shell ###

The behavior of the command line interface will differ slightly depending 
on the shell program that is being used. 

Depending on the shell used, some extra behaviors can be quite nifty.

You can find out what shell you are using by the command:

   

>  echo $SHELL

Of course you can create a file with a list of shell commands and execute it like
a program to perform a task. This is called a shell script. This is in fact the 
primary purpose of most shells, not the interactive command line behavior. 

### Pipes: ###

The pipe symbol "|" is used to direct the output of one command to the input 
of another.

For example:

    ls -l | more  

 

This commands takes the output of the long format directory list command     "ls -l" and pipes it through the more command (also known as a filter). In this case a very long list of files can be viewed a page at a time.

    du -sc * | sort -n | tail 

 
               
The command "du -sc" lists the sizes of all files and directories in the 
current working directory. That is piped through "sort -n" which orders the 
output from smallest to largest size. Finally, that output is piped through "tail" which displays only the last few (which just happen to be the largest) results.

### Searching for strings in files: The grep  command ###

`grep string filename`    prints all the lines in a file that contain the string

### Looking for help: The man and apropos commands ###

Most of the commands have a manual page which give sometimes useful, often more or less 
detailed, sometimes cryptic and unfathomable discriptions of their usage. Some say they 
are called man pages because they are only for real men. 

Example:

`man ls`      Shows the manual page for the ls command

You can search through the man pages using apropos

Example:

`apropos build`     Shows a list of all the man pages whose discriptions contain the word "build"

Do a man apropos for detailed help on apropos.

### Basics of the  vi editor ###

Opening a file

    vi filename

Creating text 

    Edit modes: These keys enter editing modes and type in the text
    of your document. 
    
    i     Insert before current cursor position
    I     Insert at beginning of current line
    a     Insert (append) after current cursor position
    A     Append to end of line
    r     Replace 1 character
    R     Replace mode
    <ESC> Terminate insertion or overwrite mode

Deletion of text

    x     Delete single character
    dd    Delete current line and put in buffer
    ndd   Delete n lines (n is a number) and put them in buffer
    J     Attaches the next line to the end of the current line (deletes carriage return).
    u     Undo last command

Cut and Paste

    yy    Yank current line into buffer
    nyy   Yank n lines into buffer
    p     Put the contents of the buffer after the current line
    P     Put the contents of the buffer before the current line

Cursor positioning
   

        ^d    Page down
        ^u    Page up
        :n    Position cursor at line n
        :$    Position cursor at end of file
        ^g    Display current line number
        h,j,k,l     Left,Down,Up, and Right respectively.

  [1]: http://linuxcommand.org/lc3_lts0010.php
  [2]: http://linuxcommand.org/lc3_lts0020.php
  [3]: http://linuxcommand.org/lc3_lts0030.php
  [4]: http://linuxcommand.org/lc3_lts0040.php
  [5]: http://linuxcommand.org/lc3_lts0050.php
  [6]: http://linuxcommand.org/lc3_lts0060.php
  [7]: http://linuxcommand.org/lc3_lts0070.php
  [8]: http://linuxcommand.org/lc3_lts0080.php
  [9]: http://linuxcommand.org/lc3_lts0090.php
  [10]: http://linuxcommand.org/lc3_lts0100.php
