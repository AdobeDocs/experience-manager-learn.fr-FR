---
title: Vidéo Installer et configurer Tomcat
description: Il s’agit de la partie 1 du tutoriel sur la création de votre premier document de communication interactive.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
duration: 242
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 100%

---

# Installer et configurer Tomcat {#install-and-configure-tomcat}

Dans cette partie, nous installons Tomcat et déployons le fichier sampleRest.war dans Tomcat. Le point d’entrée REST exposé par ce fichier WAR est la base de notre source de données et de notre modèle de données de formulaire.

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

Pour configurer Tomcat, suivez les instructions suivantes :

1. Téléchargez et installez JDK1.8.
2. Définissez JAVA_HOME pour pointer vers JDK1.8.
3. Téléchargez [Tomcat](https://tomcat.apache.org/). Ce fichier war a été testé avec les versions 8.5.x et 9.0.x de Tomcat.
4. Téléchargez la version de Tomcat de votre choix. Vous pouvez télécharger le fichier zip Windows 64 bits sous la section centrale.
5. Décompressez le contenu dans votre dossier c:\tomcat.
6. L’emplacement sur votre disque C devrait ressembler à ce qui suit : **c:\tomcat\apache-tomcat-8.5.27** selon votre version de Tomcat.
7. Créez une variable d’environnement appelée « CATALINA_HOME » et définissez sa valeur sur le dossier d’installation tomcat, par exemple c:\tomcat\apache-tomcat-8.5.27.
8. Copiez le fichier SampleRest.war dans le dossier webapps de votre installation Tomcat.
9. Lancez la nouvelle fenêtre d’invite de commande.
10. Accédez à &lt;tomcat install folder>\bin et exécutez startup.bat.
11. Une fois que votre Tomcat a démarré, testez le point d’entrée exposé par le fichier WAR en [cliquant ici](http://localhost:8080/SampleRest/webapi/getStatement/9586).
12. Vous devriez obtenir des exemples de données suite à cet appel.

Félicitations. Vous avez configuré Tomcat et déployé le fichier SampleRest.war.

La vidéo suivante explique le déploiement d’un exemple d’application dans Tomcat.
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

## Étapes suivantes

[Créer une source de données RESTful](./create-data-source.md)