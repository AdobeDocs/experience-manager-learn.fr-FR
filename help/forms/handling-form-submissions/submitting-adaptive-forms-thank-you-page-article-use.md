---
title: Envoi À La Page De Remerciements
seo-title: Envoi À La Page De Remerciements
description: Affichage d’une page de remerciement lors de l’envoi d’un formulaire adaptatif
seo-description: Affichage d’une page de remerciement lors de l’envoi d’un formulaire adaptatif
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Formulaires adaptatifs
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 7%

---


# Envoyer à la page de remerciement {#submitting-to-thank-you-page}

L’option Envoyer vers le point de terminaison REST transmet les données renseignées dans le formulaire à une page de confirmation configurée dans le cadre de la demande de GET HTTP. Vous pouvez ajouter le nom des champs à la requête. Le format de la requête est le suivant :

\{fieldName\} = \{parameterName\}. Par exemple, submitterName est le nom d’un champ de formulaire adaptatif et submitter le nom du paramètre. Dans la page de remerciement, vous pouvez accéder au paramètre submitter à l’aide de request.getParameter(&quot;submitter&quot;) pour obtenir la valeur du champ du nom de l’expéditeur.

submitterName=submitter

Dans la capture d’écran ci-dessous, nous envoyons le formulaire adaptatif pour vous remercier page située dans /content/thankyou. À cette page de remerciement, nous transmettons 3 attributs de requête qui contiendront les valeurs des champs de formulaire.

![remercier](assets/thankyoupage.gif)

Vous pouvez également envoyer des données au point de terminaison externe par POST. Pour ce faire, il vous suffit de cocher la case &quot;activer la demande de publication&quot; et de fournir l’URL du point de terminaison externe. Lorsque vous envoyez votre formulaire, vous obtenez la page de remerciement et le point de terminaison du POST est appelé simultanément.

![capture](assets/capture.gif)


Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous :

* Importez le fichier [assets associé à cet article dans AEM à l’aide du gestionnaire de packages](assets/submittingtorestendpoint.zip).
* Pointez votre navigateur sur le [Formulaire de demande de désactivation du délai](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Renseignez le champ requis et envoyez le formulaire.
* Vous devriez obtenir la page de remerciement avec vos informations renseignées sur la page.

