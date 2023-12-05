---
title: Variables dans un workflow AEM [Partie 4]
description: Utiliser des variables de type XML, JSON, ArrayList et Document dans un workflow AEM
version: 6.5
topic: Development
feature: Adaptive Forms, Workflow
role: Developer
level: Beginner
exl-id: 269e43f7-24cf-4786-9439-f51bfe91d39c
duration: 139
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '437'
ht-degree: 100%

---

# Variable ArrayList dans un workflow AEM

Des variables de type ArrayList ont été introduites dans AEM Forms 6.5. Un cas d’utilisation courant de la variable ArrayList consiste à définir des itinéraires personnalisés à utiliser dans AssignTask.

Pour utiliser la variable ArrayList dans un workflow AEM, vous devez créer un formulaire adaptatif qui génère des éléments répétés dans les données envoyées. Une pratique courante consiste à définir un schéma contenant un élément de tableau. Pour les besoins de cet article, j’ai créé un schéma JSON simple contenant des éléments de tableau. Le cas d’utilisation est celui d’une personne de l’entreprise qui remplit un rapport de dépenses. Dans le rapport de dépenses, nous capturons le nom de la personne responsable de l’émetteur ou de l’émettrice ainsi que celui de la personne responsable de la première personne. Les noms des personnes responsables sont stockés dans un tableau appelé managerchain. La capture d’écran ci-dessous montre le formulaire de rapport de dépenses et les données d’envoi du formulaire adaptatif.

![expensereport](assets/expensereport.jpg)

Voici les données provenant de l’envoi du formulaire adaptatif. Le formulaire adaptatif était basé sur le schéma JSON. Les données liées à ce schéma sont stockées sous l’élément de données de l’élément afBoundData. La chaîne de gestion « managerchain » est un tableau et nous devons remplir la liste ArrayList avec l’élément « name » de l’objet dans le tableau managerchain.

```json
{
    "afData": {
        "afUnboundData": {
            "data": {
                "numericbox_2762582281554154833426": 700
            }
        },
        "afBoundData": {
            "data": {
                "Employee": {
                    "Name": "Conrad Simms",
                    "Department": "IT",
                    "managerchain": [{
                        "name": "Gloria Rios"
                    }, {
                        "name": "John Jacobs"
                    }]
                },
                "expense": [{
                    "description": "Hotel",
                    "amount": 300
                }, {
                    "description": "Air Fare",
                    "amount": 400
                }]
            }
        },
        "afSubmissionInfo": {
            "computedMetaInfo": {},
            "stateOverrides": {},
            "signers": {},
            "afPath": "/content/dam/formsanddocuments/helpx/travelexpensereport",
            "afSubmissionTime": "20190402102953"
            }
        }
}
```

Pour initialiser la variable ArrayList de la chaîne de sous-type, vous pouvez utiliser le mode de mappage XPath ou la notation de point JSON. La capture d’écran suivante vous montre comment renseigner une variable ArrayList appelée CustomRoutes à l’aide de la notation de point JSON. Veillez à pointer vers un élément d’un objet de tableau tel qu’indiqué dans la capture d’écran ci-dessous. Nous renseignons la liste ArrayList CustomRoutes avec les noms de l’objet de tableau managerchain.
La liste ArrayList CustomRoutes est ensuite utilisée pour renseigner les itinéraires dans le composant AssignTask.
![customroutes](assets/arraylist.jpg)
Une fois la variable ArrayList CustomRoutes initialisée avec les valeurs des données envoyées, les itinéraires du composant AssignTask sont alors renseignés à l’aide de la variable CustomRoutes. La capture d’écran ci-dessous vous montre les itinéraires personnalisés dans une opération AssignTask
![asingtask](assets/customactions.jpg).

Pour tester ce workflow sur votre système, veuillez procéder comme suit :

* Téléchargez et enregistrez le fichier ArrayListVariable.zip dans votre système de fichiers.
* [Importez le fichier zip](assets/arraylistvariable.zip) à l’aide du gestionnaire de packages AEM.
* [Ouvrez le formulaire TravelExpenseReport](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled).
* Saisissez quelques dépenses et les deux noms des personnes responsables.
* Appuyez sur le bouton Envoyer.
* [Ouvrez votre boîte de réception](http://localhost:4502/aem/inbox).
* Une nouvelle tâche intitulée « Assigner à la personne chargée de l’administration des dépenses » devrait s’afficher.
* Ouvrez le formulaire associé à la tâche.
* Vous devriez voir deux itinéraires personnalisés avec les noms des personnes responsables.
  [Explorez le workflow ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Ce workflow utilise la variable ArrayList, la variable de type JSON et l’éditeur de règles dans le composant Division OU.
