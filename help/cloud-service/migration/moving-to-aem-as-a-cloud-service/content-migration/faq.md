---
title: AEM FAQ sur la migration de contenu as a Cloud Service
description: Obtenez des réponses aux questions fréquentes sur la migration de contenu vers AEM as a Cloud Service.
version: Cloud Service
doc-type: article
feature: Migration
topic: Migration
role: Architect, Developer
level: Beginner
kt: 11200
thumbnail: kt-11200.jpg
source-git-commit: b2656329270ac90458dbc25bb05f39bf76921f26
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 1%

---


# AEM FAQ sur la migration de contenu as a Cloud Service

Obtenez des réponses aux questions fréquentes sur la migration de contenu vers AEM as a Cloud Service.

## Terminologie

+ **AEMaaCS**: [AEM as a Cloud Service](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/overview/introduction.html)
+ **BPA**: [Analyseur des bonnes pratiques](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/best-practices-analyzer/overview-best-practices-analyzer.html)
+ **CTT**: [Outil de transfert de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html)
+ **CAM**: [Cloud Accelerated Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-acceleration-manager/using-cam/getting-started-cam.html)
+ **IMS**: [Système Identity Management](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html)
+ **DM**: [Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/assets/dynamicmedia/dm-journey/dm-journey-part1.html)

Veuillez utiliser le modèle ci-dessous pour fournir plus de détails lors de la création de tickets d’assistance Adobe liés à CTT.

![Modèle de ticket de prise en charge de l’Adobe de migration de contenu](../../assets/faq/adobe-support-ticket-template.png) { align=&quot;center&quot; }

## Questions générales relatives à la migration de contenu

### Q : Quelles sont les différentes méthodes pour migrer le contenu vers AEM en tant que Cloud Services ?

Trois méthodes différentes sont disponibles.

+ Utilisation de l’outil de transfert de contenu (AEM 6.3+ → AEMaaCS)
+ Via le gestionnaire de modules (AEM AEMaaCS)
+ Service d’importation en bloc prêt à l’emploi pour Assets (S3/Azure → AEMaaCS)

### Q : Existe-t-il une limite à la quantité de contenu qui peut être transférée à l’aide du CTT ?

Nombre CTT en tant qu’outil peut être extrait d’AEM source et ingéré dans AEMaaCS. Cependant, il existe des limites spécifiques sur la plateforme AEMaaCS qui doivent être prises en compte avant la migration.

Pour plus d’informations, reportez-vous à la section [conditions préalables à la migration vers le cloud](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/prerequisites-content-transfer-tool.html).

### Q : J’ai le dernier rapport BPA de mon système source, que dois-je en faire ?

Exportez le rapport au format CSV, puis transférez-le vers Cloud Acceleration Manager, [associée à votre organisation IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/getting-started-cam.html). Suivez ensuite le processus de révision comme [présenté dans la phase de préparation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-acceleration-manager/using-cam/cam-readiness-phase.html).

Passez en revue l’évaluation de la complexité du code et du contenu fournie par l’outil et notez les éléments d’action associés qui entraînent la refactorisation du code ou l’évaluation de la migration vers le cloud.

### Q : Est-il recommandé de procéder à l’extraction sur l’auteur source et de l’ingérer dans AEMaaCS Author and Publish ?

Il est toujours recommandé d’effectuer une extraction et une ingestion 1:1 entre les niveaux Auteur et Publication. Cela dit, il est acceptable d’extraire l’auteur de production source et de l’ingérer dans le CS de développement, d’évaluation et de production.

### Q : Existe-t-il un moyen d’estimer le temps nécessaire pour migrer le contenu de l’AEM source vers AEMaaCS à l’aide de CTT ?

Étant donné que le processus de migration dépend de la largeur de bande Internet, du tas alloué pour le processus CTT, de la mémoire disponible et des E/S du disque qui sont soumis à chaque système source, il est recommandé d’exécuter le Bon à tirer des migrations rapidement et d’extrapoler les points de données à fournir avec des estimations.

### Q : Comment les performances de mon AEM source sont-elles affectées si je lance le processus d&#39;extraction de CTT ?

L’outil CTT s’exécute dans son propre processus Java™ qui prend jusqu’à 4 Go de tas, configurable via la configuration OSGi. Ce nombre peut changer, mais vous pouvez remplacer le processus Java™ et le découvrir.

Si AZCopy est installé et/ou que l’option de pré-copie/la fonction de validation est activée, le processus AZCopy consomme des cycles de processeur.

Outre jvm , l’outil utilise également les E/S de disque pour stocker les données sur un espace temporaire transitoire qui sera nettoyé après le cycle d’extraction. Outre la RAM, le processeur et les E/S de disque, l’outil CTT utilise également la largeur de bande réseau du système source pour charger des données dans le magasin Azure Blob.

La quantité de ressources nécessaire au processus d’extraction de CTT dépend du nombre de noeuds, du nombre de blobs et de leur taille agrégée. Il est difficile de fournir une formule. Il est donc recommandé d’exécuter un petit Bon à tirer de la migration pour déterminer les exigences de taille du serveur source.

Si des environnements de clonage sont utilisés pour la migration, cela n’a aucune incidence sur l’utilisation des ressources du serveur de production en direct, mais présente ses propres inconvénients en ce qui concerne la synchronisation de contenu entre la production en direct et le clone.

### Q : Dans mon système de création source, nous avons configuré la connexion unique pour que les utilisateurs s’authentifient dans l’instance d’auteur. Dois-je utiliser la fonctionnalité de mappage utilisateur de CTT dans ce cas ?

La réponse courte est &quot;**Oui**&quot;.

Extraction et ingestion du CTT **without** le mappage des utilisateurs migre uniquement le contenu, les principes associés (utilisateurs, groupes) de l’AEM source vers AEMaaCS. Toutefois, ces utilisateurs (identités) présents dans Adobe IMS doivent avoir (avec) accès à l’instance AEMaaCS pour s’authentifier correctement. La tâche de [outil de mappage des utilisateurs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) est de mapper l’utilisateur AEM local à l’utilisateur IMS afin que l’authentification et les autorisations fonctionnent ensemble.

Dans ce cas, le fournisseur d’identité SAML est configuré par rapport à Adobe IMS pour utiliser Federated/Enterprise ID plutôt que directement pour AEM à l’aide du gestionnaire d’authentification.

### Q : Dans mon système de création source, nous avons configuré une authentification de base pour que les utilisateurs s’authentifient dans l’instance d’auteur avec les utilisateurs AEM locaux. Dois-je utiliser la fonctionnalité de mappage utilisateur de CTT dans ce cas ?

La réponse courte est &quot;**Oui**&quot;.

L’extraction et l’ingestion CTT sans mappage utilisateur migre le contenu, les principes associés (utilisateurs, groupes) de l’AEM source vers AEMaaCS. Toutefois, ces utilisateurs (identités) présents dans Adobe IMS doivent avoir (avec) accès à l’instance AEMaaCS pour s’authentifier correctement. La tâche de [outil de mappage des utilisateurs](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/user-mapping-tool/overview-user-mapping-tool.html) est de mapper l’utilisateur AEM local à l’utilisateur IMS afin que l’authentification et les autorisations fonctionnent ensemble.

Dans ce cas, les utilisateurs utilisent Adobe ID personnel et Adobe ID est utilisé par l’administrateur IMS pour fournir l’accès à AEMaaCS.

### Q : Que signifient les termes &quot;essuyer&quot; et &quot;remplacer&quot; dans le contexte de la CTT ?

Dans le contexte de [phase d&#39;extraction](https://experienceleague.adobe.com/docs/experience-manager-cloud-servicemoving/cloud-migration/content-transfer-tool/extracting-content.html), les options sont soit de remplacer les données du conteneur d’évaluation des cycles d’extraction précédents, soit d’y ajouter la différence (ajoutée/mise à jour/suppression). Le conteneur d’évaluation n’est rien d’autre que le conteneur de stockage blob associé au jeu de migration. Chaque jeu de migration obtient son propre conteneur intermédiaire.

Dans le contexte de [phase d&#39;ingestion](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/moving/cloud-migration/content-transfer-tool/ingesting-content.html?lang=fr), les options sont + pour remplacer l’intégralité du référentiel de contenu d’AEMaaCS ou synchroniser le contenu différentiel (ajouté/mis à jour/supprimé) du conteneur de migration intermédiaire.

### Q : Il existe plusieurs sites web, ressources associées, utilisateurs et groupes dans le système source. Est-il possible de les migrer par phases vers AEMaaCS ?

Oui, cela est possible, mais nécessite une planification attentive concernant :

+ Création de jeux de migration en supposant que les sites soient des ressources dans leurs hiérarchies respectives
   + Vérifiez s’il est acceptable de migrer toutes les ressources dans le cadre d’un jeu de migration, puis d’importer les sites qui les utilisent par phases.
+ Dans l’état actuel, le processus d’ingestion par l’auteur rend l’instance d’auteur indisponible pour la création de contenu même si le niveau de publication peut toujours diffuser le contenu.
   + Cela signifie que jusqu’à ce que l’ingestion soit terminée dans l’auteur, les activités de création de contenu sont gelées.

Consultez le processus d’extraction et d’ingestion de complément comme documenté avant de planifier les migrations.

### Q : Mes sites web seront-ils disponibles pour les utilisateurs finaux même si l’ingestion se produit dans les instances d’auteur ou de publication AEMaaCS ?

Oui. Le trafic des utilisateurs finaux n’est pas interrompu par l’activité de migration de contenu. Cependant, l’ingestion par l’auteur gèle la création de contenu jusqu’à ce qu’elle se termine.

### Q : Le rapport BPA affiche les éléments liés aux rendus originaux manquants. Dois-je les nettoyer à la source avant de les extraire ?

Oui. Le rendu d’origine manquant signifie que le fichier binaire de la ressource n’est pas correctement chargé en premier lieu. En le considérant comme des données incorrectes, veuillez passer en revue et sauvegarder à l’aide de Package Manager (selon les besoins) et les supprimer de l’AEM source avant d’exécuter l’extraction. Les mauvaises données auront des résultats négatifs sur les étapes de traitement des ressources.

### Q : Le rapport BPA comporte des éléments liés à l’absence `jcr:content` pour les dossiers. Que devrais-je faire avec eux ?

When `jcr:content` est absent au niveau du dossier, toute action de propagation des paramètres tels que les profils de traitement, etc. des parents vont rompre à ce niveau. Veuillez consulter la raison de l&#39;absence `jcr:content`. Bien que ces dossiers puissent être migrés, notez que ces dossiers dégradent l’expérience utilisateur et provoquent des cycles de dépannage inutiles par la suite.

### Q : J’ai créé un jeu de migration. est-il possible de vérifier sa taille ?

Oui, il existe une [Vérifier la taille](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#migration-set-size) qui fait partie du CTT.

### Q : J’effectue la migration (extraction, ingestion). Est-il possible de valider que tout mon contenu extrait est ingéré dans la cible ?

Oui, il existe une [validation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/validating-content-transfers.html) qui fait partie de CTT.

### Q : Mon client doit déplacer du contenu entre les environnements AEMaaCS tels que du développement AEMaaCS à l’évaluation AEMaaCS ou à la production AEMaaCS. Puis-je utiliser l’outil de transfert de contenu pour ces cas pratiques ?

Malheureusement, non. Le cas d’utilisation de CTT consiste à migrer le contenu de la source AEM 6.3+ hébergée sur AMS/On-Premise vers les environnements cloud AEMaaCS. [Veuillez lire la documentation de CTT](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/overview-content-transfer-tool.html).

### Q : Quels types de problèmes sont anticipés lors de l&#39;extraction ?

La phase d’extraction est un processus impliqué qui nécessite que plusieurs aspects fonctionnent comme prévu. Tenir compte des différents types de problèmes qui peuvent se produire et comment les atténuer augmente le succès global de la migration de contenu.

La documentation publique est continuellement améliorée en fonction des enseignements tirés, mais voici quelques catégories de problèmes de haut niveau et d’éventuelles raisons sous-jacentes.

![AEM problèmes as a Cloud Service d’extraction de migration de contenu](../../assets/faq/extraction-issues.jpg) { align=&quot;center&quot; }

### Q : Quel type de problèmes sont anticipés pendant l’ingestion ?

La phase d’ingestion se produit complètement dans la plateforme cloud et nécessite l’aide des ressources ayant accès à l’infrastructure AEMaaCS. Pour obtenir de l’aide, créez un ticket d’assistance.

Voici les catégories de problèmes possibles (ne les considérez pas comme une liste exclusive)

![AEM problèmes d’ingestion de migration de contenu as a Cloud Service](../../assets/faq/ingestion-issues.jpg) { align=&quot;center&quot; }



### Q : Mon serveur source doit-il avoir une connexion Internet sortante pour que CTT fonctionne ?

La réponse courte est &quot;**Oui**&quot;.

Le processus CTT nécessite une connectivité aux ressources suivantes :

+ L’environnement AEM as a Cloud Service cible : `author-p<program_id>-e<env_id>.adobeaemcloud.com`
+ Le service d’enregistrement blob Azure : `casstorageprod.blob.core.windows.net`
+ Le point d’entrée de l’IO de mappage des utilisateurs : `usermanagement.adobe.io`

Reportez-vous à la documentation pour plus d’informations sur [connectivité source](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/migration-journey/cloud-migration/content-transfer-tool/getting-started-content-transfer-tool.html#source-environment-connectivity).

## Questions relatives au traitement des ressources dans Dynamic Media

### Q : Les ressources vont-elles être retraitées automatiquement après ingestion dans AEMaaCS ?

Nombre Pour traiter les ressources, la demande de retraitement doit être lancée.

### Q : Les ressources vont-elles être réindexées automatiquement après ingestion dans AEMaaCS ?

Oui. Les ressources sont réindexées en fonction des définitions d’index disponibles sur AEMaaCS.

### Q : L’AEM source dispose d’une intégration à Dynamic Media. Certains éléments spécifiques doivent-ils être pris en compte avant la migration du contenu ?

Oui, veuillez tenir compte des points suivants lorsque l’AEM source dispose de l’intégration Dynamic Media.

+ AEMaaCS prend uniquement en charge le mode Scene7 Dynamic Media. Si le système source est en mode hybride, la migration DM vers les modes Scene7 est requise.
+ Si l’approche consiste à migrer à partir d’instances de clone source, il est sans risque de désactiver l’intégration DM sur un clone qui serait utilisé pour CTT. Cette étape est uniquement destinée à éviter toute écriture sur DM ou le chargement sur le trafic DM.
+ Notez que CTT migre des noeuds, des métadonnées d’un jeu de migration de l’AEM source vers AEMaaCS. Il n’effectue aucune opération directement sur DM.

### Q : Quelles sont les différentes approches de migration lorsque l’intégration DM est présente sur l’AEM source ?

Veuillez lire la question et la réponse ci-dessus avant

(Il s’agit de deux options possibles, mais pas seulement de ces deux options). Cela dépend de la manière dont le client souhaite approcher l’UAT, les tests de performance, l’environnement disponible et si un clone est utilisé ou non pour la migration. Veuillez considérer ces deux-là comme un point de départ pour la discussion.

**Option 1**

Si le nombre de ressources/noeuds dans l’environnement source est inférieur (~100 K), en supposant qu’ils puissent être migrés sur une période de 24 + 72 heures, y compris l’extraction et l’ingestion, la meilleure approche consiste à

+ Effectuer directement la migration depuis la production
+ Exécution d’une extraction et d’une ingestion initiales dans AEMaaCS avec `wipe=true`
   + Cette étape migre tous les noeuds et les fichiers binaires
+ Continuer à travailler sur site/auteur de la production AMS
+ Désormais, exécutez tous les autres tests de cycle de migration avec `wipe=true`
   + Notez que cette opération migre l’entrepôt de noeuds complet, mais uniquement les objets blob modifiés par opposition aux objets blob complets. L’ensemble précédent de blobs se trouve dans le magasin Azure Blob de l’instance AEMaaCS cible.
   + Utilisez ce BAT de migrations pour mesurer la durée de migration, le mappage utilisateur, les tests et la validation de toutes les autres fonctionnalités.
+ Enfin, avant la semaine de mise en service, effectuez une migration wipe=true
   + Connexion de Dynamic Media à AEMaaCS
   + Déconnectez la configuration DM d’AEM source sur site

Avec cette option, vous pouvez exécuter la migration une à une, c’est-à-dire la fonction de développement On-premise → AEMaaCS Dev, etc. et déplacer les configurations DM des environnements respectifs

(Si la migration est planifiée à partir du cloud)

**Option 2**

+ Création d’un clone de l’auteur Production, suppression de la configuration DM du clone
+ Migrer le clone on-premise → AEMaaCS Dev/Stage
   + Connecter brièvement la société de gestion de production à AEMaaCS Dev/Stage à des fins de validation
   + Lorsque la connexion DM est principale, évitez l’ingestion des ressources dans AEMaaCS.
   + Cela leur permet de valider des validations spécifiques CTT et DM.
+ Une fois le test terminé sur AEMaaCS
   + Exécution d’une migration d’effacement de l’étape on-premise vers l’étape AEMaaCS

Exécutez une migration d’effacement depuis le développement on-premise vers le développement AEMaaCS.

L’approche ci-dessus peut être utilisée uniquement pour mesurer la durée de la migration, mais elle doit être nettoyée ultérieurement.

## Ressources supplémentaires

+ [Conseils et astuces pour migrer vers Experience Manager dans le cloud (Summit 2022)](https://business.adobe.com/summit/2022/sessions/tips-and-tricks-for-migrating-to-experience-manage-tw109.html)

+ [Vidéo des séries d’experts CTT](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-servicemigration/moving-to-aem-as-a-cloud-service/content-migration/content-transfer-tool.html)

+ [Vidéos de séries d’experts sur d’autres rubriques d’AEMaaCS](https://experienceleague.adobe.com/docs/experience-manager-learncloud-service/aem-experts-series.html)
