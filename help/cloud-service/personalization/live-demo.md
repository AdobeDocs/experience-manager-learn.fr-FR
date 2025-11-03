---
title: Démonstration en direct de cas d’utilisation de Personalization
description: Faites l’expérience de la personnalisation en action sur le site Web d’activation WKND avec des tests A/B, un ciblage comportemental et des exemples de personnalisation d’utilisateurs connus.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations
role: Developer, Architect, Leader, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-11-03T00:00:00Z
jira: KT-19546
thumbnail: KT-19546.jpeg
source-git-commit: 9e99936fb03e085f6bc276c7d6ef5cc08e34d1e5
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 3%

---

# Démonstration en direct de cas d’utilisation de Personalization

Visitez le [site web d’activation de WKND](https://wknd.enablementadobe.com/us/en.html){target="wknd"} pour consulter des exemples concrets de tests A/B, de ciblage comportemental et de personnalisation d’utilisateurs et utilisatrices connus.

>[!VIDEO](https://video.tv.adobe.com/v/3476461/?learn=on&enablevpops)

Cette page vous guide à travers des démonstrations pratiques de chaque scénario de personnalisation. Utilisez-le pour explorer les possibilités avant de créer ces fonctionnalités sur votre propre site AEM.

>[!IMPORTANT]
>
> Ouvrez le site de démonstration dans plusieurs fenêtres de navigateur ou en mode de navigation incognito/privée pour expérimenter différentes variations personnalisées simultanément.
> Lors de l’utilisation du mode de navigation privée, Firefox et Safari peuvent bloquer le cookie ECID, ou utiliser le mode de navigation standard ou effacer les cookies avant d’essayer un nouveau scénario de personnalisation.

## Cas d’utilisation de démonstration

Le [site Web d’activation de WKND](https://wknd.enablementadobe.com/us/en.html){target="wknd"} présente trois types de personnalisation :

| Type de Personalization | Ce que vous verrez | Minutage |
|---------------------|-----------------|---------|
| **Ciblage comportemental** | Le contenu s’adapte en fonction de votre comportement de navigation et de vos centres d’intérêt. Généralement appelée _personnalisation de la page suivante ou de la même page_ | Temps réel et par lots |
| **Personalization utilisateur connu** | Des expériences personnalisées basées sur des profils clients complets créés à partir de données sur plusieurs systèmes. Généralement appelée _personnalisation à grande échelle_ | Temps réel |
| **Tests A/B** | Différentes variations de contenu ont été testées pour trouver la meilleure performance. Généralement appelé _expérimentation_ | Temps réel |

## Ciblage comportemental

Le contenu s’adapte automatiquement en fonction des actions et des centres d’intérêt des visiteurs et visiteuses au cours de leur session de navigation. On parle généralement de _personnalisation de la page suivante ou de la même page_.

### Pages d’accueil, d’Adventures et de magazine

Ces expériences s’affichent immédiatement en fonction de votre comportement de navigation actuel (personnalisation en temps réel). Adobe Experience Platform Edge Network est utilisé pour prendre des décisions de personnalisation en temps réel.

| Page | Ce que vous verrez | Test | Expérience |
|------|-----------------|-------------|------------|
| [Accueil](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Une **bannière héroïque Adventures familiale** personnalisée mettant en vedette une famille faisant du vélo ensemble au bord d&#39;un lac, faisant la promotion d&#39;expériences guidées qui créent des souvenirs partagés | Visitez [Bali Surf Camp](https://wknd.enablementadobe.com/us/en/adventures/bali-surf-camp.html){target="wknd"} ou [Gastronomic Marais Tour](https://wknd.enablementadobe.com/us/en/adventures/gastronomic-marais-tour.html){target="wknd"}, puis retournez sur la page d&#39;accueil | ![Accueil - Family Adventures Hero](./assets/live-demo/behavioral-home-family-hero.png){width="200" zoomable="yes"} |
| [Adventures.](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un héros promotionnel du **« Free Bike Tune Up » axé sur le vélo** avec un message « We&#39;ve Got You Covered » et une offre d&#39;entretien de vélo gratuite de la part des partenaires experts de WKND | Visitez toutes les Adventures liées au vélo (par exemple, [Cycling Tuscany](https://wknd.enablementadobe.com/us/en/adventures/cycling-tuscany.html){target="wknd"}), puis accédez à la page Adventures | ![Adventures - Gratuit Vélo Tune Up Hero](./assets/live-demo/behavioral-adventures-bike-hero.png){width="200" zoomable="yes"} |
| [Adventures.](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un thème de camping **héros de la collection d&#39;équipement** mettant en valeur l&#39;équipement de camping essentiel (sacs de couchage, vestes, bottes) avec le message « Votre prochaine aventure commence avec le bon équipement » | Visitez toute aventure liée au camping (par exemple, [Yosemite Backpacking](https://wknd.enablementadobe.com/us/en/adventures/yosemite-backpacking.html){target="wknd"}), puis accédez à la page Adventures . | ![Adventures - Héros de la collection d&#39;engins de camping](./assets/live-demo/behavioral-adventures-camp-hero.png){width="200" zoomable="yes"} |
| [Magazine](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Une **promotion de vente de magazine** sensible au facteur temps, avec des magazines WKND laminés et une « VENTE » bien en vue. badges et prix spéciaux pour les lecteurs sur les numéros et collections extérieures | Lisez un ou plusieurs articles de magazine (par exemple, [Ski de randonnée](https://wknd.enablementadobe.com/us/en/magazine/ski-touring.html){target="wknd"}), puis accédez à la page de destination du magazine | ![Magazine - Sale Hero](./assets/live-demo/behavioral-magazine-sale-hero.png){width="200" zoomable="yes"} |

### Adventures et pages de magazine (lot)

Ces expériences sont basées sur le comportement historique et s’affichent lors de votre prochaine visite ou plus tard le même jour (personnalisation par lots). Les données sont agrégées et traitées dans des attributs de profil, puis activées dans Adobe Experience Platform Edge Network.

| Page | Ce que vous verrez | Test | Expérience |
|------|-----------------|-------------|------------|
| [Adventures.](https://wknd.enablementadobe.com/us/en/adventures.html){target="wknd"} | Un héros du surf avec des **planches de surf colorées sous les palmiers** avec un message « Votre Parcours de surf commence ici » et du contenu de destination de surf organisé en fonction de vos intérêts | Visitez plusieurs [aventures liées au surf](https://wknd.enablementadobe.com/us/en/adventures.html#tabs-b4210c6ff3-item-b411b19941-tab){target="wknd"}, puis revenez à la page Adventures le lendemain | ![Aventures - Surfing Hero](./assets/live-demo/behavioral-adventures-surfing-hero.png){width="200" zoomable="yes"} |
| [Magazine](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} | Une **offre d’abonnement personnalisée à un magazine** proposant des destinations de voyage dans le monde entier avec un van VW classique, mettant l’accent sur « Votre expérience personnalisée de magazine » avec des avantages exclusifs pour les abonnés | [&#x200B; Lisez 3 articles de magazine ou plus](https://wknd.enablementadobe.com/us/en/magazine.html){target="wknd"} puis revenez à la page de destination du magazine le lendemain | ![Magazine - S&#39;abonner au héros](./assets/live-demo/behavioral-magazine-subscribe-hero.png){width="200" zoomable="yes"} |

**En savoir plus :** vous êtes prêt à implémenter le ciblage comportemental sur votre propre site AEM ? Commencez par le [tutoriel sur le ciblage comportemental](./use-cases/behavioral-targeting.md) pour découvrir l’ensemble du processus de configuration.

## Personalization d’utilisateurs connus

Expériences personnalisées basées sur des profils client complets créés à partir de données sur plusieurs systèmes, y compris l’historique d’achats et l’étape du cycle de vie du client. Adobe Experience Platform Edge Network est utilisé pour prendre des décisions de personnalisation en temps réel.

### Héros de la page d’accueil

La bannière principale de la page d’accueil WKND est personnalisée en fonction des profils utilisateur authentifiés. Testez avec ces comptes de démonstration pour afficher une expérience personnalisée :

| Page | Ce que vous verrez | Test | Contexte du profil | Expérience |
|------|-----------------|-------------|-----------------|------------|
| [Accueil](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | Un intérieur de magasin de ski présentant des équipements de ski **premium avec promotion « EXTRA 25% OFF »**, avec des conseils d&#39;emballage d&#39;experts pour préparer leur prochaine aventure de ski | Connectez-vous avec `teddy/teddy` ou `asmith/asmith` et actualisez la page | Récemment acheté des aventures de ski, ce qui a encouragé la vente de matériel de ski | ![Accueil - Ventes d&#39;équipement de ski](./assets/live-demo/known-user-ski-gear-hero.png){width="200" zoomable="yes"} |

**En savoir plus :** êtes-vous prêt à implémenter la personnalisation d’un utilisateur connu sur votre propre site AEM ? Commencez par le [Tutoriel sur Personalization destiné aux utilisateurs connus](./use-cases/known-user-personalization.md) pour découvrir l’ensemble du processus de configuration.

## Tests A/B (Expérimentation)

Testez différentes variations de contenu pour déterminer celle qui répond le mieux aux objectifs de votre entreprise. Adobe Target diffuse de manière aléatoire différentes variations aux visiteurs et aux suivis, qui présentent de meilleures performances. On parle généralement d’_expérimentation_.

### Article en vedette de la page d’accueil

La page d’accueil WKND exécute un test A/B actif avec trois variantes de l’article présenté _Camping en Australie occidentale_. Chaque visiteur est affecté de manière aléatoire à l’affichage de l’une de ces variations :

| Page | Ce que vous verrez | Test | Expérience |
|------|-----------------|-------------|------------|
| [Accueil](https://wknd.enablementadobe.com/us/en.html){target="wknd"} | L&#39;une des trois variations d&#39;articles vedettes attribuées au hasard dans la section « Nos vedettes » : **« Hors de la grille : des itinéraires de camping épiques à travers l&#39;Australie occidentale »** ou **« Errer dans la nature : des aventures de camping en Australie occidentale »** (ou une troisième variation), chacune avec des images et des messages uniques pour tester celle qui résonne le mieux | Accédez à la page d’accueil dans différents navigateurs, utilisez le mode incognito/privé ou effacez les cookies pour afficher différentes variations | ![Variations de test A/B](./assets/live-demo/ab-test-variations.png){width="200" zoomable="yes"} |

**En savoir plus :** vous êtes prêt à implémenter des tests A/B sur votre propre site AEM ? Commencez par le tutoriel [Expérimentation (tests A/B)](./use-cases/experimentation.md) pour découvrir l’ensemble du processus de configuration.


## Étapes suivantes

Prêt à implémenter la personnalisation sur votre propre site AEM ? Commencez par la Présentation de [Personalization](./overview.md) pour découvrir l’ensemble du processus de configuration.


