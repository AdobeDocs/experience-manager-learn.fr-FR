---
title: Gouvernance et modèles de recrutement et archétypes
description: Découvrez comment mettre en oeuvre votre plateforme Adobe Experience Manager (AEM) et tirer le meilleur parti de vos efforts.
solution: Experience Manager
exl-id: 808ab7a6-5ec5-4bbd-9a6e-cfc0b447430d
source-git-commit: 471f0fe940abb8241428beb14896d83e140136b3
workflow-type: tm+mt
source-wordcount: '1133'
ht-degree: 0%

---

# Adobe Experience Manager (AEM) - Modèles de gouvernance et de dotation en personnel et archétypes

En tant que chef de file de l’expérience client, Adobe comprend à quel point il peut être difficile pour vous de vous assurer que vous disposez des personnes et du cadre de gouvernance adéquats pour accroître l’efficacité opérationnelle. Grâce aux modèles de gouvernance et de dotation éprouvés de l’Adobe, vous disposez des outils et des connaissances nécessaires pour créer une base solide de gestion de contenu et de ressources. Dans cet article, nous discuterons des moyens de rendre opérationnel votre plateforme Adobe Experience Manager (AEM) et de tirer le meilleur parti de vos efforts.

## Créer un cadre opérationnel supérieur

Pour pouvoir exécuter et fonctionner AEM tenez compte des éléments suivants :

* Exécuter les jalons stratégiques : il va y avoir de nombreux jalons stratégiques (personnalisation, intégration multicanal, etc.) qui ne peut pas être exécuté si vous n’avez pas mis en place le modèle de dotation approprié.
* Créer les fondations d&#39;une transformation numérique : l&#39;AEM est souvent utilisée comme la première étape du processus de modernisation d&#39;une organisation. La définition d’une fondation vous permet d’exploiter AEM à sa pleine capacité.
* Engagement de l’utilisateur : avoir une équipe en place pour exécuter des tâches tactiques (mise à jour des workflows, autorisations, CSS, etc.) Plus vous avez d&#39;écart entre ce que veulent les utilisateurs et ce qu&#39;ils reçoivent, plus ils deviennent frustrés. Il est important de garder les utilisateurs investis dans le système, dans la solution, et d’avoir le bon modèle opérationnel en place.

Alors, quel est le bon modèle ? Quelle est la bonne matrice de rôles à créer ?

Il n’existe pas de réponse spécifique unique, car, tout comme les organisations varient considérablement, une configuration AEM peut également varier considérablement, ce qui entraîne la nécessité de différents rôles de soutien. Chaque secteur industriel vertical, chaque structure d’équipe nécessite une mise en oeuvre différente. Mais vous pouvez créer une ligne de base en établissant des archétypes.

## Archetypes

Les archetypes sont des idées de rôle spécifiques, de haut niveau, qui correspondent à des attributs spécifiques. Cela peut à son tour être utilisé pour créer un postulat fondamental qui aide à informer le modèle dont vous avez vraiment besoin. Il est important de noter que les archetypes ne sont pas limités à une personne par archétype. Par exemple, un bibliothécaire de la gestion des actifs numériques peut avoir une expérience technique.

### Flux de mise en oeuvre

Il existe deux modes de mise en oeuvre pour : [!DNL AEM Sites] et [!DNL AEM Assets]:

1. Exécution et fonctionnement de base au quotidien (mise à jour des métadonnées)

1. Travail de stratégie et de transformation, comme les grands projets interorganisationnels

![flux de mise en oeuvre](assets/streams-of-operationalization.png)

### Rôles de ressources AEM de haut niveau

**Écart général :** Cette ligne de base soutient des modèles centralisés et décentralisés. Si vous avez un modèle décentralisé, AEM peut être utilisé de manière abstraite. Notez que le rôle Propriétaire du produit doit être utilisé de manière créative, mais vous devez également disposer d’un Propriétaire du produit qui possède les différents styles pour un type de ressource et d’un autre qui supervise l’ensemble de l’organisation.

1. Rôles d’exécution et d’exploitation de base

   * Ressource technique : une personne qui a AEM expérience comprend les autorisations et peut mettre à jour le schéma de métadonnées
   * Gestionnaire de versions
   * Propriétaire du produit : il s’agit d’un rôle aligné sur la solution. Certains propriétaires de produits peuvent être impliqués dans les analyses.
   * Bibliothèque DAM : il s’agit d’une personne qui peut aider à piloter les processus de structure intégrative. Ce rôle créatif peut se chevaucher à d’autres rôles. (Remarque : c&#39;est un rôle qui a explosé en popularité ces cinq dernières années.)
   * Créativité

1. Stratégie et transformation

   * Équipe de développement : cette équipe est nécessaire lorsque vous franchissez une étape stratégique majeure.
   * Architecte commercial : développe les exigences pour faciliter les étapes techniques et les initiatives stratégiques ; peut être compensé par un propriétaire de produit supplémentaire.
   * Architecte technique : personne maîtrisant le niveau entreprise et ayant une présence constante au sein de l’entreprise. Ce rôle sert de centre de vérité de la gestion des actifs numériques.

**Exemples de scénarios**

1. **Exécutez et exploitez :**

Vous trouverez ci-dessous des exemples de rôles pour un scénario léger (entreprise de vêtements de sport) et lourd (entreprise cosmétique) :

1. Lumière - Rôles d’entreprise de vêtements de sport :

   * 2 développeurs à temps partiel - temps partiel, offshore
   * 1 propriétaire de produit - temps complet, sur terre
   * 1 bibliothèque DAM - temps plein, sur terre
   * 1 architecte technique - temps partiel, terrestre
   * 1 gestionnaire de versions - temps partiel, sur terre

1. Lourd - Société Cosmetics (multi-marque)

   * 3 développeurs à plein temps - temps complet, offshore
   * 4 propriétaires de produit - 3 marques spécifiques, 1 Principal
   * 1 bibliothèque DAM - temps plein, sur terre
   * 4 administrateurs principaux PME par marque
   * 1 architecte technique

### Haut niveau [!DNL AEM Sites] rôles

1. Exécution et fonctionnement de base

   **Écart général :** Les développeurs CSS créent de nouveaux habillages pour les composants. Joseph Van Buskirk, conseiller en affaires de l&#39;Adobe Sr, recommande &quot;Obtenir des composants non cordés et des systèmes de style. C&#39;est le rôle qui entraîne les économies de coûts. 80 % des expériences créées doivent être effectuées à l’aide de composants principaux ou créés précédemment.&quot; L’objectif est de réutiliser les composants principaux ou personnalisés avec de nouveaux styles à l’aide d’un développeur CSS (ou d’une équipe de développement front-end) .

   Exemples de rôles :

   * Développement CSS : crée des artefacts d’expérience en redéfinissant l’objectif des composants avec de nouveaux styles.
   * Développement principal : crée de nouveaux composants ou peut étendre un composant principal. Si cette opération est effectuée correctement, ce rôle ne doit pas comporter plus d’une personne, sauf s’il est nécessaire d’effectuer des tâches d’animation volumineuses.
   * Gestion des versions : supervise le déploiement du code et fait office d’ingénieur du service client actuel.
   * Propriétaire du produit : collabore avec l&#39;unité de traitement des données sur l&#39;alliance des visions techniques et stratégiques ; crée des tâches de maintenance et des améliorations et fait office de propriétaire de la solution.
   * Auteurs d’administration : met à jour l’habillage CSS et fournit des conseils aux auteurs qui mettent à jour et appliquent du contenu. Ce rôle fonctionne sur les configurations de workflow et crée une documentation d’orientation à l’intention des auteurs de contenu à appliquer. REMARQUE : Dans la version 6.5, Adobe recommande d’utiliser des modèles modifiables.
   * Auteurs de contenu : applique le contenu, la propriété à plusieurs niveaux et fournit des problèmes de communication et des préoccupations au fur et à mesure qu’ils surviennent avec votre CSM.

1. Stratégie et transformation

   Exemples de rôles :

   * L’équipe de développement : fournit des connaissances AEM et exécute de nouveaux jalons de transformation avec l’architecte technique.
   * Architecte technique : fournit des connaissances sur l’intégration, travaille avec le propriétaire du produit pour mapper les jalons techniques et fournit des connaissances techniques approfondies sur les AEM.
   * Architecte d’entreprise : crée des tâches pour les articles d’utilisateurs et aide le propriétaire du produit à gérer les jalons techniques et commerciaux.

### Exemples de scénarios

Voici des exemples de rôles pour un scénario client léger et lourd :

1. Clair

   * 2 développeurs CSS - onshore
   * 1 propriétaire de produit - temps plein, sur terre
   * 1 développeur principal - offshore
   * 1 architecte technique - onshore
   * 1 gestionnaire de versions - temps partiel, sur terre

1. Lourde (axée sur Campaign)

   * 4 développeurs CSS - temps plein, sur terre
   * 2 développeurs principaux - temps complet, sur terre
   * 1 architecte technique - onshore
   * 1 propriétaire de produit
   * 2 architectes commerciaux - offshore

### Points clés

**Présentation des archetypes** — Démarrez lentement, comprenez et analysez les archétypes. Soyez créatif et flexible, en gardant à l’esprit qu’il n’y a pas un modèle correct à suivre.

**Présentation de votre feuille de route** - Certaines organisations comptent de nombreux jalons qu’elles souhaitent exécuter. Soyez prêt à allouer plus de ressources techniques que vous ne pouvez l’estimer.

**Exploitation des ressources internes** - Les écarts peuvent survenir de manière inattendue. Vous pouvez les remplir plus rapidement en vous adressant aux membres de l’équipe interne plutôt qu’en effectuant des recherches en dehors de votre entreprise.

Pour une discussion plus approfondie sur la gouvernance et les modèles de dotation et les archétypes, écoutez cette table ronde d&#39;une heure : [Archétypes des rôles et création d&#39;un cadre opérationnel pour [!DNL AEM Assets] et [!DNL Sites]](https://adobecustomersuccess.adobeconnect.com/p8ml5nmy0758mp4/)

Pour en savoir plus sur la stratégie et le leadership de la pensée, voir [Succès des clients](https://experienceleague.corp.adobe.com/docs/customer-success/customer-success/overview.html) hub.
