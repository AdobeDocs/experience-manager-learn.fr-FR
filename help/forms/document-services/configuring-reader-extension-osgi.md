---
title: Configuration des extensions de Reader dans AEM Forms OSGi
description: Ajout d’informations d’identification des extensions de Reader au Trust Store dans AEM Forms OSGi
feature: Reader Extensions
audience: developer
type: Tutorial
version: 6.4,6.5
topic: Administration
role: Admin
level: Beginner
exl-id: 1f16acfd-e8fd-4b0d-85c4-ed860def6d02
last-substantial-update: 2020-08-01T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 34%

---

# Ajout d’informations d’identification Reader Extensions{#configuring-reader-extension-osgi}

Le service DocAssurance peut appliquer des droits d’utilisation aux documents PDF. Pour appliquer des droits d’utilisation aux documents PDF, configurez les certificats :

## Création d’un fichier de stockage de clés pour l’utilisateur fd-service

Les informations d’identification des extensions Reader sont associées à l’utilisateur fd-service. Pour ajouter les informations d’identification à l’utilisateur du service fd, procédez comme suit. Si vous avez déjà créé le fichier de stockage des clés pour l’utilisateur fd-service, ignorez cette section.

* Connectez-vous à votre instance de création AEM en tant qu’administrateur
* Accédez à Outils-Sécurité-Utilisateurs
* Faites défiler la liste des utilisateurs jusqu’à ce que vous trouviez un compte utilisateur fd-service
* Cliquez sur l’utilisateur fd-service.
* Cliquez sur l’onglet KeyStore .
* Cliquez sur Créer le KeyStore .
* Définissez le mot de passe d’accès KeyStore et enregistrez vos paramètres pour créer le mot de passe KeyStore

### Ajout d’informations d’identification au fichier de stockage des clés d’utilisateur fd-service

Suivez la vidéo pour ajouter les informations d’identification à l’utilisateur du service fd.

>[!VIDEO](https://video.tv.adobe.com/v/335849?quality=9&learn=on)


La commande permettant de répertorier les détails du fichier pfx est . La commande suivante suppose que vous vous trouvez dans le même répertoire que le fichier pfx .

**keytool -v -list -storetype pkcs12 -keystore &lt;name of=&quot;&quot; your=&quot;&quot; pfx=&quot;&quot; file=&quot;&quot;>**

Par exemple, keytool -v -list -storetype pkcs12 -keystore 1005566.pfx où 1005566.pfx est le nom du fichier pfx.
