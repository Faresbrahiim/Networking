# Chapitre 04 — Routage dynamique 

## 1 Qu’est-ce que le routage dynamique ?
Routage dynamique = les routeurs apprennent automatiquement les chemins vers les réseaux voisins
Pas besoin de configurer manuellement toutes les routes
Les routeurs échangent des informations de routage via des protocoles

### Différence avec le routage statique 

Caractéristique	              Statique	            Dynamique
Configuration	              Manuelle	            Automatique via protocole
Adaptation aux changements	  Non	                Oui, s’adapte automatiquement
Complexité du réseau	      Petit à moyen	        Moyen à très grand
Utilisation	                  Réseaux simples	    Réseaux complexes / Internet

## 2 Pourquoi on utilise le routage dynamique ? 
### Pourquoi le routage statique ne suffit plus ?
Imagine un réseau comme ça :
    10 routeurs
    30 réseaux
2 ou 3 liens qui peuvent tomber

En statique :
    Tu dois écrire des dizaines de routes
    Si un lien tombe  → la route reste fausse
Tu dois intervenir manuellement
 
    R1 ----- R2 ----- R3
    \               /
      ---- R4 -----
Si le lien R2–R3 tombe :

    Statique  : trafic bloqué
    Dynamique ✅ : R1 → R4 → R3
## 3 Comment fonctionne le routage dynamique (principe)
    -> Les routeurs se parlent entre eux
    -> Ils échangent des informations de routage
    -> Ils remplissent automatiquement leur table de routage
### Que s’échangent les routeurs ?
Selon le protocole, ils peuvent échanger :
    les réseaux connus
    la distance (nombre de sauts / coût)
    le meilleur chemin
Ces échanges se font périodiquement ou lors d’un changement
## 4 Comment un routeur choisit le meilleur chemin ?
Protocole	        Métrique
RIP                	Nombre de sauts (hop count)
OSPF	            Coût (basé sur la bande passante)
EIGRP	            Bande passante, délai, etc.
BGP	                Politiques (plus avancé)
### Si un lien tombe ?
    Le routeur détecte la panne
    Il informe les autres routeurs
    Les tables de routage sont recalculées
    Un nouveau chemin est utilisé automatiquement
## 5 Types de routage dynamique (will see them in details later)
Il existe plusieurs protocoles de routage dynamique.
Ils ne fonctionnent pas tous de la même manière.
### RIP (Routing Information Protocol)
Version 1 : classful (ne supporte pas CIDR)
Version 2 : classless (supporte CIDR, multicast)
#### C’est quoi RIP exactement ?
RIP est un protocole de routage dynamique qui permet aux routeurs de :
    s’échanger les réseaux qu’ils connaissent
    choisir un chemin automatiquement
    mettre à jour la table de routage
### Comment RIP décide du meilleur chemin ??
RIP utilise UNE seule métrique :
    -> le nombre de sauts (hop count)
1 routeur traversé = 1 saut
Le chemin avec le moins de sauts est choisi
#### Règle très importante  
    Maximum 15 sauts
    16 sauts = réseau inaccessible
C’est pour ça que RIP est limité aux petits réseaux
## Comment RIP échange les infos ?
Toutes les 30 secondes
Chaque routeur envoie toute sa table de routage
Aux routeurs voisins
## Configuration RIP (Cisco) — vue globale 
Avant RIP : chose TRÈS importante
RIP ne fonctionne PAS si les interfaces ne sont pas configurées

Donc l’ordre logique est toujours :

    1 Configuration des interfaces
    2️ Activation de RIP
    3️ Annonce des réseaux
    4️ Vérification
Considérons la topolgie suivante 
LAN1 ---- R1 ---- R2 ---- LAN2
R1 et R2 sont connectés
Chaque routeur connaît son LAN
RIP va leur permettre d’apprendre l’autre LAN
#### Configuration des interfaces (rappel)
Sur chaque routeur, on fait :

    Adresse IP
    Masque
    Interface activée (no shutdown)
#### Activation de RIP (idée seulement)
Sur un routeur Cisco :

On entre en mode configuration RIP
On précise la version (RIPv2)
On annonce les réseaux directement connectés
#### ou bien ... par commandes :
    R1(config)# router rip
    R1(config-router)# version 2
    R1(config-router)# network 192.168.1.0
    R1(config-router)# network 10.0.0.0


### OSPF (Open Shortest Path First)
Protocole de routage dynamique utilisé dans les entreprises
Basé sur l’état des liens (link-state)
Chaque routeur connaît la carte complète du réseau
Calcule le chemin le plus court avec l’algorithme de Dijkstra
#### Fonctionnement général
    Chaque routeur découvre ses voisins
    Échange des Link-State Advertisements (LSA)
    Construit une base de données topologique
    Calcule le chemin le plus court vers chaque réseau
    Remplit automatiquement sa table de routage
#### Configuration OSPF sur Cisco 
1 Configurer les interfaces (obligatoire)
2 Activer OSPF et définir l’ID de processus
    R1(config)# router ospf 1 // 1 =ID 
3 Annoncer les réseaux et l’area
    R1(config-router)# network 192.168.1.0 0.0.0.255 area 0
    R1(config-router)# network 10.0.0.0 0.0.0.3 area 0
        network = réseaux que tu veux annoncer
        0.0.0.255 = wildcard mask (inverse du masque)
        Exemple : masque 255.255.255.0 → wildcard = 0.0.0.255
        area 0 = backbone obligatoire
OSPF n’annonce que les réseaux connectés aux interfaces configurées
### Première partie : network 192.168.1.0
    C’est le réseau que tu annonces via OSPF (C’est ton réseau que tu veux annoncer à tes voisins via OSPF.)
    On ne met pas une IP d’un PC, mais l’adresse du réseau
    le mot network c'est just un mot clé
### Wildcard mask : 0.0.0.255
    Qu’est-ce que c’est ?? 
    C’est un masque spécial qui indique quelles adresses font partie du réseau que tu annonces.
    C’est l’inverse du masque de sous-réseau
    Sert à indiquer quelles adresses font partie du réseau que tu annonces
Comment le calculer ? 
Écris le masque du réseau : 255.255.255.0
Soustrais chaque octet de 255 : exemple :
    255 - 255 = 0
    255 - 255 = 0
    255 - 255 = 0
    255 - 0   = 255
OSPF sait exactement quelles adresses annoncer
Permet d’éviter d’annoncer des adresses inutiles ou hors 
### Area 0 : zone OSPF
OSPF divise le réseau en zones (areas) pour mieux organiser et gérer le routage
Area 0 = backbone obligatoire
Tous les autres réseaux doivent se connecter à Area 0, directement ou via une autre area
OSPF utilise area 0 pour l’échange principal des informations

Si tu annonces un réseau dans une autre area sans backbone → OSPF ne fonctionnera pas correctement
### EIGRP (Enhanced Interior Gateway Routing Protocol)
####  Type
Propriétaire Cisco, hybride entre distance-vector et link-state
#### Propriétaire
Protocole propriétaire Cisco → fonctionne seulement sur équipements Cisco
#### Fonction
Permet aux routeurs de découvrir automatiquement les chemins dans un réseau interne
    Métrique basée sur :
    Bande passante
    Délai
    Charge
    Fiabilité
#### Avantages
Convergence rapide (très réactif si un lien tombe)
Flexible pour grands réseaux internes
Moins de trafic inutile que RIP
####  Utilisation
Principalement dans les réseaux d’entreprise utilisant du matériel Cisco
#### EIGRP configuration basique sur Cisco
##### topologie 
LAN1 ---- R1 ---- R2 ---- LAN2
1 : Configurer les interfaces (IP + no shutdown)
2 : Activer EIGRP et annoncer les réseaux
    R1(config)# router eigrp 1
    R1(config-router)# network 192.168.1.0
    R1(config-router)# network 10.0.0.0

1 = numéro du processus EIGRP
network = réseaux directement connectés à ce routeur
EIGRP va annoncer ces réseaux automatiquement aux voisins
### BGP (Border Gateway Protocol)
Protocol externe, utilisé sur Internet
Protocole de routage externe (EGP)
Utilisé pour échanger des routes entre différents réseaux / fournisseurs (AS – Autonomous Systems)
Basé sur le concept de path vector
#### Topologie simple
-> Un AS = un réseau ou groupe de réseaux sous une même administration
->  Neighbor / Peer

        Les routeurs BGP établissent des sessions avec des voisins
        Les voisins s’échangent les routes qu’ils connaissent

R1 (AS 65001) ---- R2 (AS 65002)
    Chaque routeur fait partie d’un AS différent
->  Objectif : échanger les réseaux directement connectés entre eux
R1(config)# router bgp 65001
R2(config)# router bgp 65002
    Chaque routeur doit connaître son AS local
-> Définir le voisin BGP ....
R1(config-router)# neighbor 192.168.2.1 remote-as 65002
R2(config-router)# neighbor 192.168.1.1 remote-as 65001
Ici on dit : « Ce voisin fait partie de tel AS »
-> Annoncer les réseaux
R1(config-router)# network 192.168.1.0 mask 255.255.255.0
R2(config-router)# network 192.168.2.0 mask 255.255.255.0


## Il y a beaucoup de protocoles de routage (RIP, OSPF, EIGRP, BGP, etc.), et on ne choisit pas au hasard. Voici une explication claire :
Selon la taille du réseau :

Taille réseau	                                Protocoles adaptés
Petit réseau interne (quelques routeurs)	    RIP (simple mais limité)
Réseau moyen à grand	                        OSPF, EIGRP (rapides, efficaces, flexibles)
Très grands réseaux ou Internet	                BGP (inter-AS)

Selon la rapidité et la complexité

Protocole	    Avantages	                                 Limites
RIP	            Très simple, facile à configurer	        Limité à 15 sauts, lent, pas scalable
OSPF	        Rapide, stable, évolutif	                 Un peu complexe, besoin de zones
EIGRP	        Rapide, flexible, convergence rapide	    Propriétaire Cisco
BGP	            Très stable pour Internet, contrôle total	Complexe, lente convergence

Selon la politique et le contrôle
###  Résumé simple
    Petit réseau interne → RIP
    Grand réseau interne → OSPF ou EIGRP
    Réseaux interconnectés / Internet → BGP
    Critère principal : taille du réseau + contrôle + rapidité + compatibilité
