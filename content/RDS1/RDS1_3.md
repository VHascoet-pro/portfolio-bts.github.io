---
title: "Rapport Partie 2"
---
 Pendant toute la semaine, on à mis en place (Moi et mon maître de stage) un second labo, se basant sur le travail effectué pour l'ancien (le serveur LABANNU va donc être réutilisé).

Le but du labo est de conçevoir une <u>**Liaison inter-site**</u> entre le site de Quimper et celui de Vannes afin que les clients du site de Vannes puissent se connecter sur le serveur de Quimper. Simulant ainsi le réseau actuel de la Chambre des Métiers.

## Description du réseau

<u>Le premier site :</u> Celui de Quimper, doit avoir le serveur LABANNU, il s'agit d'un serveur Active Directory permettant de délivrer le domaine GSB.local, comme le serveur fait partie du site de Quimper, je lui ait attribué un VLAN, le VLAN 10.

Le second site : Celui de Vannes, doit contenir deux parties, la première contient ces serveurs:
- Un serveur AD DS RODC (ou Read Only Domain Controller) permettant de se lier à l'Active Directory LABANNU dans le site de Quimper.
- Un second serveur AD DS qui servira d'Active Directory dédié au site de Vannes.

Pour cette partie du site, j'ai attribué le **VLAN 30** afin de séparer virtuellement les serveurs des clients.

La seconde partie du site servira aux clients de Vannes, je lui ait attribué le **VLAN 40** afin de séparer virtuellement les clients des serveurs.

Chaque site doit être relié par un pare-feu sous pfSense (pour tester les configurations et simplifier la msie en oeuvre, aucune règle de filtrage ne sera mise en place).

En voici le schéma :

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/Schema_Labo2.png" alt="sch_lab2" position="center" style="border-radius: 8px;" caption="Schéma du Labo 2" captionPosition="left" captionStyle="color: black;">}}

Voici également la topologie réseau :
|Nom de la machine|Type|VLAN|N° de Site|Adresse IP|Adresse Réseau|
|:---:|:---:|:---:|:---:|:---:|:---:|
|pfQuimper **->** LABANNU|Pare-Feu|10|1|10.1.10.254||
|pfQuimper **->** Clients Quimper|Pare-Feu|15|1|10.1.15.254|10.1.15.0|
|pfQuimper **->** Clients Vannes|Pare-Feu|16|1|10.1.16.254|10.1.16.0|
|pfQuimper **->** pfVannes|Pare-Feu|60|1|192.168.1.254||
|pfVannes **->** pfQuimper|Pare-Feu|60|2|192.168.1.253|192.168.1.0|
|pfVannes **->** Serveurs|Pare-Feu|30|2|10.2.30.254|10.20.30.0|
|pfVannes **->** Clients Vannes|Pare-Feu|40|2|10.2.40.254|10.2.40.0|
||
|AD RODC|Serveur|30|2|10.2.30.253||
|AD DS|Serveur|30|2|10.2.30.252||
|LABANNU|Serveur|10|1|10.1.10.253|10.1.10.0|
||
|Client 1|PC|40|2|10.2.40.1||
|Client 2|PC|40|2|10.2.40.2||
||
|ESXI Quimper|Serveur|100|5|10.5.100.10|10.5.100.0|
|Switch Quimper|Switch|100|5|10.5.100.20||
||
|ESXI Vannes|Serveur|100|5|10.5.100.100||
|Switch Vannes|Switch|100|5|10.5.100.200||

***

## Matériel requis
|NOM|O.S.|LOCALISATION|DÉTAILS|
|:---:|:---:|:---:|:---:|
|ESXI Quimper|VSphere sous ESXI|Quiper|Hôte du serveur de virtualisation|
|Switch Quimper|Aruba Switch Manager|Quimper||
|pfQuimper|pfSense|Quimper|Liaison intersite|
|||||
|ESXI Vannes|Vsphere sous ESXI|Vannes|Hôte du second serveur de virtualisation|
|Switch Vannes|Aruba Switch Manager|Vannes||
|pfVannes|pfSense|Vannes|Liaison intersite|
|||||
|Client|Pop_Os!|Quimper|Administration "distante"|

## Début du Labo
Une fois cette partie du labo terminé, j'ai entamé la seconde partie.
Pour la seconde partie, j'ai ajouté physiquement deux nouvelles machines, dans la première, j'ai installé l'ESXi et configuré mes quatre machines virtuelles, les deux premières étant reliées à un vSwitch sur le VLAN 30, se sera les serveurs. Les deux dernières connectées au second vSwitch sur le VLAN 40, se sera les clients.

j'ai par la suite installé le second pfSense sur la deuxième machine physique, ce pfSense à deux interfaces réseaux (que j'ai configuré comme dans l'adressage IP à gauche du canvas).

J'ai ensuite relié les deux pfSense.

Pendant une grosse partie de la 2ème et 3ème semaine, il y à eu beaucoup de problèmes avec le pare-feu virtualisé (pare-feu de Quimper) qui n'arrivait pas à faire sortir son traffic de son vSwitch.
J'ai donc du faire transiter la machine virtuelle sur une machine dédiée afin d'utiliser l'interface déjà présente.

Une fois le réseau intégralement ré-architecturé, j'ai testé chaque client à chaque partie du réseau, afin d'être sûr de la pérennité de celui-ci dans le temps, pour éviter d'éventuelles pannes ou des erreurs d'inattention dans mes fichiers de configuration (en effectuant des sauvegardes de mes fichiers de configuration et des snapshots de mes machines virtuelles aux moments où je suis sûr que leur configuration est correcte et que je n'ai aucune modification ultérieure à faire dessus).

J'ai ensuite configuré les IP des deux Switch afin de pouvoir facilement les administrer, j'ai fait de même pour les deux ESXi.
Cinquième et dernière Semaine de Stage

Lors de cette dernière semaine de stage, j'ai mis en place le contrôleur RODC de Vannes pour qu'il puisse communiquer avec LABANNU pour pouvoir devenir un domaine en lecture seule pour pouvoir le faire transiter vers le réseau de Vannes.
Une fois le contrôleur RODC mis en place, j'ai intégré les clients du VLAN 40 dans celui-ci, après avoir crée des utilisateurs, chacun de mes clients (Windows 7) fonctionnant et arrivant à se connecter au serveur.

Ensuite, j'ai installé le second serveur AD DS afin d'avoir un domaine dédié au réseau de Vannes, essayé la connexion avec l'un des clients du VLAN 40, comme il arrivait à se connecter au domaine correctement, je suis passé à la phase suivante du labo.

Pour la phase suivante du labo, il fallait configurer le VLAN 15 (Clients de Quimper) et 16 (clients de Vannes) sur le Switch du réseau de Quimper, afin d'avoir une partie isolée qui sert aux clients externes pour Quimper et Vannes.

Après avoir connecté mon client Linux physique sur le VLAN 15, j'ai pu connecter mon domaine GSB.local à celui-ci, et j'ai réussi à avoir accès aux dossiers partagés que j'avais configuré sur le domaine de Quimper.

Le labo est finalement **terminé**.

***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds1/rds1_2';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds1/rds1_4';">Suivant</button>|
|-|-|