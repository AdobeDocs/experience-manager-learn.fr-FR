---
title: Ouvrir l’interface utilisateur de l’agent lors de l’envoi POST
description: Il s’agit de la partie 11 du tutoriel en plusieurs étapes sur la création de votre premier document de communication interactive pour le canal d’impression. Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.
feature: Interactive Communication
doc-type: Tutorial
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
duration: 180
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 100%

---

# Ouvrir l’interface utilisateur de l’agent lors de l’envoi POST

Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.

Cet article décrit les étapes à suivre pour ouvrir l’interface utilisateur de l’agent lors de l’envoi d’un formulaire. Le cas d’utilisation type consiste à ce que l’agent du service clientèle remplisse un formulaire avec certains paramètres d’entrée et que l’interface utilisateur de l’agent s’ouvre avec des données préremplies à partir du service de préremplissage du modèle de données de formulaire. Les paramètres d’entrée du service de préremplissage sont extraits de l’envoi du formulaire.

La vidéo suivante présente un cas d’utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Ligne 1 : obtenir le numéro de compte à partir de requestparameter.

Lignes 2-8 : créer un mappage de paramètres et définir les clés et valeurs appropriées pour refléter documentId,Random.

Lignes 9-10 : créer un autre objet de mappage destiné à contenir le paramètre d’entrée défini dans le modèle de données de formulaire.

Ligne 11 : définir l’attribut slingRequest « paramMap ».

Lignes 12-13 : transférer la requête vers le servlet.

Pour tester cette fonctionnalité sur votre serveur :

* [Importez et installez les fichiers liés à cet article à l’aide du gestionnaire de packes.](assets/launch-agent-ui.zip)
* [Connectez-vous à configMgr](http://localhost:4502/system/console/configMgr).
* Recherchez _Filtre Adobe CSRF Granite_.
* Ajoutez _/content/getprintchannel_ dans les chemins exclus.
* Enregistrez vos modifications.
* [Ouvrez POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Assurez-vous que la chaîne transmise à FormFieldRequestParameter est un documentId valide.(Ligne 19).
* [Ouvrez la page web](http://localhost:4502/content/OpenPrintChannel.html), saisissez le numéro de compte et envoyez le formulaire.
* L’interface utilisateur de l’agent doit s’ouvrir avec les données préremplies spécifiques au numéro de compte saisi dans le formulaire.

>[!NOTE]
>
>Assurez-vous que le paramètre d’entrée de l’opération GET de votre modèle de données de formulaire est lié à l’attribut de demande « accountnumber » pour que cela fonctionne. Si vous remplacez le nom de bindingvalue par un autre nom, assurez-vous que la modification est répercutée à la ligne 25 du fichier POST.jsp.
