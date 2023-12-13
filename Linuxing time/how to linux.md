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
| `/` | root path, containing `/home, /bin, /var` and all the crucial folders |
| `/root` | path only accessible by root user, a.k.a root home folder |
| `/home` | parent folder containing main folders of all users: `/home/[username]` |
| `/dev/null` | literally a black hole, sucks any useless output and discard it, never to be found again | `ping 1.1.1.1 >> /dev/null` : Throw `ping` output into `/dev/null`|
| `~` | a classy way to describe current user's folder, contains `~/Documents`, `~/Desktop` and all that |

#### while you're already learning about `/dev/null`, here's a thing or two about FILE DESCRIPTOR

| Descriptor number |  |
| --- | --- |
| 0 | stdin |
| 1 | stdout | 
| 2 | stderr |


---

