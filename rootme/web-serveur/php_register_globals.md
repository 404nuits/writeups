# PHP - Register Globals

## Principe
En lisant la doc, on apprend que si la propriété **register globals** est activée, on peut utiliser l'ambiguité de la déclaration des variables en PHP à notre avantage : c'est-à-dire que l'on peut réécrire nous-même le contenu de n'importe quelle variable.

## 1ère Partie : Récupérer le fichier PHP
Tout d'abord, l'énoncé du challenge nous aide beaucoup pour trouver le code source : 

>Il semblerait que le développeur oublie souvent des fichiers de sauvegarde...

On sait donc désormais qu'il faut récupérer une version de backup d'un fichier php existant. Notre premier choix va donc naturellement se tourner vers index.php

Voici une liste des écritures à tester lorsque l'on veut récupérer une version alternative d'un fichier :
- index.php~
- index.php.old
- index.php.bak

En essayant ce dernier cas, on peut télécharger le fichier, ce qui nous donne accès au code source que voici :

```php
<?php


function auth($password, $hidden_password){
    $res=0;
    if (isset($password) && $password!=""){
        if ( $password == $hidden_password ){
            $res=1;
        }
    }
    $_SESSION["logged"]=$res;
    return $res;
}



function display($res){
    $aff= '
	  <html>
	  <head>
	  </head>
	  <body>
	    <h1>Authentication v 0.05</h1>
	    <form action="" method="POST">
	      Password&nbsp;<br/>
	      <input type="password" name="password" /><br/><br/>
	      <br/><br/>
	      <input type="submit" value="connect" /><br/><br/>
	    </form>
	    <h3>'.htmlentities($res).'</h3>
	  </body>
	  </html>';
    return $aff;
}



session_start();
if ( ! isset($_SESSION["logged"]) )
    $_SESSION["logged"]=0;

$aff="";
include("config.inc.php");

if (isset($_POST["password"]))
    $password = $_POST["password"];

if (!ini_get('register_globals')) {
    $superglobals = array($_SERVER, $_ENV,$_FILES, $_COOKIE, $_POST, $_GET);
    if (isset($_SESSION)) {
        array_unshift($superglobals, $_SESSION);
    }
    foreach ($superglobals as $superglobal) {
        extract($superglobal, 0 );
    }
}

if (( isset ($password) && $password!="" && auth($password,$hidden_password)==1) || (is_array($_SESSION) && $_SESSION["logged"]==1 ) ){
    $aff=display("well done, you can validate with the password : $hidden_password");
} else {
    $aff=display("try again");
}

echo $aff;

?>
```

## Exploitation de la faille

En analysant le code source, on peut remarquer que pour être connecté, il faut que `$password` et `$hidden_password` soient identiques. Si c'est le cas, `$_SESSION['logged'] est à 1`

On peut donc essayer de passer `password=caca&hidden_password=caca` dans l'url

Nous voilà connecté mais le password affiché est celui que nous avons renseigné dans l'URL et non le vrai.

Il suffit juste de recharger la page et comme logged est à 1, nous sommes toujours connecté.

### Solution alternative

Comme le if pour afficher le mot de passe est un OU (soit les mdp sont identiques, soit logged est à 1), on peut essayer aussi de changer manuellement la valeur de `$_SESSION['logged']` à 1, en passant dans l'URL `?_SESSION[logged]=1`, ce qui marche aussi

