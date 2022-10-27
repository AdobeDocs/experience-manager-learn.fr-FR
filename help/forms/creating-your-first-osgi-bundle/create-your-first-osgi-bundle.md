---
title: Création de votre premier lot OSGi avec AEM Forms
description: Créez votre premier lot OSGi à l’aide de Maven et Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '667'
ht-degree: 3%

---

# Création de votre premier lot OSGi

Un lot OSGi est un fichier d’archive Java™ qui contient du code Java, des ressources et un manifeste qui décrit le lot et ses dépendances. Le lot est l’unité de déploiement d’une application. Cet article est destiné aux développeurs qui souhaitent créer un service OSGi ou un servlet utilisant AEM Forms 6.4 ou 6.5. Pour créer votre premier bundle OSGi, procédez comme suit :


## Installation du JDK

Installez la version prise en charge de JDK. J’ai utilisé JDK1.8. Assurez-vous que vous avez ajouté **JAVA_HOME** dans vos variables d’environnement et pointe vers le dossier racine de votre installation JDK.
Ajoutez %JAVA_HOME%/bin au chemin

![data-source](assets/java-home.JPG)

>[!NOTE]
> N’utilisez pas JDK 15. Il n’est pas pris en charge par AEM.

### Test de votre version JDK

Ouvrez une nouvelle fenêtre d’invite de commande et saisissez : `java -version`. Vous devez récupérer la version du JDK identifiée par la variable `JAVA_HOME` variable

![data-source](assets/java-version.JPG)

## Installer Maven

Maven est un outil d’automatisation de génération utilisé principalement pour les projets Java. Pour installer Maven sur votre système local, procédez comme suit.

* Créez un dossier appelé `maven` dans votre lecteur C
* Téléchargez la [archive zip binaire](https://maven.apache.org/download.cgi)
* Extrayez le contenu de l’archive zip dans `c:\maven`
* Créez une variable d’environnement appelée `M2_HOME` avec la valeur `C:\maven\apache-maven-3.6.0`. Dans mon cas, la variable **mvn** La version est 3.6.0. Au moment de la rédaction de cet article, la dernière version de Maven est 3.6.3.
* Ajoutez la variable `%M2_HOME%\bin` à votre chemin
* Enregistrez vos modifications
* Ouvrez une nouvelle invite de commande et saisissez `mvn -version`. Vous devriez voir la variable **mvn** version répertoriée comme illustré dans la capture d’écran ci-dessous

![data-source](assets/mvn-version.JPG)


## Installation d’Eclipse

Installez la dernière version de [eclipse](https://www.eclipse.org/downloads/)

## Créer votre premier projet

Archetype est une boîte à outils de modèle de projet Maven. Un archétype est défini comme un modèle ou un modèle d’origine à partir duquel toutes les autres choses du même type sont faites. Le nom correspond à ce que nous essayons de fournir un système qui fournit un moyen cohérent de générer des projets Maven. Archetype aide les auteurs à créer des modèles de projet Maven pour les utilisateurs et fournit aux utilisateurs les moyens de générer des versions paramétrées de ces modèles de projet.
Pour créer votre premier projet Maven, procédez comme suit :

* Créez un dossier appelé `aemformsbundles` dans votre lecteur C
* Ouvrez une invite de commande et accédez à `c:\aemformsbundles`
* Exécutez la commande suivante à l’invite de commande

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.14
```

Une fois la création terminée, un message de réussite doit s’afficher dans la fenêtre de commande.

## Créer un projet éclipse à partir de votre projet Maven

* Remplacez votre répertoire de travail par `mysite`
* Exécuter `mvn eclipse:eclipse` de la ligne de commande. La commande lit votre fichier pom et crée des projets Eclipse avec les métadonnées correctes afin qu’Eclipse comprenne les types de projets, les relations, le chemin de classe, etc.

## Importez le projet dans eclipse

Launch **Eclipse**

Accédez à **Fichier -> Importer** et sélectionnez **Projets Maven existants** comme illustré ici

![data-source](assets/import-mvn-project.JPG)

Cliquez sur Suivant

Sélectionnez le c:\aemformsbundles\mysite by clicking the **Parcourir** button

![data-source](assets/mysite-eclipse-project.png)

>[!NOTE]
>Vous pouvez choisir d&#39;importer les modules appropriés en fonction de vos besoins. Sélectionnez le module principal et importez-le uniquement si vous souhaitez créer du code Java dans votre projet.

Cliquez sur **Terminer** pour lancer le processus d&#39;import

Le projet est importé dans Eclipse et plusieurs `mysite.xxxx` dossiers

Développez l’objet `src/main/java` sous le `mysite.core` dossier. Il s’agit du dossier dans lequel vous écrivez la plupart de votre code.

![data-source](assets/mysite-core-project.png)

## Inclure le SDK client AEMFD

Vous devez inclure le sdk client AEMFD dans votre projet pour tirer parti des différents services fournis avec AEM Forms. Veuillez consulter [SDK client AEMFD](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) pour inclure le SDK client approprié dans votre projet Maven. Vous devez inclure le SDK client FD AEM dans la section des dépendances de `pom.xml` du projet principal, comme illustré ci-dessous.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Pour créer votre projet, procédez comme suit :

* Ouvrir **fenêtre d&#39;invite de commande**
* Accédez à `c:\aemformsbundles\mysite\core`.
* Exécutez la commande `mvn clean install -PautoInstallBundle`
La commande ci-dessus crée et installe le lot dans le serveur AEM s’exécutant sur `http://localhost:4502`. Le lot est également disponible sur le système de fichiers à l’adresse
   `C:\AEMFormsBundles\mysite\core\target` et peuvent être déployés à l’aide de [Console web Felix](http://localhost:4502/system/console/bundles)
