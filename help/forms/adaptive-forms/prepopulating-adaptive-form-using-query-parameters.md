---
title: Renseignez les formulaires adaptatifs à l’aide des paramètres de requête.
description: Renseignez les formulaires adaptatifs avec les données des paramètres de requête.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
jira: KT-11470
last-substantial-update: 2020-11-12T00:00:00Z
exl-id: 14ac6ff9-36b4-415e-a878-1b01ff9d3888
duration: 58
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '199'
ht-degree: 100%

---

# Préremplir les formulaires adaptatifs à l’aide des paramètres de requête

L’une des personnes faisant partie de notre clientèle avait besoin de remplir un formulaire adaptatif à l’aide des paramètres de requête. Par exemple, dans l’URL suivante, les champs FirstName et LastName du formulaire adaptatif sont respectivement définis sur John et Doe.

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Pour réaliser ce cas d’utilisation, un nouveau modèle de formulaire adaptatif a été créé et associé à un composant de page. Dans ce composant de page, nous avons un JSP pour récupérer les paramètres de requête et créer une structure XML qui peut être utilisée pour remplir le formulaire adaptatif.

Les détails sur la création d’un nouveau modèle de formulaire adaptatif et d’un composant de page sont [expliqués dans cette vidéo.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=fr)

Voici le code utilisé dans la page JSP.

```java
java.util.Enumeration enumeration = request.getParameterNames();
String dataXml = "<afData><afUnboundData><data>";
while (enumeration.hasMoreElements())
{
   String parameterName = (String) enumeration.nextElement();
   dataXml = dataXml + "<" + parameterName + ">" + request.getParameter(parameterName) + "</" + parameterName + ">";

}

dataXml = dataXml + "</data></afUnboundData></afData>";
//System.out.println("The data xml is "+dataXml);
slingRequest.setAttribute("data", dataXml);
```

>[!NOTE]
>
>Si votre formulaire utilise un schéma, la structure de votre XML sera différente et vous devrez construire le XML en conséquence.


## Déployer les ressources sur votre système

* [Télécharger et installer le modèle de formulaire adaptatif à l’aide du gestionnaire de packages](assets/populate-with-xml.zip)
* [Télécharger et installer l’exemple de formulaire adaptatif](assets/populate-af-with-query-paramters-form.zip)

* [Prévissualiser le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Vous devriez voir le formulaire adaptatif renseigné avec les valeurs John et Doe.
