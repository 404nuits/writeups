# Bienvenue dans la résistance

## Instructions

>C-3PO a reçu un appel à l'aide, malgré sa maîtrise de pas moins de six millions de langages pratiqués dans la galaxie, une partie du message reste incompréhensible. Il vient de vous transférer l'audio et vous demande de retrouver la planète sur laquelle vous devez vous rendre.

Nous avons un fichier audio de joint, si on l'écoute, on se rend compte assez facilement que c'est du morse, donc direction [ce site](https://morsecode.world/international/decoder/audio-decoder-adaptive.html), plutôt sympathique.

Le fichier est assez facilement analysé puisque les lettres sont espacés donc l'outil de détection automatique suffit.

Nous obtenons donc ce message :
`P L P X Q Q X N R B P R O Q X Q L L F K B Y B P L F K A B O B K C L O Q`

Cette fois-ci, direction [dcode](https://www.dcode.fr/cipher-identifier) !

Le cipher identifier nous informe donc qu'il y ait de forte chances que notre message soit en réalité chiffré avec caesar (ou on le devine facilement)

Direction l'outil de brute force caesar qui nous donne :
`S O S A T T A Q U E S U R T A T O O I N E B E S O I N D E R E N F O R T`

Comme c'est la planète qui nous est demandée, le flag est SHIELDS{tatooine}