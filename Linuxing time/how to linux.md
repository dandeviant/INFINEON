Basic Linux/UNIX commands, mostly on files and directory navigation. 

>`man` is always your best friend\
> \
>	- Sun Tzu, probably


---
#### Directory Navigation

| Commands | For what? | 
| --- | --- | 
| `cd ..` | cd to previous directory path | 
| `cd /` | cd to root | 
| `cd` OR `cd ~` | cd to home | 
| `cd -` | cd to previously-visited path | 

---
#### File manipulation

| Commands | For what ? |
| --- | --- | 
| `file [filename]` | Identify file type |
| `[commands] > filename.txt ` | Save stdout into new file, REPLACE old content for existing file with same name |
| `[commands] >> filename.txt` | ADD stdout into end of existing file with contents |
| `[commands] \| tee [filename]` | To save output while running sessions, perfect for logging purposes |
| --- | --- |
| `cat` | Display file contents |
| `more` | Display file contents the old-school way|
| `less` | cleaner version of more|
| `head` | Display first 10 lines|
| `tail` | Display last 10 lines |
| --- | --- | --- |
| `touch [filename]` | create blank unformatted file |  
| `nano` | enter nano, a terminal-based file editor |
| `vim` OR `vi` | enter Vi or his child, Vim (Vi Improved); another text editor |
| --- | --- | --- |
| `mv [source path] [dest path]` | move file from source to destination |
| `mv [old filename] [new filename] ` | rename file |
| `cp [source] [dest]` | copy source to destination path |
| `rm [filename]` | Delete file |
| `rm -r [foldername]` | Delete folder and all its content recursively|
| `rmdir [foldername]` | Delete empty folder

---
#### Important directories in UNIX/Linux storage

| Path | What the hell is it? | |
| --- | --- | --- |
| `/` | root path, containing `\home, \bin, \var` and all the crucial folders |
| `/root` | path only accessible by root user, a.k.a root home folder |
| `/home` | parent folder containing main folders of all users: `/home/[username]` |
| `/dev/null` | literally a black hole, sucks any useless output and discard it, never to be found again | `ping 1.1.1.1 >> /dev/null` : Throw `ping` output into `/dev/null`|
| `~` | a classy way to describe current user's folder, contains `~/Documents`, `~/Desktop` and all that |

#### while you're already learning about `/dev/null`, here's a thing about FILE DESCRIPTOR

| Descriptor number |  |
| --- | --- |
| 0 | stdin |
| 1 | stdout | 
| 2 | stderr |


---
### chmod it and mod it

> [! ]- Basic things about chmod:
> chmod = **ch**ange **mod**e
> 
> - Used to change how users can access files/directories by modifying read/write/execute permissions
> - 3 types of users in Linux and its symbols 
> 	- Owner/User (u), 
> 	- Group (g), 
> 	- Others (o); 
> 	- Include all 3 types (a)
> 
> - 3 main permissions (symbols/octal value)
> 	- Read (r / 4)
> 	- Write (w / 2)
> 	- Execute (x / 1)


How chmod works (SYMBOLIC mode):

| Commands | Description | 
| --- | --- |
| `chmod u+rwx README.txt` | Add all permission to owner |
| `chmod uo-wx README.txt` | Remove write, execute permission from owner and others |

Also how chmod works (OCTAL mode, kind of easier than symbolic, to me anyway):

| Sample | Owner | Group | Others |
| --- | --- | --- | --- |
| `chmod 777 README.txt` | `7 = 4(r) + 2(w) + 1(x)`  | `7 = 4(r) + 2(w) + 1(x)` | `7 = 4(r) + 2(w) + 1(x)` |
| `chmod 642 README.txt` | `6 = 4(r) + 2(w)` | `4(w)` | `2(r)` |

#### Also to add: chown for chmod

> [! ]- What the hell is chown?
> - chown = **ch**ange **own**ership
> - Used to assign the ownership of a specified file/folder to a different user
> 
> - Basic syntax:
> `chown [new user] [file]`

| Sample | Desc | 
| --- | --- |
| `chown daniel README.txt` | change `README.txt` owner to `daniel` |




