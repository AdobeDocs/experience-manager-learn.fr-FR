---
title: Préremplir les formulaires HTML5 à l’aide de l’attribut de données.
description: Renseignez les formulaires HTML5 en récupérant les données de la source back-end.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 116
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '449'
ht-degree: 100%

---

# Préremplir les formulaires HTML5 à l’aide de l’attribut de données {#prepopulate-html-forms-using-data-attribute}


Les modèles XDP rendus au format HTML à l’aide d’AEM Forms sont appelés HTML5 ou formulaires mobiles. Un cas d’utilisation courant consiste à préremplir ces formulaires lors de leur rendu.

Deux choix s’offrent à vous pour fusionner les données avec le modèle XDP lors de son rendu au format HTML.

**dataRef** : vous pouvez utiliser le paramètre dataRef dans l’adresse URL. Il indique le chemin d’accès absolu du fichier de données fusionné avec le modèle. Ce paramètre peut être une adresse URL d’accès à un service REST renvoyant les données au format XML.

**data** : ce paramètre spécifie les octets de données codés au format UTF-8 qui sont fusionnés avec le modèle. Si ce paramètre est spécifié, le formulaire HTML5 ignore le paramètre dataRef. Il est recommandé d’utiliser l’approche des données.

L’approche recommandée consiste à définir l’attribut de données dans la requête avec les données avec lesquelles vous souhaitez préremplir le formulaire.

slingRequest.setAttribute(&quot;data&quot;, content);

Dans cet exemple, l’attribut data est défini avec le contenu. Le contenu représente les données avec lesquelles vous souhaitez préremplir le formulaire. Pour récupérer le « contenu », vous effectuez généralement un appel REST vers un service interne.

Pour réaliser ce cas d’utilisation, vous devez d’abord créer un profil personnalisé. Pour en savoir plus sur la création d’un profil personnalisé, consultez la [Documentation d’AEM Forms ici](https://helpx.adobe.com/fr/aem-forms/6/html5-forms/custom-profile.html).

Une fois votre profil personnalisé prêt, créez un fichier JSP pour récupérer les données à l’aide d’appels vers votre système principal. Une fois les données récupérées, utilisez la propriété slingRequest.setAttribute(&quot;data&quot;, content); pour préremplir le formulaire.

Lors du rendu du fichier XDP, vous pouvez également lui transmettre certains paramètres et, en fonction de la valeur du paramètre, récupérer les données du système back-end.

[Par exemple, cette URL comporte un paramètre name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john).

Le JSP que vous écrivez aura accès au paramètre name via la propriété request.getParameter(&quot;name&quot;). Vous pouvez ensuite transmettre la valeur de ce paramètre à votre processus principal pour récupérer les données requises.
Pour que cette fonctionnalité fonctionne sur votre système, procédez comme suit :

* [Téléchargez et importez les ressources dans AEM à l’aide du gestionnaire de packages](assets/prepopulatemobileform.zip).
Le package installe les éléments suivants :

   * CustomProfile
   * Exemple de fichier XDP
   * Exemple de point d’entrée POST qui renverra des données pour remplir votre formulaire

>[!NOTE]
>
>Si vous souhaitez remplir votre formulaire en appelant le processus Workbench, vous pouvez inclure callWorkbenchProcess.jsp dans /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp au lieu de setdata.jsp.

* [Pointez votre navigateur vers cette URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Le formulaire doit être prérempli avec la valeur du paramètre name.
