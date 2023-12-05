---
title: Créer une propriété de balise
description: Découvrez comment créer une propriété de balise avec la configuration minimale à intégrer avec AEM. Les utilisateurs et utilisatrices découvrent l’interface utilisateur des balises et découvrent les extensions, les règles et les workflows de publication.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5980
thumbnail: 38553.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: d5de62ef-a2aa-4283-b500-e1f7cb5dec3b
duration: 649
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 100%

---

# Créer une propriété de balise {#create-tag-property}

Découvrez comment créer une propriété de balise avec la configuration minimale à intégrer avec Adobe Experience Manager. Les utilisateurs et utilisatrices découvrent l’interface utilisateur des balises et découvrent les extensions, les règles et les workflows de publication.

>[!VIDEO](https://video.tv.adobe.com/v/38553?quality=12&learn=on)

## Créer une propriété de balise

Pour créer une propriété de balise, procédez comme suit :

1. Dans le navigateur, accédez à la [page d’accueil d’Adobe Experience Cloud](https://experience.adobe.com/) et connectez-vous à l’aide de votre Adobe ID.

1. Cliquez sur l’application **Collecte de données** de la section _Accès rapide_ de la page d’accueil d’Adobe Experience Cloud.

1. Cliquez sur le menu **Balises** dans le volet de navigation de gauche, puis cliquez sur **Nouvelle propriété** dans le coin supérieur droit.

1. Nommez votre propriété de balise à l’aide du champ **Nom** obligatoire. Pour le champ Domaines, saisissez votre nom de domaine ou, si vous utilisez l’environnement AEM as a Cloud Service, saisissez `adobeaemcloud.com` et cliquez sur **Enregistrer**.

   ![Propriétés de balise.](assets/tag-properties.png)

## Créer une règle

Ouvrez la propriété de balise nouvellement créée en cliquant sur son nom dans la fenêtre **Propriétés de balise**. Sous l’en-tête _Mon activité récente_ vous devriez également constater que l’extension principale y a été ajoutée. L’extension de balise principale est l’extension par défaut et fournit les types d’événements fondamentaux tels que le chargement de la page, le navigateur, les formulaires et d’autres types d’événements ; veuillez consulter [Vue d’ensemble de l’extension principale](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/client/core/overview.html?lang=fr) pour plus d’informations.

Les règles vous permettent de spécifier ce qui doit se produire lorsque le visiteur ou la visiteuse interagit avec votre site AEM. Pour simplifier les choses, consignons deux messages dans la console du navigateur afin de démontrer comment l’intégration de balises de collecte de données peut injecter du code JavaScript dans votre site AEM sans mettre à jour le code du projet AEM.

Pour créer une règle, procédez comme suit :

1. Cliquez sur **Règles** dans la section _Création_ sur le volet de navigation de gauche, puis cliquez sur **Créer une règle**.

1. Nommez votre règle à l’aide du champ **Nom** obligatoire.

1. Cliquez sur **Ajouter** dans la section _Événements_, puis, dans le formulaire _Configuration d’événement_, dans le menu déroulant **Type d’événement**, sélectionnez l’option _Bibliothèque chargée (haut de page)_ et cliquez sur **Conserver les modifications**.

1. Cliquez sur **Ajouter** dans la section _Actions_, puis, dans le formulaire _Configuration d’action_, dans le menu déroulant **Type d’action**, sélectionnez _Code personnalisé_ et cliquez sur **Ouvrir l’éditeur**.

1. Dans la boîte de dialogue modale _Modifier le code_, saisissez l’extrait de code JavaScript suivant, puis cliquez sur **Enregistrer**, puis enfin cliquez sur **Conserver les modifications**.

   ```javascript
   console.log('Tags Property loaded, all set for...');
   console.log('capabilities such as capturing data, conversion tracking and delivering unique and personalized experiences');
   ```

1. Cliquez sur **Enregistrer** pour terminer le processus de création de règle.

   ![Nouvelle règle](assets/new-rule.png)

## Ajouter une bibliothèque et la publier

Les _Règles_ de propriété de balise sont activées à l’aide d’une bibliothèque ; considérez la bibliothèque comme un package contenant du code JavaScript. Activez la règle nouvellement créée en suivant les étapes indiquées.

1. Cliquez sur **Flux de publication** dans la section _Publication_ sur le volet de navigation de gauche, puis cliquez sur **Ajouter une bibliothèque**.

1. Nommez votre bibliothèque à l’aide du champ **Nom** et sélectionnez l’option _Développement (développement)_ pour le menu déroulant **Environnement**.

1. Pour sélectionner toutes les ressources modifiées depuis la création de la propriété de balise, cliquez sur **+ Ajouter toutes les ressources modifiées**. Cette action ajoute la règle nouvellement créée et la ressource d’extension principale à la bibliothèque. Pour finir, cliquez sur **Enregistrer et créer pour l’environnement de développement**.

1. Une fois la bibliothèque créée pour la piste de navigation **Développement**, sélectionnez **Envoyer pour approbation** à l’aide des _points de suspension_.

1. Ensuite, dans la piste de navigation **Envoyée**, sélectionnez **Approuver pour publication** à l’aide des _points de suspension_, ainsi que **Créer et publier dans l’environnement de production** dans la piste de navigation **Approuvée**.

![Bibliothèque publiée.](assets/published-library.png)


L’étape ci-dessus complète la création de la propriété de balise simple qui comporte une règle pour consigner un message dans la console du navigateur lorsque la page est chargée. La règle et l’extension principale sont également publiées en créant une bibliothèque.

## Étapes suivantes

[Connecter AEM avec la propriété de balise à l’aide d’IMS](connect-aem-tag-property-using-ims.md)


## Ressources supplémentaires {#additional-resources}

* [Créer une propriété de balise](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=fr)
