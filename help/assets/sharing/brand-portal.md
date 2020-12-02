---
title: Utilisation du portail des marques
seo-title: Utilisation du portail de marque avec AEM Assets
description: Présentation vidéo de l’intégration d’AEM Author et de AEM Assets Brand Portal.
seo-description: Présentation vidéo de l’intégration d’AEM Author et de AEM Assets Brand Portal.
feature: brand-portal
topics: authoring, sharing, collaboration, search, integrations, publishing, metadata, images, renditions, administration
audience: all
doc-type: feature video
activity: use
version: 6.3, 6.4, 6.5
team: tm
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1788'
ht-degree: 47%

---


# Utilisation du portail de marque avec AEM Assets{#using-brand-portal-with-aem-assets}

Guides vidéo de l’intégration Adobe Experience Manager (AEM) Assets Brand Portal.

## Portail de marque septembre 2019 Fonctionnalités et améliorations

Le portail de marque de septembre 2019 introduit notamment l&#39;approvisionnement en ressources, qui augmente la vitesse de contenu et permet un échange facile et rapide d&#39;actifs entre les auteurs Experience Manager et les créatifs et contributeurs tiers.

### Approvisionnement des ressources dans Brand Portal{#asset-sourcing}

L’approvisionnement en ressources du portail des marques permet de collecter des ressources auprès d’agences et d’équipes tierces, en les synchronisant aisément avec l’auteur Experience Manager pour révision et utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/29365/?quality=12&learn=on)

*Experience Manager Author 6.5 SP2 (6.5.2) ou version ultérieure est requis pour l’utilisation de l’approvisionnement en ressources*

Consultez [Activer l’auteur Experience Manager pour l’origine des ressources](https://docs.adobe.com/content/help/en/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/configure-asset-sourcing-in-aem/brand-portal-enable-asset-sourcing.html) pour savoir comment configurer et configurer l’origine des ressources sur l’auteur Experience Manager.

## Portail de marque Février 2019 Fonctionnalités et améliorations{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

La version de février 2019 de Brand Portal porte principalement sur les améliorations apportées à la recherche de texte et aux principales demandes des clients.

### Améliorations de la recherche

Le portail de marque améliore la recherche avec une recherche de texte partielle sur le prédicat de propriété dans le volet de filtrage. Pour permettre la recherche de texte partielle, vous devez activer l’option Recherche partielle dans le prédicat Propriété du formulaire de recherche.

Lisez les sections suivantes pour en savoir plus sur la recherche de texte partielle et la recherche par caractères génériques.

#### Recherche par expression partielle

Vous pouvez maintenant rechercher des ressources en spécifiant uniquement une partie (c’est-à-dire un mot ou deux) de l’expression recherchée dans le volet de filtrage.

**Cas d’utilisation** : La recherche par expression partielle s’avère utile lorsque vous n’êtes pas sûr de la combinaison exacte des mots apparaissant dans l’expression recherchée.

Par exemple, si votre formulaire de recherche dans Brand Portal utilise le prédicat de propriété pour une recherche partielle sur le titre des ressources, la spécification du terme champ renvoie toutes les ressources contenant le mot champ dans l’expression de leur titre.

#### Recherche par caractères génériques

Brand Portal permet d’utiliser un astérisque (*) dans la requête de recherche avec une partie du mot de l’expression recherchée.

**Cas**  d’utilisation : si vous n’êtes pas sûr des mots exacts qui se trouvent dans l’expression recherchée, vous pouvez utiliser un caractère générique pour combler les lacunes de votre requête de recherche.

Par exemple, la spécification de climb* renvoie toutes les ressources ayant des mots commençant par les caractères climb dans l’expression de leur titre si le formulaire de recherche dans Brand Portal utilise le prédicat de propriété pour une recherche partielle sur le titre des ressources.

De même, la spécification de :

* \*climb renvoie toutes les ressources ayant des mots se terminant par les caractères climb dans l’expression de leur titre.
* \*escalade\* renvoie toutes les ressources dont les mots comprennent les caractères grimpent dans leur phrase de titre.

#### Activation de la hiérarchie de dossiers 

Les administrateurs peuvent maintenant configurer la façon dont les dossiers s’affichent pour les utilisateurs non-administrateurs (éditeurs, observateurs et utilisateurs invités) lors de leur connexion.
La configuration [Activer la hiérarchie de dossiers](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) a été ajoutée dans Paramètres généraux au sein du panneau des outils d’administration. Si la configuration est :

* Activé, l’arborescence de dossiers commençant à partir du dossier racine est visible pour les utilisateurs non administrateurs. ce qui leur procure une expérience de navigation semblable à celle des administrateurs ;
* Désactivé, seuls les dossiers partagés sont affichés sur le landing page.

[La fonctionnalité Activer la ](https://helpx.adobe.com/experience-manager/brand-portal/using/brand-portal-general-configuration.html) hiérarchie des dossiers (lorsqu’elle est activée) vous permet de différencier les dossiers dont le nom est identique à celui des autres hiérarchies. Lorsqu’ils se connectent, les utilisateurs non-administrateurs voient maintenant les dossiers parents (et ancêtres) virtuels des dossiers partagés.

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

**Cas**  d’utilisation : Les entreprises peuvent répondre à leurs besoins en termes de marque en personnalisant l’URL du portail, plutôt que de s’en tenir à l’URL fournie par l’Adobe.

## Portail de marque Décembre 2018 Fonctionnalités et améliorations{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707/?quality=9&learn=on)

### Accès des invités

AEM Brand Portal permet à des invités d’accéder au portail. Un utilisateur invité ne requiert pas d’identifiants pour accéder au portail et il peut consulter et télécharger tous les dossiers et collections publics. Les utilisateurs invités peuvent ajouter des ressources à leur bibliothèque lumineuse (collection privée) et en télécharger de même. Ils peuvent aussi voir les prédicats de recherche de balises intelligentes ou non définis par les administrateurs. La session d’invité ne permet pas aux utilisateurs de créer des collections et des recherches enregistrées, ni de les partager à nouveau, d’accéder aux paramètres de collections et de dossiers et de partager les ressources en tant que liens.

### Téléchargement accéléré

Les utilisateurs de Brand Portal peuvent tirer parti des téléchargements rapides reposant sur Aspera pour obtenir des vitesses jusqu’à 25 fois plus rapides et profiter d’une expérience harmonieuse de téléchargement quel que soit leur emplacement dans le monde entier. Pour télécharger les ressources plus rapidement à partir de Brand Portal ou d’un , les utilisateurs doivent sélectionner l’option Activer l’accélération de téléchargement dans la boîte de dialogue de téléchargement, à condition que l’accélération de téléchargement soit activée sur leur organisation.

* [Guide pour accélérer les téléchargements à partir du portail de marque](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)

### Rapport Connexion utilisateur

Un nouveau rapport a été ajouté pour suivre les connexions des utilisateurs. Le rapport Connexions des utilisateurs peut être essentiel pour permettre aux entreprises de réaliser un audit et de garder un œil sur les administrateurs délégués et d’autres utilisateurs de Brand Portal.

Le rapport consigne les noms d’affichage, les ID d’adresse électronique, les personnalités (admin, lecteur, éditeur, invité), les groupes, les derniers identifiants de connexion, l’état d’activité et le nombre de connexions de chaque utilisateur.

### Accès aux rendus d’origine

Les administrateurs peuvent restreindre l’accès des utilisateurs aux fichiers d’images d’origine (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, pspsd, psd, image/vnd.adobe.photoshop ) et donner accès à des rendus basse résolution qu’ils téléchargent à partir du portail de marque ou d’un lien partagé. Cet accès peut être contrôlé au niveau de groupe d’utilisateurs depuis l’onglet Groupes de la page Rôles utilisateur du panneau des outils d’administration.

### Nouvelles configurations

Six nouvelles configurations ont été ajoutées pour permettre aux administrateurs d’activer/de désactiver les fonctionnalités suivantes sur les clients spécifiques :

* Autoriser l’accès des invités
* Autoriser les utilisateurs à demander l’accès à Brand Portal
* Autoriser les administrateurs à supprimer des ressources de Brand Portal
* Autoriser la création de collections publiques
* Autoriser la création de collections dynamiques publiques
* Autoriser l’accélération des téléchargements

### Autres améliorations

* *Chemin d&#39;accès de la hiérarchie des dossiers sur les vues*  de carte et de liste : permet aux utilisateurs de connaître l&#39;emplacement des dossiers stockés dans une instance du portail de marques. Aide les utilisateurs à différencier les dossiers portant le même nom dans différentes hiérarchies de dossiers.
* *Option*  Aperçu : fournit aux utilisateurs non-administrateurs des métadonnées sur le fichier/dossier en sélectionnant le fichier/dossier, puis en sélectionnant l&#39;option d&#39;aperçu dans la barre d&#39;outils. Affiche actuellement le titre, la date de création et le chemin d’accès

### Interface utilisateur des hôtes Adobe I/O pour configurer les intégrations d’authentification

Brand Portal utilise l’interface Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) pour créer une application JWT, qui permet de configurer les intégrations Auth pour permettre l’intégration AEM Assets à Brand Portal. Auparavant, l’IU de configuration des intégrations OAuth était hébergée à l’adresse `https://marketing.adobe.com/developer/`. Pour en savoir plus sur l’intégration d’AEM Assets à Brand Portal pour publier des ressources et des collections sur Brand Portal, consultez [Configuration de l’intégration d’AEM Assets à Brand Portal](https://helpx.adobe.com/fr/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Portail de marque Février 2018 Fonctionnalités et améliorations{#brand-portal-features-and-enhancements-632}

Nouvelles fonctionnalités améliorées orientées vers l’alignement du portail de marque avec l’AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354/?quality=9&learn=on)

### Améliorations de la navigation

* Interface utilisateur mise à niveau qui s’aligne sur l’AEM et utilise l’interface utilisateur Coral3.
* Accès rapide et facile aux outils administratifs grâce au nouveau logo de l&#39;Adobe.
* Navigation dans un produit par le biais d’une incrustation
* Navigation rapide vers les dossiers parents à partir d’un dossier enfant.
* Option Omnisearch pour accéder aux outils administratifs et au contenu.
* La Vue des cartes, la vue des Listes et la Vue des colonnes vous aident à parcourir facilement les dossiers imbriqués.
* Le tri des ressources dans la Vue de Liste n’est plus limité au nombre de ressources affichées à l’écran. Tous les fichiers d’un dossier sont triés.

### Améliorations de la recherche

* La fonctionnalité Omnisearch vous permet d&#39;effectuer une recherche rapide de fichiers et de ressources dans le portail de marque.
* Omnisearch permet également de rechercher des ressources dans un dossier ou un emplacement spécifique.
* Suggestions de mots-clés automatiques pour faciliter la recherche
* Améliorez Omnisearch avec des filtres supplémentaires. Option permettant d’enregistrer le résultat de la recherche dans une collection dynamique afin que vous puissiez relancer votre recherche ultérieurement.
* Prend en charge la recherche de ressources balisées intelligentes
* aem ressources balisées intelligentes peuvent être partagées de AEM au portail de la marque et utiliser des balises actives pour la recherche de ressources dans le portail de la marque.

### Améliorations du partage de fichiers

* L’utilisateur peut partager un fichier à l’aide de l’option de partage de liens.
* Lors du partage de fichiers, l’utilisateur peut définir une date d’expiration pour chaque fichier. Permet aux utilisateurs de mieux contrôler les ressources partagées.
* Un utilisateur externe disposant du lien de partage de ressources peut télécharger l’image et en vue ses propriétés.
* La hiérarchie des dossiers imbriqués d’origine est conservée pour les dossiers de fichiers téléchargés.

### Rapports et capacités administratives

* Le schéma de métadonnées d’AEM Assets peut désormais être publié à partir de l’AEM vers le portail de la marque.
* Les administrateurs peuvent créer et gérer trois types de rapports : ressources téléchargées, ressources arrivées à expiration et ressources publiées
* Possibilité de configurer la colonne qui doit être incluse dans le rapport.
* Créez des paramètres d’image prédéfinis pour les fichiers du portail de marques.
* Possibilité de modifier le formulaire du rail de recherche d’administration ou le Forms de recherche pour inclure d’autres options de filtrage.
* Mettre à jour et prévisualisation le papier peint personnalisé pour votre marque
* Rapport d’utilisation pour en savoir plus sur le nombre d’utilisateurs, l’espace d’enregistrement utilisé et le total des ressources.

## Ressources supplémentaires{#additional-resources}

* [Nouveautés de Brand Portal](https://helpx.adobe.com/experience-manager/brand-portal/using/whats-new.html)
* [Agents de réplication AEM Author](https://helpx.adobe.com/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guide de téléchargement accéléré](https://helpx.adobe.com/experience-manager/brand-portal/using/accelerated-download.html#main-pars_header)
* [Documents d’Adobe de la marque et du portail AEM Assets](https://helpx.adobe.com/fr/experience-manager/brand-portal/using/brand-portal.html)
* [Documentation sur les Adobes de médias dynamiques en AEM Assets](https://docs.adobe.com/docs/fr/aem/6-3/author/assets/dynamic-media.html)
* [Télécharger Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Aspera Connect Test Server](https://test-connect.asperasoft.com/)