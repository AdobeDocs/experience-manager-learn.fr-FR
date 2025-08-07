---
title: Intégrer Adobe Target
description: Découvrez comment intégrer AEM as a Cloud Service à Adobe Target pour gérer et activer du contenu personnalisé (fragments d’expérience) en tant qu’offres.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 4%

---


# Intégrer Adobe Target

Découvrez comment intégrer AEM as a Cloud Service (AEMCS) à Adobe Target pour activer du contenu personnalisé, tel que des fragments d’expérience, en tant qu’offres dans Adobe Target.

L’intégration permet à votre équipe marketing de créer et de gérer du contenu personnalisé de manière centralisée dans AEM. Ce contenu peut ensuite être activé de manière transparente en tant qu’offres dans Adobe Target.

>[!IMPORTANT]
>
>L’étape d’intégration est facultative si votre équipe préfère gérer les offres entièrement dans Adobe Target, sans utiliser AEM comme référentiel de contenu centralisé.

## Étapes de haut niveau

Le processus d’intégration comprend quatre étapes principales pour établir la connexion entre AEM et Adobe Target :

1. **Créer et configurer un projet Adobe Developer Console**
2. **Créer une configuration Adobe IMS pour Target dans AEM**
3. **Créer une configuration Adobe Target héritée dans AEM**
4. **Application de la configuration Adobe Target aux fragments d’expérience**

## Création et configuration d’un projet Adobe Developer Console

Pour permettre à AEM de communiquer en toute sécurité avec Adobe Target, vous devez configurer un projet Adobe Developer Console à l’aide de l’authentification serveur à serveur OAuth. Vous pouvez utiliser un projet existant ou en créer un nouveau.

1. Accédez à [Adobe Developer Console](https://developer.adobe.com/console) et connectez-vous avec votre Adobe ID.

2. Créez un projet ou sélectionnez-en un existant.\
   ![Projet Adobe Developer Console](../assets/setup/adc-project.png)

3. Cliquez sur **Ajouter une API**. Dans la boîte de dialogue **Ajouter une API**, filtrez par **Experience Cloud**, sélectionnez **Adobe Target**, puis cliquez sur **Suivant**.\
   ![Ajouter une API au projet](../assets/setup/adc-add-api.png)

4. Dans la boîte de dialogue **Configurer l’API**, sélectionnez la méthode d’authentification **OAuth serveur à serveur** et cliquez sur **Suivant**.\
   ![Configurer l’API](../assets/setup/adc-configure-api.png)

5. À l’étape **Sélectionner des profils de produit**, sélectionnez le **Workspace par défaut** et cliquez sur **Enregistrer l’API configurée**.\
   ![Sélectionner des profils de produit](../assets/setup/adc-select-product-profiles.png)

6. Dans le volet de navigation de gauche, sélectionnez **OAuth de serveur à serveur** et passez en revue les détails de la configuration. Notez l’ID client et le secret client : vous avez besoin de ces valeurs pour configurer l’intégration IMS dans AEM.
   ![Détails OAuth de serveur à serveur](../assets/setup/adc-oauth-server-to-server.png)

## Création d’une configuration Adobe IMS pour Target dans AEM

Dans AEM, créez une configuration Adobe IMS pour Target à l’aide des informations d’identification du Adobe Developer Console. Cette configuration permet à AEM de s’authentifier à l’aide des API Adobe Target.

1. Dans AEM, accédez à **Outils** > **Sécurité** et sélectionnez **Configurations d’Adobe IMS**.\
   ![Configurations d’Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Cliquez sur **Créer**.\
   ![Créer une configuration Adobe IMS](../assets/setup/aem-create-ims-configuration.png)

3. Sur la page **Configuration du compte technique Adobe IMS**, saisissez ce qui suit :
   - **Solution cloud** : Adobe Target
   - **Titre** : libellé de la configuration, par exemple « Adobe Target »
   - **Serveur d’autorisation** : `https://ims-na1.adobelogin.com`
   - **Identifiant client** : à partir du Adobe Developer Console
   - **Secret client** : à partir du Adobe Developer Console
   - **Portée** : à partir du Adobe Developer Console
   - **ID d’organisation** : à partir du Adobe Developer Console

   Cliquez ensuite sur **Créer**.

   ![Détails de la configuration Adobe IMS](../assets/setup/aem-ims-configuration-details.png)

4. Sélectionnez la configuration et cliquez sur **Vérifier l’intégrité** pour vérifier la connexion. Un message de réussite confirme qu’AEM peut se connecter à Adobe Target.\
   ![Vérification de l’intégrité de la configuration Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Création d’une configuration Adobe Target héritée dans AEM

Pour exporter des fragments d’expérience en tant qu’offres vers Adobe Target, créez une configuration Adobe Target héritée dans AEM.

1. Dans AEM, accédez à **Outils** > **Services cloud** et sélectionnez **Services cloud hérités**.\
   ![ Services cloud hérités ](../assets/setup/aem-legacy-cloud-services.png)

2. Dans la section **Adobe Target**, cliquez sur **Configurer maintenant**.\
   ![Configuration de l’héritage Adobe Target](../assets/setup/aem-configure-adobe-target-legacy.png)

3. Dans la boîte de dialogue **Créer une configuration**, saisissez un nom tel que « Adobe Target hérité » et cliquez sur **Créer**.\
   ![Créer une configuration Adobe Target héritée](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. Sur la page **Configuration héritée d’Adobe Target**, fournissez les informations suivantes :
   - **Authentification** : IMS
   - **Code client** : votre code client Adobe Target (présent dans Adobe Target sous **Administration** > **Implémentation**).
   - **Configuration IMS** : configuration IMS que vous avez créée précédemment

   Cliquez sur **Connexion à Adobe Target** pour valider la connexion.

   ![Configuration héritée d’Adobe Target](../assets/setup/aem-target-legacy-configuration.png)

## Application de la configuration Adobe Target aux fragments d’expérience

Associez la configuration Adobe Target à vos fragments d’expérience afin qu’ils puissent être exportés et utilisés comme offres dans Target.

1. Dans AEM, accédez à **Fragments d’expérience**.\
   ![Fragments d’expérience](../assets/setup/aem-experience-fragments.png)

2. Sélectionnez le dossier racine qui contient vos fragments d’expérience (par exemple, `WKND Site Fragments`) et cliquez sur **Propriétés**.\
   ![ Propriétés des fragments d’expérience ](../assets/setup/aem-experience-fragments-properties.png)

3. Sur la page **Propriétés**, ouvrez l’onglet **Services cloud**. Dans la section **Configurations de Cloud Service**, sélectionnez votre configuration Adobe Target.\
   ![Services cloud de fragments d’expérience](../assets/setup/aem-experience-fragments-cloud-services.png)

4. Dans la section **Adobe Target** qui s’affiche, effectuez les opérations suivantes :
   - **Format D’Exportation Adobe Target** : HTML
   - **Adobe Target Workspace** : sélectionnez l’espace de travail à utiliser (par exemple, « Workspace par défaut »).
   - **Domaines de l’externaliseur** : renseignez les domaines pour lesquels générer des URL externes

   ![Configuration Adobe Target des fragments d’expérience](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Cliquez sur **Enregistrer et fermer** pour appliquer la configuration.

## Vérification de l’intégration

Pour vérifier que l’intégration fonctionne correctement, testez la fonctionnalité d’exportation :

1. Dans AEM, créez un fragment d’expérience ou ouvrez un fragment d’expérience existant. Cliquez sur **Exporter vers Adobe Target** dans la barre d’outils.\
   ![Exporter un fragment d’expérience vers Adobe Target](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. Dans Adobe Target, accédez à la section **Offres** et vérifiez que le fragment d’expérience s’affiche en tant qu’offre.\
   ![Offres Adobe Target](../assets/setup/adobe-target-xf-as-offer.png)

## Ressources supplémentaires

- [Présentation de l’API Target](https://experienceleague.adobe.com/fr/docs/target-dev/developer/api/target-api-overview)
- [Offre Target ](https://experienceleague.adobe.com/fr/docs/target/using/experiences/offers/manage-content)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/)
- [ Fragments d’expérience dans AEM ](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)