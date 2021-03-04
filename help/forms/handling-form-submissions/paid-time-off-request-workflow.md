---
title: Processus de demande de désactivation de la date payante simple
description: Masquer et afficher les panneaux de formulaires adaptatifs dans AEM flux de travail
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Formulaires adaptatifs
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 1%

---


# Processus de demande de désactivation de la date payante simple

Dans cet article, nous allons examiner un flux de travail simple utilisé pour demander un congé payé. Les besoins commerciaux sont les suivants :

* L’utilisateur A demande un congé en remplissant un formulaire adaptatif.
* Le formulaire est acheminé vers AEM utilisateur administrateur (dans la vie réelle, il sera acheminé vers le gestionnaire de l’expéditeur).
* L’administrateur ouvre le formulaire. L’administrateur ne doit pas pouvoir modifier les informations remplies par l’expéditeur.
* La section Approbateur doit être visible pour l’approbateur (dans ce cas, il s’agit de l’utilisateur administrateur AEM).

Pour accomplir cette exigence, nous utilisons un champ masqué appelé **première étape** dans le formulaire et sa valeur par défaut est Oui. Lorsque le formulaire est envoyé, la première étape du processus définit la valeur de l&#39;étape initiale sur Non. Le formulaire comporte des règles de fonctionnement pour masquer et afficher les sections appropriées en fonction de la valeur de l’étape initiale.

**Configurer le formulaire pour déclencher le processus AEM**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=9&learn=on)

**Flux de travaux**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=9&learn=on)

**Vue de l&#39;émetteur du formulaire de demande de désactivation du délai**

![initialstep](assets/initialstep.gif)

**Vue approbatrice du formulaire**

![approverview](assets/approversview.gif)

Dans la vue approbatrice, l’approbateur ne peut pas modifier les données envoyées. Il existe également une nouvelle section destinée uniquement aux approbateurs.

Pour tester ce processus sur votre système, suivez les étapes mentionnées ci-dessous :
* [Téléchargement et déploiement de DevelopingWitheServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Téléchargement et déploiement du lot OSGI personnalisé SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importer les actifs liés à cet article dans AEM](assets/helpxworkflow.zip)
* Ouvrez le [Formulaire de demande de désactivation du temps](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Renseignez les détails et envoyez
* Ouvrez la [boîte de réception](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Une nouvelle tâche devrait être affectée. Ouvrez le formulaire. Les données de l’expéditeur doivent être en lecture seule et une nouvelle section d’approbateur doit être visible.
* Explorez le [modèle de flux de travaux](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explorez l’étape du processus. Il s’agit de l’étape qui définit la valeur de l’étape initiale sur Non.
