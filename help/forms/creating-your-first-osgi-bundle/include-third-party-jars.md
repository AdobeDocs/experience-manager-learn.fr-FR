---
title: Inclusion de fichiers jar tiers
description: Découvrez comment utiliser un fichier jar tiers dans votre projet AEM
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: 11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
source-git-commit: 4af14b7d72ebdbea04e68a9a64afa1a96d1c1aeb
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 2%

---

# Inclusion de lots tiers dans votre projet AEM

Dans cet article, nous allons passer en revue les étapes à suivre pour inclure le bundle OSGi tiers dans votre projet AEM. Pour les besoins de cet article, nous allons inclure le [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) dans notre projet AEM.  Si l’OSGi est disponible dans le référentiel Maven, incluez la dépendance du lot dans le fichier POM.xml du projet.

>[!NOTE]
> On suppose que le fichier jar tiers est un lot OSGi.

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Si votre lot OSGi se trouve sur votre système de fichiers, créez un dossier appelé **localjar** sous le répertoire de base de votre projet (C:\aemformsbundles\AEMFormsProcessStep\localjar), la dépendance ressemblera à ceci :

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Création de la structure de dossiers

Nous ajoutons ce lot à notre projet AEM **AEMFormsProcessStep** qui réside dans la variable **c:\aemformsbundles** folder

* Ouvrez le **filter.xml** à partir de C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element.

* Créez la structure de dossiers suivante C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* Le **apps/AEMFormsProcessStep-vendor-packages** est la valeur de l’attribut racine dans filter.xml.
* Mise à jour de la section des dépendances du fichier POM.xml du projet
* Ouvrez l’invite de commande. Accédez au dossier de votre projet (c:\aemformsbundles\AEMFormsProcessStep) dans mon cas. Exécutez la commande suivante

```java
mvn clean install -PautoInstallSinglePackage
```

Si tout se passe bien, le package est installé avec le lot tiers dans votre instance AEM. Vous pouvez rechercher le lot à l’aide de la fonction [console web felix](http://localhost:4502/system/console/bundles). Le lot tiers est disponible dans le dossier /apps de la propriété `crx` référentiel comme illustré ci-dessous
![tiers](assets/custom-bundle1.png)


