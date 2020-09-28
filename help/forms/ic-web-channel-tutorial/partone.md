---
title: Installation et configuration de Tomcat
seo-title: Installation et configuration de Tomcat
description: Voici la partie 1 du didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR sera la base de notre source de données et de notre modèle de données de formulaire.
seo-description: Voici la partie 1 du didacticiel en plusieurs étapes pour créer votre premier document de communications interactives. Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR sera la base de notre source de données et de notre modèle de données de formulaire.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---


# Installation et configuration de Tomcat {#install-and-configure-tomcat}

Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR sera la base de notre source de données et de notre modèle de données de formulaire.

Pour installer Tomcat, suivez les instructions suivantes :

1. Téléchargez et installez JDK1.8.
2. Définissez JAVA_HOME pour qu’il pointe vers JDK1.8.
3. Téléchargez [tomcat](https://tomcat.apache.org/). Ce fichier de guerre a été testé avec Tomcat versions 8.5.x et 9.0.x.
4. Téléchargez la version tomcat de votre préférence. Vous pouvez télécharger le fichier zip windows 64 bits sous la section core.
5. Décompressez le contenu dans votre dossier c:\tomcat.
6. Vous devriez voir quelque chose comme ça dans votre lecteur c **c:\tomcat\apache-tomcat-8.5.27** selon la version de votre tomcat
7. Créez une variable d’environnement appelée &quot;CATALINA_HOME&quot; et définissez sa valeur sur l’exemple de dossier d’installation tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copiez le fichier SampleRest.war dans le dossier webapps de votre installation Tomcat.
9. Début de la nouvelle fenêtre d&#39;invite de commande.
10. Accédez à &lt;tomcat install folder>\bin et exécutez le fichier startup.bat.
11. Une fois que votre tomcat a commencé, testez le point de terminaison exposé par le fichier WAR en [cliquant ici.](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Vous devriez obtenir des données d’exemple à la suite de cet appel.

Félicitations ! ! ! Vous avez configuré pour et déployé le fichier SampleRest.war.
