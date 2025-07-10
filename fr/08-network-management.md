# Introduction

Dans cette partie, nous aborderons les points suivants :
* Le réseau informatique (brièvement)
* Le troubleshooting ou dépannage réseau sous Linux 

## Prérequis 

Toujours la même histoire. 😉

# Le réseau informatique (brièvement)

## Introduction au réseau informatique

Un réseau informatique est un ensemble d'ordinateurs et de périphériques connectés entre eux pour partager des ressources, échanger des données et fournir des services. Les réseaux informatiques sont essentiels dans le monde moderne, permettant la communication, le partage de fichiers et l'accès à des ressources distantes.


### Comment tout cela fonctionne ?

Pour que les ordinateurs puissent communiquer entre eux, **ils doivent parler un langage commun** : ce sont les **protocoles réseau**. Le plus fondamental de tous, c’est **TCP/IP**.

---

### Le modèle TCP/IP (le vrai fonctionnement d'Internet)

**TCP/IP**, c’est **l'ensemble de règles (ou "pile de protocoles") utilisé pour faire fonctionner Internet et la plupart des réseaux**.
Il est composé de plusieurs couches, **chacune ayant un rôle spécifique**.

#### Couches du modèle TCP/IP (simplifié) :

| Couche              | Rôle                                                                        |
| ------------------- | --------------------------------------------------------------------------- |
| **1. Accès réseau** | Gère la communication avec le matériel (Wi-Fi, câble Ethernet, etc.)        |
| **2. Internet**     | Permet de trouver l’adresse IP d’un ordinateur sur le réseau (protocole IP) |
| **3. Transport**    | Assure la fiabilité ou la rapidité de la communication (TCP ou UDP)         |
| **4. Application**  | Ce que l’utilisateur utilise : HTTP, FTP, mail, etc.                        |


### TCP et UDP : deux manières de transporter les données

Au niveau de la **couche Transport**, deux protocoles sont principalement utilisés :

#### TCP (Transmission Control Protocol)

* **Fiable** : garantit que toutes les données arrivent **dans le bon ordre**, sans erreur.
* **Connexion** : établit un "lien" entre l’expéditeur et le destinataire avant d’envoyer les données.
* **Exemples d’usages** : sites web (HTTP/HTTPS), mails (SMTP, IMAP), fichiers (FTP).

#### UDP (User Datagram Protocol)

* **Rapide** : envoie les données sans vérifier si elles sont bien reçues.
* **Pas de connexion** : pas d’échange préalable, moins de surcharge.
* **Exemples d’usages** : jeux en ligne, appels audio/vidéo, streaming en direct.

> **Métaphore** :
>
> * TCP, c’est comme envoyer un **colis avec accusé de réception**.
> * UDP, c’est comme envoyer une **carte postale sans garantie qu’elle arrive**.

---

### Le modèle OSI (modèle théorique en 7 couches)

Le **modèle OSI** (*Open Systems Interconnection*) est une **représentation théorique** utilisée pour comprendre comment les données circulent dans un réseau, en les divisant en **7 couches**.

| Couche | Nom                    | Rôle simplifié                                        |
| ------ | ---------------------- | ----------------------------------------------------- |
| 7      | **Application**        | Ce que voit l'utilisateur (navigateur, mail, etc.)    |
| 6      | **Présentation**       | Encodage, chiffrement, compression                    |
| 5      | **Session**            | Gestion des connexions entre applications             |
| 4      | **Transport**          | Fiabilité et ordre des données (TCP/UDP)              |
| 3      | **Réseau**             | Adressage et routage (IP)                             |
| 2      | **Liaison de données** | Communication entre deux machines directement reliées |
| 1      | **Physique**           | Câbles, ondes, signaux électriques                    |

> **Astuce mnémotechnique** pour retenir l’ordre :
> "All People Seem To Need Data Processing" (Application, Présentation, Session, Transport, Réseau, Données, Physique)


### OSI vs TCP/IP — Quelle différence ?

| OSI (7 couches)           | TCP/IP (4 couches)                                                     |
| ------------------------- | ---------------------------------------------------------------------- |
| Modèle **théorique**      | Modèle **réel**, utilisé sur Internet                                  |
| Plus **détaillé**         | Plus **pratique et implémenté**                                        |
| Sépare bien les fonctions | Fusionne certaines couches (ex : application + présentation + session) |

> En pratique, **les réseaux utilisent TCP/IP**, mais **l’OSI aide à comprendre** ce qui se passe à chaque étape.




## Types de Réseaux Informatiques

Vous devez savoir qu'il existe différents types de réseaux :

- **Réseau Local (LAN)** : Un LAN connecte des ordinateurs dans une zone géographique limitée, telle qu'un domicile, un bâtiment ou un campus.
- **Réseau Étendu (WAN)** : Un WAN couvre une zone géographique plus large, connectant plusieurs LAN à travers des villes, des pays ou des continents.
- **Réseau Sans Fil (WLAN)** : Un WLAN connecte des ordinateurs sans fil, utilisant des ondes radio pour transmettre des données.

## Équipements Réseau

Maintenant, parlons des équipements réseau :

- **Routeur** : Un routeur connecte plusieurs réseaux et achemine le trafic entre eux. C'est comme un aiguillage pour vos paquets de données !
- **Commutateur (Switch)** : Un commutateur connecte plusieurs ordinateurs dans un réseau et achemine les paquets de données vers le destinataire prévu.

## Adresses Réseau

Dans un réseau informatique, il y a deux éléments principaux pour identifier une machine. L'adresse IP et l'adresse MAC.

- **Adresse IP** : Une adresse IP est un identifiant unique attribué à un ordinateur dans un réseau, utilisé pour la communication. Vous pouvez avoir une adresse IP statique ou dynamique. 
Exemple: **192.168.34.3**.

- **Adresse MAC** : Une adresse MAC est un identifiant unique attribué à une carte réseau, utilisé comme adresse réseau.
Exemple: **00:1A:2B:3C:4D:5E**.

<br>

**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter: 
    ```bash
    $ ip a ## Affiche les interfaces réseau de votre système ainsi que leurs adresses IP et vos différentes adresses MAC
    ```
<br>

Ci-dessous un exemple de retour.

```
$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 52:54:00:c8:9a:ac brd ff:ff:ff:ff:ff:ff
    inet 172.30.1.2/24 brd 172.30.1.255 scope global dynamic noprefixroute enp1s0
       valid_lft 86302846sec preferred_lft 75513646sec
    inet6 fe80::7008:be3c:4abc:6bcd/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1454 qdisc noqueue state DOWN group default 
    link/ether 02:42:4c:21:02:63 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

Dans notre cas, nous avons 3 interfaces réseau: **lo**, **enp1s0** et **docker0**.

| Interface | Type      | Adresse IPv4  | Adresse IPv6              | Adresse MAC       | Statut            |
| --------- | --------- | ------------- | ------------------------- | ----------------- | ----------------- |
| lo        | Loopback  | 127.0.0.1/8   | ::1/128                   | 00:00:00:00:00:00 | UP                |
| enp1s0    | Ethernet  | 172.30.1.2/24 | fe80::.../64 (link-local) | 52:54:00:c8:9a:ac | UP                |
| docker0   | Virtuelle | 172.17.0.1/16 | (aucune IPv6 ici)         | 02:42:4c:21:02:63 | DOWN (no carrier) |


- **lo** : lo est l'interface loopback, utilisée pour que la machine communique avec elle-même.
- **enp1s0** : enp1s0 est une interface réseau physique Ethernet.
- **docker0** : docker0 est une interface virtuelle créée par Docker pour permettre aux conteneurs de communiquer entre eux et avec l’hôte.


## Focus sur l'interface loopback

L’interface **loopback** (`lo`) permet à une machine de **s’envoyer des messages à elle-même**, comme si elle passait par le réseau. Cela peut paraître inutile, mais en réalité, c’est fondamental pour :

### **1. Test de réseau local (localhost)**

* **127.0.0.1** (ou `localhost`) est utilisée pour **tester des applications réseau localement**, sans avoir besoin d’une vraie connexion réseau.
* Exemple : tu développes un site web sur ton PC. Tu peux lancer un serveur local sur `127.0.0.1:8000`, et y accéder via un navigateur — ça ne sort jamais de la machine.


### **2. Services internes du système**

* Beaucoup de **services système communiquent entre eux via le réseau**, même s’ils tournent sur la même machine (par souci de modularité ou sécurité).
* Exemple : une base de données PostgreSQL, un serveur Redis, etc. Ils écoutent souvent sur `127.0.0.1`, et n’autorisent que les connexions locales.


### **3. Diagnostic réseau**

* Tu peux tester la **pile réseau** sans avoir besoin de câble ni d’Internet :

  ```bash
  ping 127.0.0.1
  ```

  Cela vérifie que le système d’exploitation peut envoyer et recevoir des paquets TCP/IP.


### **4. Sécurité**

* Les services qui **ne doivent pas être accessibles depuis l’extérieur** peuvent être liés uniquement à `127.0.0.1`.
* Exemple : une interface d’administration accessible **uniquement localement**.


###  Exemple concret :

Imaginons que tu développes une application web locale :

```bash
python3 -m http.server 8000
```

Tu peux y accéder depuis ton navigateur via :

```
http://127.0.0.1:8000
```

Même si tu n’as **aucun accès Internet**, cette communication fonctionne via l’interface loopback.

### En résumé :

L’interface `lo` permet à un ordinateur de **s’adresser à lui-même** via des protocoles réseau standard. C’est **indispensable** pour :

* tester des applis locales,
* faire tourner des services système,
* déboguer,
* isoler des services de l’extérieur.



## Configuration réseau et routage sous Linux

### Comprendre le **routage**

En réseau, **"router"** signifie **décider par quel chemin (interface, passerelle) envoyer les paquets IP** pour atteindre une destination. Voyons quelques commandes en rapport avec le routage.


#### `route` (ancienne commande)

La commande `route` est **obsolète**, mais encore utilisée parfois pour afficher la **table de routage** :

```bash
route -n
```

> Cela montre les chemins utilisés pour atteindre les réseaux connus.
> Le `-n` évite la résolution DNS pour un affichage plus rapide.



#### `ip r` ou `ip route` (moderne)

```bash
ip route
# ou plus court :
ip r
```

> Affiche la table de routage actuelle du système.

##### Exemple :

```bash
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100
```

* **default** : route par défaut → tout ce qui n’est pas connu passe par là.
* **via 192.168.1.1** : la **passerelle (gateway)** utilisée.
* **dev eth0** : interface réseau utilisée.
* **192.168.1.0/24** : sous-réseau local.


#### `ip route get`

```bash
ip route get 8.8.8.8
```

> Affiche **le chemin précis** qu'un paquet prendrait pour atteindre une IP donnée.

##### Résultat exemple :

```bash
8.8.8.8 via 192.168.1.1 dev eth0 src 192.168.1.100
```

* Cela confirme quelle interface, IP source, et gateway seront utilisées.

---

### Gateway (passerelle)

La **gateway**, ou passerelle, est **l’adresse IP d’un routeur** sur le réseau local, par lequel **le trafic sortant vers l’extérieur** (ex : Internet) est redirigé.

* **Elle fait le lien entre ton réseau local (LAN) et l'extérieur (WAN/Internet).**
* C’est en général **la box Internet ou le routeur**.

Sans gateway définie → le système **ne peut pas atteindre l’extérieur**.


###  Configuration d’une interface réseau

#### Méthode 1 – **Temporaire** (non persistante)

Utilisation des commandes `ip` :

```bash
# Donner une IP à l’interface eth0
sudo ip addr add 192.168.1.100/24 dev eth0

# Activer l’interface
sudo ip link set eth0 up

# Ajouter une passerelle
sudo ip route add default via 192.168.1.1
```

> ⛔ Cette configuration est **temporaire** : elle disparaît après un redémarrage ou désactivation de l’interface.



#### Méthode 2 – **Persistante** (configuration conservée après redémarrage)

Dépend de la distribution :



#### Debian / Ubuntu (avec Netplan ou interfaces)

##### Avec `netplan` (Ubuntu 18.04+)

Éditer le fichier YAML :

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Exemple de configuration statique :

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

Puis appliquer :

```bash
sudo netplan apply
```



##### Avec `/etc/network/interfaces` (systèmes plus anciens) :

```bash
auto eth0
iface eth0 inet static
  address 192.168.1.100
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8
```

Redémarrer le réseau :

```bash
sudo systemctl restart networking
```

---

####  Red Hat / CentOS / Fedora (avec `nmcli` ou fichiers `ifcfg-`)

Exemple fichier :

```bash
/etc/sysconfig/network-scripts/ifcfg-eth0
```

Contenu :

```ini
DEVICE=eth0
BOOTPROTO=static
ONBOOT=yes
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
```

Redémarrage de l’interface :

```bash
sudo ifdown eth0 && sudo ifup eth0
```



### Résumé

| Commande / Élément            | Rôle                                           |
| ----------------------------- | ---------------------------------------------- |
| `ip r` / `ip route`           | Affiche la table de routage                    |
| `ip route get <IP>`           | Montre le chemin réseau utilisé                |
| `route -n`                    | Ancienne méthode pour voir la table de routage |
| `gateway`                     | Passerelle vers l’extérieur (routeur)          |
| `ip addr add` / `ip link`     | Configuration **temporaire** d’interface       |
| Fichiers YAML ou `interfaces` | Configuration **persistante**                  |


## DNS

Le DNS (Domain Name System) est un système qui traduit les noms de domaine en adresses IP. C'est essentiel pour naviguer sur Internet !
Vous pouvez modifier votre DNS de différentes manières. Soit via le fichier `/etc/resovl.conf`. Soit via l'interface graphique de Network Manager.

- **Via `resolv.conf`** :
 ```bash
 # Éditer le fichier resolv.conf
 sudo nano /etc/resolv.conf

 # Ajoutez la ligne suivante :
 nameserver 8.8.8.8
 ```

 Quelques explications :
* `nameserver` : C’est une directive utilisée pour spécifier un **serveur DNS** (Domain Name System).
* `8.8.8.8` : C’est l’**adresse IP du serveur DNS** que le système utilisera pour **traduire les noms de domaine** (ex : `www.google.com`) en adresses IP (ex : `142.250.190.68`).

<br>

**⚠️ Attention**:

Dans certaines distributions modernes (Ubuntu, Fedora…), ce fichier est généré automatiquement par des services comme systemd-resolved ou NetworkManager. Donc toute modification manuelle de /etc/resolv.conf risque d’être écrasée au redémarrage.


<br>

Parlons à présent du fichier **/etc/hosts** !!!

Avant même de consulter un serveur DNS, Linux vérifie d’abord le fichier **/etc/hosts**.
Ce fichier permet d’associer manuellement des noms d’hôtes à des adresses IP. Il est utilisé en priorité, avant le DNS, pour la résolution locale.


**Exemple de fichier `/etc/hosts`** :

```bash
127.0.0.1       localhost
127.0.1.1       monpc.local monpc
192.168.1.100   serveur-web.local monsite
```
Explication :

* **127.0.0.1** : correspond à la machine elle-même (loopback).
* **localhost** : nom d’hôte par défaut.
* **192.168.1.100 serveur-web.local** : permet d’accéder à ce serveur (serveur-web.local) sans DNS.

**Cas d’usage de `/etc/hosts`**

* **Tester un site en local** avant de modifier les DNS publics.
* **Forcer un nom de domaine** à pointer vers une IP spécifique.
* **Bloquer un site** en le redirigeant vers `127.0.0.1`.
* **Travailler hors ligne** sans serveur DNS.


## Rôle des Ports réseau et des Services

Les ports sont des numéros qui identifient les services qui écoutent sur une machine. Vous pouvez voir les numéros de ports standards et les services dans le fichier `/etc/services`.

- **Exemple** :
    ```bash
    cat /etc/services
    ```

    ```
    ...
    ftp-data	20/tcp
    ftp		    21/tcp
    fsp		    21/udp		fspd
    ssh		    22/tcp				# SSH Remote Login Protocol
    telnet		23/tcp
    smtp		25/tcp		mail
    ```


# Quelques outils de dépannage Réseau sous Linux

## Ping

- **Qu'est-ce que Ping ?** : Ping teste la connectivité et mesure le temps de réponse.
- **Comment utiliser Ping** :
 ```bash
 # Tester la connectivité avec Google
 ping google.com
 ```

## Traceroute

- **Qu'est-ce que Traceroute ?** : Traceroute suit le chemin que prennent les paquets de données de la source à la destination.
- **Comment utiliser Traceroute** :
 ```bash
 # Tracer le chemin vers Google
 traceroute google.com
 ```

## Netstat

- **Qu'est-ce que Netstat ?** : Netstat affiche les connexions Internet / Réseaux actives, les tables de routage et les statistiques d'interface.
- **Comment utiliser Netstat** :
 ```bash
 # Voir les connexions actives
 netstat -tuln
 ```

## Tcpdump

- **Qu'est-ce que Tcpdump ?** : Tcpdump capture et affiche le trafic réseau.
- **Comment utiliser Tcpdump** :
 ```bash
 # Capturer le trafic sur l'interface eth0
 sudo tcpdump -i eth0
 ```

## Wireshark

- **Qu'est-ce que Wireshark ?** : Wireshark analyse le trafic réseau.
- **Comment utiliser Wireshark** :
 ```bash
 # Lancer Wireshark
 sudo wireshark
 ```

## Nmap

- **Qu'est-ce que Nmap ?** : Nmap scanne les réseaux pour les hôtes et les services.
- **Comment utiliser Nmap** :
 ```bash
 # Scanner le réseau 192.168.1.0/24
 nmap -sS 192.168.1.0/24
 ```

## Netcat

- **Qu'est-ce que Netcat ?** : Netcat lit et écrit des connexions réseau.
- **Comment utiliser Netcat** :
 ```bash
 # Connexion à Google sur le port 80
 nc google.com 80
 ```

### SS

- **Qu'est-ce que SS ?** : SS est un outil pour investiguer les sockets.
- **Comment utiliser SS** :
 ```bash
 # Voir les sockets actifs
 ss -tuln
 ```

### MTR

- **Qu'est-ce que MTR ?** : MTR combine Ping et Traceroute.
- **Comment utiliser MTR** :
 ```bash
 # Tester la connectivité et tracer le chemin vers Google
 mtr google.com
 ```


# Entraînement ⚔️

## EXERCICE 1

Faire les challenges du niveau 0 à 20 de la catégorie "Bandit" de la plateforme "overthewire".
Lien: https://overthewire.org/wargames/bandit/ (Ce lien pointe sur la catégorie "Bandit" directement)

Les premiers challenges vous serviront de révision 🙂.

## EXERCICE 2

Vous devez réaliser une analyse réseau complète  sur le serveur public scanme.nmap.org à l’aide de l’outil Nmap , afin de répondre aux questions suivantes : 

1. Quelle est la version du service SSH  (port 22) hébergé sur ce serveur ?
2. Quel service  est disponible sur le port 80/tcp  ?
3. Combien de ports TCP  sont dans l’état open  ?
4. Quel service est associé au port 9929/tcp  ?
     

**⚠️ Attention**: Il est illégal de scanner un réseau ou un site web qui n'est pas le votre sans autorisation. "scanme.nmap.org" est une plateforme de test fournie par nmap afin de tester l'outil nmap librement.

## EXERCICE 3

Lien du script du challenge: https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/Network_EXO_1.sh