 
# Config Cisco

## Configurer une interface

Help : 
> ?

a tout moment vous pouvez ajouter `?` a votre commande et le terminal affichera les arguments que vous pouvez ajouter.

Notez également qu'il  n'est pas nécéssaire de taper les commandes dans leur intégralité. Seules les quelques premières lettres sont suffisantes s'il n'y a aucune ambiguité.

### 1) Mode root
> \> enable

### 2) mode configuration 
> \# configure terminal

### 3) Choisir une interface
> (config)\# interface \<nom de l'interface (ex: g0/0)>

### 4) Configurer l'adresse de l'interface
> (config-if)\# ip address \<adresse IP> \<Masque de sous-reseau>

### 5) Allumer l'interface
>(config-if)\# no shutdown

Après, vous pouvez sortir du mode configuration, ou revenir a l'étape 3 pour configurer une autre interface.

### 6) Sortir du mode config 
>\(config)# exit

## Attention
La configuration est stockée dans la RAM ! on édite que la "running-config" il faut la passer en "startup-config" : 
(Il faut sortir du mode config pour cette commande !)
> \# copy running-config startup-config

Ou **UNIQUEMENT SUR CISCO PACKET TRACER :**
>\# write


## Afficher une configuration 

>  \# show \<nom de la configuration>

_Exemples :_ 

(afficher la configuration actuelle)
>  \# show running-config

(afficher la config d'une interface)
>  \# show ip interface g0/0

(afficher les routes configurées)
>  \# show ip route

## Ajouter une route a un routeur

Un routeur ne connais que es réseaux diretement connectés a celui ci, pour aller sur un autre réseau, on doit ajouter une route.

### Ajouter une route statique 

> \(config)# ip route \<réseau cible> \<masque> \<next hop>

next hop = prochain routeur vers le réseau cible

Pour ajouter une route par défaut, il faut utiliser la commande :
> (config)# 


