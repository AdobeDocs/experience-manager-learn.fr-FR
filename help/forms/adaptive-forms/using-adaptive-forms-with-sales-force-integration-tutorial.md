---
title: Configuration de DataSource avec Salesforce dans AEM Forms 6.3 et 6.4
seo-title: Configuration de DataSource avec Salesforce dans AEM Forms 6.3 et 6.4
description: Intégration d’AEM Forms à Salesforce à l’aide d’un modèle de données de formulaire
seo-description: Intégration d’AEM Forms à Salesforce à l’aide d’un modèle de données de formulaire
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
translation-type: tm+mt
source-git-commit: cce9f5d1dae05a36b942f6b07a46c65f82eac43c
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 0%

---


# Configuration de DataSource avec Salesforce dans AEM Forms 6.3 et 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Conditions préalables {#prerequisites}

Cet article décrit le processus de création de sources de données avec Salesforce.

Conditions préalables pour ce didacticiel :

* Faites défiler cette page jusqu&#39;au bas de cette page et téléchargez le fichier swagger et enregistrez-le sur votre disque dur.
* AEM Forms avec SSL activé

   * [Documentation officielle pour l’activation de SSL sur AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentation officielle pour l’activation de SSL sur AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Vous devez disposer d&#39;un compte Salesforce.
* Vous devez créer une application connectée. Le formulaire de documentation officielle de Salesforce pour la création de l&#39;application est [ici](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Fournir des étendues OAuth appropriées pour l’application (j’ai sélectionné toutes les étendues OAuth disponibles pour les tests)
* Fournissez l’URL de rappel. Dans mon cas, l’URL de rappel était

   * Si vous utilisez **AEM Forms 6.3**, l’URL de rappel est https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. Dans cette en-tête d’URL créée, il y a le nom de mon modèle de données de formulaire.

   * Si vous utilisez** AEM Forms 6.4**, l’URL de rappel est [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

Dans cet exemple gbedekar -w7-1:6443 est le nom de mon serveur et le port sur lequel AEM s&#39;exécute.

Une fois que vous avez créé l’application connectée, notez la **clé de Consumer key et la clé secrète**. Vous en aurez besoin lors de la création de la source de données en AEM Forms.

Maintenant que vous avez créé votre application connectée, vous devez créer un fichier swagger pour les opérations que vous devez effectuer dans Salesforce. Un exemple de fichier swagger est inclus dans les ressources téléchargeables. Ce fichier swagger vous permet de créer un objet &quot;Lead&quot; lors de l’envoi d’un formulaire adaptatif. Explorez ce fichier swagger.

L’étape suivante consiste à créer une source de données en AEM Forms. Suivez les étapes ci-après en fonction de votre version AEM Forms.

## AEM Forms 6.3 {#aem-forms}

* Connexion à AEM Forms à l’aide du protocole https
* Accédez aux services cloud en saisissant https://&lt;nom_serveur>:&lt;port_serveur> /etc/cloudservices.html, par exemple, https://gbedekar-w7-1:6443/etc/cloudservices.html.
* Faites défiler l’écran jusqu’à &quot;Modèle de données de formulaire&quot;.
* Cliquez sur &quot;Afficher les configurations&quot;.
* Cliquez sur &quot;+&quot; pour ajouter une nouvelle configuration.
* Sélectionnez &quot;Rest Full Service&quot;. Fournissez un titre et un nom significatifs à la configuration. Exemple :

   * Nom : CreateLeadInSalesForce
   * Titre : CreateLeadInSalesForce

* Cliquez sur &quot;Créer&quot;.

**Dans l’écran suivant **

* Sélectionnez &quot;Fichier&quot; comme option pour votre fichier source swagger. Accédez au fichier que vous avez téléchargé précédemment
* Sélectionnez le type d’authentification OAuth2.0.
* Fournissez les valeurs ClientID et Client Secret.
* L’URL OAuth est - **https://login.salesforce.com/services/oauth2/authorize**
* Actualiser l’URL du jeton - **https://na5.salesforce.com/services/oauth2/token**
* **Url Du Jeu D&#39;Accès : https://na5.salesforce.com/services/oauth2/token**
* Portée de l&#39;autorisation : ** api   identifiant complet de chatter_api   openid   refresh_token visualforce web**
* Gestionnaire d&#39;authentification : Responsable d&#39;autorisation
* Cliquez sur &quot;Se connecter à OAUTH&quot;.Si tout se passe bien, vous ne devriez pas voir d&#39;erreurs.

Une fois que vous avez créé votre modèle de données de formulaire à l’aide de Salesforce, vous pouvez créer l’intégration des données de formulaire à l’aide de la source de données que vous venez de créer. La documentation officielle relative à la création de l’intégration des données de formulaire est [ici](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Assurez-vous de configurer le modèle de données de formulaire pour inclure le service de POST afin de créer un objet Lead dans SFDC.

Vous devrez également configurer le service de lecture et d&#39;écriture pour l&#39;objet Lead. Reportez-vous aux captures d&#39;écran en bas de cette page.

Après avoir créé le modèle de données de formulaire, vous pouvez créer un Forms adaptatif basé sur ce modèle et utiliser les méthodes d’envoi du modèle de données de formulaire pour créer du pistes dans SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Créer une source de données

   * [Accédez à Sources de données.](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Cliquez sur le bouton Créer
   * Fournir des valeurs significatives

      * Nom : CreateLeadInSalesForce
      * Titre : CreateLeadInSalesForce
      * Type de service : Service RESTful
   * Cliquez sur Suivant
   * Source Swagger : Fichier
   * Recherchez et sélectionnez le fichier swagger que vous avez téléchargé à l’étape précédente.
   * Type d&#39;authentification : OAuth 2.0. Spécifiez les valeurs suivantes :
   * Fournissez les valeurs ClientID et Client Secret.
   * L’URL OAuth est - **https://login.salesforce.com/services/oauth2/authorize**
   * Actualiser l’URL du jeton - **https://na5.salesforce.com/services/oauth2/token**
   * jeton d&#39;accès Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Portée de l&#39;autorisation : ** api chatter_api id complet openid refresh_token visualforce web**
   * Gestionnaire d&#39;authentification : Responsable d&#39;autorisation
   * Cliquez sur le bouton &quot;Se connecter à OAuth&quot;. Si vous constatez des erreurs, veuillez passer en revue les étapes précédentes pour vous assurer que toutes les informations ont été saisies avec précision.


Une fois que vous avez créé votre source de données à l’aide de SalesForce, vous pouvez créer l’intégration des données de formulaire à l’aide de la source de données que vous venez de créer. Le lien de la documentation pour cela est [ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Assurez-vous de configurer le modèle de données de formulaire pour inclure le service de POST afin de créer un objet Lead dans SFDC.

Vous devrez également configurer le service de lecture et d&#39;écriture pour l&#39;objet Lead. Reportez-vous aux captures d&#39;écran en bas de cette page.

Après avoir créé le modèle de données de formulaire, vous pouvez créer un Forms adaptatif basé sur ce modèle et utiliser les méthodes d’envoi du modèle de données de formulaire pour créer du pistes dans SFDC.

>[!NOTE]
>
>Assurez-vous que l’URL du fichier swagger correspond à votre région. Par exemple, l’URL du fichier d’échange d’exemple est &quot;na46.salesforce.com&quot;, car le compte a été créé en Amérique du Nord. Le moyen le plus simple est de se connecter à votre compte Salesforce et de vérifier l&#39;URL .

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
