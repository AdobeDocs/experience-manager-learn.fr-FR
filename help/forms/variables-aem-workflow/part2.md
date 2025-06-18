---
title: Variables dans un workflow AEM [partie 2]
description: Utiliser des variables de type XML, JSON, ArrayList et Document dans un workflow AEM
version: Experience Manager 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: e7d3e0be-5194-47c2-a668-ce78e727986e
duration: 354
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '262'
ht-degree: 100%

---

# Variables de type JSON dans un workflow AEM

À compter d’AEM Forms 6.5, nous pouvons désormais créer des variables de type JSON dans un workflow AEM. En règle générale, vous créez des variables de type JSON si vous envoyez des formulaires adaptatifs basés sur un schéma JSON vers un workflow AEM ou si vous souhaitez stocker les résultats d’une opération d’appel de modèle de données de formulaire. La vidéo suivante vous guide tout au long des étapes nécessaires à la création et à l’utilisation d’une variable de type JSON dans un workflow AEM.

**Si vous utilisez AEM Forms 6.5.0** :

Lorsque vous créez une variable de type JSON pour capturer les données envoyées dans votre modèle de workflow, n’associez pas le schéma JSON à la variable. En effet, lorsque vous envoyez un schéma JSON basé sur un formulaire adaptatif, les données envoyées ne sont pas conformes au schéma JSON. Les données de plainte de schéma JSON sont incluses dans l’élément afData.afBoundData.data.

>[!VIDEO](https://video.tv.adobe.com/v/26444?quality=12&learn=on)


**Si vous utilisez AEM Forms 6.5.1 et versions ultérieures** :

Vous pouvez mapper le schéma avec la variable de type JSON dans votre modèle de workflow. Vous pouvez ensuite utiliser l’explorateur de schéma pour mapper les éléments de schéma avec vos variables chaîne/nombre dans votre modèle de workflow.

>[!VIDEO](https://video.tv.adobe.com/v/34825?quality=12&learn=on&captions=fre_fr)

Pour que les ressources fonctionnent sur votre système, procédez comme suit :

* [Téléchargez et importez les ressources dans AEM à l’aide du gestionnaire de packages.](assets/jsonandstringvariable.zip)
* [Explorez le modèle de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/jsonvariable.html) pour comprendre les variables utilisées dans le workflow.
* [Configurer le service de messagerie](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/notification.html?lang=fr)
* [Envoyer le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/shipping-address-addition-updation-form/jcr:content?wcmmode=disabled)
* Renseigner les informations et envoyer le formulaire
