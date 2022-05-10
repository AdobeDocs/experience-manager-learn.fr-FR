---
user-guide-title: Tutoriels d’Adobe Experience Manager as a Cloud Service
user-guide-description: Ensemble de tutoriels pour Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriels sur AEM as a Cloud Service
sub-product: cloud-service
team: TM
source-git-commit: 55583effd0400bac2e38756483d69f5bd114cb21
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 30%

---


# Tutoriels d’Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Présentation](./overview.md)
+ Présentation d’AEM as a Cloud Service{#introduction}
   + [Qu&#39;est-ce AEM as a Cloud Service ?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Evolution](./introduction/evolution.md)
   + [Architecture](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
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
   + Principes de développement{#basics}
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
      + [Nettoyage d’un projet Maven AEM](./developing/projects/remove-samples.md)
   + Services OSGi{#osgi-services}
      + [Concepts de base du service OSGi](./developing/osgi-services/basics.md)
      + [Cycle de vie des composants OSGi](./developing/osgi-services/lifecycle.md)
      + [Concepts de base des configurations OSGi](./developing/osgi-services/configurations.md)
      + [Configurations OSGi à l’aide d’OCD](./developing/osgi-services/configurations-ocd.md)
   + Avancé{#advanced}
      + [Utilisateurs du service](./developing/advanced/service-users.md)
   + [Documentation Java de l’API SDK AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ AEM de débogage{#debugging}
   + Débogage du SDK AEM{#debugging-aem-sdk}
      + [Présentation](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Journaux](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Débogage à distance](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Outils Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Autres outils](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Débogage AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Présentation](./debugging/cloud-service/overview.md)
      + [Journaux](./debugging/cloud-service/logs.md)
      + [Build et déploiement](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [Explorateur de référentiels](./debugging/cloud-service/repository-browser.md)
+ Accès aux AEM{#accessing}
   + [Présentation](./accessing/overview.md)
   + [Utilisateurs Adobe IMS](./accessing/adobe-ims-users.md)
   + [Groupes d’utilisateurs Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Profils de produit Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [AEM utilisateurs, groupes et autorisations](./accessing/aem-users-groups-and-permissions.md)
   + [Configuration de l’accès à la présentation d’AEM](./accessing/walk-through.md)
+ Mise en réseau avancée{#networking}
   + [Présentation](./networking/advanced-networking.md)
   + [Sortie de port flexible](./networking/flexible-port-egress.md)
   + [Adresse IP sortante dédiée](./networking/dedicated-egress-ip-address.md)
   + [Réseau privé virtuel](./networking/vpn.md)
   + Exemples de code{#examples}
      + [HTTP/HTTPS sur les ports non standard pour une sortie de port flexible](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS sur les ports non standard pour les adresses IP de sortie/VPN dédiées](./networking/examples/http-on-non-standard-ports.md)
      + [Connexions SQL à l’aide de DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Connexions SQL à l’aide des API Java SQL](./networking/examples/sql-java-apis.md)
      + [Service de messagerie électronique](./networking/examples/email-service.md)
+ Migration {#migration}
   + [Outil de transfert de contenu](./migration/content-transfer-tool.md)
   + [Importation en bloc de ressources](./migration/bulk-import.md)

   + Transition vers AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Présentation](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Intégration ](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
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
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
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

   + Développement pour Forms as a Cloud Service{#developing-for-cloud-service}
      + [Prise en main](./forms/developing-for-cloud-service/getting-started.md)
      + [Installation de IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [Configuration Git](./forms/developing-for-cloud-service/setup-git.md)
      + [Synchroniser IntelliJ avec AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [Création d’un formulaire](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [Activer les composants du Portail Formulaires](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [Inclure les Cloud Services et FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [Push to Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [Déploiement dans l’environnement de développement](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [Mise à jour de l’archétype Maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
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
   + Génération de documents dans AEM Forms CS{#doc-gen-formscs}
      + [Présentation](./forms/doc-gen-forms-cs/introduction.md)
      + [Création d’informations d’identification de service](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Création d’un jeton JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Créer un jeton d’accès](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Fusionner des données avec un modèle](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Tester la solution](./forms/doc-gen-forms-cs/test.md)
      + [Défi](./forms/doc-gen-forms-cs/challenge.md)
   + Génération de documents à l’aide de l’API de lot{#formscs-batch-api}
      + [Présentation](./forms/formscs-batch-api/introduction.md)
      + [Configuration du stockage Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Création d’une configuration de lot USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Créer une configuration de lot](./forms/formscs-batch-api/create-batch-config.md)
      + [Exécuter le lot](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipulation de PDF dans Forms CS{#forms-cs-assembler}
      + [Présentation](./forms/forms-cs-assembler/introduction.md)
      + [Création d’informations d’identification de service](./forms/forms-cs-assembler/service-credentials.md)
      + [Création d’un jeton JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Créer un jeton d’accès](./forms/forms-cs-assembler/create-access-token.md)
      + [Assemblage de fichiers de PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilitaires PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Tester la solution](./forms/forms-cs-assembler/test.md)
      + [Défi](./forms/forms-cs-assembler/challenge.md)
   + Stockage du portail Azure{#forms-cs-azure-portal}
      + [Présentation](./forms/forms-cs-azure-portal/introduction.md)
      + [Création d’un modèle de données de formulaire](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Stocker les données de formulaire dans Azure Storage](./forms/forms-cs-azure-portal/create-af.md)
      + [Formulaire de préremplissage](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Requête envoyée](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Créer un processus de révision{#create-aem-workflow}
      + [Externaliser le stockage des workflows](./forms/create-aem-workflow/externalize-workflow.md)
      + [Créer un modèle de workflow](./forms/create-aem-workflow/create-workflow.md)
      + [Workflow de déclenchement](./forms/create-aem-workflow/configure-af.md)
   + Adobe Sign avec AEM Forms{#forms-and-sign}
      + [Présentation](./forms/forms-and-sign/introduction.md)
      + [Application d’API Adobe Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuration cloud Adobe Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Créer un formulaire adaptatif](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configuration pour le remplissage et la signature](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Intégration à Microsoft Dynamics{#formscs-dynamics-crm}
      + [Création d’une application Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configuration de la source de données](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Création d’un modèle de données de formulaire](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Créer un formulaire adaptatif](./forms/formscs-dynamics-crm/create-adaptive-form.md)
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
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Développer{#develop}
      + [Création d’un projet d’Asset compute](./asset-compute/develop/project.md)
      + [Configuration des variables d’environnement](./asset-compute/develop/environment-variables.md)
      + [Configuration du fichier manifest.yml](./asset-compute/develop/manifest.md)
      + [Développement d’un worker](./asset-compute/develop/worker.md)
      + [Utilisation de l’outil de développement](./asset-compute/develop/development-tool.md)
   + Test et débogage{#test-debug}
      + [Tester un programme de travail](./asset-compute/test-debug/test.md)
      + [Débogage d’un programme de travail](./asset-compute/test-debug/debug.md)
   + Déploiement dʼ{#deploy}
      + [Déploiement sur Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Intégration à AEM](./asset-compute/deploy/processing-profiles.md)
   + Avancé{#advanced}
      + [Travailleurs de métadonnées](./asset-compute/advanced/metadata.md)
   + [Résolution des problèmes](./asset-compute/troubleshooting.md)
+ Cloud 5{#cloud-5}
   + [Présentation](./cloud-5/cloud5-introduction.md)
   + [Saison 1](./cloud-5/cloud5-season-1.md)
   + [AEM CDN Partie 1](./cloud-5/cloud5-aem-cdn-part1.md)
   + [AEM CDN Partie 2](./cloud-5/cloud5-aem-cdn-part2.md)
   + [AEM fichiers journaux](./cloud-5/cloud5-aem-log-files.md)
   + [Jetons de connexion](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Cloud Dispatcher](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [Migration 1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [Migration 2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Dispatcher Validator](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [Recherche et indexation](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe App Builder](./cloud-5/cloud5-adobe-app-builder.md)
+ [AEM série d’experts](./aem-experts-series.md)
+ Tutorials en plusieurs étapes{#multi-step-tutorials}
   + [Développement d’AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr)
   + [Éditeur SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html?lang=fr)
   + [Éditeur SPA (Angular)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-angular-tutorial/overview.html?lang=fr)
   + [AEM Sites et Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html)
   + [Authentification basée sur les jetons](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html)
