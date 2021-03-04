---
title: Comprendre les raisons de la mise à niveau
description: Ventilation de haut niveau des principales fonctionnalités pour les clients qui envisagent de mettre à niveau vers la dernière version de Adobe Experience Manager.
version: 6.5
sub-product: ressources, cloud-manager, commerce, content-services, dynamic-media, forms, foundation, screens, sites
topics: best-practices, upgrade
audience: all
activity: understand
doc-type: article
topic: Mise à niveau
role: Leader, Architecte, Développeur, Administrateur, Professionnel
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '3548'
ht-degree: 6%

---


# Comprendre les raisons de la mise à niveau

Ventilation de haut niveau des principales fonctionnalités pour les clients qui envisagent de mettre à niveau vers la dernière version de Adobe Experience Manager.

## Principales fonctionnalités pour la mise à niveau vers AEM 6.5

+ [Notes de mise à jour de Adobe Experience Manager 6.5](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes.html )

### Améliorations de la base

Adobe Experience Manager 6.5 continue d&#39;améliorer la stabilité, les performances et la supportabilité du système en :

+ **Prise en charge de Java 11**  (tout en conservant la prise en charge de Java 8).

### Création et gestion de sites Web

AEM Sites présente un certain nombre de fonctionnalités conçues pour accélérer la création et la création de sites Web :

+ **SPA** Editorsupport permet SPA (applications d’une seule page) d’être entièrement créées en AEM, ce qui permet une expérience de création riche et conviviale pour le marketing.
+_ **Les SDK JavaScript**, un Kit de Début de projet SPA et des outils de création de prise en charge, permettent aux développeurs frontaux de développer des applications de page unique compatibles SPA Éditeur indépendamment de l&#39;AEM.
+ **Les** composants principaux ajoutent une multitude de nouveaux composants, une  **bibliothèque de** composants ainsi qu&#39;une variété d&#39;améliorations aux composants principaux existants.
+ D&#39;autres **traductions** améliorations ont permis de rationaliser la traduction de AEM Sites.

### Expériences fluides

AEM continue d&#39;adopter les Expériences Fluid avec des outils nouveaux et améliorés qui facilitent l&#39;utilisation de contenu en dehors de l&#39;AEM.

+ **Les** fragments de contenu prennent en charge la comparaison de versions/différences et les annotations.
+ **AEM Assets HTTP** APIprend en charge l’exposition de  **fragments de** contenu directement dans la gestion des actifs numériques en tant que  **JSON**.
   **Les** fragments d’expérience prennent en charge  **la** non-validation du cache du répartiteur  **d’AEM de recherche** complète pour référencer  **les pages**.

### Gestion des ressources

AEM Assets continue de tirer parti de ses nombreuses capacités de gestion des actifs pour améliorer l&#39;utilisation, la gestion et la compréhension de la gestion des actifs numériques. AEM 6.5 continue d&#39;améliorer l&#39;intégration entre Adobe Creative Cloud et les workflows créatifs.

+ **Adobe Asset** Linkconnecte directement les créatifs à AEM Assets à partir des outils Adobe Creative Cloud.
+ **Adobe** Stockintegration permet un accès direct aux images Adobe Stock directement à partir de l’expérience AEM Assets, ce qui crée une expérience de découverte de contenu transparente.
+ **AEM Desktop** Apprelease la version 2.0 et se réinvente tout en améliorant les performances et la stabilité.
+ **Les** ressources connectées prennent en charge les instances AEM Sites discrètes pour accéder et utiliser facilement les ressources d’une autre instance AEM Assets.
+ Mise à jour de la prise en charge des vidéos dans **Dynamic Media**, y compris **360 Video** et **Miniatures vidéo personnalisées**.

### Intelligence du contenu

AEM continue de construire son intégration avec des technologies intelligentes, en exploitant l&#39;apprentissage automatique et l&#39;intelligence artificielle pour améliorer toutes les expériences.

+ **Adobe Asset** Linkajoute la recherche **de similarité visuelle, ce qui permet de découvrir et d’utiliser facilement des images similaires dans les outils**  **** Adobe Creative Cloud.

### Intégrations

AEM est plus à même de s&#39;intégrer à d&#39;autres services d&#39;Adobe :

+ **Les** fragments d’expérience approfondissent leur intégration avec le  **ciblage d’** Adobe en prenant en charge  **l’exportation en tant que** JSON vers Adobe Target et en permettant de  **supprimer les** offres basées sur le fragment d’expérience de Adobe Target.****

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), réservé aux clients Adobe Managed Services (AMS), offre les fonctionnalités suivantes :

+ Cloud Manager prend en charge l’extension de la prise en charge du déploiement AEM d’AEM Sites à **AEM Assets**, y compris **les tests de performances automatisés du traitement des ressources**.
+ **L’** évolutivité automatique du niveau de publication AEM à des seuils prédéfinis garantit une expérience optimale pour l’utilisateur final.
+ **Les** pipelines hors production permettent aux équipes de développement de tirer parti de Cloud Manager pour contrôler en permanence la qualité du code et les déployer vers des environnements inférieurs (développement et contrôle qualité).
+ **API de pipeline CI/CD** permet aux clients d’échanger par programmation avec Cloud Manager, ce qui renforce les possibilités d’intégration avec l’infrastructure de développement sur site.

## Fonctions de base

Vous trouverez ci-dessous une matrice des principales caractéristiques des fondations offertes par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions précédentes, avec des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour de AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***Améliorations <sup>+</sup> significatives de la fonctionnalité de cette version.***

***SPindique que la fonction est disponible via un Service Pack ou Feature Pack. <sup></sup>***

<table>
    <thead>
        <tr>
            <td>Fonction de base</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>
                <strong>Prise en charge de Java 11 : </strong> AEM prend en charge Java 11 (ainsi que Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a> : </strong> offre des performances et une évolutivité bien supérieures à celles du prédécesseur Jackrabbit 2.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Prise en charge</a> de l’index oak-run.jar : </strong> amélioration de la réindexation, de la collecte des statistiques et de la vérification de la cohérence des index Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Index</a> de recherche personnalisés :  </strong>
                Possibilité d’ajouter des définitions d’index personnalisées pour optimiser les performances des requêtes et la pertinence des recherches.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Nettoyage</a> de la révision en ligne : </strong>
                effectuez la maintenance du référentiel sans temps d’inactivité du serveur.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Enregistrement</a> de référentiel TarMK ou MongoMK : </strong>
                <br> Options pour utiliser un enregistrement de fichier simple et performant de TarMK (version de prochaine génération de TarPM) 
                <br> ou une mise à l'échelle horizontale avec un référentiel soutenu par MongoDB avec MongoMK.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Performances et stabilité</a> de MongoMK :Des améliorations </strong>
            continues ont été apportées à MongoMK depuis son introduction avec AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Amazon S3 DataStore</a> : </strong>
            tirez parti de la solution d’enregistrement de cloud extensible pour stocker des ressources binaires.</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Parité des fonctionnalités de l’interface utilisateur tactile : amélioration </strong>
                continue de l’interface utilisateur de création pour accélérer la productivité et la parité des fonctionnalités avec l’interface utilisateur classique.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Omnisearch : recherche et navigation </strong>
                rapides et AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Tableau de bord</a> d’exploitation : </strong>
 effectuez la maintenance, surveillez l’intégrité du serveur et analysez les performances à partir des AEM.</td>
            <td></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Améliorations</a> de la mise à niveau : les améliorations apportées à la </strong>
            mise à niveau permettent de mettre à niveau plus rapidement et plus facilement les AEM en place.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/htl/using/overview.html" target="_blank">Langage</a> de modèle HTML : </strong>
            Moteur de modèle moderne qui sépare la présentation de la logique. Réduit considérablement le temps de développement des composants. Ajout de fonctionnalités incrémentielles à chaque version.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modèles</a> Sling : </strong>
            une structure souple permettant de modéliser les ressources JCR en objets métier et en logique. Ajout de fonctionnalités incrémentielles à chaque version.
            </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a> :  </strong>
                Exclusif pour les clients Adobe Managed Services (AMS), Cloud Manager accélère le développement et le déploiement via un pipeline de CI/CD ultramoderne.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Fonctionnalités de sécurité

Vous trouverez ci-dessous une matrice des principaux dispositifs de sécurité offerts par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions précédentes, avec des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour sur la sécurité](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***► indique que des améliorations importantes ont été apportées à la fonctionnalité de cette version.***

***+<sup> </sup> indique que la fonction est disponible via un Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Fonction de sécurité</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Services </a></strong>
            <br> UsersCompartimentalise les autorisations évitent l’utilisation inutile des privilèges d’administrateur.</td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Key Store </a></strong>
            <br> ManagementTrust Store global, certificats et clés tous gérés dans le référentiel.</td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>Protection </strong> <strong></strong></a>
            <br> CSRFprotectionProtection multisite par usurpation de requête prête à l'emploi.</td>
        <td></td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong></strong> <strong></strong></a>
            <br> CORSsupportPrise en charge du partage de ressources entre Origines pour une plus grande flexibilité des applications.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://docs.adobe.com/docs/en/aem/6-5/administer/security/saml-2-0-authenticationhandler.html" target="_blank">Amélioration de la </a><br>
 </strong>prise en charge de l'authentification SAMLAmélioration de la redirection SAML, optimisation des informations de groupe et résolution des problèmes de chiffrement des clés. 
            <br>
        </td>
        <td></td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP en tant que </a><br>
 </strong>configuration OSGi Simplifie la gestion et les mises à jour de l’authentification LDAP.</td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong>La prise en charge du chiffrement OSGi pour les <br>
 </strong>mots de passe en texte brutLes mots de passe et les autres valeurs sensibles peuvent être enregistrés sous forme chiffrée et automatiquement déchiffrés.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Améliorations </a><br>
 </strong>du format CUG L’implémentation du groupe d’utilisateurs fermés a été réécrite afin de résoudre les problèmes de performances et d’évolutivité.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>►</td>
        <td><sup>+</sup></td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">SSL </a></strong>
            <br> WizardUI pour simplifier la configuration et la gestion de SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Prise en </a></strong>
            <br> charge des jetons encapsulésPlus nécessaire pour les sessions "collantes" afin de prendre en charge l’authentification horizontale entre les instances de publication.</td>
        <td> </td>
        <td> </td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
        <td>►</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Adobe de la </a><br>
 </strong>prise en charge de l’authentification IMSExclusive à Adobe Managed Services (AMS), gérez centralement l’accès aux instances d’auteur AEM via Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>►</td>
        <td>►</td>
    </tr>
</tbody>
</table>

## Fonctionnalités des sites

Vous trouverez ci-dessous une matrice des principales fonctionnalités des sites offertes par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions précédentes, avec des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour d’AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***Améliorations <sup>+</sup> significatives de la fonctionnalité de cette version.***

***SPindique que la fonction est disponible via un Service Pack ou Feature Pack. <sup></sup>***

<table>
    <thead>
        <tr>
            <td><strong>Fonctionnalité Sites</strong></td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Création</a> de pages optimisée pour les écrans tactiles : </strong>
            permet aux éditeurs d’exploiter les tablettes et les ordinateurs à l’aide d’écrans tactiles.</td>
            <td></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Création</a> de sites réactifs : </strong>
                le mode de mise en page permet aux éditeurs de redimensionner les composants en fonction de la largeur des périphériques pour les sites réactifs.</td>
            <td></td>
            <td></td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modèles</a> modifiables : </strong>
            permet aux auteurs spécialisés de créer et de modifier des modèles de page.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/core-components/user-guide.html" target="_blank">Composants</a> principaux : </strong>
            accélère le développement du site. Disponible sur GitHub pour un calendrier et une flexibilité de publication fréquents.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">SPA Éditeur</a> : </strong>
            créez des expériences Web autorisables et adaptables à l’aide de structures d’application sur une seule page (SPA) basées sur Réaction ou Angular.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/style-system-fp.html" target="_blank">Système</a> de style : </strong>
            augmentez la réutilisation du composant AEM en définissant leur apparence visuelle à l’aide du système de style contextuel.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Gestionnaire multisite (MSM)</a> : </strong>
            gérez plusieurs sites Web qui partagent du contenu commun (par exemple, plusieurs marques multilingues).</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduction</a> de contenu : </strong>
            Plug-and-Play framework s’intègre aux principaux services de traduction tiers du marché.</td>
            <td></td>
            <td></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a> : cadre contextuel client de </strong>
            nouvelle génération pour la personnalisation du contenu.</td>
            <td></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lancements</a> : </strong>
            développez du contenu pour une prochaine version sans perturber la création au quotidien.</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/content-fragments-feature-video-understand.html" target="_blank">Fragments</a> de contenu : </strong>
            créez et traitez du contenu éditorial dissocié de la présentation pour le réutiliser facilement.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragments</a> d’expérience : </strong>
            créez des expériences et des variations réutilisables optimisées pour les canaux de bureau, mobiles et sociaux.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/release-notes/content-services-fragments-featurepack.html" target="_blank">Content Services</a> : </strong>
            exportez du contenu de l’AEM au format JSON pour l’utiliser sur divers périphériques et applications.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Intégration Adobe Analytics et statistiques de contenu : intégration </strong>
                facile de Adobe Analytics et de la gestion dynamique des balises. Affichez les informations de performances dans l’environnement d’auteur.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Intégration</a> Adobe Target : assistant </strong>
            pas à pas pour créer des expériences ciblées, créer des bibliothèques d’offres réutilisables.</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Intégration</a> Adobe Campaign : intégration </strong>
            facile avec la solution de campagne par courrier électronique de prochaine génération.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Intégration</a> du lancement d’Adobe : </strong>
            intégration au service cloud de gestion des balises de prochaine génération de l’Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-screens-introduction.html" target="_blank">Ecrans</a> : </strong>
            gestion des expériences pour la signature numérique et les kiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a> : </strong>
            proposez des expériences d’achat personnalisées et de marque sur des points de contact Web, mobiles et sociaux.
            </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communautés</a> : </strong>
            Forums, commentaires liés, calendriers de événement et de nombreuses autres fonctionnalités permettent un engagement profond avec les visiteurs du site.</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Fonctionnalités des ressources

Vous trouverez ci-dessous une matrice des principales fonctionnalités d&#39;Actifs offertes par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions précédentes, avec des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour d’AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***► indique que des améliorations importantes ont été apportées à la fonctionnalité de cette version.***

***+<sup> </sup> indique que la fonction est disponible via un Service Pack ou Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Fonction Ressources</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface utilisateur</a> optimisée pour les écrans tactiles : </strong>
            gérez les fichiers sur un ordinateur de bureau ou sur des périphériques tactiles.</td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gestion</a> avancée des métadonnées : modèles de </strong>
            métadonnées, éditeur de Schéma de métadonnées et modification en masse des métadonnées.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank"></a> Tâche et  <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank"></a> WorkflowManagement : workflows et tâches </strong>
            prédéfinis pour la révision et l’approbation des ressources numériques exploitant des projets AEM.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Évolutivité et performances : prise en charge </strong>
            améliorée de l’assimilation, du transfert et de l’enregistrement à l’échelle.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API</a> HTTP des ressources : interagir </strong>
            par programmation avec les ressources via HTTP et JSON.</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Partage</a> de liens : partage ad hoc </strong>
            simple des ressources numériques sans avoir à se connecter.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Portail de marque</a> : solution SAAS du service </strong>
            Cloud pour un partage et une distribution transparents des ressources numériques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Ressources</a> connectées : les instances </strong>
            AEM Sites peuvent accéder et utiliser facilement des ressources d’une autre instance AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Informations sur les ressources</a> : </strong>
            tirez parti d’Adobe Analytics pour capturer l’interaction client des ressources et de la vue numériques en AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Ressources</a> multilingues : </strong>
            la traduction automatique des métadonnées de fichier avec les racines de langue.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Balises dynamiques et modération</a> : </strong>
            utilisez Adobe Sensei pour baliser automatiquement les images avec des métadonnées utiles.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Recherche</a> de traduction intelligente : </strong>
            Traduit automatiquement les termes de recherche lors de la recherche d’AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Intégration</a> Adobe InDesign Server : </strong>
            génération de catalogues de produits. Créez des brochures, des prospectus et des publicités imprimées à partir de modèles d’InDesign.</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=fr" target="_blank">AEM application</a> pour ordinateur de bureau : </strong>
            synchronisez les ressources sur le bureau local pour les modifier avec les produits Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Bibliothèque</a> d’images d’Adobe :</strong>
                <br> les bibliothèques Photoshop et Acrobat PDF utilisées pour la manipulation de fichiers de haute qualité.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Lien</a> de ressources d’Adobe : </strong>
            accédez à AEM Assets directement à partir des applications Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Intégration</a> Adobe Stock : permet d’</strong>
            accéder en toute transparence aux images Adobe Stock directement à partir de l’AEM et de les utiliser.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>SP</sup></td>
            <td>►</td>
        </tr>
    </tbody>
</table>

### AEM Assets

***Améliorations <sup>+</sup> significatives de la fonctionnalité de cette version.***

***SPindique que la fonction est disponible via un Service Pack ou Feature Pack. <sup></sup>***


<table>
    <thead>
        <tr>
            <td>Fonction Dynamic Media</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3 +FP</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imagerie</a> : </strong>
            diffuser dynamiquement des images dans différents formats et tailles, y compris Smart Crop.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vidéo</a> : codage vidéo </strong>
            avancé et diffusion en flux continu de la vidéo adaptative</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Supports</a> interactifs : </strong>
            créez des bannières interactives, des vidéos avec du contenu cliquable pour présenter des offres clés.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Visionneuses (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">image</a>,  <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">rotation</a>, supports <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank"> </a>variés) : </strong>
            permet aux utilisateurs de zoomer, de panoramique, de faire pivoter et de simuler une expérience d’affichage à 360 degrés.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://docs.adobe.com/docs/fr-FR/aem/6-5/administer/content/dynamic-media/viewer-presets.html" target="_blank">Visionneuses</a> : lecteurs de médias enrichis </strong>
            personnalisés et paramètres prédéfinis avec prise en charge de différents écrans/périphériques.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Diffusion</a> : options </strong>
            flexibles pour la liaison ou l’incorporation du contenu et de la diffusion Dynamic Media sur le protocole HTTP/2.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Mise à niveau de Scene7 vers Dynamic Media : </strong>
            possibilité de migrer les fichiers maîtres et de continuer à utiliser les URL S7 existantes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
    </tbody>
</table>

## Fonctionnalités de Forms

Vous trouverez ci-dessous une matrice des principales fonctions d&#39;Ajoute de l&#39;AEM Forms offertes par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions précédentes, avec des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour d’AEM Forms](https://helpx.adobe.com/experience-manager/6-5/release-notes/forms.html)

***Améliorations <sup>+</sup> significatives de la fonctionnalité de cette version.***

***SPindique que la fonction est disponible via un Service Pack ou Feature Pack. <sup></sup>***

<table>
    <thead>
        <tr>
            <td>Fonction Forms</td>
            <td>5.6.x</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Editeur</a> Forms adaptatif : </strong>
            créez des formulaires adaptatifs, réactifs et attrayants en fonction des paramètres du périphérique et du navigateur.</td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Document d’enregistrement</a> : </strong>
            créez un document pour assurer l’enregistrement à long terme d’une expérience de capture de données ou d’une version prête à l’impression.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/themes.html" target="_blank">Editeur</a> de thème : </strong>
            créez des thèmes réutilisables pour mettre en forme les composants et les panneaux d’un formulaire.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Éditeur</a> de modèle : </strong>
            standardisez et implémentez les meilleures pratiques pour les formulaires adaptatifs.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Intégration</a> Adobe Sign : </strong>
            permet le déploiement de scénarios de signature basés sur les formulaires intégrés Adobe Sign.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a> : </strong>
            avec AEM Forms, vous pouvez créer, gérer et fournir des correspondances client personnalisées et interactives.
            </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Intégration</a> de données tierces : à l’</strong>
            aide de l’intégration de données, les données sont extraites de sources de données disparates en fonction des entrées utilisateur dans un formulaire. Lors de l’envoi du formulaire, les données saisies sont enregistrées dans les sources de données.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Processus (sur OSGi) pour le traitement</a> Forms : déploiement </strong>
            simplifié des processus d’approbation des formulaires.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Intégration à Marketing Cloud</a>:</strong>
            Intégration à Adobe Analytics et Adobe Target pour améliorer et mesurer les expériences client.</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Gestionnaire de formulaires</a> : emplacement </strong>
            unique pour gérer tous les formulaires/documents/correspondances, tels que l’activation des analyses, de la traduction, des tests A/B, des révisions et de la publication.
            </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Application</a> AEM Forms : </strong>
            permet le traitement de formulaires en ligne/hors ligne dans une application sur iOS, Android ou Windows.</td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Communications</a> interactives : </strong>
            créez des communications enrichies, telles que des instructions ciblées, avec des éléments interactifs tels que des graphiques (anciennement appelés Documents adaptatifs).</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/pdf/aem-forms/6-5/WorkbenchHelp.pdf" target="_blank">Workflow (J2EE) for Forms Processing</a> : </strong>
            créez des formulaires complexes/workflows centrés sur le document à l’aide d’un IDE intuitif.</td>
            <td></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Sécurité</a> des Documents AEM Forms : accès </strong>
            sécurisé et autorisation des documents PDF et Office.
            </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Tests de cadres</a> : </strong>
            utilisez la structure Calvin et le module externe Chrome pour prendre en charge et déboguer les formulaires adaptatifs.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
    </tbody>
</table>

## Fonctionnalités des communautés

Vous trouverez ci-dessous une matrice des principales fonctions d&#39;Ajoute de l&#39;AEM Communities offertes par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions précédentes, avec des améliorations incrémentielles ajoutées dans chaque version.

+ [AEM Communities - Récapitulatif des nouvelles fonctionnalités](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html#main-pars_text)

***Améliorations <sup>+</sup> significatives de la fonctionnalité de cette version.***

***SPindique que la fonction est disponible via un Service Pack ou Feature Pack. <sup></sup>***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Fonction Communautés</td>
            <td>6,0</td>
            <td>6.1</td>
            <td>6,2</td>
            <td>6,3</td>
            <td>6,4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Fonctions de communautés</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forums</a>:</strong> (Cadre des composants sociaux) Créez de nouvelles rubriques ou vue, suivez, recherchez et déplacez des rubriques existantes.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">QnA</a> : </strong>
                Poser des questions, vue et répondre à des questions.</p>
            </td>
            <td></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a> : </strong>
                créez des articles de blog et des commentaires du côté de la publication.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Idée</a> : </strong>
                Créer et partager des idées avec la communauté, ou vue, suivre et commenter les idées existantes.
            </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendrier</a> :</strong>
                (Cadre des composants sociaux) Fournir des informations sur les événements communautaires aux visiteurs du site.
            </td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Bibliothèque</a> de fichiers : </strong>
                téléchargez, gérez et téléchargez des fichiers sur le site de la communauté.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Groupes</a> d’utilisateurs : 
            </strong>Un ensemble d’utilisateurs peut appartenir à des groupes de membres et se voir attribuer collectivement des rôles.</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Affectation</a> : </strong>
            créez et affectez des ressources d’apprentissage aux membres de la communauté.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td rowspan="5">Activation</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Gestion des </a> catalogues et des  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">ressources</a> : </strong>
            Accès aux ressources d’activation à partir du catalogue.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gestion des</a> chemins d’apprentissage : </strong>
            gérez des cours ou des groupes de ressources d’activation.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Rapports</a> d’activation:</strong>
            Rapports sur les ressources d’activation et les chemins d’apprentissage.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Engagement sur l’activation</a> : </strong>
            Ajoutez des commentaires sur les ressources d’activation.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analyses</a> d’activation:Analyses </strong>
            vidéo, Rapports de progression et Rapports d’affectation</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank"></a> Commentaires et pièces jointes :</strong>
            (Cadre des composantes sociales) En tant que membre de la communauté, il partage son opinion et ses connaissances sur le contenu du site des communautés.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Conversion de fragments de contenu : </strong>
            Convertir les contributions UGC en fragments de contenu.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Révisions</a> :</strong>
                 (Cadre des composantes sociales) En tant que membre de la communauté, il examine un élément de contenu à l’aide d’une combinaison de commentaires et de fonctions d’évaluation.</td>
            <td><sup>+</sup></td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Notations</a> : /strong&gt; (Cadre des composantes sociales) En tant que membre de la communauté, il est possible d’évaluer un élément de contenu.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votes</a> :</strong>
                 (Cadre des composants sociaux) En tant que membre de la communauté, votez ou votez un élément de contenu.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Balises</a> : </strong>
            associez des balises (mots-clés ou étiquettes) au contenu pour localiser rapidement le contenu.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Recherche</a> : recherche </strong>
            prédictive et suggestive.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traduction</a> : </strong>
            Traduction automatique du contenu généré par l’utilisateur.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td rowspan="10">Administration</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gestion</a> de site : </strong>
            création de sites avec des fonctions de communautés.</td>
            <td> </td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modèles</a> : </strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank"></a> Site et modèles de  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank"></a> groupes pour la création de sites communautaires entièrement fonctionnels à l’aide d’un assistant.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong>Modèles modifiables : </strong>
            habilitez les administrateurs de la communauté à créer des expériences enrichissantes à l’aide de modèles AEM modifiables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Groupes ou sous-communautés</a> : créez </strong>
            dynamiquement des sous-communautés dans les sites des communautés.
            </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Modération</a> : </strong>
            modération du contenu généré par l’utilisateur.
            </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Modération</a> en bloc : console </strong>
            Modération pour gérer en bloc le contenu généré par l’utilisateur.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Filtres</a> de détection des messages indésirables et de rentabilité : détection </strong>
            automatique des messages indésirables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gestion des</a> membres : </strong>
            gérez les profils utilisateur et les groupes à partir de la zone de gestion des membres.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Conception</a> réactive : les sites </strong>
            AEM Communities sont réactifs.
            </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a> : </strong>
            intégrer à Adobe Analytics pour obtenir des informations clés sur l’utilisation des sites des communautés.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td rowspan="4">Membres</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Scores et insignes</a> :</strong>
             (score avancé proposé par Adobe Sensei) Identifiez les membres de la communauté comme des experts et récompensez-les.</td>
            <td> </td>
            <td> </td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank"></a> Activités et  <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">notifications</a> :</strong>
            Vue des activités récentes, et être averti des événements d'intérêt.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Messages</a> : messages </strong>
            directs destinés aux utilisateurs et aux groupes.</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td><sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Connexions</a> sociales : </strong>
            connectez-vous avec leur compte Facebook ou Twitter.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td rowspan="5">Plate-forme</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (Enregistrement Mongo)</a> : le contenu généré par l’</strong>
            utilisateur est conservé directement dans une instance MongoDB locale.</td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (Enregistrement de base de données)</a> : le contenu généré par l’</strong>
            utilisateur est conservé directement dans une instance de base de données MySQL locale.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (Enregistrement Cloud)</a> : le contenu généré par l’</strong>
                utilisateur est conservé à distance dans un service Cloud hébergé et géré par Adobe.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a> : le contenu </strong>
                communautaire est stocké dans le JCR et l’UGC est accessible depuis l’instance d’auteur (ou de publication) à laquelle il a été publié.</td>
            <td> </td>
            <td> </td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Synchronisation</a> des utilisateurs et des groupes : </strong>
            synchronisez les utilisateurs et les groupes entre les instances de publication lors de l’utilisation d’une topologie de batterie de publication.</td>
            <td><sup>+</sup></td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
            <td>►</td>
        </tr>
    </tbody>
</table>

AEM Communities ajoute [des améliorations](https://helpx.adobe.com/experience-manager/6-5/communities/using/whats-new-aem-communities.html) par le biais de versions pour permettre aux entreprises d&#39;engager et de permettre à leurs utilisateurs, en :

+ **@** mentionprise en charge du contenu généré par l’utilisateur.
+ Améliorations de l&#39;accessibilité grâce à la **navigation par clavier** dans les composants **Activation**.
+ **Modération en bloc** améliorée à l&#39;aide de **Filtres personnalisés**.
+ **Des** modèles modifiables pour permettre aux administrateurs de la communauté de créer des expériences de la communauté riches en AEM.
+ Les utilisateurs peuvent désormais envoyer **des messages directs en bloc** à tous les membres d&#39;un groupe.
