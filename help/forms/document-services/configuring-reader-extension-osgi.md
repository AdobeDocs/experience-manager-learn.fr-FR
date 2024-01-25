---
title: Configurer Reader Extensions dans AEM Forms OSGi
description: Ajouter des informations d’identification Reader Extensions au Trust Store dans AEM Forms OSGi
feature: Reader Extensions
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
duration: 311
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 100%

---

# Ajouter des informations d’identification Reader Extensions{#configuring-reader-extension-osgi}

Le service DocAssurance peut appliquer des droits d’utilisation aux documents PDF. Pour appliquer des droits d’utilisation aux documents PDF, configurez les certificats :

## Créez un fichier de stockage pour l’utilisateur ou l’utilisatrice fd-service.

Les informations d’identification Reader Extensions sont associées à l’utilisateur ou l’utilisatrice fd-service. Pour ajouter les informations d’identification à l’utilisateur ou l’utilisatrice fd-service, procédez comme suit. Si vous avez déjà créé le fichier de stockage pour l’utilisateur ou l’utilisatrice fd-service, ignorez cette section.

* Connectez-vous à votre instance de création AEM en tant qu’administrateur ou administratrice.
* Accédez à Outils > Sécurité > Utilisateurs.
* Faites défiler la liste des utilisateurs et utilisatrices jusqu’à ce que vous trouviez un compte d’utilisateur ou d’utilisatrice fd-service.
* Cliquez sur l’utilisateur ou l’utilisatrice fd-service.
* Cliquez sur l’onglet KeyStore.
* Cliquez sur Créer KeyStore.
* Définissez le mot de passe d’accès KeyStore et enregistrez vos paramètres pour créer le mot de passe KeyStore.

### Ajouter des informations d’identification au fichier de stockage de l’utilisateur ou de l’utilisatrice fd-service

Regardez la vidéo pour ajouter les informations d’identification à l’utilisateur ou l’utilisatrice fd-service.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=12&learn=on)


La commande permettant de répertorier les détails du fichier PFX est la suivante. La commande suivante implique que vous êtes dans le même répertoire que le fichier PFX.

**keytool -v -list -storetype pkcs12 -keystore &lt;name of your .pfx file>**

Par exemple, keytool -v -list -storetype pkcs12 -keystore 1005566.pfx où 1005566.pfx est le nom du fichier pfx.
