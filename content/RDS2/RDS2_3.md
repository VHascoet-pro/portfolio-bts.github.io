---
title: "Troisième Partie - Installation de FOG"
---
#### <u>Fonctionnement de FOG</u>

FOG est un serveur de déploiement d'images qui est accessible par deux moyens :

* Le panneau d'administration
* iPXE (ou Network Boot)

Le panneau d'Administration nous permet de lancer les taches de capture d'images, l'état de capture des ordinateurs, les statistiques de téléchargement des images sur des clients etc...

iPXE nous permet de booter sur FOG en tant que client ou hôte afin de pouvoir s'enregisrer, capturer, déployer les images et effectuer des tests d'intégrité du PC (avec l'outil Memtest86+).

FOG comprend plusieurs utilitaires en son sein :

- Un serveur TFTP afin de déployer les images.
- Un serveur DHCP optionnel (j'ai découvert plus tard qu'il était possible de le configurer, je ne l'ai pas fait [Détails en bas de page]).
- Une page WEB d'administration en PHP (installation de Nginx + modules PHP obligatoires).
- Memtest86+ afin d'effectuer des diagnostics mémoire et de disque.

---

Pour faire fonctionner le serveur Fog il faut :

- Un serveur DHCP. (ici, Ubuntu Server 24.04)
- Le serveur FOG qu'il faut ensuite configurer. (un second Ubuntu Server 24.04)
- Intégrer les machines sur l'interface de FOG pour pouvoir deployer ces images sur une machine cliente.

Pour déployer l'image d'un PC depuis le serveur FOG vers le PC en question, il faut d'abord avoir capturé cette-dite image depuis soit une machine virtuelle, soit une machine physique que l'on aura configuré plus tôt.

Pour capturer une image, il faut que la machine qui héberge le système hôte soit déjà enregistrée sur le serveur, on fait cela directement sur l'interface d'administration de FOG.

<u>PS</u>: L'utilisation de deux machines physiques au lieu d'une me permettait une meilleure lisibilité sur mes services, mais la combinaison de DHCP+FOG sur une seule et unique machine et totalement possible (et conseillée par le développeur du serveur).

---

J'ai donc commencé le TP par la conception d'un schéma réseau (comme à l'accoutumée, sur le logiciel Packet Tracer)

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS/sch_réseau-RDS2-FOG.png" alt="schéma réseau sur Packet Tracer Représentant deux machines virtuelles contenues dans un ordinateur principal, relié en réseau sur un serveur de diffusion d'adresses IP (DHCP) et sur le serveur FOG." position="center" style="border-radius: 8px;" caption="" captionPosition="right" captionStyle="color: black;" >}}

---

Une fois le schéma conçu, j'ai pu commencer l'installation du serveur DHCP (isc-dhcp-server) et celui de FOG afin de distribuer les baux sur les clients.

*Ce fichier de configuration n'est pas nécessaire si FOG à été installé avec le daemon DHCP intégré, cependant, il reste utile dans le cas ou le serveur à été installé à part.*

```json
## À la fin du fichier de configuration
if substring(option vendor-class-identifier, 15,5) = "00000" {
    filename "undionly.kpxe"; # BIOS
}

elsif substring(option vendor-class-identifier, 15, 5) = "00006" {
    filename "ipxe32.efi"; # EFI client 32bit
}

else {
   filename "ipxe.efi"; # EFI client 64bit
}
```


Une fois le DHCP configuré et FOG installé, plus besoin de toucher aux fichiers de configuration, il faut maintenant passer sur l'interface graphique.

---

{{<figure src="https://vhascoet-pro.github.io/portfolio-bts.github.io/pics/RDS/2025-01-13_10-52.png" alt="Menu principal de FOG : Montrant le tableau de bord." position="center" style="border-radius: 8px;" caption="Tableau de bord de FOG" captionPosition="right" captionStyle="color: black;">}}

---
| <button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_2';">Précédent</button> | <button onclick="window.location.href='https://vhascoet-pro.github.io/portfolio-bts.github.io/rds2/rds2_4';">Suivant</button> |
|-|-|
