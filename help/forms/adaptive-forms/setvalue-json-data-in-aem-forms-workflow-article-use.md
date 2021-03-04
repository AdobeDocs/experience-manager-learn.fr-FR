---
title: Définition de la valeur de l’élément de données Json dans le processus AEM Forms
seo-title: Définition de la valeur de l’élément de données Json dans le processus AEM Forms
description: Comme un formulaire adaptatif est acheminé à différents utilisateurs dans AEM flux de travail, il sera nécessaire de masquer ou de désactiver certains champs ou panneaux en fonction de la personne qui examine le formulaire. Pour satisfaire ces cas d'utilisation, nous définissons généralement la valeur d'un champ masqué. En fonction de la valeur de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.
seo-description: Comme un formulaire adaptatif est acheminé à différents utilisateurs dans AEM flux de travail, il sera nécessaire de masquer ou de désactiver certains champs ou panneaux en fonction de la personne qui examine le formulaire. Pour satisfaire ces cas d'utilisation, nous définissons généralement la valeur d'un champ masqué. En fonction de la valeur de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.
uuid: a4ea6aef-a799-49e5-9682-3fa3b7a442fb
feature: Formulaires adaptatifs
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.4
discoiquuid: 548fb2ec-cfcf-4fe2-a02a-14f267618d68
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '767'
ht-degree: 2%

---


# Définition de la valeur de l’élément de données JSON dans le flux de travaux AEM Forms {#setting-value-of-json-data-element-in-aem-forms-workflow}

Comme un formulaire adaptatif est acheminé à différents utilisateurs dans AEM flux de travail, il sera nécessaire de masquer ou de désactiver certains champs ou panneaux en fonction de la personne qui examine le formulaire. Pour satisfaire ces cas d&#39;utilisation, nous définissons généralement la valeur d&#39;un champ masqué. En fonction de la valeur de ce champ masqué, les règles métier peuvent être créées pour masquer/désactiver les panneaux ou champs appropriés.

![Définition de la valeur d’un élément dans les données json](assets/capture-3.gif)

En AEM Forms OSGI - nous devrons écrire un lot OSGi personnalisé pour définir la valeur de l’élément de données JSON. Le lot est fourni dans le cadre de ce didacticiel.

Nous utilisons l’étape du processus dans AEM processus. Nous associons le lot OSGi &quot;Set Value of Element in Json&quot; à cette étape de processus.

Nous devons transmettre deux arguments au groupe de valeurs définies. Le premier argument est le chemin d’accès à l’élément dont la valeur doit être définie. Le deuxième argument est la valeur qui doit être définie.

Par exemple, dans la capture d’écran ci-dessus, la valeur de l’élément initialStep est définie sur &quot;N&quot;.

afData.afUnboundData.data.initialStep,N

Dans notre exemple, nous avons un simple formulaire de demande de désactivation du temps. L&#39;initiateur de ce formulaire remplit son nom et les dates de congé. Lors de l’envoi, ce formulaire est envoyé au &quot;gestionnaire&quot; pour révision. Lorsque le gestionnaire ouvre le formulaire, les champs du premier panneau sont désactivés. En effet, nous avons défini la valeur de l’élément d’étape initiale dans les données JSON sur N.

En fonction de la valeur des champs d&#39;étape initiale, nous montrons au panneau approbateur où le &quot;responsable&quot; peut approuver ou rejeter la demande.

Jetez un coup d&#39;oeil aux règles définies pour &quot;l&#39;étape initiale&quot;. En fonction de la valeur du champ initialStep, nous récupérons les détails de l’utilisateur à l’aide du modèle de données de formulaire et nous renseignons les champs appropriés et masquons/désactivez les panneaux appropriés.

Pour déployer les ressources sur votre système local :

* [Téléchargement et déploiement de DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)

* [Téléchargez et déployez le lot](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Il s’agit du lot OSGI personnalisé qui vous permet de définir les valeurs d’un élément dans les données json envoyées.

* [Téléchargement et extraction du contenu du fichier zip](assets/set-value-jsondata.zip)
   * Pointez votre navigateur sur [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
      * Importez et installez SetValueOfElementInJSONDataWorkflow.zip.Ce package contient l’exemple de modèle de processus et de modèle de données de formulaire associé au formulaire.

* Pointez votre navigateur sur [Forms et Documents](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments).
* Cliquez sur Créer | Téléchargement de fichier
* Télécharger le fichier TimeOffRequestForm.zip
   **Ce formulaire a été créé à l’aide d’AEM Forms 6.4. Assurez-vous d’être sur AEM Forms 6.4 ou version ultérieure.**
* Ouvrez le [formulaire](http://localhost:4502/content/dam/formsanddocuments/timeoffrequest/jcr:content?wcmmode=disabled)
* Renseignez les champs Début et Dates de fin et envoyez le formulaire.
* Accéder à [&quot;Boîte de réception&quot;](http://localhost:4502/aem/inbox)
* Ouvrez le formulaire associé à la tâche.
* Les champs du premier panneau sont désactivés.
* Vous pouvez constater que le groupe d’experts chargé d’approuver ou de rejeter la demande est maintenant visible.

>[!NOTE]
>
>Etant donné que nous pré-renseignons le formulaire adaptatif à l’aide du profil utilisateur, veillez à ce que les informations de profil utilisateur [admin ](http://localhost:4502/security/users.html) soient renseignées. Au minimum, assurez-vous d’avoir défini les valeurs des champs Prénom, Nom et Adresse électronique.
>Vous pouvez activer la journalisation du débogage en activant la journalisation pour com.aemforms.setvalue.core.SetValueInJson [ici](http://localhost:4502/system/console/slinglog)

>[!NOTE]
>
>Le lot OSGi permettant de définir la valeur des éléments de données dans les données JSON prend actuellement en charge la possibilité de définir une valeur d’élément à la fois. Si vous souhaitez définir plusieurs valeurs d’élément, vous devrez plusieurs fois utiliser l’étape de processus.
>
>Assurez-vous que le chemin d’accès au fichier de données dans les options d’envoi du formulaire adaptatif est défini sur &quot;Data.xml&quot;. En effet, le code de l’étape de processus recherche un fichier appelé Data.xml dans le dossier de charge utile.
