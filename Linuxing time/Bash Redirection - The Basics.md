
A bash script is commonly a set of commands. There are three standard file descriptors of any command:

- 0 → stdin
- 1 → stdout
- 2 → stderr

There are two commonly used redirection _operators:_
| `>` | output (This is the _default_ operator for fd 1) |
| `<` | input (This is the _default_ operator for fd 0) |

# Output Redirection

The most basic example of redirecting the output of a command to a file is:

```bash
echo "hello world" 1> foo.txt
```


This redirects the output of the _echo_ command to a file called `foo.txt`. Since 1 is the default file descriptor for the `>` operator, it can be omitted. Hence the following is a valid bash command.

```bash
echo “hello world” > foo.txt
```

## No Clobber and Forced Redirection

If the `noclobber` option to the `set` built-in has been enabled, the output redirection will fail if the file exists and is a regular file.

The _forced redirection operator_ `>|` will redirect the output to the file even if the `noclobber` option is set.

# Input Redirection

Likewise, the most canonical example of the `<` operator is the following command:

```bash
read input < foo.txt && echo "${input}"
```

# Assigning file descriptors to files

Turns out there’s more that can be done with file descriptors in bash than just use it to redirect the input or output of commands.

In particular, bash allows of to _assign_ file descriptors to files, and then operate on those files by just using the descriptors. This can be done with the `exec` command:

`exec Z > file`

which will assign file descriptor Y to a file for _output._
```bash
~ : exec 5> bar.txt  
~ : echo "bye, world" >& 5  
~ : cat bar.txt  
bye, world
```

The `exec X < file` command allows one to assign file descriptor X to a file for _input._

```
~ : exec 6< bar.txt  
~ : read -n input <& 6 && echo "${input}"  
bye, world
```


## Gotcha

Using _file descriptor_ [_5_ might cause problems](https://groups.google.com/g/gnu.bash.bug/c/E5Vdqv3tO1w?pli=1) since when Bash creates a child process, as with `exec`, the child inherits fd 5.

# Closing File Descriptors

Of course, the fact that bash allows one to _open_ file descriptors also means there must surely be a way to _close_ them. And there is!

`exec Z <&-`

is the syntax to _close_ file descriptor Z, where Z can include values like 0, 1 and 2.

```
~ : exec 6< bar.txt  
~ : read input <& 6 && echo "${input}"  
bye, world  
~ : exec 6<&-  
~ : read input <& 6 && echo "${input}"  
bash: 6: bad file descriptor
```

In the snippet above, we see that we can assign file descriptor 6 for input and successfully read from the file by just referring to the file descriptor 6. We then then _close file descriptor 6,_ after which trying to read from the file using descriptor 6 results in an error.

We can even close the 3 standard file descriptors (0, 1 and 2) using this command. Basic shell functions like `cat` while rely on these file descriptors become non-functional in these cases.
```bash
bash-5.0$ echo "hello world" > foo.txt  
bash-5.0$ cat foo.txt  
hello world  
bash-5.0$ exec 1<&-  
bash-5.0$ cat foo.txt  
cat: stdout: Bad file descriptor
```

When closing stdin (fd 0), the bash prompt understandably exits.
```
bash-5.0$ exec 0<&-  
bash-5.0$ exit
```


And trying to close stderr (fd 2) results in a hang, on both macOS and Linux.

Closing file descriptors becomes important, inasmuch as child processes inherit all of the open file descriptors of the parent process. Closing a file descriptor prevents it from being inherited by a child process.

# Moving File Descriptors

Yes, that’s right.

Bash allows one to _move_ file descriptors.

## Moving Input File Descriptors

exec X<&Y-

moves the file descriptor Y to file descriptor X, or the standard input (file descriptor 0) if X is not specified.

Y is closed after being duplicated to X.
```bash
bash-5.0$ echo "hello world" > foo.txt  
bash-5.0$ exec 6< foo.txt  
bash-5.0$ read input <& 6 && echo "${input}"  
hello world  
bash-5.0$ exec 7<&6-  
bash-5.0$ read input <& 6 && echo "${input}"  
bash: 6: Bad file descriptor  
bash-5.0$ read input <& 7 && echo "${input}"  
hello world
```

In the above code snippet, we see that once file descriptor 6 has been moved to 7, one can no longer access input from the file foo.txt using fd 6. However, one can access input from the file using fd 7.

---
## Moving Output File Descriptors


Likewise, it’s also possible to move _output_ descriptors.

`exec X>&Y-`

moves the file descriptor Y to file descriptor X, or the standard output (file descriptor 1) if X is not specified.
```bash
bash-5.0$ touch foo.txt  
bash-5.0$ exec 6> foo.txt  
bash-5.0$ echo "hello, world"  >& 6  
bash-5.0$ cat foo.txt  
hello, world  
bash-5.0$ echo "bye, world"  >& 6  
bash-5.0$ cat foo.txt  
hello, world  
bye, world  
bash-5.0$ exec 8>&6-  
bash-5.0$ echo "hello again, world"  >& 6  
bash: 6: Bad file descriptor  
bash-5.0$ echo "hello again, world"  >& 8  
bash-5.0$ cat foo.txt  
hello, world  
bye, world  
hello again, world
```


In the above snippet, we first assign fd 6 to file foo.txt for output. We can then redirect output to fd 6 instead of specifying the file by name. Next, we move fd 6 to fd 8, after which we can no longer use fd 6 to refer to the file for output redirection. However, we can use fd 8 for output redirection.

# Using a Single File Descriptor to Read And Write

So far, we’ve specifically assigned file descriptors for input _or_ output, but not both. It turns out bash has a special operator that allows one to perform both forms of redirection using a single operator:

exec X**<>**file

causes the file named `file` to be opened for _both_ reading and writing on file descriptor X, or on file descriptor 0 if X is not specified. If the `file` does not exist, it is created.

bash-5.0$ touch foo.txt  
bash-5.0$ exec 7<>foo.txt  
bash-5.0$ echo "Hello, World!" >&7  
bash-5.0$ echo "Hello, New World!" >&7  
bash-5.0$ cat foo.txt  
Hello, World!  
Hello, New World!  
bash-5.0$  
bash-5.0$  
bash-5.0$ read input <&7 && echo "${input}"  
bash-5.0$

In the above snippet, we open a file `foo.txt` for both input and output on file descriptor 7. We then redirect two output streams to fd 7. When we then cat the file, we see that both of output streams have been written to the file.

When we next try to read from the file, the read doesn’t return anything. This is because the file descriptor 7 is pointing to the _end_ of the file after the last character of the last output stream. Bash (thankfully!) doesn’t implement a built in or an operator that emulates `lseek(2)`, so there’s no way to _seek_ to the beginning of the file before issuing the read.

However, if we reissue the redirection command, we see that we can access input from the file descriptor.
```
bash-5.0$ touch foo.txt  
bash-5.0$ exec 7<>foo.txt  
bash-5.0$ echo "Hello, World!" >&7  
bash-5.0$ echo "Hello, New World!" >&7  
bash-5.0$ cat foo.txt  
Hello, World!  
Hello, New World!  
bash-5.0$  
bash-5.0$  
bash-5.0$ read input <&7 && echo "${input}"  
bash-5.0$  
bash-5.0$ exec 7<>foo.txt  
bash-5.0$ read input <&7 && echo "${input}"  
Hello, World!
```

Furthermore, we can redirect output of a command to the file we’ve just read a line from using the same file descriptor. It’ll overwrite the existing contents of the file, since we’re effectively just writing bytes to a file descriptor.
```
bash-5.0$ touch foo.txt  
bash-5.0$ exec 7<>foo.txt  
bash-5.0$ echo "Hello, World!" >&7  
bash-5.0$ echo "Hello, New World!" >&7  
bash-5.0$ cat foo.txt  
Hello, World!  
Hello, New World!  
bash-5.0$ read input <&7 && echo "${input}"  
bash-5.0$  
bash-5.0$ exec 7<>foo.txt  
bash-5.0$ read input <&7 && echo "${input}"  
Hello, World!  
bash-5.0$ cat foo.txt  
Hello, World!  
Hello, New World!  
bash-5.0$ echo "Bye, World!" >&7  
bash-5.0$ cat foo.txt  
Hello, World!  
Bye, World!  
orld!  
bash-5.0$
```


In the above snippet, we’re written two lines to a file, then done a “reset” on the file descriptor, then read a line from the file, and then used the same file descriptor to write another output stream to the file, which overwrites the content of the file.