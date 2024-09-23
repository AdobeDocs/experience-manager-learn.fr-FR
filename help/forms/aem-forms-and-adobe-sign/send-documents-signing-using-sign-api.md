---
title: Utilisation de l’API Adobe Sign dans AEM Forms
description: Envoi de documents pour signature à l’aide des méthodes d’assistance Adobe Sign
feature: Adaptive Forms
jira: KT-15474
topic: Development
role: Developer
level: Beginner
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 3400b728-58ca-44c3-a882-e3170755f845
duration: 74
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: ht
source-wordcount: '298'
ht-degree: 100%

---

# Utilisation des méthodes d’assistance Adobe Sign

Dans certains cas d’utilisation, vous pouvez avoir besoin d’envoyer un document pour signatures sans utiliser de workflow AEM. Dans ces cas, il sera très pratique d’utiliser les méthodes wrapper exposées par l’exemple de lot fourni dans cet article.

## Déploiement de l’exemple de lot OSGi

[Déployez le lot OSGi](assets/AdobeSignHelperMethods.core-1.0.0-SNAPSHOT.jar) via la console web OSGi AEM. Spécifiez la clé d’intégration API et l’utilisateur ou l’utilisatrice de l’API à l’aide de la configuration OSGi comme illustré ci-dessous, via le gestionnaire de configuration de la console web OSGi AEM.

 Notez que le lot OSGi `AdobeSignHelperMethods` n’est pas reconnu comme un code de produit Adobe Experience Manager (AEM) et, en tant que tel, il n’est pas pris en charge par l’assistance d’Adobe.
![sign-configuration](assets/sign-configuration.png)


## Documentation de l’API

Les éléments suivants sont disponibles via le service OSGi `AcrobatSignHelperMethods` fourni dans le lot OSGi.

### getTransientDocumentID

`String getTransientDocumentID(Document documentForSigning) throws IOException`


Document utilisé pour créer un contrat ou un formulaire web. Le document est d’abord chargé vers Acrobat Sign par l’expéditeur ou l’expéditrice. On le qualifie alors de _transitoire_, car il est disponible uniquement pendant 7 jours après le chargement. Cette méthode accepte `com.adobe.aemfd.docmanager.Document` et renvoie un ID de document transitoire.

### getAgreementID

`String getAgreementId(String transientDocumentID, String email) throws ClientProtocolException, IOException`

Envoyez le document à signer à l’aide de l’ID de document transitoire à la personne identifiée par le paramètre d’e-mail.

### getWidgetID

`String getWidgetID(String transientDocumentID)`

Un widget est comme un modèle réutilisable qui peut être présenté aux utilisateurs et aux utilisatrices plusieurs fois et signé à plusieurs reprises. Utilisez cette méthode pour obtenir un ID de widget à l’aide de l’ID de document transitoire.

### getWidgetURL

`String getWidgetURL(String widgetId) throws ClientProtocolException, IOException`

Obtenez une URL de widget pour un ID de widget spécifique. Cette URL de widget peut ensuite être présentée aux utilisateurs et aux utilisatrices pour la signature du document.

## Utilisation de l’API

`AcrobatSignHelperMethods` est un service OSGi. Il doit donc être annoté à l’aide de l’annotation @Reference dans votre code Java.

```java
...
// Import the AcrobatSignHelperMethods from the provided bundle
import com.acrobatsign.core.AcrobatSignHelperMethods;
...

@Component(service = { Example.class })
public class ExampleImpl implements Example {

 // Gain a reference to the provided AcrobatSignHelperMethods OSGi service
 @Reference
 com.acrobatsign.core.AcrobatSignHelperMethods acrobatSignHelperMethods;

 function void example() { 
    ...
    // Use the AcrobatSignHelperMethods API methods in your code
    String transientDocumentId = acrobatSignHelperMethods.getTransientDocumentID(documentForSigning);

    String agreementId = acrobatSignHelperMethods.getAgreementId(transientDocumentID, "johndoe@example.com");
    ...
 }
}
```
