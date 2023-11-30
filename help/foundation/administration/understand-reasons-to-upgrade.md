---
title: Comprendre les raisons de mettre à niveau
description: Explication de haut niveau des fonctionnalités clés pour les clientes et clients qui envisagent d’effectuer une mise à niveau vers la dernière version d’Adobe Experience Manager 6.
version: 6.5
topic: Upgrade
feature: Release Information
role: Leader, Architect, Developer, Admin, User
level: Beginner
doc-type: Article
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '2602'
ht-degree: 100%

---

# Comprendre les raisons de mettre à niveau

Explication de haut niveau des fonctionnalités clés pour les clientes et clients qui envisagent d’effectuer une mise à niveau vers la dernière version d’Adobe Experience Manager 6.

## Fonctionnalités clés pour la mise à niveau vers AEM 6.5

+ [Notes de mise à jour d’Adobe Experience Manager 6.5](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes.html)

### Améliorations de la base

Adobe Experience Manager 6.5 continue d’améliorer la stabilité, les performances et la capacité de prise en charge du système via les éléments suivants :

+ La prise en charge de **Java 11** (tout en conservant la prise en charge de Java 8).

### Créer et gérer des sites web

AEM Sites introduit un certain nombre de fonctionnalités conçues pour accélérer la création et le développement de sites web :

+ La prise en charge de l’**éditeur de SPA** permet la création complète de SPA (applications monopages) dans AEM, ce qui offre une expérience de création riche et conviviale pour les marketeurs et marketeuses.
+_ **JavaScript SDK** est un kit de démarrage de projet de SPA et des outils de création de support, permettent aux développeurs et développeuses front-end de développer des applications monopages compatibles avec l’éditeur de SPA indépendamment d’AEM.
+ Les **Composants principaux** ajoutent une multitude de nouveaux composants, une **bibliothèque de composants** ainsi que diverses améliorations apportées aux composants principaux existants.
+ Davantage d’améliorations de **traduction** rationnalisent la traduction d’AEM Sites.

### Expériences fluides

AEM continue d’adopter des expériences fluides avec des outils nouveaux et améliorés qui facilitent l’utilisation du contenu en dehors d’AEM.

+ Les **fragments de contenu** prennent en charge la comparaison/différence de versions et les annotations.
+ L’**API HTTP AEM Assets** prend en charge l’exposition des **fragments de contenu** directement dans la gestion des ressources numériques en tant que **JSON**.
  Les **fragments d’expérience** prennent en charge la **recherche de texte intégral** et l’**invalidation du cache de Dispatcher AEM** pour le référencement des **pages**.

### Gestion des ressources numériques

AEM Assets continue de tirer parti de son riche ensemble de fonctionnalités de gestion des ressources numériques pour améliorer l’utilisation, la gouvernance et la compréhension de la gestion des ressources numériques. AEM 6.5 continue d’améliorer l’intégration entre Adobe Creative Cloud et les workflows créatifs.

+ **Adobe Asset Link** connecte directement les personnes créatives à AEM Assets à partir des outils Adobe Creative Cloud.
+ L’intégration d’**Adobe Stock** permet un accès aux images d’Adobe Stock directement à partir de l’expérience AEM Assets, créant ainsi une expérience transparente de découverte de contenu.
+ L’**application de bureau AEM** présente la version 2.0 et se réinitialise tout en améliorant les performances et la stabilité.
+ Les **ressources connectées** prennent en charge les instances AEM Sites discrètes pour accéder et utiliser en toute transparence les ressources d’une autre instance AEM Assets.
+ Mise à jour de la prise en charge vidéo dans **Dynamic Media**, y compris **Vidéo 360** et les **miniatures vidéo personnalisées**.

### Intelligence de contenu

AEM continue à développer son intégration avec des technologies intelligentes, en exploitant le machine learning et l’intelligence artificielle pour améliorer toutes les expériences.

+ **Adobe Asset Link** ajoute la **recherche par analogie visuelle**, permettant de découvrir et d’utiliser facilement des images similaires dans les **outils Adobe Creative Cloud**.

### Intégrations

AEM développe sa capacité d’intégration à d’autres services Adobe :

+ Les **fragments d’expérience** approfondissent leur intégration à **Adobe Target** en prennant en charge l’**export au format JSON** vers Adobe Target et la possibilité de **supprimer les offres basées sur des fragments d’expérience** d’**Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), qui s’adresse exclusivement aux clientes et clients Adobe Managed Services (AMS), propose les fonctionnalités suivantes :

+ Cloud Manager prend en charge l’extension de la prise en charge du déploiement AEM d’AEM Sites vers **AEM Assets**, y compris le **test de performance automatisé du traitement des ressources**.
+ La **mise à l’échelle automatique** du niveau de publication AEM à des seuils prédéfinis, assurant une expérience optimale des utilisateurs et utilisatrices finaux.
+ Les **pipelines hors production** permettent aux équipes de développement d’exploiter Cloud Manager pour contrôler en permanence la qualité du code et déployer dans des environnements inférieurs (développement et assurance qualité).
+ Les **API de pipeline CI/CD** permettent aux clientes et clients d’interagir par programmation avec Cloud Manager, en approfondissant les possibilités d’intégration avec l’infrastructure de développement on-premise.

## Fonctionnalités de base

Vous trouverez ci-dessous un tableau des principales fonctionnalités de base proposées par AEM. Certaines fonctionnalités ont été introduites dans des versions antérieures et bénéficient d’améliorations à chaque nouvelle version.

+ [Notes de mise à jour d’AEM de base](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> : indique que des améliorations importantes ont été apportées à la fonctionnalité dans cette version.***

***✔<sup>SP</sup> : indique que la fonctionnalité est disponible par le biais d’un pack de services ou de fonctionnalités.***

<table>
    <thead>
        <tr>
            <td>Fonctionnalité de base</td>
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
                <strong>Prise en charge de Java 11 :</strong> AEM prend en charge Java 11 (ainsi que Java 8).
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Référentiel de contenu Oak</a> :</strong> offre des performances et une évolutivité bien supérieures à Jackrabbit 2, son prédécesseur.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html?lang=fr">Prise en charge de l’index oak-run.jar</a> :</strong> amélioration de la réindexation, de la collecte de statistiques et de la vérification de cohérence des index Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html?lang=fr" target="_blank">Index de recherche personnalisés</a> :</strong>
possibilité d’ajouter des définitions d’index personnalisées pour optimiser les performances des requêtes et la pertinence des recherches.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Nettoyage des révisions en ligne</a> :</strong>
effectuez la maintenance du référentiel sans arrêt du serveur.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/storage-elements-in-aem-6.html?&amp;lang=fr" target="_blank">Stockage du référentiel TarMK ou MongoMK</a> :</strong>
<br>options d’utilisation d’un stockage simple et performant basé sur des fichiers de TarMK (version de nouvelle génération de TarPM),
<br>ou mise à l’échelle horizontale avec un référentiel soutenu par MongoDB avec MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/aem-with-mongodb.html?&amp;lang=fr" target="_blank">Performances et stabilité de MongoMK</a> :</strong>
des améliorations continues ont été apportées à MongoMK depuis son introduction avec AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Magasin de données Amazon S3</a> :</strong>
profitez de la solution de stockage extensible dans le cloud pour stocker des ressources binaires.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Parité des fonctionnalités de l’interface utilisateur tactile :</strong>
améliorations continues de l’interface utilisateur de création, en termes de vitesse et de productivité accrue, et parité des fonctionnalités avec l’interface utilisateur classique.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch :</strong>
pour des recherches et une navigation rapides dans AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Tableau de bord des opérations</a> :</strong>
effectuez la maintenance, surveillez l’intégrité du serveur et analysez les performances depuis AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Améliorations des mises à niveau</a> :</strong>
les améliorations apportées aux mises à niveau d’AEM les rendent plus rapides et faciles.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/htl/using/overview.html" target="_blank">Langage HTL</a> :</strong>
il s’agit d’un moteur de modèle moderne qui sépare la présentation de la logique. Cela permet de réduire considérablement le temps de développement des composants. Fonctionnalités incrémentielles ajoutées à chaque version.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modèles Sling</a> :</strong>
framework flexible pour la modélisation de ressources JCR en objets commerciaux et en logique. Fonctionnalités incrémentielles ajoutées à chaque version.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a> : </strong>
réservé aux clientes et clients Adobe Managed Services (AMS), Cloud Manager accélère le développement et le déploiement via un pipeline CI/CD dernier cri.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Fonctionnalités de sécurité

Vous trouverez ci-dessous un tableau des principales fonctionnalités de sécurité proposées par AEM. Certaines fonctionnalités ont été introduites dans des versions antérieures et bénéficient d’améliorations à chaque nouvelle version.

+ [Notes de mise à jour de sécurité](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔ : indique que des améliorations importantes ont été apportées à la fonctionnalité dans cette version.***

***✔<sup>+</sup> : indique que la fonctionnalité est disponible par le biais d’un pack de services ou de fonctionnalités.***

<table>
    <thead>
        <tr>
            <td>Fonctionnalité de sécurité</td>
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
        <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-service-users.html?lang=fr" target="_blank">Utilisateurs et utilisatrices de service</a></strong>
<br>Le compartimentage des autorisations permet d’éviter l’utilisation inutile de privilèges d’administration.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gestion du KeyStore</a></strong>
<br>Le Trust Store global, les certificats et les clés sont tous gérés au sein du référentiel.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=fr" target="_blank"><strong>Protection</strong> <strong>CSRF</strong></a>
<br>Protection contre les attaques CSRF prête à l’emploi.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>Prise en charge</strong> de <strong>CORS</strong></a>
<br>Prise en charge du partage des ressources entre origines multiples pour une plus grande flexibilité des applications.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=fr" target="_blank">Amélioration de la prise en charge de l’authentification SAML</a><br>
</strong>Amélioration de la redirection SAML, optimisation des informations de groupe et résolution des problèmes de chiffrement des clés.
<br>
        </td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP comme configuration OSGi</a><br>
</strong>Simplifie la gestion et les mises à jour de l’authentification LDAP.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong>Prise en charge du chiffrement OSGi pour les mots de passe en texte brut<br>
</strong>Les mots de passe et autres valeurs sensibles peuvent être enregistrés et chiffrés, puis déchiffrés automatiquement.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Améliorations des groupes d’utilisateurs ou utilisatrices fermés</a><br>
</strong>L’implémentation du groupe d’utilisateurs ou utilisatrices fermé a été réécrite afin de résoudre les problèmes de performances et d’évolutivité.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔<sup>+</sup></td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/platform-repository/using/ssl-wizard-technical-video-use.html" target="_blank">Assistant SSL</a></strong>
<br>Interface utilisateur pour simplifier la configuration et la gestion du protocole SSL.</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/encapsulated-token.html" target="_blank">Prise en charge des jetons encapsulés</a></strong>
<br>Elle n’est plus nécessaire pour que les sessions « persistantes » prennent en charge l’authentification horizontale entre les instances de publication.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Prise en charge de l’authentification Adobe IMS</a><br>
</strong>Exclusive à Adobe Managed Services (AMS) pour gérer de manière centralisée l’accès aux instances de création AEM via Adobe IMS (Identity Management System).</td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
    </tr>
</tbody>
</table>

## Fonctionnalités de Sites

Vous trouverez ci-dessous une matrice des principales fonctionnalités de Sites proposées par AEM. Certaines fonctionnalités ont été introduites dans des versions antérieures et bénéficient d’améliorations à chaque nouvelle version.

+ [Notes de mise à jour d’AEM Sites](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> : améliorations importantes de la fonctionnalité dans cette version.***

***✔<sup>SP</sup> : indique que la fonctionnalité est disponible par le biais d’un pack de services ou de fonctionnalités.***

<table>
    <thead>
        <tr>
            <td><strong>Fonctionnalité Sites</strong></td>
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
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/page-editor-feature-video-use.html?lang=fr" target="_blank">Création de pages optimisée pour les écrans tactiles</a> :</strong>
permet aux éditeurs et éditrices d’exploiter les tablettes et les ordinateurs avec écrans tactiles.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Création de site réactifs</a> :</strong>
le mode de disposition permet aux éditeurs et éditrices de redimensionner les composants en fonction des largeurs de l’appareil pour les sites réactifs.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/page-authoring/template-editor-feature-video-use.html?lang=fr" target="_blank">Modèles modifiables</a> :</strong>
ils permettent aux créateurs et créatrices spécialisés de créer et de modifier des modèles de page.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/core-components/user-guide.html" target="_blank">Composants principaux</a> :</strong>
ils accélèrent le développement du site. Disponible sur GitHub pour un calendrier de publication fréquent et pour plus de flexibilité.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Éditeur de SPA</a> :</strong>
créez des expériences web modifiables à l’aide des frameworks d’application monopage (SPA) créés sur React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Système de style :</strong>
augmentez la réutilisation des composants AEM en définissant leur apparence visuelle à l’aide du système de style dans le contexte.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-site Manager (MSM)</a> :</strong>
gérez plusieurs sites web qui partagent du contenu commun (c’est-à-dire plusieurs langues, plusieurs marques).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduction de contenu</a> :</strong>
le framework plug-and-play s’intègre aux services de traduction tiers de pointe.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/developing/personlization/contexthub.html?lang=fr" target="_blank">ContextHub</a> :</strong>
framework de contexte client de nouvelle génération pour la personnalisation du contenu.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/launches/launches.html?lang=fr" target="_blank">Lancements</a> :</strong>
développez du contenu pour une version ultérieure sans perturber la création quotidienne.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Fragments de contenu :</strong>
créez et traitez du contenu éditorial dissocié de la présentation pour une réutilisation facile.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=fr" target="_blank">Fragments d’expérience</a> :</strong>
créez des expériences et des variations réutilisables optimisées pour les canaux de bureau, mobiles et sociaux.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Services de contenu :</strong>
exportez du contenu d’AEM au format JSON pour le consommer sur plusieurs appareils et applications.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Intégration d’Adobe Analytics et d’insights sur le contenu :</strong>
intégration facile d’Adobe Analytics et de DTM. Affichez les informations de performances dans l’environnement de création.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/personalization/content-targeting-touch.html?lang=fr" target="_blank">Intégration d’Adobe Target</a> :</strong>
assistant détaillé pour créer des expériences ciblées et des bibliothèques d’offres réutilisables.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/campaign.html?lang=fr" target="_blank">Intégration d’Adobe Campaign</a> :</strong>
intégration simple à la solution de campagne par e-mail de nouvelle génération.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Intégration d’Adobe Launch</a> :</strong>
intégration au service cloud de gestion des balises de nouvelle génération d’Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Screens :</strong>
gérez les expériences relatives à la signalétique numérique et aux kiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/commerce/classic/administering/ecommerce.html?lang-fr" target="_blank"> eCommerce</a> :</strong>
permet d’offrir des expériences d’achat personnalisées sur les points de contact web, mobiles et de réseaux sociaux.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/communities/introduction/overview.html?lang=fr" target="_blank">Communities</a> :</strong>
les forums, les commentaires sous forme de threads, les calendriers d’événement et toute une série d’autres fonctionnalités favorisent un engagement profond de la part des visiteurs et visiteuses du site.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
    </tbody>
</table>

## Fonctionnalités d’Assets

Vous trouverez ci-dessous un tableau avec les principales fonctionnalités d’Assets proposées par AEM. Certaines fonctionnalités ont été introduites dans des versions antérieures et bénéficient d’améliorations à chaque nouvelle version.

+ [Notes de mise à jour d’AEM Assets](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/assets.html)

***✔ : indique que des améliorations importantes ont été apportées à la fonctionnalité dans cette version.***

***✔<sup>+</sup> : indique que la fonctionnalité est disponible par le biais d’un pack de services ou de fonctionnalités.***

<table>
    <thead>
        <tr>
            <td>Fonctionnalité d’Assets</td>
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
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface utilisateur optimisée pour les écrans tactiles</a> :</strong>
gérez les ressources sur un ordinateur de bureau ou sur des appareils tactiles.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/metadata.html?lang=fr" target="_blank">Gestion avancée des métadonnées</a> :</strong>
modèles de métadonnées, éditeur de schéma de métadonnées et modification de métadonnées en bloc.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Gestion des <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Tâches</a> et des <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">Workflows</a> :</strong>
les workflows et les tâches préconfigurés pour la révision et l’approbation des ressources numériques exploitant les projets AEM.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Évolutivité et performance :</strong>
amélioration de la prise en charge de l’ingestion, du chargement et du stockage à grande échelle.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP de ressources</a> :</strong>
utilisez la programmation pour interagir avec des ressources via HTTP et JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Partage de liens</a> :</strong>
partage ad hoc simple de ressources numériques sans connexion.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a> :</strong>
solution SAAS du service cloud pour un partage et une distribution transparents des ressources numériques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/use-assets-across-connected-assets-instances.html" target="_blank">Ressources connectées</a>:</strong>
les instances AEM Sites peuvent facilement accéder aux ressources d’une autre instance AEM Assets et les utiliser.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/managing/asset-insights.html?lang=fr" target="_blank">Informations sur les ressources</a> :</strong>
tirez parti d’Adobe Analytics pour capturer l’interaction client avec les ressources numériques et les afficher dans AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html?lang=fr" target="_blank">Ressources multilingues</a> :</strong>
            Prise en charge automatique de la traduction des métadonnées de ressources avec des racines linguistiques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Balises intelligentes et modération</a> :</strong>
            Tirez parti d’Adobe Sensei pour baliser automatiquement les images avec des métadonnées pertinentes.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html?lang=fr" target="_blank">Recherche de traduction intelligente</a> :</strong>
            Traduisez automatiquement les termes recherchés lors de la recherche dans AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html?lang=fr" target="_blank">Intégration d’Adobe InDesign Server</a> :</strong>
            Générez des catalogues de produits. Créez des brochures, dépliants et publicités imprimées à partir de modèles InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/desktop-app/aem-desktop-app.html" target="_blank">Application de bureau AEM</a> :</strong>
            Synchronisez les ressources avec l’ordinateur local pour les modifier avec les produits Creative Suite.
            </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/administer/imaging-transcoding-library.html?lang=fr" target="_blank">Bibliothèque d’images d’Adobe</a> :</strong>
                <br> Bibliothèques Photoshop et Acrobat PDF utilisées pour la manipulation de fichiers de haute qualité.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/fr/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Lien de ressource Adobe</a> :</strong>
            Accédez à AEM Assets directement à partir des applications Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/using/aem-assets-adobe-stock.html?lang=fr" target="_blank">Intégration d’Adobe Stock</a> :</strong>
            Accédez en toute transparence aux images Adobe Stock et utilisez-les directement depuis AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

### AEM Assets Dynamic Media

***✔<sup>+</sup> améliorations importantes de la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible par le biais d’un pack de services ou d’un pack de fonctionnalités.***


<table>
    <thead>
        <tr>
            <td>Fonctionnalité de Dynamic Media</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6.5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/managing-assets.html?lang=fr" target="_blank">Imagerie</a> :</strong>
            Diffusez dynamiquement des images dans différents formats et tailles, y compris avec le recadrage intelligent.</td>
            <td> </td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vidéo</a> :</strong>
            Codage vidéo avancé et streaming de vidéo adaptative</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/interactive-images.html?lang=fr" target="_blank">Média interactif</a> :</strong>
            Créez des bannières interactives, des vidéos avec du contenu cliquable pour présenter des offres clés.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Jeux (<a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/image-sets.html?lang=fr" target="_blank">Image</a>, <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/spin-sets.html?lang=fr" target="_blank">Rotation</a>, <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/mixed-media-sets.html?lang=fr" target="_blank">Supports variés</a>) :</strong>
            Permet aux utilisateurs ou utilisatrices d’effectuer un zoom, un panoramique, une rotation et de simuler une expérience d’affichage à 360 degrés.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/?lang=fr" target="_blank">Visionneuses</a> :</strong>
            Lecteurs multimédias enrichis personnalisés et paramètres prédéfinis avec prise en charge de différents écrans/appareils.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/delivering-dynamic-media-assets.html?lang=fr" target="_blank">Diffusion</a> :</strong>
            Options flexibles pour la liaison ou l’incorporation de contenu et de diffusion de Dynamic Media via le protocole HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Mise à niveau de Scene7 vers Dynamic Media :</strong>
            Possibilité de migrer les ressources marketing et de continuer à utiliser les URL S7 existantes.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

## Fonctionnalités Forms

Vous trouverez ci-dessous une matrice des principales fonctionnalités du module complémentaire AEM Forms proposées par AEM. Certaines fonctionnalités ont été introduites dans des versions antérieures et bénéficient d’améliorations à chaque nouvelle version.

***✔<sup>+</sup> améliorations importantes de la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible par le biais d’un pack de services ou d’un pack de fonctionnalités.***

<table>
    <thead>
        <tr>
            <td>Fonctionnalité Forms</td>
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
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Éditeur de formulaires adaptatifs</a> :</strong>
            Créez des formulaires adaptatifs, réactifs et attrayants en fonction des paramètres de votre appareil et de votre navigateur.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Document d’enregistrement</a> :</strong>
            Créez un document pour garantir le stockage à long terme d’une expérience de capture de données ou d’une version prête à être imprimée.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/themes.html" target="_blank">Éditeur de thèmes</a> :</strong>
            Créez des thèmes réutilisables pour mettre en forme les composants et les panneaux d’un formulaire.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Éditeur de modèles</a> :</strong>
            Normalisez et implémentez les bonnes pratiques des formulaires adaptatifs.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Intégration d’Acrobat Sign</a> :</strong>
            Autorisez le déploiement des scénarios de signature d’Acrobat Sign basés sur les formulaires intégrés.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/letters-correspondences/cm-overview.html?lang=fr" target="_blank">Gestion des correspondances</a> :</strong>
 Avec AEM Forms, vous pouvez créer, gérer et fournir des correspondances personnalisées et interactives à la clientèle.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Intégration des données de tiers</a> :</strong>
 Avec l’intégration de données, ces dernières sont récupérées à partir de sources de données disparates en fonction des entrées des utilisateurs ou utilisatrices dans un formulaire. Lors de l’envoi du formulaire, les données capturées sont réécrites dans les sources de données.
            </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Processus (sur OSGi) pour le traitement de formulaires</a> :</strong>
 Simplification du déploiement des processus d’approbation des formulaires.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/home.html?lang=fr&amp;topic=%2Fexperience-manager%2F6-5%2Fforms%2Fmorehelp%2Fintegrations.ug.js" target="_blank">Intégration à Marketing Cloud</a> :</strong>
 Intégration à Adobe Analytics et Adobe Target pour améliorer et mesurer les expériences client.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/getting-started/introduction-managing-forms.html?lang=fr" target="_blank">Form Manager</a> :</strong>
 Emplacement unique pour gérer l’ensemble des formulaires/documents/correspondances, tel que l’activation des analyses, de la traduction, des Tests AB, des révisions et de la publication.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-65/forms/aem-forms-app/aem-forms-app.html?lang=fr" target="_blank">Application AEM Forms</a> :</strong>
 Autoriser le traitement des formulaires en ligne/hors ligne dans une application sur iOS, Android ou Windows.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/aem-forms/6-5/introduction-forms-authoring.html" target="_blank">Communications interactives</a> :</strong>
 Créer des communications enrichies telles que des instructions ciblées avec des éléments interactifs comme des graphiques (anciennement appelés documents adaptatifs).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Workflow (J2EE) pour le traitement de formulaires :</strong>
 Créer des formulaires complexes et des flux de travail centrés sur les documents à l’aide d’un IDE intuitif.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">Sécurité des documents AEM Forms</a> :</strong>
 Accès sécurisé et autorisation des documents PDF et Office.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Frameworks de test</a> :</strong>
 Utiliser le framework Calvin et le plug-in Chrome pour prendre en charge et déboguer les formulaires adaptatifs.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

