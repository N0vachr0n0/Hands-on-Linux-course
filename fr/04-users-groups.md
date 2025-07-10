# Introduction

Linux est un vrai système multi-utilisateurs ! Plusieurs utilisateurs peuvent se connecter et exécuter des tâches en même temps. Il a aussi un mode mono-utilisateur (« single user ») géré par le noyau, utilisé uniquement à des fins de maintenance. Les utilisateurs ont généralement :
* un login et un mot de passe
* un identifiant système (userid ou uid)
* un groupe principal, des groupes secondaires
* un dossier personnel, des fichiers, des données
* des processus en cours d'exécution

## Avant-propos

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

## Prérequis (La répétition est pédagogique XD)

* Avoir une machine virtuelle ou un PC ou un environnement sous Linux (Ubuntu idéalement)
* Être résilient XD

**Info:** Si vous n'avez pas de d'environnement Linux à votre disposition, vous pouvez vous inscrire sur https://killercoda.com et vous rendre ici https://killercoda.com/playgrounds/scenario/ubuntu pour avoir accès à une machine virtuelle sous Ubuntu 24.04 (sans interface graphique bien sûr !!) pendant 1 heure renouvelable gratuitement.

Vous aurez donc cette vue:

![](./pictures/killerkoda_vm.png)


# Gestion des utilisateurs

## Intro 

**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter (ligne par ligne): 
  ```bash
  whoami 
  id
  who
  w
  ```

Ci-dessous un exemple de retour similaire que vous aurez:

![](./pictures/user_intro1_cmd.png)
<br>
![](./pictures/user_intro2_cmd.png)


Un peu d'explication rapide.

### Commande : `whoami`

```bash
widal@j4rd1n-d3s-0mbr3s:~$ whoami
widal
```

- Affiche le **nom de l'utilisateur connecté**.
- Résultat ici : `widal`

---

### Commande : `id`

```bash
widal@j4rd1n-d3s-0mbr3s:~$ id
uid=1000(widal) gid=1000(widal) groupes=1000(widal),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),122(lpadmin),134(lxd),135(sambashare),136(vboxusers),141(libvirt),999(docker)
```

- Affiche l'**identité de l'utilisateur** et ses **groupes** :
  - `uid=1000` → ID utilisateur
  - `gid=1000` → ID de groupe principal
  - `groupes=...` → Groupes secondaires (ex. : `sudo`, `docker`, `plugdev`, etc.)

---

### Commande : `who`

```bash
widal@j4rd1n-d3s-0mbr3s:~$ who
widal    :1           2025-04-04 16:04 (:1)
widal    pts/0        2025-04-04 16:05 (j)
widal    pts/1        2025-04-04 16:05 (j)
widal    pts/2        2025-04-04 16:05 (j)
```

- Montre **qui est connecté** au système.
- `:1` → session graphique (interface)
- `pts/0`, `pts/1`, `pts/2` → terminaux ouverts (par ex. via terminal ou SSH)

---

### Commande : `w`

```bash
widal@j4rd1n-d3s-0mbr3s:~$ w
 17:51:24 up 17 days,  1:47,  4 users,  load average: 1,46, 1,24, 1,20
UTIL.    TTY      DE               LOGIN@   IDLE   JCPU   PCPU QUOI
widal    :1       :1               04avril25 ?xdm?  21:01m  0.01s /usr/libexec/gdm-x-session ...
widal    pts/0    j                04avril25 17jours  0.00s  0.00s /bin/bash
widal    pts/1    j                04avril25  7jours  0.09s  0.09s /bin/bash
widal    pts/2    j                04avril25 13:53   0.02s  0.02s /bin/bash
```

- **Résumé du système** :
  - Système en ligne depuis **17 jours**
  - **4 utilisateurs** connectés (Chaque terminal ouvert ainsi que l'interface graphique représentent un utilisateur connecté)
  - **Charge moyenne** (1, 5, 15 min) : `1.46`, `1.24`, `1.20`

- **Colonnes :**
  - `UTIL.` : Nom d'utilisateur
  - `TTY` : Terminal utilisé
  - `LOGIN@` : Heure/date de connexion
  - `IDLE` : Temps d'inactivité
  - `JCPU` : Temps CPU total utilisé par l'utilisateur
  - `PCPU` : Temps CPU du processus actif
  - `QUOI` : Processus/commande en cours


## C'est quoi le fichier /etc/passwd ?

* Le fichier /etc/passwd recense tous les utilisateurs du système et leurs informations associées.
	* commun à toutes les distributions
	* c'est un fichier public, généralement non restreint en lecture (mais restreint en écriture)
* Une ligne par utilisateur déclaré:
login:mdpopt:uid:gid:commentaire:rép. personnel:shell
* Exemple :
```
djo:x:15:15:Djo Dalton:/home/djo:/bin/bash
```
* L'utilisateur root a toujours l'uid 0 !
* Que signifie le x dans la ligne ?
    ```
    djo:x:15:15:Djo Dalton:/home/djo:/bin/bash
    ```
    * Il indique que le mot de passe se trouve dans **/etc/shadow**
        * très certainement chiffré
        * très certainement « salé »
* Le mot de passe d'un utilisateur est généralement défini par la commande **passwd**

**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter (ligne par ligne): 
    ```
    cat /etc/passwd
    ```

<br>

🔁🃏 **UNO Reverse !!!**

<br>

* Le fichier **/etc/shadow** contient les informations de sécurité des utilisateurs (séparées par :)
* Format de ligne dans **/etc/shadow**:
login:mdpchiffré:datedernierchangement:ageminimum:agemaximum:tempsavertissement:tempsinactivite:dateexpiration:reservé
* Exemple :
```
djo:$y$j9T$3J4GSvuGv7bM4Vn4BRaOm1$3nuzvVPg0VJoidAVUKsMJvf2Hn3Q6.TbC0H5MnqA782:15051:0:99999:7:::
```
	* Password hashé avec MD5 et salé
	* Mdp non changé depuis 15051 jours (après le 01/01/1970 )
	* Pas d'age minimum (0)
	* Age maximum de 99999 jours avant l'expiraton
	* 7 jours avant l'expiraton, l'utilisateur sera averti
* ⚠️ Il est déconseillé de modifier directement les fichiers **/etc/passwd** et **/etc/shadow** afin d'éviter le risque de fautes de frappe, qui rendrait l'authentification impossible

**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter (ligne par ligne): 
  ```bash
  cat /etc/shadow
  ```

## Les commandes de gestion des utilisateurs

### Points d'attention ⚠️
* Seul l'utilisateur **root** (le super utilisateur / l'administrateur) a la capacité d'utiliser les commandes de gestion des utilisateurs et des groupes.

* Un utilisateur simple doit avoir les droits admin. Pour ce faire, il faudra faire appel à la commande **sudo** lors de l'utilisation d'une commande de gestion d'utilisateur. En utilisant la commande **sudo**, vous serez emmené à saisir votre mot de passe.

Ci-dessous une situation.

```
widal@j4rd1n-d3s-0mbr3s:~$ whoami
widal
widal@j4rd1n-d3s-0mbr3s:~$ useradd avrell
useradd: Permission denied.
useradd : impossible de verrouiller /etc/passwd ; réessayer plus tard.
widal@j4rd1n-d3s-0mbr3s:~$ sudo useradd avrell
[sudo] Mot de passe de widal : 
widal@j4rd1n-d3s-0mbr3s:~$ id avrell
uid=1002(avrell) gid=1002(avrell) groupes=1002(avrell)
widal@j4rd1n-d3s-0mbr3s:~$ 
widal@j4rd1n-d3s-0mbr3s:~$ echo "user avrell is there"
user avrell is there
widal@j4rd1n-d3s-0mbr3s:~$ 

```

<br>

### 1. `useradd` – Ajouter un nouvel utilisateur

```bash
sudo useradd nom_utilisateur
```

- Crée un **nouvel utilisateur**.
- ⚠️ Par défaut, **ne crée pas le répertoire personnel** (`/home/nom_utilisateur`) sans option.

#### Exemple :
```bash
sudo useradd -m alice
```
- Crée l’utilisateur `alice`
- Le dossier `/home/alice` est créé avec l’option `-m`.

---

### 2. `usermod` – Modifier un utilisateur existant

```bash
sudo usermod [options] nom_utilisateur
```

- Sert à **changer les infos** d’un utilisateur : groupe, répertoire, shell, etc.

#### Exemples :
```bash
sudo usermod -aG sudo alice
```
➡️ Ajoute `alice` au groupe `sudo`.

```bash
sudo usermod -d /nouveau/chemin alice
```
➡️ Change le dossier personnel de `alice`.

---

### 3. `passwd` – Changer le mot de passe d’un utilisateur

```bash
sudo passwd nom_utilisateur
```

- Permet de **définir ou modifier** le mot de passe d’un utilisateur.

#### Exemple :
```bash
sudo passwd alice
```
➡️ Invite à saisir un nouveau mot de passe pour `alice`.

---

### 4. `userdel` – Supprimer un utilisateur

```bash
sudo userdel nom_utilisateur
```

- Supprime l’utilisateur **sans supprimer son dossier personnel**.

#### Exemple :
```bash
sudo userdel alice
```

#### Supprimer aussi son dossier :
```bash
sudo userdel -r alice
```

---

### `adduser` vs `useradd`

| Commande   | Description |
|------------|-------------|
| `useradd`  | **Commande de bas niveau** : simple, mais nécessite plus d’options. |
| `adduser`  | **Script interactif** : guide étape par étape (mot de passe, info, etc.) |
|            | Crée automatiquement le dossier personnel, le shell, etc. |

#### Exemple :
```bash
sudo adduser bob
```
➡️ Démarre un assistant pour créer un utilisateur complet.

---

### TLDR (Résumé rapide)

| Commande     | Rôle                              |
|--------------|-----------------------------------|
| `useradd`    | Créer un utilisateur (simple)     |
| `adduser`    | Créer un utilisateur (assisté)    |
| `usermod`    | Modifier un utilisateur existant  |
| `passwd`     | Modifier le mot de passe          |
| `userdel`    | Supprimer un utilisateur          |


### Commandes pour changer d’utilisateur

- **su** : Change d’utilisateur ou ouvre une session root (ex. `su utilisateur` pour passer à un autre utilisateur, ou `su` pour devenir root).  
  - Options utiles : `su - utilisateur` (charge l’environnement de l’utilisateur cible, comme si c’était une nouvelle connexion).
- **sudo** : Exécute une commande en tant qu’un autre utilisateur, généralement root (ex. `sudo commande` pour exécuter `commande` avec les privilèges root).  
  - Options utiles : `sudo -u utilisateur commande` (exécute la commande en tant qu’un utilisateur spécifique).
- **whoami** : Affiche l’utilisateur actuel (ex. `whoami` renvoie le nom de l’utilisateur actif, utile pour vérifier après un changement).

<br>

## Exercice ⚔️

* Lien du script du challenge: https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/USER_EXO_1.sh

Ci-dessous un exemple d'exécution:

```
# On télécharge le script du challenge 1
curl -LO https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/USER_EXO_1.sh

# On le rend exécutable
chmod +x USER_EXO_1.sh

# On l'exécute pour démarrer le challenge
./USER_EXO_1.sh
```

# Gestion des Groupes

## Intro

*  Un groupe est un ensemble d'utilisateurs ayant des points communs
	* mêmes autorisations sur certains fichiers
	* mêmes applications utilisées
	* classification d'utilisateurs
	* etc...
* Chaque utilisateur a son groupe de base (principal), qui lui est affecté lors de sa création. Il lui permet d'attribuer ce groupe aux fichiers qu'il crée. Ce groupe de base porte souvent le même nom que l'utilisateur.
* Un groupe a généralement :
	* un id système (group id)
	* un nom de groupe
	* un mot de passe de groupe
	* une liste d'utilisateurs qui fait partie du groupe


**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter (ligne par ligne): 
    ```bash
    groups #Pour voir son ou ses groupe(s) d'appartenance
    id #Pour voir son groupe principal et ses groupes secondaires
    ```


* Le fichier **/etc/group** recense tous les groupes du système et leurs informations associées
	* Comme **/etc/passwd**, il est commun à toutes les distributions
	* c'est également un fichier public accessible en lecture par tout le monde
* Nomenclature de **/etc/group** :
```
nomgroupe:motdepassedugroupe:gid:liste_utilisateurs
```

   * Exemple :
    ```
    admins:x:10:djo,jack,william,avrell
    ```
* Comme pour les utilisateurs, le groupe root a toujours l'guid 0 !

**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter : 
    ```bash
    cat /etc/group
    ```

Le **x** que vous verrez encore ici, représente le mot de passe du groupe. Il se trouve dans **/etc/gshadow**, et comme pour les utilisateurs dans **/etc/shadow** :

* Comme **/etc/shadow**, **/etc/gshadow** ne devrait pas être librement accessible (Info: s'immiscer dans le groupe root est dangereux !)
* Format de ligne dans **/etc/gshadow**:
```
nom_de_groupe:mot_de_passe_chiffré:administrateurs_du_groupe:membre:sgroupe
```
   * Exemple :
    ```
    daemon:$1$bmONNXPt$wRx7Xkxag20WhcT/kBc3p0::root,bin,daemon
    ```
        * Groupe daemon
        * Password hashé avec MD5 et salé
        * root, bin et daemon peuvent prendre ce groupe de base sans que le mot de passe soit demandé
* Il est déconseillé de modifier directement les fichiers **/etc/group** ou le fichier **/etc/gshadow**

## Le groupe sudo ou wheel

* Dans l'univers Linux, le groupe **sudo** et **wheel** sont des groupes qui permettent à un simple utilisateur d'utiliser des droits d'administrateur. Disons des droits de l'utilisateur **root**. **sudo** et **wheel** sont des groupes privilégiés. On les appelle aussi les **sudoers group**.

* **sudo** en plus d'être un groupe est d'abord un package / une commande qui permet à un simple utilisateur de lancer une commande / une application en tant qu'administrateur (root). Petit coup d'oeil à "Run As Administrator" ou "Exécuter en tant qu'administrateur" sur windows.


## Les commandes de gestion des groupes

⚠️ N'oublie pas les points d'attention ⚠️

- **groupadd** : Crée un nouveau groupe (ex. `groupadd nom_du_groupe` pour ajouter un groupe nommé `nom_du_groupe`).
- **groupdel** : Supprime un groupe (ex. `groupdel nom_du_groupe` pour supprimer le groupe `nom_du_groupe`).
- **groupmod** : Modifie les attributs d’un groupe (ex. `groupmod -n nouveau_nom ancien_nom` pour renommer un groupe).
- **usermod** : Modifie l’appartenance d’un utilisateur à un groupe.  
  - Options utiles : `usermod -g groupe_principal utilisateur` (change le groupe principal), `usermod -aG groupe_secondaire utilisateur` (ajoute l’utilisateur à un groupe secondaire sans supprimer les autres).
- **gpasswd** : Gère les mots de passe et membres d’un groupe (ex. `gpasswd -a utilisateur groupe` pour ajouter un utilisateur, ou `gpasswd -d utilisateur groupe` pour le retirer).
- **getent** : Affiche les informations sur un groupe (ex. `getent group nom_du_groupe` pour voir les détails du groupe, comme les membres).
- **groups** : Liste les groupes auxquels appartient un utilisateur (ex. `groups utilisateur` pour afficher les groupes de l’utilisateur spécifié, ou `groups` pour l’utilisateur actuel).


## Gestion du fichier /etc/sudoers

Le fichier `/etc/sudoers` est un fichier de configuration utilisé sur les systèmes Linux/Unix pour définir les permissions des utilisateurs et des groupes concernant l’utilisation de la commande `sudo`. Il détermine qui peut exécuter des commandes en tant qu’administrateur (root) ou un autre utilisateur, et quelles commandes spécifiques ils peuvent exécuter. Ce fichier est crucial pour la sécurité, car une mauvaise configuration peut soit bloquer l’accès légitime, soit accorder trop de privilèges.

- **Emplacement** : `/etc/sudoers` (fichier principal).
- **Format** : Contient des règles sous la forme `utilisateur hôte = (utilisateur_cible) commandes`.
- **Exemple de ligne** : `utilisateur ALL=(ALL) ALL` signifie que `utilisateur` peut exécuter toutes les commandes sur tous les hôtes en tant que n’importe quel utilisateur.

Le fichier `sudoers` ne doit **jamais** être édité directement avec un éditeur classique (comme `nano` ou `vim`), car une erreur de syntaxe peut bloquer l’accès à `sudo`. La méthode recommandée est d’utiliser la commande **visudo**, qui vérifie la syntaxe avant de sauvegarder.

   ```bash
   sudo visudo
   ```
Ci-dessous quelques exemples de ligne.

| Ligne                                   | Signification |
|----------------------------------------|---------------|
| `jean ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl` | L'utilisateur `jean` est autorisé à exécuter uniquement les commandes `/usr/bin/apt` et `/usr/bin/systemctl` en tant qu'utilisateur root ou tout autre utilisateur, depuis n'importe quel hôte. |
| `jean ALL=(ALL) NOPASSWD: ALL`         | L'utilisateur `jean` peut exécuter **toutes** les commandes en tant que n'importe quel utilisateur sans avoir à entrer son mot de passe. |
| `jean ALL=(ALL) ALL`                   | L'utilisateur `jean` peut exécuter **toutes** les commandes en tant que n'importe quel utilisateur, mais devra entrer son mot de passe. |
| `%admins ALL=(ALL) ALL`                | Tous les membres du groupe `admins` peuvent exécuter **toutes** les commandes en tant que n'importe quel utilisateur, avec obligation d'entrer leur mot de passe. (Attention au **%**) |

Pour terminer en beauté, je vous invite à faire des recherches sur:
```
sudo -l
```

## Exercice ⚔️

Exécuter le script pour débuter le challenge comme un grand 😉.

* Lien du script du challenge: https://raw.githubusercontent.com/N0vachr0n0/NoFD/refs/heads/main/USER_EXO_1.sh



