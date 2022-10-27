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
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 4%

---

# Gérer l’envoi de formulaire HTML5

Les formulaires HTML5 peuvent être envoyés au servlet hébergé dans AEM. Les données envoyées sont accessibles dans le servlet sous la forme d’un flux d’entrée. Pour envoyer votre formulaire HTML5, vous devez ajouter le &quot;bouton Envoyer via HTTP&quot; à votre modèle de formulaire à l’aide d’AEM Forms Designer.

## Créer votre gestionnaire d’envoi

Il est possible de créer un servlet simple pour gérer l’envoi de HTML5. Les données envoyées peuvent ensuite être extraites à l’aide du code suivant. Ceci [servlet](assets/html5-submit-handler.zip) est mis à votre disposition dans le cadre de ce tutoriel. Installez le [servlet](assets/html5-submit-handler.zip) using [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)

Le code de la ligne 9 peut être utilisé pour appeler le processus J2EE. Vérifiez que vous avez configuré [Configuration du SDK client Adobe LiveCycle](https://helpx.adobe.com/aem-forms/6/submit-form-data-livecycle-process.html) si vous envisagez d’utiliser le code pour appeler le processus J2EE.

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

* Appuyez sur le fichier xdp et cliquez sur _Propriétés_->_Avancé_
* copiez http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html et collez-le dans le champ de texte URL d’envoi .
* Cliquez sur _SaveAndClose_ bouton .

### Ajouter une entrée dans les chemins d’exclusion

* Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
* Rechercher _Adobe du filtre CSRF Granite_
* Ajoutez l’entrée suivante dans la section Chemins exclus .
* _/content/AemFormsSamples/handlehml5formsubmission_
* Enregistrez vos modifications

### Tester le formulaire

* Appuyez sur le modèle xdp.
* Cliquez sur _Aperçu_->Aperçu en tant que HTML
* Saisissez des données dans le formulaire, puis cliquez sur Envoyer.
* Les données envoyées doivent être écrites dans le fichier stdout.log de votre serveur.

### Lecture supplémentaire

Ceci [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html) il est également recommandé de générer le PDF à partir de l’envoi de HTML5.
