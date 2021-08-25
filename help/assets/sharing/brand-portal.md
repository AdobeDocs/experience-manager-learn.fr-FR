---
title: Utilisation de Brand Portal
description: Présentation vidéo de l’intégration AEM Author et AEM Assets Brand Portal.
feature: Brand Portal
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 48%

---


# Utilisation de Brand Portal avec AEM Assets{#using-brand-portal-with-aem-assets}

Guides vidéo de l’intégration d’Adobe Experience Manager (AEM) Assets Brand Portal.

## Fonctionnalités et améliorations de Brand Portal septembre 2019

Brand Portal de septembre 2019 introduit notamment l’approvisionnement des ressources, ce qui augmente la vitesse du contenu et permet un échange facile et rapide des ressources entre les auteurs Experience Manager et les créatifs et contributeurs tiers.

### Approvisionnement des ressources dans Brand Portal{#asset-sourcing}

L’approvisionnement des ressources Brand Portal est utilisé pour collecter des ressources d’équipes et d’agences tierces, les synchronisant en toute transparence vers l’auteur du Experience Manager pour révision et utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager Author 6.5 SP2 (6.5.2) ou version ultérieure est requis pour utiliser l’approvisionnement des ressources*

Voir [Activer l’auteur Experience Manager pour l’approvisionnement des ressources](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=fr) pour obtenir des instructions sur la configuration et la configuration de l’approvisionnement des ressources sur l’auteur Experience Manager.

## Fonctionnalités et améliorations de Brand Portal de février 2019{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

Brand Portal - La version de février 2019 porte principalement sur les améliorations apportées à la recherche de texte et aux principales demandes des clients.

### Améliorations de la recherche

Brand Portal améliore la recherche avec la recherche de texte partielle sur le prédicat de propriété dans le volet de filtrage. Pour permettre la recherche de texte partielle, vous devez activer l’option Recherche partielle dans le prédicat Propriété du formulaire de recherche.

Lisez les sections suivantes pour en savoir plus sur la recherche de texte partielle et la recherche par caractères génériques.

#### Recherche par expression partielle

Vous pouvez maintenant rechercher des ressources en spécifiant uniquement une partie (c’est-à-dire un mot ou deux) de l’expression recherchée dans le volet de filtrage.

**Cas d’utilisation** : La recherche par expression partielle s’avère utile lorsque vous n’êtes pas sûr de la combinaison exacte des mots apparaissant dans l’expression recherchée.

Par exemple, si votre formulaire de recherche dans Brand Portal utilise le prédicat de propriété pour une recherche partielle sur le titre des ressources, la spécification du terme champ renvoie toutes les ressources contenant le mot champ dans l’expression de leur titre.

#### Recherche par caractères génériques

Brand Portal permet d’utiliser un astérisque (*) dans la requête de recherche avec une partie du mot de l’expression recherchée.

**Cas d’utilisation**  : si vous n’êtes pas sûr des mots exacts apparaissant dans l’expression recherchée, vous pouvez utiliser une recherche par caractères génériques pour remplir les trous de votre requête.

Par exemple, la spécification de climb* renvoie toutes les ressources ayant des mots commençant par les caractères climb dans l’expression de leur titre si le formulaire de recherche dans Brand Portal utilise le prédicat de propriété pour une recherche partielle sur le titre des ressources.

De même, la spécification de :

* \*climb renvoie toutes les ressources ayant des mots se terminant par les caractères climb dans l’expression de leur titre.
* \*climb\* renvoie toutes les ressources ayant des mots comprenant les caractères climb dans l’expression de leur titre.

#### Activation de la hiérarchie de dossiers

Les administrateurs peuvent maintenant configurer la façon dont les dossiers s’affichent pour les utilisateurs non-administrateurs (éditeurs, observateurs et utilisateurs invités) lors de leur connexion.
La configuration [Activer la hiérarchie de dossiers](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) a été ajoutée dans Paramètres généraux au sein du panneau des outils d’administration. Si la configuration est :

* Cette option est activée. L’arborescence de dossiers à partir du dossier racine est visible par les utilisateurs non-administrateurs. ce qui leur procure une expérience de navigation semblable à celle des administrateurs ;
* Désactivé, seuls les dossiers partagés sont affichés sur la landing page.

[La fonctionnalité Activer la ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) hiérarchie de dossiers (lorsqu’elle est activée) vous aide à différencier les dossiers portant les mêmes noms, partagés à partir de différentes hiérarchies. Lorsqu’ils se connectent, les utilisateurs non-administrateurs voient maintenant les dossiers parents (et ancêtres) virtuels des dossiers partagés.

Les dossiers partagés sont organisés au sein des répertoires respectifs dans des dossiers virtuels. Vous pouvez identifier ces dossiers virtuels grâce à leur icône de cadenas.

Notez que la miniature par défaut des dossiers virtuels est l’image de miniature du premier dossier partagé.

### Prise en charge des rendus vidéo Dynamic Media

Les utilisateurs dont l’instance d’auteur AEM est en mode hybride Dynamic Media peuvent prévisualiser et télécharger les rendus Dynamic Media, en plus des fichiers vidéo originaux.

Pour autoriser la prévisualisation et le téléchargement des rendus Dynamic Media sur des comptes de client spécifiques, les administrateurs doivent spécifier Configuration Dynamic Media (URL du service vidéo (URL de la passerelle DM) et ID d’enregistrement pour récupérer la vidéo dynamique) dans la configuration Vidéo à partir du panneau des outils d’administration.

Les vidéos Dynamic Media peuvent être prévisualisées sur :

* la page des détails de la ressource ;
* l’affichage de la carte de la ressource ;
* la page de prévisualisation du partage de lien.

Les codes des vidéos Dynamic Media peuvent être téléchargés à partir de :

* Brand Portal
* Lien partagé

### Publication planifiée sur Brand Portal

Le workflow de publication des ressources (et dossiers) de l’instance d’auteur d’[AEM (6.4.2.0)](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) sur Brand Portal peut être planifié à des date et heure ultérieures.

De façon similaire, les ressources publiées peuvent être supprimées du portail à une date (ou heure) ultérieure, en planifiant le workflow Annuler la publication sur Brand Portal.

### Alias de client configurable dans l’URL

Les organisations peuvent obtenir une URL de portail personnalisée comprenant un préfixe alternatif dans l’URL. Pour obtenir un alias pour le nom de client dans leur URL de portail existante, les organisations doivent entrer en contact avec l’assistance Adobe.

Notez que seul le préfixe de l’URL Brand Portal peut être personnalisé et non l’URL entière.
Par exemple, une entreprise avec le domaine existant `wknd.brand-portal.adobe.com` peut demander et obtenir la création de `wkndinc.brand-portal.adobe.com`.

Cependant, l’instance d’auteur AEM peut uniquement être [configurée](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) avec l’URL d’ID du client et non avec l’URL (alternative) d’alias de client.

**Cas**  pratique : Les entreprises peuvent répondre à leurs besoins de valorisation de marque en personnalisant l’URL du portail, au lieu de se contenter de l’URL fournie par Adobe.

## Fonctionnalités et améliorations de Brand Portal Décembre 2018{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Accès des invités

AEM Brand Portal permet à des invités d’accéder au portail. Un utilisateur invité ne requiert pas d’identifiants pour accéder au portail et il peut consulter et télécharger tous les dossiers et collections publics. Les utilisateurs invités peuvent ajouter des ressources à leur Lightbox (collection privée) et les télécharger. Ils peuvent aussi voir les prédicats de recherche de balises intelligentes ou non définis par les administrateurs. La session d’invité ne permet pas aux utilisateurs de créer des collections et des recherches enregistrées, ni de les partager à nouveau, d’accéder aux paramètres de collections et de dossiers et de partager les ressources en tant que liens.

### Téléchargement accéléré

Les utilisateurs de Brand Portal peuvent tirer parti des téléchargements rapides reposant sur Aspera pour obtenir des vitesses jusqu’à 25 fois plus rapides et profiter d’une expérience harmonieuse de téléchargement quel que soit leur emplacement dans le monde entier. Pour télécharger les ressources plus rapidement à partir de Brand Portal ou d’un , les utilisateurs doivent sélectionner l’option Activer l’accélération des téléchargements dans la boîte de dialogue de téléchargement, à condition que l’accélération des téléchargements soit activée dans leur organisation.

* [Guide d’accélération des téléchargements à partir de Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Serveur de test Aspera Connect](https://test-connect.asperasoft.com/)

### Rapport de connexion utilisateur

Un nouveau rapport a été ajouté pour suivre les connexions des utilisateurs. Le rapport Connexions des utilisateurs peut être essentiel pour permettre aux entreprises de réaliser un audit et de garder un œil sur les administrateurs délégués et d’autres utilisateurs de Brand Portal.

Le rapport consigne les noms d’affichage, les e-mails, les personnages (administrateur, observateur, éditeur, invité), les groupes, la dernière connexion, l’état d’activité et le nombre de connexions de chaque utilisateur.

### Accès aux rendus originaux

Les administrateurs peuvent restreindre l’accès des utilisateurs aux fichiers image originaux (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe ) et donner accès aux rendus basse résolution qu’ils téléchargent à partir de Brand Portal ou d’un lien partagé. Cet accès peut être contrôlé au niveau de groupe d’utilisateurs depuis l’onglet Groupes de la page Rôles utilisateur du panneau des outils d’administration.

### Nouvelles configurations

Six nouvelles configurations ont été ajoutées pour permettre aux administrateurs d’activer/de désactiver les fonctionnalités suivantes sur les clients spécifiques :

* Autoriser l’accès des invités
* Autoriser les utilisateurs à demander l’accès à Brand Portal
* Autoriser les administrateurs à supprimer des ressources de Brand Portal
* Autoriser la création de collections publiques
* Autoriser la création de collections dynamiques publiques
* Autoriser l’accélération des téléchargements

### Autres améliorations

* *Chemin de hiérarchie de dossiers en mode Carte et Liste*  : permet aux utilisateurs de connaître l’emplacement des dossiers stockés dans une instance Brand Portal. Permet aux utilisateurs de différencier les dossiers portant le même nom dans différentes hiérarchies de dossiers.
* *Option*  Aperçu : fournit des métadonnées aux utilisateurs non-administrateurs sur la ressource/le dossier en sélectionnant la ressource/le dossier, puis en sélectionnant l’option d’aperçu dans la barre d’outils. Actuellement, affiche le titre, la date de création et le chemin d’accès

### Adobe I/O de l’interface utilisateur des hôtes pour configurer les intégrations oAuth

Brand Portal utilise l’interface [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) de l’Adobe I/O pour créer l’application JWT qui permet de configurer les intégrations oAuth pour permettre l’intégration d’AEM Assets à Brand Portal. Auparavant, l’IU de configuration des intégrations OAuth était hébergée à l’adresse `https://marketing.adobe.com/developer/`. Pour en savoir plus sur l’intégration d’AEM Assets à Brand Portal pour publier des ressources et des collections sur Brand Portal, consultez [Configuration de l’intégration d’AEM Assets à Brand Portal](https://helpx.adobe.com/fr/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Fonctionnalités et améliorations de Brand Portal de février 2018{#brand-portal-features-and-enhancements-632}

Nouvelles fonctionnalités améliorées orientées vers l’alignement de Brand Portal avec AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Améliorations de la navigation

* Interface utilisateur mise à niveau qui s’aligne sur l’AEM et utilise l’interface utilisateur Coral3.
* Accès rapide et facile aux outils d’administration grâce au nouveau logo Adobe.
* Navigation dans un produit par le biais d’une superposition
* Navigation rapide vers les dossiers parents à partir d’un dossier enfant.
* Option Omni-recherche pour accéder aux outils et au contenu d’administration.
* Les modes Carte, Liste et Colonnes vous aident à parcourir facilement les dossiers imbriqués.
* Le tri des ressources en mode Liste n’est plus limité au nombre de ressources affichées à l’écran. Toutes les ressources d’un dossier sont triées.

### Améliorations de la recherche

* La fonctionnalité Omni-recherche vous permet d’effectuer une recherche rapide de ressources et de fichiers dans Brand Portal.
* L’omni-recherche permet également de rechercher des ressources dans un dossier ou un emplacement spécifique.
* Suggestions de mots-clés automatiques pour faciliter la recherche
* Améliorez votre omni-recherche avec des filtres supplémentaires. Option permettant d’enregistrer le résultat de la recherche dans une collecte dynamique afin de pouvoir revenir sur votre recherche ultérieurement.
* Prend en charge la recherche de ressources balisées intelligentes
* AEM les ressources balisées intelligentes peuvent être partagées d’AEM vers Brand Portal et utiliser des balises intelligentes pour la recherche de ressources dans Brand Portal.

### Amélioration du partage de fichiers

* L’utilisateur peut partager une ressource à l’aide de l’option de partage de lien.
* Lors du partage de ressources, l’utilisateur peut définir une date d’expiration pour chaque ressource. Permet aux utilisateurs de mieux contrôler les ressources partagées.
* Un utilisateur externe disposant du lien de partage de ressources peut télécharger l’image et afficher ses propriétés.
* La hiérarchie des dossiers imbriqués d’origine est conservée pour les dossiers de ressources téléchargés.

### Fonctionnalités administratives et de reporting

* Le schéma de métadonnées d’AEM Assets peut désormais être publié d’AEM vers Brand Portal.
* Les administrateurs peuvent créer et gérer trois types de rapports : ressources téléchargées, ressources arrivées à expiration et ressources publiées
* Possibilité de configurer la colonne qui doit être incluse dans le rapport.
* Création de paramètres d’image prédéfinis pour les ressources dans Brand Portal.
* Possibilité de modifier le formulaire du rail de recherche d’administration ou le Forms de recherche afin d’inclure des options de filtrage supplémentaires.
* Mettre à jour et prévisualiser du papier peint personnalisé pour votre marque
* Rapport Utilisation pour en savoir plus sur le nombre d’utilisateurs, l’espace de stockage utilisé et le total des ressources.

## Ressources supplémentaires{#additional-resources}

* [Nouveautés de Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agents de réplication d’auteur AEM](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guide de téléchargement accéléré](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documents sur les Adobes AEM Assets Brand Portal](https://helpx.adobe.com/fr/experience-manager/brand-portal/using/brand-portal.html)
* [Documents sur les Adobes AEM Assets Dynamic Media](https://experienceleague.adobe.com/docs/?lang=fr)
* [Téléchargement d’Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Serveur de test Aspera Connect](https://test-connect.asperasoft.com/)