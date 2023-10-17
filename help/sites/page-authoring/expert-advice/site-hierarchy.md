---
title: Guide sur la hiérarchie, la taxonomie et le balisage du site
seo-title: AEM Sites Site Hierarchy, Taxonomy, and Tagging Guide
description: Présentation complète des métadonnées, du balisage, de la taxonomie et de la hiérarchie AEM Sites. Utilisez ce guide pour vous assurer que votre stratégie de contenu est cohérente qu’elle correspond aux bonnes pratiques.
seo-description: A full overview of AEM Sites metadata, tagging, taxonomy, and hierarchy. Use this guide to ensure your content strategy is consistent and following best practices
audience: author, marketer
topic: Content Management
feature: Learn From Your Peers
role: Admin, User
level: Beginner, Intermediate
exl-id: c88c3ec7-9060-43e2-a6a2-d47bba6f7cf3
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '2034'
ht-degree: 100%

---

# Bonnes pratiques relatives aux balises, à la taxonomie et aux métadonnées : résumé général

Les métadonnées et les balises sont essentielles pour améliorer l’efficacité d’AEM. Les utilisateurs et utilisatrices, les personnes responsables et celles chargées de la direction sont conscients de la nécessité d’adopter une stratégie holistique, mais ils ont du mal à progresser. Souvent, les connaissances sont compartimentées entre les utilisateurs et utilisatrices, ce qui rend difficile une stratégie holistique et rend les ajustements encore plus problématiques.

Quelle est la différence entre les métadonnées et les balises ? Quels sont les aspects commerciaux à prendre en compte pour définir votre stratégie ?

## À quoi servent les métadonnées ?

Les métadonnées ajoutent une structure au contenu moins structuré.
Exemple : une image de base comporte des pixels. Nous pouvons les appeler « données de base ». Il s’agit des métadonnées qui décrivent le format, la catégorie, les détails de licence, etc.
Les métadonnées sont le plus souvent utilisées pour les ressources. Il existe toutefois un grand nombre de cas d’utilisation pour les métadonnées dans les pages de contenu ou les fragments d’expérience.

## Sources de métadonnées

Les métadonnées peuvent être obtenues des manières suivantes :

* Métadonnées extraites : les informations sont déjà disponibles dans le document, par exemple en langage naturel.
* Métadonnées dérivées : les informations ne sont pas disponibles dans les données d’origine, mais elles peuvent être dérivées en faisant référence à des connaissances antérieures.
* Métadonnées ajoutées manuellement : il s’agit de métadonnées qui ne figurent dans aucune des premières catégories et qui doivent être ajoutées manuellement par une personne.

## Types de métadonnées

Dans les catégories répertoriées ci-dessus, il existe quatre types principaux :

* Métadonnées techniques et descriptives : elles fournissent des informations sur les détails techniques du contenu (par exemple, le titre, la langue, etc.).
* Métadonnées opérationnelles : elles documentent le cycle de vie d’une ressource (par exemple, approuvée, en création, campagne).
* Métadonnées administratives : le statut ou l’état d’une ressource au sein d’une organisation (par exemple, les informations de licence, la propriété).
* Métadonnées structurelles : elles permettent de classer les ressources ou les pages pour faciliter la gestion commerciale (elles s’appliquent à la plupart des balises et taxonomies).

## Noms de dossiers et de fichiers

Les dossiers sont un moyen naturel de parcourir le contenu dans AEM. Comment vos parties prenantes vont-elles interagir avec AEM ? Cela détermine la structure de vos dossiers. La structure de dossiers est normalement conçue en tenant compte d’un ou deux des éléments suivants :

* Navigation
* Navigation
* Catégorisation
* Contrôle d’accès

Pour AEM Sites, la navigation est essentielle. Les dossiers permettent de contrôler l’accès aux ressources et aux pages.

Quels niveaux de création ont besoin d’accéder aux pages d’accueil ? Qu’en est-il des pages de produits ? Ou de la campagne ? Utilisez les autorisations et la structure de dossiers pour mettre en place la bonne gouvernance.

## Stocker les métadonnées

Il existe trois méthodes pour stocker les métadonnées :

* Binaire : format binaire lié à la nature de la ressource (Photoshop, InDesign, PNG, JPG).
* Nœud de ressource : il s’agit de métadonnées sur la ressource elle-même, quel que soit le système ou le processus utilisé.
* Emplacement externe : métadonnées qui ne se trouvent pas directement sur la ressource, mais qui peuvent être utilisées comme descripteur de « l’état » d’une ressource (par exemple, un workflow qui peut affecter une ressource mais ne s’y applique pas directement).

## Modèle de métadonnées

La structure de la capture et du formatage des métadonnées est appelée modèle de métadonnées ou schéma de métadonnées. Il doit être convenu avant que les ressources ou les pages ne soient ingérées dans le système.

Un modèle de métadonnées est généralement conçu pour répondre aux cas d’utilisation suivants :

* Recherche et récupération : aide à stocker les aspects clés du contenu qui facilitent la récupération par entreprise.
* Réutilisation : permet d’utiliser les anciennes ressources à des fins de réutilisation (gain de temps et d’argent).
* Gestion des licences : effectue le suivi de la propriété des ressources de l’entreprise (souvent pour des raisons juridiques).
* Distribution : rendez le contenu disponible pour les consommateurs et les consommatrices ou transmettez les ressources aux partenaires commerciaux.
* Archive : métadonnées qui indiquent qu’une ressource est obsolète (il est toujours recommandé de placer un indicateur « archivée » sur la ressource afin de ne pas perdre des informations essentielles).
* Référence croisée : métadonnées associées qui capturent la relation entre plusieurs ressources (la synthèse des métadonnées permet une référence croisée et une organisation de groupe cohérente).
* Navigation : structure de dossiers dans laquelle les ressources sont stockées (utilisée pour récupérer des informations par navigation).

*Les métadonnées de création prennent principalement en charge les processus opérationnels. La publication prend en charge les cas d’utilisation de récupération et de distribution.*

## Utiliser des balises comme termes prédéfinis

Une balise est un mot-clé ou un terme attribué à un élément d’information. Par exemple, au lieu de saisir « voiture », « véhicule », ou « automobile », un système de balises ne permet de choisir qu’une seule valeur, ce qui rend la recherche plus prévisible.  Les balises normalisent et simplifient la catégorisation des ressources.

*Remarque : bien qu’AEM autorise le balisage ad hoc, il est recommandé de ne pas l’utiliser, car cela peut entraîner une taxonomie indéfinie et peu maniable.*

Utilisations courantes des balises :

* Recherche de mots-clés : une balise peut indiquer qu’une ressource appartient à un certain groupe d’entités. Par exemple, une balise « image/sujet/voiture » décrit la ressource appartenant au jeu d’images qui affichent une voiture.
* Relations améliorées : toutes les ressources partageant la même balise peuvent être considérées comme connectées. Le balisage au lieu de la liaison directe est particulièrement utile sur les sites web comportant beaucoup de contenu dynamique et connecté.
* Navigation améliorée : les balises classées dans une taxonomie hiérarchique peuvent créer une navigation ou un lien vers des documents similaires.
Les balises doivent également être considérées comme des informations qui relient différents types de données, en fonction des termes de l’entreprise plutôt que des propriétés techniques.

## Applications courantes des balises

Lorsqu’elles sont utilisées dans AEM, les balises contribuent à une implémentation beaucoup plus courte d’une fonctionnalité complexe, par exemple :

* Recherche à facettes
* Navigation personnalisée
* Contenu connexe
* Références du contenu
* Optimisation du moteur de recherche (SEO)
* Mise en évidence des concepts clés

## Taxonomies

Une taxonomie est un système d’organisation des balises en fonction de caractéristiques communes, généralement structurées de manière hiérarchique selon les besoins de l’organisation. La structure peut aider à trouver plus rapidement une balise ou imposer une généralisation.
Exemple : il faut sous-catégoriser les images de stock des voitures.  La taxonomie peut se présenter comme suit :

/subject/car/
/subject/car/sportscar
/subject/car/sportscar/porsche
/subject/car/sportscar/ferrari
…
/subject/car/minivan
/subject/car/minivan/mercedes
/subject/car/minivan/volkswagen
…
/subject/car/limousine
…

La personne peut ainsi choisir si elle souhaite rechercher des images de voitures de sport (sportscars) en général ou d’une « Porsche » en particulier. Après tout, les deux sont des voitures de sport.
Bonne pratique : évitez les taxonomies plates. Les taxonomies plates ne présentent pas les avantages décrits ci-dessus et nécessitent une maintenance constante.

**Utiliser une taxonomie comme thésaurus.** Lorsqu’une personne recherche un mot-clé, le système crée une seconde recherche pour tous les synonymes qui y figurent.
En outre, au lieu de saisir « car » (voiture) manuellement, le système peut fournir une liste de mots-clés pour améliorer la cohérence.

**Utiliser une taxonomie comme dictionnaire.** Au lieu de simplement imprimer « car » (voiture), vous pouvez développer la balise et utiliser tous les synonymes de la balise.

**Plusieurs catégories.** Contrairement à une hiérarchie de dossiers, les balises peuvent être utilisées pour exprimer plusieurs catégories simultanément. Une ressource balisée avec :

/subject/car/minivan/mercedes
/subject/people/family
/color/red

## Métadonnées ou balise

Toutes les métadonnées ne doivent pas être considérées comme pouvant devenir des balises. Les métadonnées techniques peuvent, inutilement, dupliquer les informations. Les métadonnées d’entreprise se prêtent le mieux aux balises. Les balises permettent d’appliquer un vocabulaire commun, une recherche à facettes et une navigation cohérents.

## Gestion des balises

La gestion des balises tire tout son potentiel d’une équipe dédiée. Les nouvelles personnes membres doivent d’abord connaître l’objectif et le fonctionnement de la taxonomie avant d’ajouter de nouvelles balises.  Des expertes et experts chevronnés, agissant comme gardiens des nouvelles balises, réduiront les incohérences à long terme.

## Créer des balises

Les créateurs et créatrices de contenu doivent appliquer les taxonomies et les utilisateurs et utilisatrices finaux doivent la comprendre. Elles doivent être créées avant le processus de création de contenu. Tout raccourci entraînera des efforts supplémentaires en matière de gestion et de maintenance.

## Maintenance en cours

Les besoins évoluent, la liste de balises doit donc également être adaptée. Mettez en place un processus de maintenance efficace afin de réduire la duplication.

Assurez-vous que les contributeurs et contributrices de contenu savent comment proposer des modifications et que les éditeurs et les éditrices ou les personnes chargées de la gestion de contenu les révisent régulièrement.

## Bonnes pratiques relatives aux balises et aux taxonomies

**Normalisez les balises.** Créez un glossaire avec les termes de référence. Sans l’établissement de normes, la duplication posera des problèmes. En outre, il est conseillé de contrôler non seulement la taxonomie, mais également l’utilisation des balises.

**Ne balisez pas outre mesure.** Les balises peuvent perdre leur sens si elles sont appliquées sans discernement.Supprimez les balises superflues pour une efficacité optimale.

**Réévaluez les balises au fil du temps.** N’oubliez pas que la terminologie et le contexte de l’entreprise sont en constante évolution. Il peut s’avérer nécessaire de normaliser à nouveau et de réappliquer des balises.

**Utilisez le balisage intelligent optimisé par l’IA.** Le balisage intelligent [voir lien] est une fonctionnalité optimisée par l’IA d’AEM qui permet de réduire les efforts de balisage manuel des ressources. Le balisage intelligent utilise une IA pour déduire des informations sur le sujet d’une image. Il génère des balises descriptives qui décrivent le contenu d’une image.

## Qualité et maintenance des métadonnées

La compréhension des besoins de l’entreprise est une étape importante dans l’exécution d’un modèle de gestion des métadonnées. Sans définition, les informations ne peuvent pas être stockées. Il sera nécessaire de revoir régulièrement le modèle. Il s’agit d’une activité de contrôle qualité vitale.

En outre, les métadonnées doivent être capturées le plus tôt possible dans le processus de création de contenu. Si les métadonnées ne sont pas appliquées au bon moment, il y a peu de chances qu’elles soient appliquées rétroactivement.

**Utilisez les métadonnées** pour améliorer la collaboration : à l’aide d’Adobe Asset Link, d’Adobe Bridge et de l’appli de bureau AEM, associez les processus créatifs et utilisez les métadonnées pour rationaliser les workflows créatifs. L’utilisation de ces outils permet d’enrichir les métadonnées et l’expérience client tout au long de votre processus de création.

## Bonnes pratiques relatives à la gestion des métadonnées

* Donnez à l’équipe dédiée une grande marge de manœuvre : créez une équipe en charge des métadonnées qui comprend parfaitement l’écosystème de l’entreprise et accordez-lui les pleins pouvoirs.
* Définissez la stratégie et la gouvernance des métadonnées : une bonne stratégie en la matière peut aider les entreprises à expliquer le besoin et les avantages des métadonnées.  Une stratégie se compose de schémas de métadonnées, de taxonomie, de processus d’entreprise (pour la qualité et la capture des données), de rôles et de responsabilités, ainsi que de processus de gouvernance. *
* Définissez et diffusez un modèle de métadonnées cohérent : la stratégie et le raisonnement que vous avez définis doivent être bien documentés et communiqués au sein des organisations.
* Convention de nommage standard : créez une convention de nommage de fichiers cohérente et descriptive pour améliorer l’image de marque, la gestion de l’information et la convivialité.
* Caractères sécurisés dans les noms de fichier : le nom de fichier doit pouvoir être interprété par tous les systèmes d’exploitation courants. Vous pouvez utiliser des caractères, des nombres, des trémas, des espaces et des soulignements. Le symbole moins est également sûr, mais si vous coupez et collez, il peut ressembler à un « tiret ».
* Convention de nommage de version : AEM offre certaines fonctionnalités pour conserver les anciennes versions des ressources. Dans certains cas, la conservation de plusieurs versions peut s’avérer nécessaire. Cependant, vous devez vous assurer que le schéma de contrôle de version est cohérent.

## Métadonnées organisationnelles par rapport aux métadonnées descriptives

Consultez les conseils suivants pour classer les métadonnées :

**Description** : si les données décrivent la ressource ou l’élément de contenu, elles doivent faire partie des métadonnées jointes.

**Recherche** : si les métadonnées sont utilisées dans le cadre de la recherche, elles doivent être jointes.

**Exposition** : si vous affichez les métadonnées d’une plateforme de distribution à un tiers, veillez à ne pas afficher également les métadonnées « internes ».

**Durée** : plus la durée de vie des métadonnées est longue, plus elles se prêtent à constituer de bonnes métadonnées jointes.

**Processus d’entreprise associés** : il est utile, et c’est un euphémisme, d’avoir un ID de produit permanent dans les métadonnées. Mais la catégorie d’un élément par rapport au catalogue de produits est une métadonnée discutable pour la ressource.

**Organisation et traitement** : si la nature des métadonnées est organisationnelle (état dans un workflow d’approbation ou propriété d’un service spécifique, par exemple), les métadonnées externes doivent être prises en compte lors de l’association des métadonnées à la ressource.

*Lors de la création de la stratégie, posez-vous les questions suivantes :*

* Quel type de contenu et d’« informations supplémentaires » (= métadonnées) sont nécessaires pour résoudre les problèmes/questions/difficultés d’entreprise ?
* Quelles sont les variables, les « champs » du schéma et quelles sont les valeurs possibles ? Quelles variables doivent être saisies en texte libre, lesquelles peuvent être définies par leur type (nombre, date, booléen, ...), par un ensemble de valeurs fixes (par exemple les pays) ou par des balises provenant d’une taxonomie donnée. Combien de balises sont nécessaires et existe-t-il une limite au nombre de balises ?
* Quels sont les problèmes/questions/difficultés techniques qui peuvent être résolus par les métadonnées ?
* Comment obtenir ou créer ce contenu et ces métadonnées ? Quel est le coût de l’obtention et de la création de ces métadonnées ?
* Quels types de métadonnées sont nécessaires pour un groupe d’utilisateurs et d’utilisatrices spécifique ?
* Comment les métadonnées sont-elles conservées et mises à jour ?
* Qui sont les personnes compétentes à chaque étape du processus ?
* Comment s’assurer que les processus d’entreprise adoptés sont suivis ?
* Quelles normes devez-vous respecter ? Devez-vous adopter une norme du secteur et l’adapter (Dublin Core, ISO 19115, PRISM, etc.) ou l’organisation doit-elle créer ses propres normes ?
* Où consulter la stratégie ? Comment pouvez-vous vous assurer que toutes les parties prenantes y ont accès ? Comment garantir que les nouvelles personnes employées respectent la norme convenue (par exemple, suivre une formation avant d’y accéder) ?
