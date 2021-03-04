---
title: Balisage et stockage d’un DE AEM Forms dans DAM
seo-title: Balisage et stockage d’un DE AEM Forms dans DAM
description: Cet article décrit le cas d’utilisation de stockage et de balisage du DE généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.
seo-description: Cet article décrit le cas d’utilisation de stockage et de balisage du DE généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.
uuid: b9ba13ed-52d5-4389-a7d5-bf85e58fea49
feature: '"Forms adaptatif,Workflow"'
topics: developing
audience: implementer
doc-type: article
activity: develop
version: 6.4,6.5
discoiquuid: 53961454-633b-4cd8-aef7-e64ab4e528e4
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '661'
ht-degree: 1%

---


# Balisage et stockage d’un DE AEM Forms dans DAM {#tagging-and-storing-aem-forms-dor-in-dam}

Cet article décrit le cas d’utilisation de stockage et de balisage du DE généré par AEM Forms dans AEM DAM. Le balisage du document est effectué en fonction des données de formulaire envoyées.

Les clients demandent souvent de stocker et de marquer le Document d’enregistrement (DE) généré par AEM Forms dans AEM DAM. Le balisage du document doit être basé sur les données envoyées par Adaptive Forms. Par exemple, si l’état d’emploi dans les données envoyées est &quot;Retraité&quot;, nous voulons marquer le document avec la balise &quot;Retraité&quot; et stocker le document dans DAM.

Le cas d’utilisation est le suivant :

* Un utilisateur remplit un formulaire adaptatif. Dans le formulaire adaptatif, l’état civil de l’utilisateur (ex. : célibataire) et l’état de l’emploi (ex : retraité) sont capturés.
* Lors de l’envoi du formulaire, un processus AEM est déclenché. Ce flux de travail marque le document avec l’état civil (célibataire) et l’état d’emploi (retraité) et stocke le document dans la gestion des actifs numériques.
* Une fois le document stocké dans DAM, l’administrateur doit être en mesure de rechercher le document à l’aide de ces balises. Par exemple, une recherche sur Single ou Retraité récupérerait les DE appropriés.

Pour satisfaire ce cas d&#39;utilisation, une étape de processus personnalisée a été écrite. Dans cette étape, nous récupérons les valeurs des éléments de données appropriés à partir des données envoyées. Nous construisons ensuite la mosaïque de balises à l’aide de cette valeur. Par exemple, si la valeur de l’élément d’état civil est &quot;Célibataire&quot;, le titre de la balise devient **Pic : EmploiStatut/Célibataire. **A l’aide de l’API TagManager, nous trouvons la balise et nous l’appliquons au DE.

Le fragment de code suivant vous montre comment trouver la balise et l’appliquer au document.

```java
Tag tagFound = tagManager.resolveByTitle(tagTitle+xmlElement.getTextContent());
//tagTitle is "Peak:EmploymentStatus/" and the xmlElement.getTextContent() will return the value Single. So the tag title becomes Peak:EmploymentStatus/Single. Once the tag is found we put the tag in array and apply the tags to the resource as shown below
tagArray[i] = tagFound;
tagManager.setTags(metadata, tagArray, true);
```

Pour que cet exemple fonctionne sur votre système, procédez comme suit :
* [Déploiement du lot Developing withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et déployez le lot](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Il s’agit du lot OSGI personnalisé qui définit les balises à partir des données de formulaire envoyées.

* [Téléchargement de l&#39;exemple de formulaire adaptatif](assets/tag-and-store-in-dam-assets.zip)

* [Aller à Forms et Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)

* Cliquez sur Créer | Téléchargement de fichier et téléchargement du fichier sampleadaptiveform.zip

* [Importation des ressources de l’article ](assets/tag-and-store-in-dam-assets.zip) à l’aide d’AEM gestionnaire de packages
* Ouvrez l&#39;[exemple de formulaire en mode prévisualisation](http://localhost:4502/content/dam/formsanddocuments/summit/peakform/jcr:content?wcmmode=disabled). Renseignez la section Personnes et envoyez le formulaire.
* [Accédez au dossier Pic dans DAM](http://localhost:4502/assets.html/content/dam/Peak). Vous devriez voir le DE dans le dossier Pic. Vérifiez les propriétés du document. Il doit être balisé correctement.
Félicitations!! L&#39;exemple a été installé sur votre système.

* Examinons le [flux de travaux](http://localhost:4502/editor.html/conf/global/settings/workflow/models/TagAndStoreDoRinDAM.html) qui est déclenché lors de l’envoi du formulaire.
* La première étape du processus crée un nom de fichier unique en concaténant le nom des candidats et le pays de résidence.
* La deuxième étape du processus transmet la hiérarchie des balises et les éléments des champs de formulaire qui doivent être balisés. L’étape de processus extrait la valeur des données envoyées et construit le titre de la balise qui doit marquer le document.
* Si vous souhaitez stocker un DE dans un autre dossier du DAM, vous devez spécifier l’emplacement du dossier à l’aide des propriétés de configuration, comme indiqué dans la capture d’écran ci-dessous.

Les deux autres paramètres sont spécifiques au DE et au chemin d’accès au fichier de données, comme indiqué dans les options d’envoi de formulaire adaptatif. Assurez-vous que les valeurs que vous spécifiez ici correspondent aux valeurs que vous avez spécifiées dans les options d’envoi de formulaire adaptatif.

![Balise Dor](assets/tag_dor_service_configuration.gif)

