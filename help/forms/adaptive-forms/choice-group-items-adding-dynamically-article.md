---
title: Ajout d’éléments au composant de groupe de choix
seo-title: Ajout d’éléments au composant de groupe de choix
description: Ajouter dynamiquement des éléments au composant de groupe de choix
seo-description: Ajouter dynamiquement des éléments au composant de groupe de choix
feature: Formulaires adaptatifs
topics: authoring
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Développement
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '530'
ht-degree: 3%

---



# Ajout dynamique d’éléments au composant de groupe de choix

AEM Forms 6.5 offre la possibilité d’ajouter dynamiquement des éléments à un composant de groupe de choix de Forms adaptatif tel que Case à cocher, Bouton radio et Liste d’images.

[Cette fonctionnalité est disponible en direct sur le serveur](https://forms.enablementadobe.com/content/samples/samples.html?query=0) Samples. Recherchez la carte d’éléments de case à cocher dynamique et cliquez sur &quot;Essayer&quot;.


Vous pouvez ajouter des éléments à l’aide de l’éditeur visuel ainsi que de l’éditeur de code en fonction de votre cas d’utilisation.

**À l’aide de l’éditeur visuel :**  vous pouvez renseigner les éléments du groupe de choix à partir des résultats d’un appel de fonction ou d’un appel de service. Par exemple, vous pouvez définir les éléments du groupe de choix en utilisant la réponse d’un appel API REST.

Dans la capture d’écran ci-dessous, nous définissons les options de la période de prêt (années) avec les résultats d’un appel de service appelé getLoanPeriods.

![Éditeur de règles](assets/ruleeditor.png)

**Utilisation de l’éditeur** de code : Lorsque vous souhaitez définir dynamiquement les éléments du groupe de choix en fonction des valeurs saisies dans le formulaire. Par exemple, le fragment de code suivant définit les éléments de la case à cocher sur les valeurs saisies dans les champs du nom du demandeur et de l’épouse du formulaire adaptatif.

Dans le fragment de code, nous définissons les éléments de WorkingMembers qui est un composant de case à cocher. Le tableau des éléments est créé dynamiquement en récupérant les valeurs des champs de texte applicantName et épouse des formulaires adaptatifs.

```javascript
 
 if(MaritalStatus.value=="Married")
  {
WorkingMembers.items =["spouse="+spouse.value,"applicant="+applicantName.value];
  }
else
  {
    WorkingMembers.items =["applicant="+applicantName.value];
  }
```

Les données envoyées sont les suivantes :

```xml
<afUnboundData>

<data>

<applicantName>John Jacobs</applicantName>

<MaritalStatus>Married</MaritalStatus>

<spouse>Gloria Rios</spouse>

<WorkingMembers>spouse,applicant</WorkingMembers>

</data>

</afUnboundData>
```

**Ajout d’éléments à l’aide de l’éditeur de règles**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Ajout d’éléments à l’aide de l’éditeur de code**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Pour essayer ceci sur votre système :

**Utilisation de l’éditeur de code pour ajouter des éléments**

* [Téléchargement des ressources](assets/usingthecodeeditor.zip)
* [Ouvrir Forms Et Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur &quot;Créer&quot; | Téléchargement du fichier&quot; et chargez le fichier que vous avez téléchargé à l’étape précédente.
* [Aperçu des formulaires](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Saisissez le nom du demandeur et sélectionnez État civil au mariage
* Saisissez le nom du conjoint
* Cliquez sur Suivant
* Vous devriez voir une case à cocher remplie avec le nom du demandeur et le nom du conjoint si la situation de famille est mariée.

**Utilisation de l’éditeur visuel pour ajouter des éléments**

* [Téléchargement des ressources](assets/usingthevisualeditor.zip)
* Installez Tomcat si vous ne l&#39;avez pas déjà. [Les instructions d’installation de tomcat sont disponibles ici](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html)
* [Déployer le fichier SampleRest.war dans Tomcat](https://forms.enablementadobe.com/content/DemoServerBundles/SampleRest.war)
* [Ouvrir Forms Et Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur &quot;Créer&quot; | Téléchargement du fichier&quot; et chargez le fichier que vous avez téléchargé à l’étape précédente.
* [Aperçu des formulaires](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Saisissez le montant du prêt et sortez du champ par tabulation. Cela déclenche la règle qui affiche le champ de période de prêt.
* Sélectionnez la période de prêt appropriée (les éléments de la période de prêt sont renseignés à partir de l’appel de reste).
* Sélectionnez le taux d’intérêt et cliquez sur &quot;Obtenir un calendrier d’amortissement&quot;.
* La table d&#39;amortissement doit être renseignée. Le planning d’amortissement est récupéré à l’aide d’un appel REST.

>[!NOTE]
> On suppose que tomcat fonctionne sur le port 8080 et AEM sur le port 4502
