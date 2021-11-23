---
title: Microservices AEM Assets et passage à AEM as a Cloud Service
description: Découvrez comment les microservices d’asset compute d’AEM Assets as a Cloud Service vous permettent de générer automatiquement et efficacement n’importe quel rendu pour vos ressources, en remplaçant ce rôle de workflow d’AEM traditionnel.
version: Cloud Service
feature: Asset Compute Microservices
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8635
thumbnail: 336990.jpeg
exl-id: 327e8663-086b-4b31-b159-a0cf30480b45
source-git-commit: 1dcb66bc3535231c89f3e7fc127688fcf96f2b61
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 5%

---

# Microservices AEM Assets - Passage à AEM as a Cloud Service

Découvrez comment les microservices d’asset compute d’AEM Assets as a Cloud Service vous permettent de générer automatiquement et efficacement n’importe quel rendu pour vos ressources, en remplaçant ce rôle de workflow d’AEM traditionnel.

>[!VIDEO](https://video.tv.adobe.com/v/336990/?quality=12&learn=on)

## Outil de migration des workflows

![Outil de migration des workflows de ressources](./assets/asset-workflow-migration.png)

Dans le cadre de la refactorisation de votre base de code, utilisez le [Outil de migration des workflows de ressources](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/asset-workflow-migration-tool.html?lang=fr) pour migrer les workflows existants afin d&#39;utiliser les microservices Asset compute dans AEM as a Cloud Service.

## Activités clés

+ Utilisez la variable [Adobe I/O Workflow Migrator](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#command-aio-aem-migrationworkflow-migrator) pour migrer les workflows de traitement des ressources afin d’utiliser les microservices Asset compute.
+ Configurez une [environnement de développement local](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview.html?lang=fr) et déployer les workflows mis à jour. Un ajustement manuel peut être nécessaire pour les workflows complexes.
+ Continuez à itérer dans un environnement de développement local à l’aide du SDK AEM jusqu’à ce que le workflow mis à jour corresponde à la parité des fonctionnalités.
+ Déployez la base de code mise à jour dans un environnement de développement as a Cloud Service AEM et continuez à valider.

## Exercice pratique

Appliquez vos connaissances en essayant ce que vous avez appris grâce à cet exercice pratique.

Avant d’essayer l’exercice pratique, assurez-vous d’avoir visionné et compris la vidéo ci-dessus, ainsi que les documents suivants :

+ [Penser différemment à AEM as a Cloud Service](./introduction.md)
+ [Intégration ](./onboarding.md)

Assurez-vous également que vous avez terminé l’exercice pratique précédent :

+ [Exercice pratique de recherche et d’indexation](./search-and-indexing.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices"><img alt="Référentiel GitHub d’exercice pratique" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Utilisation des procédures de chargement de ressources</div>
            <p style="margin:1rem 0">
                Découvrez comment définir et affecter des profils de traitement AEM Assets à des dossiers et charger des ressources vers AEM à l’aide du module d’interface de ligne de commande npm "aem-upload".
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session8-assets#cloud-acceleration-bootcamp---session-8-assets-and-microservices" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Tester la gestion des ressources</span>
            </a>
        </td>
    </tr>
</table>
