---
title: Recherche et indexation dans AEM as a Cloud Service
description: Découvrez les index de recherche d’AEM as a Cloud Service, comment convertir AEM définitions d’index 6 et comment déployer des index.
version: Cloud Service
feature: Search
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8634
thumbnail: 336963.jpeg
exl-id: f752df86-27d4-4dbf-a3cb-ee97b7d9a17e
source-git-commit: 77b960315c07ba194642a412a0cc6049edcf7bd2
workflow-type: tm+mt
source-wordcount: '332'
ht-degree: 2%

---

# Recherche et indexation

Découvrez les index de recherche d’AEM as a Cloud Service, comment convertir AEM 6 définitions d’index pour qu’elles soient compatibles avec l’as a Cloud Service et comment déployer les index vers l’as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/336963?quality=12&learn=on)

## Outil convertisseur d’index

![Outil convertisseur d’index](./assets/index-converter.png)

Dans le cadre de la refactorisation de votre base de code, utilisez le [Outil convertisseur d’index](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) pour convertir des définitions d’index Oak personnalisées en définitions d’index compatibles AEM.

Consultez la section [documentation du convertisseur d’index](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/refactoring-tools/index-converter.html) pour l’ensemble complet et actuel des fonctionnalités du convertisseur d’index.

## Activités clés

+ Utilisez la variable [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationindex-converter) pour migrer les workflows de traitement des ressources afin d’utiliser les microservices Asset compute.
+ Configurez une [environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr) et déployez les index personnalisés. Vérifiez que les index mis à jour sont à jour.
+ Déployez la base de code mise à jour dans un environnement de développement as a Cloud Service AEM et continuez à valider.
+ Si vous modifiez un index prêt à l’emploi **TOUJOURS** copiez la dernière définition d’index à partir d’un environnement as a Cloud Service AEM s’exécutant sur la dernière version. Modifiez la définition d’index copiée selon vos besoins.

## Exercice pratique

Appliquez vos connaissances en essayant ce que vous avez appris grâce à cet exercice pratique.

Avant d’essayer l’exercice pratique, assurez-vous d’avoir visionné et compris la vidéo ci-dessus, ainsi que les documents suivants :

+ [Penser différemment à AEM as a Cloud Service](./introduction.md)
+ [Modernisation du référentiel](./repository-modernization.md)

Assurez-vous également que vous avez terminé l’exercice pratique précédent :

+ [Exercice pratique de l’outil de transfert de contenu](./content-migration/content-transfer-tool.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing"><img alt="Référentiel GitHub d’exercice pratique" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Utilisation des index</div>
            <p style="margin:1rem 0">
                Explorez la définition et le déploiement des index Oak pour AEM as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session7-indexes#cloud-acceleration-bootcamp---session-7-search-and-indexing" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Tester l’indexation</span>
            </a>
        </td>
    </tr>
</table>
