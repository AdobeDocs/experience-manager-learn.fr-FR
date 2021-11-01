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
source-git-commit: 3657e7798774f9cc673ff6ccd8af1a555b1d4013
workflow-type: tm+mt
source-wordcount: '325'
ht-degree: 9%

---


# Outils de modernisation d’AEM

Découvrez comment les outils de modernisation d’AEM sont utilisés pour mettre à niveau un contenu AEM Sites existant afin d’être AEM compatible as a Cloud Service et de s’aligner sur les bonnes pratiques.

>[!VIDEO](https://video.tv.adobe.com/v/336965/?quality=12&learn=on)

## Utilisation des outils de modernisation d’AEM

![Cycle de vie des outils de modernisation d’AEM](./assets/aem-modernization-tools.png)

Les outils de modernisation d’AEM convertissent automatiquement les pages AEM existantes composées de modèles statiques hérités, de composants de base et de parsys afin d’utiliser des approches modernes telles que les modèles modifiables, les composants WCM principaux et les conteneurs de mise en page.

### Principales activités

+ Cloner AEM production 6.x pour exécuter AEM outils de modernisation par rapport à
+ Téléchargez et installez le [les derniers outils de modernisation des AEM](https://github.com/adobe/aem-modernize-tools/releases/latest) sur le clone de production AEM 6.x via Package Manager

+ [Convertisseur de structure de page](https://opensource.adobe.com/aem-modernize-tools/pages/tools/page-structure.html) met à jour le contenu d’une page existante d’un modèle statique à un modèle modifiable mappé à l’aide de conteneurs de mise en page.
   + Définition de règles de conversion à l’aide de la configuration OSGi
   + Exécuter le convertisseur de structure de page par rapport aux pages existantes

+ [Convertisseur de composants](https://opensource.adobe.com/aem-modernize-tools/pages/tools/component.html) met à jour le contenu d’une page existante d’un modèle statique à un modèle modifiable mappé à l’aide de conteneurs de mise en page.
   + Définir des règles de conversion via des définitions de noeud JCR/XML
   + Exécutez l’outil de convertisseur de composants par rapport aux pages existantes.

+ [Importateur de stratégies](https://opensource.adobe.com/aem-modernize-tools/pages/tools/policy-importer.html) crée des stratégies à partir de la configuration de conception
   + Définition de règles de conversion à l’aide de définitions de noeud JCR/XML
   + Exécuter l’importateur de stratégies par rapport aux définitions de conception existantes
   + Application de stratégies importées à des composants et des conteneurs AEM

+ [Convertisseur de boîtes de dialogue](https://opensource.adobe.com/aem-modernize-tools/pages/tools/dialog.html) convertit les boîtes de dialogue de composants Classic (ExtJS) et CoralUI 2 en boîtes de dialogue de composants CoralUI 3 TouchUI.
   + Exécutez l’outil Dialog Converter par rapport aux boîtes de dialogue ExtJS ou Coral2 basées sur l’interface utilisateur existantes.
   + Synchronisation des boîtes de dialogue converties dans le référentiel Git

### Autres ressources 

+ [Téléchargement des outils de modernisation des AEM](https://github.com/adobe/aem-modernize-tools/releases/latest)
+ [Documentation sur les outils de modernisation d’AEM](https://opensource.adobe.com/aem-modernize-tools/)
+ [Astuces AEM - Présentation d’AEM Modernization Suite](https://helpx.adobe.com/experience-manager/kt/eseminars/gems/Introducing-the-AEM-Modernization-Suite.html)
