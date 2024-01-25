---
title: Variables dans un workflow AEM [Partie 1]
description: Utiliser des variables de type XML, JSON, ArrayList et Document dans un workflow AEM
feature: Adaptive Forms, Workflow
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f9782684-3a74-4080-9680-589d3f901617
duration: 570
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '404'
ht-degree: 100%

---

# Variables XML dans un workflow AEM

Les variables de type XML sont généralement utilisées lorsque vous disposez d’un formulaire adaptatif basé sur XSD et que vous souhaitez extraire des valeurs de l’envoi du formulaire adaptatif dans votre workflow.

La vidéo suivante vous guide tout au long des étapes nécessaires pour créer des variables de type chaîne et XML et les utiliser dans votre workflow.

La variable XML peut être utilisée pour préremplir le formulaire adaptatif ou stocker les données d’envoi du formulaire adaptatif dans votre workflow.

La variable de chaîne peut être renseignée par XPath dans la variable XML. Cette variable de chaîne est ensuite généralement utilisée pour renseigner les espaces réservés du modèle d’e-mail dans le composant Envoyer un e-mail.

>[!NOTE]
>
>Si votre formulaire adaptatif n’est pas associé à XSD, le XPath permettant d’obtenir la valeur d’un élément ressemblera à
>
>**/afData/afUnboundData/data/submitterName**

Les données du formulaire adaptatif sont stockées sous l’élément de données comme illustré ci-dessus. **_Dans le XPath ci-dessus, submitterName est le nom du champ de texte dans le formulaire adaptatif._**

>[!NOTE]
>
>**AEM Forms 6.5.0** : lorsque vous créez une variable de type XML pour capturer les données envoyées dans votre modèle de workflow, n’associez pas le XSD à la variable. En effet, lorsque vous envoyez un formulaire adaptatif basé sur XSD, les données envoyées ne sont pas conformes au XSD. Les données de plainte XSD sont incluses dans l’élément /afData/afBoundData/.
>
>**AEM Forms 6.5.1** : si vous associez XSD à votre variable XML, vous pouvez parcourir les éléments de schéma pour effectuer le mappage des variables. Vous ne pourrez pas accéder aux données de formulaire qui ne sont pas liées aux éléments de schéma. Si votre cas d’utilisation consiste à accéder aux données liées aux éléments de schéma ainsi qu’aux données non liées, ne liez pas le schéma à votre variable XML dans le workflow. Vous devrez utiliser l’expression XPath appropriée pour obtenir les données dont vous avez besoin.

## Créer des variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12&learn=on)

### Utiliser un schéma avec une variable XML

**Mapper une variable XML avec le schéma. Utiliser cette fonctionnalité avec AEM Forms 6.5.1 et versions ultérieures**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=12&learn=on)

#### Utiliser la variable dans l’envoi d’un e-mail

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Pour que les ressources fonctionnent sur votre système, procédez comme suit :

* [Téléchargez et importez les ressources dans AEM à l’aide du gestionnaire de packages.](assets/xmlandstringvariable.zip)
* [Explorez le modèle de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) pour comprendre les variables utilisées dans le workflow.
* [Configurer le service de messagerie](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=fr)
* [Ouvrez le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled).
* Renseignez les détails et envoyez le formulaire.
