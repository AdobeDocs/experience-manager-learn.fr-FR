---
title: Renseignez le Forms adaptatif à l’aide de paramètres de requête.
description: Renseignez le Forms adaptatif avec les données des paramètres de requête.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Experienced
kt: 11470
last-substantial-update: 2020-11-12T00:00:00Z
source-git-commit: fad7630d2d91d03b98a3982f73a689ef48700319
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# Préremplir le Forms adaptatif à l’aide de paramètres de requête

L’un de nos clients avait l’obligation de renseigner le formulaire adaptatif à l’aide des paramètres de requête. Par exemple, dans l’URL suivante, les champs FirstName et LastName du formulaire adaptatif sont définis sur John et Doe respectivement.

```html
https://forms.enablementadobe.com/content/forms/af/testingxml.html?FirstName=John&LastName=Doe
```

Pour réaliser ce cas d’utilisation, un nouveau modèle de formulaire adaptatif a été créé et associé à un composant de page. Dans ce composant de page, nous disposons d’un jsp pour récupérer les paramètres de requête et créer une structure xml qui peut être utilisée pour remplir le formulaire adaptatif.

Les détails de la création d’un modèle de formulaire adaptatif et d’un composant de page sont les suivants : [expliqué dans cette vidéo.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/storing-and-retrieving-form-data/part5.html?lang=en)

Voici le code qui a été utilisé dans la page jsp.

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
>Si votre formulaire utilise un schéma, la structure de votre fichier xml sera différente et vous devrez créer le fichier xml en conséquence.


## Déployer les ressources sur votre système

* [Téléchargez et installez le modèle de formulaire adaptatif à l’aide du gestionnaire de modules.](assets/populate-with-xml.zip)
* [Télécharger et installer l’exemple de formulaire adaptatif](assets/populate-af-with-query-paramters-form.zip)

* [Aperçu du formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/testingxml/jcr:content?wcmmode=disabled&amp;FirstName=John&amp;LastName=Doe)
Le formulaire adaptatif doit être renseigné avec la valeur John and Doe
