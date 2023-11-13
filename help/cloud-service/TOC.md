---
user-guide-title: Tutoriels d’Adobe Experience Manager as a Cloud Service
user-guide-description: Ensemble de tutoriels pour Adobe Experience Manager as a Cloud Service.
breadcrumb-title: Tutoriels d’AEM as a Cloud Service
solution: Experience Manager, Experience Manager as a Cloud Service
sub-product: Experience Manager as a Cloud Service
version: Cloud Service
team: TM
source-git-commit: bfc4d843c53373010ee04cfa590272cedea7a686
workflow-type: tm+mt
source-wordcount: '1058'
ht-degree: 99%

---


# Tutoriels d’Adobe Experience Manager as a Cloud Service {#cloud-service}

+ [Vue d’ensemble](./overview.md)
+ Essais AEM {#aem-trials}
   + [Images](./aem-trials/images.md)
+ Présentation d’AEM as a Cloud Service{#introduction}
   + [Qu’est-ce qu’AEM as a Cloud Service ?](./introduction/what-is-aem-as-a-cloud-service.md)
   + [Architecture](./introduction/architecture.md)
   + [Cloud Manager](./introduction/cloud-manager.md)
   + Stratégie et leadership de la pensée{#strategy}
      + [Experience Manager - Modèles et archétypes de gouvernance et de dotation en personnel](./introduction/experience-manager-governance-and-staffing-models.md)
      + [Comment générer la vitesse du contenu avec Adobe Experience Manager](./introduction/drive-content-velocity-for-sites.md)
      + [Accélérer la vitesse du contenu avec les systèmes de style AEM](./introduction/accelerate-content-velocity-aem.md)
+ Intégrations Experience Cloud{#integrations}
   + [Intégrations](./integrations/experience-cloud.md)
   + [Adobe Target](./integrations/target.md)
+ Technologie sous-jacente {#underlying-technology}
   + [Architecture AEM](./underlying-technology/introduction-architecture.md)
   + [OSGi](./underlying-technology/introduction-osgi.md)
   + [Référentiel de contenu Java](./underlying-technology/introduction-jcr.md)
   + [Sling](./underlying-technology/introduction-sling.md)
   + [Services de création et de publication](./underlying-technology/introduction-author-publish.md)
   + [Dispatcher](./underlying-technology/introduction-dispatcher.md)
+ Edge Delivery Services {#edge-delivery-services}
   + [Module externe AEM Assets Sidekick](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/edge-delivery-services/sidekick-plugin.html?lang=fr){target=_blank}
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
   + [SDK AEM local](./local-development-environment/aem-runtime.md)
   + [Outils du Dispatcher local](./local-development-environment/dispatcher-tools.md)
+ Développement{#developing}
   + Extensibilité{#extensibility}
      + App Builder{#app-builder}
         + [Générer un jeton d’accès](./developing/extensibility/app-builder/jwt-auth.md)
      + Extensibilité de l’interface utilisateur{#ui}
         + [Vue d’ensemble](./developing/extensibility/ui/overview.md)
         + [Projet Adobe Developer Console](./developing/extensibility/ui/adobe-developer-console-project.md)
         + [Initialiser l’application](./developing/extensibility/ui/app-initialization.md)
         + [Enregistrer l’extension](./developing/extensibility/ui/extension-registration.md)
         + [Boîte de dialogue modale](./developing/extensibility/ui/modal.md)
         + [Action Adobe I/O Runtime](./developing/extensibility/ui/runtime-action.md)
         + [Vérifier](./developing/extensibility/ui/verify.md)
         + [Déployer](./developing/extensibility/ui/deploy.md)
         + Fragments de contenu{#content-fragments}
            + [Vue d’ensemble](./developing/extensibility/ui/content-fragments/overview.md)
            + Exemples{#examples}
               + [Génération d’images IA](./developing/extensibility/ui/content-fragments/examples/console-image-generation-and-image-upload.md)
               + [Mise à jour en bloc des propriétés](./developing/extensibility/ui/content-fragments/examples/console-bulk-property-update.md)
               + [Colonnes de grille personnalisées](./developing/extensibility/ui/content-fragments/examples/custom-grid-columns.md)
               + [Exports au format XML](./developing/extensibility/ui/content-fragments/examples/editor-export-to-xml.md)
               + [Bouton de la barre d’outils de l’éditeur de texte enrichi](./developing/extensibility/ui/content-fragments/examples/editor-rte-toolbar.md)
               + [Widgets de l’éditeur de texte enrichi](./developing/extensibility/ui/content-fragments/examples/editor-rte-widget.md)
               + [Badges de l’éditeur de texte enrichi](./developing/extensibility/ui/content-fragments/examples/editor-rte-badges.md)
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
      + [Mettre en cache des variantes de page](./developing/advanced/variant-caching.md)
      + [Protection CSRF](./developing/advanced/csrf-protection.md)
      + [Espaces de noms personnalisés](./developing/advanced/custom-namespaces.md)
      + [Utilisateurs et utilisatrices de service](./developing/advanced/service-users.md)
      + [API d’images optimisées pour le web](./developing/advanced/web-optimized-image-delivery-java-apis.md)
   + Environnement de développement rapide{#rde}
      + [Vue d’ensemble](./developing/rde/overview.md)
      + [Configuration](./developing/rde/how-to-setup.md)
      + [Utilisation](./developing/rde/how-to-use.md)
      + [Cycle de vie de développement](./developing/rde/development-life-cycle.md)
   + [JavaDocs de l’API SDK AEM](https://javadoc.io/doc/com.adobe.aem/aem-sdk-api/latest/index.html){target=_blank}
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
   + [Redirections d’URL](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/administration/url-redirection.html?lang=fr){target=_blank}.
+ Mise en cache{#caching}
   + [Vue d’ensemble](./caching/overview.md)
   + [Service de publication AEM](./caching/publish.md)
   + [Service de création AEM](./caching/author.md)
   + [Analyse du ratio d’accès au cache du réseau CDN](./caching/cdn-cache-hit-ratio-analysis.md)
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
+ Sécurité {#security}
   + Règles de filtre de trafic, y compris les règles WAF{#traffic-filter-and-waf-rules}
      + [Vue d’ensemble](./security/traffic-filter-rules/overview.md)
      + [Configuration](./security/traffic-filter-rules/how-to-setup.md)
      + [Exemples et analyse des résultats](./security/traffic-filter-rules/examples-and-analysis.md)
      + [Bonnes pratiques](./security/traffic-filter-rules/best-practices.md)
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
      + [Outil de migration des workflows de ressources](./migration/cloud-acceleration-manager/asset-workflow-migration-tool.md)
      + [Naviguer dans Cloud Acceleration Manager](./migration/cloud-acceleration-manager/navigating.md)
      + [Utiliser Cloud Acceleration Manager](./migration/cloud-acceleration-manager/using.md)
+ [Fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-learn/content-fragments-console/overview.html?lang=fr){target=_blank}.
+ Forms{#forms}
   + Développer pour Forms as a Cloud Service{#developing-for-cloud-service}
      + [1 - Prise en main](./forms/developing-for-cloud-service/getting-started.md)
      + [2 - Installer IntelliJ](./forms/developing-for-cloud-service/intellij-set-up.md)
      + [3 - Configurer Git](./forms/developing-for-cloud-service/setup-git.md)
      + [4 - Synchroniser IntelliJ avec AEM](./forms/developing-for-cloud-service/intellij-and-aem-sync.md)
      + [5 - Créer un formulaire](./forms/developing-for-cloud-service/deploy-your-first-form.md)
      + [6 - Gestionnaire d’envoi personnalisé](./forms/developing-for-cloud-service/custom-submit-to-servlet.md)
      + [7 - Activer les composants du Portail Formulaires](./forms/developing-for-cloud-service/forms-portal-components.md)
      + [8 - Inclure Cloud Services et FDM](./forms/developing-for-cloud-service/azure-storage-fdm.md)
      + [9 - Configuration cloud basée sur le contexte](./forms/developing-for-cloud-service/context-aware-fdm.md)
      + [10 -Transmettre à Cloud Manager](./forms/developing-for-cloud-service/push-project-to-cloud-manager-git.md)
      + [11 - Déployer sur l’environnement de développement](./forms/developing-for-cloud-service/deploy-to-dev-environment.md)
      + [12 - Mettre à jour l’archétype Maven](./forms/developing-for-cloud-service/updating-project-archetype.md)
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
   + Service d’envoi personnalisé avec formulaire découplé{#custom-submit-headless-forms}
      + [1. Présentation](./forms/custom-submit-headless-forms/introduction.md)
      + [2 - Créer un service d’envoi personnalisé](./forms/custom-submit-headless-forms/custom-submit-service.md)
      + [3 - Afficher la réponse](./forms/custom-submit-headless-forms/handle-response-react-app.md)
   + AEM Forms et Analytics{#forms-and-analytics}
      + [Présentation](./forms/form-data-analytics/introduction.md)
      + [Créer des éléments de données](./forms/form-data-analytics/data-elements.md)
      + [Créer des règles](./forms/form-data-analytics/rules.md)
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
   + Enregistrer et reprendre le remplissage du formulaire{#prefill-azure-storage}
      + [1. Présentation](./forms/prefill-azure-storage/introduction.md)
      + [2 - Créer un composant Page](./forms/prefill-azure-storage/page-component.md)
      + [3 - Créer un modèle de formulaire adaptatif](./forms/prefill-azure-storage/associate-page-component.md)
      + [4 - Créer une intégration du stockage Azure](./forms/prefill-azure-storage/create-fdm.md)
      + [5 - Créer une intégration SendGrid](./forms/prefill-azure-storage/send-grid-fdm.md)
      + [6 - Créer un formulaire adaptatif](./forms/prefill-azure-storage/create-af.md)
      + [7 - Déployer les exemples de ressources](./forms/prefill-azure-storage/deploy-sample-assets.md)

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
   + Stocker les envois de formulaire sur un lecteur et dans SharePoint{#one-drive}
      + [Stocker les données de formulaire sur un lecteur](./forms/forms-cs-one-drive/store-form-submission-one-drive.md)
      + [Stocker les données de formulaire dans SharePoint](./forms/forms-cs-sharepoint/store-form-submission-in-sharepoint.md)
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

+ Tutoriels en plusieurs étapes{#multi-step-tutorials}
   + [Développement d’AEM Sites](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr){target=_blank}.
   + [GraphQL](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/graphql/overview.html?lang=fr){target=_blank}
   + [Éditeur de SPA (React)](https://experienceleague.adobe.com/docs/experience-manager-learn/spa-react-tutorial/overview.html?lang=fr){target=_blank}
   + [AEM Sites et Adobe Target](https://experienceleague.adobe.com/docs/experience-manager-learn/aem-target-tutorial/overview.html?lang=fr){target=_blank}
   + [Authentification basée sur les jetons](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/overview.html?lang=fr){target=_blank}
+ Ressources expertes {#expert-resources}
   + AEM Champions {#aem-champions}
      + [Playbook d’intégration de Cloud Manager](./expert-resources/aem-champions/onboarding-playbook.md)
      + [Types d’environnements Cloud Manager](./expert-resources/aem-champions/environment-types.md)
      + [Interface utilisateur de Cloud Manager](./expert-resources/aem-champions/cloud-manager-ui.md)
   + [AEM Experts Series](./expert-resources/expert-series/aem-experts-series.md)
   + Cloud 5{#cloud-5}
      + [Présentation](./expert-resources/cloud-5/cloud5-introduction.md)
      + [Saison 1](./expert-resources/cloud-5/cloud5-season-1.md)
      + [Saison 2](./expert-resources/cloud-5/cloud5-season-2.md)
      + [Réseau CDN AEM Partie 1](./expert-resources/cloud-5/cloud5-aem-cdn-part1.md)
      + [Réseau CDN AEM Partie 2](./expert-resources/cloud-5/cloud5-aem-cdn-part2.md)
      + [Fichiers journaux AEM](./expert-resources/cloud-5/cloud5-aem-log-files.md)
      + [Jetons de connexion](./expert-resources/cloud-5/cloud5-getting-login-token-integrations.md)
      + [Dispatcher Cloud](./expert-resources/cloud-5/cloud5-aem-dispatcher-cloud.md)
      + [Migration 1](./expert-resources/cloud-5/cloud5-aem-content-migration-part-1.md)
      + [Migration 2](./expert-resources/cloud-5/cloud5-aem-content-migration-part-2.md)
      + [Validateur du Dispatcher](./expert-resources/cloud-5/cloud5-aem-dispatcher-validator.md)
      + [Recherche et indexation](./expert-resources/cloud-5/cloud5-aem-search-and-indexing.md)
      + [Adobe App Builder](./expert-resources/cloud-5/cloud5-adobe-app-builder.md)
      + Saison 2{#season-2}
         + [Fragments](./expert-resources/cloud-5/season-2/cloud5-experience-v-content-fragments.md)
         + [Modernisateur de référentiel](./expert-resources/cloud-5/season-2/cloud5-repo-modernizer.md)
         + [Admin Console](./expert-resources/cloud-5/season-2/cloud5-admin-console.md)
         + [REPOINIT](./expert-resources/cloud-5/season-2/cloud5-repoinit.md)
         + [Planificateur de tâches Sling](./expert-resources/cloud-5/season-2/cloud5-sling-job-scheduler.md)
         + [Correction de votre cache](./expert-resources/cloud-5/season-2/cloud5-fix-your-cache.md)
         + [Correction de vos réécritures](./expert-resources/cloud-5/season-2/cloud5-fix-your-rewrites.md)
         + [Cloud Manager - Audit de l’expérience](./expert-resources/cloud-5/season-2/cloud5-mocm-experience-audit.md)
         + [Cloud Manager - Tests unitaires](./expert-resources/cloud-5/season-2/cloud5-mocm-unit-tests.md)
         + [Cloud Manager - Tests fonctionnels](./expert-resources/cloud-5/season-2/cloud5-mocm-functional-tests.md)
