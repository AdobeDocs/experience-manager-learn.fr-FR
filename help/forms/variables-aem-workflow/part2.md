---
title: Variables dans AEM Workflow[Part2]
seo-title: Variables dans AEM Workflow[Part2]
description: Utilisation de variables de type xml,json,arraylist,document dans le processus aem
seo-description: Utilisation de variables de type xml,json,arraylist,document dans le processus aem
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 0%

---

# Variables de type JSON dans AEM Workflow

À compter d’AEM Forms 6.5, nous pouvons désormais créer des variables de type JSON dans AEM Workflow. En règle générale, vous créez des variables de type JSON si vous envoyez un Forms adaptatif basé sur un schéma JSON à un processus AEM ou si vous souhaitez stocker les résultats d’une opération d’appel de modèle de données de formulaire. La vidéo suivante vous guide tout au long des étapes nécessaires à la création et à l’utilisation d’une variable de type JSON dans AEM workflow

**Si vous utilisez AEM Forms 6.5.0**

Lorsque vous créez une variable de type JSON pour capturer les données envoyées dans votre modèle de workflow, n’associez pas le schéma JSON à la variable . En effet, lorsque vous envoyez un formulaire adaptatif basé sur un schéma JSON, les données envoyées ne sont pas conformes au schéma JSON. Les données de plainte de schéma JSON sont incluses dans l’élément afData.afBoundData.data .

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si vous utilisez AEM Forms 6.5.1 et versions ultérieures**

Vous pouvez mapper le schéma avec la variable de type JSON dans votre modèle de workflow. Vous pouvez ensuite utiliser l’explorateur de schémas pour mapper les éléments de schéma avec vos variables string/number dans votre modèle de workflow.

>[!VIDEO](https://video.tv.adobe.com/v/28097?quality=12&learn=on)

Pour que les ressources fonctionnent sur votre système, procédez comme suit :

* [Télécharger et importer les ressources dans AEM à l’aide du gestionnaire de packages](assets/jsonandstringvariable.zip)
* [Explorez le ](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) modèle de workflow pour comprendre les variables utilisées dans le workflow.
* [Configuration du service de messagerie](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/notification.html#ConfiguringtheMailService)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/afbasedonjson/jcr:content?wcmmode=disabled)
* Renseignez les détails et envoyez le formulaire
