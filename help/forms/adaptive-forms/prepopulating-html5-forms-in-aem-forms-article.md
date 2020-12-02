---
title: PréRenseigner HTML5 Forms à l’aide de l’attribut data.
seo-title: PréRenseigner HTML5 Forms à l’aide de l’attribut data.
description: Renseignez les formulaires HTML5 en récupérant les données de la source principale.
seo-description: Renseignez les formulaires HTML5 en récupérant les données de la source principale.
feature: integrations
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 2%

---


# PréRenseigner HTML5 Forms à l’aide de l’attribut de données {#prepopulate-html-forms-using-data-attribute}

Consultez la page [Exemples d&#39;AEM Forms](https://forms.enablementadobe.com/content/samples/samples.html?query=0) pour obtenir un lien vers une démonstration en direct de cette fonctionnalité.

Les modèles XDP générés au format HTML à l’aide d’AEM Forms sont appelés HTML5 ou Mobile Forms. Un cas d’utilisation courant consiste à préremplir ces formulaires lors de leur génération.

Il existe deux façons de fusionner les données avec le modèle xdp lorsqu’elles sont rendues au format HTML.

**dataRef** : Vous pouvez utiliser le paramètre dataRef dans l’URL. Ce paramètre spécifie le chemin absolu du fichier de données fusionné avec le modèle. Ce paramètre peut être une URL vers un service REST renvoyant les données au format XML.

**data** : Ce paramètre spécifie les octets de données codés au format UTF-8 qui sont fusionnés avec le modèle. Si ce paramètre est spécifié, le formulaire HTML5 ignore le paramètre dataRef. En règle générale, il est recommandé d’utiliser l’approche des données.

L’approche recommandée consiste à définir l’attribut de données dans la requête avec les données que vous souhaitez pré-remplir le formulaire.

slingRequest.setAttribute(&quot;data&quot;, content);

Dans cet exemple, nous définissons l’attribut de données avec le contenu. Le contenu représente les données que vous souhaitez préremplir le formulaire. En règle générale, vous récupérez le &quot;contenu&quot; en lançant un appel REST à un service interne.

Pour ce faire, vous devez créer un profil personnalisé. Les détails sur la création de profil personnalisé sont clairement documentés dans la [documentation AEM Forms ici](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Une fois votre profil personnalisé créé, vous allez créer un fichier JSP qui récupérera les données en appelant votre système principal. Une fois les données extraites, vous utiliserez slingRequest.setAttribute(&quot;data&quot;, content); pour pré-remplir le formulaire

Lorsque le fichier XDP est rendu, vous pouvez également transférer certains paramètres au fichier xdp et, en fonction de la valeur du paramètre, récupérer les données du système principal.

[Par exemple, cette URL comporte un paramètre name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

Le JSP que vous écrivez aura accès au paramètre name via request.getParameter(&quot;name&quot;) . Vous pouvez ensuite transmettre la valeur de ce paramètre à votre processus principal pour récupérer les données requises.
Pour que cette fonctionnalité fonctionne sur votre système, procédez comme suit :

* [Téléchargez et importez les ressources dans AEM à l&#39;aide de Package ](assets/prepopulatemobileform.zip)
ManagerLe package va installer les éléments suivants :

   * Profil personnalisé
   * Exemple XDP
   * Exemple de point de terminaison POST qui renverra des données pour remplir le formulaire

>[!NOTE]
>
>Si vous souhaitez remplir votre formulaire en appelant le processus Workbench, vous pouvez inclure le fichier callWorkbenchProcess.jsp dans votre fichier /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp au lieu du fichier setdata.jsp.

* [Pointez votre navigateur favori sur cette URL](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). Le formulaire doit être prérenseigné avec la valeur du paramètre name.
