---
title: Personalization d’utilisateurs connus
description: Découvrez comment personnaliser du contenu en fonction de données utilisateur connues à l’aide de Adobe Experience Platform et d’Adobe Target.
version: Experience Manager as a Cloud Service
feature: Personalization
topic: Personalization, Content Management, Integrations
role: Developer, Leader
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-10-17T00:00:00Z
jira: KT-16331
thumbnail: KT-16331.jpeg
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '6446'
ht-degree: 25%

---

# Personnalisation d’utilisateurs connus

Découvrez comment personnaliser le contenu en fonction de données utilisateur connues telles que l’historique des achats, les données CRM ou d’autres données collectées sur l’utilisateur.

La personnalisation des utilisateurs connus vous permet de proposer des expériences personnalisées aux utilisateurs en fonction des données que vous avez collectées à leur sujet. Les _données utilisateur auraient pu être collectées via différents systèmes_ ou canaux tels que le site web, l’application mobile, le centre d’appels, etc. Ces _données sont ensuite regroupées pour créer un profil utilisateur complet_ et utilisées pour personnaliser les expériences.

Voici quelques exemples de scénarios courants :

- **Personnalisation du contenu** : affichez des expériences personnalisées en fonction des données de profil de l’utilisateur. Par exemple, affichez un héros personnalisé sur la page d’accueil en fonction de l’historique d’achats de l’utilisateur.
- **Vente incitative et vente croisée** : affichez des recommandations personnalisées de vente incitative et de vente croisée basées sur l’historique des achats de l’utilisateur. Par exemple, affichez une recommandation de montée en gamme personnalisée pour l’historique des achats de l’utilisateur.
- **Programme de fidélité** : affichez les avantages personnalisés du programme de fidélité en fonction de l’historique d’achats de l’utilisateur. Par exemple, affichez un avantage de programme de fidélité personnalisé pour l’historique d’achats de l’utilisateur.

Votre organisation peut avoir différents cas d’utilisation pour la personnalisation d’utilisateurs connus. Voici quelques exemples.

## Exemple de cas d’utilisation

>[!VIDEO](https://video.tv.adobe.com/v/3476105/?learn=on&enablevpops)

Dans ce tutoriel, à l’aide de l’exemple de site WKND [](https://github.com/adobe/aem-guides-wknd), le processus montre comment **les utilisateurs connectés** qui ont acheté une Adventure **Ski** voient un héros personnalisé sur la page d’accueil **WKND**.

L&#39;expérience du héros tente de _vendre_ l&#39;équipement de ski essentiel aux utilisateurs qui ont acheté toute aventure **Ski**. Les utilisateurs qui n’ont acheté aucune Adventure **Ski** voient le contenu héros par défaut. Ainsi, l’expérience du héros est personnalisée pour les utilisateurs en fonction de leur historique d’achats et de leur statut de connexion. Pour activer cette personnalisation, les données de différents systèmes sont regroupées afin de créer un profil client complet et utilisées pour les activités de personnalisation.

![Personalization utilisateur connu](../assets/use-cases/known-user-personalization/personalized-hero-on-wknd-home-page.png)

### Gestion des données utilisateur sur plusieurs systèmes

À des fins de démonstration, supposons que les données utilisateur de WKND se trouvent dans les systèmes suivants. Chaque système stocke différents types de données qui peuvent être classés en deux catégories :

- **Données comportementales** : capture les interactions et les activités des utilisateurs sur les canaux numériques (pages vues, clics, navigation sur le site, statut de connexion, schémas de navigation).
- **Données transactionnelles** : enregistre les transactions commerciales terminées et les informations de profil client (achats, historique de commande, détails de profil, préférences).

| Système | Objectif | Quelles données sont stockées ? | Type de données |
|------|------|------|------|
| AEM | Système de gestion de contenu (CMS), listes et réservations d’Adventure, et fonctionnalité de connexion | Interactions utilisateur : pages vues, statut de connexion, navigation sur le site. Identifiants utilisateur minimaux tels que l’identifiant utilisateur, le nom, l’adresse e-mail. | Données comportementales |
| Autres systèmes | Les enregistrements de profil utilisateur et de transaction d’achat constituent un système d’enregistrement complet. | Profils client complets : identifiant utilisateur, nom, adresse, numéro de téléphone, historique d’achats, détails de commande, préférences. | Données transactionnelles |

L’autre système peut être un système Order Management (OMS), un système de gestion de la relation client (CRM), un système de gestion des données par Principal (MDM) ou tout autre système qui stocke les données transactionnelles.

Nous partons également du principe que le site WKND possède une interface utilisateur qui permet aux utilisateurs et utilisatrices d’acheter ou de réserver les **Adventures**. AEM est intégré à l’autre système pour stocker les données d’achat d’Adventure. En outre, avant ou pendant l’achat, l’utilisateur a créé un compte sur le site WKND.

Le diagramme logique montre l’interaction de l’utilisateur avec le site WKND et la manière dont les données comportementales et transactionnelles sont collectées et intégrées à Experience Platform.

![Personalization utilisateur connu](../assets/use-cases/known-user-personalization/logical-known-user-personalization.png)

Il s’agit d’une version simplifiée à l’extrême du concept de personnalisation d’utilisateurs connus. Dans un scénario réel, vous pouvez disposer de plusieurs systèmes dans lesquels les données comportementales et transactionnelles sont collectées et stockées.

### Principaux points à retenir

- **Stockage distribué des données** : les données utilisateur sont stockées sur plusieurs systèmes. AEM stocke un minimum de données utilisateur (identifiant utilisateur, nom, e-mail) pour la fonctionnalité de connexion, tandis que les autres systèmes (OMS, CRM, MDM) conservent l’intégralité du profil utilisateur et des données transactionnelles, telles que l’historique des achats.
- **Combinaison d’identités** : les systèmes sont liés à l’aide d’un identifiant commun (ID utilisateur WKND - `wkndUserId`) qui identifie de manière unique les utilisateurs sur différentes plateformes et canaux.
- **Création complète de profil** : l’objectif est d’assembler les données utilisateur de ces systèmes distribués pour créer un profil client unifié, qui est ensuite utilisé pour offrir des expériences personnalisées.

Votre cas d’utilisation peut avoir différents systèmes et stockage de données. La clé consiste à identifier un identifiant commun qui identifie de manière unique les utilisateurs sur différentes plateformes et canaux.

## Conditions préalables

Avant de poursuivre le cas d’utilisation de la personnalisation utilisateur connu, assurez-vous d’avoir terminé les étapes suivantes :

- [Intégrer Adobe Target](../setup/integrate-adobe-target.md) : permet aux équipes de créer et de gérer du contenu personnalisé de manière centralisée dans AEM et de l’activer en tant qu’offres dans Adobe Target.
- [Intégrer Tags dans Adobe Experience Platform](../setup/integrate-adobe-tags.md) : permet aux équipes de gérer et de déployer du JavaScript pour la personnalisation et la collecte de données sans avoir à redéployer de code AEM.

Familiarisez-vous également avec les concepts de [Adobe Experience Cloud Identity Service (ECID)](https://experienceleague.adobe.com/fr/docs/id-service/using/home) et de [Adobe Experience Platform](https://experienceleague.adobe.com/fr/docs/experience-platform/landing/home) tels que le schéma, le jeu de données, le flux de données, les audiences, les identités et les profils.

Dans ce tutoriel, vous en apprendrez plus sur la combinaison d’identités et la création d’un profil client dans Adobe Experience Platform. Ainsi, la combinaison des données comportementales avec les données transactionnelles permet de créer un profil client complet.

## Étapes avancées

Le processus de configuration de la personnalisation de l’utilisateur connu implique des étapes dans Adobe Experience Platform, AEM et Adobe Target.

1. **Dans Adobe Experience Platform :**
   1. Créez _Espace de noms d’identité_ pour l’ID utilisateur WKND (`wkndUserId`).
   1. Créez et configurez deux schémas XDM (modèle de données d’expérience) : des structures de données normalisées qui définissent la manière dont les données sont organisées et validées, l’un pour les données comportementales et l’autre pour les données transactionnelles
   1. Créez et configurez deux jeux de données, un pour les données comportementales et un pour les données transactionnelles
   1. Créer et configurer un flux de données
   1. Créer et configurer une propriété de balise
   1. Configurer une politique de fusion pour le profil
   1. Configurer une destination Adobe Target (V2)

2. **Dans AEM :**
   1. Améliorez la fonctionnalité Connexion au site WKND pour stocker l’identifiant utilisateur dans l’espace de stockage de session du navigateur.
   1. Intégrer et injecter la propriété Tags dans les pages AEM
   1. Vérifier la collecte de données sur les pages AEM
   1. Intégrer Adobe Target
   1. Création d’offres personnalisées

3. **Dans Adobe Experience Platform :**
   1. Vérifier les données comportementales et la création de profil
   1. Ingestion des données transactionnelles
   1. Vérifier l’assemblage des données comportementales et transactionnelles
   1. Créer et configurer une audience
   1. Activer l’audience vers Adobe Target

4. **Dans Adobe Target :**
   1. Vérifier les audiences et les offres
   1. Créer et configurer une activité

5. **Vérifiez l’implémentation de la personnalisation d’utilisateur connu sur vos pages AEM**

Les différentes solutions Adobe Experience Platform (AEP) sont utilisées pour collecter, gérer, identifier et regrouper les données utilisateur sur plusieurs systèmes. À l’aide des données utilisateur assemblées, les audiences sont créées et activées dans Adobe Target. Grâce aux activités dans Adobe Target, des expériences personnalisées sont diffusées aux utilisateurs et utilisatrices qui correspondent aux critères d’audience.

## Configuration de Adobe Experience Platform

Pour créer un profil client complet, il est nécessaire de collecter et de stocker des données comportementales (données de page vue) et transactionnelles (achats WKND Adventure). Les données comportementales sont collectées à l’aide de la propriété Tags et les données transactionnelles sont collectées à l’aide du système d’achat WKND Adventure.

Les données transactionnelles sont ensuite ingérées dans Experience Platform et regroupées avec les données comportementales afin de créer un profil client complet.

Dans cet exemple, pour classer un utilisateur qui a acheté une Adventure **Ski**, les données de page vue ainsi que les données d’achat d’Adventure sont nécessaires. Les données sont regroupées à l’aide de l’identifiant utilisateur WKND (`wkndUserId`), qui est un identifiant commun à tous les systèmes.

Commençons par nous connecter au Adobe Experience Platform pour configurer les composants nécessaires à la collecte et à l’assemblage des données.

Connectez-vous à [Adobe Experience Cloud](https://experience.adobe.com/) puis accédez à **Experience Platform** à partir de la section Sélecteur d’applications ou Accès rapide .

![Experience Platform.](../assets/use-cases/known-user-personalization/experience-platform.png)

### Créer un espace de noms d’identité

Un espace de noms d’identité est un conteneur logique qui fournit un contexte aux identités, ce qui permet à Experience Platform de comprendre le système d’identifiants utilisé (par exemple, e-mail, identifiant CRM ou identifiant de fidélité). Pour mettre en relation plusieurs éléments discrets de données de profil, un espace de noms d’identité est utilisé. Lorsque ces deux éléments distincts de données de profil ont la même valeur pour un attribut et partagent le même espace de noms, ils sont regroupés. Pour qualifier un attribut en tant qu’attribut d’assemblage d’identités, il doit appartenir au même espace de noms.

Dans cet exemple, l’identifiant utilisateur WKND (`wkndUserId`) est l’identifiant commun aux données comportementales et transactionnelles. À l’aide de cet identifiant commun, les données sont regroupées pour créer un profil client complet.

Créons un espace de noms d’identité pour l’identifiant utilisateur WKND (`wkndUserId`).

- Dans **Adobe Experience Platform**, cliquez sur **Identités** dans le volet de navigation de gauche. Cliquez ensuite sur le bouton **Créer un espace de noms d’identité** en haut à droite.

  ![Créer un espace de noms d’identité](../assets/use-cases/known-user-personalization/create-identity-namespace.png)

- Dans la boîte de dialogue **Créer un espace de noms d’identité**, saisissez ce qui suit :
   - **Nom d’affichage** : ID utilisateur WKND.
   - **Description** : ID utilisateur ou nom d’utilisateur de l’utilisateur WKND connecté
   - **Sélectionner un type** : identifiant individuel sur l’ensemble des appareils

  Cliquez sur **Créer** pour créer l’espace de noms d’identité.

  ![Créer un espace de noms d’identité](../assets/use-cases/known-user-personalization/create-identity-namespace-dialog.png)

### Création de schémas

Un schéma définit la structure et le format des données que vous collectez dans Adobe Experience Platform. Il garantit la cohérence des données et vous permet de créer des audiences significatives basées sur des champs de données normalisés. Pour la personnalisation d’utilisateurs connus, deux schémas sont nécessaires, l’un pour les données comportementales et l’autre pour les données transactionnelles.

#### Schéma De Données Comportementales

Créez tout d’abord un schéma pour collecter les données comportementales telles que les événements de page vue et les interactions utilisateur.

- Dans **Adobe Experience Platform**, cliquez sur **Schémas** dans le volet de navigation de gauche, puis cliquez sur le bouton **Créer un schéma** dans le coin supérieur droit. Sélectionnez ensuite l’option **Manuel**, puis cliquez sur le bouton **Sélectionner**.

  ![Créer un schéma](../assets/use-cases/known-user-personalization/create-schema.png)

- Dans l’assistant **Créer un schéma**, pour l’étape **Détails du schéma**, sélectionnez l’option **Événement d’expérience** (pour les données de série temporelle telles que les pages vues, les clics et les interactions utilisateur) et cliquez sur **Suivant**.

  ![Assistant Créer un schéma](../assets/use-cases/known-user-personalization/create-schema-details.png)

- Pour l’étape **Nommer et vérifier**, saisissez les informations suivantes :
   - **Nom d’affichage du schéma** : WKND-RDE-Known-User-Personalization-Behavioural
   - **Classe sélectionnée** : XDM ExperienceEvent

  ![Détails du schéma](../assets/use-cases/known-user-personalization/create-schema-name-review.png)

- Mettez à jour le schéma comme suit :
   - **Ajouter un groupe de champs** : AEP Web SDK ExperienceEvent
   - **Profil** : Activer

  Cliquez sur **Enregistrer** pour créer le schéma.

  ![Mettre à jour le schéma](../assets/use-cases/known-user-personalization/update-schema.png)

- Pour savoir si l’utilisateur est connecté (authentifié) ou anonyme, ajoutez un champ personnalisé au schéma. Dans ce cas d’utilisation, l’objectif est de personnaliser le contenu pour les utilisateurs connus qui ont acheté une Adventure **Ski**. Il est donc important d’identifier si l’utilisateur est connecté (authentifié) ou anonyme.


   - Cliquez sur le bouton **+** en regard du nom du schéma.
   - Dans la section **Propriétés du champ** , saisissez ce qui suit :
      - **Nom du champ** : wkndLoginStatus
      - **Nom d’affichage** : statut de connexion de WKND.
      - **Type** : chaîne
      - **Affecter à** : Groupe de champs > `wknd-user-details`

     Faites défiler vers le bas et cliquez sur le bouton **Appliquer**.

     ![Ajouter un champ personnalisé](../assets/use-cases/known-user-personalization/add-custom-field.png)

- Le schéma de données comportementales final doit se présenter comme suit :

  ![Schéma final](../assets/use-cases/known-user-personalization/final-schema.png)

#### Schéma De Données Transactionnelles

Créez ensuite un schéma pour collecter les données transactionnelles comme les achats WKND Adventure.

- Dans l’assistant **Créer un schéma**, pour **Détails du schéma**, sélectionnez l’option **Profil individuel** (pour les données basées sur les enregistrements telles que les attributs du client, les préférences et l’historique d’achats) et cliquez sur **Suivant**.

  ![Assistant Créer un schéma](../assets/use-cases/known-user-personalization/create-transactional-schema.png)

- Pour l’étape **Nommer et vérifier**, saisissez les informations suivantes :
   - **Nom d’affichage du schéma** : WKND-RDE-Known-User-Personalization-Transactionnel.
   - **Classe sélectionnée** : XDM Individual Profile

  ![Détails du schéma](../assets/use-cases/known-user-personalization/create-transactional-schema-name-review.png)

- Pour stocker les détails d’achat de l’Adventure WKND d’un utilisateur ou d’une utilisatrice, ajoutons tout d’abord un champ personnalisé qui sert d’identifiant pour l’achat. N’oubliez pas que l’identifiant utilisateur WKND (`wkndUserId`) est l’identifiant commun à tous les systèmes.
   - Cliquez sur le bouton **+** en regard du nom du schéma.
   - Dans la section **Propriétés du champ** , saisissez ce qui suit :
      - **Nom du champ** : wkndUserId
      - **Nom d’affichage** : ID d’utilisateur WKND.
      - **Type** : chaîne
      - **Affecter à** : Groupe de champs > `wknd-user-purchase-details`

  ![Ajouter un champ personnalisé](../assets/use-cases/known-user-personalization/add-custom-identity.png)

   - Faites défiler vers le bas, cochez **Identité**, cochez **Identité du Principal** (l’identifiant principal utilisé pour regrouper des données provenant de différentes sources dans un profil unifié) et dans la liste déroulante **Espace de noms d’identité** sélectionnez **Identifiant utilisateur WKND**. Enfin, cliquez sur le bouton **Appliquer**.

  ![Ajouter un champ personnalisé](../assets/use-cases/known-user-personalization/add-custom-identity-as-primary.png)

- Après avoir ajouté le champ Identité principale personnalisée , le schéma doit se présenter comme suit :

  ![Schéma final](../assets/use-cases/known-user-personalization/final-transactional-schema.png)

- De même, ajoutez les champs suivants pour stocker des détails d’achat d’utilisateur et d’Adventure supplémentaires :

  | Nom du champ | Nom d’affichage | Type | Affecter à |
  |----------|------------|----|---------|
  | adventurePurchased | Adventure Purchased | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | adventurePurchaseAmount | Montant d’achat de l’Adventure. | Double | Groupe de champs > `wknd-user-purchase-details` |
  | adventurePurchaseQuantity | Quantité d’achat d’Adventure. | Entier | Groupe de champs > `wknd-user-purchase-details` |
  | adventurePurchaseDate | Date d’achat de l’Adventure. | Date | Groupe de champs > `wknd-user-purchase-details` |
  | adventureStartDate | Date de début de l’Adventure | Date | Groupe de champs > `wknd-user-purchase-details` |
  | adventureEndDate | Date de fin de l’Adventure. | Date | Groupe de champs > `wknd-user-purchase-details` |
  | firstName | Prénom | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | lastName | Nom | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | email | E-mail | Adresse e-mail | Groupe de champs > `wknd-user-purchase-details` |
  | téléphone | Téléphone | Objet | Groupe de champs > `wknd-user-purchase-details` |
  | sexe | Genre | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | age | Âge | Entier | Groupe de champs > `wknd-user-purchase-details` |
  | adresse | Adresse | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | city | Ville | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | state | État | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | pays | Pays | Chaîne | Groupe de champs > `wknd-user-purchase-details` |
  | zipCode | Code postal | Chaîne | Groupe de champs > `wknd-user-purchase-details` |

  ![Champs supplémentaires](../assets/use-cases/known-user-personalization/additional-fields.png)

- Activez le Profil pour le schéma.

  ![Activer le profil](../assets/use-cases/known-user-personalization/enable-profile.png)

Vous avez maintenant créé les deux schémas pour les données comportementales et transactionnelles.

### Création et configuration de jeux de données

Un jeu de données est un conteneur pour les données qui suit un schéma spécifique. Dans cet exemple, créez deux jeux de données, l’un pour les données comportementales et l’autre pour les données transactionnelles.

#### Jeu De Données Comportemental

- Dans **Adobe Experience Platform**, cliquez sur **Jeux de données** dans le volet de navigation de gauche, puis cliquez sur le bouton **Créer un jeu de données** dans le coin supérieur droit. Sélectionnez ensuite l’option **Basée sur un schéma** et cliquez sur **Suivant**.

  ![Créer un jeu de données](../assets/use-cases/known-user-personalization/create-behavioral-dataset.png)

- Pour l’étape **Sélectionner un schéma**, sélectionnez le schéma **WKND-RDE-Known-User-Personalization-Behavioural** et cliquez sur **Suivant**.

  ![Sélectionner un schéma](../assets/use-cases/known-user-personalization/select-behavioral-schema.png)

- Pour l’étape **Configurer le jeu de données**, saisissez les informations suivantes :
   - **Name** : WKND-RDE-Known-User-Personalization-Behavioural.
   - **Description** : jeu de données pour les données comportementales telles que les pages vues avec statut de connexion de l’utilisateur.

  ![Configurer le jeu de données](../assets/use-cases/known-user-personalization/configure-behavioral-dataset.png)

  Cliquez sur **Terminer** pour créer le jeu de données.

- Activez le bouton (bascule) **Profil** pour activer le jeu de données pour Profil.

  ![Activer le profil](../assets/use-cases/known-user-personalization/enable-behavioral-profile.png)

#### Jeu de données de données transactionnelles

- Répétez les mêmes étapes pour le jeu de données transactionnel. La seule différence est le nom du schéma et du jeu de données.

   - **Schéma** : WKND-RDE-Known-User-Personalization-Transactionnel.
   - **Jeu de données** : WKND-RDE-Known-User-Personalization-Transactional
   - **Description** : jeu de données pour les données transactionnelles telles que les achats WKND Adventure.
   - **Profil** : Activer

  Le jeu de données de données transactionnelles final doit ressembler à ceci :

  ![Jeu De Données Transactionnel Final](../assets/use-cases/known-user-personalization/final-transactional-data-dataset.png)

Une fois les deux jeux de données en place, vous pouvez créer un flux de données pour activer le flux de données entre votre site web et Experience Platform.

### Créer et configurer un flux de données

Un flux de données est une configuration qui définit le flux de données de votre site web vers Adobe Experience Platform via le SDK web. Il agit comme un Bridge entre votre site web et la plateforme, en s’assurant que les données sont correctement formatées et acheminées vers les jeux de données corrects. Pour une personnalisation utilisateur connue, activez des services tels que Segmentation Edge et Destinations Personalization.

Créons un flux de données pour envoyer les données _comportementales_ (et non transactionnelles) à Experience Platform via le SDK Web.

- Dans **Adobe Experience Platform**, cliquez sur **Flux de données** dans le volet de navigation de gauche, puis cliquez sur **Créer un flux de données**.

  ![Créer un flux de données](../assets/use-cases/known-user-personalization/create-behavioral-datastream.png)

- À l’étape **Nouveau flux de données**, saisissez les informations suivantes :

   - **Name** : WKND-RDE-Known-User-Personalization-Behavioural.
   - **Description** : flux de données pour envoyer des données comportementales à Experience Platform
   - **Schéma de mappage** : WKND-RDE-Known-User-Personalization-Behavioural

  ![Configurer le flux de données](../assets/use-cases/known-user-personalization/configure-behavioral-datastream.png)

  Cliquez sur **Enregistrer** pour créer le flux de données.

- Une fois le flux de données créé, cliquez sur **Ajouter un service**.

  ![Ajouter un service](../assets/use-cases/known-user-personalization/add-service.png)

- À l’étape **Ajouter un service**, sélectionnez **Adobe Experience Platform** dans la liste déroulante et saisissez les informations suivantes :
   - **Jeu de données d’événement** : WKND-RDE-Known-User-Personalization-Behavioural
   - **Jeu de données de profil** : WKND-RDE-Known-User-Personalization-Behavior
   - **Offer Decisioning** : activer (permet à Adobe Target de demander et de diffuser des offres personnalisées en temps réel)
   - **Segmentation Edge** : activez (évalue les audiences en temps réel sur le réseau Edge pour une personnalisation immédiate)
   - **Destinations Personalization** : activez (permet le partage d’audience avec des outils de personnalisation tels qu’Adobe Target).

  Cliquez sur **Enregistrer** pour ajouter le service.

  ![Configurer le service Adobe Experience Platform](../assets/use-cases/known-user-personalization/configure-adobe-experience-platform-service.png)

- À l’étape **Ajouter un service**, sélectionnez **Adobe Target** dans la liste déroulante et saisissez l’**ID d’environnement cible**. Vous pouvez retrouver l’ID d’environnement cible dans Adobe Target sous **Administration** > **Environnements**. Cliquez sur **Enregistrer** pour ajouter le service.
  ![Configuration du service Adobe Target](../assets/use-cases/known-user-personalization/configure-adobe-target-service.png)

- Le flux de données final doit se présenter comme suit :

  ![Flux de données final](../assets/use-cases/known-user-personalization/final-behavioral-datastream.png)

Le flux de données est maintenant configuré pour envoyer des données comportementales à Experience Platform via le SDK Web.

Notez que les données _transactionnelles_ sont ingérées dans Experience Platform à l’aide de l’ingestion par lots (une méthode pour charger des jeux de données volumineux à des intervalles planifiés plutôt qu’en temps réel). Les données d’achat de WKND Adventure sont collectées à l’aide du site WKND et stockées dans l’autre système (par exemple, OMS ou CRM ou MDM). Les données sont ensuite ingérées dans Experience Platform par ingestion par lots.

Il est également possible d’ingérer ces données directement du site web vers Experience Platform, ce qui n’est pas abordé dans ce tutoriel. Le cas d’utilisation souhaite mettre en évidence le processus d’assemblage des données utilisateur entre les systèmes et de création d’un profil client complet.

## Création et configuration d’une propriété Tags

Une propriété Tags est un conteneur de code JavaScript qui collecte des données de votre site web et les envoie vers Adobe Experience Platform. Il agit comme la couche de collecte de données qui capture les interactions des utilisateurs et utilisatrices et les pages vues. Pour la personnalisation d’utilisateurs connus, avec les données de pages vues (par exemple, nom de page, URL, section du site et nom d’hôte), l’état de connexion de l’utilisateur et l’identifiant utilisateur WKND sont également collectés. L’ID utilisateur WKND (`wkndUserId`) est envoyé dans le cadre de l’objet de carte des identités.

Créons une propriété Balises qui capture les données de pages vues et le statut de connexion de l’utilisateur + l’ID d’utilisateur (s’il est connecté) lorsque les utilisateurs visitent le site WKND.

Vous pouvez mettre à jour la propriété Tags que vous avez créée à l’étape [Intégrer Adobe Tags](../setup/integrate-adobe-tags.md). Toutefois, pour simplifier le processus, une nouvelle propriété Tags est créée.

### Créer une propriété Tags

- Dans **Adobe Experience Platform**, cliquez sur **Tags** dans le volet de navigation de gauche, puis cliquez sur le bouton **Nouvelle propriété**.

  ![Création d’une propriété Tags](../assets/use-cases/known-user-personalization/create-new-tags-property.png)

- Dans la fenêtre **Créer une propriété**, saisissez les informations suivantes :
   - **Nom de la propriété** : WKND-RDE-Known-User-Personalization.
   - **Type de propriété** : sélectionnez **Web**.
   - **Domaine** : domaine dans lequel vous déployez la propriété (par exemple, `adobeaemcloud.com`).

  Cliquez sur **Enregistrer** pour créer la propriété.

  ![Création d’une propriété Tags](../assets/use-cases/known-user-personalization/create-new-tags-property-dialog.png)

- Ouvrez la nouvelle propriété, cliquez sur **Extensions** dans le volet de navigation de gauche, puis sur l’onglet **Catalogue**. Recherchez **SDK web** et cliquez sur le bouton **Installer**.
  ![Installation de l’extension SDK web](../assets/use-cases/known-user-personalization/install-web-sdk-extension.png)

- Dans la boîte de dialogue **Installer l’extension**, sélectionnez le **Flux de données** créé précédemment et cliquez sur **Enregistrer**.
  ![Sélection du flux de données](../assets/use-cases/known-user-personalization/select-datastream.png)

#### Ajouter des éléments de données

Les éléments de données sont des variables qui capturent des points de données spécifiques sur votre site web et les rendent disponibles pour une utilisation dans des règles et d’autres configurations de balises. Ils servent de blocs de création pour la collecte de données, ce qui vous permet d’extraire des informations significatives à partir des interactions d’utilisation et des pages vues. Pour la personnalisation d’utilisateurs connus, les détails de la page, tels que le nom d’hôte, la section du site et le nom de page, doivent être capturés pour créer des segments d’audience. En outre, le statut de connexion de l’utilisateur et l’ID utilisateur WKND (s’il est connecté) doivent être capturés.

Créez les éléments de données suivants pour capturer les détails importants de la page.

- Cliquez sur **Éléments de données** dans le volet de navigation de gauche, puis sur le bouton **Créer un élément de données**.
  ![Créer un élément de données](../assets/use-cases/known-user-personalization/create-new-data-element.png)

- Dans la boîte de dialogue **Créer une propriété**, saisissez les informations suivantes :
   - **Name** : Host Name
   - **Extension** : sélectionnez **Core**
   - **Data Element Type** : sélectionnez le bouton **Custom Code**
   - **Open Editor**, puis saisissez le fragment de code suivant :

     ```javascript
     if(window && window.location && window.location.hostname) {
         return window.location.hostname;
     }        
     ```

  ![Host Name Data Element](../assets/use-cases/known-user-personalization/host-name-data-element.png)

- De même, créez les éléments de données suivants :

   - **Name** : section du site
   - **Extension** : sélectionnez **Core**
   - **Data Element Type** : sélectionnez le bouton **Custom Code**
   - **Open Editor**, puis saisissez le fragment de code suivant :

     ```javascript
     if(event && event.component && event.component.hasOwnProperty('repo:path')) {
         let pagePath = event.component['repo:path'];
     
         let siteSection = '';
     
         //Check of html String in URL.
         if (pagePath.indexOf('.html') > -1) { 
         siteSection = pagePath.substring(0, pagePath.lastIndexOf('.html'));
     
         //replace slash with colon
         siteSection = siteSection.replaceAll('/', ':');
     
         //remove `:content`
         siteSection = siteSection.replaceAll(':content:','');
         }
     
         return siteSection 
     }        
     ```

  ![Élément de données de section Site](../assets/use-cases/known-user-personalization/site-section-data-element.png)

   - **Name** : le nom de la page
   - **Extension** : sélectionnez **Core**
   - **Data Element Type** : sélectionnez le bouton **Custom Code**
   - **Open Editor**, puis saisissez le fragment de code suivant :

     ```javascript
     if(event && event.component && event.component.hasOwnProperty('dc:title')) {
         // return value of 'dc:title' from the data layer Page object, which is propogated via 'cmp:show' event
         return event.component['dc:title'];
     }
     ```

  ![Élément de données Nom de page](../assets/use-cases/known-user-personalization/page-name-data-element.png)


   - **Name** : ID d’utilisateur WKND.
   - **Extension** : sélectionnez **Core**
   - **Data Element Type** : sélectionnez le bouton **Custom Code**
   - **Open Editor**, puis saisissez le fragment de code suivant :

     ```javascript
     // Data element for WKND User ID
     if(event && event.user && event.user.userId) {
         console.log('UserID:', event.user.userId);
         return event.user.userId;
     } else {
         console.log('UserID:');
         return "";
     }        
     ```

  ![Élément de données ID d’utilisateur WKND](../assets/use-cases/known-user-personalization/wknd-user-id-data-element.png)


   - **Name** : statut de l’utilisateur WKND.
   - **Extension** : sélectionnez **Core**
   - **Data Element Type** : sélectionnez le bouton **Custom Code**
   - **Open Editor**, puis saisissez le fragment de code suivant :

     ```javascript
     // Data element for user login status
     if(event && event.user && event.user.status) {
         console.log('User status:', event.user.status);
         return event.user.status;
     } else {
         console.log('User status:anonymous');
         return 'anonymous';
     }        
     ```

  ![Élément de données État de connexion WKND](../assets/use-cases/known-user-personalization/wknd-login-status-data-element.png)

- Créez ensuite un élément de données de type **Mappage d’identités**. La carte des identités est une structure XDM standard qui stocke plusieurs identifiants d’utilisateur et les associe, permettant la combinaison d’identités entre les systèmes. Cet élément de données est utilisé pour stocker l’ID utilisateur WKND (s’il est connecté) dans le cadre de l’objet de mappage d’identité.

   - **Name** : ID d’utilisateur IdentityMap-WKND
   - **Extension** : sélectionnez **SDK web d’Adobe Experience Platform**
   - **Type d’élément de données** : sélectionnez **Mappage d’identités**

  Dans le panneau de droite,
   - **Espace de noms** : sélectionnez **wkndUserId**
   - **ID** : sélectionnez l’élément de données **ID d’utilisateur WKND**.
   - **État d’authentification** : sélectionnez **Authentifié**
   - **Principal** : sélectionnez **true**


  Cliquez sur **Enregistrer** pour créer l’élément de données.

  ![Élément de données du mappage d’identités](../assets/use-cases/known-user-personalization/identity-map-data-element.png)

- Créez ensuite un élément de données de type **Variable**. Cet élément de données est renseigné avec les détails de la page avant d’être envoyé à Experience Platform.

   - **Name** : XDM-Variable Pageview
   - **Extension** : sélectionnez **SDK web d’Adobe Experience Platform**
   - **Data Element Type** : sélectionnez **Variable**

  Dans le panneau de droite,
   - **Sandbox** : sélectionnez votre sandbox
   - **Schéma** : sélectionnez le schéma **WKND-RDE-Known-User-Personalization**

  Cliquez sur **Enregistrer** pour créer l’élément de données.

  ![Élément de données Pageview variable XDM](../assets/use-cases/known-user-personalization/xdm-variable-pageview-data-element.png)

   - Les éléments de données finaux doivent se présenter comme suit :

     ![Éléments de données finaux](../assets/use-cases/known-user-personalization/final-data-elements.png)

#### Ajouter des règles

Les règles définissent quand et comment les données sont collectées et envoyées à Adobe Experience Platform. Ils agissent comme la couche logique qui détermine ce qui se produit lorsque des événements spécifiques se produisent sur votre site web. Pour la personnalisation d’utilisateurs connus, créez des règles pour capturer les données de pages vues et le statut de connexion de l’utilisateur + ID d’utilisateur (s’il est connecté) lorsque les utilisateurs visitent le site WKND.

Créez une règle pour renseigner l’élément de données **XDM-Variable Pageview** à l’aide des autres éléments de données avant de l’envoyer à Experience Platform. La règle est déclenchée lorsqu’un utilisateur ou une utilisatrice parcourt le site Web WKND.

- Cliquez sur **Règles** dans le volet de navigation de gauche, puis sur le bouton **Créer une règle**.
  ![Création d’une règle](../assets/use-cases/known-user-personalization/create-new-rule.png)

- Dans la boîte de dialogue **Créer une règle**, saisissez les informations suivantes :
   - **Nom** : toutes les pages - au chargement - avec des données utilisateur

   - Dans la section **Événements**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration d’événement**.
      - **Extension** : sélectionnez **Core**
      - **Type d’événement** : sélectionnez **Code personnalisé**
      - Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez l’extrait de code suivant :

     ```javascript
     var pageShownEventHandler = function(evt) {
         // defensive coding to avoid a null pointer exception
         if(evt.hasOwnProperty("eventInfo") && evt.eventInfo.hasOwnProperty("path")) {
             //trigger Launch Rule and pass event
             console.debug("cmp:show event: " + evt.eventInfo.path);
     
             // Get user data from session storage
             var userData = getUserDataFromSession();
     
             var event = {
                 //include the path of the component that triggered the event
                 path: evt.eventInfo.path,
                 //get the state of the component that triggered the event
                 component: window.adobeDataLayer.getState(evt.eventInfo.path),
                 //include user data in the event
                 user: userData
             };
     
             //Trigger the Launch Rule, passing in the new 'event' object
             trigger(event);
         }
     }
     
     /**
      * Get user data from session storage
     */
     function getUserDataFromSession() {
         var userData = {
             userId: null,
             status: 'anonymous'
         };
     
         try {
             var cachedUserState = sessionStorage.getItem('wknd_user_state');
     
             if (cachedUserState) {
                 var userState = JSON.parse(cachedUserState);
                 var userInfo = userState.data;
     
                 // Validate user data structure before transformation
                 if (userInfo && typeof userInfo === 'object' && userInfo.hasOwnProperty('authorizableId')) {
                     // Transform AEM user data to minimal AEP format
                     userData = {
                         userId: userInfo.authorizableId !== 'anonymous' ? userInfo.authorizableId : null,
                         status: userInfo.authorizableId === 'anonymous' ? 'anonymous' : 'authenticated',
                     };
     
                     //console.log('User details from session storage:', userData.username || 'Anonymous');
                 } else {
                     console.warn('Invalid user data structure in session storage');
                     console.log('Using anonymous user data');
                 }
             } else {
                 console.log('No user data in session storage, using anonymous');
             }
         } catch (e) {
             console.warn('Failed to read user data from session storage:', e);
             console.log('Using anonymous user data');
         }
     
         return userData;
     }
     
     //set the namespace to avoid a potential race condition
     window.adobeDataLayer = window.adobeDataLayer || [];
     
     //push the event listener for cmp:show into the data layer
     window.adobeDataLayer.push(function (dl) {
         //add event listener for 'cmp:show' and callback to the 'pageShownEventHandler' function
         dl.addEventListener("cmp:show", pageShownEventHandler);
     });
     ```

     Notez que la fonction `getUserDataFromSession` est utilisée pour obtenir le statut de connexion de l’utilisateur et l’ID utilisateur WKND (s’il est connecté) à partir du stockage de la session. Le code AEM est chargé de renseigner le stockage de session avec le statut de connexion de l’utilisateur et l’ID utilisateur WKND. À l’étape spécifique à AEM, vous avez amélioré la fonctionnalité de connexion au site WKND afin de stocker l’identifiant utilisateur dans l’espace de stockage de session du navigateur.

   - Pour la section **Conditions**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de condition**.
      - **Type de logique** : sélectionnez **Standard**
      - **Extension** : sélectionnez **Core**
      - **Type de condition** : sélectionnez **Code personnalisé**
      - Cliquez sur le bouton **Ouvrir l’éditeur** et saisissez l’extrait de code suivant :

     ```javascript
     if(event && event.component && event.component.hasOwnProperty('@type') && event.component.hasOwnProperty('xdm:template')) {
         console.log('The cmp:show event is from PAGE HANDLE IT');
         return true;
     } else {
         console.log('The event is NOT from PAGE - IGNORE IT');
         return false;
     }
     ```

   - Pour la section **Actions**, cliquez sur **Ajouter** pour ouvrir l’assistant **Configuration de l’action**.
      - **Extension** : sélectionnez **SDK web d’Adobe Experience Platform**
      - **Type d’action** : sélectionnez **Mettre à jour la variable**

      - Mappez les champs XDM aux éléments de données :

        | Champ XDM | Élément de données |
        |----------|------------|
        | web.webPageDetails.name | Nom de page |
        | web.webPageDetails.server | Nom d’hôte |
        | web.webPageDetails.siteSection | Section du site |
        | web.webPageDetails.value | 1 |
        | identityMap | Identifiant utilisateur IdentityMap-WKND |
        | _$YOUR_NAMESPACE$.wkndLoginStatus | Statut de l’utilisateur WKND |

     ![Action Mettre à jour la variable](../assets/use-cases/known-user-personalization/update-variable-action.png)

      - Cliquez sur **Conserver les modifications** pour enregistrer la configuration de l’action.

   - Cliquez à nouveau sur Ajouter pour ajouter une autre action et ouvrir l’assistant Configuration de l’action .

      - **Extension** : sélectionnez **SDK web d’Adobe Experience Platform**
      - **Type d’action** : sélectionnez **Envoyer l’événement**
      - Dans la section **Données** du panneau de droite, mappez l’élément de données **XDM-Variable Pageview** au type **Web Webpagedetails Page Views**.

     ![Action Envoyer l’événement](../assets/use-cases/known-user-personalization/send-event-action.png)

   - En outre, dans la section **Personalization** du panneau de droite, cochez l’option **Rendre les décisions de personnalisation visuelle**. Cliquez ensuite sur **Conserver les modifications** pour enregistrer l’action.

     ![Section Personnalisation](../assets/use-cases/known-user-personalization/personalization-section.png)

- Votre règle ressembler à ceci :

  ![Règle finale](../assets/use-cases/known-user-personalization/final-rule.png)

La règle est maintenant configurée pour envoyer les données de pages vues et le statut de connexion de l’utilisateur + l’ID d’utilisateur (s’il est connecté) à Experience Platform.

Les étapes de création de règle ci-dessus comportent un nombre considérable de détails. Soyez donc prudent lors de la création de la règle. Cela peut sembler complexe, mais souvenez-vous que ces étapes de configuration la rendent prête à l’emploi sans devoir mettre à jour le code AEM ni redéployer l’application.

#### Ajout et publication de la bibliothèque de balises

Une bibliothèque est une collection de toutes vos configurations Tags (éléments de données, règles, extensions) qui est créée et déployée sur votre site web. Elle regroupe toutes les informations afin que la collecte de données fonctionne correctement. Pour une personnalisation utilisateur connue, la bibliothèque est publiée pour que les règles de collecte de données soient actives sur votre site web.

- Cliquez sur **Flux de publication** dans le volet de navigation de gauche, puis cliquez sur le bouton **Ajouter une bibliothèque**.
  ![Ajouter une bibliothèque](../assets/use-cases/known-user-personalization/add-library.png)

- Dans la boîte de dialogue **Ajouter une bibliothèque**, saisissez les informations suivantes :
   - **Nom** : 1.0
   - **Environnement** : sélectionnez **Développement**.
   - Cliquez sur **Ajouter toutes les ressources modifiées** pour sélectionner toutes les ressources.

  Cliquez sur **Enregistrer et créer pour le développement** pour créer la bibliothèque.

  ![Ajouter une bibliothèque](../assets/use-cases/known-user-personalization/add-library-dialog.png)

- Pour publier la bibliothèque en production, cliquez sur **Approuver et publier en production**. Une fois la publication terminée, la propriété est prête à être utilisée dans AEM.
  ![Approuver et publier](../assets/use-cases/known-user-personalization/approve-publish.png)

La bibliothèque est maintenant publiée et prête à collecter les données de vos pages AEM.

### Configurer une politique de fusion pour le profil

Une politique de fusion définit la manière dont les données client et cliente provenant de plusieurs sources sont unifiées dans un seul et même profil. Il détermine les données prioritaires en cas de conflit, ce qui vous permet d’obtenir une vue complète et cohérente de chaque client pour une personnalisation utilisateur connue.

- Dans **Adobe Experience Platform**, cliquez sur **Profils** dans le volet de navigation de gauche, puis cliquez sur l’onglet **Politiques de fusion**.

  ![Politiques de fusion](../assets/use-cases/known-user-personalization/profile-merge-policies.png)

Pour les besoins de ce cas d’utilisation, une politique de fusion est créée. Cependant, vous pouvez utiliser une politique de fusion existante si vous en avez une. Veillez à activer les deux options **Politique de fusion par défaut** et **Politique de fusion Active-On-Edge** (ce qui permet aux données de profil d’être disponibles sur le réseau Edge pour les décisions de personnalisation en temps réel).

Ces paramètres garantissent que vos données comportementales et transactionnelles sont correctement unifiées et disponibles pour l’évaluation des audiences en temps réel.

![ Politique de fusion ](../assets/use-cases/known-user-personalization/merge-policy.png)

### Configurer une destination Adobe Target (V2)

La Destination Adobe Target (V2) permet d’activer les audiences créées dans Experience Platform directement dans Adobe Target. Cette connexion permet d’utiliser vos audiences pour des activités de personnalisation dans Adobe Target.

- Dans **Adobe Experience Platform**, cliquez sur **Destinations** dans le volet de navigation de gauche, puis cliquez sur l’onglet **Catalogue**. Recherchez **Personalization** et sélectionnez la destination **(v2) Adobe Target**.

  ![Destination Adobe Target](../assets/use-cases/known-user-personalization/adobe-target-destination.png)

- À l’étape **Activer les destinations**, donnez un nom à la destination et cliquez sur le bouton **Se connecter à la destination**.
  ![Se connecter à la destination](../assets/use-cases/known-user-personalization/connect-to-destination.png)

- Dans la section **Détails de la destination**, saisissez les informations suivantes :
   - **Name** : WKND-RDE-Known-User-Personalization-Destination
   - **Description** : destination de la personnalisation utilisateur connue
   - **Flux de données** : sélectionnez le **Flux de données** que vous avez créé précédemment
   - **Espace de travail** : sélectionnez votre espace de travail Adobe Target

  ![Détails de la destination](../assets/use-cases/known-user-personalization/destination-details.png)

- Cliquez sur **Suivant** et terminez la configuration de la destination.

  ![Configuration de la destination](../assets/use-cases/known-user-personalization/destination-configuration.png)

Une fois configurée, cette destination vous permet d’activer les audiences créées dans Experience Platform pour Adobe Target en vue de les utiliser dans des activités de personnalisation.

## Configuration d’AEM

Dans les étapes suivantes, vous allez améliorer la fonctionnalité de connexion au site WKND afin de stocker l’identifiant utilisateur dans l’espace de stockage de session du navigateur, et d’intégrer et d’injecter la propriété Tags dans les pages AEM.

La propriété des balises est injectée dans les pages AEM pour collecter les données de pages vues et le statut de connexion de l’utilisateur + l’ID d’utilisateur (s’il est connecté) lorsque les utilisateurs visitent le site WKND. L&#39;intégration d&#39;Adobe Target permet d&#39;exporter des offres personnalisées vers Adobe Target.

### Améliorer la fonctionnalité de connexion au site WKND

Pour améliorer la fonctionnalité de connexion au site WKND, clonez le [projet de site WKND](https://github.com/adobe/aem-guides-wknd) à partir de GitHub, créez une branche de fonctionnalité et ouvrez-la dans votre IDE préféré.

```shell
$ mkdir -p ~/Code
$ git clone git@github.com:adobe/aem-guides-wknd.git
$ cd aem-guides-wknd
$ git checkout -b feature/known-user-personalization
```

- Accédez au module `ui.frontend` et ouvrez le fichier `ui.frontend/src/main/webpack/components/form/sign-in-buttons/sign-in-buttons.js`. Vérifiez le code. Après avoir effectué un appel AJAX à `currentuser.json`, il affiche le bouton Se connecter ou Se déconnecter en fonction du statut de connexion de l’utilisateur.

- Mettez à jour le code pour stocker l’identifiant utilisateur dans le stockage de session du navigateur et optimisez également le code pour éviter d’effectuer plusieurs appels AJAX à `currentuser.json`.

  ```javascript
  import jQuery from "jquery";
  
  jQuery(function($) {
      "use strict";
  
      (function() {
          const currentUserUrl = $('.wknd-sign-in-buttons').data('current-user-url'),
              signIn = $('[href="#sign-in"]'),
              signOut = $('[href="#sign-out"]'),
              greetingLabel = $('#wkndGreetingLabel'),
              greetingText = greetingLabel.text(),
              body = $('body');
  
          // Cache configuration
          const CACHE_KEY = 'wknd_user_state';
          const CACHE_DURATION = 5 * 60 * 1000; // 5 minutes in milliseconds
  
          /**
           * Get cached user state from session storage
           */
          function getCachedUserState() {
              try {
                  const cached = sessionStorage.getItem(CACHE_KEY);
                  if (cached) {
                      const userState = JSON.parse(cached);
                      const now = Date.now();
  
                      // Check if cache is still valid
                      if (userState.timestamp && (now - userState.timestamp) < CACHE_DURATION) {
                          return userState.data;
                      } else {
                          // Cache expired, remove it
                          sessionStorage.removeItem(CACHE_KEY);
                      }
                  }
              } catch (e) {
                  console.warn('Failed to read cached user state:', e);
                  sessionStorage.removeItem(CACHE_KEY);
              }
              return null;
          }
  
          /**
           * Cache user state in session storage
           */
          function cacheUserState(userData) {
              try {
                  const userState = {
                      data: userData,
                      timestamp: Date.now()
                  };
                  sessionStorage.setItem(CACHE_KEY, JSON.stringify(userState));
              } catch (e) {
                  console.warn('Failed to cache user state:', e);
              }
          }
  
          /**
           * Clear cached user state
           */
          function clearCachedUserState() {
              try {
                  sessionStorage.removeItem(CACHE_KEY);
              } catch (e) {
                  console.warn('Failed to clear cached user state:', e);
              }
          }
  
          /**
           * Update UI based on user state
           */
          function updateUI(userData) {
              const isAnonymous = 'anonymous' === userData.authorizableId;
  
              if(isAnonymous) {
                  signIn.show();
                  signOut.hide();
                  greetingLabel.hide();
                  body.addClass('anonymous');
              } else {
                  signIn.hide();
                  signOut.show();
                  greetingLabel.text(greetingText + ", " + userData.name);
                  greetingLabel.show();
                  body.removeClass('anonymous');
              }
          }
  
          /**
           * Fetch user data from AEM endpoint
           */
          function fetchUserData() {
              return $.getJSON(currentUserUrl + "?nocache=" + new Date().getTime())
                  .fail(function(xhr, status, error) {
                      console.error('Failed to fetch user data:', error);
                      updateUI({ authorizableId: 'anonymous' });
                  });
          }
  
          /**
           * Initialize user state (check cache first, then fetch if needed)
           */
          function initializeUserState() {
              const cachedUserState = getCachedUserState();
  
              if (cachedUserState) {
                  updateUI(cachedUserState);
              } else {
                  fetchUserData().done(function(currentUser) {
                      updateUI(currentUser);
                      cacheUserState(currentUser);
                  });
              }
          }
  
          // Initialize user state
          initializeUserState();
  
          // Clear cache on sign-in/sign-out clicks
          $(document).on('click', '[href="#sign-in"], [href="#sign-out"]', function() {
              clearCachedUserState();
          });
  
          // Clear cache when modal is shown
          $('body').on('wknd-modal-show', function() {
              clearCachedUserState();
          });
  
          // Clear cache when on dedicated sign-in page
          if (window.location.pathname.includes('/sign-in') || window.location.pathname.includes('/errors/sign-in')) {
              clearCachedUserState();
          }
  
          // Clear cache when sign-in form is submitted
          $(document).on('submit', 'form[id*="sign-in"], form[action*="login"]', function() {
              clearCachedUserState();
          });
  
          // Clear cache on successful login redirect
          const urlParams = new URLSearchParams(window.location.search);
          if (urlParams.has('login') || urlParams.has('success') || window.location.hash === '#login-success') {
              clearCachedUserState();
          }
  
          // Debug function for testing
          window.debugUserState = function() {
              console.log('Cache:', sessionStorage.getItem('wknd_user_state'));
              clearCachedUserState();
              initializeUserState();
          };
  
      })();
  });
  ```

  Notez que la règle de propriété Balises repose sur l’ID utilisateur stocké dans l’espace de stockage de session du navigateur. La clé `wknd_user_state` est un contrat commun entre le code AEM et la règle de propriété Balises permettant de stocker et de récupérer l’identifiant utilisateur.

- Vérifiez les modifications localement en créant le projet et en l’exécutant localement.

  ```shell
  $ mvn clean install -PautoInstallSinglePackage
  ```

  Connectez-vous à l’aide des informations d’identification `asmith/asmith` (ou de tout autre utilisateur que vous avez créé). Elles sont [incluses](https://github.com/adobe/aem-guides-wknd/blob/main/ui.content.sample/src/main/content/jcr_root/home/users/wknd/l28HasMYWAMHAaGkv-Lj/.content.xml) dans le projet `aem-guides-wknd`.

  ![Connexion](../assets/use-cases/known-user-personalization/userid-in-session-storage.png)

  Dans mon cas, j’ai créé un nouvel utilisateur avec l’ID `teddy` pour le test.

- Après avoir confirmé que l’ID utilisateur est stocké dans le stockage de session du navigateur (à l’aide des outils de développement du navigateur), validez et envoyez les modifications au référentiel distant Adobe Cloud Manager.

  ```shell
  $ git add .
  $ git commit -m "Enhance the WKND site Login functionality to store the user ID in browser's session storage"
  $ git push adobe-origin feature/known-user-personalization
  ```

- Déployez les modifications dans l’environnement AEM as a Cloud Service à l’aide des pipelines Cloud Manager ou de la commande de RDE AEM.

### Intégration et injection de la propriété Tags dans les pages AEM

Cette étape intègre la propriété Balises qui a été créée précédemment dans vos pages AEM, ce qui permet la collecte de données pour une personnalisation utilisateur connue. La propriété Balises capture automatiquement les données de pages vues et le statut de connexion de l’utilisateur + l’ID d’utilisateur (s’il est connecté) lorsque les utilisateurs visitent le site WKND.

Pour intégrer la propriété Tags dans les pages AEM, suivez la procédure décrite dans [Intégrer Tags dans Adobe Experience Platform](../setup/integrate-adobe-tags.md).

Veillez à utiliser la propriété Balises **WKND-RDE-Known-User-Personalization** créée précédemment, et non une autre propriété.

![Propriété Tags](../assets/use-cases/known-user-personalization/tags-property.png)

Une fois intégrée, la propriété Tags commence à collecter des données de personnalisation utilisateur connues de vos pages AEM et à les envoyer à Experience Platform pour création d’audiences.

### Vérifier la collecte de données sur les pages AEM

Pour vérifier la collecte de données à partir des pages AEM, vous pouvez utiliser les outils de développement du navigateur pour inspecter les requêtes réseau et voir les données envoyées à Experience Platform. Vous pouvez également utiliser [Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) pour vérifier la collecte de données.

- Dans le navigateur , accédez au site WKND déployé dans votre environnement AEM as a Cloud Service. Étant anonyme, vous devriez voir des demandes de collecte de données similaires.

  ![Collecte de données anonyme](../assets/use-cases/known-user-personalization/anonymous-data-collection.png)

- Connectez-vous à l’aide des informations d’identification `asmith/asmith`. Des requêtes de collecte de données similaires devraient s’afficher.

  ![Connexion à la collecte de données](../assets/use-cases/known-user-personalization/logged-in-data-collection.png)

Notez que les variables `identityMap` et `_YOUR_NAMESPACE.wkndLoginStatus` sont définies respectivement sur l’ID utilisateur et le statut de connexion .

### Intégrer Adobe Target

Cette étape intègre Adobe Target à AEM et permet d’exporter du contenu personnalisé (fragments d’expérience) vers Adobe Target. Cette connexion permet à Adobe Target d’utiliser le contenu créé dans AEM pour des activités de personnalisation avec les audiences d’utilisateurs connus créées dans Experience Platform.

Pour intégrer Adobe Target et exporter les offres d’audience **WKND-RDE-Known-User-Personalization** vers Adobe Target, suivez la procédure décrite dans la section [Intégration d’Adobe Target à Adobe Experience Platform](../setup/integrate-adobe-target.md).

Assurez-vous que la configuration de Target est appliquée aux fragments d’expérience afin qu’ils puissent être exportés vers Adobe Target en vue d’être utilisés dans des activités de personnalisation.

![Configuration des fragments d’expérience avec Target](../assets/use-cases/known-user-personalization/experience-fragments-with-target-configuration.png)

Une fois intégrés, vous pouvez exporter des fragments d’expérience d’AEM vers Adobe Target, où ils sont utilisés en tant qu’offres personnalisées pour les audiences d’utilisateurs et utilisatrices connus.

### Création d’offres personnalisées

Les fragments d’expérience sont des composants de contenu réutilisables qui peuvent être exportés vers Adobe Target en tant qu’offres personnalisées. Pour la personnalisation d’utilisateurs connus, nous allons _vendre_ l’équipement de ski en créant un nouveau fragment d’expérience.

- Dans AEM, cliquez sur **Fragments d’expérience** et accédez au dossier **Fragments de site WKND**. À l’emplacement souhaité, créez un fragment d’expérience.

  ![Créer un fragment d’expérience](../assets/use-cases/known-user-personalization/create-experience-fragment.png)

- Créez le fragment d’expérience en ajoutant un composant Teaser et personnalisez-le avec du contenu pertinent pour augmenter la vente de l’équipement de ski.

  ![Développer un fragment d’expérience](../assets/use-cases/known-user-personalization/author-experience-fragment.png)

- Exportez le fragment d’expérience vers Adobe Target.

  ![Exporter un fragment d’expérience](../assets/use-cases/known-user-personalization/export-experience-fragment.png)

Votre offre personnalisée est désormais disponible dans Adobe Target pour être utilisée dans les activités .

## Configuration de Adobe Experience Platform

Consultez à nouveau le Adobe Experience Platform pour vérifier que les données comportementales sont collectées et que les profils associés sont créés. Ensuite, ingérez les données transactionnelles, vérifiez l’assemblage des données comportementales et transactionnelles, créez et configurez une audience, puis activez l’audience dans Adobe Target.

### Vérifier les données comportementales et la création de profil

Vérifions que les données comportementales sont collectées et que les profils associés sont créés.

- Dans Adobe Experience Platform, cliquez sur **Jeux de données** puis ouvrez le jeu de données **WKND-RDE-Known-User-Personalization-Behavioural**. Assurez-vous que les statistiques des données ingérées sont valides.

  ![ Ingérer des statistiques de données ](../assets/use-cases/known-user-personalization/ingest-data-stats.png)

- Pour vérifier que les profils sont créés, cliquez sur **Profils** dans le volet de navigation de gauche. Accédez ensuite à l’onglet **Parcourir** et filtrez à l’aide des critères suivants :
   - **Politique de fusion** : $YOUR_MERGE_POLICY_NAME
   - **Espace de noms d’identité** : ECID (identifiant Experience Cloud, un identifiant unique attribué automatiquement par Adobe à chaque navigateur de visiteurs)
   - **Valeur d’identité** : recherchez à l’aide des outils de développement du navigateur ou d’Experience Platform Debugger. Il s’agit de la valeur du cookie AMCV_$NAMESPACE$ sans le préfixe `MCMID|`.

  ![ECID ](../assets/use-cases/known-user-personalization/ecid.png)

- Cliquez sur le bouton **Afficher**.
  ![Liste des profils](../assets/use-cases/known-user-personalization/profile-list.png)

- Cliquez sur le profil, vous devriez voir les détails du profil.
  ![Détails du profil](../assets/use-cases/known-user-personalization/profile-details.png)

  Dans mon cas, j’ai accédé à mon site WKND à partir de deux navigateurs différents. J’ai donc deux ECID associés aux `teddy` utilisateur. Les données des deux ECID sont regroupées pour créer le profil. Vous avez commencé à réaliser la puissance de la combinaison d’identités et la manière dont elle est utilisée pour créer un profil client complet. Les données transactionnelles sont bientôt regroupées avec les données comportementales pour créer un profil client complet.

- Cliquez sur l’onglet **Événements** pour afficher les événements liés au profil.
  ![Événements de profil](../assets/use-cases/known-user-personalization/profile-events.png)

### Ingestion des données transactionnelles

Ensuite, vous ingérez les données transactionnelles factices dans Experience Platform. Dans cet exemple, les données transactionnelles sont stockées dans l’autre système (par exemple, OMS, CRM ou MDM) et ingérées dans Experience Platform à l’aide de l’ingestion par lots. Les données transactionnelles contiennent l’identifiant utilisateur WKND, qui est utilisé pour regrouper les données comportementales et transactionnelles.

- Dans Adobe Experience Platform, cliquez sur **Jeux de données** puis ouvrez le jeu de données **WKND-RDE-Known-User-Personalization-Transactionnel**.

  ![Jeu de données transactionnel](../assets/use-cases/known-user-personalization/transactional-dataset.png)

- Dans le panneau de droite, recherchez la section **AJOUTER DES DONNÉES** et faites glisser le fichier ski[adventure-purchase-data.json](../assets/use-cases/known-user-personalization/ski-adventure-purchase-data.json) vers celle-ci. Ce fichier contient les données transactionnelles factices pour les achats WKND Adventure. Dans un scénario réel, ces données sont ingérées à partir de l’autre système (par exemple, OMS, CRM ou MDM) à l’aide de l’ingestion par lots ou par flux.

  ![Ajouter des données](../assets/use-cases/known-user-personalization/add-data.png)

- Patientez jusqu’à la fin du traitement des données.

  ![Traitement des données](../assets/use-cases/known-user-personalization/data-processing.png)

- Une fois le traitement des données terminé, actualisez la page du jeu de données.

  ![Jeu de données transactionnel avec des données](../assets/use-cases/known-user-personalization/transactional-dataset-with-data.png)

### Vérifier l’assemblage des données comportementales et transactionnelles

Ensuite, vous vérifiez l’assemblage des données comportementales et transactionnelles, la partie la plus importante du cas d’utilisation de la personnalisation des utilisateurs connus. N’oubliez pas que l’ID d’utilisateur du site WKND est l’identifiant commun à tous les systèmes et qu’il est utilisé pour regrouper les données. Dans cet exemple, l’ID utilisateur `teddy` est utilisé pour regrouper les données.

- Cliquez sur **Profils** dans le volet de navigation de gauche. Accédez ensuite à l’onglet **Parcourir** et filtrez à l’aide des critères suivants :
   - **Politique de fusion** : $YOUR_MERGE_POLICY_NAME
   - **Espace de noms d’identité** : ECID
   - **Valeur d’identité** : utilisez la même valeur ECID que celle utilisée pour filtrer les données comportementales et le profil associé.

  ![ Liste de profils groupés ](../assets/use-cases/known-user-personalization/stitched-profile-list.png)

- Cliquez sur le profil, vous devriez voir les détails du profil. Les données transactionnelles sont regroupées avec les données comportementales afin de créer le profil client complet.

  ![Détails du profil assemblé](../assets/use-cases/known-user-personalization/stitched-profile-details.png)

- Cliquez sur l’onglet **Attributs**. Vous devriez voir les détails des données transactionnelles et comportementales associées au profil.
  ![Attributs de profil groupés](../assets/use-cases/known-user-personalization/stitched-profile-attributes.png)

- Cliquez sur le lien **Afficher le graphique d’identités** pour afficher le graphique d’identités du profil.
  ![Graphique d’identités](../assets/use-cases/known-user-personalization/identity-graph.png)

Félicitations ! Vous avez assemblé les données comportementales et transactionnelles pour créer un profil client complet.

L’assemblage des identités est une puissante fonctionnalité qui crée un profil client complet en combinant des données issues de plusieurs systèmes. À des fins de démonstration, seuls deux systèmes sont utilisés pour assembler les données. Dans un scénario réel, vous pouvez avoir plusieurs systèmes tels que l’application mobile, le centre d’appels, le bot conversationnel, le point de vente, etc. qui collectent les données et les stockent dans leurs systèmes respectifs. À l’aide de l’identifiant commun, les données sont regroupées pour créer un profil client complet et utilisées pour les activités de personnalisation. Cette approche modernise l’expérience client en offrant des expériences personnalisées aux utilisateurs et utilisatrices, en remplaçant le contenu statique unique par des expériences personnalisées basées sur les profils individuels des clients et clientes.

#### Recherche de profil à l’aide de l’ID utilisateur WKND

Il est possible de rechercher le profil à l’aide de l’identifiant utilisateur WKND (et non de l’ECID) dans Experience Platform.

- Cliquez sur **Profils** dans le volet de navigation de gauche. Accédez ensuite à l’onglet **Parcourir** et filtrez à l’aide des critères suivants :
   - **Politique de fusion** : $YOUR_MERGE_POLICY_NAME
   - **Espace de noms d’identité** : ID utilisateur WKND.
   - **Valeur d’identité** : `teddy` ou `asmith` ou tout autre identifiant utilisateur que vous avez utilisé.

  ![ Liste de profils groupés ](../assets/use-cases/known-user-personalization/stitched-profile-list-using-wknd-user-id.png)

- Cliquez sur le profil. Vous devriez voir les mêmes détails de profil que ceux de l’étape précédente.
  ![Détails du profil assemblé](../assets/use-cases/known-user-personalization/stitched-profile-details-using-wknd-user-id.png)

### Créer et configurer une audience

Une audience définit un groupe spécifique d’utilisateurs et d’utilisatrices en fonction de leurs données comportementales et transactionnelles. Dans cet exemple, une audience est créée pour qualifier les utilisateurs et utilisatrices qui ont acheté une Adventure **Ski** et qui sont connectés au site WKND.

Pour créer une audience, effectuez les étapes suivantes :

- Dans Adobe Experience Platform, cliquez sur **Audiences** dans le volet de navigation de gauche, puis cliquez sur le bouton **Créer une audience**. Sélectionnez ensuite l’option **Créer-règle** et cliquez sur le bouton **Créer**.
  ![Créer une audience](../assets/use-cases/known-user-personalization/create-audience.png)

- À l’étape **Créer**, saisissez les informations suivantes :
   - **Name** : UpSell-Ski-Equipment-To-Authenticated
   - **Description** : utilisateurs connectés et ayant acheté une aventure de ski
   - **Méthode d’évaluation** : sélectionnez **Edge** (évalue l’appartenance à l’audience en temps réel au fur et à mesure que les utilisateurs naviguent, permettant ainsi une personnalisation instantanée)

  ![Créer une audience](../assets/use-cases/known-user-personalization/create-audience-step.png)

- Cliquez ensuite sur l’onglet **Attributs** et accédez au groupe de champs **Techmarketingdemos** (ou votre $NAMESPACE$). Faites glisser et déposez le champ **Adventure Purchased** dans la section **Commencer la création**. Saisissez les informations suivantes :

  **Aventure achetée** : sélectionnez **Contient** et saisissez la valeur **ski**.

  ![Créer une audience](../assets/use-cases/known-user-personalization/create-audience-step-attributes.png)

- Ensuite, passez à l’onglet **Événements** et accédez au groupe de champs **techmarketingdemos** (ou votre $NAMESPACE$). Faites glisser et déposez le champ **État de connexion WKND** dans la section **Événements**. Saisissez les informations suivantes :

  **Statut de connexion de WKND** : sélectionnez **Est égal à** et saisissez la valeur **authentifié**.

  Sélectionnez également l’option **Aujourd’hui**.

  ![Créer une audience](../assets/use-cases/known-user-personalization/create-audience-step-events.png)

- Passez en revue l’audience et cliquez sur le bouton **Activer sur la destination**.

  ![Vérifier l’audience](../assets/use-cases/known-user-personalization/review-audience.png)

- Dans la boîte de dialogue **Activer vers la destination**, sélectionnez la destination Adobe Target que vous avez créée précédemment et suivez les étapes pour activer l’audience. Cliquez sur **Suivant** et terminez la configuration de la destination.

  ![Activer sur la destination](../assets/use-cases/known-user-personalization/activate-to-destination.png)

Félicitations ! Vous avez créé l’audience et l’avez activée vers la destination Adobe Target.

## Configuration d’Adobe Target

Dans Adobe Target, la disponibilité de l’audience créée dans Experience Platform et des offres personnalisées exportées depuis AEM est vérifiée. Ensuite, une activité est créée, qui combine le ciblage des audiences avec le contenu personnalisé, afin de fournir une expérience de personnalisation à l’utilisateur connu.

- Connectez-vous à Adobe Experience Cloud et accédez à **Adobe Target** à partir de la section Sélecteur d’applications ou Accès rapide .

  ![Adobe Target](../assets/use-cases/known-user-personalization/adobe-target.png)

### Vérification des audiences et des offres

Vérifions que les audiences et les offres sont correctement disponibles dans Adobe Target.

- Dans Adobe Target, cliquez sur **Audiences** et vérifiez que l’audience **UpSell-Ski-Equipment-To-Authenticated** a été créée.

  ![Audiences](../assets/use-cases/known-user-personalization/audiences.png)

- En cliquant sur l’audience, vous pouvez afficher les détails de l’audience et vérifier qu’elle est correctement configurée.

  ![Détails de l’audience](../assets/use-cases/known-user-personalization/audience-details.png)

- Cliquez sur **Offres** et vérifiez que l’offre exportée AEM existe. Dans mon cas, l’offre (ou le fragment d’expérience) s’appelle **Doit contenir des éléments pour ski**.

  ![Offres connues de Personalization pour les utilisateurs](../assets/use-cases/known-user-personalization/known-user-personalization-offers.png)

  Cela valide les actions d’intégration dans Adobe Experience Platform, AEM et Adobe Target.

### Création et configuration d’une activité

Une activité dans Adobe Target est une campagne de personnalisation qui définit quand et comment le contenu personnalisé est distribué à des audiences spécifiques. Pour la personnalisation d’utilisateurs connus, une activité est créée pour afficher l’offre de montée en gamme des équipements de ski aux utilisateurs et utilisatrices connectés et qui ont acheté une aventure de ski.

- Dans Adobe Target, cliquez sur **Activités**, sur le bouton **Créer une activité**, puis sélectionnez le type d’activité **Ciblage d’expérience**.
  ![Création d’une activité](../assets/use-cases/known-user-personalization/create-activity.png)

- Dans la boîte de dialogue **Créer une activité de ciblage d’expérience**, sélectionnez l’option de compositeur **Web** de type et **Visual** (un éditeur WYSIWYG qui vous permet de créer et de tester des expériences personnalisées directement sur votre site Web), puis saisissez l’URL de la page d’accueil du site WKND. Cliquez sur le bouton **Créer** pour créer l’activité.

  ![Créer une activité de ciblage d’expérience](../assets/use-cases/known-user-personalization/create-experience-targeting-activity.png)

- Dans l’éditeur, sélectionnez l’audience **UpSell-Ski-Equipment-To-Authenticated** et ajoutez l’offre **Must Have Items for Ski** au lieu du contenu du héros principal. Consultez la capture d’écran ci-dessous à titre de référence.

  ![Activité avec audience et offre](../assets/use-cases/known-user-personalization/activity-with-audience-n-offer.png)

- Cliquez sur **Suivant** et configurez la section **Objectifs et paramètres** avec les objectifs et mesures appropriés, puis activez-la pour appliquer les modifications.

  ![Activer avec Objectifs et paramètres](../assets/use-cases/known-user-personalization/activate-with-goals-and-settings.png)

Félicitations ! Vous êtes prêt à fournir l’expérience de personnalisation d’utilisateur connu aux utilisateurs qui sont connectés et ont acheté des aventures skiables.

## Vérifier l’implémentation de la personnalisation d’un utilisateur connu

Il est temps de vérifier l’implémentation de la personnalisation d’utilisateurs connus sur votre site WKND.

- Rendez-vous sur la page d’accueil du site WKND. Si vous n’êtes pas connecté, vous devriez voir le contenu principal par défaut.

  ![Contenu principal par défaut](../assets/use-cases/known-user-personalization/default-hero-content.png)

- Connectez-vous à l’aide d’informations d’identification `teddy/teddy` (ou `asmith/asmith`). Le contenu personnalisé en héros s’affichera probablement.

  ![Contenu héros personnalisé](../assets/use-cases/known-user-personalization/personalized-hero-content.png)

- Ouvrez les outils de développement de votre navigateur et consultez l’onglet **Réseau**. Filtrez par `interact` pour trouver la requête SDK web. La requête/réponse doit afficher les détails de l’événement Web SDK et de la décision Adobe Target.

  La sortie de la requête doit se présenter comme suit :
  ![Requête réseau du SDK web](../assets/use-cases/known-user-personalization/web-sdk-network-request.png)

  La sortie de réponse doit se présenter comme suit :

  ![Réponse réseau de Web SDK](../assets/use-cases/known-user-personalization/web-sdk-network-response.png)

Félicitations ! Vous êtes un expert dans la diffusion de l’expérience de personnalisation d’utilisateur connue en créant un profil client complet à l’aide des données regroupées sur l’ensemble des systèmes.

## Ressources supplémentaires

- [SDK web d’Adobe Experience Platform](https://experienceleague.adobe.com/fr/docs/experience-platform/web-sdk/home)
- [Vue d’ensemble des flux de données](https://experienceleague.adobe.com/fr/docs/experience-platform/datastreams/overview)
- [Visual Experience Composer (VEC)](https://experienceleague.adobe.com/fr/docs/target/using/experiences/vec/visual-experience-composer)
- [Segmentation Edge](https://experienceleague.adobe.com/fr/docs/experience-platform/segmentation/methods/edge-segmentation)
- [Types d’audiences](https://experienceleague.adobe.com/fr/docs/experience-platform/segmentation/types/overview)
- [Connexion d’Adobe Target](https://experienceleague.adobe.com/fr/docs/experience-platform/destinations/catalog/personalization/adobe-target-connection)
