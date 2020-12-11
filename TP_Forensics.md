# TP 10/12/2020
# Analyse forensique de Kali Live ISO

## 1. Imager le disque

Commencer par démarrer votre Kali linux, puis insérez notre disque Kali live
Quand Kali Live a été inséré, lancez Guymager pour créer l'image du disque
 - Selectionner le disque (/dev/sr0)
 - Selectionner "Aquire Image" dans le menu contextuel
 - Choisissez une image dd linux pour éviter de la séparer en plusieurs images
 - Selectionner MD5 et SHA-256 afin d'avoir les sommes de contrôle 

### 1.1 Verification des sommes de controle : 

Fichier 	: case04.dd
MD5 		: 928c38903d0c2213f8e076165d69c6ff
SHA-256		: 4d764a2ba67f41495c17247184d24b7f9ac9a7c57415bbbed663402aec78952b

Nous avons les sommes de contrôles avec: `hashdeep -c MD5,SHA-256 /mnt/case04/case04.dd`

```
└─# hashdeep -c MD5,SHA-256 case04.dd 
%%%% HASHDEEP-1.0
%%%% size,md5,sha256,filename
## Invoked from: /mnt/TP
## # hashdeep -c MD5,SHA-256 case04.dd
## 
3517394944,928c38903d0c2213f8e076165d69c6ff,4d764a2ba67f41495c17247184d24b7f9ac9a7c57415bbbed663402aec78952b,/mnt/TP/case04.dd
```

## 2. Forensique : Récupération d'images

Nous allons extraire des images png de l'image du disque a l'aide de l'outil "Scalpel".
Pour cela, nous l'avons d'abord configurer en modifiant le fichier : `/etc/scalpel/scalpel.conf`
puis décommenté la ligner png (et seulement la ligne png)

ensuite, lancez simplement la commande `scalpel case04.dd`

Aucun fichier n'a été trouvé, mais plusieurs headers ont été détéctés (on peut voir ca avec l'argument verbose (-v))

On a donc essayé magicrescue afin de comprendre le problème avec scalpel
magicrescue trouve plein de png (353 fichiers)

## 3. Réparer scalpel

En regardant la config de scalpel, on y trouve le header et le footer que scalpel cherche dans l'image. En cherchant, la spécification des fichiers PNG, nous trouvons le header valide et nous pouvons voir qu'il manque un octet a l'entête. 

En regardant l'hexadecimal de plusieurs fichiers trouvés par magicrescue, on voit plusieurs fichiers ayant un footer différents. 
Nous avons donc enlever le footer afin de matcher tous les png ayant la bonne entête
Il faut donc remplacer la ligne :
```
png	y	20000000	\x50\x4e\47?	\xff\xfc\xfe
```
par : 
```
png     y       20000000        \x89\x50\x4e\x47?
```

## 4. Trouver des emails


### 4.1 Avec autopsy 

Nous devons trouver des emails avec Autopsy, pour cela, ouvrez autopsy puis ouvrez une "nouvelle affaire" (new case). Remplissez les informations correspondantes et ouvrez le disque "raw" dans "analysis". Cliquez ensuite sur Keyword search. Cochez toutes les cases et tapez la regex suivante :
```
[0-9a-z]+@[0-9a-z]+\.[a-z]{2,}
```

Nous trouvons 685 emails.

### 4.2 Avec bulk extractor

Nous devons comparer les résultats de Autopsy et de bulk extractor 
Nous devons donc lancer bulk_extractor:
```
bulk_extractor -o bulk case04.dd
``` 
nous précisons rien, cela va tout récupérer dans l'image
regardez ensuite dans bulk/email_histogram.txt, il y a 19811 adresses différentes.




