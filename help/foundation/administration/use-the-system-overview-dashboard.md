---
title: Utilisation du Tableau de bord Aperçu du système dans AEM
description: Dans les versions précédentes, les administrateurs AEM devaient examiner plusieurs emplacements afin d’obtenir une image complète de l’instance AEM. L'Aperçu du système vise à résoudre ce problème en fournissant une vue de haut niveau de la configuration, du matériel et de l'état de l'instance AEM à partir d'un seul tableau de bord.
version: 6.4, 6.5
feature: null
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---


# Utilisation du Tableau de bord d&#39;aperçu du système

La fonction Aperçu  du système de Adobe Experience Manager (AEM) fournit une vue de haut niveau de la configuration, du matériel et de l’intégrité de l’instance AEM, à partir d’un seul tableau de bord.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. Vous pouvez accéder à la présentation du système à partir de : **aem Début** > **[!UICONTROL Outils]** > **[!UICONTROL Opérations]** > Aperçu **[!UICONTROL du système]**

   Directement à **[!DNL <server-host>/libs/granite/operations/content/systemoverview.html]**

1. Vous pouvez exporter les informations de la Présentation [!UICONTROL du] système en cliquant sur le bouton [!UICONTROL Télécharger] . Les informations sont également exposées au moyen du [!DNL REST] point de terminaison suivant :
1. Vous trouverez ci-dessous un exemple de sortie du fichier JSON qui est exporté depuis la Présentation [!UICONTROL du]système :

   ```json
   {
       "Health Checks": {
           "1 Critical": "System Maintenance",
           "3 Warn": "Replication Queue, Log Errors, Sling/Granite Content Access Check"
       },
       "Instance": {
           "Adobe Experience Manager": "6.4.0",
           "Run Modes": "s7connect, crx3, non-composite, author, samplecontent, crx3tar",
           "Instance Up Since": "2018-01-22 10:50:37"
       },
       "Repository": {
           "Apache Jackrabbit Oak": "1.8.0",
           "Node Store": "Segment Tar",
           "Repository Size": "0.26 GB",
           "File Data Store": "crx-quickstart/repository/datastore"
       },
       "Maintenance Tasks": {
           "Failed": "AuditLog Maintenance Task, Project Purge, Workflow Purge",
           "Succeeded": "Data Store Garbage Collection, Lucene Binaries Cleanup, Revision Clean Up, Version Purge, Purge of ad-hoc tasks"
       },
       "System Information": {
           "Mac OS X": "10.12.6",
           "System Load Average": "2.29",
           "Usable Disk Space": "89.44 GB",
           "Maximum Heap": "0.97 GB"
       },
       "Estimated Node Counts": {
           "Total": "232448",
           "Tags": "62",
           "Assets": "267",
           "Authorizables": "210",
           "Pages": "1592"
       },
       "Replication Agents": {
           "Blocked": "publish, publish2",
           "Idle": "offloading_b3deb296-9b28-4f60-8587-c06afa2e632c, offloading_outbox, offloading_reverse_b3deb296-9b28-4f60-8587-c06afa2e632c, publish_reverse, scene7, screens, screens2, test_and_target"
       },
       "Distribution Agents": {
           "Blocked": "publish"
       },
       "Workflows": {
           "Running Workflows": "15",
           "Failed Workflows": "15",
           "Failed Jobs": "150"
       },
       "Sling Jobs": {
           "Failed": "305"
       }
   }
   ```
