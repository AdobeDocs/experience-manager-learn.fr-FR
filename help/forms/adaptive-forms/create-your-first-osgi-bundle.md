---
title: Création de votre premier lot OSGi avec des formulaires AEM
description: Créez votre premier lot OSGi à l’aide de maven et eclipse.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '820'
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
* Téléchargez la [archive zip binaire](http://maven.apache.org/download.cgi)
* Extrayez le contenu de l’archive zip dans `c:\maven`
* Créez une variable d’environnement appelée `M2_HOME` avec la valeur `C:\maven\apache-maven-3.6.0`. Dans mon cas, la variable **mvn** La version est 3.6.0. Au moment de la rédaction de cet article, la dernière version de Maven est 3.6.3.
* Ajoutez la variable `%M2_HOME%\bin` à votre chemin
* Enregistrez vos modifications
* Ouvrez une nouvelle invite de commande et saisissez `mvn -version`. Vous devriez voir la variable **mvn** version répertoriée comme illustré dans la capture d’écran ci-dessous

![data-source](assets/mvn-version.JPG)

## Settings.xml

Un Maven `settings.xml` définit des valeurs qui configurent l’exécution Maven de différentes manières. Il est généralement utilisé pour définir un emplacement de référentiel local, d’autres serveurs de référentiel distants et des informations d’authentification pour les référentiels privés.

Accédez à `C:\Users\<username>\.m2 folder`
Extraire le contenu de [settings.zip](assets/settings.zip) et placez-le dans le `.m2` dossier.

## Installation d’Eclipse

Installez la dernière version de [eclipse](https://www.eclipse.org/downloads/)

## Créer votre premier projet

Archetype est une boîte à outils de modèle de projet Maven. Un archétype est défini comme un modèle ou un modèle d’origine à partir duquel toutes les autres choses du même type sont faites. Le nom correspond à ce que nous essayons de fournir un système qui fournit un moyen cohérent de générer des projets Maven. Archetype aide les auteurs à créer des modèles de projet Maven pour les utilisateurs et fournit aux utilisateurs les moyens de générer des versions paramétrées de ces modèles de projet.
Pour créer votre premier projet Maven, procédez comme suit :

* Créez un dossier appelé `aemformsbundles` dans votre lecteur C
* Ouvrez une invite de commande et accédez à `c:\aemformsbundles`
* Exécutez la commande suivante à l’invite de commande
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

Le projet Maven est généré de manière interactive et vous êtes invité à fournir des valeurs à un certain nombre de propriétés telles que

| Nom de la propriété | Significativité | Valeur |
|------------------------|---------------------------------------|---------------------|
| groupId | groupId identifie de manière unique votre projet dans tous les projets. | com.learningaemforms.adobe |
| appsFolderName | Nom du dossier contenant la structure de votre projet | apprentissage de aemforms |
| artifactId | artifactId est le nom du fichier jar sans version. Si vous l&#39;avez créée, vous pouvez choisir n&#39;importe quel nom avec des lettres minuscules et pas de symboles étranges. | apprentissage de aemforms |
| version | Si vous le distribuez, vous pouvez choisir n&#39;importe quelle version type avec des nombres et des points (1.0, 1.1, 1.0.1, ...). | 1.0 |

Acceptez les valeurs par défaut des autres propriétés en appuyant sur la touche Entrée.
Si tout se passe bien, vous devriez voir un message de réussite de création dans votre fenêtre de commande.

## Créer un projet éclipse à partir de votre projet Maven

Remplacez votre répertoire de travail par `learningaemforms`.
Exécution `mvn eclipse:eclipse` à partir de la ligne de commande La commande ci-dessus lit votre fichier pom et crée des projets Eclipse avec les métadonnées correctes afin qu’Eclipse comprenne les types de projets, les relations, le chemin de classe, etc.

## Importez le projet dans eclipse

Launch **Eclipse**

Accédez à **Fichier -> Importer** et sélectionnez **Projets Maven existants** comme illustré ici

![data-source](assets/import-mvn-project.JPG)

Cliquez sur Suivant

Sélectionnez la `c:\aemformsbundles\learningaemform`s en cliquant sur le bouton **Parcourir** button

![data-source](assets/select-mvn-project.JPG)

>[!NOTE]
>Vous pouvez choisir d&#39;importer les modules appropriés en fonction de vos besoins. Sélectionnez le module principal et importez-le uniquement si vous souhaitez créer du code Java dans votre projet.

Cliquez sur **Terminer** pour lancer le processus d&#39;import

Le projet est importé dans Eclipse et plusieurs `learningaemforms.xxxx` dossiers

Développez l’objet `src/main/java` sous le `learningaemforms.core` dossier. Il s’agit du dossier dans lequel vous écrivez la plupart de votre code.

![data-source](assets/learning-core.JPG)

## Créer votre projet

Une fois que vous avez écrit votre service OSGi, ou servlet, vous devez créer votre projet pour générer le lot OSGi qui peut être déployé à l’aide de la console web Felix. Veuillez consulter [SDK client AEMFD](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) pour inclure le SDK client approprié dans votre projet Maven. Vous devez inclure le SDK client FD AEM dans la section des dépendances de la section `pom.xml` du projet principal, comme illustré ci-dessous.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Pour créer votre projet, procédez comme suit :

* Ouvrir **fenêtre d&#39;invite de commande**
* Accédez à `c:\aemformsbundles\learningaemforms\core`.
* Exécutez la commande `mvn clean install`
Si tout se passe bien, le lot doit s’afficher à l’emplacement suivant : `C:\AEMFormsBundles\learningaemforms\core\target`. Ce lot est maintenant prêt à être déployé dans AEM à l’aide de la console web Felix.
