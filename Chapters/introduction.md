# Chapitre 01 — Introduction aux réseaux

## 1 Qu’est-ce qu’un réseau ?

Un réseau informatique est un ensemble d'equipements (ordinateurs , serveurs , routes , switches .... ) interconnectés afin de communiquer et échanger des données.

### La communication peut se faire : 
    -> par câblage (Ethernet, fibre optique)
    -> ou san fil (wifi , bluetooth)
### Un réseau repose aussi sur :
    -> une configuration (adresses IP, passerelles, DNS)
    -> des paramétrages (sécurité, routage, partage des ressources) , (wil dive later...)
## 2 Objectifs d’un réseau ? 

Les premiers réseaux sont apparus aux États‑Unis dans les années 1960, avec pour objectif principal le partage de données.
Aujourd’hui, les réseaux permettent notamment : 
    -> le partage de données et de fichiers,
    -> le partage des imprimantes et autres ressources,
    -> la recherche d’informations sur Internet,
    -> la messagerie électronique (emails),
    -> la messagerie instantanée,
    -> la maintenance à distance,
    -> l’enseignement et la formation en ligne.

Vous pourriez être confus quant à la façon dont les ordinateurs discutent ?
    -> c'est Appartir des outils notament :

## 3 Le concentrateur (Hub) (old way....)

Un hub est un équipement réseau simple qui permet de relier plusieurs ordinateurs dans un réseau local LAN (LOCAN AREA Network)

### Fonctionnement :
    -> Il reçoit les données sur un port.
    -> Il les diffuse à tous les autres ports (division du signal) (not good)
### Inconvénients :
    -> Provoque une saturation du réseau (trop de données circulent inutilement).
    -> Le réseau devient lent.
    -> Si le hub tombe en panne, tout le réseau est arrêté.

Aujourd’hui, le hub est presque abandonné au profit du switch.

## 4 Le commutateur (Switch) 

Le switch est un équipement qui corrige les problèmes du hub. Il envoie les données uniquement vers le bon destinataire.

Principe de fonctionnement : 
    -> Chaque appareil est connecté à un port du switch.
    -> Chaque appareil possède une adresse MAC unique.

### Exemple de communication
    -> MAC source : PC1
    -> MAC destination : PC4

    Étapes: 
    Le switch reçoit le paquet (infos , MAC des....) sur le port 1 
    Il lit l’adresse MAC de destination.
    ->  Deux cas possibles : 
        -> L’adresse MAC est connue → le paquet est envoyé directement au port correspondant.
        -> L’adresse MAC est inconnue → le switch fait un broadcast (envoi à tous les ports).

Le switch apprend progressivement les adresses MAC (table MAC).

Maintenent c'est qoui équipement qui connecte plusieurs réseau ?
## 5 Le routeur (Router)

Le routeur est un équipement réseau plus avancé, généralement plus coûteux, qui permet de connecter plusieurs réseaux entre eux.
    -> Le switch connecte des appareils.
    -> Le routeur connecte des réseaux.
    -> Utilise les adresses IP (not like switch)
    -> Connecte LAN ↔ WAN / Internet

### Exemple : accéder à Google
    PC1 (dans le LAN) crée un paquet :
        -> IP source = PC1
        -> IP destination = serveur Google
    -> Le switch transmet le paquet au routeur.
    -> Le routeur détecte que la destination est hors du LAN.
    -> Il envoie le paquet vers Internet (avec NAT si nécessaire)
    -> Le serveur Google répond
    -> La réponse revient : Internet → routeur → switch → PC1
(to simplify router connecte many networks ... will see that in details next chapters)

## 6 Les types de réseaux 

###  LAN — Local Area Network
Portée : zone géographique limitée (maison, bureau, entreprise)
Nombre d’appareils : environ 100 à 1000
Maintenance assurée par une équipe technique locale

### WAN — Wide Area Network
Relie plusieurs LAN
Portée : très large (ville, pays, monde)
Exemple : Internet

### PAN — Personal Area Network
Portée : très courte (quelques mètres).
Relie des appareils personnels.
Exemples : Bluetooth (téléphone ↔ casque), AirDrop.

### MAN — Metropolitan Area Network
Relie plusieurs LAN à l’échelle d’une ville.

### CAN — Campus Area Network
Portée : plusieurs bâtiments.
Exemple : réseau d’une université ou d’une grande entreprise.


### extra info 

####  La mémoire RAM
La RAM (Random Access Memory) est la mémoire à court terme de l’ordinateur.
    ->Elle stocke temporairement les programmes et données en cours d’utilisation.
    ->Plus la RAM est grande, plus l’ordinateur peut gérer plusieurs tâches rapidement.

#### NEXT chapter : adressage d'un réseau 