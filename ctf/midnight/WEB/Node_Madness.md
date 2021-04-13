Petit write-up pour le challenge Node Madness :

On commence en accédant au chall avec ce lien : http://nodemadness.ctf.midnightflag.fr/.

![Capture connexion](https://i.imgur.com/dKP6ylz.png)

On arrive sur une page nous proposant un lien pour nous connecter. On choisit un username et un password au hasard (pas d'importance, j'ai choisi admin admin)

On arrive après s'être connecté sur une page nous montrant nos informations:
![Capture page du profil](https://i.imgur.com/rb6sVeQ.png)

En ayant lu qu'il n'y avait pas de liens avec une base de données externe, on peut se demander si des cookies ne rentrent pas en jeu.
En cherchant un peu, on trouve aussi une page admin : 
![Capture page admin](https://i.imgur.com/0WNpVrN.png)

En regardant l'onglet Réseau de l'inspecteur d'élément, on peut voir dans le header de la requête profile (lors du chargement de la page profile) qu'il y a bel et bien un cookie.
![Capture panel réseau](https://i.imgur.com/obR8hFF.png)


Cookie: currentUser=eyJ1c2VybmFtZSI6ImFkbWluIiwicGFzc3dvcmQiOiJhZG1pbiIsImlzQWRtaW4iOjB9


On trouve vite que le cookie est encodé en base64.
On utilise un decoder en ligne (https://www.base64decode.org/   par exemple) et on obtient en clair :
![Capture décodage cookie](https://i.imgur.com/deRrntl.png)


On a donc juste à modifier le cookie en passant la valeur de isAdmin à 1. On encode en base 64 ce qui nous donne : 
![Capture encodage nouveau cookie](https://i.imgur.com/GoOxeIe.png)
eyJ1c2VybmFtZSI6ImFkbWluIiwicGFzc3dvcmQiOiJhZG1pbiIsImlzQWRtaW4iOjF9

On va dans la console de l'inspecteur d'élément et on tape :
![Capture changement valeur cookie](https://i.imgur.com/Qo4I9tq.png)

Si on refresh la page /profile, on voit bien que la valeur de isAdmin est à 1.

On a plus qu'a aller dans l'onglet /admin

Et on obtient : 
![Capture page admin avec le flag](https://i.imgur.com/FYWLkR1.png)