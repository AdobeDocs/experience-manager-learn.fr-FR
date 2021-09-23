---
user-guide-title: Tutoriels sur Adobe Experience Manager as a Cloud Service
user-guide-description: Ensemble de tutoriels pour Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriels sur AEM as a Cloud Service
sub-product: cloud-service
team: TM
source-git-commit: 598d00578e5179f76b6f309c5c14dc7b1634f051
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 33%

---


# Tutoriels sur Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Présentation](./overview.md)
+ Présentation d’AEM as a Cloud Service{#introduction}
   + [Qu&#39;est-ce qu&#39;AEM en tant que Cloud Service ?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolution](./introduction/evolution.md)
   + [Architecture](./introduction/architecture.md)
   + [Cloud Manager ](./introduction/cloud-manager.md)
+ Technologie sous-jacente {#underlying-technology}
   + [Architecture AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Référentiel de contenu Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Services de création et de publication](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Cloud Manager {#cloud-manager}
   + [Programmes](./cloud-manager/programs.md)
   + [Environnements](./cloud-manager/environments.md)
   + [Pipeline de production CI/CD](./cloud-manager/cicd-production-pipeline.md)
   + [Pipeline hors production CI/CD](./cloud-manager/cicd-non-production-pipeline.md)
   + [Activité](./cloud-manager/activity.md)
   + Ops de développement{#devops}
      + [Déploiement du code](./cloud-manager/devops/deploy-code.md)
      + [Fusion de projets](./cloud-manager/devops/merge-projects.md)
      + [Configuration des pipelines](./cloud-manager/devops/configure-pipelines.md)
      + [Intégration continue](./cloud-manager/devops/continuous-integration.md)
      + [Analyse des résultats des tests](./cloud-manager/devops/analyze-test-results.md)
      + [Configurations de Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [API de Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Configuration de l’environnement de développement local {#local-development-environment-set-up}
   + [Présentation](./local-development-environment/overview.md)
   + [Outils de développement](./local-development-environment/development-tools.md)
   + [Exécution locale AEM](./local-development-environment/aem-runtime.md)
   + [Outils du Dispatcher local](./local-development-environment/dispatcher-tools.md)
+ Développement{#developing}
   + Concepts de base du développement{#basics}
      + [AEM SDK](./developing/basics/aem-sdk.md)
      + [Environnement de développement local](./developing/basics/local-development-environment.md)
      + [Archétype de projet AEM](./developing/basics/aem-project-archetype.md)
      + [Structure de projet AEM](./developing/basics/project-structure.md)
      + [Contenu mutable ou non mutable](./developing/basics/mutable-immutable.md)
      + [Module de structure du référentiel](./developing/basics/repository-structure-package.md)
      + [Publication de contenu](./developing/basics/content-publishing.md)
      + [Configurations OSGi](./developing/basics/osgi-configurations.md)
      + [Migration de la configuration de Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Projets AEM{#aem-projects}
      + [Projet AEM Maven](./developing/projects/maven-project-structure.md)
   + Services OSGi{#osgi-services}
      + [Concepts de base du service OSGi](./developing/osgi-services/basics.md)
      + [Cycle de vie des composants OSGi](./developing/osgi-services/lifecycle.md)
      + [Concepts de base des configurations OSGi](./developing/osgi-services/configurations.md)
      + [Configurations OSGi à l’aide d’OCD](./developing/osgi-services/configurations-ocd.md)
   + [Documentation Java de l’API SDK AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ Débogage AEM{#debugging}
   + Débogage du SDK AEM{#debugging-aem-sdk}
      + [Présentation](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Journaux](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Débogage à distance](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Outils Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Autres outils](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Débogage d’AEM en tant que Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Présentation](./debugging/cloud-service/overview.md)
      + [Journaux](./debugging/cloud-service/logs.md)
      + [Build et déploiement](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [CRXDE Lite](./debugging/cloud-service/crxde-lite.md)
+ Accès à AEM{#accessing}
   + [Présentation](./accessing/overview.md)
   + [Adobe des utilisateurs IMS](./accessing/adobe-ims-users.md)
   + [Adobe des groupes d’utilisateurs IMS](./accessing/adobe-ims-user-groups.md)
   + [Adobe de profils de produits IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM utilisateurs, groupes et autorisations](./accessing/aem-users-groups-and-permissions.md)
   + [Configuration de l’accès à la présentation d’AEM](./accessing/walk-through.md)
+ Migration {#migration}
   + [Outil de transfert de contenu](./migration/content-transfer-tool.md)
   + [Importation en bloc de ressources](./migration/bulk-import.md)

   + Transition vers AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Présentation](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Intégration ](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager ](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA et CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Outils de modernisation d’AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernisation du référentiel](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservices Asset compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Recherche et indexation](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migration du contenu {#content-migration}
         + [Service d’importation en bloc](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Outil de transfert de contenu](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
      + [Résolution des problèmes](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      +  dʼAEM Forms as a Cloud Service{#aem-forms}
         + [Présentation](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Inscription numérique](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Communications](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Accelerated Manager {#cloud-acceleration-manager}
      + [Présentation](./migration/cloud-acceleration-manager/introduction.md)
      + [Readiness and Best Practice Analyzer](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Phase de mise en oeuvre](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Outil de transfert de contenu](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [Outils de refactorisation du code](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernizer du référentiel de code](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Convertisseur du Dispatcher](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Index Converter](./migration/cloud-acceleration-manager/index-converter.md)
      + [Migration des workflows de ressources Outil](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Navigation dans Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Utilisation de Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + Créer un formulaire adaptatif{#create-first-af}
      + [Présentation](./forms/create-first-af/introduction.md)
      + [Créer un thème](./forms/create-first-af/create-theme.md)
      + [Créer un modèle](./forms/create-first-af/create-template.md)
      + [Créer un fragment](./forms/create-first-af/create-fragments.md)
      + [Créer un formulaire](./forms/create-first-af/create-af.md)
      + [Configuration du panneau racine](./forms/create-first-af/configure-root-panel.md)
      + [Configuration du panneau Personnes](./forms/create-first-af/configure-people-panel.md)
      + [Configurer le panneau des revenus](./forms/create-first-af/configure-income-panel.md)
      + [Configuration du panneau Ressources](./forms/create-first-af/configure-assets-panel.md)
      + [Configuration du panneau de démarrage](./forms/create-first-af/configure-start-panel.md)
      + [Barre d’outils Ajouter et configurer](./forms/create-first-af/add-configure-toolbar.md)
   + API Document Cloud et AEM Forms CS{#doc-cloud-sdk}
      + [Présentation](./forms/doc-cloud-sdk/introduction.md)
      + [Créer un projet Adobe I/O](./forms/doc-cloud-sdk/create-document-cloud-credentials.md)
      + [Création d’une configuration OSGi](./forms/doc-cloud-sdk/create-doc-cloud-configuration.md)
      + [Définition de l’interface](./forms/doc-cloud-sdk/create-interface.md)
      + [Interface de mise en oeuvre](./forms/doc-cloud-sdk/implement-interface.md)
      + [Création d’une partie JSON](./forms/doc-cloud-sdk/get-content-analyzer.md)
      + [Étape de processus personnalisée](./forms/doc-cloud-sdk/custom-process-step.md)
   + Stockage du portail Azure{#forms-cs-azure-portal}
      + [Présentation](./forms/forms-cs-azure-portal/introduction.md)
      + [Création d’un modèle de données de formulaire](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Stocker les données de formulaire dans Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Formulaire de préremplissage](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Requête envoyée](./forms/forms-cs-azure-portal/query-submitted-data.md)


      + Créer un processus de révision{#create-aem-workflow}
         + [Créer un modèle de workflow](./forms/create-aem-workflow/create-workflow.md)
         + [Workflow de déclenchement](./forms/create-aem-workflow/configure-af.md)
      + Adobe Sign avec AEM Forms{#forms-and-sign}
         + [Présentation](./forms/forms-and-sign/introduction.md)
         + [Application d’API Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
         + [Configuration cloud Adobe Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
         + [Créer un formulaire adaptatif](./forms/forms-and-sign/create-adaptive-form.md)
         + [Configuration pour le remplissage et la signature](./forms/forms-and-sign/configure-form-fill-and-sign.md)
      + Intégration à Salesforce{#integrate-with-salesforce}
         + [Présentation](./forms/integrate-with-salesforce/introduction.md)
         + [Création d’une application connectée](./forms/integrate-with-salesforce/create-connected-app.md)
         + [Création d’un fichier swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
         + [Création d’une source de données](./forms/integrate-with-salesforce/create-data-source.md)
         + [Création d’un modèle de données de formulaire](./forms/integrate-with-salesforce/create-form-data-model.md)
         + [Test de l’envoi du formulaire](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
         + [Événement de clic de test](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ Extensibilité des Assets compute{#asset-compute}
   + [Présentation](./asset-compute/overview.md)
   + Configuration{#set-up}
      + [Approvisionnement des comptes et des services](./asset-compute/set-up/accounts-and-services.md)
      + [Environnement de développement local](./asset-compute/set-up/development-environment.md)
      + [Adobe Project Firefly](./asset-compute/set-up/firefly.md)
   + Develop{#develop}
      + [Création d’un projet d’Asset compute](./asset-compute/develop/project.md)
      + [Configuration des variables d’environnement](./asset-compute/develop/environment-variables.md)
      + [Configuration du fichier manifest.yml](./asset-compute/develop/manifest.md)
      + [Développement d’un worker](./asset-compute/develop/worker.md)
      + [Utilisation de l’outil de développement](./asset-compute/develop/development-tool.md)
   + Test et débogage{#test-debug}
      + [Tester un programme de travail](./asset-compute/test-debug/test.md)
      + [Débogage d’un programme de travail](./asset-compute/test-debug/debug.md)
   + Déploiement{#deploy}
      + [Déploiement sur Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Intégration à AEM](./asset-compute/deploy/processing-profiles.md)
   + Avancé{#advanced}
      + [Travailleurs de métadonnées](./asset-compute/advanced/metadata.md)
   + [Résolution des problèmes](./asset-compute/troubleshooting.md)
+ Tutorials en plusieurs étapes{#multi-step-tutorials}
   + [Développement d’AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr)
   + [Éditeur SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html)
   + [Éditeur SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html)
   + [AEM Sites et Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Authentification basée sur les jetons](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
