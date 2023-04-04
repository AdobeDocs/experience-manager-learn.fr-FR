---
title: Processus simple de demande de désactivation payante
description: Masquage et affichage des panneaux de formulaire adaptatif dans AEM processus
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
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Processus simple de demande de désactivation payante

Dans cet article, nous examinons un processus simple utilisé pour demander la désactivation payante. Les besoins de l’entreprise sont les suivants :

* L’utilisateur A demande un délai d’attente en remplissant un formulaire adaptatif.
* Le formulaire est acheminé vers AEM utilisateur administrateur (en temps réel, il est redirigé vers le responsable de l’expéditeur).
* L’administrateur ouvre le formulaire. L’administrateur ne doit pas pouvoir modifier les informations renseignées par l’expéditeur.
* La section Approbateur doit être visible par l’approbateur (dans ce cas, il s’agit de l’utilisateur administrateur AEM).

Pour accomplir l’exigence ci-dessus, nous utilisons un champ masqué appelé **initialstep** dans le formulaire et sa valeur par défaut est définie sur Oui. Lorsque le formulaire est envoyé, la première étape du processus définit la valeur de l’étape initiale sur Non. Le formulaire comporte des règles de fonctionnement pour masquer et afficher les sections appropriées en fonction de la valeur de l’étape initiale.

**Configuration d’un formulaire pour déclencher AEM processus**

>[!VIDEO](https://video.tv.adobe.com/v/28406?quality=12&learn=on)

**Présentation du workflow**

>[!VIDEO](https://video.tv.adobe.com/v/28407?quality=12&learn=on)

**Vue de l’émetteur du formulaire de demande de désactivation du temps**

![initialstep](assets/initialstep.gif)

**Vue Approbateur du formulaire**

![approverview](assets/approversview.gif)

Dans la vue approbateur, l’approbateur ne peut pas modifier les données envoyées. Il existe également une nouvelle section réservée aux approbateurs.

Pour tester ce workflow sur votre système, procédez comme suit :
* [Télécharger et déployer DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Téléchargement et déploiement du bundle OSGI personnalisé SetValue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar)
* [Importez dans AEM les actifs liés à cet article](assets/helpxworkflow.zip)
* Ouvrez le [Formulaire de demande d’annulation de l’heure](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Renseignez les détails et envoyez
* Ouvrez le [boîte de réception](http://localhost:4502/mnt/overlay/cq/inbox/content/inbox.html). Une nouvelle tâche devrait vous être affectée. Ouvrez le formulaire. Les données de l’expéditeur doivent être en lecture seule et une nouvelle section de l’approbateur doit être visible.
* Explorez les [modèle de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/helpxworkflow.html)
* Explorez l’étape du processus. Il s’agit de l’étape qui définit la valeur de l’étape initiale sur Non.
