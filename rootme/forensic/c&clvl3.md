# Command and Control - Level 3

> Berthier, l’antivirus n’a rien trouvé. A vous d’essayer de trouver le programme malveillant dans le dump de la mémoire vive. Le mot de passe de validation est le hash MD5 (en minuscules) du chemin d’accès absolu vers l’exécutable.

We have to find malware's absolute path.

Still using volatility, we can do `pslist` or `pstree` to show running processes during the dump, with their PID and PPID
```bash
$ >  volatility -f ch2.dmp --profile=Win7SP1x86_23418 pstree
```
Output :
```bash
Name                                                  Pid   PPid   Thds   Hnds Time
-------------------------------------------------- ------ ------ ------ ------ ----
 [...]
. 0x88c3ed40:smss.exe                                 308      4      2     29 2013-01-12 16:38:09 UTC+0000
 0x87ac6030:explorer.exe                             2548   2484     24    766 2013-01-12 16:40:27 UTC+0000
. 0x87b6b030:iexplore.exe                            2772   2548      2     74 2013-01-12 16:40:34 UTC+0000
.. 0x89898030:cmd.exe                                1616   2772      2    101 2013-01-12 16:55:49 UTC+0000
. 0x95495c18:taskmgr.exe                             1232   2548      6    116 2013-01-12 16:42:29 UTC+0000
. 0x87bf7030:cmd.exe                                 3152   2548      1     23 2013-01-12 16:44:50 UTC+0000
[...]
```

We can see that among all those processes, we have a `cmd.exe` launched by `iexplorer.exe`, which is pretty weird, let's write down iexplorer.exe's PID

Next, we can run a `cmdline`, which displays processes, their PID and the argument to execute the process with a cmd, very often his **absolute path**.

Since there are lot of processes, we can search specifically for our process using `-p <PID>` :

```bash
$ >  volatility -f ch2.dmp --profile=Win7SP1x86_23418 cmdline -p 2772
```
Output :
```bash
iexplore.exe pid:   2772
Command line : "C:\Users\John Doe\AppData\Roaming\Microsoft\Internet Explorer\Quick Launch\iexplore.exe"
```

And that's it, we have our flag !