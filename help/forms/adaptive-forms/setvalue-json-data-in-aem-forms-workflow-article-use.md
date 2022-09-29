---
title: Définition de la valeur de l’élément de données Json dans le processus AEM Forms
description: Lorsqu’un formulaire adaptatif est acheminé vers différents utilisateurs dans AEM flux de travaux, il est nécessaire de masquer ou désactiver certains champs ou panneaux en fonction de la personne qui le visualise. Pour répondre à ces cas d’utilisation, nous définissons généralement la valeur d’un champ masqué. En fonction des valeurs de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '685'
ht-degree: 2%

---

# Définition de la valeur de l’élément de données JSON dans le processus AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Lorsqu’un formulaire adaptatif est acheminé vers différents utilisateurs dans AEM flux de travaux, il est nécessaire de masquer ou désactiver certains champs ou panneaux en fonction de la personne qui le visualise. Pour répondre à ces cas d’utilisation, nous définissons généralement la valeur d’un champ masqué. En fonction des valeurs de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.

![Définition de la valeur d’un élément dans les données JSON](assets/capture-3.gif)

Dans AEM Forms OSGi : nous devons créer un lot OSGi personnalisé pour définir la valeur de l’élément de données JSON. Le bundle est fourni dans le cadre de ce tutoriel.

Nous utilisons l’étape du processus dans AEM workflow. Nous associons le lot OSGi &quot;Set Value of Element in Json&quot; à cette étape de processus.

Nous devons transmettre deux arguments au lot de valeurs définies. Le premier argument est le chemin d’accès à l’élément dont la valeur doit être définie. Le deuxième argument est la valeur qui doit être définie.

Par exemple, dans la capture d’écran ci-dessus, nous définissons la valeur de l’élément initialStep sur &quot;N&quot;

afData.afUnboundData.data.initialStep,N

Dans notre exemple, nous avons un simple formulaire de demande de désactivation du temps. L&#39;initiateur de ce formulaire remplit son nom et les dates de congé. Lors de l’envoi, ce formulaire est envoyé au &quot;gestionnaire&quot; pour révision. Lorsque le gestionnaire ouvre le formulaire, les champs du premier panneau sont désactivés. Ceci car nous avons défini la valeur de l’élément d’étape initial dans les données JSON sur N.

En fonction de la valeur des champs de l’étape initiale, nous affichons le panneau d’approbateur dans lequel le &quot;responsable&quot; peut approuver ou rejeter la demande.

Veuillez consulter les règles définies par rapport à &quot;Étape initiale&quot;. En fonction de la valeur du champ initialStep, nous récupérons les détails de l’utilisateur à l’aide du modèle de données de formulaire, nous renseignons les champs appropriés et masquons/désactivez les panneaux appropriés.

Pour déployer les ressources sur votre système local :

* [Télécharger et déployer DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Télécharger et déployer le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui vous permet de définir les valeurs d’un élément dans les données JSON envoyées.

* [Télécharger et extraire le contenu du fichier zip](assets/set-value-jsondata.zip)
   * Pointez votre navigateur sur [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)
      * Importez et installez le fichier SetValueOfElementInJSONDataWorkflow.zip.Ce package contient l’exemple de modèle de workflow et de modèle de données de formulaire associés au formulaire.

* Pointez votre navigateur sur [Forms et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Cliquez sur Créer | Téléchargement du fichier
* Télécharger le fichier TimeOffRequestForm.zip
   **Ce formulaire a été créé à l’aide d’AEM Forms 6.4. Assurez-vous d’être sur AEM Forms 6.4 ou version ultérieure.**
* Ouvrez le [formulaire](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Renseignez les champs Dates de début et de fin et envoyez le formulaire.
* Accédez à [&quot;Boîte de réception&quot;](http://localhost:4502/aem/inbox)
* Ouvrez le formulaire associé à la tâche.
* Notez que les champs du premier panneau sont désactivés.
* Le panneau d’approbation ou de refus de la requête est maintenant visible.

>[!NOTE]
>
>Puisque nous prérenseignons le formulaire adaptatif à l’aide du profil utilisateur, assurez-vous que l’administrateur [informations de profil utilisateur ](http://localhost:4502/security/users.html). Assurez-vous au minimum d’avoir défini les valeurs des champs FirstName, LastName et Email .
>Vous pouvez activer la journalisation de débogage en activant la journalisation pour com.aemforms.setvalue.core.SetValueInJson. [ici](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>Le lot OSGi pour définir la valeur des éléments de données dans les données JSON prend actuellement en charge la possibilité de définir une valeur d’élément à la fois. Si vous souhaitez définir plusieurs valeurs d’élément, vous devrez plusieurs fois utiliser l’étape de processus.
>
>Assurez-vous que le chemin d’accès au fichier de données dans les options d’envoi du formulaire adaptatif est défini sur &quot;Data.xml&quot;. En effet, le code de l’étape de processus recherche un fichier appelé Data.xml sous le dossier de charge utile.
