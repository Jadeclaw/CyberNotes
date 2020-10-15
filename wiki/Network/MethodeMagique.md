# Determiner une plage d'adresse en fonction du masque avec la methode du chiffre magique

La méthode du chiffre magique permet de calculer des plages d'adresses rapidement, sans passer par le binaire.
Elle permet donc de faciliter le calcul de sous-réseaux.


## La méthode :

### 1 - Trouver l'octet significatif
L'octet significatif est octet dans lequel se situe le premier bit à 0, autrement dit, __le premier octet du masque qui n'est pas a 255__.

 _Exemple :_ 
 
 Un réseau avec une IP __10.20.15.0__ et un masque __255.255.224.0__
 
Ici, l'octet significatif du masque est __224__, et celui de l'ip est __15__.

### 2 - Calculer le nombre magique
Pour calculer le nombre magique, il suffit de faire :
> 256 - \<octet significatif du masque>

_Dans notre exemple :_

> 256 - 224 = 32

Ici le nombre magique est __128__.

__Note :__ Le nombre magique correspond au nombre d'IP total du réseau (en comptant l'adresse réseau et broadcast) 


### 3 - Lister les multiples du nombre magique

Il suffit de lister les multiples de notre nombre magique en commençant à 0 et jusqu'a 256.

_Dans notre exemple :_
> 0 32 64 96 128 160 192 224 256

Ensuite, il suffit de suivre les deux prochaines règles pour trouver les bornes de notre réseau.

## Les règles :

Ces règles nous permettent de trouver l'otet sigificatif de nitre adresse réseau et notre adresse broadcast :

- L'octet de l'adresse réseau est le plus proche multiple plus petit ou égal a l'octet significatif de l'IP.
- L'octet de l'adresse de broadcast est le plus proche mutiple supérieur __-1__.

Il suffit ensuite de mettre les octets suivants de l'adresse réseau à 0 et les octets suivants dans l'adresse broadcast a 255.

---

_Dans notre exemple :_

Octet significatif de l'IP : __15__

Multiples : __0__ __32__ 64 96 128 160 192 224 256

Le plus proche multiple plus petit ou égal est __0__.

Le plus proche multiple plus grand est __32__ ( 32 - 1 = __31__ ).

Notre plage d'adresse va donc de :
```10.20.0.0``` à ```10.20.31.255```