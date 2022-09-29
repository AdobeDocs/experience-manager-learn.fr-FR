---
title: Installation et configuration de Tomcat
seo-title: Install and Configure Tomcat
description: Ce didacticiel en plusieurs étapes fait partie de la première partie du processus de création de votre premier document de communication interactive. Dans cette partie, nous allons installer TOMCAT et déployer le fichier sampleRest.war dans TOMCAT.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 0%

---

# Installation et configuration de Tomcat {#install-and-configure-tomcat}

Dans cette partie, nous installons TOMCAT et déployons le fichier sampleRest.war dans TOMCAT. Le point de terminaison REST exposé par ce fichier WAR est la base de notre source de données et de notre modèle de données de formulaire.

Pour configurer Tomcat, suivez les instructions suivantes :

1. Téléchargez et installez JDK1.8.
2. Définissez JAVA_HOME pour qu’il pointe vers JDK1.8.
3. Télécharger [tomcat](https://tomcat.apache.org/). Ce fichier war a été testé avec les versions 8.5.x et 9.0.x de Tomcat.
4. Téléchargez la version tomcat de votre choix. Vous pouvez télécharger le fichier zip Windows 64 bits sous la section centrale.
5. Décompressez le contenu dans votre c:\tomcat.
6. Vous devriez voir quelque chose comme ça sur votre disque c **c:\tomcat\apache-tomcat-8.5.27** selon la version de votre tomcat
7. Créez une variable d’environnement appelée &quot;CATALINA_HOME&quot; et définissez sa valeur sur l’exemple de dossier d’installation tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copiez le fichier SampleRest.war dans le dossier webapps de votre installation Tomcat.
9. Lancez la nouvelle fenêtre d&#39;invite de commande.
10. Accédez à &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin et exécutez le fichier startup.bat
11. Une fois que votre tomcat a commencé, testez le point de terminaison exposé par le fichier WAR par [cliquer ici](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Vous devriez obtenir des exemples de données suite à cet appel.

Félicitations !!!. Vous avez configuré et déployé le fichier SampleRest.war.
