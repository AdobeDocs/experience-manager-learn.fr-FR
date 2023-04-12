---
user-guide-title: Tutoriels d’Adobe Experience Manager as a Cloud Service
user-guide-description: Ensemble de tutoriels pour Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriels d’AEM as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: 9917b16248ef1f0a9c86f03a024c634636b2304e
workflow-type: tm+mt
source-wordcount: '913'
ht-degree: 98%

---


# Tutoriels d’Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Présentation](./overview.md)
+ Présentation d’AEM as a Cloud Service{#introduction}
   + [Qu’est-ce qu’AEM as a Cloud Service ?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Évolution](./introduction/evolution.md)
   + [Architecture](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Stratégie et leadership de la pensée{#strategy}
      + [Experience Manager - Modèles et archétypes de gouvernance et de dotation en personnel](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Comment générer la vitesse du contenu avec Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
      + [Accélérer la vitesse du contenu avec les systèmes de style AEM](./introduction/accelerate-content-velocity-aem.md)
+ [Intégrations Experience Cloud](./experience-cloud/integrations.md)
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
   + [Activity](./cloud-manager/activity.md)
   + Opérations de développement{#devops}
      + [Déployer le code](./cloud-manager/devops/deploy-code.md)
      + [Fusionner des projets](./cloud-manager/devops/merge-projects.md)
      + [Configurer des pipelines](./cloud-manager/devops/configure-pipelines.md)
      + [Intégration continue](./cloud-manager/devops/continuous-integration.md)
      + [Analyser des résultats de test](./cloud-manager/devops/analyze-test-results.md)
      + [Configurations du Dispatcher](./cloud-manager/devops/dispatcher-configurations.md)
      + [API Cloud Manager](./cloud-manager/devops/cloud-manager-apis.md)
+ Configuration de l’environnement de développement local {#local-development-environment-set-up}
   + [Présentation](./local-development-environment/overview.md)
   + [Outils de développement](./local-development-environment/development-tools.md)
   + [Exécution locale d’AEM](./local-development-environment/aem-runtime.md)
   + [Outils du Dispatcher local](./local-development-environment/dispatcher-tools.md)
+ Développement{#developing}
   + Extensibilité{#extensibility}
      + App Builder{#app-builder}
         + [Générer un jeton d’accès](./developing/extensibility/app-builder/jwt-auth.md)
      + Console Fragments de contenu{#content-fragments}
         + [Présentation](./developing/extensibility/content-fragments/overview.md)
         + [Projet Adobe Developer Console](./developing/extensibility/content-fragments/adobe-developer-console-project.md)
         + [Initialisation de l’application](./developing/extensibility/content-fragments/app-initialization.md)
         + [Enregistrement d’une extension](./developing/extensibility/content-fragments/extension-registration.md)
         + [Menu d’en-tête](./developing/extensibility/content-fragments/header-menu.md)
         + [Barre d’actions](./developing/extensibility/content-fragments/action-bar.md)
         + [Boîte de dialogue modale](./developing/extensibility/content-fragments/modal.md)
         + [Action Adobe I/O Runtime](./developing/extensibility/content-fragments/runtime-action.md)
         + [Tester](./developing/extensibility/content-fragments/test.md)
         + [Déployer](./developing/extensibility/content-fragments/deploy.md)
         + Exemples d’extensions{#example-extensions}
            + [Mise à jour en bloc des propriétés](./developing/extensibility/content-fragments/example-extensions/bulk-property-update.md)
            + [Génération des ressources d’image AEM à l’aide d’OpenAI](./developing/extensibility/content-fragments/example-extensions/image-generation-and-image-upload.md)
   + Principes de base de développement{#basics}
      + [SDK AEM](./developing/basics/aem-sdk.md)
      + [Environnement de développement local](./developing/basics/local-development-environment.md)
      + [Archétype de projet AEM](./developing/basics/aem-project-archetype.md)
      + [Structure de projet AEM](./developing/basics/project-structure.md)
      + [Contenu modifiable et non modifiable](./developing/basics/mutable-immutable.md)
      + [Package de structure du référentiel](./developing/basics/repository-structure-package.md)
      + [Publication de contenu](./developing/basics/content-publishing.md)
      + [Configurations OSGi](./developing/basics/osgi-configurations.md)
      + [Migration de la configuration du Dispatcher](./developing/basics/dispatcher-configuration.md)
   + Projets AEM{#aem-projects}
      + [Projet Maven AEM](./developing/projects/maven-project-structure.md)
      + [Nettoyer un projet Maven AEM](./developing/projects/remove-samples.md)
   + Services OSGi{#osgi-services}
      + [Principes de base du service OSGi](./developing/osgi-services/basics.md)
      + [Cycle de vie des composants OSGi](./developing/osgi-services/lifecycle.md)
      + [Principes de base des configurations OSGi](./developing/osgi-services/configurations.md)
      + [Configurations OSGi à l’aide d’OCD](./developing/osgi-services/configurations-ocd.md)
   + Avancé{#advanced}
      + [API d’images optimisées pour le web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
      + [Utilisateurs et utilisatrices de service](./developing/advanced/service-users.md)
      + [Espaces de noms personnalisés](./developing/advanced/custom-namespaces.md)
      + [Mettre en cache des variantes de page](./developing/advanced/variant-caching.md)
   + Environnement de développement rapide{#rde}
      + [Présentation](./developing/rde/overview.md)
      + [Configuration](./developing/rde/how-to-setup.md)
      + [Utilisation](./developing/rde/how-to-use.md)
      + [Cycle de vie du développement](./developing/rde/development-life-cycle.md)
   + [Documentation JavaDocs de l’API SDK AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html)
+ Déboguer AEM{#debugging}
   + Déboguer le SDK AEM{#debugging-aem-sdk}
      + [Présentation](./debugging/aem-sdk-local-quickstart/overview.md)
      + [Journaux](./debugging/aem-sdk-local-quickstart/logs.md)
      + [Débogage à distance](./debugging/aem-sdk-local-quickstart/remote-debugging.md)
      + [Console web OSGi](./debugging/aem-sdk-local-quickstart/osgi-web-consoles.md)
      + [Outils Dispatcher](./debugging/aem-sdk-local-quickstart/dispatcher-tools.md)
      + [Autres outils](./debugging/aem-sdk-local-quickstart/other-tools.md)
   + Déboguer AEM as a Cloud Service{#debugging-aem-as-a-cloud-service}
      + [Présentation](./debugging/cloud-service/overview.md)
      + [Journaux](./debugging/cloud-service/logs.md)
      + [Version et déploiement](./debugging/cloud-service/build-and-deployment.md)
      + [Developer Console](./debugging/cloud-service/developer-console.md)
      + [Navigateur de référentiel](./debugging/cloud-service/repository-browser.md)
      + Risques{#risks}
         + [Avertissements transversaux](./debugging/cloud-service/risks/traversals.md)
+ Diffusion de contenu{#content-delivery}
   + [Redirections d’URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html?lang=fr)
+ Accéder à AEM{#accessing}
   + [Présentation](./accessing/overview.md)
   + [Utilisateurs Adobe IMS](./accessing/adobe-ims-users.md)
   + [Groupes d’utilisateurs Adobe IMS](./accessing/adobe-ims-user-groups.md)
   + [Profils de produit Adobe IMS](./accessing/adobe-ims-product-profiles.md)
   + [Utilisateurs et utilisatrices, groupes et autorisations AEM](./accessing/aem-users-groups-and-permissions.md)
   + [Présentation de la configuration de l’accès à AEM](./accessing/walk-through.md)
+ Authentification{#authentication}
   + [Présentation](./authentication/authentication.md)
   + [SAML 2.0](./authentication/saml-2-0.md)
+ Mise en réseau avancée{#networking}
   + [Présentation](./networking/advanced-networking.md)
   + [Sortie de port flexible](./networking/flexible-port-egress.md)
   + [Adresse IP de sortie dédiée](./networking/dedicated-egress-ip-address.md)
   + [Réseau privé virtuel](./networking/vpn.md)
   + Exemples de code{#examples}
      + [HTTP/HTTPS sur les ports non standard pour une sortie de port flexible](./networking/examples/http-on-non-standard-ports-flexible-port-egress.md)
      + [HTTP/HTTPS pour l’adresse IP ou le VPN de sortie dédié(e)](./networking/examples/http-dedicated-egress-ip-vpn.md)
      + [Connexions SQL à l’aide de DataSourcePool](./networking/examples/sql-datasourcepool.md)
      + [Connexions SQL à l’aide des API Java SQL](./networking/examples/sql-java-apis.md)
      + [Service E-mail](./networking/examples/email-service.md)
+ Migration {#migration}
   + [Outil de transfert de contenu](./migration/content-transfer-tool.md)
   + [Importation en bloc de ressources](./migration/bulk-import.md)
   + Transition vers AEM as a Cloud Service {#moving-to-aem-as-a-cloud-service}
      + [Présentation](./migration/moving-to-aem-as-a-cloud-service/introduction.md)
      + [Intégration](./migration/moving-to-aem-as-a-cloud-service/onboarding.md)
      + [Cloud Manager](./migration/moving-to-aem-as-a-cloud-service/cloud-manager.md)
      + [BPA et CAM](./migration/moving-to-aem-as-a-cloud-service/bpa-and-cam.md)
      + [Outils de modernisation AEM](./migration/moving-to-aem-as-a-cloud-service/aem-modernization-tools.md)
      + [Modernisation du référentiel](./migration/moving-to-aem-as-a-cloud-service/repository-modernization.md)
      + [Microservices Asset Compute](./migration/moving-to-aem-as-a-cloud-service/asset-compute-microservices.md)
      + [Dispatcher](./migration/moving-to-aem-as-a-cloud-service/dispatcher.md)
      + [Recherche et indexation](./migration/moving-to-aem-as-a-cloud-service/search-and-indexing.md)
      + Migration du contenu {#content-migration}
         + [Service d’importation en bloc](./migration/moving-to-aem-as-a-cloud-service/content-migration/bulk-import-service.md)
         + [Outil de transfert de contenu](./migration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.md)
         + [FAQ](./migration/moving-to-aem-as-a-cloud-service/content-migration/faq.md)
      + [Résolution des problèmes](./migration/moving-to-aem-as-a-cloud-service/troubleshooting.md)
      + dʼAEM Forms as a Cloud Service {#aem-forms}
         + [Présentation](./migration/moving-to-aem-as-a-cloud-service/aem-forms/introduction.md)
         + [Inscription numérique](./migration/moving-to-aem-as-a-cloud-service/aem-forms/digital-enrollment.md)
         + [Communications](./migration/moving-to-aem-as-a-cloud-service/aem-forms/communications.md)
   + Cloud Acceleration Manager {#cloud-acceleration-manager}
      + [Présentation](./migration/cloud-acceleration-manager/introduction.md)
      + [Personne chargée de l’analyse de l’état de préparation et des bonnes pratiques](./migration/cloud-acceleration-manager/readiness-and-best-practice-analyzer.md)
      + [Phase dʼimplémentation](./migration/cloud-acceleration-manager/implementation-phase.md)
      + [Outil de transfert de contenu](./migration/cloud-acceleration-manager/content-transfer-tool.md)
      + [Outils de refactorisation du code](./migration/cloud-acceleration-manager/code-refactoring-tools.md)
      + [Modernisateur du référentiel de code](./migration/cloud-acceleration-manager/code-repository-modernizer.md)
      + [Convertisseur du Dispatcher](./migration/cloud-acceleration-manager/dispatcher-converter.md)
      + [Index Converter](./migration/cloud-acceleration-manager/index-converter.md)
      + [Migration des workflows de ressources  Outil](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Naviguer dans Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Utiliser Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ Forms{#forms}
   + Développer pour Forms as a Cloud Service{#developing-for-cloud-service}
      + [Prise en main](./forms/developing-for-cloud-service/getting-started.md)
      + [Installer IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [Configurer Git](./forms/developing-for-cloud-service/setup-git.md)
      + [Synchroniser IntelliJ avec AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [Créer un formulaire](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [Activer les composants du Portail Formulaires](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [Inclure Cloud Services et FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [Configuration cloud basée sur le contexte](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [Transmettre à Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [Déployer sur l’environnement de développement](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [Mettre à jour l’archétype Maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
   + Créer un formulaire adaptatif{#create-first-af}
      + [Présentation](./forms/create-first-af/introduction.md)
      + [Créer un thème](./forms/create-first-af/create-theme.md)
      + [Créer un modèle](./forms/create-first-af/create-template.md)
      + [Créer un fragment](./forms/create-first-af/create-fragments.md)
      + [Créer un formulaire](./forms/create-first-af/create-af.md)
      + [Configurer le panneau Racine](./forms/create-first-af/configure-root-panel.md)
      + [Configurer le panneau Personnes](./forms/create-first-af/configure-people-panel.md)
      + [Configurer le panneau Revenus](./forms/create-first-af/configure-income-panel.md)
      + [Configurer le panneau Ressources](./forms/create-first-af/configure-assets-panel.md)
      + [Configurer le panneau Démarrage](./forms/create-first-af/configure-start-panel.md)
      + [Ajouter et configurer la barre d’outils](./forms/create-first-af/add-configure-toolbar.md)
   + AEM Forms et Analytics{#forms-and-analytics}
      + [Présentation](./forms/form-data-analytics/introduction.md)
      + [Création d’éléments de données](./forms/form-data-analytics/data-elements.md)
      + [Création de règles](./forms/form-data-analytics/rules.md)
      + [Tester la solution](./forms/form-data-analytics/test.md)
   + Génération de documents dans AEM Forms CS{#doc-gen-formscs}
      + [Présentation](./forms/doc-gen-forms-cs/introduction.md)
      + [Créer des informations d’identification de service](./forms/doc-gen-forms-cs/service-credentials.md)
      + [Créer un jeton JWT](./forms/doc-gen-forms-cs/create-jwt.md)
      + [Créer un jeton d’accès](./forms/doc-gen-forms-cs/create-access-token.md)
      + [Fusionner des données avec le modèle](./forms/doc-gen-forms-cs/merge-data-with-template.md)
      + [Tester la solution](./forms/doc-gen-forms-cs/test.md)
      + [Difficulté](./forms/doc-gen-forms-cs/challenge.md)
   + Génération de documents à l’aide de l’API par lot{#formscs-batch-api}
      + [Présentation](./forms/formscs-batch-api/introduction.md)
      + [Configurer le stockage Azure](./forms/formscs-batch-api/configure-azure-storage.md)
      + [Créer une configuration par lot USC](./forms/formscs-batch-api/configure-usc-batch.md)
      + [Créer une configuration par lot](./forms/formscs-batch-api/create-batch-config.md)
      + [Exécuter le lot](./forms/formscs-batch-api/execute-batch-generate-documents.md)
   + Manipulation de PDF dans Forms CS{#forms-cs-assembler}
      + [Présentation](./forms/forms-cs-assembler/introduction.md)
      + [Créer des informations d’identification de service](./forms/forms-cs-assembler/service-credentials.md)
      + [Créer un jeton JWT](./forms/forms-cs-assembler/create-jwt.md)
      + [Créer un jeton d’accès](./forms/forms-cs-assembler/create-access-token.md)
      + [Assembler des fichiers PDF](./forms/forms-cs-assembler/assemble-pdf-files.md)
      + [Utilitaires PDF/A](./forms/forms-cs-assembler/pdfa-utilities.md)
      + [Tester la solution](./forms/forms-cs-assembler/test.md)
      + [Difficulté](./forms/forms-cs-assembler/challenge.md)
   + Stockage de portail Azure{#forms-cs-azure-portal}
      + [Présentation](./forms/forms-cs-azure-portal/introduction.md)
      + [Créer un modèle de données de formulaire](./forms/forms-cs-azure-portal/create-fdm.md)
      + [Stocker les données de formulaire dans le stockage Azure](./forms/forms-cs-azure-portal/create-af.md)
      + [Préremplir un formulaire](./forms/forms-cs-azure-portal/prefill-af-storage.md)
      + [Envois de requêtes](./forms/forms-cs-azure-portal/query-submitted-data.md)
   + Créer un processus d’analyse{#create-aem-workflow}
      + [Externaliser le stockage des workflows](./forms/create-aem-workflow/externalize-workflow.md)
      + [Créer un modèle de workflow](./forms/create-aem-workflow/create-workflow.md)
      + [Déclencher un workflow](./forms/create-aem-workflow/configure-af.md)
   + Acrobat Sign avec AEM Forms{#forms-and-sign}
      + [Présentation](./forms/forms-and-sign/introduction.md)
      + [Application d’API Acrobat Sign](./forms/forms-and-sign/create-sign-api-application.md)
      + [Configuration cloud d’Acrobat Sign](./forms/forms-and-sign/create-adobe-sign-cloud-configuration.md)
      + [Créer un formulaire adaptatif](./forms/forms-and-sign/create-adaptive-form.md)
      + [Configurer l’outil Remplir et signer](./forms/forms-and-sign/configure-form-fill-and-sign.md)
   + Intégrer à Microsoft Power Automate{#forms-cs-and-power-automate}
      + [Configurer l’intégration](./forms/forms-cs-and-power-automate/integrate-formscs-power-automate.md)
      + [Analyser les données de formulaire envoyées](./forms/forms-cs-and-power-automate/send-email-notification.md)
      + [Envoyer un document d’enregistrement en tant que pièce jointe d’un e-mail](./forms/forms-cs-and-power-automate/send-dor-email-attachment.md)
      + [Extraire les pièces jointes de formulaire des données envoyées](./forms/forms-cs-and-power-automate/send-af-attachments-in-email.md)
   + Intégrer à Microsoft Dynamics{#formscs-dynamics-crm}
      + [Créer une application Dynamics](./forms/formscs-dynamics-crm/create-dynamics-account.md)
      + [Configurer la source de données](./forms/formscs-dynamics-crm/configure-odata-data-source.md)
      + [Créer un modèle de données de formulaire](./forms/formscs-dynamics-crm/create-form-data-model.md)
      + [Créer un formulaire adaptatif](./forms/formscs-dynamics-crm/create-adaptive-form.md)
   + Intégrer à Salesforce{#integrate-with-salesforce}
      + [Présentation](./forms/integrate-with-salesforce/introduction.md)
      + [Créer une application connectée](./forms/integrate-with-salesforce/create-connected-app.md)
      + [Créer un fichier Swagger](./forms/integrate-with-salesforce/describe-rest-api.md)
      + [Créer une source de données](./forms/integrate-with-salesforce/create-data-source.md)
      + [Créer un modèle de données de formulaire](./forms/integrate-with-salesforce/create-form-data-model.md)
      + [Tester un envoi de formulaire](./forms/integrate-with-salesforce/create-lead-submitting-form.md)
      + [Tester un événement de clic](./forms/integrate-with-salesforce/create-lead-click-event.md)
+ Extensibilité Assets Compute{#asset-compute}
   + [Présentation](./asset-compute/overview.md)
   + Configuration{#set-up}
      + [Approvisionnement des comptes et des services](./asset-compute/set-up/accounts-and-services.md)
      + [Environnement de développement local](./asset-compute/set-up/development-environment.md)
      + [App Builder](./asset-compute/set-up/app-builder.md)
   + Développer{#develop}
      + [Créer un projet Asset Compute](./asset-compute/develop/project.md)
      + [Configurer les variables d’environnement](./asset-compute/develop/environment-variables.md)
      + [Configurer le fichier manifest.yml](./asset-compute/develop/manifest.md)
      + [Développer un programme de travail](./asset-compute/develop/worker.md)
      + [Utiliser l’outil de développement](./asset-compute/develop/development-tool.md)
   + Tester et déboguer{#test-debug}
      + [Tester un programme de travail](./asset-compute/test-debug/test.md)
      + [Déboguer un programme de travail](./asset-compute/test-debug/debug.md)
   + Déployer{#deploy}
      + [Déployer sur Adobe I/O Runtime](./asset-compute/deploy/runtime.md)
      + [Intégrer à AEM](./asset-compute/deploy/processing-profiles.md)
   + Avancé{#advanced}
      + [Programmes de travail de métadonnées](./asset-compute/advanced/metadata.md)
   + [Résolution des problèmes](./asset-compute/troubleshooting.md)
+ Cloud 5{#cloud-5}
   + [Présentation](./cloud-5/cloud5-introduction.md)
   + [Saison 1](./cloud-5/cloud5-season-1.md)
   + [Saison 2](./cloud-5/cloud5-season-2.md)
   + [Réseau CDN AEM Partie 1](./cloud-5/cloud5-aem-cdn-part1.md)
   + [Réseau CDN AEM Partie 2](./cloud-5/cloud5-aem-cdn-part2.md)
   + [Fichiers journaux AEM](./cloud-5/cloud5-aem-log-files.md)
   + [Jetons de connexion](./cloud-5/cloud5-getting-login-token-integrations.md)
   + [Dispatcher Cloud](./cloud-5/cloud5-aem-dispatcher-cloud.md)
   + [Migration 1](./cloud-5/cloud5-aem-content-migration-part-1.md)
   + [Migration 2](./cloud-5/cloud5-aem-content-migration-part-2.md)
   + [Validateur du Dispatcher](./cloud-5/cloud5-aem-dispatcher-validator.md)
   + [Recherche et indexation](./cloud-5/cloud5-aem-search-and-indexing.md)
   + [Adobe App Builder](./cloud-5/cloud5-adobe-app-builder.md)
   + Saison 2{#season-2}
      + [Fragments](./cloud-5/season-2/cloud5-experience-v-content-fragments.md)
      + [Modernisateur de référentiel](./cloud-5/season-2/cloud5-repo-modernizer.md)
      + [Admin Console](./cloud-5/season-2/cloud5-admin-console.md)
      + [REPOINIT](./cloud-5/season-2/cloud5-repoinit.md)
      + [Planificateur de tâches Sling](./cloud-5/season-2/cloud5-sling-job-scheduler.md)
      + [Correction de votre cache](./cloud-5/season-2/cloud5-fix-your-cache.md)
      + [Correction de vos réécritures](./cloud-5/season-2/cloud5-fix-your-rewrites.md)
      + [Cloud Manager - Audit de l’expérience](./cloud-5/season-2/cloud5-mocm-experience-audit.md)
      + [Cloud Manager - Tests unitaires](./cloud-5/season-2/cloud5-mocm-unit-tests.md)
      + [Cloud Manager - Tests fonctionnels](./cloud-5/season-2/cloud5-mocm-functional-tests.md)
+ [AEM Experts Series](./aem-experts-series.md)
+ Tutoriels en plusieurs étapes{#multi-step-tutorials}
   + [Développement d’AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr)
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr)
   + [Éditeur de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html?lang=fr)
   + [AEM Sites et Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html?lang=fr)
   + [Authentification basée sur les jetons](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=fr)
