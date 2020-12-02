---
title: Variables dans AEM Workflow[Part1]
seo-title: Variables dans AEM Workflow[Part1]
description: Utilisation de variables de type xml, json, arraylist, document dans le processus aem
seo-description: Utilisation de variables de type xml, json, arraylist, document dans le processus aem
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 0%

---


# Variables XML dans le flux de travail AEM

Les variables de type XML sont généralement utilisées lorsque vous disposez d’un formulaire adaptatif basé sur XSD et que vous souhaitez extraire des valeurs de l’envoi du formulaire adaptatif dans votre flux de travail.

La vidéo suivante vous guide tout au long des étapes nécessaires pour créer des variables de type chaîne et XML et les utiliser dans votre flux de travail.

La variable XML peut être utilisée pour prérenseigner le formulaire adaptatif ou pour stocker les données d’envoi du formulaire adaptatif dans votre processus.

La variable String peut être renseignée par Xpathing dans la variable XML. Cette variable de chaîne est ensuite généralement utilisée pour renseigner les espaces réservés du modèle de courrier électronique dans le composant Envoyer un courrier électronique.

>[!NOTE]
>
>Si votre formulaire adaptatif n’est pas associé à XSD, le XPath permettant d’obtenir la valeur d’un élément ressemblera à
>
>**/afData/afUnboundData/data/submitterName**

Les données du formulaire adaptatif sont stockées sous l’élément de données comme illustré ci-dessus. **_Dans le XPath ci-dessus submitterName est le nom du champ de texte dans le formulaire adaptatif._**

>[!NOTE]
>
>**AEM Forms 6.5.0**  - Lorsque vous créez une variable de type XML pour capturer les données envoyées dans votre modèle de processus, n’associez pas le schéma XSD à la variable. En effet, lorsque vous envoyez un formulaire adaptatif basé sur XSD, les données envoyées ne sont pas conformes au schéma XSD. Les données de plainte XSD sont incluses dans l’élément /afData/afBoundData/.
>
>**AEM Forms 6.5.1**  - Si vous associez XSD à votre variable XML, vous pouvez parcourir les éléments du schéma pour faire le mappage des variables. Vous ne pourrez pas accéder aux données de formulaire qui ne sont pas liées aux éléments de schéma. Si votre cas d&#39;utilisation consiste à accéder aux données liées aux éléments de schéma ainsi qu&#39;aux données non liées, ne liez pas le schéma à votre variable XML dans le flux de travail. Vous devrez utiliser l&#39;expression XPath appropriée pour obtenir les données dont vous avez besoin.

## Création de variables XML

>[!VIDEO](https://video.tv.adobe.com/v/26440?quality=12?autoplay=1)

### Utilisation du Schéma avec une variable XML

**Mappage d’une variable XML avec un schéma. Utilisez cette fonctionnalité avec AEM Forms 6.5.1 à partir de**

>[!VIDEO](https://video.tv.adobe.com/v/28098?quality=9&learn=on)

#### Utilisation de la variable dans l’envoi d’un courrier électronique

>[!VIDEO](https://video.tv.adobe.com/v/26441?quality=12&learn=on)

Pour que les ressources fonctionnent sur votre système, procédez comme suit :

* [Téléchargement et importation des ressources dans AEM à l’aide du gestionnaire de packages](assets/xmlandstringvariable.zip)
* [Explorez le ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/vacationrequest.html) modèle de processus pour comprendre les variables utilisées dans le processus.
* [Configuration du service de messagerie](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/applicationfortimeoff/jcr:content?wcmmode=disabled)
* Renseignez les détails et envoyez le formulaire.

