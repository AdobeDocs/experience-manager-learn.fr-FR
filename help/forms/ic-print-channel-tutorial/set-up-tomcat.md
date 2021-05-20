---
title: Installation et configuration de Tomcat
seo-title: Installation et configuration de Tomcat
description: Ce didacticiel en plusieurs étapes fait partie de la première partie du processus de création de votre premier document de communication interactive. Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR sera la base de notre source de données et de notre modèle de données de formulaire.
seo-description: Ce didacticiel en plusieurs étapes fait partie de la première partie du processus de création de votre premier document de communication interactive. Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR sera la base de notre source de données et de notre modèle de données de formulaire.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 1%

---


# Installer et configurer Tomcat {#install-and-configure-tomcat}

Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR sera la base de notre source de données et de notre modèle de données de formulaire.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Pour configurer Tomcat, suivez les instructions suivantes :

1. Téléchargez et installez JDK1.8.
2. Définissez JAVA_HOME pour qu’il pointe vers JDK1.8.
3. Téléchargez [tomcat](https://tomcat.apache.org/). Ce fichier war a été testé avec les versions 8.5.x et 9.0.x de Tomcat.
4. Téléchargez la version tomcat de votre choix. Vous pouvez télécharger le fichier zip Windows 64 bits sous la section centrale.
5. Décompressez le contenu dans votre c:\tomcat.
6. Vous devriez voir quelque chose comme ça dans votre lecteur c **c:\tomcat\apache-tomcat-8.5.27** en fonction de la version de votre tomcat
7. Créez une variable d’environnement appelée &quot;CATALINA_HOME&quot; et définissez sa valeur sur l’exemple de dossier d’installation tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copiez le fichier SampleRest.war dans le dossier webapps de votre installation Tomcat.
9. Lancez la nouvelle fenêtre d&#39;invite de commande.
10. Accédez à &lt;tomcat install folder>\bin et exécutez le fichier startup.bat.
11. Une fois que votre tomcat a commencé, testez le point de terminaison exposé par le fichier WAR en [cliquant ici](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Vous devriez obtenir des exemples de données suite à cet appel.

Félicitations !!!. Vous avez configuré et déployé le fichier SampleRest.war.

La vidéo suivante explique le déploiement d’un exemple d’application dans Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815)