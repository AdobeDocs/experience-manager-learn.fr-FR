---
title: Comprendre les raisons de la mise à niveau
description: Une ventilation de haut niveau des fonctionnalités clés pour les clients qui envisagent d’effectuer une mise à niveau vers la dernière version d’Adobe Experience Manager 6.
version: 6.5
topic: Upgrade
role: Leader, Architect, Developer, Admin, User
level: Beginner
exl-id: bf4030b0-67c4-4b00-af95-f63e6f79e995
source-git-commit: 34fbb22916cf8a8df0e3240835c71e0979fd11bd
workflow-type: tm+mt
source-wordcount: '3460'
ht-degree: 6%

---

# Comprendre les raisons de la mise à niveau

Une ventilation de haut niveau des fonctionnalités clés pour les clients qui envisagent d’effectuer une mise à niveau vers la dernière version d’Adobe Experience Manager 6.

## Principales fonctionnalités pour la mise à niveau vers AEM 6.5

+ [Notes de mise à jour d’Adobe Experience Manager 6.5](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes.html )

### Améliorations de la base

Adobe Experience Manager 6.5 continue d’améliorer la stabilité, les performances et la capacité de prise en charge du système en procédant comme suit :

+ **Java 11** prise en charge (tout en conservant la prise en charge de Java 8).

### Création et gestion de site web

AEM Sites s’accompagne d’un certain nombre de fonctionnalités conçues pour accélérer la création et le développement de sites web :

+ **Éditeur de SPA** La prise en charge permet la création complète de SPA (applications d’une seule page) dans AEM, ce qui permet une expérience de création riche et conviviale pour les marketeurs.
+_ **SDK JavaScript**, un kit de démarrage de projet SPA et des outils de génération pris en charge, permettent aux développeurs front-end de développer SPA applications d’une seule page compatibles avec l’éditeur, indépendamment d’AEM.
+ **Composants principaux** ajoute une multitude de nouveaux composants, une **Bibliothèque de composants** ainsi que diverses améliorations apportées aux composants principaux existants.
+ Plus **Traductions** améliorations de la rationalisation de la traduction d’AEM Sites.

### Expériences fluides

AEM continue d’adopter des expériences fluides avec des outils nouveaux et améliorés qui facilitent l’utilisation du contenu en dehors d’AEM.

+ **Fragments de contenu** prendre en charge la comparaison/comparaison de versions et les annotations.
+ **API HTTP AEM Assets** prend en charge l’exposition **Fragments de contenu** directement dans la gestion des actifs numériques en tant que **JSON**.
   **Fragments d’expérience** support **Recherche de texte intégral** et **Invalidation du cache de Dispatcher AEM** pour le référencement **Pages**.

### Gestion des ressources

AEM Assets continue de tirer parti de son riche ensemble de fonctionnalités de gestion des ressources pour améliorer l’utilisation, la gestion et la compréhension de la gestion des ressources numériques. AEM 6.5 continue d’améliorer l’intégration entre Adobe Creative Cloud et les workflows créatifs.

+ **Adobe d’un lien de ressource** connecte directement les créatifs à AEM Assets à partir des outils Adobe Creative Cloud.
+ **Adobe Stock** l’intégration permet un accès direct aux images Adobe Stock directement à partir de l’expérience AEM Assets, créant ainsi une expérience transparente de découverte de contenu.
+ **AEM Desktop App** publie la version 2.0 et se réinitialise tout en améliorant les performances et la stabilité.
+ **Ressources connectées** prend en charge les instances AEM Sites discrètes pour accéder et utiliser en toute transparence les ressources d’une autre instance AEM Assets.
+ Mise à jour de la prise en charge vidéo dans **Dynamic Media**, y compris **Vidéo 360** et **Miniatures vidéo personnalisées**.

### Content Intelligence

AEM continue à construire son intégration avec des technologies intelligentes, en exploitant l&#39;apprentissage automatique et l&#39;intelligence artificielle pour améliorer toutes les expériences.

+ **Adobe d’un lien de ressource** ajoute **Recherche par analogie visuelle**, permettant de découvrir et d’utiliser facilement des images similaires dans **Outils Adobe Creative Cloud**.

### Intégrations

AEM développe sa capacité d’intégration à d’autres services Adobe :

+ **Fragments d’expérience** approfondit leur intégration à **Adobe Target** en soutenant **Exporter au format JSON** vers Adobe Target et la possibilité de **suppression d’offres basées sur des fragments d’expérience** de **Adobe Target**.

### AMS Cloud Manager

[Cloud Manager](https://adobe.ly/2HODmsv), qui s’adresse exclusivement aux clients Adobe Managed Services (AMS), propose les fonctionnalités suivantes :

+ Cloud Manager prend en charge l’extension AEM prise en charge du déploiement d’AEM Sites vers **AEM Assets**, y compris **test de performance automatisé du traitement des ressources**.
+ **Mise à l’échelle automatique** du niveau Publication AEM à des seuils prédéfinis, assurant une expérience optimale à l’utilisateur final.
+ **Pipelines hors production** permettre aux équipes de développement d’exploiter Cloud Manager pour contrôler en permanence la qualité du code et le déploiement dans des environnements inférieurs (développement et assurance qualité).
+ **API de pipeline CI/CD** permettent aux clients d’interagir par programmation avec Cloud Manager, en approfondissant les possibilités d’intégration avec l’infrastructure de développement on-premise.

## Fonctionnalités de base

Vous trouverez ci-dessous un tableau des principales fonctionnalités de base proposées par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions antérieures par des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour d’AEM Foundation](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html)

***✔<sup>+</sup> améliorations importantes apportées à la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***

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
                <strong>Prise en charge de Java 11 :</strong> AEM prend en charge Java 11 (ainsi que Java 8).
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
                <strong><a href="https://jackrabbit.apache.org/oak/docs/index.html" target="_blank">Oak Content Repository</a>:</strong> Offre des performances et une évolutivité bien supérieures à celles du prédécesseur Jackrabbit 2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/indexing-via-the-oak-run-jar.html">Prise en charge de l’index oak-run.jar</a>:</strong> Amélioration de la réindexation, de la collecte de statistiques et de la vérification de cohérence des index Oak.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/queries-and-indexing.html" target="_blank">Index de recherche personnalisés</a>: </strong>
                Possibilité d’ajouter des définitions d’index personnalisées pour optimiser les performances des requêtes et la pertinence des recherches.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/revision-cleanup.html" target="_blank">Nettoyage des révisions en ligne</a>:</strong>
                Effectuez la maintenance du référentiel sans temps d’arrêt du serveur.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/storage-elements-in-aem-6.html" target="_blank">Stockage du référentiel TarMK ou MongoMK</a>:</strong>
                <br> Options d’utilisation d’un stockage simple et performant basé sur des fichiers de TarMK (version de nouvelle génération de TarPM)
                <br> ou effectuez une mise à l’échelle horizontale avec un référentiel pris en charge par MongoDB avec MongoMK.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/aem-with-mongodb.html" target="_blank">Performances et stabilité de MongoMK</a>:</strong>
            Des améliorations continues ont été apportées à MongoMK depuis son introduction avec AEM 6.0.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/data-store-config.html#AmazonS3DataStore">Entrepôt de données Amazon S3</a>:</strong>
            Tirez parti de la solution de stockage dans le cloud extensible pour stocker les ressources binaires.</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Parité des fonctionnalités de l’interface utilisateur tactile :</strong>
                Améliorations continues de l’interface utilisateur de création pour une vitesse accrue de productivité et de parité des fonctionnalités avec l’interface utilisateur classique.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Omnisearch :</strong>
                Recherchez et accédez rapidement à AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/operations-dashboard.html" target="_blank">Tableau de bord des opérations</a>:</strong>
 Effectuez la maintenance, surveillez l’intégrité du serveur et analysez les performances depuis AEM.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/upgrade.html" target="_blank">Améliorations de la mise à niveau</a>:</strong>
            Les améliorations apportées à la mise à niveau permettent des mises à niveau sur place plus rapides et plus faciles d’AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/htl/using/overview.html" target="_blank">HTL Template Language</a>:</strong>
            Moteur de modèle moderne qui sépare la présentation de la logique. Réduit considérablement le temps de développement des composants. Fonctionnalités incrémentielles ajoutées à chaque version.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://sling.apache.org/documentation/bundles/models.html" target="_blank">Modèles Sling</a>:</strong>
            Structure flexible pour la modélisation de ressources JCR en objets commerciaux et en logique. Fonctionnalités incrémentielles ajoutées à chaque version.
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
            <td><strong><a href="https://adobe.ly/2HODmsv" target="_blank">Cloud Manager</a>: </strong>
                Exclusif aux clients Adobe Managed Services (AMS), Cloud Manager accélère le développement et le déploiement via un pipeline CI/CD dernier cri.</td>
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

Vous trouverez ci-dessous une matrice des principales fonctionnalités de sécurité proposées par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions antérieures par des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour de sécurité](https://helpx.adobe.com/experience-manager/6-5/release-notes/wcm-platform.html#Security)

***✔ indique que des améliorations importantes ont été apportées à la fonctionnalité dans cette version.***

***✔<sup>+</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***

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
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/security-service-users.html" target="_blank">Utilisateurs du service</a></strong>
            <br> Les autorisations de compartimentage évitent l’utilisation inutile de privilèges d’administrateur.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank">Gestion des magasins clés</a></strong>
            <br> Trust Store, certificats et clés globaux gérés dans le référentiel.</td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/csrf-protection.html" target="_blank"><strong>CSRF</strong> <strong>protection</strong></a>
            <br> Protection par falsification de requête intersites prête à l’emploi.</td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/saml-2-0-authenticationhandler.html" target="_blank"><strong>CORS</strong> <strong>support</strong></a>
            <br> Prise en charge du partage des ressources cross-origin pour une plus grande flexibilité des applications.</td>
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
 </strong>Amélioration de la redirection SAML, optimisation des informations de groupe et résolution des problèmes de cryptage des clés.
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
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html" target="_blank">LDAP comme configuration OSGi</a><br>
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
 </strong>Les mots de passe et autres valeurs sensibles peuvent être enregistrés sous forme chiffrée et déchiffrés automatiquement.</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/user-group-ac-admin.html" target="_blank">Améliorations apportées aux CUG</a><br>
 </strong>L’implémentation du groupe d’utilisateurs fermé a été réécrite afin de résoudre les problèmes de performances et d’évolutivité.</td>
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
            <br> Interface utilisateur pour simplifier la configuration et la gestion du protocole SSL.</td>
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
            <br> Plus nécessaire pour que les sessions "persistantes" prennent en charge l’authentification horizontale entre les instances de publication.</td>
        <td> </td>
        <td> </td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
        <td>✔</td>
    </tr>
    <tr>
        <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ims-config-and-admin-console.html" target="_blank">Prise en charge de l’authentification Adobe IMS</a><br>
 </strong>Exclusif à Adobe Managed Services (AMS), gérez de manière centralisée l’accès aux instances d’auteur AEM via Adobe IMS (système Identity Management).</td>
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

## Fonctionnalités des sites

Vous trouverez ci-dessous une matrice des principales fonctionnalités de Sites proposées par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions antérieures par des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour d’AEM Sites](https://helpx.adobe.com/experience-manager/6-5/release-notes/sites.html)

***✔<sup>+</sup> améliorations importantes apportées à la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***

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
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/page-editor-feature-video-use.html" target="_blank">Création de pages optimisée pour les écrans tactiles</a>:</strong>
            Permet aux éditeurs d’exploiter les tablettes et les ordinateurs avec écrans tactiles.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/authoring/using/responsive-layout.html" target="_blank">Création de site réactive</a>:</strong>
                Le mode de mise en page permet aux éditeurs de redimensionner les composants en fonction des largeurs d’appareil pour les sites réactifs.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/template-editor-feature-video-use.html" target="_blank">Modèles modifiables</a>:</strong>
            Permet aux auteurs spécialisés de créer et de modifier des modèles de page.</td>
            <td></td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/core-components/user-guide.html" target="_blank">Composants principaux</a>:</strong>
            Accélérer le développement du site. Disponible sur GitHub pour un calendrier de publication et une flexibilité fréquents.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/spa-overview.html" target="_blank">Éditeur de SPA</a>:</strong>
            Créez des expériences web modifiables à l’aide des structures d’application sur une seule page (SPA) créées sur React.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Système de style :</strong>
            Augmentez la réutilisation des composants AEM en définissant leur apparence visuelle à l’aide du système de style dans le contexte.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/msm.html" target="_blank">Multi-site Manager (MSM)</a>:</strong>
            Gérez plusieurs sites web qui partagent du contenu commun (c’est-à-dire plusieurs marques multilingues).</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/translation.html" target="_blank">Traduction de contenu</a>:</strong>
            La structure Plug-and-play s’intègre aux services de traduction tiers de pointe.</td>
            <td></td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/contexthub.html" target="_blank">ContextHub</a>:</strong>
            Cadre de contexte client de nouvelle génération pour la personnalisation du contenu.</td>
            <td></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/launches.html" target="_blank">Lancements</a>:</strong>
            Développez du contenu pour une version ultérieure sans perturber la création quotidienne.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Fragments de contenu :</strong>
            Créez et traitez du contenu éditorial dissocié de la présentation pour une réutilisation facile.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/sites/using/experience-fragments-feature-video-use.html" target="_blank">Fragments d’expérience</a>:</strong>
            Créez des expériences et des variations réutilisables optimisées pour les canaux de bureau, mobiles et sociaux.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Content Services :</strong>
            Exportez du contenu d’AEM au format JSON pour le consommer sur plusieurs appareils et applications.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>SP</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Intégration d’Adobe Analytics et statistiques sur le contenu :</strong>
                Intégration facile d’Adobe Analytics et de DTM. Affichez les informations de performances dans l’environnement de création.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/content-targeting-touch.html" target="_blank">Intégration Adobe Target</a>:</strong>
            Assistant détaillé pour créer des expériences ciblées et des bibliothèques d’offres réutilisables.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/campaign.html" target="_blank">Intégration Adobe Campaign</a>:</strong>
            Intégration simple avec la solution de campagne email de nouvelle génération.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/using/aem_launch_adobeio_integration.html" target="_blank">Intégration d’Adobe Launch</a>:</strong>
            Intégration au service cloud de nouvelle génération de gestion des balises d’Adobe.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Screens :</strong>
            Gérez les expériences relatives à la signalétique numérique et aux kiosques.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ecommerce.html" target="_blank">eCommerce</a>:</strong>
            proposer des expériences d’achat personnalisées et de marque sur des points de contact web, mobiles et sociaux ;
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html" target="_blank">Communautés</a>:</strong>
            Les forums, les commentaires sous forme de threads, les calendriers d’événement et de nombreuses autres fonctionnalités permettent un engagement profond auprès des visiteurs du site.</td>
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

## Fonctionnalités Assets

Vous trouverez ci-dessous une matrice des principales fonctionnalités d’Assets proposées par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions antérieures par des améliorations incrémentielles ajoutées dans chaque version.

+ [Notes de mise à jour d’AEM Assets](https://helpx.adobe.com/experience-manager/6-5/release-notes/assets.html)

***✔ indique que des améliorations importantes ont été apportées à la fonctionnalité dans cette version.***

***✔<sup>+</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***

<table>
    <thead>
        <tr>
            <td>Fonctionnalité Ressources</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/managing-assets-touch-ui.html" target="_blank">Interface utilisateur optimisée pour les écrans tactiles</a>:</strong>
            Gestion des ressources sur un ordinateur de bureau ou sur des périphériques tactiles.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/metadata.html" target="_blank">Gestion avancée des métadonnées</a>:</strong>
            Modèles de métadonnées, Éditeur de schéma de métadonnées et Modification de métadonnées en bloc.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/task-content.html" target="_blank">Tâche</a> et <a href="https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/projects-with-workflows.html" target="_blank">Workflow</a> Gestion :</strong>
            Workflows et tâches préconfigurés pour la révision et l’approbation des ressources numériques exploitant AEM projets.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Évolutivité et performance :</strong>
            Amélioration de la prise en charge de l’ingestion, du chargement et du stockage à grande échelle.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mac-api-assets.html" target="_blank">API HTTP Assets</a>:</strong>
            Interagir par programmation avec des ressources via HTTP et JSON.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/link-sharing.html" target="_blank">Partage de liens</a>:</strong>
            Partage ad hoc simple de ressources numériques sans avoir à se connecter.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/brand-portal/using/brand-portal.html" target="_blank">Brand Portal</a>:</strong>
            Solution SAAS du service cloud pour un partage et une distribution transparents des ressources numériques.</td>
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
            Les instances AEM Sites peuvent facilement accéder aux ressources d’une autre instance AEM Assets et les utiliser.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/touch-ui-asset-insights.html" target="_blank">Informations sur les ressources</a>:</strong>
            Tirez parti d’Adobe Analytics pour capturer l’interaction client des ressources numériques et les afficher dans AEM.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/multilingual-assets.html" target="_blank">Ressources multilingues</a>:</strong>
            Prise en charge automatique de la traduction des métadonnées de ressources avec les racines de langue.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/enhanced-smart-tags.html" target="_blank">Balises intelligentes et modération</a>:</strong>
            Utilisez Adobe Sensei pour baliser automatiquement les images avec des métadonnées utiles.</td>
            <td> </td>
            <td></td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/kt/assets/using/smart-translation-search-feature-video-use.html" target="_blank">Recherche de traduction dynamique</a>:</strong>
            Traduire automatiquement les termes de recherche lors de la recherche dans AEM Assets.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/indesign.html" target="_blank">Intégration Adobe InDesign Server</a>:</strong>
            Générer des catalogues de produits Créer des brochures, dépliants et publicités imprimées à partir de modèles d'InDesign.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/experience-manager-desktop-app/using/using.html?lang=fr" target="_blank">AEM Desktop App</a>:</strong>
            Synchronisez les ressources avec l’ordinateur local pour les modifier avec les produits Creative Suite.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/imaging-transcoding-library.html" target="_blank">Bibliothèque d’images d’Adobe</a>:</strong>
                <br> Bibliothèques Photoshop et Acrobat PDF utilisées pour la manipulation de fichiers de haute qualité.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://www.adobe.com/fr/creativecloud/business/enterprise/adobe-asset-link.html" target="_blank">Adobe d’un lien de ressource</a>:</strong>
            Accédez à AEM Assets directement à partir des applications Adobe Create Cloud.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/aem-assets-adobe-stock.html" target="_blank">Intégration Adobe Stock</a>:</strong>
            Accédez en toute transparence aux images Adobe Stock et utilisez-les directement depuis AEM.</td>
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

### AEM Assets Dynamic Media

***✔<sup>+</sup> améliorations importantes apportées à la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***


<table>
    <thead>
        <tr>
            <td>Fonctionnalité Dynamic Media</td>
            <td>5.6.x</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3 +FP</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/managing-assets.html" target="_blank">Imagerie</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/video-profiles.html" target="_blank">Vidéo</a>:</strong>
            Codage vidéo avancé et diffusion en continu de vidéo adaptative</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/interactive-images.html" target="_blank">Média interactif</a>:</strong>
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
            <td><strong>Définit (<a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/image-sets.html" target="_blank">Image</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/spin-sets.html" target="_blank">Rotation</a>, <a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/mixed-media-sets.html" target="_blank">Supports variés</a>) :</strong>
            Permet aux utilisateurs d’effectuer un zoom, un panoramique, une rotation et de simuler une expérience d’affichage à 360 degrés.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://experienceleague.adobe.com/docs/" target="_blank">Visionneuses</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/assets/using/delivering-dynamic-media-assets.html" target="_blank">Diffusion</a>:</strong>
            Options flexibles pour la liaison ou l’incorporation de contenu et de diffusion Dynamic Media via le protocole HTTP/2.</td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Mise à niveau de Scene7 vers Dynamic Media :</strong>
            Possibilité de migrer les ressources maîtres et de continuer à utiliser les URL S7 existantes.</td>
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

Vous trouverez ci-dessous une matrice des principales fonctionnalités du module complémentaire AEM Forms proposées par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions antérieures par des améliorations incrémentielles ajoutées dans chaque version.

***✔<sup>+</sup> améliorations importantes apportées à la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***

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
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/introduction-forms-authoring.html" target="_blank">Éditeur Forms adaptatif</a>:</strong>
            Créez des formulaires adaptatifs, réactifs et attrayants en fonction des paramètres du périphérique et du navigateur.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/generate-document-of-record-for-non-xfa-based-adaptive-forms.html" target="_blank">Document d’enregistrement</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/themes.html" target="_blank">Éditeur de thème</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/fr/experience-manager/6-5/forms/using/template-editor.html" target="_blank">Éditeur de modèles</a>:</strong>
            Normalisez et implémentez les bonnes pratiques pour les formulaires adaptatifs.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedintegrationwithAdobeSign" target="_blank">Intégration Adobe Sign</a>:</strong>
            Autoriser le déploiement des scénarios de signature Adobe Sign basés sur les formulaires intégrés.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/cm-overview.html" target="_blank">Correspondence Management</a>:</strong>
            Avec AEM Forms, vous pouvez créer, gérer et diffuser des correspondances client personnalisées et interactives.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#AEMFormsdataintegration" target="_blank">Intégration de données tierces</a>:</strong>
            À l’aide de l’intégration de données, les données sont récupérées à partir de sources de données disparates en fonction des entrées utilisateur dans un formulaire. Lors de l’envoi du formulaire, les données saisies sont enregistrées dans les sources de données.
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#FormscentricAEMWorkflowsforAEMFormsonOSGi" target="_blank">Processus (sur OSGi) pour le traitement Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/user-guide.html?topic=/experience-manager/6-5/forms/morehelp/integrations.ug.js" target="_blank">Intégration avec Marketing Cloud</a>:</strong>
            Intégration à Adobe Analytics et Adobe Target pour améliorer et mesurer les expériences client.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/introduction-managing-forms.html" target="_blank">Form Manager</a>:</strong>
            Emplacement unique pour gérer l’ensemble des formulaires/documents/correspondances, tel que l’activation des analyses, de la traduction, des tests A/B, des révisions et de la publication.
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
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/forms/using/aem-forms-app.html" target="_blank">Application AEM Forms</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/adaptive-document.html" target="_blank">Communications interactives</a>:</strong>
            Créez des communications enrichies telles que des instructions ciblées avec des éléments interactifs tels que des graphiques (anciennement appelés documents adaptatifs).</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong>Workflow (J2EE) pour le traitement Forms :</strong>
            Créez des formulaires/processus complexes axés sur les documents à l’aide d’un IDE intuitif.</td>
            <td></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#EnhancedDocumentSecurity" target="_blank">AEM Forms Document Security</a>:</strong>
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
            <td><strong><a href="https://helpx.adobe.com/aem-forms/6-5/whats-new.html#Simplifiedauthoringexperience" target="_blank">Structures de test</a>:</strong>
            Utilisez la structure Calvin et le module externe Chrome pour prendre en charge et déboguer les formulaires adaptatifs.</td>
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

## Fonctions de communauté

Vous trouverez ci-dessous une matrice des principales fonctionnalités du module complémentaire AEM Communities proposées par AEM. Certaines de ces fonctionnalités ont été introduites dans les versions antérieures par des améliorations incrémentielles ajoutées dans chaque version.

***✔<sup>+</sup> améliorations importantes apportées à la fonctionnalité dans cette version.***

***✔<sup>SP</sup> indique que la fonctionnalité est disponible via un Service Pack ou un Feature Pack.***

<table>
    <thead>
        <tr>
            <td> </td>
            <td>Fonctionnalité de communautés</td>
            <td>6.0</td>
            <td>6.1</td>
            <td>6.2</td>
            <td>6.3</td>
            <td>6.4</td>
            <td>6,5</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="7">Fonctions de communauté</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/forum.html" target="_blank">Forums</a>:</strong> (Structure de composants sociaux) Créez de nouvelles rubriques ou affichez, suivez, recherchez et déplacez des rubriques existantes.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <p><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-qna.html" target="_blank">Q&amp;R</a>:</strong>
                Posez, affichez et répondez à des questions.</p>
            </td>
            <td></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/blog-feature.html" target="_blank">Blogs</a>:</strong>
                Créez des articles de blog et des commentaires côté publication.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/ideation-feature.html" target="_blank">Idéation</a>:</strong>
                Créez et partagez des idées avec la communauté, ou regardez, suivez et commentez des idées existantes.
            </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/calendar.html" target="_blank">Calendrier</a>:</strong>
                (Structure des composants sociaux) Fournissez des informations sur les événements communautaires aux visiteurs du site.
            </td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td>
                <strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/file-library.html" target="_blank">Bibliothèque de fichiers</a>:</strong>
                Chargez, gérez et téléchargez des fichiers sur le site de la communauté.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/users.html#AboutCommunityGroups" target="_blank">Groupes d’utilisateurs</a>:
            </strong>Un ensemble d’utilisateurs peut appartenir à des groupes de membres et se voir attribuer collectivement des rôles.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong> </strong></td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Attribution</a>:</strong>
            Créez et affectez des ressources d’apprentissage aux membres de la communauté.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Activation</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/catalog.html" target="_blank">Catalogue</a> et <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resource.html" target="_blank">Gestion des ressources</a>:</strong>
            Accédez aux ressources d’activation à partir du catalogue.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#CreateaLearningPath" target="_blank">Gestion des chemins d’apprentissage</a>:</strong>
            Gérez des cours ou des groupes de ressources d’activation.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reports.html#main-pars_text_1739724213" target="_blank">Création de rapports d’activation</a>:</strong>
            Création de rapports sur les ressources d’activation et les parcours d’apprentissage.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/resources.html#main-pars_text_899882038" target="_blank">Engagement sur l’activation</a>:</strong>
            Ajoutez des commentaires sur les ressources d’activation.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics d’activation</a>:</strong>
            Analyses vidéo, création de rapports de progression et création de rapports d’affectation</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="8">Commons</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/comments.html" target="_blank">Commentaires</a> et pièces jointes :</strong>
            (Cadre des composants sociaux) En tant que membre de la communauté, il partage son opinion et ses connaissances sur le contenu du site des communautés.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Conversion de fragment de contenu :</strong>
            Convertir les contributions UGC en fragments de contenu.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/reviews.html" target="_blank">Révisions</a>:</strong>
                (Structure de composants sociaux) En tant que membre de la communauté, passez en revue un élément de contenu à l’aide d’une combinaison de commentaires et de fonctions d’évaluation.</td>
            <td>✔<sup>+</sup></td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/rating.html" target="_blank">Évaluations</a>:/strong&gt; (Social Component Framework) En tant que membre de la communauté, évaluez un élément de contenu.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/voting.html" target="_blank">Votes</a>:</strong>
                (Structure de composants sociaux) En tant que membre de la communauté, votez ou annulez un élément de contenu.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tag-ugc.html" target="_blank">Balises</a>:</strong>
            Joindre des balises (mots-clés ou étiquettes) au contenu pour localiser rapidement le contenu.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/search.html" target="_blank">Rechercher</a>:</strong>
            Recherches prédictives et suggestives.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/translate-ugc.html" target="_blank">Traduction</a>:</strong>
            Traduction automatique du contenu généré par l’utilisateur.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="10">Administration</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/create-site.html" target="_blank">Gestion de site</a>:</strong>
            Création de sites avec des fonctions de communautés.</td>
            <td> </td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Modèles</a>:</strong>
                <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sites.html" target="_blank">Site</a> et <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/tools-groups.html" target="_blank">group</a> modèles pour la création par assistant de sites de la communauté entièrement fonctionnels.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong>Modèles modifiables :</strong>
            Donnez aux administrateurs de la communauté les moyens de créer des expériences riches à l’aide AEM modèles modifiables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/creating-groups.html" target="_blank">Groupes ou sous-communautés</a>:</strong>
            Créez dynamiquement des sous-communautés dans des sites de communautés.
            </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/in-context.html" target="_blank">Modération</a>:</strong>
            Modération de contenu généré par l’utilisateur.
            </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderation.html" target="_blank">Modération en bloc</a>:</strong>
            Console de modération pour gérer en masse le contenu généré par l’utilisateur.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/moderate-ugc.html#CommonModerationConcepts" target="_blank">Détection des emails et filtres de profil</a>:</strong>
            Détection automatique des emails indésirables.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/members.html" target="_blank">Gestion des membres</a>:</strong>
            Gérez les profils utilisateur et les groupes à partir de la zone de gestion des membres.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/overview.html#main-pars_text_866731966" target="_blank">Responsive Design</a>:</strong>
            Les sites AEM Communities sont réactifs.
            </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/analytics.html" target="_blank">Analytics</a>:</strong>
            Intégration à Adobe Analytics pour obtenir des informations clés sur l’utilisation des sites Communities.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="4">Membres</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/advanced.html" target="_blank">Notation et attribution de badges</a>:</strong>
            (Score avancé proposé par Adobe Sensei) Identifiez les membres de la communauté en tant qu’experts et récompensez-les.</td>
            <td> </td>
            <td> </td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/activities.html" target="_blank">Activités</a> et <a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/notifications.html" target="_blank">Notifications</a>:</strong>
            Affichez le flux d’activités récentes et soyez informé des événements qui vous intéressent.</td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
            <td> </td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/configure-messaging.html" target="_blank">Messages</a>:</strong>
            Messagerie directe vers les utilisateurs et les groupes.</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔<sup>+</sup></td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/social-login.html" target="_blank">Connexions aux réseaux sociaux</a>:</strong>
            Connectez-vous avec leur compte Facebook ou Twitter.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td rowspan="5">Plateforme</td>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">MSRP (stockage Mongo)</a>:</strong>
            Le contenu généré par l’utilisateur est conservé directement dans une instance MongoDB locale.</td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">DSRP (stockage de base de données)</a>:</strong>
            Le contenu généré par l’utilisateur est conservé directement dans une instance de base de données MySQL locale.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank">SRP (espace de stockage)</a>:</strong>
                Le contenu généré par l’utilisateur est conservé à distance dans un service cloud hébergé et géré par Adobe.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/working-with-srp.html" target="_blank"><strong>JSRP</a>:</strong>
                Le contenu de la communauté est stocké dans JCR et le contenu généré par l’utilisateur est accessible à partir de l’instance d’auteur (ou de publication) à laquelle il a été publié.</td>
            <td> </td>
            <td> </td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
        <tr>
            <td><strong><a href="https://helpx.adobe.com/experience-manager/6-5/communities/using/sync.html" target="_blank">Synchronisation des utilisateurs et des groupes</a>:</strong>
            Synchronisez les utilisateurs et les groupes entre les instances de publication lors de l’utilisation d’une topologie de ferme de publication.</td>
            <td>✔<sup>+</sup></td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
            <td>✔</td>
        </tr>
    </tbody>
</table>

AEM Communities ajoute des améliorations par le biais de mises à jour pour permettre aux entreprises d’interagir et de permettre à leurs utilisateurs, en procédant comme suit :

+ **@mention** prise en charge du contenu généré par l’utilisateur.
+ Améliorations de l’accessibilité par le biais de **Navigation au clavier** in **Activation** composants.
+ Amélioré **Modération en bloc** using **Filtres personnalisés**.
+ **Modèles modifiables** pour permettre aux administrateurs de la communauté de créer des expériences de la communauté riches dans AEM.
+ Les utilisateurs peuvent désormais envoyer **messages directs en bloc** à tous les membres d’un groupe.
