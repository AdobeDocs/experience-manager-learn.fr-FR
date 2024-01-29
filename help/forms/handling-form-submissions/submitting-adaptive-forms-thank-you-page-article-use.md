---
title: Rediriger vers la page de remerciement lors de l’envoi du formulaire
description: Affichez une page de remerciement lors de l’envoi d’un formulaire adaptatif.
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
last-substantial-update: 2020-07-07T00:00:00Z
duration: 65
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '251'
ht-degree: 100%

---

# Rediriger vers la page de remerciement lors de l’envoi du formulaire {#submitting-to-thank-you-page}

L’option Envoyer vers le point d’entrée REST transmet les données renseignées dans le formulaire à une page de confirmation configurée dans le cadre de la requête HTTP GET. Vous pouvez ajouter le nom des champs à demander. Le format de la requête est :

\{fieldName\} = \{parameterName\}. Par exemple, « submitterName » est le nom d’un champ de formulaire adaptatif et « submitter » le nom du paramètre. Sur la page de remerciement, transmettez la propriété request.getParameter(&quot;submitter&quot;) pour accéder au paramètre « submitter » et ainsi obtenir la valeur du champ du nom de la personne qui a envoyé le formulaire.

`submitterName=submitter`

Dans la copie d’écran ci-dessous, le formulaire adaptatif est envoyé à la page de remerciement, située sous /content/thankyou. Trois attributs de requête, contenant les valeurs des champs de formulaire, sont transmis à cette page de remerciement.

![Page de remerciement.](assets/thankyoupage.gif)

Vous pouvez également envoyer le formulaire vers le point d’entrée externe via une requête POST. Pour ce faire, il vous suffit de cocher la case « Activer la requête POST » et d’indiquer l’URL du point d’entrée externe. Lorsque vous envoyez le formulaire, une page de remerciement s’affiche et le point d’entrée POST est appelé simultanément.

![Configuration de la capture](assets/capture.gif).

Pour tester cette fonctionnalité sur votre serveur, procédez comme suit :

* Importez le [fichier de ressources associé à cet article dans AEM à l’aide du gestionnaire de packages](assets/submittingtorestendpoint.zip).
* Pointez votre navigateur sur le [Formulaire de demande de congés](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Renseignez le champ obligatoire et envoyez le formulaire.
* La page de remerciement s’affiche et contient les informations que vous avez renseignées.
