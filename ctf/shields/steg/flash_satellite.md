# Flash Satellite

## Instructions

> Un de nos satellite allié a intercepté des flashs lumineux provenant d'une de nos base en camps ennemi, R2D2 a remis le message en forme et vous le transfère en gif. Nos alliées ont besoin de vous, décodez la transmission !

En lien, nous avons un gif :
![](flash/flash.gif)

On se rend compte que chaque image du gif cache une information, on va donc [split le gif en images](https://ezgif.com/split) :

Ensuite, on a donc un message codé sur 3 lignes et 2 colonnes, avec 2 états (noir ou blanc ici).
On peut donc deviner que le message est en braille.
Ensuite, on peut convertir le message à la main, on faire un script (j'ai fait à la main), ce qui nous donne :
`PALPATINE EST DANS LA SAUCE SHIELDS SEND THE NEGOTIATOR`

donc le flag : **SHIELDS{SEND THE NEGOTIATOR}**