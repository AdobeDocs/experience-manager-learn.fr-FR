---
title: Créer votre premier lot OSGi avec AEM Forms
description: Créer votre premier lot OSGi à l’aide de Maven et Eclipse
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 203
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 100%

---


# Créer votre premier lot OSGi

Un lot OSGi est un fichier d’archive Java™ qui contient du code Java, des ressources et un manifeste qui décrit le lot et ses dépendances. Un lot est une unité de déploiement d’application. Cet article est destiné à l’équipe de développement qui souhaite créer un service ou un servlet OSGi à l’aide d’AEM Forms 6.4 ou 6.5. Pour créer votre premier lot OSGi, procédez comme suit :


## Installer JDK

Installez la version prise en charge de JDK. En ce qui me concerne, j’ai installé JDK 1.8. Vérifiez que vous avez ajouté **JAVA_HOME** dans vos variables d’environnement et qu’il pointe vers le dossier racine de votre installation JDK.
Ajoutez %JAVA_HOME%/bin au chemin.

![data-source](assets/java-home.JPG)

>[!NOTE]
> N’utilisez pas JDK 15. Cette version n’est pas prise en charge par AEM.

### Tester votre version JDK

Ouvrez une nouvelle fenêtre d’invite de commande et saisissez : `java -version`. Vous devez recevoir la version JDK identifiée par la variable `JAVA_HOME`.

![data-source](assets/java-version.JPG)

## Installer Maven

Maven est un outil d’automatisation de création utilisé principalement dans les projets Java. Pour installer Maven sur votre système local, procédez comme suit.

* Créez un dossier appelé `maven` dans votre lecteur C.
* Téléchargez l’[archive zip binaire](http://maven.apache.org/download.cgi).
* Procédez à l’extraction du contenu de l’archive zip dans `c:\maven`.
* Créez une variable d’environnement appelée `M2_HOME` avec la valeur suivante : `C:\maven\apache-maven-3.6.0`. J’ai installé la version 3.6.0 de **mvn**. Au moment de la rédaction de cet article, la dernière version de Maven est 3.6.3.
* Ajoutez `%M2_HOME%\bin` à votre chemin.
* Enregistrez vos modifications.
* Ouvrez une nouvelle invite de commande et saisissez `mvn -version`. La version **mvn** doit s’afficher, comme illustré dans la copie d’écran ci-dessous.

![data-source](assets/mvn-version.JPG)

## Settings.xml

Un fichier `settings.xml` Maven définit des valeurs qui permettent de configurer l’exécution Maven de différentes manières. Il est généralement utilisé pour définir un emplacement de référentiel local, d’autres serveurs de référentiel distants et des informations d’authentification pour les référentiels privés.

Accédez à `C:\Users\<username>\.m2 folder`.
Extrayez le contenu du fichier [settings.zip](assets/settings.zip) et placez-le dans le dossier `.m2`.

## Installer Eclipse

Installez la dernière version d’[Eclipse](https://www.eclipse.org/downloads/).

## Créer votre premier projet

Archetype est une boîte à outils permettant de créer des modèles de projet Maven. Un archétype est défini comme un modèle ou un motif d’origine à partir duquel tous les autres éléments de même type sont créés. Le nom reflète bien notre objectif de fournir un système qui offre un moyen cohérent de générer des projets Maven. Archetype permet de créer des modèles de projet Maven à destination des utilisateurs et des utilisatrices, qui peuvent générer des versions paramétrées de ces modèles de projet.
Pour créer votre premier projet Maven, procédez comme suit :

* Créez un dossier appelé `aemformsbundles` dans votre lecteur C.
* Ouvrez une invite de commande, puis accédez à `c:\aemformsbundles`.
* Exécutez la commande suivante dans l’invite de commande :
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Le projet Maven est généré de manière interactive et on vous invite à fournir des valeurs à un certain nombre de propriétés telles que :

| Nom de la propriété | Importance | Valeur |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId identifie de manière unique votre projet dans tous les projets. | com.learning.aemforms.adobe |
| appsFolderName | Nom du dossier contenant la structure de votre projet. | learningaemforms |
| artifactId | artifactId est le nom du fichier jar sans version. Si vous l’avez créé, vous pouvez choisir n’importe quel nom avec des lettres minuscules et pas de symboles étrangers. | learningaemforms |
| version | Si vous le distribuez, vous pouvez alors choisir n’importe quelle version type avec des nombres et des points (1.0, 1.1, 1.0.1, etc.). | 1.0 |

Acceptez les valeurs par défaut des autres propriétés en appuyant sur la touche Entrée.
Si tout se passe bien, vous devriez voir un message de réussite de création dans votre fenêtre de commande.

## Créer un projet Eclipse à partir de votre projet Maven

Remplacez votre répertoire de travail par `learningaemforms`.
Exécutez `mvn eclipse:eclipse` à partir de la ligne de commande.
La commande ci-dessus lit votre fichier POM et crée des projets Eclipse avec les métadonnées correctes afin qu’Eclipse comprenne les types de projets, les relations, le chemin de classe, etc.

## Importer le projet dans Eclipse

Lancez **Eclipse**.

Accédez à **Fichier -> Importer** et sélectionnez **Projets Maven existants** comme illustré ici.

![data-source](assets/import-mvn-project.JPG)

Cliquez sur Suivant.

Sélectionnez les `c:\aemformsbundles\learningaemform` en cliquant sur le bouton **Parcourir**.

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>Vous pouvez choisir d’importer les modules appropriés en fonction de vos besoins. Sélectionnez et importez le module principal uniquement si vous souhaitez créer du code Java dans votre projet.

Cliquez sur **Terminer** pour lancer le processus d’import.

Le projet est importé dans Eclipse et vous pouvez voir plusieurs dossiers `learningaemforms.xxxx`.

Développez `src/main/java` sous le dossier `learningaemforms.core`. Il s’agit du dossier dans lequel vous écrivez la plupart de votre code.

![data-source](assets/learning-core.JPG)

## Créer votre projet

Une fois que vous avez écrit votre service OSGi, ou servlet, vous devez créer votre projet pour générer le lot OSGi pouvant être déployé à l’aide de la console web Felix. Veuillez consulter [SDK client AEMFD](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) pour inclure le SDK client approprié dans votre projet Maven. Vous devez inclure le SDK client AEMFD dans la section des dépendances du `pom.xml` du projet principal, comme illustré ci-dessous.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Pour créer votre projet, veuillez procéder comme suit :

* Ouvrez la **fenêtre d’invite de commande**.
* Accédez à `c:\aemformsbundles\learningaemforms\core`.
* Exécutez la commande `mvn clean install`.
Si tout se passe bien, le lot doit s’afficher à l’emplacement suivant : `C:\AEMFormsBundles\learningaemforms\core\target`. Ce lot est maintenant prêt à être déployé dans AEM à l’aide de la console web Felix.
