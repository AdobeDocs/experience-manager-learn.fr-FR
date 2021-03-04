---
title: Variables dans le flux de travail AEM[Partie2]
seo-title: Variables dans le flux de travail AEM[Partie2]
description: Utilisation de variables de type xml, json, arraylist, document dans le processus aem
seo-description: Utilisation de variables de type xml, json, arraylist, document dans le processus aem
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Développement
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 1%

---

# Variables de type JSON dans AEM Workflow

A compter de AEM Forms 6.5, nous pouvons désormais créer des variables de type JSON dans AEM Workflow. En règle générale, vous créez des variables de type JSON si vous envoyez une Forms adaptative basée sur le schéma JSON à un flux de travail AEM ou si vous souhaitez stocker les résultats d’une opération d’appel de modèle de données de formulaire. La vidéo suivante vous guide tout au long des étapes nécessaires pour créer et utiliser une variable de type JSON dans AEM flux de travail.

**Si vous utilisez AEM Forms 6.5.0**

Lorsque vous créez une variable de type JSON pour capturer les données envoyées dans votre modèle de processus, n’associez pas le schéma JSON à la variable. En effet, lorsque vous envoyez un formulaire adaptatif basé sur un schéma JSON, les données envoyées ne sont pas conformes au schéma JSON. Les données de plainte du schéma JSON sont incluses dans l’élément afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si vous utilisez AEM Forms 6.5.1 ou version ultérieure**

Vous pouvez mapper le schéma avec la variable de type JSON dans votre modèle de processus. Vous pouvez ensuite utiliser le navigateur de schéma pour mapper les éléments de schéma avec vos variables de chaîne/nombre dans votre modèle de processus.

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Pour que les ressources fonctionnent sur votre système, procédez comme suit :

* [Téléchargement et importation des ressources dans AEM à l’aide du gestionnaire de packages](assets/jsonandstringvariable.zip)
* [Explorez le ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) modèle de processus pour comprendre les variables utilisées dans le processus.
* [Configuration du service de messagerie](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Renseignez les détails et envoyez le formulaire.
