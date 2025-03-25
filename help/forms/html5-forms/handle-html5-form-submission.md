---
title: Gérer un envoi de formulaire HTML5
description: Créez un gestionnaire d’envoi de formulaire HTML5.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-5269
thumbnail: kt-5269.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 93e1262b-0e93-4ba8-aafc-f9c517688ce9
last-substantial-update: 2020-07-07T00:00:00Z
duration: 66
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 18%

---


# Gérer l’envoi du formulaire HTML5

Les formulaires HTML5 peuvent être envoyés à une servlet hébergée dans AEM. Les données envoyées sont accessibles dans le servlet sous la forme d’un flux d’entrée. Pour envoyer votre formulaire HTML5, ajoutez un « bouton Envoyer HTTP » sur votre modèle de formulaire à l’aide d’AEM Forms Designer.

## Créer le gestionnaire d’envoi

Un servlet simple peut gérer l’envoi du formulaire HTML5. Extrayez les données envoyées à l’aide du fragment de code suivant. Téléchargez le [servlet](assets/html5-submit-handler.zip) fourni dans ce tutoriel. Installez le [servlet](assets/html5-submit-handler.zip) à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).

```java
StringBuffer stringBuffer = new StringBuffer();
String line = null;
java.io.InputStreamReader isReader = new java.io.InputStreamReader(request.getInputStream(), "UTF-8");
java.io.BufferedReader reader = new java.io.BufferedReader(isReader);
while ((line = reader.readLine()) != null) {
    stringBuffer.append(line);
}
System.out.println("The submitted form data is " + stringBuffer.toString());
```

Assurez-vous d’avoir configuré la [Configuration du SDK client Adobe LiveCycle](https://helpx.adobe.com/fr/aem-forms/6/submit-form-data-livecycle-process.html) si vous prévoyez d’utiliser le code pour appeler un processus J2EE.

## Configurer l’URL d’envoi du formulaire HTML5

![URL d’envoi](assets/submit-url.PNG)

- Ouvrez le fichier xdp et accédez à _Propriétés_->_Avancé_.
- Copiez http://localhost:4502/content/AemFormsSamples/handlehml5formsubmission.html et collez-le dans le champ de texte URL d’envoi .
- Cliquez sur le bouton _Enregistrer et fermer_.

### Ajouter une entrée dans les chemins d’exclusion

- Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
- Recherchez _Filtre CSRF Granite Adobe_.
- Ajoutez l’entrée suivante dans la section Chemins exclus : _/content/AemFormsSamples/handlehml5formsubmission_.
- Enregistrez vos modifications.

### Tester le formulaire

- Ouvrez le modèle xdp.
- Cliquez sur _Aperçu_->Aperçu en tant qu’HTML.
- Saisissez les données dans le formulaire, puis cliquez sur Envoyer.
- Recherchez les données envoyées dans le fichier stdout.log du serveur.

### Lecture supplémentaire

Pour plus d’informations sur la génération de PDF à partir d’envois de formulaire HTML5, reportez-vous à cet [article](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/generate-pdf-from-mobile-form-submission-article.html?lang=fr).

