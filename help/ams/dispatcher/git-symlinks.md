---
title: Ajouter correctement des liens symboliques dans GIT
description: Instructions sur comment et où ajouter des liens symboliques lorsque vous travaillez sur vos configurations Dispatcher.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 6e751586-e92e-482d-83ce-6fcae4c1102c
duration: 364
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1231'
ht-degree: 100%

---

# Ajouter des liens symboliques dans GIT

[Table des matières](./overview.md)

[&lt;- Précédent : Contrôle de l’intégrité de Dispatcher](./health-check.md)

Dans AMS, vous obtenez un référentiel GIT prérempli contenant le code source de Dispatcher, opérationnel pour le développement et la personnalisation.

Après avoir créé votre premier fichier `.vhost` ou votre fichier de premier niveau `farm.any`, vous devrez créer un lien symbolique entre le répertoire `available_*` et le répertoire `enabled_*`. L’utilisation du type de lien approprié est essentielle pour un déploiement réussi via le pipeline Cloud Manager. Cette page explique comment procéder.

## Archétype de Dispatcher

La personne chargée du développement AEM démarre généralement son projet à partir de [l’archétype AEM](https://github.com/adobe/aem-project-archetype).

Voici un exemple de la zone du code source où des liens symboliques sont utilisés :

```
$ tree dispatcher
dispatcher
└── src
   ├── conf.d
.....SNIP.....
    │   └── available_vhosts
    │   │   ├── 000_unhealthy_author.vhost
    │   │   ├── 000_unhealthy_publish.vhost
    │   │   ├── aem_author.vhost
    │   │   ├── aem_flush.vhost
    │   │   ├── aem_health.vhost
    │   │   ├── aem_lc.vhost
    │   │   └── aem_publish.vhost
    └── dispatcher_vhost.conf
    │   └── enabled_vhosts
    │   │   ├── aem_author.vhost -> ../available_vhosts/aem_author.vhost
    │   │   ├── aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
    │   │   ├── aem_health.vhost -> ../available_vhosts/aem_health.vhost
    │   │   └── aem_publish.vhost -> ../available_vhosts/aem_publish.vhost
.....SNIP.....
    └── conf.dispatcher.d
    │   ├── available_farms
    │   │   ├── 000_ams_author_farm.any
    │   │   ├── 001_ams_lc_farm.any
    │   │   └── 002_ams_publish_farm.any
.....SNIP.....
    │   └── enabled_farms
    │   │   ├── 000_ams_author_farm.any -> ../available_farms/000_ams_author_farm.any
    │   │   └── 002_ams_publish_farm.any -> ../available_farms/002_ams_publish_farm.any
.....SNIP.....
17 directories, 60 files
```

Par exemple, le répertoire `/etc/httpd/conf.d/available_vhosts/` contient les fichiers `.vhost` potentiels que nous pouvons utiliser dans notre configuration en cours d’exécution.

Les fichiers activés `.vhost` apparaîtront sous la forme d’un chemin relatif `symlinks` dans le répertoire `/etc/httpd/conf.d/enabled_vhosts/`.

## Créer un lien symbolique

Nous utilisons des liens symboliques vers le fichier afin que le serveur web Apache traite le fichier de destination comme le même fichier.  Mieux vaut éviter de dupliquer le fichier dans les deux répertoires.  Il s’agit simplement d’un raccourci entre un répertoire (lien symbolique) et l’autre.

Il faut savoir que les configurations déployées cibleront un hôte Linux.  La création d’un lien symbolique non compatible avec le système cible entraîne des échecs et des résultats indésirables.

Si votre poste de travail n’est pas une machine Linux, certaines commandes doivent être utilisées pour créer correctement ces liens afin qu’ils puissent être validés dans GIT.

> `TIP:`Il est important d’utiliser des liens relatifs, car si vous avez installé une copie locale du serveur web Apache et que vous avez une base d’installation différente, les liens fonctionneront toujours.  Si vous utilisez un chemin absolu, votre poste de travail ou d’autres systèmes devront correspondre exactement à la même structure de répertoires.

### OSX/Linux

Les liens symboliques sont natifs de ces systèmes d’exploitation et voici quelques exemples de création de ces liens.  Ouvrez votre application de terminal préférée et utilisez les exemples de commandes suivants pour créer le lien :

```
$ cd <LOCATION OF CLONED GIT REPO>\src\conf.d\enabled_vhosts
$ ln -s ../available_vhosts/<Destination File Name> <Target File Name>
```

Voici un exemple de commande renseigné :

```
$ git clone https://github.com/adobe/aem-project-archetype.git
$ cd aem-project-archetype/src/main/archetype/dispatcher.ams/src/conf.d/enabled_vhosts/
$ ln -s ../available_vhosts/aem_flush.vhost aem_flush.vhost
```

Voici un exemple de lien si vous listez le fichier à l’aide de la commande `ls` :

```
ls -l
total 0
lrwxrwxrwx. 1 root root 35 Oct 13 21:38 aem_flush.vhost -> ../available_vhosts/aem_flush.vhost
```

### Windows

> `Note:`Il s’avère que MS Windows (mieux, NTFS) prend en charge les liens symboliques depuis Windows Vista.

![Image de l’invite de commande Windows montrant la sortie d’aide de la commande mklink.](./assets/git-symlinks/windows-terminal-mklink.png)

> `Warning:`La commande mklink, qui permet de créer des liens symboliques, nécessite des privilèges d’administration pour s’exécuter correctement. Même si vous avez un compte d’administration, vous devez exécuter l’invite de commandes « En tant qu’administrateur », sauf si le mode de développement est activé.
> <br/>Autorisations incorrectes :
> ![Image de l’invite de commande Windows montrant l’échec de la commande en raison des autorisations.](./assets/git-symlinks/windows-mklink-underpriv.png)
> <br/>Autorisations adéquates :
> ![Image de l’invite de commande Windows exécutée « En tant qu’administrateur ».](./assets/git-symlinks/windows-mklink-properpriv.png)

Voici la ou les commandes pour créer le lien :

```
C:\<PATH TO SRC>\enabled_vhosts> mklink <Target File Name> ..\available_vhosts\<Destination File Name>
```


Voici un exemple de commande renseigné :

```
C:\> git clone https://github.com/adobe/aem-project-archetype.git
C:\> cd aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts\
C:\aem-project-archetype\src\main\archetype\dispatcher.ams\src\conf.d\enabled_vhosts> mklink aem_flush.vhost ..\available_vhost\aem_flush.vhost
symbolic link created for aem_flush.vhost <<===>> ..\available_vhosts\aem_flush.vhost
```

#### Mode de développement (Windows 10)

En [mode de développement](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development), Windows 10 permet de tester plus facilement les applications que vous développez, d’utiliser l’environnement shell Ubuntu Bash, de modifier divers paramètres axés sur lee développement et d’effectuer d’autres tâches de ce type.

Microsoft semble continuer à ajouter des fonctionnalités au mode de développement, ou à activer certaines de ces fonctionnalités par défaut une fois qu’elles ont été largement adoptées et qu’elles sont considérées comme stables (par exemple, avec la mise à jour de création, l’environnement Shell Ubuntu Bash n’a plus besoin du mode de développement).

Qu’en est-il des liens symboliques ? Lorsque le mode de développement est activé, il n’est pas nécessaire d’exécuter une invite de commande avec des privilèges élevés pour pouvoir créer des liens symboliques. Par conséquent, une fois que le mode de développement est activé, tous les utilisateurs et utilisatrices peuvent créer des liens symboliques.

> Après avoir activé le mode de développement, les utilisateurs et utilisatrices doivent se déconnecter et se reconnecter pour que les modifications prennent effet.

Vous pouvez maintenant voir que la commande fonctionne sans être exécutée « En tant qu’administrateur ».

![Image de l’invite de commande Windows exécutée par une personne normale avec le mode de développement activé.](./assets/git-symlinks/windows-mklink-devmode.png)

#### Approche alternative/programmatique

Une stratégie spécifique permet à certaines personnes de créer des liens symboliques → [Créer des liens symboliques (Windows 10) - Sécurité Windows | Documents Microsoft](https://docs.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/create-symbolic-links)

Avantages :
- Cela peut être exploité par les clientes et clients pour permettre la création de liens symboliques par programmation aux développeurs et développeuses de leur organisation (c’est-à-dire Active Directory) sans avoir à activer le mode de développement manuellement sur chaque appareil.
- En outre, cette politique doit être disponible dans les versions antérieures de MS Windows qui ne proposent pas le mode de développement.

Inconvénients :
- Cette politique semble n’avoir aucun effet sur les personnes appartenant au groupe d’administration. Les personnes chargées de l’administration doivent toujours exécuter une invite de commande avec des privilèges élevés. Bizarre.

> La déconnexion et la reconnexion sont nécessaires pour que les modifications apportées à la politique locale/de groupe prennent effet.

Exécutez `gpedit.msc`, ajoutez/modifiez des personnes selon les besoins. Les personnes chargées de l’administration sont là par défaut.

![Fenêtre de l’éditeur de politique de groupe présentant les autorisations de modification.](./assets/git-symlinks/windows-localpolicies-symlinks.png)

#### Activer des liens symboliques dans GIT

Git gère les liens symboliques en fonction de l’option core.symlinks.

Source : [Documentation Git - git-config](https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks)

*Si core.symlinks a la valeur false, les liens symboliques sont extraits sous la forme de petits fichiers simples contenant le texte du lien. `git-update-index[1]` et `git-add[1]` ne modifient pas le type enregistré en fichier normal. Utile pour les systèmes de fichiers tels que FAT qui ne prennent pas en charge les liens symboliques.
La valeur par défaut est true, sauf pour `git-clone[1]` ou `git-init[1] will probe and set core.symlinks false if appropriate when the repository is created.`. Dans la plupart des cas, Git supposera que Windows ne convient pas aux liens symboliques et définira cette valeur sur false.*

Le comportement de Git sous Windows est bien expliqué ici : Liens symboliques ・ Wiki git-for-windows/git ・ GitHub

> `Info` : les hypothèses répertoriées dans la documentation ci-dessus semblent correspondre à une configuration possible d’AEM Developer sous Windows, notamment NTFS et le fait que nous n’avons que des liens symboliques de fichier et des liens symboliques de répertoire.

Voici la bonne nouvelle. Depuis la [version 2.10.2 Git pour Windows](https://github.com/git-for-windows/git/releases/tag/v2.10.2.windows.1), le programme d’installation dispose d’une [option explicite pour activer la prise en charge des liens symboliques.](https://github.com/git-for-windows/git/issues/921)

> `Warning` : l’option core.symlink peut être fournie au moment de l’exécution lors du clonage du référentiel ou peut être stockée en tant que configuration globale.

![Affichage du programme d’installation GIT affichant la prise en charge des liens symboliques.](./assets/git-symlinks/windows-git-install-symlink.png)

Git pour Windows stocke les préférences globales dans `"C:\Program Files\Git\etc\gitconfig"`. Ces paramètres peuvent ne pas être pris en compte par d’autres applications clientes de bureau Git.
La difficulté est que toutes les personnes chargées du développement n’utiliseront pas le client Git natif (c’est-à-dire Git Cmd, Git Bash), et certaines des applications de bureau Git (par exemple, GitHub Desktop, Atlassian Sourcetree) peuvent avoir des paramètres/valeurs par défaut différents pour utiliser le système ou un Git intégré.

Voici un exemple de ce qui se trouve à l’intérieur du fichier `gitconfig`.

```
[diff "astextplain"]
    textconv = astextplain
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge -- %f
    process = git-lfs filter-process
    required = true
[http]
    sslBackend = openssl
    sslCAInfo = C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
[core]
    autocrlf = true
    fscache = true
    symlinks = true
[pull]
    rebase = false
[credential]
    helper = manager-core
[credential "https://dev.azure.com"]
    useHttpPath = true
[init]
    defaultBranch = master
```

#### Conseils de ligne de commande Git

Il peut y avoir des scénarios où vous devez créer de nouveaux liens symboliques (par exemple, ajouter un nouveau fichier vhost ou fichier de batterie).

Nous avons vu dans la documentation ci-dessus que Windows offre une commande « mklink » pour créer des liens symboliques.

Si vous utilisez un environnement Git Bash, vous pouvez utiliser la commande Bash standard `ln -s` à la place, mais elle devra être précédée d’une instruction spéciale comme dans l’exemple suivant :

```
MSYS=winsymlinks:nativestrict ln -s test_vhost_symlink ../dispatcher/src/conf.d/available_vhosts/default.vhost
```

#### Résumé

Pour que Git gère correctement les liens symboliques (au moins dans le cadre de la ligne de base actuelle de la configuration Dispatcher AEM) sur un système d’exploitation Windows Microsoft, vous aurez besoin de ce qui suit :

| Élément | Version/configuration minimale | Version/configuration recommandée |
|------|---------------------------------|-------------------------------------|
| Système d’exploitation | Windows Vista ou version ultérieure | Mise à jour Windows 10 Creator Update ou mise à jour plus récente |
| Système de fichiers | NTFS | NTFS |
| Possibilité de gérer des liens symboliques pour l’utilisateur ou l’utilisatrice Windows | `"Create symbolic links"` groupe/stratégie locale `under "Group Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment"` | Mode de développement de Windows 10 activé |
| GIT | Version 1.5.3 du client natif | Version 2.10.2 ou ultérieure du client natif |
| Configuration Git | Option `--core.symlinks=true` lors de l’exécution d’un clone Git à partir d’une ligne de commande | Configuration globale Git <br/>`[core]`<br/> liens symboliques = true <br/>. Chemin de configuration de client Git natif : `C:\Program Files\Git\etc\gitconfig`. <br/>Emplacement standard pour le client de bureau Git : `%HOMEPATH%\.gitconfig`. |

> `Note:` si vous disposez déjà d’un référentiel local, vous devrez effectuer une nouvelle duplication à partir de l’origine. Vous pouvez effectuer un clonage vers un nouvel emplacement et fusionner manuellement vos modifications locales non validées/non mises en page dans le référentiel nouvellement cloné.
