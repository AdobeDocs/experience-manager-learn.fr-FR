---
title: Création de la structure pour le composant Pays
description: Création de la structure de composant dans crx
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
source-git-commit: f9a1fb40aabb6fdc1157e1f2576f9c0d9cf1b099
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 6%

---

# Création de la structure pour le composant Pays

Connectez-vous à votre instance AEM Forms et procédez comme suit pour créer un composant en fonction du composant déroulant prêt à l’emploi :

1. Accédez à &quot;/apps/&lt;votre projet>/components/adaptiveForm/dropdown&quot; dans CRXDE Lite.
2. Copiez le composant déroulant et collez-le au même niveau de répertoire.
3. Renommez le composant copié en pays.
4. Mettez à jour la propriété jcr:title du noeud cq:template vers Country.
5. Enregistrez les modifications.

Vous disposez désormais d’un nouveau composant nommé Pays, qui est une réplique exacte du composant de liste déroulante d’usine. Cela sert de base à une personnalisation plus poussée.

## Création du fichier HTL

Pour créer le fichier HTL pour le composant Pays :

1. Accédez au dossier pays dans le référentiel crx
2. Créez un fichier nommé countries.html.
3. Ouvrez le fichier /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html dans le référentiel crx et copiez son contenu.
4. Collez le contenu copié dans countries.html.
5. Modifiez le code pour utiliser le nouveau modèle sling comme illustré dans la capture d’écran.
6. Enregistrez vos modifications.

![sling-model](assets/countriesdropdown.png)

Enfin, synchronisez votre projet avec ces mises à jour afin de vous assurer que les modifications apportées au référentiel CRX sont répercutées dans votre projet AEM.


## Étapes suivantes

[Création de cq-dialog](./dialog.md)
