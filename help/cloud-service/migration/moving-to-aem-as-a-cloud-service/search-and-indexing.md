---
title: Rechercher et indexer dans AEM as a Cloud Service
description: Découvrez les index de recherche d’AEM as a Cloud Service et apprenez à convertir les définitions d’index d’AEM 6 ainsi qu’à déployer des index.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
jira: KT-8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
duration: 1247
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 100%

---

# Rechercher et indexer

Découvrez les index de recherche d’AEM as a Cloud Service et apprenez à convertir les définitions d’index d’AEM 6 pour qu’elles soient compatibles avec AEM as a Cloud Service ainsi qu’à déployer des index vers AEM as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Outil convertisseur d’index

![Outil convertisseur d’index.](./assets/index-converter.png)

Dans le cadre de la refactorisation de votre base de code, utilisez l’[outil convertisseur d’index](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) pour convertir des définitions d’index Oak personnalisées en définitions d’index compatibles avec AEM as a Cloud Service.

Consultez la [documentation du convertisseur d’index](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html?lang=fr) pour obtenir l’ensemble complet et actuel des fonctionnalités du convertisseur d’index.

## Activités clés

+ Utilisez l’outil de [migration des workflows Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) pour migrer les workflows de traitement des ressources afin d’utiliser les microservices Asset Compute.
+ Configurez un [environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr) et déployez les index personnalisés. Vérifiez que les index mis à jour sont à jour.
+ Déployez la base de code mise à jour dans un environnement de développement AEM as a Cloud Service et cliquez sur Continuer pour valider.
+ Si vous modifiez un index prêt à l’emploi, veuillez **TOUJOURS** copier la dernière définition d’index à partir d’un environnement AEM as a Cloud Service s’exécutant sur la dernière version. Modifiez la définition d’index copiée selon vos besoins.

## Exercice pratique

Mettez en pratique les connaissances que vous venez d’acquérir grâce à cet exercice.

Avant de commencer cet exercice pratique, assurez-vous d’avoir visionné et bien compris le contenu de la vidéo ci-dessus, ainsi que les documents suivants :

+ [Penser différemment AEM as a Cloud Service](./introduction.md)
+ [Modernisation du référentiel](./repository-modernization.md)

Assurez-vous également d’avoir terminé l’exercice pratique précédent :

+ [Exercice pratique de l’outil de transfert de contenu](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Exercice pratique : référentiel GitHub" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Utilisation pratique des index</div>
            <p style="margin:1rem 0">
                Explorez la définition et le déploiement des index Oak pour AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Essayer l’indexation</span>
</a>
        </td>
    </tr>
</table>
