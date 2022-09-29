---
title: Utilisation des outils de modernisation AEM pour passer à AEM as a Cloud Service
description: Découvrez comment les outils de modernisation d’AEM sont utilisés pour mettre à niveau un projet et un contenu AEM existant afin d’être as a Cloud Service.
version: Cloud Service
topic: Migration, Upgrade
role: Developer
level: Experienced
kt: 8629
thumbnail: 336965.jpeg
exl-id: 310f492c-0095-4015-81a4-27d76f288138
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '438'
ht-degree: 6%

---


# Outils de modernisation d’AEM

Découvrez comment les outils de modernisation d’AEM sont utilisés pour mettre à niveau un contenu AEM Sites existant afin d’être AEM compatible as a Cloud Service et de s’aligner sur les bonnes pratiques.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Utilisation des outils de modernisation d’AEM

![Cycle de vie des outils de modernisation d’AEM](./assets/aem-modernization-tools.png)

Les outils de modernisation d’AEM convertissent automatiquement les pages AEM existantes composées de modèles statiques hérités, de composants de base et de parsys afin d’utiliser des approches modernes telles que les modèles modifiables, les composants WCM principaux et les conteneurs de mise en page.

## Activités clés

+ Cloner AEM production 6.x pour exécuter AEM outils de modernisation par rapport à
+ Téléchargez et installez le [les derniers outils de modernisation des AEM](https://github.com/adobe/aem-modernize-tools/releases/latest) sur le clone de production AEM 6.x via Package Manager

+ [Convertisseur de structure de page](https://opensource.adobe.com/aem-modernize-tools/pages/structure/about.html) met à jour le contenu d’une page existante d’un modèle statique à un modèle modifiable mappé à l’aide de conteneurs de mise en page.
   + Définition de règles de conversion à l’aide de la configuration OSGi
   + Exécuter le convertisseur de structure de page par rapport aux pages existantes

+ [Convertisseur de composants](https://opensource.adobe.com/aem-modernize-tools/pages/component/about.html) met à jour le contenu d’une page existante d’un modèle statique à un modèle modifiable mappé à l’aide de conteneurs de mise en page.
   + Définir des règles de conversion via des définitions de noeud JCR/XML
   + Exécutez l’outil de convertisseur de composants par rapport aux pages existantes.

+ [Importateur de stratégies](https://opensource.adobe.com/aem-modernize-tools/pages/policy/about.html) crée des stratégies à partir de la configuration de conception
   + Définition de règles de conversion à l’aide de définitions de noeud JCR/XML
   + Exécuter l’importateur de stratégies par rapport aux définitions de conception existantes
   + Application de stratégies importées à des composants et des conteneurs AEM

## Exercice pratique

Appliquez vos connaissances en essayant ce que vous avez appris grâce à cet exercice pratique.

Avant d’essayer l’exercice pratique, assurez-vous d’avoir visionné et compris la vidéo ci-dessus, ainsi que les documents suivants :

+ [Penser différemment à AEM as a Cloud Service](./introduction.md)
+ [Modernisation du référentiel](./repository-modernization.md)
+ [Contenu mutable et non modifiable](../../developing/basics/mutable-immutable.md)
+ [AEM structure du projet](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/developing/aem-project-content-package-structure.html?lang=fr)

Assurez-vous également que vous avez terminé l’exercice pratique précédent :

+ [Exercice pratique BPA et CAM](./bpa-and-cam.md#hands-on-exercise)

<table style="border-width:0">
    <tr>
        <td style="width:150px">
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology"><img alt="Référentiel GitHub d’exercice pratique" src="./assets/github.png"/>
            </a>        
        </td>
        <td style="width:100%;margin-bottom:1rem;">
            <div style="font-size:1.25rem;font-weight:400;">Des mains en l'air pour la modernisation AEM</div>
            <p style="margin:1rem 0">
                Explorez l’utilisation des outils de modernisation d’AEM pour mettre à jour un site WKND hérité afin de vous conformer aux AEM bonnes pratiques as a Cloud Service.
            </p>
            <a  rel="noreferrer"
                target="_blank"
                href="https://github.com/adobe/aem-cloud-engineering-video-series-exercises/tree/session2-migration#bootcamp---session-2-migration-methodology" class="spectrum-Button spectrum-Button--primary spectrum-Button--sizeM">
                <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Testez les outils de modernisation AEM</span>
            </a>
        </td>
    </tr>
</table>

## Autres ressources

+ [Téléchargement des outils de modernisation des AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentation sur les outils de modernisation d’AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [Astuces AEM - Présentation d’AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)


1. Déployez le site wknd-legacy récemment modernisé sur le SDK AEM local. AEM Demander peut être téléchargé ici :
   + [Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html).
