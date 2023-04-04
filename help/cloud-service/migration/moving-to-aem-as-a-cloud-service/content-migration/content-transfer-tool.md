---
title: Migration de contenu à l’aide de l’outil de transfert de contenu
description: Découvrez comment l’outil de transfert de contenu vous aide à migrer le contenu vers AEM as a Cloud Service depuis AEM 6.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8919
thumbnail: 336970.jpeg
exl-id: c51ce8e3-e83c-4f8b-a835-70335ed3a5b9
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 2%

---


# Outil de transfert de contenu

Découvrez comment l’outil de transfert de contenu vous aide à migrer le contenu vers AEM as a Cloud Service à partir d’AEM 6.3+.

>[!VIDEO](https://video.tv.adobe.com/v/336970?quality=12&learn=on)

## Utilisation de l’outil de transfert de contenu

![Cycle de vie de l’outil de transfert de contenu](../assets/content-transfer-tool.png)

L’outil de transfert de contenu est installé sur AEM 6.3+ et transfère le contenu vers AEM as a Cloud Service.

## Activités clés

+ Téléchargez la [dernier outil de transfert de contenu](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=2).
+ Transférez le contenu final d’AEM Author 6.3+ vers AEM service de création as a Cloud Service.
   + Installez l’outil de transfert de contenu sur l’auteur AEM 6.3+ contenant le contenu final à transférer.
   + Exécutez l’outil de transfert de contenu par lots, en transférant des ensembles de contenu.
+ Transférez le contenu final de la publication AEM 6.3+ vers AEM service de publication as a Cloud Service.
   + Installez l’outil de transfert de contenu sur AEM version 6.3+ Publier contenant le contenu final à transférer.
   + Exécutez l’outil de transfert de contenu par lots, en transférant des ensembles de contenu.
+ Éventuellement, le contenu &quot;complément&quot; sur AEM as a Cloud Service, en transférant le nouveau contenu depuis le dernier transfert de contenu

## Exercice pratique

Appliquez vos connaissances en essayant ce que vous avez appris grâce à cet exercice pratique.

Avant d’essayer l’exercice pratique, assurez-vous d’avoir visionné et compris la vidéo ci-dessus, ainsi que les documents suivants :

+ [Outils de modernisation d’AEM](../aem-modernization-tools.md)
+ [Intégration](../onboarding.md)
+ [Cloud Manager](../cloud-manager.md)

Assurez-vous également que vous avez terminé l’exercice pratique précédent :

+ [Exercice pratique de Dispatcher](../dispatcher.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content"><img alt="Référentiel GitHub d’exercice pratique" src="../assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Utilisation de l’outil de transfert de contenu</div>
            <p style="margin:1rem 0">
                Découvrez comment l’outil de transfert de contenu peut déplacer automatiquement le contenu d’AEM 6 vers AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session6-transfercontent#cloud-acceleration-bootcamp---session-6-content" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Essayer l’outil de transfert de contenu</span>
            </a>
        </td>
    </tr>
</table>

## Autres ressources

+ [Télécharger l’outil de transfert de contenu](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Content*+Transfer*+Tool*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.properties.operation=equals&amp;1_group.properties.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout list&amp;p.offset=0&amp;p.limit=2)
+ [Vidéo pratique du service d’importation en bloc](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/migration/bulk-import.html)

