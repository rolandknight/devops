# DevOps Tools Part1 - Basic Client Tools

Let's strip down to the basics first - it's all about text. Implementation source, and configuration data, is primarily text.
Source code management - text. Server management and log - also text. So we better be really good at handling...you got it - TEXT.
In this document, installation and the very basics of the following tools are covered:

- **Git:** Distributed version control
- **Terminal/Shell** (local bash shell and SSH)
- **Command line tools**: Posix tools like cp, mv, sed, awk and other useful tools like curl.
- **Text editing**: Visual Studio Code (text editor and much more), plus an overview of vi (you can't avoid it - so learn the basics!).
  And don't forget **vi**. You can't escape it so we'll cover that too.

These are the basic tools you'll need. If you've already got great tools you're comfortable with, then great! Skip this document and
move on to the next document. Otherwise, read on...

## Git, Terminal/Shell, and Command Line Tool Installation

Here we cover foundational commandline tools. Resist the urge to install a wackload of tools -
be ephermeral and modern. Avoid pollution of your system and use containers when possible. 
Note that development tools like Java, Go, etc are covered in another document.

### Linux/MAC

Linux and MAC users already have terminal and many command line tools installed.
The only common tools you'll need are:
- **Git** Go old school and play with the command line version of [Git](https://git-scm.com/).
  Learn how to use this first, then move up to a UI like the one provided with Visual Studio Code cover later in this document.
- **curl:** [Curl](https://curl.haxx.se/) is swiss army knife URL based data transfer tool.
  Generally not installed by default but is an essential tool.
#### **Installation:** Install the above tools on Ubuntu like so:
  ```
  apt-get upgrade
  apt-get install git curl
  ```
  For other OSes like MAC and Redhat, have a look [here](https://git-scm.com/downloads). 
  For other tools like **curl**, we'll leave as a [googling](https://www.merriam-webster.com/dictionary/google) exercise.
  
- That's it! We like to be lean. There are many more useful tools around such as [**rsync**](https://rsync.samba.org/?) but are either not so portable
  or can be run in a container. Try some out after you install docker which, of course, is covered in another doc.

### Windows 7/10

Windows does not come with the base tools required for Git. Fortunately, the Git install package comes with a Posix-like toolset [Min GW](http://www.mingw.org/ ).

#### Install Git and friends

- Ready to install? Get it from [here](https://github.com/git-for-windows/git/releases/download/v2.17.0.windows.1/Git-2.17.0-64-bit.exe)
- This installs MinGW (including SSH), Git and Git GUI (ignore that for now). Now you can use Git via command line by running **Git Bash** from your start menu.
- Now that you have Git, read up on it [here](https://git-scm.com/book/en/v2).

### Common Setup and Usage

You can't quite use Git yet. A little setup is required...

- **Git Setup:**
  ```
  git config --global user.name "<username>"
  git config --global user.email <email>
  ```
  The above is used to identify commit owner - so it's important to be correct.

- **Git Setup (windows only):**
  ```
  git config --global core.autocrlf input
  ```
  This takes care of the age old Linux vs Windows line ending problem. The best practice is to use an editor that handles Linux line endings properly and enable this option to prevent inadvertantly checking in Windows line endings. Note the assumption is being made that you really like Linux line endings. If not, I do feel sorry for you.

- **SSH Key Generation**: Generate yourself a public/private key pair.

```
ssh-keygen
```
A public private key pair is required for SSH. You can specify a password for extra security but this will be a pain in the ass for you later. Just keep your private key safe (in ~/.ssh/id_rsa) and NEVER share it with anyone or transfer/copy it anywhere except in a secure backup.

#### All Set
This is just to get you started. Separate documents will go deeper into Git and branch/merge strategies for your project. Meanwhile read up on Git [here](https://git-scm.com/book/en/v2).

## Text Editor - Visual Studio Code

Why [Visual Studio Code](https://code.visualstudio.com/). So many reasons. In 3 short
years it has become one of top choices for source code editing - google it and
you'll quickly understand why. Just a few key features you'll love:
1. Fast simple text editor with workspaces
1. Cross platform (Windows 7/10, Linux, MAC)
1. Integrated markdown - kiss Word goodbye for project documentation.
1. Slick [Git integration](https://code.visualstudio.com/docs/editor/versioncontrol
)
1. Plugins - thousands of them. [Git repo viewer](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory), [UML rendering](https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml), to name a few.

1. Integrated terminal
1. Integrated debugging

### So what is it?

Visual Studio Code is an open source editor from Microsoft (note that it is not related to Visual Studio). The editor is Node.js based built on top of [Electron](https://electronjs.org/).
- Download and install Visual Studio code from [here](https://code.visualstudio.com/download).

### Small and Useful Tool
- **Clipboard History Manager:** [ClipX](https://bluemars.org/clipx/) very useful little tool - download from [here](https://bluemars.org/clipx/clipx-1.0.3.9g-setup-x64.exe).

### Fallback to vi

There will always be cases where it is inconvenient to edit using your favourite editor. Luckily, **vi** is almost always available and it works well in a basic text terminal window. This is both great and horrendous since **vi** has very cryptic modes and commands but you will need it someday - it's just a matter of when. And when you do you can use this [reference card](http://web.mit.edu/merolish/Public/vi-ref.pdf).

Don't have time to even read that one page vi cheat sheet? Here's what you can get by with:
- **\<ESC\>** Return to command mode (vi starts in command mode)
- **Navigation:** **h**=left, **j**=down, **k**=up, **l**=right
- **More Nav:** **0**=line start, **$**=line end, **1G**=file start, **G**=file end
- **Cut & Paste**: **\<n\>dd**=cut n lines, **p**=paste after current line
- **Search/Replace**: **/\<regex\>**=search,**/\<regex\>/\<replace\>/g**=replace all 
- **Save/Exit**: **ZZ**=Save and exit, **:q!**=Exit without save

## What's Next
Lot's of homework to do already. Some recommended reading:
- [**Git**](https://git-scm.com/book/en/v2)
- [**Bash**](https://www.gnu.org/software/bash/manual/)
- TODO - add linux commands including sed, awk, ssh, rsync
