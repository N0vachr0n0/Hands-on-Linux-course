# Introduction

Futur hacker ? Futur administrateur ou administratrice système Linux ? Futur·e DevOps ? Petit curieux ou petite curieuse ? Passionné(e) d'informatique ? Explorons ensemble cet univers merveilleux qu'est Linux. Linux, ce fameux système d'exploitation open source que tout le monde redoute.

## Avant-propos

Nous vous recommandons de ne pas utiliser d'IA pour faire les exercices car vous êtes en phase d'apprentissage.

# 1. Historique de Linux

Linux est né en 1991, créé par Linus Torvalds, un étudiant finlandais inspiré par UNIX, un système d'exploitation puissant et modulaire développé dans les années 1970 par Ken Thompson, Dennis Ritchie et d'autres chez Bell Labs. UNIX était réputé pour sa stabilité et sa portabilité, mais son code n'était pas libre. Torvalds a voulu recréer un système similaire, gratuit et ouvert, en publiant le noyau Linux, qu'il a partagé avec la communauté.
En l'associant aux outils du projet GNU (lancé par Richard Stallman dans les années 1980 pour créer un système d'exploitation libre similaire à UNIX), Linux est devenu un système d'exploitation complet, souvent appelé GNU/Linux. Il a hérité de ses racines UNIX des concepts comme la gestion multi-utilisateur et le multitâche, tout en évoluant grâce à l'open source. Aujourd'hui, Linux domine le marché des serveurs, des superordinateurs et même des smartphones (via Android), surpassant souvent UNIX en popularité.

Faisons un p'tit récap des points essentiels à retenir:

- **UNIX comme inspiration** : Créé dans les années 1970 chez Bell Labs, UNIX a introduit des concepts clés (stabilité, modularité, multi-utilisateur) qui ont influencé Linux, bien qu’il soit resté propriétaire.

- **Linux, une alternative libre** : En 1991, Linus Torvalds développe le noyau Linux, inspiré d’UNIX, et le rend open source, marquant le début d’un projet collaboratif mondial.

- **Rôle du projet GNU** : Initié par Richard Stallman dans les années 1980, GNU fournit les outils qui, combinés au noyau Linux, créent un système UNIX-like entièrement libre.

- **Force de l’open source** : La communauté mondiale fait évoluer Linux, le rendant adaptable et dominant dans les serveurs, superordinateurs et smartphones (Android).

- **Héritage et impact** : Linux conserve l’esprit d’UNIX tout en le surpassant en accessibilité et en popularité, devenant un pilier de l’informatique moderne.


## Petit Exercice

* Faire des recherches sur les différences entre Unix, Linux, BSD et GNU.
* Qu'est-ce que l’open source ?
* Qu'est-ce qu’un logiciel libre ?
* Qu'est-ce qu’un logiciel propriétaire ?
* Quelle est la différence entre l’open source et un logiciel libre ?


# 2. Les distributions Linux 

Vous connaissez à présent c'est quoi Linux. Vous pouvez vous demander comment l'installer. L'installation de Linux se fait en trois grandes étapes.

1. Le choix de la distribution (Debian ? Ubuntu ? Kali linux ? Linux Mint ? Arch Linux ...)
2. Le choix de la méthode d'installation (OS principal ? Dualboot ? Machine Virtuelle ? Cloud ? )
3. L'installation XD


## Parlons des distributions Linux 

Linux s’installe via une **distribution**. Une distribution Linux (ou "distro") est une version complète et prête à l’emploi du système d’exploitation Linux, construite autour du noyau Linux. Elle inclut non seulement le noyau, mais aussi un ensemble de logiciels, d’outils, de bibliothèques et souvent une interface utilisateur (comme un environnement de bureau), tous adaptés à des besoins spécifiques. Il faut aussi noter que chaque distribution a ses particularités.

Dans l’univers Linux, on parle de **distributions mères**. Une distribution mère est une distribution Linux qui sert de base ou de point de départ pour d’autres distributions dérivées. Ces "mères" sont souvent stables, bien établies, et fournissent une fondation technique sur laquelle d’autres projets construisent leurs propres versions, en y ajoutant des personnalisations ou des objectifs spécifiques.


## Exemples de distributions mères 

* **Debian** : L’une des plus influentes, connue pour sa stabilité. Elle est à l’origine de nombreuses distributions comme Ubuntu, Linux Mint ou Kali Linux.
* **Red Hat** : Principalement utilisée en entreprise, elle a donné naissance à Fedora (version communautaire), CentOS (avant sa transition vers CentOS Stream) et Rocky Linux.
* **Arch Linux** : Minimaliste et flexible, elle inspire des dérivées comme Manjaro, qui mise sur la facilité d’utilisation.

**NB :** Certaines distributions disposent d’une interface utilisateur par défaut, d’autres non (au démarrage du système, vous atterrissez directement sur une interface en ligne de commande).


Une distribution Linux (Ubuntu 24.04) avec un interface utilisateur peut ressembler à ça:

![](./pictures/Ubuntu_24.png)

Une distribution Linux sans interface utilisateur peut ressembler à ça:

![](./pictures/Linux_NOGUI.png)


**Info:** En général, Linux s'installe sur les serveurs sans interface utilisateur. La gestion se fait donc en ligne de commande uniquement.


## Les différences majeures entre les distributions

Nous allons comparer les distributions selon trois critères : **la gestion des paquets et mises à jour**, **la philosophie et le public cible**, et **la facilité d’utilisation**.

1. **Gestion des paquets et mises à jour**

   * **Debian** : Utilise le gestionnaire `apt` (format `.deb`). Privilégie la stabilité avec des mises à jour moins fréquentes, mais rigoureusement testées. Les versions "Stable" peuvent embarquer des logiciels plus anciens.
   * **Red Hat** : Utilise `yum` ou `dnf` (format `.rpm`). Orientée entreprise, avec des cycles longs (RHEL) pour assurer la stabilité. Fedora, sa dérivée, est plus à jour et expérimentale.
   * **Arch Linux** : Utilise `pacman`. Modèle "rolling release" : mises à jour continues, toujours à la pointe, mais moins stable si mal géré.

2. **Philosophie et public cible**

   * **Debian** : Polyvalente, axée sur la liberté logicielle (open source stricte), adaptée aux serveurs, aux postes de travail ou aux utilisateurs techniques.
   * **Red Hat** : Conçue pour les entreprises (RHEL est payant), avec un support commercial. Fedora vise les innovateurs, et Rocky Linux cible ceux qui cherchent une alternative gratuite à RHEL.
   * **Arch Linux** : Destinée aux utilisateurs avancés souhaitant un contrôle total, sans configuration par défaut imposée.

3. **Facilité d’utilisation**

   * **Debian** : Moyennement accessible. Ses dérivées comme Ubuntu simplifient l’installation et l’expérience pour les débutants.
   * **Red Hat** : RHEL est complexe et orienté professionnels. Fedora est plus accessible, tandis que Rocky Linux cherche à offrir une solution simple pour les anciens utilisateurs de CentOS.
   * **Arch Linux** : Pas de facilité native (installation manuelle). Manjaro, sa dérivée, apporte une interface conviviale et simplifiée.

> 💡 Un critère secondaire qu’on pourrait ajouter est la **spécificité de la distribution** : certaines sont orientées sécurité (hacking/pentest), d'autres vers le gaming, ou encore conçues pour un usage quotidien (généralistes) ou adaptées aux machines peu puissantes (légères).


# Exercice ⚔️

* Faire un tour sur https://distrowatch.com/
* Trouver deux distributions Linux en fonction des différentes particularités (hacking, gaming, généraliste et légères)
* Faire des recherches sur Tails OS et Qubes OS
* Qu'est ce qu'une version LTS ?
* Quelles sont les versions LTS de Ubuntu ?
* Qu'est ce que le Dualboot ? Quels sont ses avantages et inconvénients ?
* Qu'est ce qu'un hyperviseur ? Qu'est ce que la virtualisation ? Quels sont les logiciels de virtualisation pour PC ?
* Faire des recherches sur les fournisseurs cloud
* Installer une distribution Linux de son choix en machine virtuelle afin de l'explorer XD

---
---

## Feedback

> ENG: Please give us your feedback about this chapter.

> FR: Faites-nous part de votre avis sur ce chapitre.

> 👉🏾 https://forms.gle/1oYNEGehhKUCMcoP7 