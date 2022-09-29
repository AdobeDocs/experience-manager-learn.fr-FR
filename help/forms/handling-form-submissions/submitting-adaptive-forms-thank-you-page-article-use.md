---
title: Envoi À La Page De Remerciement
seo-title: Submitting To Thank You Page
description: Affichage d’une page de remerciement sur l’envoi d’un formulaire adaptatif
seo-description: Display a thank you page on submitting Adaptive Form
uuid: ec695b87-083a-47f6-92ac-c9a6dc2b85fb
feature: Adaptive Forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 58c6bf42-efe5-41a3-8023-d84f3675f689
topic: Development
role: Developer
level: Beginner
exl-id: 85e1b450-39c0-4bb8-be5d-d7f50b102f3d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '260'
ht-degree: 6%

---

# Envoi À La Page De Remerciement {#submitting-to-thank-you-page}

L’option Envoyer vers le point de fin REST transmet les données renseignées dans le formulaire à une page de confirmation configurée dans le cadre de la demande de GET HTTP. Vous pouvez ajouter le nom des champs à la requête. Le format de la requête est le suivant :

\{fieldName\} = \{parameterName\}. Par exemple, submitterName est le nom d’un champ de formulaire adaptatif et submitter le nom du paramètre. Dans la page de remerciement, vous pouvez accéder au paramètre submitter à l’aide de request.getParameter(&quot;submitter&quot;) pour obtenir la valeur du champ du nom de l’expéditeur.

`submitterName=submitter`

Dans la capture d’écran ci-dessous, nous envoyons le formulaire adaptatif pour vous remercier, sur la page /content/thankyou. À cette page de remerciement, nous transmettons 3 attributs de requête contenant les valeurs des champs de formulaire.

![Page de remerciement](assets/thankyoupage.gif)

Vous pouvez également envoyer vers le point de terminaison externe via POST. Pour ce faire, il vous suffit de cocher la case &quot;Activer la requête post&quot; et de fournir l’URL du point de terminaison externe. Lorsque vous envoyez votre formulaire, vous obtenez une page de remerciement et le point de terminaison du POST est appelé simultanément.

![Configuration de capture](assets/capture.gif)

Pour tester cette fonctionnalité sur votre serveur, suivez les instructions ci-dessous :

* Importez la variable [fichier de ressources associé à cet article dans AEM à l’aide du gestionnaire de modules](assets/submittingtorestendpoint.zip)
* Pointez votre navigateur sur la [Formulaire de demande d’activation du délai](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Renseignez le champ requis et envoyez le formulaire
* Vous devriez obtenir la page de remerciement avec vos informations renseignées sur la page.
