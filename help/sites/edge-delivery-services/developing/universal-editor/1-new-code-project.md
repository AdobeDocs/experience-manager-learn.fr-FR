---
title: Créer un projet de code
description: Créez un projet de code pour Edge Delivery Services, modifiable à l’aide de l’éditeur universel.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: e1fb7a58-2bba-4952-ad53-53ecf80836db
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 1%

---

# Création d’un projet de code Edge Delivery Services

Pour créer des sites web AEM pour Edge Delivery Services et l’éditeur universel, utilisez le modèle de projet [AEM Boilerplate XWalk d’Adobe](https://github.com/adobe-rnd/aem-boilerplate-xwalk). Ce modèle crée un projet de code contenant le code CSS et JavaScript utilisé pour créer l’expérience du site web. Ce modèle permet de créer un référentiel GitHub et de le charger avec la configuration et le code standard d’Adobe, offrant ainsi une base solide à votre projet de site web AEM.

N’oubliez pas que les [sites web AEM diffusés par Edge Delivery Services](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/sites/edge-delivery-services/overview) n’ont que du code côté client (navigateur). Le code du site web n’est pas exécuté dans les services de création ou de publication AEM.

![Nouveau projet Edge Delivery Services](./assets/1-new-project/new-project.png)

Suivez les [étapes détaillées décrites dans la documentation](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) pour créer un projet de code Edge Delivery Services dont le contenu est modifiable dans l’éditeur universel.  Vous trouverez ci-dessous un résumé des étapes, y compris les valeurs utilisées dans ce tutoriel.

1. **Configuration d’un compte GitHub.** Si vous créez un projet pour votre organisation, assurez-vous qu’elle dispose d’un compte GitHub et que vous en êtes membre.
2. **Créez un projet de code** à l’aide du modèle de projet XWalk standard [AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).
3. **Installez l’application GitHub de synchronisation du code AEM** et accordez l’accès au référentiel. Vous pouvez trouver l’application [ ici](https://github.com/apps/aem-code-sync).
4. **Configurez la`fstab.yaml`** de votre nouveau projet pour pointer vers le service de création AEM approprié.

   * Pour effectuer des expériences, vous pouvez utiliser des environnements AEM as a Cloud Service inférieurs (évaluation ou développement), mais les implémentations de sites web réels doivent être configurées pour utiliser un service AEM de production.

5. **Modifiez le`paths.json`** de votre nouveau projet pour mapper le chemin du service de création AEM à la racine de votre site web.

Ce référentiel Git est cloné dans le chapitre [ Environnement de développement local ](https://experienceleague.adobe.com/fr/docs/experience-manager-learn/sites/edge-delivery-services/developing/universal-editor/3-local-development-environment) et à l’endroit où le code est développé.
