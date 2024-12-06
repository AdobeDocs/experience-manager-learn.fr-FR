---
title: Créer la structure pour le composant Pays
description: Créer la structure de composant dans CRX
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
workflow-type: ht
source-wordcount: '215'
ht-degree: 100%

---

# Créer la structure pour le composant Pays

Connectez-vous à votre instance AEM Forms et procédez comme suit pour créer un composant en fonction du composant déroulant prêt à l’emploi :

1. Accédez à « /apps/&lt;yourproject>/components/adaptiveForm/dropdow » dans CRXDE Lite.
2. Copiez le composant déroulant et collez-le au même niveau de répertoire.
3. Renommez le composant copié en « pays ».
4. Mettez à jour la propriété jcr:title du nœud cq:template vers Pays.
5. Enregistrez les modifications.

Vous disposez désormais d’un nouveau composant nommé Pays, qui est une réplique exacte du composant déroulant prêt à l’emploi. Il s’utilise comme une base pour une personnalisation plus poussée.

## Créer le fichier HTL

Pour créer le fichier HTL pour le composant Pays :

1. Accédez au dossier Pays dans le référentiel crx
2. Créez un fichier nommé pays.html.
3. Ouvrez le fichier /apps/core/fd/components/form/dropdown/v1/dropdown/dropdown.html dans le référentiel crx et copiez son contenu.
4. Collez le contenu copié dans pays.html.
5. Modifiez le code pour qu’il utilise le nouveau modèle sling, tel qu’indiqué dans la capture d’écran.
6. Enregistrez vos modifications.

![sling-model](assets/countriesdropdown.png)

Enfin, synchronisez votre projet avec ces mises à jour afin de vous assurer que les modifications apportées au référentiel CRX sont répercutées dans votre projet AEM.


## Étapes suivantes

[Créer cq-dialog](./dialog.md)
