---
title: Intégration de balises dans Adobe Experience Platform
description: Découvrez comment intégrer AEM as a Cloud Service à des balises dans Adobe Experience Platform. L’intégration vous permet de déployer Adobe Web SDK et d’injecter du JavaScript personnalisé pour la collecte de données et la personnalisation dans vos pages AEM.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 3%

---


# Intégration de balises dans Adobe Experience Platform

Découvrez comment intégrer AEM as a Cloud Service (AEMCS) aux balises dans Adobe Experience Platform. L’intégration Balises (ou Launch) vous permet de déployer Adobe Web SDK et d’injecter du JavaScript personnalisé pour la collecte de données et la personnalisation dans vos pages AEM.

L’intégration permet à votre équipe de marketing ou de développement de gérer et de déployer JavaScript à des fins de personnalisation et de collecte de données, sans avoir à redéployer le code AEM.

## Étapes de haut niveau

Le processus d’intégration comprend quatre étapes principales qui établissent la connexion entre AEM et les balises :

1. **Créer, configurer et publier une propriété Tags dans Adobe Experience Platform**
2. **Vérification d’une configuration Adobe IMS pour les balises dans AEM**
3. **Création d’une configuration Balises dans AEM**
4. **Application de la configuration Balises à vos pages AEM**

## Création, configuration et publication d’une propriété Tags dans Adobe Experience Platform

Commencez par créer une propriété Tags dans Adobe Experience Platform. Cette propriété vous aide à gérer le déploiement d’Adobe Web SDK et de tout JavaScript personnalisé requis pour la personnalisation et la collecte de données.

1. Accédez à [Adobe Experience Platform](https://experience.adobe.com/platform), connectez-vous avec votre Adobe ID, puis accédez à **Balises** à partir du menu de gauche.\
   ![Balises Adobe Experience Platform](../assets/setup/aep-tags.png)

2. Cliquez sur **Nouvelle propriété** pour créer une propriété Tags.\
   ![Créer une propriété Tags](../assets/setup/aep-create-tags-property.png)

3. Dans la boîte de dialogue **Créer une propriété**, saisissez ce qui suit :
   - **Nom de la propriété** : nom de la propriété Tags
   - **Type De Propriété** : Sélectionnez **Web**
   - **Domain** : domaine dans lequel vous déployez la propriété (par exemple, `.adobeaemcloud.com`)

   Cliquez sur **Enregistrer**.

   ![Propriété Adobe Tags](../assets/setup/adobe-tags-property.png)

4. Ouvrez la nouvelle propriété. L’extension **Core** doit déjà être incluse. Plus tard, vous allez ajouter l’extension **Web SDK** lors de la configuration du cas d’utilisation Expérimentation, car elle nécessite une configuration supplémentaire telle que l’**identifiant de flux de données**.\
   ![Extension principale Adobe Tags](../assets/setup/adobe-tags-core-extension.png)

5. Publiez la propriété Tags en accédant à **Flux de publication** et en cliquant sur **Ajouter une bibliothèque** pour créer une bibliothèque de déploiement.
   ![Flux de publication des balises Adobe](../assets/setup/adobe-tags-publishing-flow.png)

6. Dans la boîte de dialogue **Créer une bibliothèque**, fournissez les éléments suivants :
   - **Nom** : nom de votre bibliothèque
   - **Environnement** : sélectionnez **Développement**.
   - **Modifications de ressource** : sélectionnez **Ajouter toutes les ressources modifiées**

   Cliquez sur **Enregistrer et créer dans le développement**.

   ![Bibliothèque de création de balises Adobe](../assets/setup/adobe-tags-create-library.png)

7. Pour publier la bibliothèque en production, cliquez sur **Approuver et publier en production**. Une fois la publication terminée, la propriété est prête à être utilisée dans AEM.\
   ![Approbation et publication des balises Adobe](../assets/setup/adobe-tags-approve-publish.png)

## Vérification d’une configuration Adobe IMS pour les balises dans AEM

Lorsqu’un environnement AEMCS est configuré, il inclut automatiquement une configuration Adobe IMS pour les balises, ainsi qu’un projet Adobe Developer Console correspondant. Cette configuration garantit la sécurité de la communication de l’API entre AEM et les balises.

1. Dans AEM, accédez à **Outils** > **Sécurité** > **Configurations d’Adobe IMS**.\
   ![Configurations d’Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Recherchez la configuration **Adobe Launch**. Le cas échéant, sélectionnez-la et cliquez sur **Vérifier l’intégrité** pour vérifier la connexion. Vous devriez voir une réponse de succès.\
   ![Vérification de l’intégrité de la configuration Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Création d’une configuration de balises dans AEM

Créez une configuration Balises dans AEM pour spécifier la propriété et les paramètres nécessaires pour vos pages de site.

1. Dans AEM, accédez à **Outils** > **Services cloud** > **Configurations d’Adobe Launch**.\
   ![Configurations Adobe Launch](../assets/setup/aem-launch-configurations.png)

2. Sélectionnez le dossier racine de votre site (par exemple, WKND Site) et cliquez sur **Créer**.\
   ![Créer une configuration Adobe Launch](../assets/setup/aem-create-launch-configuration.png)

3. Dans la boîte de dialogue, saisissez ce qui suit :
   - **Titre** : par exemple, « Adobe Tags »
   - **Configuration IMS** : sélectionnez la configuration IMS **Adobe Launch** validée
   - **Société** : sélectionnez la société liée à votre propriété Balises
   - **Property** : sélectionnez la propriété Tags créée précédemment

   Cliquez sur **Suivant**.

   ![Détails de la configuration d’Adobe Launch](../assets/setup/aem-launch-configuration-details.png)

4. À des fins de démonstration, conservez les valeurs par défaut pour les environnements **Évaluation** et **Production**. Cliquez sur **Créer**.\
   ![Détails de la configuration d’Adobe Launch](../assets/setup/aem-launch-configuration-create.png)

5. Sélectionnez la configuration que vous venez de créer et cliquez sur **Publier** pour la rendre disponible pour les pages de votre site.\
   ![Publication de la configuration Adobe Launch](../assets/setup/aem-launch-configuration-publish.png)

## Application de la configuration de balises à votre site AEM

Appliquez la configuration Balises pour injecter la logique de SDK web et de personnalisation dans les pages de votre site.

1. Dans AEM, accédez à **Sites**, sélectionnez le dossier de votre site racine (par exemple, WKND Site), puis cliquez sur **Propriétés**.\
   ![Propriétés du site AEM](../assets/setup/aem-site-properties.png)

2. Dans la boîte de dialogue **Propriétés du site**, ouvrez l’onglet **Avancé**. Sous **Configurations**, assurez-vous que `/conf/wknd` est sélectionné pour **Configuration du cloud**.\
   ![Propriétés avancées du site AEM](../assets/setup/aem-site-advanced-properties.png)

## Vérification de l’intégration

Pour vérifier que la configuration Balises fonctionne correctement, vous pouvez :

1. Vérifiez la source d’affichage d’une page de publication AEM ou inspectez-la à l’aide des outils de développement du navigateur
2. Utiliser [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) pour valider l’injection Web SDK et JavaScript

![Adobe Experience Platform Debugger](../assets/setup/aep-debugger.png)

## Ressources supplémentaires

- [Vue d’ensemble d’Adobe Experience Platform Debugger](https://experienceleague.adobe.com/fr/docs/experience-platform/debugger/home)
- [Vue d’ensemble de Balises](https://experienceleague.adobe.com/fr/docs/experience-platform/tags/home)
