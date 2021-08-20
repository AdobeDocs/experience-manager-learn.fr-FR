---
title: Utilisation de l’API pour générer un document d’enregistrement avec AEM Forms
description: Générer un document d’enregistrement (DOR) par programmation
feature: Formulaires adaptatifs
version: 6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 6%

---


# Utilisation de l’API pour générer un document d’enregistrement dans AEM Forms {#using-api-to-generate-document-of-record-with-aem-forms}

Générer un document d’enregistrement (DOR) par programmation

Cet article illustre l’utilisation de `com.adobe.aemds.guide.addon.dor.DoRService API` pour générer par programmation un **document d’enregistrement**. [Document d’](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/generate-document-of-record-for-non-xfa-based-adaptive-forms.html) enregistrement est une version PDF des données capturées dans le formulaire adaptatif.

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
1. Assurez-vous d’avoir installé et démarré le lot DevelopingWithServiceUser fourni dans le cadre de l’ [article Créer un utilisateur de service](service-user-tutorial-develop.md)
1. [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
1. Recherchez Apache Sling Service User Mapper Service .
1. Assurez-vous que l’entrée suivante _DevelopingWithServiceUser.core:getformsresourceresolver=fd-service_ est dans la section Mappages de service .
1. [Ouvrir le formulaire](http://localhost:4502/content/dam/formsanddocuments/sandbox/1201-borrower-payments/jcr:content?wcmmode=disabled)
1. Remplissez le formulaire et cliquez sur &quot;Afficher le PDF&quot;.
1. L’affichage de l’outil de recherche dans un nouvel onglet doit s’afficher dans votre navigateur.


**Conseils de dépannage**

Le fichier PDF ne s’affiche pas dans le nouvel onglet du navigateur :

1. Veillez à ne pas bloquer les fenêtres contextuelles dans votre navigateur.
1. Assurez-vous d’avoir suivi les étapes décrites dans cet [article](service-user-tutorial-develop.md)
1. Assurez-vous que le lot &#39;DevelopingWithServiceUser&#39; est à *principal état*
1. Assurez-vous que les données de l’utilisateur système disposent des autorisations de lecture, de modification et de création sur le noeud suivant `/content/usergenerated/content/aemformsenablement`

