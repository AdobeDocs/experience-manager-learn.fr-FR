---
title: Ouverture de l'interface utilisateur de l'agent lors de l'envoi du POST
seo-title: Ouverture de l'interface utilisateur de l'agent lors de l'envoi du POST
description: Il s'agit de la partie 11 du didacticiel en plusieurs étapes pour créer votre premier document de communications interactives pour le canal d'impression. Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.
seo-description: Il s'agit de la partie 11 du didacticiel en plusieurs étapes pour créer votre premier document de communications interactives pour le canal d'impression. Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
translation-type: tm+mt
source-git-commit: 824efde8d90dd77d41dce093998b4215db2532ae
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 2%

---


# Ouverture de l&#39;interface utilisateur de l&#39;agent lors de l&#39;envoi du POST

Dans cette partie, nous allons lancer l’interface utilisateur de l’agent pour créer une correspondance ad hoc lors de l’envoi du formulaire.

Cet article décrit les étapes nécessaires à l’ouverture de l’interface utilisateur de l’agent lors de l’envoi d’un formulaire. Le cas d’utilisation typique est que l’agent du service à la clientèle remplisse un formulaire avec certains paramètres d’entrée et que l’interface utilisateur de l’agent d’envoi de formulaire est ouverte avec des données préremplies à partir du service de préremplissage du modèle de données de formulaire. Les paramètres d’entrée du service de préremplissage du modèle de données de formulaire sont extraits de l’envoi du formulaire.

La vidéo suivante montre la casse d&#39;utilisation

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

Ligne 1 : Obtenir le numéro de compte à partir du paramètre de demande

Ligne 2-8 : Créez un mappage de paramètres et définissez les clés et les valeurs appropriées pour refléter le documentId, Random.

Ligne 9-10 : Créez un autre objet Map pour stocker le paramètre d’entrée défini dans le modèle de données de formulaire.

Ligne 11 : Définition de l’attribut slingRequest &quot;paramMap&quot;

Ligne 12-13 : Transférer la demande à la servlet

Pour tester cette fonctionnalité sur votre serveur

* [Importez et installez les actifs associés à cet article à l’aide du gestionnaire de packages.](assets/launch-agent-ui.zip)
* [Connexion à configMgr](http://localhost:4502/system/console/configMgr)
* Rechercher _Adobe Granite CSRF Filter_
* Ajoutez _/content/getprintchannel_ dans les Chemins exclus.
* Enregistrez vos modifications.
* [Ouvrez POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Assurez-vous que la chaîne transmise à FormFieldRequestParameter est un documentId valide.(Ligne 19).
* [Ouvrez la page ](http://localhost:4502/content/OpenPrintChannel.html) Web, saisissez le numéro de compte et envoyez le formulaire.
* L’interface utilisateur de l’agent doit s’ouvrir avec les données pré-renseignées spécifiques au numéro de compte saisi dans le formulaire.

>[!NOTE]
>
>Assurez-vous que le paramètre d’entrée Get de l’opération de votre modèle de données de formulaire est lié à Request Attribute appelé &quot;accountnumber&quot; pour que cela fonctionne. Si vous remplacez le nom de la valeur de liaison par un autre nom, assurez-vous que la modification est répercutée à la ligne 25 du fichier POST.jsp.

