---
title: Variables dans AEM Workflow[Part4]
seo-title: Variables dans AEM Workflow[Part4]
description: Utilisation de variables de type xml,json,arraylist,document dans le processus aem
seo-description: Utilisation de variables de type xml,json,arraylist,document dans le processus aem
feature: Workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 0%

---


# Variable ArrayList dans AEM Workflow

Des variables de type ArrayList ont été introduites dans AEM Forms 6.5. Un cas d’utilisation courant pour l’utilisation de la variable ArrayList consiste à définir des itinéraires personnalisés à utiliser dans AssignTask.

Pour utiliser la variable ArrayList dans un processus AEM, vous devez créer un formulaire adaptatif qui génère des éléments répétés dans les données envoyées. Une pratique courante consiste à définir un schéma contenant un élément de tableau . Pour les besoins de cet article, j’ai créé un schéma JSON simple contenant des éléments de tableau. Le cas d’utilisation est celui d’un employé qui remplit un rapport de dépenses. Dans le rapport de dépenses, nous capturons le nom du responsable de l’émetteur et celui du responsable. Les noms du responsable sont stockés dans un tableau appelé managerchain. La capture d’écran ci-dessous montre le formulaire de rapport de dépenses et les données de l’envoi du Forms adaptatif.

![rapport de dépenses](assets/expensereport.jpg)

Voici les données provenant de l’envoi du formulaire adaptatif. Le formulaire adaptatif était basé sur le schéma JSON. Les données liées au schéma sont stockées sous l’élément de données de l’élément afBoundData . La chaîne de gestion est un tableau et nous devons remplir la liste ArrayList avec l’élément name de l’objet dans le tableau managerchain.

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

Pour initialiser la variable ArrayList de la chaîne de sous-type, vous pouvez utiliser le mode de notation JSON ou de mappage XPath. La capture d’écran suivante vous montre comment renseigner une variable ArrayList appelée CustomRoutes à l’aide de la notation JSON. Veillez à pointer vers un élément d’un objet de tableau comme illustré dans la capture d’écran ci-dessous. Nous renseignons le tableau ArrayList CustomRoutes avec les noms de l’objet de tableau managerchain.
La liste ArrayList CustomRoutes est ensuite utilisée pour renseigner les itinéraires dans le composant AssignTask
![customroutes](assets/arraylist.jpg)
Une fois la variable ArrayList CustomRoutes initialisée avec les valeurs des données envoyées, les Routes du composant AssignTask sont alors renseignées à l’aide de la variable CustomRoutes . La capture d’écran ci-dessous vous montre les itinéraires personnalisés dans une tâche AssignTask
![asingtask](assets/customactions.jpg)

Pour tester ce workflow sur votre système, procédez comme suit :

* Téléchargez et enregistrez le fichier ArrayListVariable.zip dans votre système de fichiers.
* [Importez le ](assets/arraylistvariable.zip) fichier zip à l’aide du gestionnaire de modules AEM
* [Ouvrez le formulaire TravelExpenseReport .](http://localhost:4502/content/dam/formsanddocuments/helpx/travelexpensereport/jcr:content?wcmmode=disabled)
* Saisissez quelques dépenses et les deux noms du responsable
* Appuyez sur le bouton Envoyer .
* [Ouvrir votre boîte de réception](http://localhost:4502/aem/inbox)
* Une nouvelle tâche intitulée &quot;Assigner à l’administrateur des dépenses&quot; devrait s’afficher.
* Ouvrir le formulaire associé à la tâche
* Vous devriez voir deux itinéraires personnalisés avec les noms du gestionnaire.
   [Explorez le Workflow ReviewExpenseReportWorkflow.](http://localhost:4502/editor.html/conf/global/settings/workflow/models/ReviewExpenseReport.html) Ce workflow utilise la variable ArrayList, la variable de type JSON, l’éditeur de règles dans le composant Ou-Fractionner.
