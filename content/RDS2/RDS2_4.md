---
title: "Troisième Partie - Installation de FOG"
---
#### <u>Fonctionnement de FOG</u>

FOG est un serveur de déploiement d'images qui est accessible par deux moyens :

- Le panneau d'administration
- iPXE (ou Network Boot)

Le panneau d'Administration nous permet de lancer les taches de capture d'images, l'état de capture des ordinateurs, les statistiques de téléchargement des images sur des clients etc...

iPXE nous permet de booter sur FOG en tant que client ou hôte afin de pouvoir s'enregisrer, capturer, déployer les images et effectuer des tests d'intégrité du PC (avec l'outil Memtest86+).

FOG comprend plusieurs utilitaires en son sein :

- Un serveur TFTP afin de déployer les images.
- Un serveur DHCP optionnel (j'ai découvert plus tard qu'il était possible de le configurer, je ne l'ai pas fait [Détails en bas de page]).
- Une page WEB d'administration en PHP (installation de Nginx + modules PHP obligatoires).
- Memtest86+ afin d'effectuer des diagnostics mémoire et de disque.

***
Pour faire fonctionner le serveur Fog il faut :

- Un serveur DHCP. (ici, Ubuntu Server 24.04)
- Le serveur FOG qu'il faut ensuite configurer. (un second Ubuntu Server 24.04)
- Intégrer les machines sur l'interface de FOG pour pouvoir deployer ces images sur une machine cliente.

Pour déployer l'image d'un PC depuis le serveur FOG vers le PC en question, il faut d'abord avoir capturé cette-dite image depuis soit une machine virtuelle, soit une machine physique que l'on aura configuré plus tôt.

Pour capturer une image, il faut que la machine qui héberge le système hôte soit déjà enregistrée sur le serveur, on fait cela directement sur l'interface d'administration de FOG.

<u>PS</u>: L'utilisation de deux machines physiques au lieu d'une me permettait une meilleure lisibilité sur mes services, mais la combinaison de DHCP+FOG sur une seule et unique machine et totalement possible (et conseillée par le développeur du serveur).
***

J'ai donc commencé le TP par la conception d'un schéma réseau (comme à l'accoutumée, sur le logiciel Packet Tracer)

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/FOG/sch_réseau-RDS2-FOG.png" alt="schéma réseau sur Packet Tracer Représentant deux machines virtuelles contenues dans un ordinateur principal, relié en réseau sur un serveur de diffusion d'adresses IP (DHCP) et sur le serveur FOG." position="center" style="border-radius: 8px;" caption="" captionPosition="right" captionStyle="color: black;" >}}

***

Une fois le schéma conçu, j'ai pu commencer l'installation du serveur DHCP (isc-dhcp-server) et celui de FOG afin de distribuer les baux sur les clients.

*Ce fichier de configuration n'est pas nécessaire si FOG à été installé avec le daemon DHCP intégré, cependant, il reste utile dans le cas ou le serveur à été installé à part.*

{{<code language="cpp" title="extrait de /etc/dhcp/dhcpd.conf" id="1" expand="Montrer" collapse="Cacher" isCollapsed="false">}}
option space PXE;
option PXE.mtftp-ip        code 1 = ip-adress;
option PXE.mtftp-cport     code 2 = unsigned integer 16;
option PXE.mtftp-sport     code 3 = unsigned integer 16;
option PXE.mtftp-tmout     code 4 = unsigned integer 8;
option PXE.mtftp-delay     code 5 = unsigned integer 8;
option arch code 93 = unsigned integer 16;

# Les lignes au dessus servent à assigner une série de codes en fonction de l'architecture trouvée lors du network boot.
# Une fois les codes assignés, il faut desservir les fichiers *.efi aux BIOS/UEFI.
# Les assignations des fichiers *.efi se font au même endroit que l'assignation des baux DHCP.

subnet 172.168.1.0 netmask 255.255.55.0{
    range dynamic-bootp 172.168.1.50 172.178.1.100;
    option domain-name-servers 172.168.1.254;

    class "UEFI-32-1" {
        match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00006";
        filename "i386-efi/ipxe.efi";
    }
    class "UEFI-32-2" {
        match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00002";
        filename "i386-efi/ipxe.efi";
    }
    class "UEFI-64-1" {
        match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00007";
        filename "ipxe.efi";
    }
    class "UEFI-64-2" {
        match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00008";
        filename "ipxe.efi";
    }
    class "UEFI-64-3" {
        match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00009";
        filename "ipxe.efi";
    }
    class "legacy" {
        match if substring(option vendor-class-identifier, 0, 20) = "PXEClient:Arch:00000";
        filename "undionly.kpxe";
    }
}
{{</code>}}
#### Spécificité des BIOS/UEFI
Au fur et à mesure du temps, on est assez vite passé du BIOS (32 bits seulement, prise en charge de disques jusqu'à 2.2To, pas de secure boot, pas de fast-boot), au système UEFI (Prise en charge du 32 et 64 bits, de disques allant jusqu'à 9.4Zo, prise en charge du secure boot et un démarrage plus rapide), les différences notables étant la façon dont est stocké celui-ci, le BIOS est dans une ROM tandis qu'un UEFI est dans une mémoire flash (ce qui permet de le mettre à jour bien plus facilement qu'avec un BIOS classique).
L'UEFI est adopté par les grandes marques de cartes-mères depuis 2006 (19 ans déjà !).

Et comme ces deux systèmes sont assez différents dans leur fonctionnement, il nous faut plusieurs moyens de booter en fonction de la génération de l'UEFI ou du BIOS.
Par exemple ici (code au dessus), pour booter sur un BIOS, on doit fournir le fichier **undionly.kpxe**, en revanche pour l'UEFI ce sera toujours **ipxe.efi** (et ici, on ne voit souvent ce fichier car je l'ai assigné à chaque architecture ou il est présent soit *UEFI-32* et *UEFI-64*, *Legacy* étant une architecture classique d'un BIOS ou son code est noté *00000*).
***
Une fois le DHCP configuré et FOG installé, plus besoin de toucher aux fichiers de configuration, il faut maintenant passer sur l'interface graphique.

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/FOG/2025-01-13_10-52.png" alt="Menu principal de FOG : Montrant le tableau de bord." position="center" style="border-radius: 8px;" caption="Tableau de bord de FOG" captionPosition="right" captionStyle="color: black;">}}
***
On peut désormais capturer une machine depuis cette interface, celà nous donne ces images (ce sont des photos) :
|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/FOG/Capture-FOG_1.jpg" alt="Première phase de capture" position="center" style="border-radius: 8px;" caption="Calcul de la taille de l'image." captionPosition="right" captionStyle="color: black;">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/FOG/Capture-FOG_2.jpg" alt="Seconde phase de la capture" position="center" style="border-radius: 8px;" caption="Capture de la partition *FAT32* du périphérique hôte" captionPosition="right" captionStyle="color: black;">}}|
|-|-|

|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/FOG/Capture-FOG_3.jpg" alt="Troisième phase de la capture" position="center" style="border-radius: 8px;" caption="Capture de la seconde partition" captionPosition="right" captionStyle="color: black;">}}|{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS2/FOG/Capture-FOG_4.jpg" alt="Dernière phase de la capture" position="center" style="border-radius: 8px;" caption="Capture de la partition principale en *NTFS* (512Go)" captionPosition="right" captionStyle="color: black;">}}|
|-|-|

La restoration des images sont les mêmes écrans qu'en haut, même interface.
***
Ce TP est donc **terminé**
***
|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_3';">Précédent</button>|<button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_5';">Suivant</button>|
|-|-|
