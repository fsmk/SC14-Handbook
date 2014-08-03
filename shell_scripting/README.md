## SHELL SCRIPTING ##

Hello. In this module,we would be learning shell scripting and how the same works in Linux and Unix based systems.

###HERE ARE THE VARIOUS TOPICS WELL LOOK INTO###

**Writing your first script and making it to work**

To successfully write a shell script, you have to do three things:

 - Write a script
 - Give the shell permission to execute it
 - Put it somewhere the shell can find it
 

####**Writing a script**####

A shell script is a file that contains ASCII text. To create a shell script, you use a text editor. A text editor is a program, like a word processor, that reads and writes ASCII text files. There are many, many text editors available for your Linux system, both for the command line environment and the GUI environment. Here is a list of some common ones:

 1. vi
 2. emacs
 3. nano
 4. gedit
 5. kwrite
 
Now, fire up your text editor and type in your first script as follows:

    #!/bin/bash
    # My first script
    
    echo "Hello World!"



The clever among you will have figured out how to copy and paste the text into your text editor ;-)

Save the script as filename.sh

If you have ever opened a book on programming, you would immediately recognize this as the traditional "Hello World" program. Save your file with some descriptive name. How about my_script?

The first line of the script is important. This is a special clue given to the shell indicating what program is used to interpret the script. In this case, it is /bin/bash. Other scripting languages such as perl, awk, tcl, Tk, and python can also use this mechanism.

The second line is a comment. Everything that appears after a "#" symbol is ignored by bash. As your scripts become bigger and more complicated, comments become vital. They are used by programmers to explain what is going on so that others can figure it out. The last line is the echo command. This command simply prints what it is given on the display.

####**Setting permissions**####

The next thing we have to do is give the shell permission to execute your script. This is done with the chmod command as follows:

    [me@linuxbox me]$ chmod 755 my_script

The "755" will give you read, write, and execute permission. Everybody else will get only read and execute permission. If you want your script to be private (i.e., only you can read and execute), use "700" instead.

Putting it in your path

At this point, your script will run. Try this:

    [me@linuxbox me]$ ./my_script

You should see "Hello World!" displayed. If you do not, see what directory you really saved your script in, go there and try again.

Before we go any further, I have to stop and talk a while about paths. When you type in the name of a command, the system does not search the entire computer to find where the program is located. That would take a long time. You have noticed that you don't usually have to specify a complete path name to the program you want to run, the shell just seems to know.

Well, you are right. The shell does know. Here's how: the shell maintains a list of directories where executable files (programs) are kept, and just searches the directories in that list. If it does not find the program after searching each directory in the list, it will issue the famous command not found error message.

This list of directories is called your path. You can view the list of directories with the following command:

    [me@linuxbox me]$ echo $PATH

This will return a colon separated list of directories that will be searched if a specific path name is not given when a command is attempted. In our first attempt to execute your new script, we specified a pathname ("./") to the file.

You can add directories to your path with the following command, where directory is the name of the directory you want to add:

    [me@linuxbox me]$ export PATH=$PATH:directory

A better way would be to edit your .bash_profile file to include the above command. That way, it would be done automatically every time you log in.

Most modern Linux distributions encourage a practice in which each user has a specific directory for the programs he/she personally uses. This directory is called bin and is a subdirectory of your home directory. If you do not already have one, create it with the following command:

    [me@linuxbox me]$ mkdir bin

Move your script into your new bin directory and you're all set. Now you just have to type:

    [me@linuxbox me]$ my_script

and your script will run.

####**WRITING AN HTML FILE WITHIN A SCRIPT**####

In the following lessons, we will construct a useful application. This application will produce an HTML document that contains information about your system. I spent a lot of time thinking about how to teach shell programming, and the approach I have come up with is very different from most approaches that I have seen. Most favor a rather systematic treatment of the many features, and often presume experience with other programming languages. Although I do not assume that you already know how to program, I realize that many people today know how to write HTML, so our first program will make a web page. As we construct our script, we will discover step by step the tools needed to solve the problem at hand.

As you may know, a well formed HTML file contains the following content:

    <HTML>
    <HEAD>
        <TITLE>
        The title of your page
        </TITLE>
    </HEAD>
    
    <BODY>
        Your page content goes here.
    </BODY>
    </HTML>

Now, with what we already know, we could write a script to produce the above content:

    #!/bin/bash
    
    # make_page - A script to produce an HTML file
    
    echo "<HTML>"
    echo "<HEAD>"
    echo "  <TITLE>"
    echo "  The title of your page"
    echo "  </TITLE>"
    echo "</HEAD>"
    echo ""
    echo "<BODY>"
    echo "  Your page content goes here."
    echo "</BODY>"
    echo "</HTML>"
       
This script can be used as follows:

    [me@linuxbox me]$ make_page > page.html

It has been said that the greatest programmers are also the laziest. They write programs to save themselves work. Likewise, when clever programmers write programs, they try to save themselves typing.

The first improvement to this script will be to replace the repeated use of the echo command with a here script, thusly:

    #!/bin/bash
    
    # make_page - A script to produce an HTML file
    
    cat << _EOF_
    <HTML>
    <HEAD>
        <TITLE>
        The title of your page
        </TITLE>
    </HEAD>
    
    <BODY>
        Your page content goes here.
    </BODY>
    </HTML>
    _EOF_
       
A here script (also sometimes called a here document) is an additional form of I/O redirection. It provides a way to include content that will be given to the standard input of a command. In the case of the script above, the cat command was given a stream of input from our script to its standard input.

A here script is constructed like this:

    command << token
    content to be used as command's standard input
    token
       
token can be any string of characters. I use "_EOF_" (EOF is short for "End Of File") because it is traditional, but you can use anything, as long as it does not conflict with a bash reserved word. The token that ends the here script must exactly match the one that starts it, or else the remainder of your script will be interpreted as more standard input to the command.

There is one additional trick that can be used with a here script. Often you will want to indent the content portion of the here script to improve the readability of your script. You can do this if you change the script as follows:

    #!/bin/bash
    
    # make_page - A script to produce an HTML file
    
    cat <<- _EOF_
        <HTML>
        <HEAD>
            <TITLE>
            The title of your page
            </TITLE>
        </HEAD>
    
        <BODY>
            Your page content goes here.
        </BODY>
        </HTML>
    _EOF_
       
Changing the the "<<" to "<< -" causes bash to ignore the leading tabs (but not spaces) in the here script. The output from the cat command will not contain any of the leading tab characters.

O.k. let's make our page. We will edit our page to get it to say something:

    #!/bin/bash
    
    # make_page - A script to produce an HTML file
    
    cat <<- _EOF_
        <HTML>
        <HEAD>
            <TITLE>
            My System Information
            </TITLE>
        </HEAD>
    
        <BODY>
        <H1>My System Information</H1>
        </BODY>
        </HTML>
    _EOF_
    

####**VARIABLES**####


What we have done is to introduce a very fundamental idea that appears in almost every programming language, variables. Variables are areas of memory that can be used to store information and are referred to by a name. In the case of our script, we created a variable called "title" and placed the phrase "My System Information" into memory. Inside the here script that contains our HTML, we use "$title" to tell the shell to substitute the contents of the variable.

As we shall see, the shell performs various kinds of substitutions as it processes commands. Wildcards are an example. When the shell reads a line containing a wildcard, it expands the meaning of the wildcard and then continues processing the command line. To see this in action, try this:

    [me@linuxbox me]$ echo *

Variables are treated in much the same way by the shell. Whenever the shell sees a word that begins with a "$", it tries to find out what was assigned to the variable and substitutes it.

####How to create a variable####

To create a variable, put a line in your script that contains the name of the variable followed immediately by an equal sign ("="). No spaces are allowed. After the equal sign, assign the information you wish to store. Note that no spaces are allowed on either side of the equal sign.

####Where does the variable's name come from?####

You make it up. That's right; you get to choose the names for your variables. There are a few rules.

 - It must start with a letter.
 - It must not contain embedded spaces. Use underscores instead.
 - Don't use punctuation marks.
 - Don't use a name that is already a word understood by bash. These are
   called reserved words and should not be used as variable names. If
   you use one of these words, bash will get confused. To see a list of
   reserved words, use the help command.

[Learn more about variables from here][1] 

###**FLOW CONTROL**###


In this lesson, we will look at how to add intelligence to our scripts. So far, our script has only consisted of a sequence of commands that starts at the first line and continues line by line until it reaches the end. Most programs do more than this. They make decisions and perform different actions depending on conditions.

The shell provides several commands that we can use to control the flow of execution in our program. These include:

if
exit
for
while
until
case
break
continue
if

The first command we will look at is if. The if command is fairly simple on the surface; it makes a decision based on a condition. The if command has three forms:

    # First form
    
    if condition ; then
        commands
    fi
    
    # Second form
    
    if condition ; then
        commands
    else
        commands
    fi
    
    # Third form
    
    if condition ; then
        commands
    elif condition ; then
        commands
    fi
          

 
In the first form, if the condition is true, then commands are performed. If the condition is false, nothing is done.

In the second form, if the condition is true, then the first set of commands is performed. If the condition is false, the second set of commands is performed.

In the third form, if the condition is true, then the first set of commands is performed. If the condition is false, and if the second condition is true, then the second set of commands is performed.

**What is a "condition"?**

To be honest, it took me a long time to really understand how this worked. To help answer this, there is yet another basic behavior of commands we must discuss.

**Exit status**

A properly written Unix application will tell the operating system if it was successful or not. It does this by means of an exit status. The exit status is a numeric value in the range of 0 to 255. A "0" indicates success; any other value indicates failure. Exit status provides two important features. First, it can be used to detect and handle errors and second, it can be used to perform true/false tests.

It is easy to see that handling errors would be valuable. For example, in our script we will want to look at what kind of hardware is installed so we can include it in our report. Typically, we will try to query the hardware, and if an error is reported by whatever tool we use to do the query, our script will be able to skip the portion of the script which deals with the missing hardware.

We can also use the exit status to perform simple true/false decisions. We will cover this next.

**test**

The test command is used most often with the if command to perform true/false decisions. The command is unusual in that it has two different syntactic forms:

    # First form
    
    test expression
    
    # Second form
    
    [ expression ]
       
The test command works simply. If the given expression is true, test exits with a status of zero; otherwise it exits with a status of 1.

The neat feature of test is the variety of expressions you can create. Here is an example:

    if [ -f .bash_profile ]; then
        echo "You have a .bash_profile. Things are fine."
    else
        echo "Yikes! You have no .bash_profile!"
    fi
       
In this example, we use the expression " -f .bash_profile ". This expression asks, "Is .bash_profile a file?" If the expression is true, then test exits with a zero (indicating true) and the if command executes the command(s) following the word then. If the expression is false, then test exits with a status of one and the if command executes the command(s) following the word else.

Before we go on, I want to explain the rest of the example above, since it also reveals more important ideas.

In the first line of the script, we see the if command followed by the test command, followed by a semicolon, and finally the word then. I chose to use the [ expression ] form of the test command since most people think it's easier to read. Notice that the spaces between the "[" and the beginning of the expression are required. Likewise, the space between the end of the expression and the trailing "]".

The semicolon is a command separator. Using it allows you to put more than one command on a line. For example:

    [me@linuxbox me]$ clear; ls

will clear the screen and execute the ls command.

I use the semicolon as I did to allow me to put the word then on the same line as the if command, because I think it is easier to read that way.

On the second line, there is our old friend echo. The only thing of note on this line is the indentation. Again for the benefit of readability, it is traditional to indent all blocks of conditional code; that is, any code that will only be executed if certain conditions are met. The shell does not require this; it is done to make the code easier to read.

In other words, we could write the following and get the same results:

    # Alternate form
    
    if [ -f .bash_profile ]
    then
        echo "You have a .bash_profile. Things are fine."
    else
        echo "Yikes! You have no .bash_profile!"
    fi

    # Another alternate form
    
    if [ -f .bash_profile ]
    then echo "You have a .bash_profile. Things are fine."
    else echo "Yikes! You have no .bash_profile!"
    fi
       
**exit**

In order to be good script writers, we must set the exit status when our scripts finish. To do this, use the exit command. The exit command causes the script to terminate immediately and set the exit status to whatever value is given as an argument. For example:

    exit 0
       
exits your script and sets the exit status to 0 (success), whereas

    exit 1
       
exits your script and sets the exit status to 1 (failure).

**Testing for root**

When we last left our script, we required that it be run with superuser privileges. This is because the home_space function needs to examine the size of each user's home directory, and only the superuser is allowed to do that.

But what happens if a regular user runs our script? It produces a lot of ugly error messages. What if we could put something in the script to stop it if a regular user attempts to run it?

The id command can tell us who the current user is. When executed with the "-u" option, it prints the numeric user id of the current user.

    [me@linuxbox me]$ id -u
    501
    [me@linuxbox me]$ su
    Password:
    [root@linuxbox me]# id -u
    0

If the superuser executes id -u, the command will output "0." This fact can be the basis of our test:

    if [ $(id -u) = "0" ]; then
        echo "superuser"
    fi
        

   
In this example, if the output of the command id -u is equal to the string "0", then print the string "superuser."

While this code will detect if the user is the superuser, it does not really solve the problem yet. We want to stop the script if the user is not the superuser, so we will code it like so:

    if [ $(id -u) != "0" ]; then
        echo "You must be the superuser to run this script" >&2
        exit 1
    fi
       
With this code, if the output of the id -u command is not equal to "0", then the script prints a descriptive error message, exits, and sets the exit status to 1, indicating to the operating system that the script executed unsuccessfully.

Notice the ">&2" at the end of the echo command. This is another form of I/O direction. You will often notice this in routines that display error messages. If this redirection were not done, the error message would go to standard output. With this redirection, the message is sent to standard error. Since we are executing our script and redirecting its standard output to a file, we want the error messages separated from the normal output.

We could put this routine near the beginning of our script so it has a chance to detect a possible error before things get under way, but in order to run this script as an ordinary user, we will use the same idea and modify the home_space function to test for proper privileges instead, like so:

    function home_space
    {
        # Only the superuser can get this information
    
        if [ "$(id -u)" = "0" ]; then
            echo "<h2>Home directory space by user</h2>"
            echo "<pre>"
            echo "Bytes Directory"
                du -s /home/* | sort -nr
            echo "</pre>"
        fi
    
    }   # end of home_space
       
This way, if an ordinary user runs the script, the troublesome code will be passed over, rather than executed and the problem will be solved.

####Keyboard Input and Arithmetic####

Up to now, our scripts have not been interactive. That is, they did not require any input from the user. In this lesson, we will see how your scripts can ask questions, and get and use responses.

**read**

To get input from the keyboard, you use the read command. The read command takes input from the keyboard and assigns it to a variable. Here is an example:

    #!/bin/bash
    
    echo -n "Enter some text > "
    read text
    echo "You entered: $text"
       
As you can see, we displayed a prompt on line 3. Note that "-n" given to the echo command causes it to keep the cursor on the same line; i.e., it does not output a carriage return at the end of the prompt.

Next, we invoke the read command with "text" as its argument. What this does is wait for the user to type something followed by a carriage return (the Enter key) and then assign whatever was typed to the variable text.

Here is the script in action:

    [me@linuxbox me]$ read_demo.bash
    Enter some text > this is some text
    You entered: this is some text

If you don't give the read command the name of a variable to assign its input, it will use the environment variable REPLY.

The read command also takes some command line options. The two most interesting ones are -t and -s. The -t option followed by a number of seconds provides an automatic timeout for the read command. This means that the read command will give up after the specified number of seconds if no response has been received from the user. This option could be used in the case of a script that must continue (perhaps resorting to a default response) even if the user does not answer the prompts. Here is the -t option in action:

    #!/bin/bash
    
    echo -n "Hurry up and type something! > "
    if read -t 3 response; then
        echo "Great, you made it in time!"
    else
        echo "Sorry, you are too slow!"
    fi
       
The -s option causes the user's typing not to be displayed. This is useful when you are asking the user to type in a password or other security related information.

**Arithmetic**

Since we are working on a computer, it is natural to expect that it can perform some simple arithmetic. The shell provides features for integer arithmetic.

What's an integer? That means whole numbers like 1, 2, 458, -2859. It does not mean fractional numbers like 0.5, .333, or 3.1415. If you must deal with fractional numbers, there is a separate program called bc which provides an arbitrary precision calculator language. It can be used in shell scripts, but is beyond the scope of this tutorial.

Let's say you want to use the command line as a primitive calculator. You can do it like this:

    [me@linuxbox me]$ echo $((2+2))

As you can see, when you surround an arithmetic expression with the double parentheses, the shell will perform arithmetic evaluation.

Notice that whitespace is not very important:

    [me@linuxbox me]$ echo $((2+2))
    4
    [me@linuxbox me]$ echo $(( 2+2 ))
    4
    [me@linuxbox me]$ echo $(( 2 + 2 ))
    4

The shell can perform a variety of common (and not so common) arithmetic operations. Here is an example:

    #!/bin/bash
    
    first_num=0
    second_num=0
    
    echo -n "Enter the first number --> "
    read first_num
    echo -n "Enter the second number -> "
    read second_num
    
    echo "first number + second number = $((first_num + second_num))"
    echo "first number - second number = $((first_num - second_num))"
    echo "first number * second number = $((first_num * second_num))"
    echo "first number / second number = $((first_num / second_num))"
    echo "first number % second number = $((first_num % second_num))"
    echo "first number raised to the"
    echo "power of the second number   = $((first_num ** second_num))"
       
Notice how the leading "$" is not needed to reference variables inside the arithmetic expression such as "first_num + second_num".

Try this program out and watch how it handles division (remember this is integer division) and how it handles large numbers. Numbers that get too large overflow like the odometer in a car when you exceed the number of miles it was designed to count. It starts over but first it goes through all the negative numbers because of how integers are represented in memory. Division by zero (which is mathematically invalid) does cause an error.

I'm sure that you recognize the first four operations as addition, subtraction, multiplication and division, but that the fifth one may be unfamiliar. The "%" symbol represents remainder (also known as modulo). This operation performs division but instead of returning a quotient like division, it returns the remainder. While this might not seem very useful, it does, in fact, provide great utility when writing programs. For example, when a remainder operation returns zero, it indicates that the first number is an exact multiple of the second. This can be very handy:

    #!/bin/bash
    
    number=0
    
    echo -n "Enter a number > "
    read number
    
    echo "Number is $number"
    if [ $((number % 2)) -eq 0 ]; then
        echo "Number is even"
    else
        echo "Number is odd"
    fi
       
Or, in this program that formats an arbitrary number of seconds into hours and minutes:

    #!/bin/bash
    
    seconds=0
    
    echo -n "Enter number of seconds > "
    read seconds
    
    hours=$((seconds / 3600))
    seconds=$((seconds % 3600))
    minutes=$((seconds / 60))
    seconds=$((seconds % 60))
    
    echo "$hours hour(s) $minutes minute(s) $seconds second(s)"
    
####**FLOW CONTROL - REVISITED**####

In the previous lesson on flow control we learned about the if command and how it is used to alter program flow based on a condition. In programming terms, this type of program flow is called branching because it is like traversing a tree. You come to a fork in the tree and the evaluation of a condition determines which branch you take.

There is a second and more complex kind of branching called a case. A case is multiple-choice branch. Unlike the simple branch, where you take one of two possible paths, a case supports several possible outcomes based on the evaluation of a condition.

You can construct this type of branch with multiple if statements. In the example below, we evaluate some input from the user:

    #!/bin/bash
    
    echo -n "Enter a number between 1 and 3 inclusive > "
    read character
    if [ "$character" = "1" ]; then
        echo "You entered one."
    else
        if [ "$character" = "2" ]; then
            echo "You entered two."
        else
            if [ "$character" = "3" ]; then
                echo "You entered three."
            else
                echo "You did not enter a number"
                echo "between 1 and 3."
            fi
        fi
    fi
Not very pretty.

Fortunately, the shell provides a more elegant solution to this problem. It provides a built-in command called case, which can be used to construct an equivalent program:

    #!/bin/bash
    
    echo -n "Enter a number between 1 and 3 inclusive > "
    read character
    case $character in
        1 ) echo "You entered one."
            ;;
        2 ) echo "You entered two."
            ;;
        3 ) echo "You entered three."
            ;;
        * ) echo "You did not enter a number"
            echo "between 1 and 3."
    esac

The case command has the following form:

    case word in
        patterns ) statements ;;
    esac
       
case selectively executes statements if word matches a pattern. You can have any number of patterns and statements. Patterns can be literal text or wildcards. You can have multiple patterns separated by the "|" character. Here is a more advanced example to show what I mean:

    #!/bin/bash
    
    echo -n "Type a digit or a letter > "
    read character
    case $character in
                # Check for letters
        [a-z] | [A-Z] ) echo "You typed the letter $character"
                ;;
    
                # Check for digits
        [0-9] )     echo "You typed the digit $character"
                ;;
    
                # Check for anything else
        * )         echo "You did not type a letter or a digit"
    esac
       
Notice the special pattern "*". This pattern will match anything, so it is used to catch cases that did not match previous patterns. Inclusion of this pattern at the end is wise, as it can be used to detect invalid input.

**Loops**

The final type of program flow control we will discuss is called looping. Looping is repeatedly executing a section of your program based on a condition. The shell provides three commands for looping: while, until and for. We are going to cover while and until in this lesson and for in a future lesson.

The while command causes a block of code to be executed over and over, as long as a condition is true. Here is a simple example of a program that counts from zero to nine:

    #!/bin/bash
    
    number=0
    while [ $number -lt 10 ]; do
        echo "Number = $number"
        number=$((number + 1))
    done
       
On line 3, we create a variable called number and initialize its value to 0. Next, we start the while loop. As you can see, we have specified a condition that tests the value of number. In our example, we test to see if number has a value less than 10.

Notice the word do on line 4 and the word done on line 7. These enclose the block of code that will be repeated as long as the condition is met.

In most cases, the block of code that repeats must do something that will eventually change the outcome of the condition, otherwise you will have what is called an endless loop; that is, a loop that never ends.

In the example, the repeating block of code outputs the value of number (the echo command on line 5) and increments number by one on line 6. Each time the block of code is completed, the condition is tested again. After the tenth iteration of the loop, number has been incremented ten times and the condition is no longer true. At that point, the program flow resumes with the statement following the word done. Since done is the last line of our example, the program ends.

The until command works exactly the same way, except the block of code is repeated as long as the condition is false. In the example below, notice how the condition has been changed from the while example to achieve the same result:

    #!/bin/bash
    
    number=0
    until [ $number -ge 10 ]; do
        echo "Number = $number"
        number=$((number + 1))
    done

**Building a menu**

One common way of presenting a user interface for a text based program is by using a menu. A menu is a list of choices from which the user can pick.

In the example below, we use our new knowledge of loops and cases to build a simple menu driven application:

    #!/bin/bash
    
    selection=
    until [ "$selection" = "0" ]; do
        echo ""
        echo "PROGRAM MENU"
        echo "1 - display free disk space"
        echo "2 - display free memory"
        echo ""
        echo "0 - exit program"
        echo ""
        echo -n "Enter selection: "
        read selection
        echo ""
        case $selection in
            1 ) df ;;
            2 ) free ;;
            0 ) exit ;;
            * ) echo "Please enter 1, 2, or 0"
        esac
    done
       
The purpose of the until loop in this program is to re-display the menu each time a selection has been completed. The loop will continue until selection is equal to "0," the "exit" choice. Notice how we defend against entries from the user that are not valid choices.

To make this program better looking when it runs, we can enhance it by adding a function that asks the user to press the Enter key after each selection has been completed, and clears the screen before the menu is displayed again. Here is the enhanced example:

    #!/bin/bash
    
    function press_enter
    {
        echo ""
        echo -n "Press Enter to continue"
        read
        clear
    }
    
    selection=
    until [ "$selection" = "0" ]; do
        echo ""
        echo "PROGRAM MENU"
        echo "1 - display free disk space"
        echo "2 - display free memory"
        echo ""
        echo "0 - exit program"
        echo ""
        echo -n "Enter selection: "
        read selection
        echo ""
        case $selection in
            1 ) df ; press_enter ;;
            2 ) free ; press_enter ;;
            0 ) exit ;;
            * ) echo "Please enter 1, 2, or 0"; press_enter
        esac
    done

It is time to cover the remaining flow control statement, **for**. Like while and until, for is used to construct loops. for works like this:

    for variable in words; do
        statements
    done
     
In essence, for assigns a word from the list of words to the specified variable, executes the statements, and repeats this over and over until all the words have been used up. Here is an example:

    #!/bin/bash
    
    for i in word1 word2 word3; do
        echo $i
    done
     
In this example, the variable i is assigned the string "word1", then the statement echo $i is executed, then the variable i is assigned the string "word2", and the statement echo $i is executed, and so on, until all the words in the list of words have been assigned.

The interesting thing about for is the many ways you can construct the list of words. All kinds of substitutions can be used. In the next example, we will construct the list of words from a command:

    #!/bin/bash
    
    count=0
    for i in $(cat ~/.bash_profile); do
        count=$((count + 1))
        echo "Word $count ($i) contains $(echo -n $i | wc -c) characters"
    done


Here we take the file .bash_profile and count the number of words in the file and the number of characters in each word.


> And with this, we finish wish a brief introduction to shell scripting. However , we have provided below with a set of links to help you understand shell scripting better.

[How to write a shell script ?][2]
[Userdefined variables][3]
[Printing and accessing userdefined variables][4]
[Shell Arithmetic][5]
[More about quotes][6]
[for loop][7]
[while loop][8]
[if..then..fi][9]



       


  [1]: http://www.tutorialspoint.com/unix/unix-using-variables.htm
  [2]: http://www.freeos.com/guides/lsst/ch02sec01.html
  [3]: http://www.freeos.com/guides/lsst/ch02sec03.html
  [4]: http://www.freeos.com/guides/lsst/ch02sec05.html
  [5]: http://www.freeos.com/guides/lsst/ch02sec07.html
  [6]: http://www.freeos.com/guides/lsst/ch02sec08.html
  [7]: http://www.freeos.com/guides/lsst/ch03sec06.html
  [8]: http://www.freeos.com/guides/lsst/ch03sec07.html
  [9]: http://www.freeos.com/guides/lsst/ch03sec03.html
