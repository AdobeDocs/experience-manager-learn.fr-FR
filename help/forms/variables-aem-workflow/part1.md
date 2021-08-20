---
title: Variables dans AEM Workflow[Part1]
description: Utilisation de variables de type XML, JSON, ArrayList, Document dans un workflow AEM
feature: Formulaires adaptatifs
version: 6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 0%

---


# Variables XML dans AEM workflow

Les variables de type XML sont généralement utilisées lorsque vous disposez d’un formulaire adaptatif basé sur XSD et souhaitez extraire des valeurs de l’envoi du formulaire adaptatif dans votre processus.

La vidéo suivante vous guide tout au long des étapes nécessaires pour créer des variables de type chaîne et XML et les utiliser dans votre workflow.

La variable XML peut être utilisée pour préremplir le formulaire adaptatif ou stocker les données d’envoi du formulaire adaptatif dans votre processus.

La variable String peut être renseignée par Xpath dans la variable XML. Cette variable de chaîne est ensuite généralement utilisée pour renseigner les espaces réservés du modèle de courrier électronique dans le composant Envoyer un courrier électronique .

>[!NOTE]
>
>Si votre formulaire adaptatif n’est pas associé à XSD, le XPath permettant d’obtenir la valeur d’un élément ressemblera à
>
>**/afData/afUnboundData/data/submitterName**

Les données du formulaire adaptatif sont stockées sous l’élément de données comme illustré ci-dessus. **_Dans le XPath submitterName ci-dessus est le nom du champ de texte dans le formulaire adaptatif._**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - Lorsque vous créez une variable de type XML pour capturer les données envoyées dans votre modèle de workflow, n’associez pas le schéma XSD à la variable. En effet, lorsque vous envoyez un formulaire adaptatif basé sur XSD, les données envoyées ne sont pas conformes au schéma XSD. Les données de plainte XSD sont incluses dans l’élément /afData/afBoundData/ .
>
>**AEM Forms 6.5.1**  - Si vous associez XSD à votre variable XML, vous pouvez parcourir les éléments de schéma pour effectuer le mappage de variable. Vous ne pourrez pas accéder aux données de formulaire qui ne sont pas liées aux éléments de schéma. Si votre cas d’utilisation consiste à accéder aux données liées aux éléments de schéma ainsi qu’aux données non liées, ne liez pas le schéma à votre variable XML dans le workflow. Vous devrez utiliser l’expression XPath appropriée pour obtenir les données dont vous avez besoin.

## Création de variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Utilisation d’un schéma avec une variable XML

**Mappage d’une variable XML avec le schéma. Utilisez cette fonctionnalité avec AEM Forms 6.5.1 et les versions ultérieures**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Utilisation de la variable dans l’envoi d’un email

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Pour que les ressources fonctionnent sur votre système, procédez comme suit :

* [Télécharger et importer les ressources dans AEM à l’aide du gestionnaire de packages](assets/xmlandstringvariable.zip)
* [Explorez le ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) modèle de workflow pour comprendre les variables utilisées dans le workflow.
* [Configuration du service de messagerie](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Renseignez les détails et envoyez le formulaire.

