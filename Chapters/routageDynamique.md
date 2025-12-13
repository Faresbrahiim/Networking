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

## 2 Types de routage dynamique

### RIP (Routing Information Protocol)
Version 1 : classful (ne supporte pas CIDR)
Version 2 : classless (supporte CIDR, multicast)

### OSPF (Open Shortest Path First)
Protocol link-state
Calcule le chemin le plus court (algorithme Dijkstra)
Utilisé pour les réseaux d’entreprise

### EIGRP (Enhanced Interior Gateway Routing Protocol)
Propriétaire Cisco, hybride entre distance-vector et link-state

### BGP (Border Gateway Protocol)
Protocol externe, utilisé sur Internet

# to be continued