---
title: Configuration des extensions de Reader dans AEM Forms OSGi
description: Ajout d’informations d’identification des extensions de Reader au Trust Store dans AEM Forms OSGi
feature: Reader Extensions
feature-set: Reader Extensions
topics: development
audience: developer
doc-type: Tutorial
activity: implement
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
source-git-commit: 55a6ff5d01898b994aee60f214126c5c18a06a5e
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 8%

---


# Ajout d’informations d’identification Reader Extensions{#configuring-reader-extension-osgi}

Le service DocAssurance peut appliquer des droits d’utilisation aux documents PDF. Pour appliquer des droits d’utilisation aux documents PDF, configurez les certificats.

## Création d’un fichier de stockage de clés pour l’utilisateur fd-service

Les informations d’identification des extensions Reader sont associées à l’utilisateur fd-service. Pour ajouter les informations d’identification à l’utilisateur du service fd, procédez comme suit. Si vous avez déjà créé le fichier de stockage des clés pour l’utilisateur fd-service, ignorez cette section.

* Connectez-vous à votre instance d’auteur AEM en tant qu’administrateur.
* Accédez à Outils-Sécurité-Utilisateurs
* Faites défiler la liste des utilisateurs jusqu’à ce que vous trouviez un compte utilisateur fd-service.
* Cliquez sur l’utilisateur fd-service.
* Cliquez sur l’onglet KeyStore .
* Cliquez sur Créer le KeyStore .
* Définissez le mot de passe d’accès KeyStore et enregistrez vos paramètres pour créer le mot de passe KeyStore.

### Ajout d’informations d’identification au fichier de stockage des clés d’utilisateur fd-service

Suivez la vidéo pour ajouter les informations d’identification à l’utilisateur du service fd.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)











