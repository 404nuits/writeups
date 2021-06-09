# Bienvenue dans la résistance

## Instructions

>Un message de 17 caractères minuscules se chache dans le fichier ci-joint. Trouver le et commencez votre aventure dans la resistance !

Nous avons à disposition, un fichier txt qui semble un peu en bazar avec des caractères aléatoire.

Comme l'énoncé mentionne une chaine de 17 caractères minuscules, nous pouvons essayer de faire une recherche regex (grep ou [en ligne](https://regexr.com/)) avec ces critères précis :
```bash
grep -o -E '[a-z]{17}' hello_there.txt

>   lioioioioioioioio
    aerererererererer
    fopopopopopopopop
    obnbnbnbnbnbnbnbn
    rqsqsqsqsqsqsqsqs
    crtrtrtrtrtrtrtrt
    egbgbgbgbgbgbgbgb
    ehjhjhjhjhjhjhjhj
    sazazazazazazazaz
    tqwqwqwqwqwqwqwqw
    ampmpmpmpmpmpmpmp
    vuiuiuiuiuiuiuiui
    ejnjnjnjnjnjnjnjn
    cfifififififififi
    txoxoxoxoxoxoxoxo
    oplplplplplplplpl
    ikjkjkjkjkjkjkjkj
```
On se rend compte que chaque chaine commence par un caractère, puis répète les 2 suivants plusieurs fois.
Comme le message final doit faire 17 caractères, on peut se dire qu'on doit extraire 1 caractère de chaque chaine (il y en a 17). De plus, si on regarde un petit peu, on peut apercevoir quelque chose d'à peu près compréhensible avec les 1ers caractères de chaque ligne :
```bash
grep -o -E '[a-z]{17}' hello_there.txt | cut -c1 | tr -d '\n'

>   laforceestavectoi
```

Voici notre flag !