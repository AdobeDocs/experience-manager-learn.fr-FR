---
title: Guide sur la hiérarchie, la taxonomie et le balisage du site
seo-title: AEM Sites Site Hierarchy, Taxonomy, and Tagging Guide
description: Présentation complète des métadonnées, du balisage, de la taxonomie et de la hiérarchie AEM Sites. Utilisez ce guide pour vous assurer que votre stratégie de contenu est cohérente et suivez les bonnes pratiques
seo-description: A full overview of AEM Sites metadata, tagging, taxonomy, and hierarchy. Use this guide to ensure your content strategy is consistent and following best practices
audience: author, marketer
source-git-commit: d545e7bb5e937959e2ede2b3c1ecfc312df5a044
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 0%

---


# Bonnes pratiques relatives aux balises, à la taxonomie et aux métadonnées : Résumé de haut niveau

Les métadonnées et les balises sont essentielles pour accroître l’efficacité d’AEM. Les utilisateurs, les dirigeants et la direction se rendent compte de la nécessité d&#39;une stratégie globale, mais ils ont du mal à progresser. Souvent, les connaissances sont cloisonnées parmi les utilisateurs, ce qui rend difficile une stratégie holistique et rend les ajustements encore plus problématiques.

Quelle est la différence entre les métadonnées et les balises ? Quels sont les aspects commerciaux à prendre en compte lors de la conduite de votre stratégie ?

## Quel est l’objectif des métadonnées ?

Les métadonnées ajoutent une structure au contenu moins structuré.
Exemple : Une image de base comporte des pixels. Nous pouvons appeler ces &quot;données de base&quot;. Il s’agit des métadonnées qui décrivent le format, la catégorie, les détails de licence, etc.
Les métadonnées sont le plus souvent utilisées pour les ressources. Il existe toutefois un grand nombre de cas d’utilisation des métadonnées dans les pages de contenu ou les fragments d’expérience.

## Sources de métadonnées

Voici les catégories pour lesquelles des métadonnées peuvent être générées :

* Métadonnées extraites : les informations sont déjà disponibles dans le document, par exemple en langage naturel.
* Métadonnées dérivées : les informations ne sont pas disponibles dans les données d’origine, mais elles peuvent être dérivées en faisant référence à des connaissances antérieures.
* Métadonnées ajoutées manuellement : il s’agit de métadonnées qui ne figurent dans aucune des premières catégories et qui doivent être ajoutées manuellement par un humain.

## Types de métadonnées

Dans les catégories répertoriées ci-dessus, il existe quatre types principaux :

* Métadonnées techniques et descriptives : Fournit des informations sur les détails techniques du contenu (ex. tite, langue, etc.)
* Métadonnées opérationnelles : Documents le cycle de vie d’une ressource (c’est-à-dire approuvée, dans la création, la campagne)
* Métadonnées d’administration : État ou propriété d’une ressource au sein d’une organisation (informations de licence, propriété, par exemple)
* Métadonnées structurelles : Permet de classer les ressources ou les pages pour faciliter les processus d’entreprise (s’applique à la plupart des balises et taxonomies).

## Noms de dossiers et de fichiers

Les dossiers sont un moyen naturel de parcourir le contenu dans AEM. Comment vos parties prenantes vont-elles interagir avec AEM ? Cela détermine la structure de vos dossiers. Structure de dossiers normalement conçue en tenant compte d’un ou deux des éléments suivants :

* Navigation
* Navigation
* Categorization
* Contrôle d’accès

Pour AEM Sites, la navigation est essentielle. Les dossiers permettent de contrôler l’accès aux ressources et aux pages.

Quels niveaux d’auteurs vont avoir besoin d’accéder aux pages d’accueil ? Qu’en est-il des pages de produits ? Ou campagne ? Utilisez les autorisations et la structure de dossiers pour mettre en place la bonne gouvernance.

## Stockage des métadonnées

Il existe trois méthodes pour stocker les métadonnées :

* Binaire : Format binaire lié à la nature de la ressource (Photoshop, InDesign, PNG, JPG).
* Noeud de ressource : Il s’agit de métadonnées sur la ressource elle-même, quel que soit le système ou le processus utilisé.
* Emplacement externe : Métadonnées qui ne se trouvent pas directement sur la ressource, mais qui peuvent être utilisées comme descripteur de &quot;l’état&quot; d’une ressource (exemple : un workflow qui peut affecter une ressource mais ne lui est pas directement appliqué)

## Modèle de métadonnées

La structure de la capture et du formatage des métadonnées est appelée modèle de métadonnées ou schéma de métadonnées. Cela doit être convenu avant que les ressources ou les pages ne soient ingérées dans le système.

Un modèle de métadonnées est généralement conçu pour répondre aux cas d’utilisation suivants :

* Recherche et récupération : Permet de stocker les aspects clés du contenu qui facilitent la récupération par les entreprises.
* Réutiliser : Permet d’utiliser les anciennes ressources à des fins de réutilisation (gain de temps et d’argent).
* Gestion des licences : Effectue le suivi de la propriété de l’entreprise de la ressource (souvent pour des raisons juridiques)
* Distribuer : Rendre le contenu disponible pour les consommateurs ou diviser les ressources en ressources pour les partenaires commerciaux.
* Archive : Métadonnées qui indiquent qu’une ressource est obsolète (il est toujours recommandé de placer un indicateur &quot;archivé&quot; sur la ressource afin de ne pas perdre des informations vitales)/
* Référence croisée : Métadonnées associées qui capture la relation entre plusieurs ressources (la synthèse des métadonnées permet un référencement croisé et une organisation de groupe cohérente)
* Naviguer : La structure de dossiers dans laquelle les ressources sont stockées (utilisée pour récupérer des informations lors de la navigation)

*Les métadonnées de création prennent principalement en charge les processus opérationnels. La publication prend en charge les cas d’utilisation de récupération et de distribution.*

## Utilisation de balises comme termes prédéfinis

Une balise est un mot-clé ou un terme attribué à un élément d’information. Par exemple, au lieu de saisir &quot;voiture&quot;, &quot;véhicule&quot;, &quot;automobile&quot;, un système de balises ne permet de choisir qu’une seule valeur, ce qui rend la recherche plus prévisible.  Les balises normalisent et simplifient la catégorisation des ressources.

*Remarque : Bien qu’AEM autorise le balisage ad hoc, il est recommandé de ne pas s’en préoccuper, car cela peut entraîner une taxonomie indéfinie et peu maniable.*

Utilisations courantes des balises :

* Recherche de mots-clés : Une balise peut décrire qu’une ressource appartient à un certain groupe d’entités. Par exemple, une balise &quot;image/sujet/voiture&quot; décrit la ressource appartenant au jeu d’images qui affichent une voiture.
* Motivation des relations : Toutes les ressources partageant la même balise peuvent être considérées comme connectées. Le balisage au lieu de la liaison directe est particulièrement utile sur les sites web comportant beaucoup de contenu dynamique et connecté.
* Navigation sur le lecteur : Les balises classées dans une taxonomie hiérarchique peuvent créer une navigation, un lien ou vers des documents similaires.
Les balises doivent également être considérées comme des informations qui relient différents types de données, en fonction des termes de l’entreprise plutôt que des propriétés techniques.

## Applications courantes des balises

Lorsqu’elles sont utilisées dans AEM, les balises peuvent aider à obtenir une mise en oeuvre beaucoup plus courte de la fonctionnalité complexe, par exemple :

* Recherche facettée
* Navigation personnalisée
* Contenu connexe
* Références du contenu
* Optimisation du moteur de recherche
* Mise en évidence des concepts clés

## Taxonomies

Une taxonomie est un système d&#39;organisation des balises en fonction de caractéristiques communes, généralement structurées de manière hiérarchique selon les besoins de l&#39;organisation. La structure peut aider à trouver plus rapidement une balise ou imposer une généralisation.
Exemple : Il est nécessaire de sous-catégoriser l&#39;imagerie boursière des voitures.  La taxonomie peut se présenter comme suit :

/subject/car/ /subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Désormais, l&#39;utilisateur peut choisir s&#39;il souhaite rechercher des images de sport en général ou d&#39;une &quot;Porsche&quot; en particulier. Après tout, les deux sont des voitures de sport.
Bonne pratique : Évitez les taxonomies plates. Les taxonomies plats ne présentent pas les avantages décrits ci-dessus et nécessitent une maintenance constante.

**Utiliser une taxonomie comme thésaurus.**  Lorsqu’un utilisateur recherche un mot-clé, le système crée une seconde recherche pour tous les synonymes qui y figurent.
En outre, au lieu de saisir &quot;voiture&quot; manuellement, le système peut fournir une liste de mots-clés pour améliorer la cohérence.

**Utiliser une taxonomie comme dictionnaire.** Au lieu de simplement imprimer &quot;voiture&quot;, vous pouvez développer la balise unique et utiliser tous les synonymes de la balise.

**Catégories multiples.** Contrairement à une hiérarchie de dossiers, les balises peuvent être utilisées pour exprimer plusieurs catégories simultanément. Une ressource balisée avec :

/subject/car/minivan/mercedes /subject/people/family /color/red

## Métadonnées ou balise

Toutes les métadonnées ne doivent pas être considérées comme candidates au système de balisage. Les métadonnées techniques peuvent, inutilement, dupliquer les informations. Le meilleur candidat pour les balises est les métadonnées commerciales. .Les balises sont un bon choix pour appliquer un vocabulaire cohérent, une recherche facettée et une navigation cohérente.

## Gestion des balises

La gestion des balises bénéficie des avantages d’une équipe principale dédiée. Les nouveaux membres doivent d’abord connaître l’objectif et le fonctionnement de la taxonomie avant d’ajouter de nouvelles balises.  Des experts chevronnés, agissant comme gardiens de nouvelles balises, réduiront les incohérences à long terme.

## Création de balises

Les auteurs de contenu et les utilisateurs finaux doivent recourir à la taxonomie pour la comprendre. Elles doivent être créées avant le processus de création de contenu. Tout raccourci entraînera des efforts supplémentaires pour la gestion et la maintenance.

## Maintenance en cours

Les choses changent et les besoins de la liste de balises aussi. Proposez un processus de maintenance efficace qui réduira la duplication.

Assurez-vous que les contributeurs de contenu savent comment ils peuvent proposer des modifications et que les éditeurs ou les gestionnaires de contenu les révisent régulièrement.

## Bonnes pratiques relatives aux balises et aux taxonomies

**Normaliser les balises.** Créez un glossaire qui fournit un vocabulaire faisant autorité. Sans l&#39;établissement de normes, la duplication posera des problèmes. En outre, il est conseillé de contrôler non seulement la taxonomie, mais également l’utilisation des balises.

**Ne surmarquez pas.** Les balises peuvent perdre leur signification si elles sont trop souvent distribuées.Supprimez les balises superflues pour une efficacité optimale.

**Réévaluer les balises au fil du temps.** Souvenez-vous que la terminologie et le contexte commercial restent rarement statiques. Il peut s’avérer nécessaire de normaliser à nouveau et de réappliquer des balises.

**Utilisation du balisage intelligent optimisé par l’IA.** Balisage intelligent [voir lien] est une fonctionnalité d’IA d’AEM permettant de réduire les efforts de balisage manuel des ressources. Le balisage intelligent utilise une IA pour déduire des informations sur l’objet d’une image. Elle génère des balises descriptives qui décrivent le contenu d’une image.

## Qualité et maintenance des métadonnées

Comprendre les besoins de l’entreprise est une étape importante dans l’exécution d’un modèle de gestion des métadonnées. Sans définition, les informations ne peuvent pas être stockées. Il sera nécessaire de revoir régulièrement le modèle. Il s’agit d’une activité de contrôle qualité vitale.

En outre, les métadonnées doivent être capturées le plus tôt possible dans le processus de création de contenu. Si les métadonnées ne sont pas appliquées au bon moment, il y a peu de chances qu’elles soient appliquées rétroactivement.

**Utilisation des métadonnées** pour améliorer la collaboration : Utilisez Adobe Asset Link, Adobe Bridge et AEM Desktop pour associer les processus créatifs et utiliser les métadonnées afin de rationaliser les workflows créatifs. L’utilisation de ces outils permet d’enrichir les métadonnées et l’expérience utilisateur tout au long de votre processus de création.

## Bonnes pratiques relatives à la gestion des métadonnées

* Attribuer à l&#39;équipe principale un solide mandat exécutif : Formez une équipe de base de métadonnées qui comprend parfaitement l’écosystème de l’entreprise et dont la direction de l’entreprise a un mandat fort.
* Définition de la stratégie et de la gouvernance des métadonnées : Une bonne stratégie de métadonnées peut aider les entreprises à expliquer les besoins et les avantages des métadonnées.  Une stratégie se compose de schémas de métadonnées, de taxonomie, de processus d’entreprise (pour la qualité et la capture des données), de rôles et de responsabilités, ainsi que de processus de gouvernance. *
* Définition et communication d’un modèle de métadonnées cohérent : La stratégie et le raisonnement définis devraient être bien documentés et communiqués au sein des organisations.
* Convention d’affectation de nom standard : Créez une convention d’appellation des fichiers cohérente et descriptive pour améliorer la valorisation de la marque, la gestion des informations et la convivialité.
* Caractères de sécurité dans les noms de fichier : Le nom de fichier doit pouvoir être interprété par tous les systèmes d’exploitation courants. Vous pouvez utiliser en toute sécurité des caractères, des nombres, des trémas, des espaces et des traits de soulignement. Le symbole moins est également sûr, mais si vous coupez et collez, il peut ressembler à un &quot;tiret&quot;.
* Convention d’affectation de nom de version : AEM offre des fonctionnalités permettant de conserver les anciennes versions des ressources. Dans certains cas, vous pouvez conserver plusieurs versions. Cependant, vous devez vous assurer que le schéma de contrôle de version est cohérent.

## Métadonnées organisationnelles et descriptives

Quelques instructions peuvent vous aider à décider comment classer les métadonnées :

**Description** - Si les données décrivent la ressource ou l’élément de contenu, elles doivent faire partie des métadonnées jointes.

**Rechercher** - Si les métadonnées doivent être utilisées dans la recherche, elles doivent être jointes.

**Exposition** - Si vous exposez les métadonnées d’une plateforme de distribution à un tiers, veillez à ne pas exposer également les métadonnées &quot;internes&quot;.

**Durée** - Plus les métadonnées sont censées vivre longtemps, plus il est probable qu’elles soient un bon candidat pour les métadonnées jointes.

**Processus métier associés** - Il est absolument utile d’avoir un ID de produit permanent dans les métadonnées. Mais la catégorie d’un élément par rapport au catalogue de produits est une métadonnée discutable pour la ressource.

**Organisation et traitement** - Si la nature des métadonnées est de nature organisationnelle (état dans un workflow d’approbation ou propriété d’un service spécifique, par exemple), les métadonnées externes doivent être prises en compte lors de l’association des métadonnées à la ressource.

*Pour créer la stratégie, posez les questions suivantes :*

* Quel type de contenu et &quot;informations supplémentaires&quot; (= métadonnées) sont nécessaires pour résoudre les problèmes d’entreprise/de problèmes d’entreprise/d’entreprise ?
* Quelles sont les variables, les &quot;champs&quot; dans le schéma et quelles sont les valeurs possibles ? Les variables ont besoin d’une entrée en texte libre, qui peut être réduite par type (nombre, date, booléen, ...), un ensemble de valeurs fixes (par exemple pays) ou de balises d’une taxonomie donnée. Combien de balises sont requises, autorisées ?
* Quels problèmes/problèmes/questions techniques peuvent être résolus par les métadonnées ?
* Comment obtenir/créer ce contenu/ces métadonnées ? Combien coûtera l’obtention/la création de ces métadonnées ?
* Quels types de métadonnées sont nécessaires pour un groupe d’utilisateurs spécifique ?
* Comment les métadonnées seront-elles conservées et mises à jour ?
* Qui est responsable de quelle partie du processus ?
* Comment peut-on s&#39;assurer que les processus d&#39;entreprise convenus sont suivis ?
* Quelles normes devriez-vous suivre ? Vous devez adopter et modifier une norme du secteur (Dublin Core, ISO 19115, PRISM, etc.) ou l’organisation doit-elle créer ses propres normes ?
* Où est documentée la stratégie ? Comment pouvez-vous vous assurer que toutes les parties prenantes y ont accès ? Comment pouvez-vous vous assurer que le personnel nouvellement intégré respecte la norme convenue (par exemple, suivre une formation avant d’y accéder ?)
