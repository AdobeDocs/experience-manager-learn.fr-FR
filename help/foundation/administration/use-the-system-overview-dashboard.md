---
title: Utilisation du tableau de bord de présentation du système dans AEM
description: Dans les versions précédentes d’AEM, les administrateurs devaient consulter plusieurs emplacements afin d’obtenir une vue d’ensemble de l’instance AEM. L’ aperçu du système vise à résoudre ce problème en fournissant une vue de haut niveau de la configuration, du matériel et de l’intégrité de l’instance AEM, le tout à partir d’un seul tableau de bord.
version: 6.4, 6.5
topics: administration, operations, monitoring
activity: use
audience: administrator, architect, developer, implementer
doc-type: technical video
contentOwner: dgordon
topic: Administration
role: Administrator
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 0%

---


# Utilisation du tableau de bord de présentation du système

Adobe Experience Manager (AEM) [!UICONTROL Présentation du système] fournit une vue de haut niveau de la configuration, du matériel et de l’intégrité de l’instance AEM à partir d’un seul tableau de bord.

>[!VIDEO](https://video.tv.adobe.com/v/21340?quality=12&learn=on)

1. L’ aperçu du système est accessible à partir de : **AEM Démarrage** > **[!UICONTROL Outils]** > **[!UICONTROL Opérations]** > **[!UICONTROL Présentation du système]**

   Directement à **`<server-host>/libs/granite/operations/content/systemoverview.html`**

1. Les informations de la [!UICONTROL Présentation du système] peuvent être exportées en cliquant sur le bouton [!UICONTROL Télécharger]. Les informations sont également exposées via le point de terminaison [!DNL REST] suivant :
1. Vous trouverez ci-dessous un exemple de sortie du fichier JSON exporté à partir de [!UICONTROL System Overview] :

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
