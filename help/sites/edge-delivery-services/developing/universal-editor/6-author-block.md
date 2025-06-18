---
title: Créer un bloc
description: Créez un bloc Edge Delivery Services avec l’éditeur universel.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '376'
ht-degree: 100%

---

# Créer un bloc

Après avoir envoyé le [fichier JSON du bloc teaser](./5-new-block.md) à la branche `teaser`, le bloc devient modifiable dans l’éditeur universel d’AEM.

La création d’un bloc en développement est importante pour plusieurs raisons :

1. Cela permet de vérifier que la définition et le modèle du bloc sont exacts.
1. Cela permet aux développeurs et développeuses de passer en revue le code HTML sémantique du bloc, qui sert de base au développement.
1. Cela permet de déployer le contenu et le code HTML sémantique dans l’environnement de prévisualisation, ce qui accélère le développement des blocs.

## Ouvrez l’éditeur universel en utilisant le code de la branche `teaser`.

1. Connectez-vous au service de création AEM.
2. Accédez à **Sites** et sélectionnez le site (WKND (éditeur universel)) créé dans le [chapitre précédent](./2-new-aem-site.md).

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. Créez ou modifiez une page pour ajouter le nouveau bloc, en vous assurant que le contexte est disponible pour prendre en charge le développement local. Bien que les pages puissent être créées n’importe où sur le site, il est souvent préférable de créer des pages discrètes pour chaque nouveau corps de travail. Créez une page « dossier » nommée **Branches**. Chaque sous-page est utilisée pour prendre en charge le développement de la branche Git portant le même nom.

   ![AEM Sites - Création de la page Branches](./assets/6-author-block/branches-page-3.png)

4. Sous la page **Branches**, créez une page intitulée **Teaser**, correspondant au nom de la branche de développement, puis cliquez sur **Ouvrir** pour modifier la page.

   ![AEM Sites - Création de la page Teaser](./assets/6-author-block/teaser-page-3.png)

5. Mettez à jour l’éditeur universel pour charger le code à partir de la branche `teaser` en ajoutant `?ref=teaser` à l’URL. Veillez à ajouter le paramètre de requête **AVANT** le symbole `#`.

   ![Éditeur universel - Sélection de la branche de teaser](./assets/6-author-block/select-branch.png)

6. Sélectionnez la première section sous **Principal**, cliquez sur le bouton **Ajouter**, puis choisissez le bloc **Teaser**.

   ![Éditeur universel - Ajout d’un bloc](./assets/6-author-block/add-teaser-2.png)

7. Sur la zone de travail, sélectionnez le teaser nouvellement ajouté et créez les champs sur la droite, ou via la fonctionnalité de modification en ligne.

   ![Éditeur universel - Création de bloc](./assets/6-author-block/author-block.png)

8. Une fois la création terminée, sélectionnez le bouton **Publier** en haut à droite de l’éditeur universel, choisissez Publier dans la **Prévisualisation**, puis publiez les modifications dans l’environnement de prévisualisation. Les modifications sont ensuite publiées dans le domaine `aem.page` du site web.
   ![AEM Sites - Publication ou Prévisualisation](./assets/6-author-block/publish-to-preview.png)

9. Attendez que les modifications soient publiées pour prévisualiser, puis ouvrez la page web via l’[interface de ligne de commande AEM](./3-local-development-environment.md#install-the-aem-cli) à l’adresse [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser).

   ![Site local - Actualisation](./assets/6-author-block/preview.png)

Désormais, le contenu du bloc de teaser créé et le code HTML sémantique sont disponibles sur le site web de prévisualisation, prêts pour le développement à l’aide de l’interface de ligne de commande AEM dans l’environnement de développement local.
