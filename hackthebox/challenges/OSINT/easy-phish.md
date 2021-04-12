# Easy Phish

> Customers of secure-startup.com have been receiving some very convincing phishing emails, can you figure out why?

When we try to access the URL in a browser, it shows nothing, same on the Wayback Machine.

We can then try to see informations about DNS record of `secure-startup.com` with for example `whois`

```bash
$ > whois secure-startup.com
```

Since it's kind of a dead end, we can try to test some lookup tools on email (since users have received phishing emails), [here's](https://mxtoolbox.com/NetworkTools.aspx?showLogin=1&tab=Email) an online toolbox

(Note : The Name Servers were not Google so I didn't tested SPF DKIM and DMARC with Gmail but [here](https://linuxincluded.com/testing-spf-dkim-and-dmarc/) is a nice article on how to do it)

When we try out in every single tool, we got something weird in the spf lookup :
![](https://i.imgur.com/AomQ4kM.png)
One of the values looks alot to a flag, but there's a part missing, so let's continue using those tools.

And bingo ! We found the second part of the flag in a DMARC Lookup :
![](https://i.imgur.com/VdPaJkh.png)



We could also have made this using `dig` :

Since SPF records are mainly stored as TXT, we will specify that we want TXT records
```bash
$ > dig secure-startup.com TXT
```
Output :
```
; <<>> DiG 9.11.5-P4-5.1+deb10u3-Debian <<>> secure-startup.com txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 37643
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;secure-startup.com.            IN      TXT

;; ANSWER SECTION:
secure-startup.com.     1800    IN      TXT     "v=spf1 a mx ?all - HTB{RIP_SPF_Always_2nd"

;; Query time: 27 msec
;; SERVER: 192.168.0.254#53(192.168.0.254)
;; WHEN: Tue Apr 13 00:35:25 CEST 2021
;; MSG SIZE  rcvd: 101
```

So we got the first part of the flag, what about the second ?

Well we can still try some other protocols like dmarc :
```bash
$ > dig _dmarc.secure-startup.com txt
```
Output :
```
; <<>> DiG 9.11.5-P4-5.1+deb10u3-Debian <<>> _dmarc.secure-startup.com txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 58294
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;_dmarc.secure-startup.com.     IN      TXT

;; ANSWER SECTION:
_dmarc.secure-startup.com. 1800 IN      TXT     "v=DMARC1;p=none;_F1ddl3_2_DMARC}"

;; Query time: 34 msec
;; SERVER: 192.168.0.254#53(192.168.0.254)
;; WHEN: Tue Apr 13 00:47:24 CEST 2021
;; MSG SIZE  rcvd: 99
```

And that's it, we got the flag