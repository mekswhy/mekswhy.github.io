---
layout: post
title: Linux command-line tools
category: linux
tags: [command-line, tool]
---

## Directing Input/Output
In a UNIX system, the keyboard input (standard input), information printed to the screen (standard output) and error output (standard error) are treated as separate file streams. The difference between **standard output** and **standard error** is that standard error is unbuffered (it appears immediately on the screen).

#### 1. \>
Redirects the standard output to a file.

```
$ echo hello > a-file
$ cat a-file
hello
```

#### 2. <
Redirects a file to the standard input.

```
$ cat < a-file
hello
```

#### 3. \>\>
Similar to \> except it's in *append* mode.

#### 4. <<
Similar to using **CTRL-D** (EOF key), except it uses a string to perform the end-of-file function.

```
$ cat << OK
heredoc> a
heredoc> b
heredoc> c
heredoc> OK
a
b
c
```

#### 5. 2\>
Redirects the standard error to a file.

```
$ cat a-file 2> /dev/null
```

#### 6. |
The "pipe" command redirects the output of the previous command to the input of the next command.

```
$ cat a-file | grep hello
hello
```

#### 7. tee
Redirects standard input to both standard output and a file.

```
$ echo hello | tee a-file
hello
$ cat a-file
hello
```

#### 8. &\>
Redirects both standard output and standard error to a file.

#### 9. Command Substitution
Command substitution is basically another way to do a pipe, you can use pipe and command substitution *interchangeably*.

```
$ grep hello $cat a-file
hello
$ cat a-file | grep hello
hello
```

#### 10. &&
Executing the second command only if the first is successful.

```
$ cat non-existent-file && echo OK
cat: non-existent-file: No such file or directory
$ cat a-file && echo OK
hello
OK
```

#### 11. ||
Executing the second command only if the first fails. We can combine && and || to make use of the logical shortcut.

```
$ cat a-file && echo OK || echo NO    
hello
OK
```

#### 12. ;
Executing the second command regardless of the previous command is successful.

<!-- more -->

## Working with the file-system

