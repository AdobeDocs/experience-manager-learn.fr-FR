---
title: Paramètre de requête GET
description: Accès au paramètre de requête d’un service de préremplissage de modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 8%

---

# Paramètre de requête GET

## Get empID parameter

L’étape suivante consiste à accéder au paramètre empID à partir de l’URL. La valeur du paramètre de requête empID est ensuite transmise à la variable **_get_** opération de service du modèle de données de formulaire.
Pour les besoins de ce cours, nous avons créé et fourni les éléments suivants :

* Modèle de formulaire adaptatif appelé **_FDMDemo_**
* Composant de page appelé **_fdmdemo_**
* Inclusion de notre jsp personnalisé avec le composant de page
* Association du modèle de formulaire adaptatif au composant de page

Ce faisant, notre code dans le jsp personnalisé ne sera exécuté que lorsque le formulaire adaptatif basé sur ce modèle personnalisé est rendu.

* [Importez le package](assets/template-page-component.zip) using [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
* [Ouvrez fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Ne commentez pas les lignes commentées.
* Enregistrez vos modifications

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

La valeur de empID est associée à la clé appelée empID dans ParaMap. Ce mappage est ensuite transmis à slingRequest.

>[!NOTE]
>
>La clé empID doit correspondre à la valeur de liaison des entités de recherche obtenir le service.

## Étapes suivantes

[Création d’un formulaire adaptatif basé sur un modèle de données de formulaire](./create-adaptive-form.md)
