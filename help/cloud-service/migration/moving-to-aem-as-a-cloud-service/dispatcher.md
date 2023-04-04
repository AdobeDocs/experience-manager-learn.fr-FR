---
title: Configuration de Dispatcher lors du passage à AEM as a Cloud Service
description: Découvrez les modifications notables apportées à AEM Dispatcher pour AEM as a Cloud Service, l’outil de conversion de Dispatcher et comment utiliser le SDK des outils de Dispatcher.
version: Cloud Service
feature: Dispatcher
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8633
thumbnail: 336962.jpeg
exl-id: 81397b21-b4f3-4024-a6da-a9b681453eff
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 6%

---


# Dispatcher

Découvrez AEM Dispatcher pour AEM as a Cloud Service, en vous concentrant sur les modifications notables apportées par Dispatcher pour la version 6, l’outil de conversion de Dispatcher et sur l’utilisation du SDK des outils Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/336962?quality=12&learn=on)

## Convertisseur du Dispatcher

![Convertisseur du Dispatcher](./assets/dispatcher-converter-diagram.png)

Dans le cadre de la refactorisation de votre base de code, utilisez le [Convertisseur du Dispatcher AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/refactoring-tools/dispatcher-transformation-utility-tools.html) pour refactoriser les configurations Dispatcher On-Premise ou Adobe Managed Services existantes afin d’AEM la configuration Dispatcher compatible as a Cloud Service.

## Activités clés

+ Utilisez la variable [Outil convertisseur du Dispatcher Adobe I/O](https://github.com/adobe/aio-cli-plugin-aem-cloud-service-migration#aio-aem-migrationdispatcher-converter) pour migrer une configuration Dispatcher existante.
+ Référencez le module de Dispatcher à partir de [AEM Archétype de projet](https://github.com/adobe/aem-project-archetype/tree/develop/src/main/archetype/dispatcher.cloud) en tant que bonne pratique.
+ [Configuration des outils Dispatcher locaux](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/dispatcher-tools.html?lang=fr) pour valider le dispatcher, avant de le tester dans un environnement de Cloud Service.

## Exercice pratique

Appliquez vos connaissances en essayant ce que vous avez appris grâce à cet exercice pratique.

Avant d’essayer l’exercice pratique, assurez-vous d’avoir visionné et compris la vidéo ci-dessus, ainsi que les documents suivants :

+ [Outils de modernisation d’AEM](./aem-modernization-tools.md)
+ [Intégration](./onboarding.md)
+ [Cloud Manager](./cloud-manager.md)

Assurez-vous également que vous avez terminé l’exercice pratique précédent :

+ [Exercice pratique de Cloud Manager](./cloud-manager.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher"><img alt="Référentiel GitHub d’exercice pratique" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Utilisation des outils Dispatcher</div>
            <p style="margin:1rem 0">
                Explorez l’utilisation des outils Dispatcher du SDK AEM pour valider les configurations de Dispatcher et exécuter AEM Dispatcher localement à l’aide de Docker.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session5-dispatcher#cloud-acceleration-bootcamp---session-5-dispatcher" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Essai des outils de Dispatcher</span>
            </a>
        </td>
    </tr>
</table>
