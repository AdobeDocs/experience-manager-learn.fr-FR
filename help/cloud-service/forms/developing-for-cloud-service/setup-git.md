---
title: Installation et configuration de Git
description: Initialisation de votre référentiel git local
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 8848
source-git-commit: 9063c3dfd9ab9ac537850694ce6545a3fdc840e9
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 0%

---

# Installer Git


[Installer Git](https://git-scm.com/downloads). Vous pouvez sélectionner les paramètres par défaut et terminer le processus d’installation.
Accédez à l’invite de commande à l’adresse c:\cloudmanager\aem-banking-app type in git —version. Vous devriez voir la version de GIT installée sur votre système.

## Initialisation Du Référentiel Git Local

Vérifiez que vous vous trouvez dans le fichier c:\cloudmanager\aem-banking-app folder

```
git init
```

La commande ci-dessus initialise le projet en tant que référentiel local Git.

```
git add .
```

Cela permet d’ajouter tous les fichiers de projet au référentiel git prêt à être validé dans le référentiel git.

```
git commit -m "initial commit"
```

Cela valide les fichiers dans le référentiel git.



## Enregistrer le référentiel de gestionnaire de cloud avec notre référentiel Git local

Accès au référentiel de votre gestionnaire de cloud
![accéder aux informations sur les rep](assets/cloud-manager-repo.png)
Obtention des informations d’identification du référentiel de cloud manager
![get-credentials](assets/cloud-manager-repo1.png)

Enregistrez le nom d’utilisateur dans le fichier de configuration.

```java
git config --global credential.username "gbedekar-adobe-com"
```

enregistrer le mot de passe dans le fichier de configuration ;

```java
git config --global user.password "bqwxfvxq2akawtqx3oztacb5wax5a7"
```

(Le mot de passe est votre mot de passe du référentiel git de cloud manager)

Enregistrez le référentiel git de Cloud Manager avec votre référentiel git local. La commande ci-dessous associe **adobe** avec le référentiel git de cloud manager distant. Vous pouvez utiliser n’importe quel nom au lieu de **adobe**


```java
git remote add adobe https://git.cloudmanager.adobe.com/techmarketingdemos/Program2-p24107/
```

(Assurez-vous d’utiliser l’URL de votre référentiel)

Vérifier si le référentiel distant est enregistré

```java
git remote -v
```



