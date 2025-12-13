# Chapitre 03 — Routage statique 

## 1 Qu’est-ce que le routage ?
Le routage permet à un paquet de trouver son chemin d’un réseau à un autre
Chaque routeur prend des décisions pour envoyer les paquets vers la bonne destination
Deux types principaux de routage :
    -> Routage statique → défini manuellement par l’administrateur
    -> Routage dynamique → appris automatiquement par les routeurs via des protocoles (ex : OSPF, RIP, BGP)

## 2 Qu’est-ce que le routage statique ?
### dabord  Qu’est-ce que la table de routage ?
La table de routage est comme une carte du réseau que possède chaque routeur.
Elle indique où envoyer chaque paquet selon son adresse de destination.
Chaque ligne de la table correspond à un réseau et la “route” à prendre pour y accéder.
### routage statique :
Dans le routage statique, les routes sont entrées manuellement dans la table de routage du routeur
Le routeur ne change pas la route automatiquement
Avantages :
     simple, sûr, prévisible
Inconvénients : 
    peu flexible, doit être mis à jour manuellement si le réseau change

Exemple simple :
Imaginons 2 réseaux connectés via un routeur :

    Réseau A : 192.168.1.0/24
    Réseau B : 192.168.2.0/24
    Routeur R1 : connecte les deux réseaux

Pour que PC dans le Réseau A atteigne PC dans le Réseau B :
    On ajoute une route statique sur R1

## 3 Table de routage et lecture
Exemple de table de routage sur R1 :
Destination	    Masque	        Passerelle	    Interface	Type
192.168.1.0 	255.255.255.0	-	            Eth0	    Directe
192.168.2.0 	255.255.255.0	192.168.1.2 	Eth0	    Statique
### Destination
Indique le réseau ou l’hôte cible que tu veux atteindre.
Exemples dans la table :
    192.168.1.0 → réseau local directement connecté
    192.168.2.0 → réseau distant, nécessite une route statique
#### mask
Définit quelle partie de l’adresse est le réseau et quelle partie est pour les hôtes.
255.255.255.0 → 24 bits pour le réseau (/24), 8 bits pour les hôtes.
Permet au routeur de savoir si une IP de destination appartient à ce réseau ou non.
### Passerelle (Next Hop)
Adresse du prochain routeur pour atteindre le réseau de destination.
Pour les réseaux directement connectés, la passerelle est - car le routeur peut envoyer directement le paquet.
Pour les réseaux distants, il faut préciser le routeur suivant qui connait la destination.

Exemple : 192.168.1.2 → c’est l’IP du routeur R2 sur le réseau A.
le routeur envoie le paquet à cette passerelle, et ensuite elle continue jusqu’au réseau final.
### Interface 
later
### Type
Directe → le réseau est connecté directement au routeur
Statique → la route a été configurée manuellement pour atteindre le réseau B

## 4 Configuration d’une route statique (Cisco style)
### Ajouter une route statique :
    R1(config)# ip route 192.168.2.0 255.255.255.0 192.168.1.2
    192.168.2.0 → réseau de destination
    255.255.255.0 → masque de sous-réseau
    192.168.1.2 → passerelle (next hop) pour atteindre ce réseau (ip du routeur)
### Vérifier la table de routage
    R1# show ip route
### Supprimer une route statique
    R1(config)# no ip route 192.168.2.0 255.255.255.0 192.168.1.2

## 5 Routage par défaut 
Une route par défaut est une route spéciale qui est utilisée quand le routeur ne connaît pas de route spécifique pour une destination.
C’est le fameux “catch-all” : toutes les adresses qui ne sont pas dans la table de routage utilisent cette route.

Exemple : pour aller sur Internet depuis un LAN, ton routeur utilise la route par défaut pour envoyer tous les paquets vers ton fournisseur d’accès (FAI).

### Route par défaut sur R1 :
    R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
    0.0.0.0/0 → toutes les destinations inconnues
    10.0.0.1 → passerelle vers Internet
###  Comment ça fonctionne ?
    PC dans le LAN veut accéder à 8.8.8.8 (Google)
    R1 regarde sa table → pas de route spécifique pour 8.8.8.8
    R1 utilise la route par défaut et envoie le paquet vers 10.0.0.1 (FAI)
    Le paquet traverse Internet et atteint Googl
### commande en genaral : 
    R1(config)# ip route 0.0.0.0 0.0.0.0 [passerelle]
    R1# show ip route
    La table doit montrer une ligne comme :
    S* 0.0.0.0/0 [1/0] via [passrelle]
    S* → S = statique, * = route par défaut
Supprimer la route par défaut
    R1(config)# no ip route 0.0.0.0 0.0.0.0 [passrelle]

## 6 Routage vers une destination inconnue
### Que se passe-t-il si le routeur ne connaît pas la destination ??
    Deux cas possibles :
    Si une route par défaut existe → envoie le paquet via la route par défaut
    Sinon → le paquet est jeté / drop, et souvent un message ICMP “destination unreachable” est renvoyé

### tp will do it later....