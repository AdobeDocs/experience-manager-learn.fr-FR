---
title: Utilisation de setvalue dans le workflow AEM Forms
description: Définir la valeur de l’élément dans les données envoyées par Forms adaptative dans AEM Forms OSGI
feature: Formulaires adaptatifs
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 3%

---


# Utilisation de setvalue dans le workflow AEM Forms

Définissez la valeur d’un élément XML dans les données envoyées par Forms adaptative dans le workflow OSGI AEM Forms.

![SetValue](assets/setvalue.png)

LiveCycle utilisé pour avoir un composant de valeur définie qui vous permet de définir la valeur d’un élément XML.

En fonction de cette valeur, lorsque le formulaire est renseigné avec le code XML, vous pouvez masquer/désactiver certains champs ou panneaux du formulaire.

Dans AEM Forms OSGI : nous devrons écrire un lot OSGi personnalisé pour définir la valeur dans le XML. Le bundle est fourni dans le cadre de ce tutoriel.
Nous utilisons l’étape du processus dans AEM workflow. Nous associons le lot OSGi &quot;Set Value of Element in XML&quot; à cette étape de processus.
Nous devons transmettre deux arguments au lot de valeurs définies. Le premier argument est le XPath de l’élément XML dont la valeur doit être définie. Le deuxième argument est la valeur qui doit être définie.
Par exemple, dans la capture d’écran ci-dessus, nous définissons la valeur de l’élément d’étape initiale sur &quot;N&quot;.
En fonction de cette valeur, certains panneaux dans le Forms adaptatif sont masqués ou affichés.
Dans notre exemple, nous avons un simple formulaire de demande de désactivation du temps. L&#39;initiateur de ce formulaire remplit son nom et les dates de congé. Lors de l’envoi, ce formulaire est envoyé à &quot;admin&quot; pour révision. Lorsque l’administrateur ouvre le formulaire, les champs du premier panneau sont désactivés. Ceci car nous avons défini la valeur de l’élément d’étape initial dans le XML sur &quot;N&quot;.

En fonction de la valeur des champs de l’étape initiale, nous affichons le deuxième panneau dans lequel &quot;l’administrateur&quot; peut approuver ou rejeter la demande.

Consultez les règles définies par rapport au champ &quot;Heure de désactivation requise par&quot; à l’aide de l’éditeur de règles.

Pour déployer les ressources sur votre système local, procédez comme suit :

* [Déployer le lot Developer withserviceuser](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Déployez l’exemple de lot](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui vous permet de définir les valeurs d’un élément dans les données XML envoyées.

* [Télécharger et extraire le contenu du fichier zip](assets/setvalueassets.zip)
* Pointez votre navigateur sur [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp).
* Importez et installez setValueWorkflow.zip. Il contient l’exemple de modèle de workflow.
* Pointez votre navigateur sur [Forms and Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur Créer | Téléchargement du fichier
* Téléchargez le fichier TimeOfRequestForm.zip
* Ouvrez [TimeOffRequestform](http://localhost:4502/content/dam/formsanddocuments/timeoffapplication/jcr:content?wcmmode=disabled)
* Renseignez les 3 champs obligatoires et envoyez
* Connectez-vous en tant qu’&quot;admin&quot; à AEM (si ce n’est pas déjà fait).
* Accédez à [&quot;AEM Boîte de réception&quot;](http://localhost:4502/aem/inbox)
* Ouvrez le formulaire &quot;Demande de désactivation de la date de révision&quot;.
* Notez que les champs du premier panneau sont désactivés. En effet, le formulaire est ouvert par le réviseur. Notez également que le panneau d’approbation ou de refus de la requête est désormais visible.

>[!NOTE]
>
>Vous pouvez activer la journalisation de débogage en activant l’enregistreur pour
>com.aemforms.setvalue.core.SetValueinXml
>en pointant votre navigateur vers http://localhost:4502/system/console/slinglog

>[!NOTE]
>
>Assurez-vous que le chemin d’accès au fichier de données dans les options d’envoi du formulaire adaptatif est défini sur &quot;Data.xml&quot;. En effet, l’étape de processus recherche un fichier appelé Data.xml sous le dossier de charge utile.
