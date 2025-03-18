---
title: Créer un bloc
description: Créez un bloc Edge Delivery Services avec l’éditeur universel.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 500
exl-id: ca356d38-262d-4c30-83a0-01c8a1381ee6
source-git-commit: 77beb9f543bc6dc8c1ab4993c969375ce3e238e8
workflow-type: tm+mt
source-wordcount: '376'
ht-degree: 0%

---

# Créer un bloc

Après avoir envoyé le fichier JSON du bloc [teaser](./5-new-block.md) à la branche `teaser`, le bloc devient modifiable dans l’éditeur universel d’AEM.

La création d’un bloc en développement est importante pour plusieurs raisons :

1. Il vérifie que la définition et le modèle du bloc sont exacts.
1. Il permet aux développeurs de passer en revue l’HTML sémantique du bloc, qui sert de base au développement.
1. Il permet de déployer le contenu et l’HTML sémantique dans l’environnement de prévisualisation, ce qui accélère le développement des blocs.

## Ouvrez l’éditeur universel en utilisant le code de la branche `teaser` .

1. Connectez-vous au service de création d’AEM.
2. Accédez à **Sites** et sélectionnez le site (WKND (éditeur universel)) créé dans le [chapitre précédent](./2-new-aem-site.md).

   ![AEM Sites](./assets/6-author-block/open-new-site.png)

3. Créez ou modifiez une page pour ajouter le nouveau bloc, en vous assurant que le contexte est disponible pour prendre en charge le développement local. Bien que les pages puissent être créées n’importe où sur le site, il est souvent préférable de créer des pages discrètes pour chaque nouveau corps de travail. Créez une page « dossier » nommée **Branches**. Chaque sous-page est utilisée pour prendre en charge le développement de la branche Git portant le même nom.

   ![AEM Sites - Page Créer des branches](./assets/6-author-block/branches-page-3.png)

4. Sous la page **Branches**, créez une page intitulée **Teaser**, correspondant au nom de la branche de développement, puis cliquez sur **Ouvrir** pour modifier la page.

   ![AEM Sites - Créer une page de teaser](./assets/6-author-block/teaser-page-3.png)

5. Mettez à jour l’éditeur universel pour charger le code à partir de la branche `teaser` en ajoutant `?ref=teaser` à l’URL. Veillez à ajouter le paramètre de requête **BEFORE** au symbole `#`.

   ![Éditeur universel - Sélectionnez la branche de teaser](./assets/6-author-block/select-branch.png)

6. Sélectionnez la première section sous **Principal**, cliquez sur le bouton **ajouter**, puis choisissez le bloc **Teaser**.

   ![Éditeur universel - Ajouter un bloc](./assets/6-author-block/add-teaser-2.png)

7. Sur la zone de travail, sélectionnez le teaser nouvellement ajouté et créez les champs sur la droite, ou via la fonctionnalité de modification en ligne.

   ![Éditeur universel - Bloc Auteur](./assets/6-author-block/author-block.png)

8. Une fois la création terminée, cliquez sur le bouton **Publier** en haut à droite de l’éditeur universel, choisissez Publier dans **Aperçu**, puis publiez les modifications dans l’environnement de prévisualisation. Les modifications sont ensuite publiées dans le domaine `aem.page` du site web.
   ![AEM Sites - Publier ou prévisualiser](./assets/6-author-block/publish-to-preview.png)

9. Attendez que les modifications soient publiées pour prévisualiser, puis ouvrez la page web via l’[AEM CLI](./3-local-development-environment.md#install-the-aem-cli) à l’adresse [http://localhost:3000/branches/teaser](http://localhost:3000/branches/teaser).

   ![Site local - Actualiser](./assets/6-author-block/preview.png)

Désormais, le contenu du bloc de teaser créé et l’HTML sémantique sont disponibles sur le site web de prévisualisation, prêts pour le développement à l’aide de l’interface de ligne de commande AEM dans l’environnement de développement local.
