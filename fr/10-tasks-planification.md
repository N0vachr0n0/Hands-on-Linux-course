# Introduction

Dans ce chapitre, nous allons explorer la **planification des tâches** sous Linux, un concept essentiel pour automatiser des processus répétitifs et optimiser la gestion d’un système. Que ce soit pour des sauvegardes, des nettoyages de fichiers temporaires ou des vérifications périodiques, la planification des tâches est une compétence clé pour tout administrateur système ou utilisateur Linux. Nous nous concentrerons sur **cron**, l’outil de planification le plus répandu et puissant sous Linux. Préparez-vous à devenir un maître de l’automatisation chère padawan !

---

## Prérequis 

Toujours la même histoire. 😉

---

# Qu'est-ce que la planification des tâches ?

La **planification des tâches** permet d’exécuter automatiquement des commandes ou des scripts à des moments précis ou à des intervalles réguliers. Imagine un réveil qui, au lieu de sonner, exécute une tâche sur ton système : sauvegarder des fichiers, envoyer des rapports, ou nettoyer des dossiers. Cela te libère des tâches répétitives et garantit que ton système reste à jour sans intervention manuelle.

Sous Linux, plusieurs outils existent pour planifier des tâches :
- **cron** : L’outil standard, flexible et robuste, parfait pour les tâches périodiques.  
- **at** : Pour des tâches ponctuelles à une heure précise.  
- **anacron** : Idéal pour les tâches qui doivent s’exécuter même si le système était éteint au moment prévu.

Dans ce cours, nous nous concentrerons sur **cron**, le choix privilégié pour l’automatisation récurrente.

---

## Cron

### Qu'est-ce que cron ?

**cron** est un **démon** (un programme qui s’exécute en arrière-plan) qui lit des fichiers de configuration appelés **crontabs** pour exécuter des tâches planifiées. Chaque utilisateur peut avoir son propre **crontab**, et il existe aussi un **crontab système** pour les tâches globales. Pense à **cron** comme un chef d’orchestre qui veille à ce que chaque tâche soit jouée au bon moment.

### Structure du crontab

Le fichier **crontab** est composé de lignes, chacune représentant une tâche planifiée. Chaque ligne suit cette structure avec **six champs** :

```
* * * * * commande 2>&1 >/dev/null
^ ^ ^ ^ ^
| | | | |
| | | | +­­ jour de la semaine 0­6
| | | +­­­­ mois 1­12
| | +­­­­­­ jour du mois 1­31
| +­­­­­­­­ heure 0 23
+­­­­­­­­­­ minute 0­59
```

**Note**: **/dev/null** est un fichier qui ne pointe sur rien, la sortie de commande est donc envoyée nulle part. Cela permet de se débarrasser de la sortie pour ne pas la loguer - d'autant que sans
redirection, la sortie standard est envoyée par mail à root. Cela dit, vous pouvez rediriger la sortie dans un fichier de log pour vous tenir informer des erreurs qui peuvent survenir lors d'une tâche planifiée ou utiliser la commande chronic.

Chaque champ peut contenir :
- Une valeur spécifique (par ex., `15` pour 15 minutes).  
- Un astérisque (`*`) pour "toutes les valeurs possibles".  
- Une liste (par ex., `1,3,5`).  
- Une plage (par ex., `1-5`).  
- Un intervalle (par ex., `*/5` pour toutes les 5 minutes).

### Exemples de tâches cron

Voici des exemples concrets pour mieux comprendre :

- **Tous les jours à 14h30** :  
  ```bash
  30 14 * * * /usr/bin/backup.sh
  ```

  Exécute le script backup.sh à 14h30 tous les jours.

- **Tous les lundis à 10h00** :  
    ```bash
    0 10 * * 1 /usr/bin/clean_logs.sh
    ```

    Exécute clean_logs.sh chaque lundi à 10h00.
    
- Le 1er de chaque mois à 12h00 :  
   ```bash
    0 12 1 * * /usr/bin/monthly_report.sh
   ```

    Exécute monthly_report.sh le 1er jour de chaque mois à midi.

- Toutes les heures :  
    ```bash
    0 * * * * /usr/bin/check_status.sh
    ```

    Exécute check_status.sh à chaque début d’heure (00:00, 01:00, etc.).

- Tous les jours à minuit :  
    ```bash
    0 0 * * * /usr/bin/daily_backup.sh
    ```

    Exécute daily_backup.sh chaque jour à 00:00.

<br>

**Astuce pédagogique** : Utilisez l’outil en ligne crontab guru (https://crontab.guru/) pour tester et visualiser les expressions cron en temps réel.

### Édition du fichier crontab

Pour gérer les tâches cron d’un utilisateur, utilisez la commande crontab :

`crontab -e` : Ouvre le fichier crontab de l’utilisateur dans l’éditeur par défaut (par ex., nano ou vi).  

`crontab -l` : Affiche les tâches planifiées dans le crontab de l’utilisateur.  

`crontab -r` : Supprime toutes les tâches planifiées de l’utilisateur (attention, irréversible !).

<br>
<br>


**À tester 👨🏾‍💻👩🏾‍💻:**
- Ouvrir son terminal
- Exécuter: 
    ```bash
    $ crontab -e ## Ouvre le crontab 
    ```
- Ajoutez une ligne : */10 * * * * /home/user/script.sh.
- Enregistrez et quittez. Le démon cron détecte automatiquement les modifications.
<br>

(( J'espère que vous avez créé le fichier de script 😝))


**Note** : Le crontab système (pour les tâches root ou globales) se trouve dans /etc/crontab ou /etc/cron.d/. Il inclut un champ supplémentaire pour spécifier l’utilisateur exécutant la commande.


### Spécificateurs de temps

Pour simplifier certaines planifications, cron propose des spécificateurs spéciaux :

    @reboot : Exécute la tâche une fois au démarrage du système.  
    @yearly : Exécute une fois par an (équivalent à 0 0 1 1 *).  
    @monthly : Exécute une fois par mois (équivalent à 0 0 1 * *).  
    @weekly : Exécute une fois par semaine (équivalent à 0 0 * * 0).  
    @daily : Exécute une fois par jour (équivalent à 0 0 * * *).  
    @hourly : Exécute une fois par heure (équivalent à 0 * * * *).

**Exemple :**

Pour initialiser un service au démarrage :  

```bash
@reboot /usr/bin/start_service.sh
```

## Conseils et meilleures pratiques

Pour éviter les erreurs courantes et optimiser tes tâches cron :

- **Utilise des chemins absolus** : Les variables d’environnement dans cron sont limitées. Toujours spécifier les chemins complets (par ex., /usr/bin/echo au lieu de echo).  

- **Redirige les sorties** : Les tâches cron envoient leurs sorties (stdout/stderr) par e-mail à l’utilisateur. Redirige-les vers un fichier pour éviter cela :  
    
    ```bash
    0 0 * * * /usr/bin/script.sh >> /var/log/script.log 2>&1
    ```
    

 - **Teste tes scripts avant** : Exécute manuellement ton script pour t’assurer qu’il fonctionne correctement avant de l’ajouter au crontab.

 - **Vérifie les permissions** : Assure-toi que les scripts sont exécutables (chmod +x script.sh) et que l’utilisateur cron a les permissions nécessaires.

- **Utilise des logs** : Enregistre les résultats de tes tâches dans des fichiers journaux pour faciliter le débogage.

- **Surveille cron** : Vérifie les logs système (/var/log/cron ou /var/log/syslog) pour voir si tes tâches s’exécutent correctement.

- **Évite les chevauchements** : Si une tâche peut prendre du temps, utilise un fichier de verrouillage (lockfile) pour empêcher plusieurs exécutions simultanées.


<br>

**Exemple de script avec journalisation** :  
```bash

#!/bin/bash
echo "Task started at $(date)" >> /var/log/task.log
# Tâche ici
echo "Task completed at $(date)" >> /var/log/task.log
```


### Mini explication rapide !

**"Évite les chevauchements"** : C’est quoi exactement ?

Lorsque tu programmes une tâche à intervalles réguliers  (par exemple via cron), il peut arriver que la même tâche soit relancée avant que la précédente ne se termine .
Cela peut causer des problèmes : duplication de traitement, surcharge du système, erreurs inattendues...

Imaginons que tu as une tâche cron qui tourne toutes les 5 minutes , mais parfois elle prend plus de 5 minutes  pour finir.
Résultat : deux instances du même script tournent en même temps → conflit possible ! 

**Question:** Comment éviter cela ?

**Réponse:** Utiliser un fichier de verrouillage (lockfile) .

**Principe d'un script utilisant un lockfile:**

1. Au début du script :

    Il vérifie si un fichier de verrou ("/var/lock/monscript.lock", par exemple) existe.
     

2. Si le fichier existe :

    Le script s’arrête → une autre instance est déjà en cours.
     

3. Si le fichier n’existe pas :

    Il crée le fichier de verrou.
    Il exécute la tâche.
     

4. À la fin :

    Il supprime le fichier de verrou.


**Exemple simple d'un script en Bash:**

```bash
#!/bin/bash

LOCKFILE="/var/lock/monscript.lock"

# Vérifier si le fichier de verrou existe
if [ -f "$LOCKFILE" ]; then
  echo "Une autre instance est déjà en cours. Arrêt."
  exit 1
fi

# Créer le fichier de verrou
touch $LOCKFILE

# Code principal de la tâche
echo "Tâche en cours..."
sleep 10

# Supprimer le fichier de verrou à la fin
rm -f $LOCKFILE

```


# Entraînement ⚔️

Pour mettre en pratique tes connaissances sur cron, voici trois exercices progressifs pour maîtriser la planification des tâches. 

## Exercice 1 : Tâche cron simple

Créer une tâche cron qui exécute un script toutes les 5 minutes pour enregistrer la date et l’heure.


## Exercice 2 : Tâche quotidienne avec gestion des erreurs

Planifier une tâche quotidienne pour nettoyer les fichiers temporaires et gérer les erreurs dans un fichier journal.

    
## Exercice 3 : Tâche avancée avec dépendances

Configurer deux tâches cron dépendantes pour une sauvegarde et une vérification.

---
---

## Feedback

> ENG: Please give us your feedback about this chapter.

> FR: Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 https://forms.gle/Br22WxcwgJSeLGkW9