---
title: Configuration de votre projet BPA et CAM
description: Découvrez comment l’analyseur des bonnes pratiques et Cloud Acceleration Manager fournissent un guide personnalisé pour la migration vers AEM as a Cloud Service.
version: Cloud Service
feature: Developer Tools
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8627
thumbnail: 336957.jpeg
exl-id: f8289dd4-b293-4b8f-b14d-daec091728c0
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '428'
ht-degree: 4%

---

# Analyseur des bonnes pratiques et gestionnaire d’accélération du cloud

Découvrez comment Best Practices Analyzer (BPA) et Cloud Acceleration Manager (CAM) fournissent un guide personnalisé pour la migration vers AEM as a Cloud Service. 

>[!VIDEO](https://video.tv.adobe.com/v/336957/?quality=12&learn=on)

## Utilisation de BPA et CAM

![Diagramme de haut niveau BPA et CAM](assets/bpa-cam-diagram.png)

Le package BPA doit être installé sur un clone de l’environnement de production AEM 6.x. Le BPA va générer un rapport qui pourra ensuite être téléchargé dans le CAM, qui fournira des conseils sur les activités clés qui doivent avoir lieu afin de passer à AEM as a Cloud Service.

## Activités clés

+ Effectuez un clone de votre environnement de production 6.x. Lorsque vous migrez du contenu et refactorisez le code, disposer d’un clone d’environnement de production sera utile pour tester divers outils et modifications.
+ Téléchargez la dernière version de l’outil BPA à partir du [Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html) et installez sur votre environnement cloné AEM 6.x.
+ Utilisez l’outil BPA pour générer un rapport qui peut être téléchargé vers Cloud Acceleration Manager (CAM). Le mode d’accès CAM est accessible via [https://experience.adobe.com/](https://experience.adobe.com/) > **Experience Manager** > **Cloud Accelerated Manager**.
+ Utilisez le format CAM pour fournir des conseils sur les mises à jour à apporter à la base de code et à l’environnement actuels afin de passer à AEM as a Cloud Service.

## Exercice pratique

Appliquez vos connaissances en essayant ce que vous avez appris grâce à cet exercice pratique.

Avant d’essayer l’exercice pratique, assurez-vous d’avoir visionné et compris la vidéo ci-dessus, ainsi que les documents suivants :

+ [Penser différemment à AEM as a Cloud Service](./introduction.md)
+ [Qu&#39;est-ce AEM as a Cloud Service ?](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/what-is-aem-as-a-cloud-service.html?lang=en)
+ [Architecture d’AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/introduction/architecture.html?lang=en)
+ [Contenu mutable et non modifiable](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/basics/mutable-immutable.html?lang=en)
+ [Différences dans le développement pour AEM as a Cloud Service et AEM 6.x](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/development-guidelines.html#developing)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently"><img alt="Référentiel GitHub d’exercice pratique" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Utilisation de l’analyseur des bonnes pratiques</div>
            <p style="margin:1rem 0">
                Explorez l’analyseur des bonnes pratiques (BPA) et passez en revue les résultats en l’exécutant par rapport à une base de code WKND héritée qui contient des exemples de violations.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session1-differently#bootcamp---session-1-introduction-and-thinking-differently" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Essai de l’analyseur des bonnes pratiques</span>
            </a>
        </td>
    </tr>
</table>


## Autres ressources

+ [Téléchargement de l’analyseur des bonnes pratiques](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=Best*+Practices*+Analyzer*&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1)