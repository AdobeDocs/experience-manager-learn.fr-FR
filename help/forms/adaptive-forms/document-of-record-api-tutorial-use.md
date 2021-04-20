---
title: Utilisation de l’API pour générer un Document d’enregistrement avec AEM Forms
seo-title: Utilisation de l’API pour générer un Document d’enregistrement avec AEM Forms
description: Générer un Document d’enregistrement (DOR) par programmation
seo-description: Utilisation de l’API pour générer un Document d’enregistrement avec AEM Forms
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 94ac3b13-01b4-4198-af81-e5609c80324c
discoiquuid: ba91d9df-dc61-47d8-8e0a-e3f66cae6a87
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 6%

---


# Utilisation de l’API pour générer un Document d’enregistrement dans AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Générer un Document d’enregistrement (DOR) par programmation

Cet article illustre l&#39;utilisation de `com.adobe.aemds.guide.addon.dor.DoRService API` pour générer par programme **Document d&#39;enregistrement**. [Document d&#39;](https://docs.adobe.com/content/help/en/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) enregistrement est une version PDF des données capturées dans le formulaire adaptatif.

1. Voici le fragment de code. La première ligne reçoit le service DOR.
1. Définissez les options DoRO.
1. Appelez la méthode render de DoRService et transmettez l&#39;objet DoROOptions à la méthode render.

```java
com.adobe.aemds.guide.addon.dor.DoRService dorService = sling.getService(com.adobe.aemds.guide.addon.dor.DoRService.class);
com.adobe.aemds.guide.addon.dor.DoROptions dorOptions =  new com.adobe.aemds.guide.addon.dor.DoROptions();
 dorOptions.setData(dataXml);
 dorOptions.setFormResource(resource);
 java.util.Locale locale = new java.util.Locale("en");
 dorOptions.setLocale(locale);
 com.adobe.aemds.guide.addon.dor.DoRResult dorResult = dorService.render(dorOptions);
 byte[] fileBytes = dorResult.getContent();
 com.adobe.aemfd.docmanager.Document dorDocument = new com.adobe.aemfd.docmanager.Document(fileBytes);
```

Pour essayer cela sur votre système local, suivez les étapes ci-dessous.

1. [Téléchargement et installation des ressources de l’article à l’aide du gestionnaire de packages](assets/dor-with-api.zip)
1. Vérifiez que vous avez installé et démarré le lot DevelopingWithServiceUser fourni dans le cadre de l&#39;[article Créer un utilisateur de service](service-user-tutorial-develop.md)
1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Recherchez Apache Sling Service User Mapper Service .
1. Assurez-vous que l’entrée suivante _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ dans la section Mappages de service.
1. [Ouvrir le formulaire](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Remplissez le formulaire et cliquez sur &quot;Vue PDF&quot;
1. Vous devriez voir le DOR dans le nouvel onglet de votre navigateur.


**Conseils de dépannage**

Le fichier PDF ne s’affiche pas dans le nouvel onglet du navigateur :

1. Veillez à ne pas bloquer les fenêtres contextuelles dans votre navigateur.
1. Effectuez les étapes décrites dans cet [article](service-user-tutorial-develop.md).
1. Assurez-vous que le lot &quot;DevelopingWithServiceUser&quot; est à *principal état*.
1. Assurez-vous que les données de l&#39;utilisateur système possèdent des autorisations de lecture, de modification et de création sur le noeud suivant `/content/usergenerated/content/aemformsenablement`.

