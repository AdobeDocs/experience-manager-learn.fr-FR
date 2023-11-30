---
title: Inclure des fichiers JAR tiers
description: Découvrez comment utiliser un fichier JAR tiers dans votre projet AEM.
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 100%

---

# Inclure des lots tiers dans votre projet AEM.

Cet article décrit les étapes à suivre pour inclure le lot OSGi tiers dans votre projet AEM. Pour les besoins de cet article, nous allons inclure le fichier [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) dans notre projet AEM.  Si OSGi est disponible dans le référentiel Maven, incluez la dépendance du lot dans le fichier POM.xml du projet.

>[!NOTE]
> On suppose que le fichier JAR tiers est un lot OSGi.

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Si votre lot OSGi se trouve sur votre système de fichiers, créez un dossier appelé **localjar** sous le répertoire de base de votre projet (C:\aemformsbundles\AEMFormsProcessStep\localjar) ; la dépendance ressemblera à ceci :

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Créer la structure de dossiers

Nous ajoutons ce lot à notre projet AEM **AEMFormsProcessStep** qui réside dans le dossier **c:\aemformsbundles**.

* Ouvrez **filter.xml** à partir du dossier C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault de votre projet.
Prenez note de l’attribut racine de l’élément de filtre.

* Créez la structure de dossiers suivante : C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* **apps/AEMFormsProcessStep-vendor-packages** est la valeur de l’attribut racine dans filter.xml.
* Mettre à jour la section des dépendances du fichier POM.xml du projet
* Ouvrez l’invite de commande. Accédez au dossier de votre projet (c:\aemformsbundles\AEMFormsProcessStep, dans mon cas). Exécutez la commande suivante :

```java
mvn clean install -PautoInstallSinglePackage
```

Si tout se passe bien, le package est installé avec le lot tiers dans votre instance AEM. Vous pouvez rechercher le lot à l’aide de la [console web Felix](http://localhost:4502/system/console/bundles). Le lot tiers est disponible dans le dossier /apps du référentiel `crx` comme illustré ci-dessous :
![third-party](assets/custom-bundle1.png)
