---
title: Utilisation de l’API pour générer un document d’enregistrement avec AEM Forms
description: Générer un document d’enregistrement (DOR) par programmation
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 9a3b2128-a383-46ea-bcdc-6015105c70cc
last-substantial-update: 2020-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '254'
ht-degree: 5%

---

# Utilisation de l’API pour générer un document d’enregistrement dans AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Générer un document d’enregistrement (DOR) par programmation

Cet article illustre l’utilisation de la fonction `com.adobe.aemds.guide.addon.dor.DoRService API` pour générer **Document d’enregistrement** par programmation. [Document d’enregistrement](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) est une version PDF des données capturées dans le formulaire adaptatif.

1. Voici le fragment de code. La première ligne récupère le service DOR.
1. Définissez les DoROptions.
1. Appeler la méthode render du DoRService et transmettre l’objet DoROptions à la méthode render

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

Pour essayer cela sur votre système local, procédez comme suit :

1. [Téléchargez et installez les ressources de l’article à l’aide du gestionnaire de modules.](assets/dor-with-api.zip)
1. Assurez-vous d’avoir installé et démarré le lot DevelopingWithServiceUser fourni dans le cadre de [Article Créer un utilisateur de service](service-user-tutorial-develop.md)
1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Recherchez Apache Sling Service User Mapper Service .
1. Assurez-vous que l’entrée suivante _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ dans la section Mappages de service
1. [Ouvrir le formulaire](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Remplissez le formulaire et cliquez sur &quot;Afficher le PDF&quot;.
1. L’affichage de l’outil de recherche dans un nouvel onglet doit s’afficher dans votre navigateur.


**Conseils de dépannage**

PDF ne s’affiche pas dans le nouvel onglet du navigateur :

1. Veillez à ne pas bloquer les fenêtres contextuelles dans votre navigateur.
1. Assurez-vous d’avoir suivi les étapes décrites dans cette section [article](service-user-tutorial-develop.md)
1. Assurez-vous que le lot &#39;DevelopingWithServiceUser&#39; se trouve dans *principal état*
1. Assurez-vous que les données de l’utilisateur système disposent des autorisations de lecture, de modification et de création sur le noeud suivant. `/content/usergenerated/content/aemformsenablement`
