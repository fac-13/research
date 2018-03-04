# Command Line Research

## Mac Users
Locate the .bash file in the home directory 
```bash
ls -a
```
Open the file in terminal 
```bash
nano ~/.bash
```


## Linux users
Locate the .bashrc file in the home directory 
```bash
ls -a
```
Open the file in terminal 
```bash
nano ~/.bashrc
```
---
## What is PS1?
Your Bash prompt configuration is stored in the PS1 variable.
So first save the contents of the PS1 variable into a new variable, run the following command: `DEFAULT=$PS1`
To revert back to the default just run `PS1=$DEFAULT`

- `PS1="\\u\\$ "` If you want to see only the userName
>`isaac$`

- `PS1="\\u:\\w\\$ "` If you want to see your path
>`isaac:~/tom-isaac$` If you are in tom-isaac dir
`isaac:~$` If you are in home directory
---
## Add Git Branch Name to Terminal Prompt

Open `~/.bash_profile` and add the following content into your PS1 variable (after color codes)


```
export PS1= "(\\$(git branch 2>/dev/null | grep '^*' | colrm 1 2)) \\$"
```
---
## Colours 

> Here’s what you need to know: You must include the entire color code information between the `\[`  and `\]` character's. Inside the tag, you must begin with either `\033[` or `\e[ `to indicate to Bash that this is color information. Both `\033[` and `\e[` do the same thing. `\e[` is shorter so might be more convenient to use, but we’ll use `\033[ `here as it matches what’s used by default. At the end of the tag, you must end with `m\ `to indicate the end of a color tag.

> Bash allows you to change the color of foreground text, add attributes like “bold” or “underline” to the text, and set a background color.

**Foreground colour** = "\\\[\\033\[**X**m\\\]"

Where X is the foreground colour:
-   Black: 30
-   Blue: 34
-   Cyan: 36
-   Green: 32
-   Purple: 35
-   Red: 31
-   White: 37
-   Yellow: 33
-   No colour: 00

**Foreground colour and attribute** = "\\\[\\033\[**Y**;**X**m\\\]"

Where Y is the attribute:
-   Normal Text: 0 (default)
-   Bold or Light Text: 1
-   Dim Text: 2
-   Underlined Text: 4
-   Blinking Text: 5
-   Reversed Text: 7 (This inverts the foreground and background colors, so you’ll see black text on a white background if the current text is white text on a black background.)
-   Hidden Text: 8

You can also specify a **background color**, but you can’t add an attribute to a background color.
Here are the values for background colors:

-   Black background: 40
-   Blue background: 44
-   Cyan background: 46
-   Green background: 42
-   Purple background: 45
-   Red background: 41
-   White background: 47
-   Yellow background: 43

**Forground colour (X) and background colour (Z)** = 
"\\\[\\033\[**X**m\\\]\\\[\\033\[**Z**m\\\]"

foreground colour (X) with attribute (Y) and background colour (Z):
"\\\[\\033\[**Y**;**X**m\\\]\\\[\\033\[**Z**m\\\]"

##### Example 

- green text: "\\\[\\033\[32m\\\]"
- bold red text: "\\\[\\033\[1;31m\\\]"
- blue text on white background: "\\\[\\033\[34m\\\]\\\[\\033\[47m\\\]"
- bold black text on yellow background: "\\\[\\033\[1;30m\\\]\\\[\\043\[47m\\\]"


So with
```bash
PS1="\[\033[42m\]\[\033[31m\]\u@\h:\w\$ "
```
We have red foreground and green background.

To stop the colour information affecting all the text after the code you need to put the no colour code before the text you want. e.g.
```bash
PS1="\[\033[42m\]\[\033[31m\]\u\[\033[00m\]@\h:\w\$ "
```
Now only the user name is coloured.

---
## KEYBOARD SHORTCUTS 

- Esc + F   move forwards a chunk 

- Esc + B    move backwards a chunk 

- Ctrl + A   move to start 

- Ctrl + E   move to end 

- Ctrl + K  deletes line from position of cursor onwards 

- Tab    auto complete 

### CREATING SHORTCUTS/FUNCTIONS 


#### FUNCTIONS 

alias chrome=’open –a “Google Chrome”’ 

#### SHORTCUTS 

alias fac=”cd desktop/coding/fac” 

NOTE: 

curly quotations 

close terminal and open again

---
#### *Resources:*
- [How to Customize (and Colorize) Your Bash Prompt](https://www.howtogeek.com/307701/how-to-customize-and-colorize-your-bash-prompt/)
- [Setting Up Your Workspace on Mac](https://classroom.udacity.com/courses/ud775/lessons/2980038599/concepts/33331589510923)
- [Setting Up Your Workspace on Windows](https://classroom.udacity.com/courses/ud775/lessons/2980038599/concepts/33417185870923)