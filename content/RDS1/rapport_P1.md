---
title: "Premier Labo"
---
***
Lors de la première partie de la semaine on m'à alloué une salle afin que je puisse faire mes Travaux Pratiques J'ai donc commencé la mise en place de mon premier labo, le même que celui que nous avions commencé au lycée : Le laboratoire GSB.

Il se composait de ceci :
{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/Schema_Labo1.jpg" alt="sch_gsb" position="center" style="border-radius: 8px;" caption="Schéma du Laboratoire GSB (allegé pour cause de manque de temps à l'entreprise)" captionPosition="left" captionStyle="color: black;" >}}

Le plan d'adressage était le suivant :

|Nom de la machine|VLAN|@IP de la machine|@Réseau (CIDR EN /24)|Rôle|Nom VSwitch|
|:---:|:---:|:---:|:---:|:---:|:---:|
|LABANNU|10|10.1.10.253|10.1.10.0|Active Directory|SW_SIO1.1||
|REZOLAB|10|10.1.10.252|10.1.10.0|Serveur DHCP|SW_SIO1.1|
|WEBVISLAB|10|10.1.10.251|10.1.10.0|Serveur WEB|SW_SIO1.1|
||||||
|Routeur côté VLAN 10|10|10.1.10.254|10.1.10.0|Routeur Logiciel|RT|
|Routeur côté VLAN 20|20|10.1.20.254|10.1.20.0|Routeur Logiciel|RT|
||||||
|Clients|20|10.1.20.xxx (DHCP)|10.1.20.0|Clients|LAB1.1|
||||||
|Switch|1|x.x.x.x|x.x.x.x|||

|NOM|SYSTÈME D'EXPLOITATION|VIRTUALISÉ ?|
|:---:|:---:|:---:|
|LABANNU|Windows Server 2019|OUI|
|REZOLAB|Ubuntu Server 24.04|OUI|
|WEBVISLAB|Ubuntu Server 24.04|OUI|
|ROUTEUR LOGICIEL|Ubuntu Server 24.04|OUI|
||||
|CLIENT 1 (B8)|Pop OS|NON|
|CLIENT 2 (B8)|Windows 11 Professionnel|OUI|
|CLIENT 3 (SALLE INFO)|Windows 11 Professionnel|NON|

Ici, ESXI fait rapport au système d'exploitation utilisé afin de virtualiser toutes les machines, ESXI utilise VMware vCenter sur l'hyperviseur Hyper-V.

À l'aide de la fonctionnalité de _Switch Virtuels_ de l'ESXi, j'ai pu installer deux switch virtuels à l'intérieur de celui-ci, le premier switch virtuel (ou VSwitch) était lié à un VLAN, le vlan 60, Le routeur se connectera dessus et le VLAN 20 servira de patte pour aller d'un site à un autre.
J'ai également créé un second VSwitch lié au VLAN 10, qui sera attaché à LABANNU et au Routeur (pour le site 10 - LABANNU).

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/Capture_vswitch.png" alt="cap_vswitch" position="center" style="border-radius: 8px;" caption="Capture de la configuration des VSwitchs" captionPosition="left" captionStyle="color: black;">}}

Ensuite, j'ai commencé la configuration du routeur sur l'ESXi, comme la machine n'est pas connectable à internet (il n'y à qu'une seule interface RJ45 dessus), j'ai téléchargé depuis une autre station le paquet _isc-dhcp-relay_ afin de pouvoir passer le routeur en relai DHCP quand il y en aura besoin.

Durant la seconde partie de la semaine, j'ai terminé la mise en place du routeur Linux afin :
- De configurer la redirection NAT avec MASQUERADE.
- D'autoriser la redirection IPv4 afin de rendre le NAT fonctionnel.
- D'ajouter la table de routage avec des routes statiques (pas de routes dynamiques car il n'y en à pas besoin pour ce labo).
- D'activer le relai DHCP sur le pare-feu (qui sert ici de routeur) afin de délivrer les baux depuis le serveur LABANNU vers l'autre partie du réseau.

<br>J'ai ensuite testé à l'aide de pings les différentes parties du réseau afin de connaître et de fiabiliser toute ma configuration :

|![capture vlan 10](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/capture_10.png)|![capture vlan 20](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/capture_20.png)|![capture vlan 60](https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/capture_60.png)|
|:---:|:---:|:---:|
|Ping vers le réseau situé dans le **VLAN 10**.|Ping vers le réseau situé dans le **VLAN 20**|Ping vers le réseau situé dans le **VLAN 60**|

Une fois le tout verifié, j'ai commencé la configuration du serveur LABANNU :

1. Changement sur nom par défaut (généralement WIN-xxxxxxxx) en LABANNU afin de simplifier la connexion et configuration de services qui vont interagir avec lui.
2. Le serveur DNS (ajout de zones de recherche directes et inversées nécessaire au bon fonctionnement de l'Active Directory, il est donc conseillé de le configurer en premier).
3. Le serveur Active Directory ([AD DS](https://learn.microsoft.com/fr-fr/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)) pour pouvoir créer un domaine en (*.local), je l'ai donc nommé **GSB.local** et ajouté celui-ci dans les domaines de recherches du serveur DNS. Le serveur DHCP afin de pouvoir distribuer des baux sur tout le réseau client (côté client, au VLAN 20 donc)

Une fois les adresses attribuées, j'ai commencé à tester la configuration en y mettant un client sur le VLAN 20 (de l'autre côté du Switch donc), comme indiqué ici, je reçois bien le bail DHCP de LABANNU, en y exécutant la commande "**traceroute 10.1.10.253**" sur le client. Je vois bel et bien que je passe par la machine <u>LABANNU.GSB.local</u>, voulant dire que le DNS et l'Active Diretory fonctionne correctement.

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/capture_ipconfig.png" alt="cap_ipconf" position="center" style="border-radius: 8px;" caption="Capture de l'Active Directory" captionPosition="left" captionStyle="color: black;">}}

Le premier labo est **terminé**
***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds1/rapport';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds1/rapport_p2';">Suivant</button>|
|---:|:---|
***
