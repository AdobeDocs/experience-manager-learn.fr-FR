---
title: Configuration de DataSource avec Salesforce dans AEM Forms 6.3 et 6.4
seo-title: Configuration de DataSource avec Salesforce dans AEM Forms 6.3 et 6.4
description: Intégration d’AEM Forms à Salesforce à l’aide d’un modèle de données de formulaire
seo-description: Intégration d’AEM Forms à Salesforce à l’aide d’un modèle de données de formulaire
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: Forms adaptatif, modèle de données de formulaire
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 0%

---


# Configuration de DataSource avec Salesforce dans AEM Forms 6.3 et 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Prérequis {#prerequisites}

Dans cet article, nous allons passer en revue le processus de création de sources de données avec Salesforce.

Conditions préalables pour ce tutoriel :

* Faites défiler la page jusqu’au bas de cette page, téléchargez le fichier swagger et enregistrez-le sur votre disque dur.
* AEM Forms avec SSL activé

   * [Documentation officielle pour l’activation de SSL sur AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentation officielle pour l’activation de SSL sur AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Vous devrez disposer d’un compte Salesforce.
* Vous devez créer une application connectée. Le formulaire de documentation officiel Salesforce pour la création de l’application est [ici](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Fournissez les portées OAuth appropriées pour l’application (j’ai sélectionné toutes les portées OAuth disponibles à des fins de test).
* Indiquez l’URL de rappel. Dans mon cas, l’URL de rappel était

   * Si vous utilisez **AEM Forms 6.3**, l’URL de rappel est https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. Dans cette URL, le nom de mon modèle de données de formulaire est .

   * Si vous utilisez** AEM Forms 6.4**, l’URL de rappel sera [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

Dans cet exemple gbedekar -w7-1:6443 est le nom de mon serveur et le port sur lequel AEM s’exécute.

Une fois que vous avez créé l’application connectée, notez la **clé client et la clé secrète**. Vous en aurez besoin lors de la création de la source de données dans AEM Forms.

Maintenant que vous avez créé votre application connectée, vous devez créer un fichier swagger pour les opérations que vous devez effectuer dans Salesforce. Un exemple de fichier swagger est inclus dans les ressources téléchargeables. Ce fichier swagger vous permet de créer un objet &quot;Lead&quot; lors de l’envoi du formulaire adaptatif. Veuillez explorer ce fichier swagger.

L’étape suivante consiste à créer une source de données dans AEM Forms. Suivez les étapes suivantes en fonction de votre version d’AEM Forms.

## AEM Forms 6.3 {#aem-forms}

* Connectez-vous à AEM Forms à l’aide du protocole https.
* Accédez aux services cloud en saisissant https://&lt;nom_serveur>:&lt;port_serveur> /etc/cloudservices.html, par exemple, https://gbedekar-w7-1:6443/etc/cloudservices.html
* Faites défiler l’écran jusqu’à &quot;Modèle de données de formulaire&quot;.
* Cliquez sur &quot;Afficher les configurations&quot;.
* Cliquez sur &quot;+&quot; pour ajouter une nouvelle configuration.
* Sélectionnez &quot;Reest Full Service&quot;. Attribuez un titre et un nom significatifs à la configuration. Exemple :

   * Nom : CreateLeadInSalesForce
   * Titre : CreateLeadInSalesForce

* Cliquez sur &quot;Créer&quot;.

**Dans l’écran suivant **

* Sélectionnez &quot;Fichier&quot; comme option pour votre fichier source de swagger. Accédez au fichier que vous avez téléchargé précédemment.
* Sélectionnez le type d’authentification OAuth2.0.
* Fournir les valeurs ClientID et Client Secret
* L’URL OAuth est - **https://login.salesforce.com/services/oauth2/authorize**
* Actualiser L’Url Du Jeton - **https://na5.salesforce.com/services/oauth2/token**
* **Accéder À L’Url Du Jeu - https://na5.salesforce.com/services/oauth2/token**
* Portée de l’autorisation : api **   identifiant complet de chatter_api   openid   refresh_token visualforce web**
* Gestionnaire d’authentification : Opérateur d’autorisation
* Cliquez sur &quot;Se connecter à OAUTH&quot;. Si tout se passe bien, aucune erreur ne devrait s’afficher.

Une fois que vous avez créé votre modèle de données de formulaire à l’aide de Salesforce, vous pouvez créer une intégration de données de formulaire à l’aide de la source de données que vous venez de créer. La documentation officielle de création de l’intégration des données de formulaire est [ici](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Veillez à configurer le modèle de données de formulaire de manière à inclure le service de POST afin de créer un objet Lead dans SFDC.

Vous devrez également configurer le service de lecture et d’écriture pour l’objet Lead. Reportez-vous aux captures d’écran en bas de cette page.

Après avoir créé le modèle de données de formulaire, vous pouvez créer un Forms adaptatif basé sur ce modèle et utiliser les méthodes d’envoi du modèle de données de formulaire pour créer le prospect dans SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Création d’une source de données

   * [Accès aux sources de données](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Cliquez sur le bouton &quot;Créer&quot;
   * Fournir des valeurs significatives

      * Nom : CreateLeadInSalesForce
      * Titre : CreateLeadInSalesForce
      * Type de service : Service RESTful
   * Cliquez sur Suivant
   * Source Swagger : Fichier
   * Recherchez et sélectionnez le fichier swagger que vous avez téléchargé à l’étape précédente.
   * Type d’authentification : OAuth 2.0. Spécifiez les valeurs suivantes :
   * Fournir les valeurs ClientID et Client Secret
   * L’URL OAuth est - **https://login.salesforce.com/services/oauth2/authorize**
   * Actualiser L’Url Du Jeton - **https://na5.salesforce.com/services/oauth2/token**
   * Access Token Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Portée de l’autorisation : ** api chatter_api id full openid refresh_token visualforce web**
   * Gestionnaire d’authentification : Opérateur d’autorisation
   * Cliquez sur le bouton &quot;Se connecter à OAuth&quot;. Si des erreurs s’affichent, veuillez passer en revue les étapes précédentes afin de vous assurer que toutes les informations ont été saisies avec précision.


Une fois que vous avez créé votre source de données à l’aide de SalesForce, vous pouvez créer une intégration de données de formulaire à l’aide de la source de données que vous venez de créer. Le lien de documentation pour cela est [ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Veillez à configurer le modèle de données de formulaire de manière à inclure le service de POST afin de créer un objet Lead dans SFDC.

Vous devrez également configurer le service de lecture et d’écriture pour l’objet Lead. Reportez-vous aux captures d’écran en bas de cette page.

Après avoir créé le modèle de données de formulaire, vous pouvez créer un Forms adaptatif basé sur ce modèle et utiliser les méthodes d’envoi du modèle de données de formulaire pour créer le prospect dans SFDC.

>[!NOTE]
>
>Assurez-vous que l’URL du fichier swagger correspond à votre région. Par exemple, l’URL du fichier d’exemple de sélecteur est &quot;na46.salesforce.com&quot;, car le compte a été créé en Amérique du Nord. La méthode la plus simple consiste à vous connecter à votre compte Salesforce et à vérifier l’URL .

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
