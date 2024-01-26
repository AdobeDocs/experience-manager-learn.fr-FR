---
title: Créer un profil Campaign à l’aide du modèle de données de formulaire
description: Procédure de création d’un profil Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 59d5ba6d-91c1-48c7-8c87-8e0caf4f2d7e
duration: 126
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 100%

---

# Créer un profil Campaign à l’aide du modèle de données de formulaire {#create-campaign-profile-using-form-data-model}

Procédure de création d’un profil Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms

## Créer une authentification personnalisée {#create-custom-authentication}

Lors de la création d’une source de données avec le fichier Swagger, AEM Forms prend en charge les types d’authentification suivants :

* Aucun
* OAuth 2.0
* Authentification de base
* Clé API
* Authentification personnalisée

![campaingfdm](assets/campaignfdm.gif)

Nous devrons utiliser une authentification personnalisée pour effectuer des appels REST vers Adobe Campaign Standard.

Pour utiliser l’authentification personnalisée, nous devrons développer un composant OSGi qui implémente l’interface d’authentification.

La méthode getAuthDetails doit être implémentée. Cette méthode renvoie l’objet AuthenticationDetails. Cet objet AuthenticationDetails dispose des en-têtes HTTP requis nécessaires pour effectuer l’appel de l’API REST à Adobe Campaign.

Voici le code utilisé pour créer l’authentification personnalisée. La méthode getAuthDetails effectue l’ensemble du travail. Nous créons l’objet AuthenticationDetails. Ensuite, nous ajoutons les HttpHeaders appropriés à cet objet et renvoyons cet objet.

```java
package aemfd.campaign.core;

import java.io.IOException;
import java.security.NoSuchAlgorithmException;
import java.security.spec.InvalidKeySpecException;

import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;

import aemforms.campaign.core.CampaignService;
import formsandcampaign.demo.CampaignConfigurationService;
@Component(service=IAuthentication.class,immediate=true)

public class CampaignAuthentication implements IAuthentication {
 @Reference
 CampaignService campaignService;
  @Reference
     CampaignConfigurationService config;
private Logger log = LoggerFactory.getLogger(CampaignAuthentication.class);
 @Override
 public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException {
 try {
   AuthenticationDetails auth = new AuthenticationDetails();
   auth.addHttpHeader("Cache-Control", "no-cache");
   auth.addHttpHeader("Content-Type", "application/json");
   auth.addHttpHeader("X-Api-Key",config.getApiKey() );
         auth.addHttpHeader("Authorization", "Bearer "+campaignService.getAccessToken());
         log.debug("Returning auth");
         return auth;
   
  } catch (NoSuchAlgorithmException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (InvalidKeySpecException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (IOException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return null;
  
 }

 @Override
 public String getAuthenticationType() {
  // TODO Auto-generated method stub
  return "Campaign Custom Authentication";
 }

}
```

## Créer une source de données {#create-data-source}

La première étape consiste à créer le fichier Swagger. Le fichier Swagger définit l’API REST qui sera utilisée pour créer un profil dans Adobe Campaign Standard. Le fichier Swagger définit les paramètres d’entrée et les paramètres de sortie de l’API REST.

Une source de données est créée à l’aide du fichier Swagger. Lors de la création de la source de données, vous pouvez spécifier le type d’authentification. Dans ce cas, nous allons utiliser l’authentification personnalisée pour effectuer l’authentification auprès d’Adobe Campaign. Le code répertorié ci-dessus a été utilisé pour effectuer l’authentification auprès d’Adobe Campaign.

Un exemple de fichier Swagger vous est fourni dans le cadre des ressources liées à cet article.**Veillez à modifier l’hôte et le basePath dans le fichier Swagger pour qu’ils correspondent à votre instance ACS.**

## Tester la solution {#test-the-solution}

Pour tester la solution, procédez comme suit :
* [Veillez à suivre les étapes décrites ici.](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Téléchargez et décompressez ce fichier pour obtenir le fichier Swagger.](assets/create-acs-profile-swagger-file.zip)
* Créer une source de données à l’aide du fichier Swagger
Créez un modèle de données de formulaire et basez-le sur la source de données créée à l’étape précédente.
* Créez un formulaire adaptatif basé sur le modèle de données de formulaire créé à l’étape précédente.
* Faites glisser les éléments suivants de l’onglet Sources de données vers le formulaire adaptatif.

   * E-mail
   * Prénom
   * Nom
   * Téléphone portable

* Configurez l’action d’envoi sur « Envoyer à l’aide du modèle de données de formulaire ».
* Configurez le modèle de données à envoyer de manière appropriée.
* Prévisualisez le formulaire. Renseignez les champs et envoyez.
* Vérifiez que le profil est créé dans Adobe Campaign Standard.
