---
title: Définir la valeur de l’élément de données Json dans AEM Forms Workflow
description: Lorsqu’un formulaire adaptatif est acheminé vers différents utilisateurs et utilisatrices dans le workflow AEM, il est nécessaire de masquer ou désactiver certains champs ou panneaux en fonction de la personne qui le visualise. Pour répondre à ces cas d’utilisation, nous définissons généralement la valeur d’un champ masqué. En fonction des valeurs de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.
feature: Adaptive Forms
version: 6.4
topic: Development
role: Developer
level: Experienced
exl-id: fbe6d341-7941-46f5-bcd8-58b99396d351
last-substantial-update: 2021-06-09T00:00:00Z
duration: 167
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '656'
ht-degree: 100%

---

# Définir la valeur de l’élément de données JSON dans AEM Forms Workflow {#setting-value-of-json-data-element-in-aem-forms-workflow}

Lorsqu’un formulaire adaptatif est acheminé vers différents utilisateurs et utilisatrices dans le workflow AEM, il est nécessaire de masquer ou désactiver certains champs ou panneaux en fonction de la personne qui le visualise. Pour répondre à ces cas d’utilisation, nous définissons généralement la valeur d’un champ masqué. En fonction des valeurs de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.

![Définition de la valeur d’un élément dans les données json.](assets/capture-3.gif)

Dans OSGi AEM Forms : nous devons créer un lot OSGi personnalisé pour définir la valeur de l’élément de données JSON. Le lot est fourni dans le cadre de ce tutoriel.

Nous utilisons l’étape du processus dans le workflow AEM. Nous associons le lot OSGi « Set Value of Element in Json » à cette étape de processus.

Nous devons transmettre deux arguments au lot de définition de valeur. Le premier est le chemin d’accès à l’élément dont la valeur doit être définie. Le deuxième argument est la valeur qui doit être définie.

Par exemple, dans la capture d’écran ci-dessus, nous définissons la valeur de l’élément initialStep sur « N ».

afData.afUnboundData.data.initialStep,N

Dans notre exemple, nous avons un simple formulaire de demande de congés. L’initiateur ou l’iniatrice de ce formulaire saisit son nom et ses dates de congés. Lors de l’envoi, ce formulaire est envoyé à la personne « responsable » pour examen. Lorsque la personne responsable ouvre le formulaire, les champs du premier panneau sont désactivés. C’est parce que nous avons défini la valeur de l’élément d’étape initial dans les données JSON sur N.

En fonction de la valeur des champs de l’étape initiale, nous affichons le panneau d’approbation dans lequel la personne « responsable » peut approuver ou rejeter la demande.

Veuillez consulter les règles définies par rapport à « Étape initiale ». En fonction de la valeur du champ initialStep, nous récupérons les détails de l’utilisateur ou de l’utilisatrice à l’aide du modèle de données de formulaire, nous renseignons les champs appropriés et masquons/désactivons les panneaux appropriés.

Pour déployer les ressources sur votre système local :

* [Téléchargez et déployez DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et déployez le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé qui vous permet de définir les valeurs d’un élément dans les données json envoyées.

* [Téléchargez et extrayez les contenus du fichier zip.](assets/set-value-jsondata.zip)
   * Pointez votre navigateur sur le [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
      * Importez et installez le fichier SetValueOfElementInJSONDataWorkflow.zip. Ce package contient les exemples de modèle de workflow et de modèle de données de formulaire associés au formulaire.

* Pointez votre navigateur sur [Formulaires et documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Cliquez sur Créer | Charger un fichier.
* Chargez le fichier TimeOffRequestForm.zip.
  **Ce formulaire a été créé avec AEM Forms 6.4. Vérifiez que vous utilisez AEM Forms 6.4 ou une version ultérieure.**
* Ouvrez le [formulaire](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled).
* Renseignez les champs Dates de début et de fin et envoyez le formulaire.
* Accédez à [« Boîte de réception »](http://localhost:4502/aem/inbox).
* Ouvrez le formulaire associé à la tâche.
* Notez que les champs du premier panneau sont désactivés.
* Le panneau d’approbation ou de refus de la demande est maintenant visible.

>[!NOTE]
>
>Puisque nous prérenseignons le formulaire adaptatif à l’aide du profil de l’utilisateur ou de l’utilisatrice, veillez à vérifier les [informations de profil de l’utilisateur ou de l’utilisatrice](http://localhost:4502/security/users.html) d’administration. Assurez-vous au minimum d’avoir défini les valeurs des champs FirstName, LastName et Email.
>Vous pouvez activer la journalisation de débogage en activant la journalisation pour com.aemforms.setvalue.core.SetValueInJson [ici](http://localhost:4502/system/console/slinglog).

>[!NOTE]
>
>Le lot OSGi pour définir la valeur des éléments de données dans les données JSON prend actuellement en charge la possibilité de définir une valeur d’élément à la fois. Si vous souhaitez définir plusieurs valeurs d’élément, vous devrez utiliser plusieurs fois l’étape de processus.
>
>Assurez-vous que le chemin d’accès au fichier de données dans les options d’envoi du formulaire adaptatif est défini sur « Data.xml ». En effet, le code de l’étape de processus recherche un fichier appelé Data.xml sous le dossier de payload.
