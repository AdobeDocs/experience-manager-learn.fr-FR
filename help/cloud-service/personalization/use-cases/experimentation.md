---
title: Expérimentation (tests A/B)
description: Découvrez comment tester différentes variations de contenu dans AEM as a Cloud Service (AEMCS) à l’aide d’Adobe Target pour les tests A/B.
version: Experience Manager as a Cloud Service
feature: Personalization
topic: Personalization,Content Management, Integrations
role: Developer, Leader
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18720
thumbnail: KT-18720.jpeg
exl-id: c8a4f0bf-1f80-4494-abe6-9fbc138e4039
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '1493'
ht-degree: 100%

---

# Expérimentation (tests A/B)

Découvrez comment tester différentes variations de contenu sur un site web AEM as a Cloud Service (AEMCS) à l’aide d’Adobe Target.

Les tests A/B vous aident à comparer différentes versions de contenu afin de déterminer la plus performante pour atteindre vos objectifs métier. Voici quelques exemples de scénarios courants :

- Tests des variations dans les titres, les images ou les boutons d’appel à l’action sur une page de destination
- Comparaison des différentes mises en page ou conceptions pour une page de détails de produit
- Évaluation des offres promotionnelles ou des stratégies de remise

## Cas d’utilisation de démonstration

Dans ce tutoriel, vous configurez les tests A/B pour le fragment d’expérience (XF) **Camping in Western Australia** sur le site web WKND. Vous créez trois variations XF et gérez le test A/B via Adobe Target.

Les variations s’affichent sur la page d’accueil WKND, ce qui vous permet de mesurer les performances et de déterminer la version qui génère le plus d’engagement et de conversions.

![Test A/B](../assets/use-cases/experiment/view-ab-test-variations.png)

### Démonstration en direct

Rendez-vous sur le [site web de mise en œuvre WKND](https://wknd.enablementadobe.com/us/en.html) pour voir le test A/B en action. Dans la vidéo ci-dessous, vous pouvez voir les trois variantes de **Camping in Western Australia** affichées sur la page d&#39;accueil via différents navigateurs.

>[!VIDEO](https://video.tv.adobe.com/v/3473005/?learn=on&enablevpops)

## Conditions préalables

Avant de poursuivre ce cas d’utilisation d’expérimentation, assurez-vous d’avoir effectué les étapes suivantes :

- [Intégrer Adobe Target](../setup/integrate-adobe-target.md) : permet à votre équipe de créer et de gérer du contenu personnalisé de manière centralisée dans AEM et de l’activer en tant qu’offres dans Adobe Target.
- [Intégrer Tags dans Adobe Experience Platform](../setup/integrate-adobe-tags.md) : permet à votre équipe de gérer et de déployer du JavaScript pour la personnalisation et la collecte de données sans avoir à redéployer de code AEM.

## Étapes avancées

Le processus de configuration des tests A/B implique six étapes principales pour créer et configurer l’expérience :

1. **Créer des variations de contenu dans AEM**
2. **Exporter les variations en tant qu’offres vers Adobe Target**
3. **Créer une activité de test A/B dans Adobe Target**
4. **Créer et configurer un flux de données dans Adobe Experience Platform**
5. **Mettre à jour la propriété Tags avec l’extension de SDK web**
6. **Vérifier l’implémentation du test A/B sur vos pages AEM**

## Créer des variations de contenu dans AEM

Dans cet exemple, vous utilisez le fragment d’expérience (XF) **Camping in Western Australia** du projet AEM WKND pour créer trois variations, qui seront utilisées sur la page d’accueil du site web WKND pour les tests A/B.

1. Dans AEM, cliquez sur la carte **Fragments d’expérience**, accédez à **Camping in Western Australia**, puis cliquez sur **Modifier**.
   ![Fragments d’expérience](../assets/use-cases/experiment/camping-in-western-australia-xf.png)

1. Dans l’éditeur, sous la section **Variations**, cliquez sur **Créer**, puis sélectionnez **Variation**.\
   ![Création d’une variation](../assets/use-cases/experiment/create-variation.png)

1. Dans la boîte de dialogue **Créer une variation** :
   - **Modèle** : modèle de variation web de fragment d’expérience
   - **Titre** : par exemple, « Déconnexion totale »

   Cliquez sur **Terminé**.

   ![Boîte de dialogue Créer une variation](../assets/use-cases/experiment/create-variation-dialog.png)

1. Créez la variation en copiant le composant **Teaser** à partir de la variation principale, puis personnalisez le contenu (par exemple, mettez à jour le titre et l’image).\
   ![Création de Variation-1](../assets/use-cases/experiment/author-variation-1.png)

   >[!TIP]
   >Vous pouvez utiliser [Générer des variations](https://experience.adobe.com/aem/generate-variations/) pour créer rapidement des variations à partir du fragment d’expérience principal.

1. Répétez les étapes pour créer une autre variation (par exemple, « Retour à la vie sauvage »).\
   ![Création de Variation-2](../assets/use-cases/experiment/author-variation-2.png)

   Vous disposez désormais de trois variations de fragment d’expérience pour les tests A/B.

1. Avant d’afficher des variations à l’aide d’Adobe Target, vous devez supprimer le teaser statique existant de la page d’accueil. Cela évite d’obtenir du contenu en double, car les variations de fragment d’expérience sont injectées de manière dynamique via Target.

   - Accédez à la page d’accueil **English** `/content/wknd/language-masters/en`.
   - Dans l’éditeur, supprimez le composant de teaser **Camping in Western Australia**.\
     ![Suppression du composant de teaser](../assets/use-cases/experiment/delete-teaser-component.png)

1. Déployez les modifications sur la page d’accueil **US > English** (`/content/wknd/us/en`) pour propager les mises à jour.\
   ![Déploiement des modifications](../assets/use-cases/experiment/rollout-changes.png)

1. Publiez la page d’accueil **US > English** pour mettre les mises à jour en ligne.\
   ![Publication de la page d’accueil](../assets/use-cases/experiment/publish-homepage.png)

## Exporter les variations en tant qu’offres vers Adobe Target

Exportez les variations de fragment d’expérience afin qu’elles puissent être utilisées en tant qu’offres dans Adobe Target pour le test A/B.

1. Dans AEM, accédez à **Camping in Western Australia**, sélectionnez les trois variations, puis cliquez sur **Exporter vers Adobe Target**.\
   ![Exporter vers Adobe Target](../assets/use-cases/experiment/export-to-adobe-target.png)

2. Dans Adobe Target, accédez à **Offres** et confirmez que les variations ont été importées.\
   ![Offres dans Adobe Target](../assets/use-cases/experiment/offers-in-adobe-target.png)

## Créer une activité de test A/B dans Adobe Target

Créez maintenant une activité de test A/B pour exécuter l’expérience sur la page d’accueil.

1. Installez l’extension Chrome [Adobe Experience Cloud Visual Editing Helper](https://chromewebstore.google.com/detail/adobe-experience-cloud-vi/kgmjjkfjacffaebgpkpcllakjifppnca).

1. Dans Adobe Target, accédez à **Activités** et cliquez sur **Créer une activité**.\
   ![Création d’une activité](../assets/use-cases/experiment/create-activity.png)

1. Dans la boîte de dialogue **Créer une activité de test A/B**, saisissez ce qui suit :
   - **Type** : web
   - **Compositeur** : visuel
   - **URL de l’activité** : par exemple, `https://wknd.enablementadobe.com/us/en.html`

   Cliquez sur **Créer**.

   ![Création d’une activité de test A/B](../assets/use-cases/experiment/create-ab-test-activity.png)

1. Donnez un nom explicite à l’activité (par exemple, « Test A/B de la page d’accueil WKND »).\
   ![Renommage de l’activité](../assets/use-cases/experiment/rename-activity.png)

1. Dans **Expérience A**, ajoutez le composant **Fragment d’expérience** au-dessus de la section **Recent Articles**.\
   ![Ajout d’un composant de fragment d’expérience](../assets/use-cases/experiment/add-experience-fragment-component.png)

1. Dans la boîte de dialogue du composant, cliquez sur **Sélectionner une offre**.\
   ![Sélection d’une offre](../assets/use-cases/experiment/select-offer.png)

1. Choisissez la variation **Camping in Western Australia** et cliquez sur **Ajouter**.\
   ![Sélection du fragment d’expérience Camping in Western Australia](../assets/use-cases/experiment/select-camping-in-western-australia-xf.png)

1. Répétez l’opération pour **Expérience B** et **C**, en sélectionnant **Déconnexion totale** et **Retour à la vie sauvage**, respectivement.\
   ![Ajout des expériences B et C](../assets/use-cases/experiment/add-experience-b-and-c.png)

1. Dans la section **Ciblage**, vérifiez que le trafic est partagé uniformément entre toutes les expériences.\
   ![Affectation du trafic](../assets/use-cases/experiment/traffic-allocation.png)

1. Dans **Objectifs et paramètres**, définissez votre mesure de succès (par exemple, clics CTA sur le fragment d’expérience).\
   ![Objectifs et paramètres](../assets/use-cases/experiment/goals-and-settings.png)

1. Cliquez sur **Activer** dans le coin supérieur droit pour lancer le test.\
   ![Activation de l’activité](../assets/use-cases/experiment/activate-activity.png)

## Créer et configurer un flux de données dans Adobe Experience Platform

Pour connecter le SDK Adobe Web à Adobe Target, créez un flux de données dans Adobe Experience Platform. Le flux de données sert de couche de routage entre le SDK web et Adobe Target.

1. Dans Adobe Experience Platform, accédez à **Flux de données** et cliquez sur **Créer un flux de données**.\
   ![Création d’un flux de données](../assets/use-cases/experiment/aep-create-datastream.png)

1. Dans la boîte de dialogue **Créer un flux de données**, saisissez un **Nom** pour votre flux de données et cliquez sur **Enregistrer**.\
   ![Saisie du nom du flux de données](../assets/use-cases/experiment/aep-datastream-name.png)

1. Une fois le flux de données créé, cliquez sur **Ajouter un service**.\
   ![Ajout d’un service au flux de données](../assets/use-cases/experiment/aep-add-service.png)

1. À l’étape **Ajouter un service**, sélectionnez **Adobe Target** dans la liste déroulante et saisissez l’**ID d’environnement cible**. Vous pouvez retrouver l’ID d’environnement cible dans Adobe Target sous **Administration** > **Environnements**. Cliquez sur **Enregistrer** pour ajouter le service.\
   ![Configuration du service Adobe Target](../assets/use-cases/experiment/aep-target-service.png)

1. Consultez les détails du flux de données pour vérifier que le service Adobe Target est répertorié et correctement configuré.\
   ![Vérification du flux de données](../assets/use-cases/experiment/aep-datastream-review.png)

## Mettre à jour la propriété Tags avec l’extension de SDK web

Pour envoyer des événements de personnalisation et de collecte de données à partir de pages AEM, ajoutez l’extension SDK web à votre propriété Tags et configurez une règle qui se déclenche au chargement de la page.

1. Dans Adobe Experience Platform, accédez à **Tags** et ouvrez la propriété que vous avez créée à l’étape [Intégrer Adobe Tags](../setup/integrate-adobe-tags.md).
   ![Ouverture de la propriété Tags](../assets/use-cases/experiment/open-tags-property.png)

1. Dans le menu de gauche, cliquez sur **Extensions**, accédez à l’onglet **Catalogue**, puis recherchez **SDK web**. Cliquez sur **Installer** dans le panneau de droite.\
   ![Installation de l’extension SDK web](../assets/use-cases/experiment/web-sdk-extension-install.png)

1. Dans la boîte de dialogue **Installer l’extension**, sélectionnez le **Flux de données** créé précédemment et cliquez sur **Enregistrer**.\
   ![Sélection du flux de données](../assets/use-cases/experiment/web-sdk-extension-select-datastream.png)

1. Après l’installation, vérifiez que les extensions **SDK web d’Adobe Experience Platform** et **Core** apparaissent sous l’onglet **Installé**.\
   ![Extensions installées](../assets/use-cases/experiment/web-sdk-extension-installed.png)

1. Configurez ensuite une règle pour envoyer l’événement de SDK web lorsque la bibliothèque est chargée. Accédez à **Règles** dans le menu de gauche, puis cliquez sur **Créer une règle**.

   ![Création d’une règle](../assets/use-cases/experiment/web-sdk-rule-create.png)

   >[!TIP]
   >
   >Une règle vous permet de définir quand et comment les balises se déclenchent en fonction des interactions de l’utilisateur ou de l’utilisatrice ou des événements de navigateur.

1. Dans l’écran **Créer une règle**, saisissez un nom de règle (par exemple, `All Pages - Library Loaded - Send Event`) et cliquez sur **+ Ajouter** sous la section **Événements**.
   ![Nom de la règle](../assets/use-cases/experiment/web-sdk-rule-name.png)

1. Dans la boîte de dialogue **Configuration de l’événement** :
   - **Extension** : sélectionnez **Core**
   - **Type d’événement** : sélectionnez **Bibliothèque chargée (Haut de page)**
   - **Nom** : saisissez `Core - Library Loaded (Page Top)`

   Cliquez sur **Conserver les modifications** pour enregistrer l’événement.

   ![Création d’une règle d’événement](../assets/use-cases/experiment/web-sdk-rule-event.png)

1. Sous la section **Actions**, cliquez sur **+ Ajouter** pour définir l’action qui se produit lorsque l’événement se déclenche.

1. Dans la boîte de dialogue **Configuration de l’action** :
   - **Extension** : sélectionnez **SDK web d’Adobe Experience Platform**
   - **Type d’action** : sélectionnez **Envoyer l’événement**
   - **Nom** : sélectionnez **SDK web d’AEP - Envoyer l’événement**

   ![Configuration de l’action Envoyer l’événement](../assets/use-cases/experiment/web-sdk-rule-action.png)

1. Dans la section **Personnalisation** du panneau de droite, cochez l’option **Afficher les choix de personnalisation**. Cliquez ensuite sur **Conserver les modifications** pour enregistrer l’action.\
   ![Création d’une règle d’action](../assets/use-cases/experiment/web-sdk-rule-action.png)

   >[!TIP]
   >
   >   Cette action envoie un événement de SDK web d’AEP au chargement de la page, ce qui permet à Adobe Target de diffuser du contenu personnalisé.

1. Vérifiez la règle terminée, puis cliquez sur **Enregistrer**.
   ![Vérification de la règle](../assets/use-cases/experiment/web-sdk-rule-review.png)

1. Pour appliquer les modifications, accédez à **Flux de publication** et ajoutez la règle mise à jour à une **Bibliothèque**.\
   ![Publication de la bibliothèque de balises](../assets/use-cases/experiment/web-sdk-rule-publish.png)

1. Pour finir, passez la bibliothèque en **Production**.
   ![Publication des balises en production](../assets/use-cases/experiment/web-sdk-rule-publish-production.png)

## Vérifier l’implémentation du test A/B sur vos pages AEM

Une fois que l’activité est en ligne et que la bibliothèque de balises est publiée en production, vous pouvez vérifier le test A/B sur vos pages AEM.

1. Rendez-vous sur le site publié (par exemple, le [site web de mise en œuvre WKND](https://wknd.enablementadobe.com/us/en.html)) et observez la variation affichée. Essayez d’y accéder à partir d’un autre navigateur ou appareil mobile pour voir d’autres variations.
   ![Affichage des variations de test A/B](../assets/use-cases/experiment/view-ab-test-variations.png)

1. Ouvrez les outils de développement de votre navigateur et consultez l’onglet **Réseau**. Filtrez par `interact` pour trouver la requête SDK web. La requête doit contenir les détails de l’événement SDK web.

   ![Requête réseau du SDK web](../assets/use-cases/experiment/web-sdk-network-request.png)

La réponse doit inclure les choix de personnalisation effectués par Adobe Target et indiquer la variation présentée.\
![Réponse du SDK web](../assets/use-cases/experiment/web-sdk-response.png)

1. Vous pouvez également utiliser l’extension Chrome [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) pour inspecter les événements du SDK web.
   ![AEP Debugger](../assets/use-cases/experiment/aep-debugger-variation.png)

## Démonstration en direct

Pour voir le test A/B en action, rendez-vous sur le [site web de mise en œuvre WKND](https://wknd.enablementadobe.com/us/en.html) et observez comment différentes variations du fragment d’expérience s’affichent sur la page d’accueil.

## Ressources supplémentaires

- [Vue d’ensemble du test A/B](https://experienceleague.adobe.com/fr/docs/target/using/activities/abtest/test-ab)
- [SDK web d’Adobe Experience Platform](https://experienceleague.adobe.com/fr/docs/experience-platform/web-sdk/home)
- [Vue d’ensemble des flux de données](https://experienceleague.adobe.com/fr/docs/experience-platform/datastreams/overview)
- [Visual Experience Composer (VEC)](https://experienceleague.adobe.com/fr/docs/target/using/experiences/vec/visual-experience-composer)
