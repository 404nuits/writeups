# Trouvez le chat

> Le chat du président a été kidnappé par des indépendantistes. Un suspect a été interpellé par la gendarmerie. Il détenait sur lui une clef USB. Berthier, une nouvelle fois, à vous de jouer ! Essayez de faire parler cette clef et de trouver dans quelle ville est retenu ce chat !

First, we try to find what we just got :
```bash
$ > file ch9
```
Output :
```bash
ch9: DOS/MBR boot sector; partition 1 : ID=0xb, start-CHS (0x0,32,33), end-CHS (0x10,81,1), startsector 2048, 260096 sectors, extended partition table (last)
```
We can see that this is a full disk

Then, we can use `testdisk` to recover files contained in it :
```bash
$ > testdisk ch9
```
It opens an IDE that allows us to see files :
![](https://i.imgur.com/hxogtS7.png)
![](https://i.imgur.com/UFS6vjv.png)
![](https://i.imgur.com/6YIdCj0.png)
![](https://i.imgur.com/AAXQFaZ.png)
In folders' choice, we see a `Files` folder, which looks pretty interesting
![](https://i.imgur.com/a9rhmZ9.png)
Inside of itm we see a `revendications.odt` file, we recover it :
![](https://i.imgur.com/Qi8Motl.png)
If we open it :
![](https://i.imgur.com/fgCqjf4.png)
Pretty menacing...

Since we want city's name and the picture seems to be took by the suspect, the idea is to determine the city using metadata from the image.

Thus, if we right click and 'save image as...', we will loose those metadata.

We want the original image, but digging a bit into the nature of a .odt file, and we learn that .odt are in fact an archive file. We juste change its extention to '.zip' and we have this :
![](https://i.imgur.com/NQgFXpg.png)

If we go into the 'Pictures' folder, we can see our image, we extract it, and using `exiftool`, we can get metadata :
```bash
$ > exiftool Files/1000000000000CC000000990038D2A62.jpg
```
Output :
```
[...]
GPS Altitude                    : 16.7 m Above Sea Level
GPS Latitude                    : 47 deg 36' 16.15" N
GPS Longitude                   : 7 deg 24' 52.48" E
GPS Position                    : 47 deg 36' 16.15" N, 7 deg 24' 52.48" E
[...]
```

We can then get the city using Google Maps, or a [GPS coordinates converter](https://www.gps-coordinates.net/gps-coordinates-converter)