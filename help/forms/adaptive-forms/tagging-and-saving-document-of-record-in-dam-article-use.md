---
title: Balisage et stockage d’un document d’enregistrement AEM Forms dans la gestion des ressources numériques
description: Cet article décrit le cas pratique de stockage et de balisage du document d’enregistrement généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.
feature: Formulaires adaptatifs
version: 6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '616'
ht-degree: 1%

---


# Balisage et stockage d’un document d’enregistrement AEM Forms dans la gestion des ressources numériques {#tagging-and-storing-aem-forms-dor-in-dam}

Cet article décrit le cas pratique de stockage et de balisage du document d’enregistrement généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.

Les clients ont généralement pour tâche de stocker et de baliser le document d’enregistrement généré par AEM Forms dans AEM DAM. Le balisage du document doit être basé sur les données envoyées par la Forms adaptative. Par exemple, si l’état de l’emploi dans les données envoyées est &quot;Retiré&quot;, nous voulons baliser le document avec la balise &quot;Retiré&quot; et le stocker dans la gestion des ressources numériques.

Le cas pratique est le suivant :

* Un utilisateur remplit un formulaire adaptatif. Dans le formulaire adaptatif, l’état civil de l’utilisateur (ex Célibataire) et l’état de l’emploi (ex. Retraité) sont capturés.
* Lors de l’envoi du formulaire, un workflow d’AEM est déclenché. Ce processus balise le document avec l’état civil (célibataire) et l’état de travail (retraité) et stocke le document dans la gestion des ressources numériques.
* Une fois le document stocké dans DAM, l’administrateur doit pouvoir le rechercher à l’aide de ces balises. Par exemple, la recherche sur un document d’enregistrement unique ou à la retraite récupérerait les DE appropriés.

Pour répondre à ce cas d’utilisation, une étape de processus personnalisée a été écrite. Au cours de cette étape, nous récupérons les valeurs des éléments de données appropriés à partir des données envoyées. Nous construisons ensuite la mosaïque de balise à l’aide de cette valeur. Par exemple, si la valeur de l’élément d’état civil est &quot;Célibataire&quot;, le titre de la balise devient **Pic:EmploiStatus/Célibataire. **À l’aide de l’ API TagManager , nous recherchons la balise et nous l’appliquons au DE.

Le fragment de code suivant montre comment trouver la balise et l’appliquer au document.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Pour que cet exemple fonctionne sur votre système, procédez comme suit :
* [Déployer le lot Developer withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et déployez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui définit les balises à partir des données de formulaire envoyées.

* [Téléchargement de l’exemple de formulaire adaptatif](assets/tag-and-store-in-dam-assets.zip)

* [Accès à Forms et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Cliquez sur Créer | Téléchargement du fichier et transfert du fichier sampleadaptiveform.zip

* [Importez les ressources de l’article ](assets/tag-and-store-in-dam-assets.zip) à l’aide AEM gestionnaire de packages
* Ouvrez l’ [exemple de formulaire en mode d’aperçu](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Renseignez la section Personnes et envoyez le formulaire.
* [Accédez au dossier Pic dans DAM](http://localhost:4502/assets.html/content/dam/Peak). Vous devriez voir le document d’enregistrement dans le dossier Pic . Vérifiez les propriétés du document. Il doit être correctement balisé.
Félicitations!! Vous avez installé l’exemple sur votre système.

* Explorons le [workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) qui est déclenché lors de l’envoi du formulaire.
* La première étape du workflow crée un nom de fichier unique en concaténant le nom et le lieu de résidence des candidats.
* La deuxième étape du workflow transmet la hiérarchie de balises et les éléments de champs de formulaire qui doivent être balisés. L’étape de processus extrait la valeur des données envoyées et construit le titre de la balise qui doit marquer le document.
* Si vous souhaitez stocker un document d’enregistrement dans un autre dossier de la gestion des actifs numériques, vous devez spécifier l’emplacement du dossier à l’aide des propriétés de configuration, comme indiqué dans la capture d’écran ci-dessous.

Les deux autres paramètres sont spécifiques au DE et au chemin d’accès au fichier de données, comme indiqué dans les options d’envoi du formulaire adaptatif. Assurez-vous que les valeurs que vous spécifiez ici correspondent aux valeurs que vous avez spécifiées dans les options d’envoi du formulaire adaptatif.

![Balise Dor](assets/tag_dor_service_configuration.gif)

