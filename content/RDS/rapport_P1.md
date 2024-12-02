---
title: "Premier Labo"
---
***
Lors de la première partie de la semaine on m'à alloué une salle afin que je puisse faire mes Travaux Pratiques J'ai donc commencé la mise en place de mon premier labo, le même que celui que nous avions commencé au lycée : Le laboratoire GSB.

Il se composait de ceci :
<div align="center">[![Schéma du labo 1](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/Schema_Labo1.jpg)](https://vhascoet-pro.github.io/pics/Schema_Labo1.jpg)</div>

Le plan d'adressage était le suivant :

|Nom de la machine|VLAN|@IP de la machine|@Réseau (CIDR EN /24)|Rôle|
|:---:|:---:|:---:|:---:|:---:|
|LABANNU|10|10.1.10.253|10.1.10.0|Active Directory|
|REZOLAB|10|10.1.10.252|10.1.10.0|Serveur DHCP|
|WEBVISLAB|10|10.1.10.251|10.1.10.0|Serveur WEB|
||||||
|Routeur côté VLAN 10|10|10.1.10.254|10.1.10.0|Routeur Logiciel|
|Routeur côté VLAN 20|20|10.1.20.254|10.1.20.0|Routeur Logiciel|
||||||
|Clients|20|10.1.20.xxx (DHCP)|10.1.20.0||
||||||
|Switch|1|x.x.x.x|x.x.x.x||

|NOM|SYSTÈME D'EXPLOITATION|VIRTUALISÉ ?|
|:---:|:---:|:---:|
|LABANNU|Windows Server 2019|ESXI|
|REZOLAB|Ubuntu Server 24.04|ESXI|
|WEBVISLAB|Ubuntu Server 24.04|ESXI|
|ROUTEUR LOGICIEL|Ubuntu Server 24.04|ESXI|
||||
|CLIENT 1 (B8)|Pop OS|NON|
|CLIENT 2 (B8)|Windows 11 Professionnel|ESXI|
|CLIENT 3 (SALLE INFO)|Windows 11 Professionnel|NON|

Ici, ESXI fait rapport au système d'exploitation utilisé afin de virtualiser toutes les machines, ESXI utilise Virtualbox sur l'hyperviseur Hyper-V.

À l'aide de la fonctionnalité de _Switch Virtuels_ de l'ESXi, j'ai pu installer deux switch virtuels à l'intérieur de celui-ci, le premier switch virtuel (ou VSwitch) était lié à un VLAN, le vlan 60, Le routeur se connectera dessus et le VLAN 20 servira de patte pour aller d'un site à un autre.
J'ai également créé un second VSwitch lié au VLAN 10, qui sera attaché à LABANNU et au Routeur (pour le site 10 - LABANNU).

L'ESXi sera relié au Switch sur un VLAN 1 qui sera taggé dans tous les autres VLAN.
J'y ai donc ajouté le VLAN 10 & 20 sur celui-ci afin de pouvoir faire passer les trames des VSwitch dans l'ESXI et de pouvoir faire fonctionner le routeur.

J'ai ajouté mon client sur le switch dans le VLAN 20, il me servira de **"cobaye"** pour le reste de mes tests et d'être sûr de la façon dont agit ma configuration sur le reste du réseau.
Ensuite, j'ai commencé la configuration du routeur sur l'ESXi, comme la machine n'est pas connectable à internet (il n'y à qu'une seule interface RJ45 dessus), j'ai téléchargé depuis une autre station le paquet isc-dhcp-relay afin de pouvoir passer le routeur en relai DHCP quand il y en aura besoin.

Durant la seconde partie de la semaine, j'ai terminé la mise en place du routeur Linux afin :
- De lui configurer la redirection NAT avec MASQUERADE
- D'autoriser la redirection IPv4 afin d'activer correctement le NAT
- D'ajouter la table de routage avec des routes statiques.
- D'activer le relai DHCP afin de délivrer les baux depuis le serveur LABANNU vers l'autre partie du réseau.

J'ai ensuite testé à l'aide de ping sur les différentes parties du réseau (et le routeur) afin de connaître et de fiabiliser toute ma configuration.

Une fois le tout testé, j'ai commencé la configuration du serveur LABANNU afin d'installer et de configurer:

- Le serveur DNS (ajout de zones de recherche directe et inversées)
- Le serveur Active Directory (AD DS) pour pouvoir créer un domaine en (.local), je l'ai donc nommé GSB.local et ajouté celui-ci dans les domaines de recherches du serveur DNS. Le serveur DHCP afin de pouvoir distribuer des baux sur tout le réseau client (côté client, au VLAN 20 donc)

Une fois le tout testé, j'ai commencé la configuration du serveur LABANNU afin d'installer et de le configurer:

Une fois les adresses attribuées, j'ai commencé à tester la configuration en y mettant un client sur le VLAN 20 (de l'autre côté du Switch donc), comme indiqué ici, je reçois bien le bail DHCPde LABANNU, en y exécutant la commande "**traceroute 10.1.10.253**" sur le client, je vois bienque je passe par la machine LABANNU.GSB.local, voulant dire que le DNS et l'AD DS fonctionne correctement.

Le premier labo est **terminé**
***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds/rapport';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds/rapport_p2';">Suivant</button>|
|---:|:---|
***