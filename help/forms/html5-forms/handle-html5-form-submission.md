---
title: Gérer l’envoi de formulaire HTML5
description: Créer un gestionnaire d’envoi de formulaire HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 6%

---


# Gérer l’envoi de formulaire HTML5

Les formulaires HTML5 peuvent être envoyés à la servlet hébergée dans AEM. Les données envoyées sont accessibles dans la servlet en tant que flux d’entrée. Pour envoyer votre formulaire HTML5, vous devez ajouter le bouton Envoyer via HTTP à votre modèle de formulaire à l’aide de AEM Forms Designer.

## Créer votre gestionnaire d’envoi

Une servlet simple peut être créée pour gérer l’envoi du formulaire HTML5. Les données envoyées peuvent ensuite être extraites à l’aide du code suivant. Ce [servlet](assets/html5-submit-handler.zip) est mis à votre disposition dans le cadre de ce didacticiel. Installez [servlet](assets/html5-submit-handler.zip) à l&#39;aide de [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).

Le code de la ligne 9 peut être utilisé pour appeler le processus J2EE. Assurez-vous d&#39;avoir configuré [la configuration du SDK client du LiveCycle d&#39;Adobe](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) si vous avez l&#39;intention d&#39;utiliser le code pour appeler le processus J2EE.

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
/*
        * java.util.Map params = new java.util.HashMap();
        * params.put("in",stringBuffer.toString());
        * com.adobe.livecycle.dsc.clientsdk.ServiceClientFactoryProvider scfp =
        * sling.getService(com.adobe.livecycle.dsc.clientsdk.
        * ServiceClientFactoryProvider.class);
        * com.adobe.idp.dsc.clientsdk.ServiceClientFactory serviceClientFactory =
        * scfp.getDefaultServiceClientFactory(); com.adobe.idp.dsc.InvocationRequest ir
        * = serviceClientFactory.createInvocationRequest("Test1/NewProcess1", "invoke",
        * params, true);
        * ir.setProperty(com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE,com.adobe.livecycle.dsc.clientsdk.InvocationProperties.
        * INVOKER_TYPE_SYSTEM); com.adobe.idp.dsc.InvocationResponse response1 =
        * serviceClientFactory.getServiceClient().invoke(ir);
        * System.out.println("The response is "+response1.getInvocationId());
        */
```


## Configuration de l’URL d’envoi du formulaire HTML5

![submit-url](assets/submit-url.PNG)

* Appuyez sur xdp et cliquez sur _Propriétés_->_Avancé_.
* copier http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html et coller ceci dans le champ de texte Envoyer l’URL
* Cliquez sur le bouton _SaveAndClose_.

### Entrée d&#39;Ajoute dans les chemins d&#39;exclusion

* Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
* Rechercher _Adobe Granite CSRF Filter_
* Ajoutez l’entrée suivante dans la section Chemins exclus.
* _/content/AemFormsSamples/handlehml5formsubmission_
* Enregistrez vos modifications

### Test du formulaire

* Appuyez sur le modèle xdp.
* Cliquez sur _Prévisualisation_->Prévisualisation au format HTML.
* Saisissez des données dans le formulaire et cliquez sur Envoyer.
* Vous devriez voir les données envoyées écrites dans le fichier stdout.log de votre serveur.

### Lecture supplémentaire

Cet [article](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) sur la génération de PDF à partir de l’envoi de formulaire HTML5 est également recommandé.




