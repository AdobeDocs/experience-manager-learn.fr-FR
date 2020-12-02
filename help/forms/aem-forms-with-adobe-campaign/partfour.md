---
title: Création d’un Profil Campaign à l’aide du modèle de données de formulaire
seo-title: Création d’un Profil Campaign à l’aide du modèle de données de formulaire
description: Étapes de création d’un profil Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms
seo-description: Étapes de création d’un profil Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms
uuid: 3216827e-e1a2-4203-8fe3-4e2a82ad180a
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 461c532e-7a07-49f5-90b7-ad0dcde40984
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 3%

---


# Créer un Profil Campaign à l’aide du modèle de données de formulaire {#create-campaign-profile-using-form-data-model}

Étapes de création d’un profil Adobe Campaign Standard à l’aide du modèle de données de formulaire AEM Forms

## Créer une authentification personnalisée {#create-custom-authentication}

Lors de la création d’une source de données à l’aide du fichier swagger, AEM Forms prend en charge les types d’authentification suivants :

* Aucune
* OAuth 2.0
* Authentification de base
* Clé API
* Authentification personnalisée

![campaingfdm](assets/campaignfdm.gif)

Nous devrons utiliser l&#39;authentification personnalisée pour effectuer des appels REST à Adobe Campaign Standard.

Pour utiliser l&#39;authentification personnalisée, nous devrons développer un composant OSGi qui implémente l&#39;interface IAuthentication

La méthode getAuthDetails doit être implémentée. Cette méthode renvoie l&#39;objet AuthenticationDetails. Les en-têtes HTTP requis sont définis pour cet objet AuthenticationDetails afin d’effectuer l’appel d’API REST à Adobe Campaign.

Voici le code utilisé pour créer l’authentification personnalisée. La méthode getAuthDetails fait tout le travail. Nous créons l&#39;objet AuthenticationDetails. Ensuite, nous ajoutons les HttpHeaders appropriés à cet objet et retournons cet objet.

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

La première étape consiste à créer le fichier swagger. Le fichier swagger définit l’API REST qui va être utilisée pour créer un profil en Adobe Campaign Standard. Le fichier swagger définit les paramètres d’entrée et de sortie de l’API REST.

Une source de données est créée à l’aide du fichier swagger. Lors de la création de la source de données, vous pouvez spécifier le type d’authentification. Dans ce cas, nous allons utiliser l&#39;authentification personnalisée pour nous authentifier auprès d&#39;Adobe Campaign. Le code répertorié ci-dessus a été utilisé pour s&#39;authentifier auprès d&#39;Adobe Campaign.

Un exemple de fichier swagger vous est fourni dans le cadre de l’élément lié à cet article.**Veillez à modifier l&#39;hôte et basePath dans le fichier swagger pour qu&#39;ils correspondent à votre instance ACS.**

## Testez la solution {#test-the-solution}

Pour tester la solution, procédez comme suit :
* [Veillez à suivre les étapes décrites ici.](aem-forms-with-campaign-standard-getting-started-tutorial.md)
* [Téléchargez et décompressez ce fichier pour obtenir le fichier swagger.](assets/create-acs-profile-swagger-file.zip)
* Création d’une source de données à l’aide du fichier swagger
Créer un modèle de données de formulaire et le baser sur la source de données créée à l’étape précédente
* Créez un formulaire adaptatif basé sur le modèle de données de formulaire créé à l’étape précédente.
* Faites glisser les éléments suivants de l’onglet Sources de données vers le formulaire adaptatif

   * Courrier électronique
   * Prénom
   * Nom
   * Téléphone mobile

* Configurez l’action d’envoi sur &quot;Envoyer à l’aide du modèle de données de formulaire&quot;.
* Configurez le modèle de données à envoyer de manière appropriée.
* Prévisualiser le formulaire. Renseignez les champs et envoyez.
* Vérifiez que le profil est créé dans Adobe Campaign Standard.
