---
title: Lister les types de ressources personnalisés dans AEM Forms
description: Partie 2 - Lister les types de ressources personnalisés dans AEM Forms
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
duration: 163
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '584'
ht-degree: 100%

---

# Lister les types de ressources personnalisés dans AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Créer un modèle personnalisé {#creating-custom-template}

Pour les besoins de cet article, nous allons créer un modèle personnalisé permettant d’afficher les types de ressources personnalisés et les types de ressources prêts à l’emploi sur la même page. Pour créer un modèle personnalisé, suivez les instructions suivantes :

1. Créez un dossier sling: sous /apps. Nommez-le « myportalcomponent ».
1. Ajoutez une propriété « fpContentType ». Définissez sa valeur sur « **/libs/fd/ fp/formTemplate ».**
1. Ajoutez une propriété « title » et définissez sa valeur sur « custom template ». Il s’agit du nom qui s’affichera dans la liste déroulante du composant Search and Lister.
1. Créez un fichier « template.html » sous ce dossier. Ce fichier contiendra le code à mettre en forme et affichera les différents types de ressources.

![appsfolder](assets/appsfolder_.png)

Le code suivant répertorie les différents types de ressources à l’aide du composant Search and Lister. Nous créons des éléments HTML distincts pour chaque type de ressource, comme illustré par la balise data-type = &quot;videos&quot;. Pour le type de ressource « videos », nous utilisons l’élément &lt;video> pour lire la vidéo intégrée. Pour le type de ressource « worddocuments », nous utilisons un marquage HTML différent.

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>Ligne 11 - Modifiez l’image src pour qu’elle pointe vers une image de votre choix dans la DAM.
>
>Pour lister les formulaires adaptatifs dans ce modèle, créez un élément div et définissez son attribut data-type sur « guide ». Vous pouvez copier et coller l’élément div où data-type=&quot;printForm&quot; et définir le type de données de l’élément div nouvellement copié sur « guide ».

## Configurer le composant Search and Lister {#configure-search-and-lister-component}

Une fois le modèle personnalisé défini, nous devons l’associer au composant « Search and Lister ». Pointez votre navigateur [sur cette URL](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Passez en mode de conception et configurez le système de paragraphes pour inclure le composant Search and Lister dans le groupe de composants autorisés. Le composant Search and Lister fait partie du groupe Document Services.

Passez en mode d’édition et ajoutez le composant Search and Lister à ParSys.

Ouvrez les propriétés de configuration du composant « Search and Lister ». Assurez-vous que l’onglet « Dossiers de ressources » est sélectionné. Sélectionnez les dossiers à partir desquels vous souhaitez lister les ressources dans le composant Search and Lister. Pour les besoins de cet article, j’ai sélectionné :

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Appuyez sur l’onglet « Affichage ». Choisissez ici le modèle dont vous souhaitez afficher les ressources dans le composant Search and Lister.

Sélectionnez « custom template » dans la liste déroulante, comme illustré ci-dessous.

![searchandlister](assets/searchandlistercomponent.gif)

Configurez les types de ressources que vous souhaitez lister dans le portail. Pour configurer les types de ressources, accédez à l’onglet « Liste des ressources » et configurez les types de ressources. Dans cet exemple, nous avons configuré les types de ressources suivants :

1. Fichiers MP4
1. Documents Word
1. Document (type de ressource prêt à l’emploi)
1. Modèle de formulaire (type de ressource prêt à l’emploi)

La copie d’écran suivante montre les types de ressources configurés pour être listés.

![assettypes](assets/assettypes.png)

Maintenant que vous avez configuré votre composant de portail Search and Lister, il est temps de voir la liste en action. Pointez votre navigateur [sur cette URL](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). Les résultats doivent ressembler à l’image affichée ci-dessous.

>[!NOTE]
>
>Si votre portail répertorie les types de ressources personnalisés sur un serveur de publication, veillez à accorder l’autorisation « lecture » à l’utilisateur ou utilisatrice « fd-service » sur le nœud **/apps/fd/fp/extensions/querybuilder**.

![assettypes](assets/assettypeslistings.png)
[Téléchargez et installez ce package à l’aide du gestionnaire de packages.](assets/customassettypekt1.zip) Il contient des exemples de documents MP4 et Word et de fichiers XDP utilisés comme types de ressources à répertorier à l’aide du composant Search and Lister.
