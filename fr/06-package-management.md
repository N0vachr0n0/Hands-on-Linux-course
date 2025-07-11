# Introduction

Vous aurez souvent besoin d’installer des logiciels qui ne sont pas fournis avec votre distribution ou supprimer les logiciels indésirables afin qu’ils ne prennent pas de l’espace disque dur. Dans cette partie, nous aborderons la gestion des paquets de logiciels ou applications sur Linux. 

## Avant-propos (La répétition est pédagogique XD)

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

## Prérequis 

Toujours la même histoire. 😉

# Les gestionnaires de paquets (packages) sous linux

Tout d'abord un paquet est une archive qui contient un ensemble de fichiers et de répertoires à déployer sur le système d'exploitation pour permettre le bon fonctionnement d'un logiciel à installer. Un paquet ou package peut nécessiter la présence d'autres packages pour
fonctionner. On parle alors de dépendances. 

## Les dépôts (repo or repository) linux

Un dépôt Linux  (en anglais repository ), c’est un endroit (souvent sur Internet) où sont stockés des packages logiciels , prêts à être téléchargés et installés. Nous pouvons aussi voir les dépôts comme des supermarchés ou des réserves bien organisées  où nous trouvons ces paquets. XD

Sur linux, il existe plusieurs types de dépôts:

- **Les Dépôts officiels:** Ceux fournis par la distribution linux elle-même. 
- **Les Dépôts tiers (third-party repositories):** Ajoutés manuellement par l’utilisateur lors de l'installation d'un logiciel.

Les dépôts linux sont définis dans un ou plusieurs fichiers sur votre OS. Sous Debian/Ubuntu, ils sont listés dans  **/etc/apt/sources.list** et dans les fichiers du dossier **/etc/apt/sources.list.d/**.

**Exemples de lignes dans sources.list** :

```
Types: deb
URIs: http://archive.ubuntu.com/ubuntu
Suites: noble noble-updates noble-backports
Components: main universe restricted multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

```
deb http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse
```


## Les outils de gestion de paquets sous linux

L'installation d'un package passe par un gestionnaire / outil. Ci-dessous un tableau présentant quelques gestionnaires.

| **Gestionnaire** | **Distributions concernées**               | **Fichier source.list ou équivalent**           | **Exemples de commandes**                          |
|------------------|---------------------------------------------|--------------------------------------------------|----------------------------------------------------|
| **APT / APT-GET** | Debian, Ubuntu, Linux Mint, Kali, etc.     | `/etc/apt/sources.list` et `/etc/apt/sources.list.d/` | `apt install`, `apt update`                      |
| **DPKG**          | Debian-based                                | Non applicable (paquet local)                    | `dpkg -i package.deb`, `dpkg -r package`         |
| **DNF**           | Fedora, RHEL 8+, CentOS Stream              | `/etc/yum.repos.d/`                              | `dnf install`, `dnf upgrade`                     |
| **YUM**           | CentOS 7, RHEL 7, Fedora <21                | `/etc/yum.repos.d/`                              | `yum install`, `yum update`                      |
| **ZYPPER**        | openSUSE, SUSE Linux Enterprise             | `/etc/zypp/repos.d/`                             | `zypper install`, `zypper update`                |
| **PACMAN**        | Arch Linux, Manjaro, EndeavourOS            | `/etc/pacman.conf` + `/etc/pacman.d/mirrorlist` | `pacman -S`, `pacman -Syu`                       |
| **SNAP**          | Toutes les distros supportant Snap          | Géré via `snap install` ou fichiers internes     | `snap install`, `snap refresh`                   |
| **FLATPAK**       | Toutes les distros supportant Flatpak       | `/var/lib/flatpak/` ou `~/.local/share/flatpak/` | `flatpak install flathub`, `flatpak update`      |
| **APPIMAGE**      | Toutes les distros                         | Aucun fichier de dépôt nécessaire                | Télécharger → rendre exécutable → lancer         |

<br>

**Info**:

⚠️ dpkg ne gère pas automatiquement les dépendances. En cas d’erreur, utilisez apt --fix-broken install.


## Exploration du gestionnaire APT (Advanced Package Tool)

APT est l’un des gestionnaires de paquets Linux les plus populaires, car il est fourni avec Ubuntu et d’autres distributions basées sur Debian. Voici quelques exemples en action.

| **Action**                          | **Commande APT**                                   | **Description** |
|------------------------------------|-----------------------------------------------------|------------------|
| Mettre à jour la liste des paquets | `sudo apt update`                                   | Télécharge la liste mise à jour des paquets depuis les dépôts. |
| Installer un paquet                | `sudo apt install nom_du_paquet`                  | Installe le paquet spécifié et ses dépendances. |
| Supprimer un paquet                | `sudo apt remove nom_du_paquet`                   | Désinstalle le paquet mais laisse les fichiers de configuration. |
| Supprimer un paquet + config       | `sudo apt purge nom_du_paquet`                    | Supprime le paquet ET ses fichiers de configuration. |
| Mettre à jour les paquets          | `sudo apt upgrade`                                  | Met à jour tous les paquets installés vers leur dernière version. |
| Mise à niveau du système           | `sudo apt full-upgrade`                           | Comme `upgrade`, mais supprime ou installe des paquets si nécessaire. |
| Rechercher un paquet               | `apt search nom_paquet`                          | Cherche un paquet contenant le mot-clé donné. |
| Obtenir des infos sur un paquet    | `apt show nom_du_paquet`                          | Affiche les détails d’un paquet (version, taille, dépendances, etc.). |
| Nettoyer les anciens téléchargements | `sudo apt clean`                                 | Supprime les anciens fichiers `.deb` téléchargés. |
| Supprimer les paquets inutiles     | `sudo apt autoremove`                             | Supprime les paquets installés automatiquement et qui ne sont plus nécessaires. |
| Lister les paquets disponibles     | `apt list all`                                    | Affiche tous les paquets disponibles (installés ou non). |
| Lister les paquets installés       | `apt list --installed`                            | Montre uniquement les paquets actuellement installés. |

Désolé pour ceux qui ne sont pas sur une distribution basée sur debian 😝.

**INFO:** L'option **--help** ou **-h** et la commande **man** est votre meilleur ami pour avoir des infos sur l'utilisation d'une commande.

## Bon à savoir

L'installation d'un logiciel sur linux se fait principalement via un gestionnaire de paquet mais il est aussi possible d'installer des logiciels manuellement via des dépôts GitHub ou d’autres sources et méthodes.

# Entraînement ⚔️

## Exercice 1

1. Installer l'application **figlet** et afficher le texte "it's crazy" avec figlet
2. Installer l'application **cowsay** et afficher le texte "subarashi" avec cowsay
3. Installer l'application **atop** et visualiser vos métriques système.

Test : Tapez figlet -v, cowsay -h et atop -V pour vérifier l’installation.

## Exercice 2 (Deep dive)

Il est temps de passer aux choses sérieuses !!! <br>
Ce challenge consiste à rechercher le fichier flag.zip et de le compresser afin d'obtenir le flag (chaîne de caractères spéciale).
Avant de démarrer le challenge, il faudra mettre en place l'environnement comme suit:

**Info:**Prêtez attention à votre prompt.

```bash
# Installation du prérequis: l'app docker
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker --now
sudo usermod -aG docker $USER # Afin d'utiliser la commande docker sans sudo dans vos prochaines sessions

# Initialisation de l'environnement
sudo docker ps # Affiche les conteneurs en cours d'exécution
sudo docker run -dit --name ctf-sysadmin fs0ci3ty/adminsys_basic-ctf # Téléchargement et mise en place du conteneur du challenge
sudo docker ps
sudo docker exec -it ctf-sysadmin bash # Permet de rentrer dans le conteneur Docker et de lancer le terminal bash pour pouvoir y exécuter des commandes.
```

A ce stade vous devez être dans le conteneur docker et non dans le terminal rattaché à votre OS comme illustré ci-dessous.

![](./pictures/prompt_docker_chall.png)

<br>

L'environnement du challenge étant prêt, vous pouvez débuter le challenge. Bonne chance !

### INFO

* Cours rapide: Docker est un outil qui permet de lancer des applications dans des environnements isolés appelés "conteneurs" . C’est **comme** une **mini-machine** virtuelle , mais beaucoup plus légère et rapide à démarrer. Dans notre cas, Docker nous permet de mettre en place une version super légère d'une distribution linux prête à être utilisée dans un conteneur.
* Les indices du challenge se trouvent ici: https://github.com/N0vachr0n0/NoFD/blob/main/Hint_PKG_EXO_2.md
* La remise en place de l'environnement se fait comme suit:

```bash
exit # Pour quitter le conteneur et reprendre votre prompt initial
sudo docker rm -f ctf-sysadmin # Suppression du conteneur
sudo docker run -dit --name ctf-sysadmin fs0ci3ty/adminsys_basic-ctf 
sudo docker exec -it ctf-sysadmin bash
```

---
---

## Feedback

> ENG: Please give us your feedback about this chapter.

> FR: Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 https://forms.gle/QxgTWzCfPTpg9Mks7