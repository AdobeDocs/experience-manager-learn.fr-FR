---
title: Préremplir le Forms HTML5 à l’aide de l’attribut data .
description: Renseignez les formulaires HTML5 en récupérant les données de la source principale.
feature: Formulaires adaptatifs
version: 6.3,6.4,6.5.
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 2%

---


# Préremplir le Forms HTML5 à l’aide de l’attribut data {#prepopulate-html-forms-using-data-attribute}

Consultez la page [Exemples AEM Forms](https://forms.enablementadobe.com/content/samples/samples.html?query=0) pour obtenir un lien vers une démonstration en direct de cette fonctionnalité.

Les modèles XDP rendus au format HTML à l’aide d’AEM Forms sont appelés HTML5 ou Mobile Forms. Un cas d’utilisation courant consiste à préremplir ces formulaires lors de leur rendu.

Il existe deux manières de fusionner les données avec le modèle xdp lorsqu’il est rendu au format HTML.

**dataRef** : Vous pouvez utiliser le paramètre dataRef dans l’URL. Ce paramètre spécifie le chemin d’accès absolu au fichier de données fusionné avec le modèle. Ce paramètre peut être une URL vers un service REST renvoyant les données au format XML.

**data** : Ce paramètre spécifie les octets de données codés UTF-8 qui sont fusionnés avec le modèle. Si ce paramètre est spécifié, le formulaire HTML5 ignore le paramètre dataRef. Il est recommandé d’utiliser l’approche des données.

L’approche recommandée consiste à définir l’attribut data dans la requête avec les données que vous souhaitez préremplir le formulaire.

slingRequest.setAttribute(&quot;data&quot;, content);

Dans cet exemple, nous définissons l’attribut data avec le contenu. Le contenu représente les données que vous souhaitez préremplir le formulaire. En règle générale, vous récupérez le &quot;contenu&quot; en effectuant un appel REST vers un service interne.

Pour réaliser ce cas pratique, vous devez créer un profil personnalisé. Les détails de la création d’un profil personnalisé sont clairement détaillés dans la [documentation AEM Forms ici](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Une fois que vous avez créé votre profil personnalisé, vous créez un fichier JSP qui récupérera les données en effectuant des appels vers votre système principal. Une fois les données récupérées, vous utiliserez slingRequest.setAttribute(&quot;data&quot;, content); pour préremplir le formulaire

Lorsque le fichier XDP est en cours de rendu, vous pouvez également transférer certains paramètres au xdp et, en fonction de la valeur du paramètre, récupérer les données du système principal.

[Par exemple, cette URL comporte un paramètre name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

Le JSP que vous écrivez aura accès au paramètre name via request.getParameter(&quot;name&quot;) . Vous pouvez ensuite transmettre la valeur de ce paramètre à votre processus principal pour récupérer les données requises.
Pour que cette fonctionnalité fonctionne sur votre système, procédez comme suit :

* [Téléchargez et importez les ressources dans AEM à l’aide du ](assets/prepopulatemobileform.zip)
gestionnaire de modules. Le module installe ce qui suit :

   * CustomProfile
   * Exemple XDP
   * Exemple de point de terminaison de POST qui renverra des données pour remplir votre formulaire

>[!NOTE]
>
>Si vous souhaitez remplir votre formulaire en appelant le processus Workbench, vous pouvez inclure callWorkbenchProcess.jsp dans votre /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp au lieu de setdata.jsp.

* [Pointez votre navigateur favori vers cette URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Le formulaire doit être prérenseigné avec la valeur du paramètre name .
