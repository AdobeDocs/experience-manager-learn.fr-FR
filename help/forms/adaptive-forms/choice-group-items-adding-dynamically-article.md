---
title: Ajouter des éléments au composant de groupe de choix
description: Ajouter dynamiquement des éléments au composant de groupe de choix
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
exl-id: 8fbea634-7949-417f-a4d6-9e551fff63f3
last-substantial-update: 2021-09-10T00:00:00Z
duration: 362
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '456'
ht-degree: 100%

---

# Ajouter dynamiquement des éléments au composant de groupe de choix

AEM Forms 6.5 offre la possibilité d’ajouter dynamiquement des éléments à un composant de groupe de choix de formulaire adaptatif, comme Case à cocher, Bouton radio et Liste d’images.


Vous pouvez ajouter des éléments à l’aide de l’éditeur visuel ainsi que de l’éditeur de code en fonction de votre cas d’utilisation.

**À l’aide de l’éditeur visuel :** vous pouvez renseigner les éléments du groupe de choix à partir des résultats d’un appel de fonction ou d’un appel de service. Par exemple, vous pouvez définir les éléments du groupe de choix en utilisant la réponse d’un appel API REST.

Dans la capture d’écran ci-dessous, nous définissons les options de la période de prêt (années) avec les résultats d’un appel de service appelé getLoanPeriods.

![Éditeur de règles](assets/ruleeditor.png)

**À l’aide de l’éditeur de code** : lorsque vous souhaitez définir dynamiquement les éléments du groupe de choix en fonction des valeurs saisies dans le formulaire. Par exemple, l’extrait de code suivant définit les éléments de la case à cocher sur les valeurs saisies dans les champs de la personne à l’origine de la demande et de son conjoint ou sa conjointe du formulaire adaptatif.

Dans l’extrait de code, nous définissons les éléments de WorkingMembers qui est un composant Case à cocher. Le tableau des éléments est créé dynamiquement en récupérant les valeurs des champs de texte applicantName et conjoint ou conjointe des formulaires adaptatifs.

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

Les données envoyées sont les suivantes :

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

**Ajouter des éléments à l’aide de l’éditeur de règles**

>[!VIDEO](https://video.tv.adobe.com/v/26847?quality=12&learn=on)

**Ajouter des éléments à l’aide de l’éditeur de code**

>[!VIDEO](https://video.tv.adobe.com/v/26848?quality=12&learn=on)

Pour essayer ceci sur votre système :

**À l’aide de l’éditeur de code pour ajouter des éléments :**

* [Téléchargez les ressources.](assets/usingthecodeeditor.zip)
* [Ouvrez Formulaires et documents.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur « Créer | Charger un fichier » et chargez le fichier que vous avez téléchargé à l’étape précédente.
* [Prévisualisez les formulaires.](http://localhost:4502/content/dam/formsanddocuments/simpleform/jcr:content?wcmmode=disabled)
* Saisissez le nom de la personne à l’origine de la demande et sélectionnez État civil Marié(e).
* Saisissez le nom du conjoint ou de la conjointe.
* Cliquez sur Suivant.
* Vous devriez voir une case à cocher remplie avec le nom de la personne à l’origine de la demande et le nom du conjoint ou de la conjointe si la situation de famille est « Marié(e) ».

**À l’aide de l’éditeur visuel pour ajouter des éléments :**

* [Téléchargez les ressources.](assets/usingthevisualeditor.zip)
* Installez Tomcat si vous ne l’avez pas déjà. [Les instructions d’installation de Tomcat sont disponibles ici](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/introduction.html?lang=fr).
* [Déployez le fichier SampleRest.war contenu dans ce fichier zip dans votre Tomcat.](assets/sample-rest.zip)
* [Ouvrez Formulaires et documents.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur « Créer | Charger un fichier » et chargez le fichier que vous avez téléchargé à l’étape précédente.
* [Prévisualisez les formulaires.](http://localhost:4502/content/dam/formsanddocuments/amortizationschedule/jcr:content?wcmmode=disabled)
* Saisissez le montant du prêt et sortez du champ par tabulation. Cela déclenche la règle qui affiche le champ de période de prêt.
* Sélectionnez la période de prêt appropriée (les éléments de la période de prêt sont renseignés à partir de l’appel REST).
* Sélectionnez le taux d’intérêt et cliquez sur « Obtenir un planning d’amortissement ».
* Le tableau d’amortissement doit être renseigné. Le planning d’amortissement est récupéré à l’aide d’un appel REST.

>[!NOTE]
> On suppose que Tomcat fonctionne sur le port 8080 et AEM sur le port 4502.
