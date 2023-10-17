---
title: Workflow simple de requête de congés payés
description: Masquer et afficher des panneaux de formulaire adaptatif dans le workflow AEM
uuid: 28ceb72b-24d9-488e-92af-7e85775dc682
feature: Adaptive Forms
topics: workflow
audience: developer
doc-type: article
activity: use
version: 6.4,6.5
discoiquuid: 1c4822e6-76ce-446b-98cb-408900d68b24
topic: Development
role: Developer
level: Experienced
exl-id: 9342bd2f-2ba9-42ee-9224-055649ac3c90
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '330'
ht-degree: 100%

---

# Workflow simple de requête de congés payés

Dans cet article, nous examinons un workflow simple pour demander des congés payés. Les critères de l’entreprise sont les suivants :

* Pour demander des congés, la personne A remplit un formulaire adaptatif.
* Le formulaire est acheminé vers l’administration AEM (concrètement, il est envoyé à la personne responsable de la personne qui a envoyé le formulaire).
* L’administration ouvre le formulaire. L’administration ne doit pas pouvoir modifier les informations renseignées par la personne qui a envoyé le formulaire.
* La section d’approbation doit être visible pour la personne chargée de l’approbation (dans ce cas, il s’agit de l’administrateur ou de l’administratrice AEM).

Pour répondre au critère ci-dessus, nous utilisons un champ masqué appelé **initialstep** dans le formulaire. Sa valeur par défaut est définie sur Oui. Lors de l’envoi du formulaire, la première étape du workflow définit la valeur de initialstep sur Non. Le formulaire comporte des règles de fonctionnement visant à masquer et à afficher les sections appropriées, en fonction de la valeur de initialstep.

**Configurer un formulaire pour déclencher un workflow AEM**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**Présentation du workflow**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**Vue de la personne qui envoie le formulaire sur le formulaire de requête de congés**

![initialstep](assets/initialstep.gif)

**Vue d’approbation du formulaire**

![approverview](assets/approversview.gif)

Dans la vue d’approbation, la personne ne peut pas modifier les données envoyées. Il existe également une nouvelle section réservée aux approbateurs ou approbratrices.

Pour tester ce workflow sur votre système, procédez comme suit :
* [Téléchargez et déployez DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Téléchargez et déployez le lot OSGI personnalisé SetValue.](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importez les ressources liées à cet article dans AEM](assets/helpxworkflow.zip).
* Ouvrez le [formulaire de requête de congés](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Renseignez les détails et envoyez le document.
* Ouvrez la [boîte de réception](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Une nouvelle tâche devrait vous être attribuée. Ouvrez le formulaire. Les données de l’expéditeur ou de l’expéditrice doivent être en lecture seule et une nouvelle section d’approbation doit être visible.
* Explorer le [modèle de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explorez l’étape du processus. Il s’agit de l’étape qui définit la valeur de initialstep sur Non.
