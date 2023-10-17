---
title: Gérer l’envoi du formulaire HTML5
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
workflow-type: ht
source-wordcount: '275'
ht-degree: 100%

---

# Gérer l’envoi du formulaire HTML5

Les formulaires HTML5 peuvent être envoyés au servlet hébergé dans AEM. Les données envoyées sont accessibles dans le servlet sous la forme d’un flux d’entrée. Pour envoyer votre formulaire HTML5, vous devez ajouter le « bouton Envoyer via HTTP » à votre modèle de formulaire à l’aide d’AEM Forms Designer.

## Créer le gestionnaire d’envoi

Il est possible de créer un servlet simple pour gérer l’envoi du formulaire HTML5. Les données envoyées peuvent ensuite être extraites à l’aide du code suivant. Ce [servlet](assets/html5-submit-handler.zip) est mis à votre disposition dans le cadre de ce tutoriel. Installez le [servlet](assets/html5-submit-handler.zip) à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).

Vous pouvez utiliser le code de la ligne 9 pour appeler le processus J2EE. Vérifiez que vous avez configuré la [configuration du SDK client Adobe LiveCycle](https://helpx.adobe.com/fr/aem-forms/6/submit-form-data-livecycle-process.html) si vous prévoyez d’utiliser le code pour appeler le processus J2EE.

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


## Configurer l’URL d’envoi du formulaire HTML5

![submit-url](assets/submit-url.PNG)

* Appuyez sur le fichier xdp et cliquez sur _Propriétés_ -> _Avancé_.
* Copiez l’URL suivante : http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html et collez-la dans le champ de texte URL d’envoi.
* Cliquez sur le bouton _Enregistrer et fermer_.

### Ajouter une entrée dans les chemins d’exclusion

* Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
* Recherchez _Filtre Adobe CSRF Granite_.
* Ajoutez l’entrée suivante dans la section Chemins d’exclusion :
* _/content/AemFormsSamples/handlehml5formsubmission_
* Enregistrez vos modifications.

### Tester le formulaire

* Appuyez sur le modèle xdp.
* Cliquez sur _Aperçu_->Aperçu en tant que HTML.
* Saisissez des données dans le formulaire, puis cliquez sur Envoyer.
* Les données envoyées doivent être écrites dans le fichier stdout.log de votre serveur.

### Lecture supplémentaire

Nous vous recommandons également cet [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=fr) sur la génération de PDF à partir de l’envoi de formulaires HTML5.
