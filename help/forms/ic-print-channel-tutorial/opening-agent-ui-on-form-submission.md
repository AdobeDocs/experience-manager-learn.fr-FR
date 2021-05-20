---
title: Ouverture de l’interface utilisateur de l’agent lors de l’envoi du POST
seo-title: Ouverture de l’interface utilisateur de l’agent lors de l’envoi du POST
description: Il s’agit de la 11e partie du tutoriel en plusieurs étapes pour créer votre premier document de communication interactive pour le canal d’impression. Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.
seo-description: Il s’agit de la 11e partie du tutoriel en plusieurs étapes pour créer votre premier document de communication interactive pour le canal d’impression. Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Communication interactive
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 2%

---


# Ouverture de l’interface utilisateur de l’agent lors de l’envoi du POST

Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.

Cet article décrit les étapes à suivre pour ouvrir l’interface utilisateur de l’agent lors de l’envoi d’un formulaire. Le cas d’utilisation type consiste à ce que l’agent du service client remplisse un formulaire avec certains paramètres d’entrée et que l’interface utilisateur de l’agent d’envoi de formulaire s’ouvre avec des données préremplies à partir du service de préremplissage du modèle de données de formulaire. Les paramètres d’entrée du service de préremplissage du modèle de données de formulaire sont extraits de l’envoi du formulaire.

La vidéo suivante présente un cas pratique

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

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

Ligne 1 : Obtention du numéro de compte à partir du paramètre de requête

Ligne 2-8 : Créez un mappage de paramètres et définissez les clés et valeurs appropriées pour refléter documentId, Random.

Ligne 9 à 10 : Créez un autre objet Map destiné à contenir le paramètre d’entrée défini dans le modèle de données de formulaire.

Ligne 11 : Définition de l’attribut slingRequest &quot;paramMap&quot;

Ligne 12-13 : Transfert de la requête vers la servlet

Pour tester cette fonctionnalité sur votre serveur

* [Importez et installez les actifs liés à cet article à l’aide du gestionnaire de packages.](assets/launch-agent-ui.zip)
* [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
* Recherchez _Adobe Granite CSRF Filter_
* Ajoutez _/content/getprintchannel_ dans les chemins exclus.
* Enregistrez vos modifications.
* [Ouvrez POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Assurez-vous que la chaîne transmise à FormFieldRequestParameter est un documentId valide.(Ligne 19).
* [Ouvrez la page ](http://localhost:4502/content/OpenPrintChannel.html) web, saisissez le numéro de compte et envoyez le formulaire.
* L’interface utilisateur de l’agent doit s’ouvrir avec les données prérenseignées spécifiques au numéro de compte saisi dans le formulaire.

>[!NOTE]
>
>Assurez-vous que le paramètre d’entrée de l’opération Get de votre modèle de données de formulaire est lié à l’attribut de demande appelé &quot;numéro de compte&quot; pour que cela fonctionne. Si vous remplacez le nom de la valeur de liaison par un autre nom, assurez-vous que la modification est répercutée à la ligne 25 du fichier POST.jsp.

