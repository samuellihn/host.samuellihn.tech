# Cybersecurity 3/22/22: Linux Shell

--
## Why are we learning this?
- Some things are much easier to do if you know the Linux Shell
- Basic computer skill for IT/fixing and navigating computers
- Access low-level computer configuration (initram, etc.)
- Improves access to open-source software (package managers, etc.)

--
## Enables us to use lower-level cybersecurity tools

--
## Vocabulary:
| Term | Definition
| --- | ----
| **Terminal Emulator** | the program (`lxterm`, Windows Terminal) that displays the **shell**
| **Shell** | a computer program that lets you run commands on the command line (`bash`, `fish`, `cmd`, `PowerShell`)
| **Prompt** | Indicator that the shell is ready to recieve system commands


--

To open the terminal, 

- Windows: `cmd` or `Powershell` in windows search bar
- MacOS: `Terminal`
- Linux:  `Ctrl + Alt + t`

--

# Disclaimer
⚠ ⚠ ⚠ ⚠ ⚠

Everything done in the command line is usually **final**. 

**Think before you type**

Luckily, we're using virtual machines, so you won't actually ruin your own system--you'll ruin someone else's

**If you use the shell in your own system, be careful!**

---
# Paths, Directories, and Folders
--
- The **path** tells you where you are in your **filesystem**
- **Filesystems can be represented as a tree**

```
>>> tree ~
├── Documents
│   ├── document.txt
│   └── document2.md
└── Downloads
    └── download.txt
```
Paths can either be **relative** to your current directory or **absolute** to the **system root**

--

**Directories** "folders" contain files. They are specified with a slash in front:
```
Downloads/
Documents/
```

--

- `~` refers to the **home directory** (`/home/user/`)
- Equivalent to `C:\Users\user`
- Where you should do all your work
--

## `cd`
- `cd` is a command to **change directory**

--
## Relative Paths

Relative paths ("relpaths") are specified **without** a slash in front
For example, if I'm in home (`/home/samuellihn/`) and I want to go to `Documents/`, I can type:

```
cd Documents/
# Equates to cd /home/samuellihn/Documents/, since I'm in /home/samuellihn/
```

--
## Relpath Specifiers
- `.` refers to the **current directory**
- `../` refers to the parent directory

```
root@SAMUELLIHN-P52:~/Documents tree .
├── Documents
│   ├── document.txt
│   └── document2.md
└── Downloads
    └── download.txt
root@SAMUELLIHN-P52:~/Documents cd ..
root@SAMUELLIHN-P52:~ cd ..
```

> You can also chain this to specify files: ex. `cd ../Downloads`

--
## Absolute Path

Absolute paths are relative to the **root** (`/`) of your system
> ⚠ doing things outside of root is dangerous, type with care

Will be longer than a relpath

You can use `pwd` to find your current absolute path
```
root@SAMUELLIHN-P52:~/Documents pwd
/home/samuellihn/Documents

# abspath: /home/samuellihn/Documents/
# relpath to home (~): Documents/
```
---
# Navigation commands
--
## `ls`

`ls` allows you to **list** the current files in a directory

Without typing in any options/args, `ls` will list the contents of your current directory

```sh
root@SAMUELLIHN-P52:~/Documents# ls
document.txt  document2.md
```


--
You can specify the directory with an argument:

```
# list contents of root
root@SAMUELLIHN-P52:~/Documents# ls / 
bin   dev  home  lib    lib64   lost+found  mnt  proc  run   snap  sys  usr
boot  etc  init  lib32  libx32  media       opt  root  sbin  srv   tmp  var
```

--

`ls` also has options:

Option | effect 
--- | ---
`-a` | Lists all files, including hidden ones starting with `.`
`-l` | Lists all files in long list format

--
## Command options & arguments
Commands can take in **arguments** and **options**
- **Arguments** give the command more information about our intentions
- **Options** are used to "switch" the behavior of a command, and start with a `-` or `--`
---

> Usually, single slash options are for the short letter commands (`-a`)
> And double slash options are for the verbose, long commands (`--all`)

--

## Getting Help 
You can use the `man` command or the `-h` or `--help` commands to get more information about a command

`-h` will display a **short help message** when used after a command

Whereas `--help` will display a **longer help message**

--
### `man`

`man` will display a **man**ual (manpage) of a specific command

It will bring up a terminal program called `less` and the manual page can be displayed:

- Inside `less`, use `Space` to go down one line and `q` to quit

Manpages are also available online [here](https://man7.org/linux/man-pages/)


---
# Other useful file manipulation commands

--
### `mkdir`

Creates a directory. (**M**a**k**e **dir**ectory) Can specify an absolute or relative path.


```
root@SAMUELLIHN-P52:~# tree -L 1
.
├── Documents
└── Downloads

2 directories, 0 files
root@SAMUELLIHN-P52:~# mkdir NewDirectory
root@SAMUELLIHN-P52:~# tree -L 1
.
├── Documents
├── Downloads
└── NewDirectory

3 directories, 0 files
```
--
### `mv`
Moves a file

```
root@SAMUELLIHN-P52:~# tree -L 2
.
├── Documents
│   ├── document.txt
│   └── document2.md
├── Downloads
│   └── download.txt
└── NewDirectory

3 directories, 3 files
```
---
```
root@SAMUELLIHN-P52:~/Documents# mv document2.md ../Downloads/
root@SAMUELLIHN-P52:~/Documents# tree ..
..
├── Documents
│   └── document.txt
├── Downloads
│   ├── document2.md
│   └── download.txt
└── NewDirectory

3 directories, 3 files
```

--
You can also rename files with `mv`

```
root@SAMUELLIHN-P52:~/Documents# ls
document.txt
root@SAMUELLIHN-P52:~/Documents# mv document.txt newname.txt
root@SAMUELLIHN-P52:~/Documents# ls
newname.txt
```
--
### `touch`

Creates an empty file as specified


```
root@SAMUELLIHN-P52:~/Documents# ls
newname.txt
root@SAMUELLIHN-P52:~/Documents# touch empty.txt
root@SAMUELLIHN-P52:~/Documents# ls
empty.txt  newname.txt
```
--

### `cp`

Copies a file (similar syntax to `mv` except that it copies)
```
root@SAMUELLIHN-P52:~# tree
.
├── Documents
│   └── newname.txt
├── Downloads
│   ├── document2.md
│   └── download.txt
└── NewDirectory

3 directories, 3 files
root@SAMUELLIHN-P52:~# cp Documents/newname.txt Downloads/
root@SAMUELLIHN-P52:~# tree
.
├── Documents
│   └── newname.txt
├── Downloads
│   ├── document2.md
│   ├── download.txt
│   └── newname.txt
└── NewDirectory

3 directories, 4 files
```

--

## `rm`
> ⚠ Unlike what you're used to, **there are no undos!** Be careful.

Allows you to remove a file

```
root@SAMUELLIHN-P52:~/Documents# ls
empty.txt  newname.txt
root@SAMUELLIHN-P52:~/Documents# rm empty.txt
root@SAMUELLIHN-P52:~/Documents# ls
newname.txt
```

With the `-r` option, you can **recursively delete a directory**

--
```
root@SAMUELLIHN-P52:~# tree
.
├── Documents
│   └── newname.txt
├── Downloads
│   ├── document2.md
│   ├── download.txt
│   └── newname.txt
└── NewDirectory

3 directories, 4 files
root@SAMUELLIHN-P52:~# rm -r Documents/
root@SAMUELLIHN-P52:~# tree
.
├── Downloads
│   ├── document2.md
│   ├── download.txt
│   └── newname.txt
└── NewDirectory

2 directories, 3 files
```

---
# Tips & Considerations

--

You can press `Tab` to autocomplete files (keep pressing `Tab` to cycle through possibilities)

--
You can press the up and down arrow keys to scroll through previous commands

You can use right and left arrow keys to edit commands

`Home` and `End` also come in handy with longer commands

---

# Users and Permissions

--

Installing packages and changing system configuration is restricted to **superusers** (administrators)

You can become a **superuser** using the `sudo` command (you must know the superuser password)

`sudo su` will permanently switch over to the superuser
> The `sudo` password expires after 15 minutes

`whoami` displays the currently signed in user

--
## File Permissions
Sometimes files are protected and only superusers or file owners can access them
We can can change the permissions of a flie with `chmod` 


Learn more [here](https://www.howtogeek.com/437958/how-to-use-the-chmod-command-on-linux/) 

---

# Viewing and manipulating files
--

To display the contents of a file, use `cat` (con**cat**enate the file to the terminal)

```
root@SAMUELLIHN-P52:~/Downloads# ls
document2.md  download.txt  newname.txt
root@SAMUELLIHN-P52:~/Downloads# cat document2.md
# hello! This is document 2
hello!
root@SAMUELLIHN-P52:~/Downloads#
```

This will always display the file in **plain text** no mater what file it is

--

You can also use a **pager** like `less`, which splits the file up into pages
- Use `space` to go to the next page and `q` to quit

> Why do we use this? Why not just scroll the terminal?
> 
> Some terminals cannot scroll (common in very barebones environments like the default Ubuntu Server shell, or in the Arch installer)

--

## Editing files

You can edit plain text files with a command-line based **text editor**

`nano` comes preinstalled with Ubuntu and can be used to edit configuration files, code, etc.

To edit a file, use `nano {filename}`

--

- Inside `nano`, use the arrow keys to move around

- Backspace, delete, text insertion all work

- To save and quit, use `Ctrl-X`, then press `Y` then `Enter`

- To quit without saving, use `Ctrl-X` but answer `N` to "Save modified buffer?"

`nano` gives you help with other commands with a hint at the bottom of the screen 

---
### Command reference
--

Command | Action
---|---
`ls` | Lists files in directory
`cd` | Change current directory
`man` | Display manpage for command

--

Command | Action
---|---
`touch` | Creates a file with the specified name
`mkdir` | Create a directory
`mv` | Moves or renames a file
`cp` | Copies a file
`rm` | Remove a file (`-r` to remove directory)

--

Command | Action
---|---
`cat` | Display contents of entire file
`less` | Pages a file
`nano` | Plain text editor that you should use. Preinstalled in Ubuntu & most Linux
`vim` | Plain text editor that you shouldn't use
