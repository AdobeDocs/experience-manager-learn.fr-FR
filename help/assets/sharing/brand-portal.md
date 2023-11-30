---
title: Utiliser Brand Portal
description: Présentation vidéo de l’intégration d’AEM Assets Brand Portal à l’instance de création AEM.
feature: Brand Portal
version: 6.4, 6.5
topic: Content Management
role: User
level: Beginner
last-substantial-update: 2022-06-15T00:00:00Z
doc-type: Feature Video
exl-id: 42f13a19-52bf-413d-a141-63f1f0910dce
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '1764'
ht-degree: 100%

---

# Utiliser Brand Portal avec AEM Assets{#using-brand-portal-with-aem-assets}

Guides vidéo de l’intégration d’Adobe Experience Manager (AEM) Assets Brand Portal.

## Fonctionnalités et améliorations de la version de septembre 2019 de Brand Portal

La version de septembre 2019 de Brand Portal introduit l’approvisionnement des ressources, qui permet d’augmenter la vitesse du contenu et offre un échange facile et rapide des ressources entre les auteurs et autrices d’Experience Manager et les autres personnes tierces responsables de la création ou qui y contribuent.

### Approvisionnement des ressources dans Brand Portal{#asset-sourcing}

La fonctionnalité d’approvisionnement des ressources de Brand Portal permet de collecter des ressources d’équipes et d’agences tierces et de les synchroniser en toute transparence sur l’instance de création d’Experience Manager à des fins de révision et d’utilisation.

>[!VIDEO](https://video.tv.adobe.com/v/29365?quality=12&learn=on)

*L’instance de création Experience Manager 6.5 SP2 (6.5.2) ou version ultérieure est requise pour utiliser l’approvisionnement des ressources*.

Consultez la section [Activer l’instance de création Experience Manager pour l’approvisionnement des ressources](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/asset-sourcing-in-brand-portal/brand-portal-asset-sourcing.html?lang=fr) pour obtenir des instructions sur la configuration de l’approvisionnement des ressources sur l’instance de création Experience Manager.

## Fonctionnalités et améliorations de la version de février 2019 de Brand Portal{#brand-portal-features-and-enhancements-644}

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

La version de février 2019 de Brand Portal apporte des améliorations à la recherche de texte et aux principales demandes des clientes et clients.

### Améliorations de la recherche

Brand Portal améliore l’expérience de recherche, avec la recherche de texte partiel sur le prédicat de propriété du panneau de filtrage. Pour permettre la recherche de texte partielle, vous devez activer l’option Recherche partielle dans le prédicat de propriété du formulaire de recherche.

Lisez les sections suivantes pour en savoir plus sur la recherche de texte partielle et la recherche par caractères génériques.

#### Recherche par expression partielle

Vous pouvez désormais rechercher des ressources en spécifiant uniquement une partie (un mot ou deux) de l’expression recherchée dans le volet de filtrage.

**Cas d’utilisation** : la recherche par expression partielle s’avère utile lorsque vous avez des doutes sur la combinaison exacte des mots apparaissant dans l’expression recherchée.

Par exemple, si votre formulaire de recherche dans Brand Portal utilise le prédicat de propriété pour une recherche partielle sur le titre des ressources, la spécification du terme camp renvoie toutes les ressources contenant le mot « camp » dans l’expression de leur titre.

#### Recherche par caractères génériques

Brand Portal permet d’utiliser un astérisque (*) dans la requête de recherche avec une partie du mot de l’expression recherchée.

**Cas d’utilisation** : si vous n’avez pas la certitude de connaître les mots exacts apparaissant dans l’expression recherchée, vous pouvez utiliser une recherche par caractères génériques pour remplir les trous de votre requête de recherche.

Par exemple, la spécification de climb* renvoie toutes les ressources ayant des mots commençant par les caractères climb dans l’expression de leur titre si le formulaire de recherche dans Brand Portal utilise le prédicat de propriété pour une recherche partielle sur le titre des ressources.

De même, la spécification de :

* \*climb renvoie toutes les ressources ayant des mots se terminant par les caractères climb dans l’expression de leur titre.
* \*climb\* renvoie toutes les ressources ayant des mots comprenant les caractères climb dans l’expression de leur titre.

#### Activer la hiérarchie de dossiers

Les administrateurs et administratrices peuvent maintenant configurer la façon dont les dossiers s’affichent pour les utilisateurs et utilisatrices non-administrateurs (éditeurs et éditrices, observateurs et observatrices, et utilisateurs et utilisatrices invités) lors de leur connexion.
La configuration [Activer la hiérarchie de dossiers](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/admin-tools/brand-portal-general-configuration.html?lang=fr) a été ajoutée dans Paramètres généraux au sein du panneau des outils d’administration. Si la configuration est :

* activée, l’arborescence de dossiers à partir du dossier racine est visible par les utilisateurs et les utilisatrices ne disposant pas de droits d’administration. Ces utilisateurs et utilisatrices bénéficient ainsi d’une expérience de navigation similaire à celle des administrateurs et administratrices.
* désactivée, seuls les dossiers partagés sont affichés sur la page de destination.

La fonctionnalité [Activer la hiérarchie de dossiers](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/admin-tools/brand-portal-general-configuration.html?lang=fr) (lorsqu’elle est activée) vous aide à différencier les dossiers portant les mêmes noms, mais qui sont partagés depuis différentes hiérarchies. Lorsqu’ils se connectent, les utilisateurs non-administrateurs voient maintenant les dossiers parents (et ancêtres) virtuels des dossiers partagés.

Les dossiers partagés sont organisés dans les répertoires respectifs des dossiers virtuels. Vous pouvez identifier ces dossiers virtuels grâce à leur icône de cadenas.

Notez que la miniature par défaut des dossiers virtuels est l’image de miniature du premier dossier partagé.

### Prise en charge des rendus vidéo Dynamic Media

Les utilisateurs dont l’instance d’auteur AEM est en mode hybride Dynamic Media peuvent prévisualiser et télécharger les rendus Dynamic Media, en plus des fichiers vidéo originaux.

Pour autoriser la prévisualisation et le téléchargement des rendus Dynamic Media sur des comptes de client ou cliente spécifiques, les administrateurs et administratrices doivent spécifier la configuration Dynamic Media (URL du service vidéo (URL de la passerelle DM) et ID d’enregistrement pour récupérer la vidéo Dynamic Video) dans la configuration Vidéo à partir du panneau des outils d’administration.

Vous pouvez prévisualiser les vidéos Dynamic Media sur :

* la page des détails de la ressource ;
* l’affichage de la carte de la ressource ;
* la page de prévisualisation du partage de lien.

Les codes vidéo Dynamic Media peuvent être téléchargés à partir de :

* Brand Portal
* Lien partagé

### Publication planifiée sur Brand Portal

Le workflow de publication des ressources (et dossiers) de l’instance d’auteur d’[AEM (6.4.2.0)](https://helpx.adobe.com/fr/experience-manager/6-5/release-notes/sp-release-notes.html#main-pars_header_9658011) sur Brand Portal peut être planifié à des date et heure ultérieures.

De façon similaire, les ressources publiées peuvent être supprimées du portail à une date (ou heure) ultérieure, en planifiant le workflow Dépublier sur Brand Portal.

### Alias du client ou de la cliente configurable dans l’URL

Les organisations peuvent personnaliser l’URL de leur portail en y ajoutant un autre préfixe. Pour obtenir un alias pour le nom de client dans leur URL de portail existante, les organisations doivent entrer en contact avec l’assistance Adobe.

Notez que seul le préfixe de l’URL Brand Portal peut être personnalisé et non l’URL entière.
Par exemple, une entreprise avec le domaine existant `wknd.brand-portal.adobe.com` peut demander et obtenir la création de `wkndinc.brand-portal.adobe.com`.

Cependant, l’instance d’auteur AEM peut uniquement être [configurée](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html) avec l’URL d’ID du client et non avec l’URL (alternative) d’alias de client.

**Cas d’utilisation** : les organisations peuvent répondre à leurs besoins d’image de marque en faisant personnaliser l’URL de leur portail, au lieu de se contenter de l’URL fournie par Adobe.

## Fonctionnalités et améliorations de la version de décembre 2018 de Brand Portal{#brand-portal-features-and-enhancements-642}

>[!VIDEO](https://video.tv.adobe.com/v/23707?quality=12&learn=on)

### Accès des personnes invitées

AEM Brand Portal permet aux personnes invitées d’accéder au portail. Un utilisateur invité ne requiert pas d’identifiants pour accéder au portail et il peut consulter et télécharger tous les dossiers et collections publics. Les utilisateurs et utilisatrices invités peuvent ajouter des ressources à leur Lightbox (collection privée) et les télécharger. Ils peuvent aussi voir les prédicats de recherche de balises intelligentes ou non définis par les administrateurs. La session de personne invitée ne permet pas aux utilisateurs et utilisatrices de créer des collections et des recherches enregistrées ou de les partager, d’accéder aux paramètres des dossiers et des collections et de partager des ressources sous forme de liens.

### Téléchargement accéléré

Les utilisateurs et utilisatrices de Brand Portal peuvent tirer parti des téléchargements rapides reposant sur Aspera pour obtenir des vitesses jusqu’à 25 fois plus rapides et profiter d’une expérience de téléchargement transparente quel que soit leur situation géographique. Pour télécharger les ressources plus rapidement à partir de Brand Portal ou du lien partagé, les utilisateurs et utilisatrices doivent sélectionner l’option Activer l’accélération des téléchargements dans la boîte de dialogue de téléchargement, à condition que l’accélération des téléchargements soit activée dans leur organisation.

* [Guide d’accélération des téléchargements à partir de Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/download/accelerated-download.html?lang=fr)
* [Serveur de test d’Aspera Connect](https://test-connect.asperasoft.com/)

### Rapport de connexion d’utilisateur ou d’utilisatrice

Un nouveau rapport a été ajouté pour suivre les connexions des utilisateurs et des utilisatrices. Le rapport Connexions des utilisateurs et des utilisatrices peut être essentiel pour permettre aux entreprises de réaliser un audit et de garder un œil sur les administrateurs et administratrices délégués et d’autres utilisateurs et utilisatrices de Brand Portal.

Le rapport consigne les noms d’affichage, les ID d’e-mail, les personnages (administrateur ou administratrice, observateur ou observatrice, éditeur ou éditrice, invité ou invitée), les groupes, la dernière connexion, le statut d’activité et le nombre de connexions de chaque utilisateur et utilisatrice.

### Accéder aux rendus originaux

Les administrateurs et administratrices peuvent restreindre l’accès des utilisateurs et utilisatrices aux fichiers images originaux (jpeg, tiff, png, bmp, gif, pjpeg, x-portable-anymap, x-portable-bitmap, x-portable-graymap, x-portable-pixmap, x-rgb, x-xbitmap, x-xpixmap, x-icon, image/photoshop, image/x-photoshop, psd, image/vnd.adobe.photoshop) et accorder l’accès aux rendus de basse résolution téléchargés à partir de Brand Portal ou d’un lien partagé. Cet accès peut être contrôlé au niveau du groupe d’utilisateurs et d’utilisatrices à partir de l’onglet Groupes de la page Rôles d’utilisateur et d’utilisatrice du panneau des outils d’administration.

### Nouvelles configurations

Six nouvelles configurations sont ajoutées pour permettre aux administrateurs et administratrices d’activer et de désactiver les fonctionnalités suivantes pour des clientes et clientes spécifiques :

* Autoriser l’accès des invités
* Autoriser les utilisateurs et utilisatrices à demander l’accès à Brand Portal
* Autoriser la suppression des ressources de Brand Portal par les utilisateurs et utilisatrices disposant de droits d’administration
* Autoriser la création de collections publiques
* Autoriser la création de collections dynamiques publiques
* Autoriser l’accélération des téléchargements

### Autres améliorations

* *Chemin de hiérarchie de dossiers en mode Carte et Liste* : permet aux utilisateurs et aux utilisatrices de connaître l’emplacement des dossiers stockés dans une instance Brand Portal. Permet aux utilisateurs et aux utilisatrices de différencier les dossiers portant le même nom dans différentes hiérarchies de dossiers.
* *Option vue d’ensemble* : fournit des métadonnées aux utilisateurs et utilisatrices qui ne disposent pas de droits d’administration sur la ressource/le dossier en sélectionnant la ressource/le dossier, puis en cliquant sur l’option de vue d’ensemble dans la barre d’outils. Pour l’instant, le titre, la date de création et le chemin d’accès sont affichés.

### Interface utilisateur Adobe I/O pour configurer les intégrations OAuth

Brand Portal utilise l’interface Adobe I/O [https://legacy-oauth.cloud.adobe.io/](https://legacy-oauth.cloud.adobe.io/) pour créer l’application JWT qui rend possible la configuration des intégrations OAuth de façon à permettre l’intégration d’AEM Assets à Brand Portal. Auparavant, l’IU de configuration des intégrations OAuth était hébergée à l’adresse `https://marketing.adobe.com/developer/`. Pour en savoir plus sur l’intégration d’AEM Assets à Brand Portal pour publier des ressources et des collections sur Brand Portal, consultez [Configuration de l’intégration d’AEM Assets à Brand Portal](https://helpx.adobe.com/fr/experience-manager/6-4/assets/using/brand-portal-configuring-integration.html).

## Fonctionnalités et améliorations de la version de février 2018 de Brand Portal{#brand-portal-features-and-enhancements-632}

Les nouvelles fonctionnalités cherchent à aligner Brand Portal sur AEM.

>[!VIDEO](https://video.tv.adobe.com/v/26354?quality=12&learn=on)

### Améliorations de la navigation

* Refonte de l’interface utilisateur avec la bibliothèque Coral3, similaire à AEM.
* Accès rapide et facile aux outils d’administration par le biais du nouveau logo Adobe.
* Navigation dans le produit à travers un recouvrement.
* Navigation rapide vers les dossiers parents à partir d’un dossier enfant.
* Option Omnisearch pour accéder aux outils et au contenu administratifs.
* Modes Carte, Liste et Colonnes pour vous aider à parcourir facilement les dossiers imbriqués.
* Tri des ressources en mode Liste qui n’est plus limité au nombre de ressources affichées à l’écran. Toutes les ressources d’un dossier sont triées.

### Améliorations de la recherche

* La fonctionnalité Omnisearch vous permet d’effectuer une recherche rapide de ressources et de fichiers dans Brand Portal.
* Omnisearch permet également de rechercher des ressources dans un dossier ou un emplacement spécifique.
* Suggestions de mots-clés automatiques pour faciliter la recherche.
* Ajoutez des filtres à Omnisearch pour plus d’efficacité. Option permettant d’enregistrer les résultat de recherche dans une collecte dynamique afin de pouvoir les consulter ultérieurement.
* Prise en charge de la recherche de ressources avec balisage intelligent.
* Les ressources AEM avec balisage intelligent peuvent être partagées d’AEM vers Brand Portal et utiliser des balises intelligentes pour la recherche de ressources dans Brand Portal.

### Améliorations du partage de fichiers

* L’utilisateur ou l’utilisatrice peut partager une ressource à l’aide de l’option de partage de lien.
* Lors du partage de ressources, l’utilisateur ou l’utilisatrice peut définir une date d’expiration pour chaque ressource. Cela permet aux utilisateurs et aux utilisatrices d’exercer un contrôle accru sur les ressources partagées.
* Un utilisateur ou une utilisatrice externe disposant du lien de partage de ressources peut télécharger l’image et afficher ses propriétés.
* La hiérarchie des dossiers imbriqués d’origine est conservée pour les dossiers de ressources téléchargés.

### Fonctionnalités administratives et de création de rapports

* Le schéma de métadonnées d’AEM Assets peut désormais être publié d’AEM vers Brand Portal.
* Les administrateurs et administratrices peuvent créer et gérer trois types de rapports : sur les ressources téléchargées, sur celles expirées et enfin sur celles publiées.
* Possibilité de configurer la colonne à inclure dans le rapport.
* Création de paramètres d’image prédéfinis pour les ressources dans Brand Portal.
* Possibilité de modifier le formulaire du rail de recherche d’administration ou le formulaire de recherche afin d’inclure des options de filtrage supplémentaires.
* Mise à jour et prévisualisation du papier peint personnalisé pour votre marque.
* Rapport d’utilisation pour en savoir plus sur le nombre d’utilisateurs et d’utilisatrices, l’espace de stockage utilisé et le total des ressources.

## Ressources supplémentaires{#additional-resources}

* [Nouveautés de Brand Portal](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/introduction/whats-new.html?lang=fr#introduction)
* [Agents de réplication de l’instance de création AEM](https://helpx.adobe.com/fr/experience-manager/6-5/assets/using/brand-portal-configuring-integration.html)
* [Guide de téléchargement accéléré](https://experienceleague.adobe.com/docs/experience-manager-brand-portal/using/download/accelerated-download.html?lang=fr)
* [Documents sur Adobe AEM Assets Brand Portal](https://helpx.adobe.com/fr/experience-manager/brand-portal/using/brand-portal.html)
* [Documents sur Adobe AEM Assets Dynamic Media](https://experienceleague.adobe.com/docs/?lang=fr)
* [Télécharger Aspera Connect](https://downloads.asperasoft.com/connect2/)
* [Serveur de test Aspera Connect](https://test-connect.asperasoft.com/)
