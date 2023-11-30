---
title: Paramètre de requête GET
description: Accéder au paramètre de requête d’un service de préremplissage de modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 100%

---

# Paramètre de requête GET

## Paramètre GET empID

L’étape suivante consiste à accéder au paramètre empID à partir de l’URL. La valeur du paramètre de requête empID est ensuite transmise à l’opération de service **_GET_** du modèle de données de formulaire.
Pour les besoins de ce cours, nous avons créé et fourni les éléments suivants :

* Modèle de formulaire adaptatif appelé **_FDMDemo_**
* Composant de page appelé **_fdmdemo_**
* Notre jsp personnalisé avec le composant de page
* Modèle de formulaire adaptatif associé au composant de page

Ce faisant, notre code dans le jsp personnalisé ne sera exécuté que lorsque le formulaire adaptatif basé sur ce modèle personnalisé sera rendu.

* [Installer le package ](assets/template-page-component.zip) à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp)
* [Ouvrir fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Supprimez les commentaires des lignes commentées.
* Enregistrez vos modifications.

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
>La clé empID doit correspondre à la valeur de liaison du service GET des entités représentant les personnes récemment embauchées.

## Étapes suivantes

[Créer un formulaire adaptatif en fonction du modèle de données de formulaire](./create-adaptive-form.md)
