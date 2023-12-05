---
title: Utiliser setvalue dans le workflow AEM Forms
description: Définir la valeur de l’élément dans les données envoyées des formulaires adaptatifs dans AEM Forms OSGi
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 3919efee-6998-48e8-85d7-91b6943d23f9
last-substantial-update: 2020-01-09T00:00:00Z
duration: 140
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 100%

---

# Utiliser setvalue dans le workflow AEM Forms

Définissez la valeur d’un élément XML dans les données envoyées des formulaires adaptatifs dans le workflow OSGi AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle sert à obtenir un composant setvalue qui vous permet de définir la valeur d’un élément XML.

En fonction de cette valeur, lorsque le formulaire est renseigné avec le code XML, vous pouvez masquer/désactiver certains champs ou panneaux du formulaire.

Dans AEM Forms OSGi : nous devrons écrire un lot OSGi personnalisé pour définir la valeur dans le XML. Le lot est fourni dans le cadre de ce tutoriel.
Nous utilisons l’étape du processus dans le workflow AEM. Nous associons le lot OSGi « Set Value of Element in XML » à cette étape de processus.
Nous devons transmettre deux arguments au lot de définition de valeur. Le premier argument est le XPath de l’élément XML dont la valeur doit être définie. Le deuxième argument est la valeur qui doit être définie.
Par exemple, dans la capture d’écran ci-dessus, nous définissons la valeur de l’élément d’étape initiale sur « N ».
En fonction de cette valeur, certains panneaux du formulaire adaptatif sont masqués ou affichés.
Dans notre exemple, nous avons un simple formulaire de demande de congés. L’initiateur ou l’iniatrice de ce formulaire saisit son nom et ses dates de congés. Lors de l’envoi, ce formulaire est envoyé à « admin » pour révision. Lorsque l’administrateur ou l’administratrice ouvre le formulaire, les champs du premier panneau sont désactivés. C’est parce que nous avons défini la valeur de l’élément d’étape initial dans le XML sur « N ».

En fonction de la valeur des champs de l’étape initiale, nous affichons le deuxième panneau dans lequel la personne « admin » peut approuver ou rejeter la demande.

Consultez les règles définies par rapport au champ « Congés demandés par » à l’aide de l’éditeur de règles.

Pour déployer les ressources sur votre système local, procédez comme suit :

* [Déployez le lot Developingwithserviceuser.](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Déployez l’exemple de lot](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGi personnalisé qui vous permet de définir les valeurs d’un élément dans les données XML envoyées.

* [Téléchargez et extrayez les contenus du fichier zip.](assets/setvalueassets.zip)
* Pointez votre navigateur sur le [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Importez et installez setValueWorkflow.zip. Il contient l’exemple de modèle de workflow.
* Pointez votre navigateur sur [Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Cliquez sur Créer | Charger un fichier.
* Chargez le fichier TimeOfRequestForm.zip.
* Ouvrez [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled).
* Renseignez les 3 champs obligatoires et envoyez.
* Connectez-vous en tant qu’« admin » à AEM (si ce n’est pas déjà fait).
* Accédez à [« Boîte de réception AEM »](http://localhost:4502/aem/inbox).
* Ouvrez le formulaire « Révision de la demande de congés ».
* Notez que les champs du premier panneau sont désactivés. Cela est dû au fait que le formulaire est ouvert par le réviseur ou la réviseuse. Notez également que le panneau d’approbation ou de refus de la requête est désormais visible.

>[!NOTE]
>
>Vous pouvez activer la journalisation de débogage en activant le journal pour
>com.aemforms.setvalue.core.SetValueinXml
>en pointant votre navigateur vers http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Assurez-vous que le chemin d’accès au fichier de données dans les options d’envoi du formulaire adaptatif est défini sur « Data.xml ». En effet, l’étape de processus recherche un fichier appelé Data.xml sous le dossier payload.
