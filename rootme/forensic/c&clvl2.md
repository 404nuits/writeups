# Command and Control - Level 2

> Berthier, grâce à vous la machine a été identifiée, vous avez demandé un dump de la mémoire vive de la machine et vous aimeriez bien jeter un coup d’œil aux logs de l’antivirus.
> 
> Malheureusement, vous n’avez pas pensé à noter le nom de cette machine. Heureusement ce n’est pas un problème, vous disposez du dump de memoire.

Since the documentation is about Volatility, we know that we are going to use it.

Thus, the first thing to do is to make `imageinfo` in order to determine image's type :
```bash
$ > volatility -f ch2.dmp imageinfo
```
Output :
```bash
Volatility Foundation Volatility Framework 2.6
INFO    : volatility.debug    : Determining profile based on KDBG search...
          Suggested Profile(s) : Win7SP1x86_23418, Win7SP0x86, Win7SP1x86_24000, Win7SP1x86
                     AS Layer1 : IA32PagedMemoryPae (Kernel AS)
                     AS Layer2 : FileAddressSpace (/mnt/c/Users/wewen/Documents/hacking/rootme/forensic/berthier/ch2.dmp)
                      PAE type : PAE
                           DTB : 0x185000L
                          KDBG : 0x82929be8L
          Number of Processors : 1
     Image Type (Service Pack) : 0
                KPCR for CPU 0 : 0x8292ac00L
             KUSER_SHARED_DATA : 0xffdf0000L
           Image date and time : 2013-01-12 16:59:18 UTC+0000
     Image local date and time : 2013-01-12 17:59:18 +0100
```

Then, we take one of the suggested profiles and we display environment variables since on Windows, there is one that contains computer's name : `COMPUTERNAME`
```bash
$ > volatility -f ch2.dmp envars | egrep COMPUTERNAME
```
And that's it, we got the flag !