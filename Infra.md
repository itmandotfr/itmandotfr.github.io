# Récapitulatif Infra TAI/TIP

# AD
## BASE
- Création VM
- Installation OS
- Nom
- Adressage IP (IP Fixe, Masque, 127.0.0.1 DNS)

## MEP AD
- Installation rôle AD-DS (Active Directory Domain Services)
- Configuration AD (Promotion 1 forêt , 1 domaine) => 5 rôles FSMO

## NTP (rôle AD)
- SNTP native clients AD
- SNTP pour autres (w32tm /config /reliable:yes /update)

## DNS (rôle AD)
- Résolution locale native (domain.local) 
- Redirection DNS (9.9.9.9)

## Manipulation AD basique :
- Activation corbeille AD
- Création groupe, utilisateurs, UO, affectations

## Manipulation AD Avancée :
- GPO (wallpaper user/admin, bloque modif wallpaper + déploiement logiciel MSI Firefox, onlyoffice)

## DHCP
- Installation rôle DHCP
- Configuration (Création, Activation étendue, Plage)
- Configuration options passerelle, DNS, NTP)

## Partage de fichiers :
- Création Disque
- Formatage disque (NTFS) + attribution lecteur logique
- Paramétrage Sécurité NTFS racine
- Création Arborescence (Perso + Services)
- Définition droits NTFS (sécurité)
- Partage et définitions droits de partage
- Mappage des lecteurs réseau (Paramètre User AD + Scripts AD)
- Quotas (pour chaque utilisateur/profil utilisateur)

## Sauvegardes
- VM Complète (VBoxManage)
- Fichiers : robocopy (commande) ou Rôle Windows Server sur nouveau disque
- Switch : Serveur TFTP (Open TFTP)

## Imprimante
- Identifier le constructeur et modèle d'imprimante
- Télécharger le/les pilotes (architecture arm ou x86-32/x64)
- Ajouter rôle serveur d'impression
- Ajouter le pilote dans l'interface de gestion des imprimantes
- Ajouter l'imprimante soit
    - en USB (remonter le périphérique dans VB)
    - en réseau (TCP/IP)
 Partager l'imprimante


# Client AD
## BASE
- Création VM
- Installation OS
- Nom
- Adressage IP (IP dynamique, Masque, passerelle (routeur OPNsense), DNS @IPAD)

## MEP AD
- Intégration Domaine


# GLPI
## BASE
- Création VM
- Installation OS (Debian 12.12)
- Nom
- Adressage IP (IP Fixe, Masque, @IP AD en DNS)

## MEP GLPI
- Installation Apache2
- Installation PHP
- Installation MariaDB
- Site GLPI

## Fonctionnalité SSO 
- Liaison AD pour authentification (création compte AD dédié)
- Importation des utilisateurs
- Automatisation synchro des comptes

## Fonctionnalité Inventaire
- Activation Inventaire (Serveur GLPI)
- Récupération Agent GLPI.MSI
- Script paramétré pour déploiement Agent GLPI (PC AD Client)

## Fonctionnalité Administration/Sécurisation
- SSH
- HTTP
- Pare-feu (ICMP, SSH, HTTP, HTTPS)


# Routeur/filtrage/NAT-PAT
## BASE
- Création VM (2 NIC minimum, em0: LAN => em1: WAN)
- Installation OPNSense (FreeBSD 64)
- Nom
- Adressage IP em0 : IP Fixe, Masque
- Adressage IP em1 : IP Dynamique
- Client NTP (Serveur AD SNTP)

## Fonctionnalités NAT-PAT
- LAN vers WAN en NAT-PAT

## Fonctionnalités Pare-feu
- Permettre LAN vers WAN (uniquement)
- Règles sortantes (ICMP, DNS, HTTP, HTTPS uniquement)

## Fonctionnalités CA
- CA
- Signer CA OPNSense (Web)
- Importation CA Client AD

## Fonctionnalités DMZ
- Ajouter NIC (Virtualisation)
- Ajouter Interface (OS)
- Paramétrage Interface (IP Fixe, Masque)
- Création SRV-Web DMZ



# SRV-WEB (DMZ)
## BASE
- Création VM
- Installation OS (Debian 13 ou WS 2022)
- Nom
- Adressage IP (IP Fixe, Masque, @IP AD en DNS)

## MEP
- Serveur Web (Apache ou IIS)
- PCAD (Remote Management SSH/RDP)
- Pare-feu (local)
- DNAT sur OPNSense (WAN => DMZ)
- Sécurité (règles filtrage totales)


# Switch
## BASE
- Nom
- Mot de passe Console (port série/COM)
- Mot de passe Administrateur (enable)

## Configuration Avancée
- Création VLAN + NOM
- Affectation port Access VLAN (4 premiers ports)
- Attribution IP Fixe VLAN
- Configuration NTP (liaison Serveur AD)
- Désactivation des autres ports (shutdown)
- Sauvegarde configuration OpenTFTP sur AD


# Wifi
## BASE
- Nom
- Adressage IP (IP Fixe, Masque)

## MEP
- Identifiant Wifi (SSID)
- Clé de Sécurité (WPA2...)

## Fonctionnalités Avancées/Sécurisées :
- Cacher SSID
- Isolation Wifi
- Filtrage MAC (liste blanche/liste noire)


# PC Physique
## BASE
- Installation OS
- Nom
- Installation Pilotes/drivers
- Activation Windows
- Adressage IP (IP dynamique, Masque, passerelle (routeur OPNsense), DNS @IPAD)

## MEP AD
- Intégration Domaine


# FOG
## BASE
- Création VM
- Installation OS (Debian 13)
- Nom
- Adressage IP (IP Fixe, Masque, @IP AD en DNS)

## MEP FOG
- Installation Apache2
- Installation PHP
- Installation MariaDB
- Site FOG
- Paramétrage DHCP (option 66 & 67)

# Manipulations Création Template
- Démarrage PC physique
- Boot Menu (généralement F12)
- Wizard création

# Manipulations Déploiement Template
- Depuis Interface Web
- Création Host
- Tâche de déploiement (avec arrêt du PC)
- Démarrage PC physique
- Boot Menu (généralement F12)

# VPN Nomade (nécessite CA)
## Serveur VPN (OPNSense)
- Signer un certificat VPN
- Configuration Service VPN
- Création utilisateur
- Export de la configuration

## Client VPN (Client AD)
- Installation logiciel OpenVPN
- Importation configuration
- Tests de connexion