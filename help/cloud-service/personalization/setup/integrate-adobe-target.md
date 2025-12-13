---
title: Intégrer Adobe Target
description: Découvrez comment intégrer AEM as a Cloud Service à Adobe Target pour gérer et activer le contenu personnalisé (fragments d’expérience) en tant qu’offres.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
exl-id: 86767e52-47ce-442c-a620-bc9e7ac2eaf3
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 100%

---

# Intégrer Adobe Target

Découvrez comment intégrer AEM as a Cloud Service (AEMCS) à Adobe Target pour activer le contenu personnalisé, tel que des fragments d’expérience, en tant qu’offres dans Adobe Target.

L’intégration permet à votre équipe marketing de créer et de gérer du contenu personnalisé de manière centralisée dans AEM. Ce contenu peut ensuite être activé de manière transparente en tant qu’offres dans Adobe Target.

>[!IMPORTANT]
>
>L’étape d’intégration est facultative si votre équipe préfère gérer intégralement les offres dans Adobe Target, sans utiliser AEM en tant que référentiel de contenu centralisé.

## Étapes avancées

Le processus d’intégration comprend quatre étapes principales pour établir la connexion entre AEM et Adobe Target :

1. **Créer et configurer un projet Adobe Developer Console**
2. **Créer une configuration Adobe IMS pour Target dans AEM**
3. **Créer une configuration d’Adobe Target héritée dans AEM**
4. **Appliquer la configuration d’Adobe Target aux fragments d’expérience**

## Créer et configurer un projet Adobe Developer Console

Pour permettre à AEM de communiquer en toute sécurité avec Adobe Target, vous devez configurer un projet Adobe Developer Console à l’aide de l’authentification de serveur à serveur OAuth. Vous pouvez utiliser un projet existant ou en créer un nouveau.

1. Accédez à l’[Adobe Developer Console](https://developer.adobe.com/console) et connectez-vous avec votre Adobe ID.

2. Créez un projet ou ouvrez-en un existant.\
   ![Projet Adobe Developer Console](../assets/setup/adc-project.png)

3. Cliquez sur **Ajouter une API**. Dans la boîte de dialogue **Ajouter une API**, filtrez par **Experience Cloud**, sélectionnez **Adobe Target**, puis cliquez sur **Suivant**.\
   ![Ajout d’une API au projet](../assets/setup/adc-add-api.png)

4. Dans la boîte de dialogue **Configurer l’API**, sélectionnez la méthode d’authentification **Serveur à serveur OAuth** et cliquez sur **Suivant**.\
   ![Configuration de l’API](../assets/setup/adc-configure-api.png)

5. À l’étape **Sélectionner des profils de produit**, sélectionnez l’**Espace de travail par défaut** et cliquez sur **Enregistrer l’API configurée**.\
   ![Sélection des profils de produit](../assets/setup/adc-select-product-profiles.png)

6. Dans le volet de navigation de gauche, sélectionnez **Serveur à serveur OAuth** et consultez les détails de la configuration. Notez l’ID client ou cliente et le secret client ou cliente : vous avez besoin de ces valeurs pour configurer l’intégration d’IMS dans AEM.
   ![Détails de l’authentification de serveur à serveur OAuth](../assets/setup/adc-oauth-server-to-server.png)

## Créer une configuration d’Adobe IMS pour Target dans AEM

Dans AEM, créez une configuration d’Adobe IMS pour Target en utilisant les informations d’identification de l’Adobe Developer Console. Cette configuration permet à AEM de s’authentifier auprès des API Adobe Target.

1. Dans AEM, accédez à **Outils** > **Sécurité** et sélectionnez **Configurations d’Adobe IMS**.\
   ![Configurations d’Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Cliquez sur **Créer**.\
   ![Création d’une configuration d’Adobe IMS](../assets/setup/aem-create-ims-configuration.png)

3. Sur la page **Configuration du compte technique Adobe IMS**, saisissez ce qui suit :
   - **Solution cloud** : Adobe Target
   - **Titre** : libellé de la configuration, par exemple « Adobe Target »
   - **Serveur d’autorisation** : `https://ims-na1.adobelogin.com`
   - **ID client ou cliente** : de l’Adobe Developer Console
   - **Secret client ou cliente** : de l’Adobe Developer Console
   - **Portée** : de l’Adobe Developer Console
   - **ID d’organisation** : de l’Adobe Developer Console

   Cliquez ensuite sur **Créer**.

   ![Détails de la configuration d’Adobe IMS](../assets/setup/aem-ims-configuration-details.png)

4. Sélectionnez la configuration et cliquez sur **Contrôler l’intégrité** pour vérifier la connexion. Un message de réussite confirme qu’AEM peut se connecter à Adobe Target.\
   ![Contrôle de l’intégrité de la configuration d’Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Créer une configuration d’Adobe Target héritée dans AEM

Pour exporter des fragments d’expérience en tant qu’offres vers Adobe Target, créez une configuration d’Adobe Target héritée dans AEM.

1. Dans AEM, accédez à **Outils** > **Services cloud** et sélectionnez **Services cloud hérités**.\
   ![Services cloud hérités](../assets/setup/aem-legacy-cloud-services.png)

2. Dans la section **Adobe Target**, cliquez sur **Configurer maintenant**.\
   ![Configuration d’Adobe Target héritée](../assets/setup/aem-configure-adobe-target-legacy.png)

3. Dans la boîte de dialogue **Créer une configuration**, saisissez un nom tel que « Adobe Target hérité » et cliquez sur **Créer**.\
   ![Création d’une configuration d’Adobe Target héritée](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. Sur la page **Configuration d’Adobe Target héritée**, fournissez les informations suivantes :
   - **Authentification** : IMS
   - **Code client ou cliente** : votre code client ou cliente Adobe Target (présent dans Adobe Target sous **Administration** > **Implémentation**)
   - **Configuration d’IMS** : configuration d’IMS que vous avez créée précédemment

   Cliquez sur **Se connecter à Adobe Target** pour valider la connexion.

   ![Configuration d’Adobe Target héritée](../assets/setup/aem-target-legacy-configuration.png)

## Appliquer la configuration d’Adobe Target aux fragments d’expérience

Associez la configuration d’Adobe Target à vos fragments d’expérience afin de pouvoir les exporter et les utiliser en tant qu’offres dans Target.

1. Dans AEM, accédez à **Fragments d’expérience**.\
   ![Fragments d’expérience](../assets/setup/aem-experience-fragments.png)

2. Sélectionnez le dossier racine qui contient vos fragments d’expérience (par exemple, `WKND Site Fragments`) et cliquez sur **Propriétés**.\
   ![Propriétés des fragments d’expérience](../assets/setup/aem-experience-fragments-properties.png)

3. Sur la page **Propriétés**, ouvrez l’onglet **Services cloud**. Dans la section **Configurations du service cloud**, sélectionnez votre configuration d’Adobe Target.\
   ![Services cloud des fragments d’expérience](../assets/setup/aem-experience-fragments-cloud-services.png)

4. Dans la section **Adobe Target** qui s’affiche, renseignez les informations suivantes :
   - **Format d’exportation Adobe Target** : HTML
   - **Espace de travail Adobe Target** : sélectionnez l’espace de travail à utiliser (par exemple, « Espace de travail par défaut »)
   - **Domaines de l’externaliseur** : saisissez les domaines pour la génération d’URL externes

   ![Configuration des fragments d’expérience d’Adobe Target](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Cliquez sur **Enregistrer et fermer** pour appliquer la configuration.

## Vérifier l’intégration

Pour vérifier que l’intégration fonctionne correctement, testez la fonctionnalité d’export :

1. Dans AEM, créez un fragment d’expérience ou ouvrez-en un existant. Cliquez sur **Exporter vers Adobe Target** dans la barre d’outils.\
   ![Export d’un fragment d’expérience vers Adobe Target](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. Dans Adobe Target, accédez à la section **Offres** et vérifiez que le fragment d’expérience s’affiche en tant qu’offre.\
   ![Offres Adobe Target](../assets/setup/adobe-target-xf-as-offer.png)

## Ressources supplémentaires

- [Vue d’ensemble de l’API Target](https://experienceleague.adobe.com/fr/docs/target-dev/developer/api/target-api-overview)
- [Offre Target](https://experienceleague.adobe.com/fr/docs/target/using/experiences/offers/manage-content)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/)
- [Fragments d’expérience dans AEM](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)
