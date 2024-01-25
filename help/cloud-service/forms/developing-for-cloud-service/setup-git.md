---
title: Installer et configurer Git
description: Initialiser votre référentiel git local
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
jira: KT-8848
exl-id: 31487027-d528-48ea-b626-a740b94dceb8
duration: 57
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 100%

---

# Installer Git


[Installer Git](https://git-scm.com/downloads). Sélectionnez les paramètres par défaut et terminez le processus d’installation.
Accédez à l’invite de commande.
Accédez à c:\cloudmanager\aem-banking-app.
Saisissez git --version. La version de GIT installée sur votre système s’affiche.

## Initialiser le référentiel git local

Vérifiez que vous vous trouvez dans le dossier c:\cloudmanager\aem-banking-app

```
git init
```

La commande ci-dessus initialise le projet en tant que référentiel local git.

```
git add .
```

Cela permet d’ajouter tous les fichiers de projet au référentiel git prêt à être validé dans le référentiel git.

```
git commit -m "initial commit"
```

Cela valide les fichiers dans le référentiel git.



## Enregistrer le référentiel de Cloud Manager avec notre référentiel Git local

Accédez à votre référentiel de Cloud Manager.
![Accès aux informations sur les reférentiels.](assets/cloud-manager-repo.png)
Obtenez les informations d’identification du référentiel de Cloud Manager.
![get-credentials](assets/cloud-manager-repo1.png)

Enregistrez le nom d’utilisateur ou d’utilisatrice dans le fichier de configuration.

```java
git config --global credential.username "gbedekar-adobe-com"
```

Enregistrez le mot de passe dans le fichier de configuration.

```java
git config --global user.password "XXXX"
```

(Le mot de passe est le mot de passe de votre référentiel git de Cloud Manager.)

Enregistrez le référentiel git de Cloud Manager avec votre référentiel git local. La commande ci-dessous associe **bankingapp** avec le référentiel git de Cloud Manager distant. Vous pouvez utiliser n’importe quel nom au lieu de **bankingapp**.


```shell
git remote add bankingapp https://git.cloudmanager.adobe.com/<cloud-manager-repo-path>
```

(Assurez-vous d’utiliser l’URL de votre référentiel.)

Vérifiez si le référentiel distant est enregistré.

```java
git remote -v
```

## Étapes suivantes

[Synchroniser AEM avec le projet AEM dans IntelliJ](./intellij-and-aem-sync.md)
