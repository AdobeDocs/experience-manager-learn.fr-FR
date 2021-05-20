---
title: Obtenir le paramètre de requête
description: Accès au paramètre de requête d’un service de préremplissage de modèle de données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 6%

---

# Obtenir le paramètre de requête

## Get empID parameter

L’étape suivante consiste à accéder au paramètre empID à partir de l’URL. La valeur du paramètre de requête empID est ensuite transmise à l’opération de service **_get_** du modèle de données de formulaire.
Pour les besoins de ce cours, nous avons créé et fourni les éléments suivants :

* Modèle de formulaire adaptatif appelé **_FDMDemo_**
* Composant de page appelé **_fdmdemo_**
* Inclusion de notre jsp personnalisé avec le composant de page
* Association du modèle de formulaire adaptatif au composant de page

Ce faisant, notre code dans le jsp personnalisé ne sera exécuté que lorsque le formulaire adaptatif basé sur ce modèle personnalisé est rendu.

* [Importez les ](assets/template-page-component.zip) packages à l’aide du gestionnaire de  [packages](http://localhost:4502/crx/packmgr/index.jsp)
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
