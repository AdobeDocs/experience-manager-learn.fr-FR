---
title: Créer un site AEM
description: Créez un site dans AEM Sites pour Edge Delivery Services, modifiable à l’aide de l’éditeur universel.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: d1ebcaf4-cea6-4820-8b05-3a0c71749d33
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '302'
ht-degree: 100%

---

# Créer un site AEM

Le site AEM est l’endroit à partir duquel le contenu du site web est modifié, géré et publié. Pour créer un site AEM diffusé via Edge Delivery Services et créé à l’aide de l’éditeur universel, utilisez le [modèle de site de création Edge Delivery Services avec AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases) afin de créer un site sur l’instance de création AEM.

Le site AEM est l’endroit où le contenu du site web est stocké et créé. L’expérience finale est une combinaison du contenu du site AEM avec le [code du site web](./1-new-code-project.md).

![Nouveau site AEM pour Edge Delivery Services et l’éditeur universel](./assets/2-new-aem-site/new-site.png)

Suivez les [étapes détaillées décrites dans la documentation](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-aem-site) pour créer un site AEM.  Vous trouverez ci-dessous un résumé des étapes, y compris les valeurs utilisées dans ce tutoriel.
1. **Créez un site** dans l’instance de création AEM. Ce tutoriel utilise le nommage de site suivant :
   * Titre du site : `WKND (Universal Editor)`
   * Nom du site : `aem-wknd-eds-ue`

      * La valeur du nom du site doit correspondre au nom du chemin du site [ajouté à `paths.json`](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/path-mapping).

2. **Importez le dernier modèle** à partir du [modèle de site de création Edge Delivery Services avec AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk/releases).
3. **Nommez le site** pour qu’il corresponde au nom du référentiel GitHub et définissez l’URL GitHub comme URL du référentiel.

## Publier le nouveau site à prévisualiser

Après avoir créé le site dans l’instance de création AEM, publiez-le dans la prévisualisation Edge Delivery Services en rendant le contenu disponible pour l’[environnement de développement local](./3-local-development-environment.md).

1. Connectez-vous à l’**instance de création AEM** et accédez à **Sites**.
2. Sélectionnez le **nouveau site** (`WKND (Universal Editor)`) et cliquez sur **Gérer les publications**.
3. Choisissez **Prévisualisation** sous **Destinations** et cliquez sur **Suivant**.
4. Sous **Inclure les paramètres enfant**, sélectionnez **Inclure les enfants**, désélectionnez les autres options, puis cliquez sur **OK**.
5. Cliquez sur **Publier** pour publier le contenu du site à prévisualiser.
6. Une fois publiées pour la prévisualisation, les pages sont disponibles dans l’environnement de prévisualisation Edge Delivery Services (les pages n’apparaîtront pas dans le service de prévisualisation AEM).
