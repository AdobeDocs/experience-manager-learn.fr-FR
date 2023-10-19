---
title: Intégrer AEM Headless et Target
description: Découvrez comment intégrer AEM Headless et Adobe Target pour personnaliser les expériences découplées à l’aide du SDK web Experience Platform.
version: Cloud Service
feature: Content Fragments, Integrations
topic: Personalization, Headless
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-05-09T00:00:00Z
jira: KT-12433
thumbnail: KT-12433.jpeg
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Headless as a Cloud Service" before-title="false"
exl-id: be886c64-9b8e-498d-983c-75f32c34be4b
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: ht
source-wordcount: '1679'
ht-degree: 100%

---

# Intégrer AEM Headless et Target

Découvrez comment intégrer AEM Headless à Adobe Target en exportant des fragments de contenu AEM vers Adobe Target et en les utilisant pour personnaliser les expériences découplées à l’aide du fichier alloy.js du SDK web Adobe Experience Platform. L’[application React WKND](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/how-to/example-apps/react-app.html) est utilisée pour découvrir comment ajouter à l’expérience une activité Target personnalisée à l’aide d’offres de fragments de contenu, afin de promouvoir une aventure WKND.

>[!VIDEO](https://video.tv.adobe.com/v/3416585/?quality=12&learn=on)

Le tutoriel décrit les étapes à suivre pour configurer AEM et Adobe Target :

1. [Créer une configuration Adobe IMS pour Adobe Target](#adobe-ims-configuration) dans l’instance de création AEM.
2. [Créer un compte Adobe Target Cloud Service](#adobe-target-cloud-service) dans l’instance de création AEM.
3. [Appliquer le compte Adobe Target Cloud Service aux dossiers AEM Assets](#configure-asset-folders) dans l’instance de création AEM.
4. [Autoriser Adobe Target Cloud Service](#permission) dans Adobe Admin Console.
5. [Exporter des fragments de contenu](#export-content-fragments) de l’instance de création AEM vers Target.
6. [Créer une activité à l’aide d’offres de fragments de contenu](#activity) dans Adobe Target.
7. [Créer un train de données Experience Platform](#datastream-id) dans Experience Platform.
8. [Intégrer la personnalisation dans une application AEM Headless basée sur React](#code) à l’aide du SDK web Adobe.

## Configuration Adobe IMS{#adobe-ims-configuration}

Une configuration Adobe IMS qui facilite l’authentification entre AEM et Adobe Target.

Lisez [la documentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integration-adobe-target-ims.html) pour obtenir des instructions étape par étape pour créer une configuration Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/3416495/?quality=12&learn=on)

## Adobe Target Cloud Service{#adobe-target-cloud-service}

Un Adobe Target Cloud Service est créé dans AEM pour faciliter l’export de fragments de contenu vers Adobe Target.

Lisez [la documentation](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/integrations/integrating-adobe-target.html?lang=fr) pour obtenir des instructions détaillées sur la création d’un Adobe Target Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3416499/?quality=12&learn=on)


## Configurer les dossiers de ressources{#configure-asset-folders}

Adobe Target Cloud Service, dans une configuration contextuelle, doit être appliqué à la hiérarchie de dossiers AEM Assets qui contient les fragments de contenu à exporter vers Adobe Target.

+++Développer pour obtenir des instructions détaillées

1. Connectez-vous au __service de l’instance de création AEM__ en tant qu’administrateur ou administratrice de gestion des ressources numériques.
1. Accédez à __Assets > Fichiers__, recherchez le dossier de ressources qui contient le `/conf` concerné.
1. Sélectionnez le dossier de ressources et choisissez __Propriétés__ dans la barre d’actions supérieure.
1. Sélectionnez l’onglet __Services cloud__
1. Assurez-vous que la configuration du cloud est définie sur la configuration contextuelle (`/conf`) contenant la configuration Adobe Target Cloud Services.
1. Sélectionnez __Adobe Target__ dans la liste déroulante __Configurations de Cloud Service__.
1. Sélectionnez __Enregistrer et fermer__ en haut à droite

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416504/?quality=12&learn=on)

## Autoriser l’intégration AEM Target{#permission}

L’intégration Adobe Target, qui se manifeste sous la forme d’un projet developer.adobe.com, doit se voir accorder le rôle produit __Éditeur__ dans Adobe Admin Console, pour exporter des fragments de contenu vers Adobe Target.

+++Développer pour obtenir des instructions détaillées

1. Connectez-vous à Experience Cloud en tant qu’utilisateur ou utilisatrice capable d’administrer le produit Adobe Target dans Adobe Admin Console.
1. Ouvrez [Adobe Admin Console](https://adminconsole.adobe.com).
1. Sélectionnez __Produits__, puis ouvrez __Adobe Target__.
1. Sur l’onglet __Profils de produit__, sélectionnez __*DefaultWorkspace*__.
1. Sélectionnez l’onglet __Informations d’identification de l’API__.
1. Localisez votre application developer.adobe.com dans cette liste et définissez son __Rôle produit__ sur __Éditeur__.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416505/?quality=12&learn=on)

## Exporter les fragments de contenu vers Target{#export-content-fragments}

Les fragments de contenu qui existent sous la [hiérarchie de dossiers AEM Assets configurée](#apply-adobe-target-cloud-service-to-aem-assets-folders) peuvent être exportés vers Adobe Target en tant qu’offres de fragments de contenu. Ces offres de fragments de contenu, une forme spéciale d’offres JSON dans Target, peuvent être utilisées dans des activités Target pour proposer des expériences personnalisées dans des applications découplées.

+++Développer pour obtenir des instructions détaillées

1. Connectez-vous à l’__instance de création AEM__ en tant qu’utilisateur ou utilisatrice de gestion des ressources numériques.
1. Accédez à __Assets > Fichiers__ et recherchez les fragments de contenu à exporter au format JSON vers Target sous le dossier « Autorisé par Adobe Target ».
1. Sélectionner les fragments de contenu à exporter vers Adobe Target
1. Sélectionnez __Exporter vers des offres Adobe Target__ à partir de la barre d’actions supérieure.
   + Cette action exporte la représentation JSON entièrement hydratée du fragment de contenu vers Adobe Target en tant qu’« offre de fragment de contenu ».
   + La représentation JSON entièrement hydratée peut être examinée dans AEM.
      + Sélectionnez le fragment de contenu.
      + Développez le panneau latéral.
      + Sélectionnez __Aperçu__ dans le panneau de gauche.
      + La représentation JSON exportée vers Adobe Target s’affiche dans la vue principale.
1. Connectez-vous à [Adobe Experience Cloud](https://experience.adobe.com) avec un utilisateur ou une utilisatrice ayant le rôle Éditeur ou Éditrice pour Adobe Target
1. Dans [Experience Cloud](https://experience.adobe.com), sélectionnez __Target__ dans le sélecteur de produits en haut à droite pour ouvrir Adobe Target.
1. Assurez-vous que l’espace de travail par défaut est sélectionné dans le __Sélecteur d’espace de travail__ en haut à droite.
1. Sélectionnez l’onglet __Offres__ dans la barre de navigation supérieure.
1. Sélectionnez la liste déroulante __Type__, puis sélectionnez __Fragments de contenu__.
1. Vérifiez que le fragment de contenu exporté depuis AEM apparaît dans la liste.
   + Pointez sur l’offre, puis cliquez sur le bouton __Affichage__.
   + Consultez la section __Informations sur l’offre__ et reportez-vous au __lien profond AEM__ qui ouvre le fragment de contenu directement dans le service de l’instance de création AEM.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416506/?quality=12&learn=on)

## Activité Target nécessitant des offres de fragments de contenu{#activity}

Dans Adobe Target, il est possible de créer une activité qui utilise le contenu JSON de l’offre de fragment de contenu, afin d’offrir des expériences personnalisées dans une application découplée avec du contenu créé et géré dans AEM.

Dans cet exemple, nous utilisons une activité A/B simple, mais toute activité Target peut être utilisée.

+++Développer pour obtenir des instructions détaillées

1. Sélectionnez l’onglet __Activités__ dans la barre de navigation supérieure.
1. Sélectionnez __+ Créer une activité__, puis sélectionnez le type d’activité à créer.
   + Cet exemple crée un __Test A/B__, mais les offres de fragments de contenu peuvent alimenter n’importe quel type d’activité.
1. Dans l’assistant __Créer une activité__ :
   + Sélectionnez __Web__.
   + Dans __Choisir un compositeur d’expérience__, sélectionnez __Formulaire__.
   + Dans __Choisir un espace de travail__, sélectionnez __Espace de travail par défaut__.
   + Dans __Choisir une propriété__, sélectionnez la propriété dans laquelle l’activité est disponible ou sélectionnez __Aucune restriction de propriété__ pour permettre son utilisation dans toutes les propriétés.
   + Sélectionnez __Suivant__ pour créer l’activité.
1. Renommez l’activité en sélectionnant __Renommer__ en haut à gauche.
   + Attribuez un nom significatif à l’activité.
1. Dans l’expérience initiale, définissez __Emplacement 1__ pour l’activité Target.
   + Dans cet exemple, ciblez un emplacement personnalisé nommé `wknd-adventure-promo`.
1. Sous __Contenu__ sélectionnez le contenu par défaut, puis sélectionnez __Modifier le fragment de contenu__.
1. Sélectionnez le fragment de contenu exporté à diffuser pour cette expérience, puis sélectionnez __Terminé__.
1. Examinez le fichier JSON de l’offre de fragment de contenu dans la zone de texte Contenu. Il s’agit du même fichier JSON disponible dans le service de l’instance de création AEM via l’action Aperçu du fragment de contenu.
1. Dans le rail de gauche, ajoutez une expérience et sélectionnez une autre offre de fragment de contenu à diffuser.
1. Sélectionnez __Suivant__, et configurez les règles de ciblage selon les besoins de l’activité.
   + Dans cet exemple, conservez le partage 50/50 manuel pour le test AB.
1. Sélectionnez __Suivant__, puis renseignez les paramètres de l’activité.
1. Sélectionnez __Enregistrer et fermer__ et donnez-lui un nom significatif.
1. Dans l’activité Adobe Target, sélectionnez __Activer__ dans la liste déroulante Désactiver/Activer/Archiver en haut à droite.

L’activité Adobe Target qui cible l’emplacement `wknd-adventure-promo` peut désormais être intégrée et exposée dans une application AEM découplée.

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416507/?quality=12&learn=on)

## Identifiant du train de données Experience Platform{#datastream-id}

Un ID de [train de données Adobe Experience Platform](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/initial-configuration/configure-datastream.html) est requis pour que les applications AEM découplées interagissent avec Adobe Target à l’aide du [SDK web d’Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++Développer pour obtenir des instructions détaillées

1. Accédez à [Adobe Experience Cloud](https://experience.adobe.com/).
1. Ouvrez __Experience Platform__.
1. Sélectionnez __Collecte de données > Trains de données__ et sélectionnez __Nouveau train de données__.
1. Dans l’assistant Nouveau train de données, saisissez :
   + Nom : `AEM Target integration`
   + Description : `Datastream used by the Adobe Web SDK to serve personalized Content Fragments Offers.`
   + Schéma d’événement : `Leave blank`
1. Sélectionnez __Enregistrer__.
1. Sélectionnez __Ajouter un service__.
1. Dans __Service__, sélectionnez __Adobe Target__.
   + Activé : __Oui__.
   + Jeton de propriété : __Laisser vide__.
   + ID de l’environnement Target : __Laisser vide__.
      + L’environnement Target peut être défini dans Adobe Target sous __Administration > Hôtes__.
   + Espace de noms de l’ID tiers de Target : __Laisser vide__.
1. Sélectionnez __Enregistrer__.
1. Sur le côté droit, copiez l’__ID du train de données__ pour l’utiliser dans l’appel de configuration du [SDK web d’Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/configuring-the-sdk.html).

+++

<br/>

>[!VIDEO](https://video.tv.adobe.com/v/3416500/?quality=12&learn=on)

## Personnaliser une application AEM Headless{#code}

Ce tutoriel explique comment personnaliser une application React simple à l’aide d’offres de fragments de contenu dans Adobe Target via le [SDK web d’Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=fr). Cette approche peut être utilisée pour personnaliser toute expérience web basée sur JavaScript.

Les expériences mobiles Android™ et iOS peuvent être personnalisées en suivant des modèles similaires à l’aide du [SDK mobile d’Adobe](https://developer.adobe.com/client-sdks/documentation/).

### Conditions préalables

+ Node.js 14
+ Git
+ [WKND Shared 2.1.4+](https://github.com/adobe/aem-guides-wknd-shared/releases/latest) installé sur les services de création et de publication d’AEM as a Cloud Service.

### Configuration

1. Téléchargez le code source de l’exemple d’application React depuis [Github.com](https://github.com/adobe/aem-guides-wknd-graphql).

   ```shell
   $ mkdir -p ~/Code
   $ git clone git@github.com:adobe/aem-guides-wknd-graphql.git
   ```

1. Ouvrez la base de code dans `~/Code/aem-guides-wknd-graphql/personalization-tutorial` dans votre IDE préféré.
1. Mettez à jour l’hôte du service AEM auquel vous souhaitez que l’application se connecte `~/Code/aem-guides-wknd-graphql/personalization-tutorial/src/.env.development`.

   ```
   ...
   REACT_APP_HOST_URI=https://publish-p1234-e5678.adobeaemcloud.com/
   ...
   ```

1. Exécutez l’application et assurez-vous qu’elle se connecte au service d’AEM configuré. Depuis la ligne de commande, exécutez :

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install
   $ npm run start
   ```

1. Installez le [SDK web d’Adobe](https://experienceleague.adobe.com/docs/experience-platform/edge/fundamentals/installing-the-sdk.html#option-3%3A-using-the-npm-package) sous la forme d’un package NPM.

   ```shell
   $ cd ~/Code/aem-guides-wknd-graphql/personalization-tutorial
   $ npm install @adobe/alloy
   ```

   Le SDK web peut être utilisé dans le code pour récupérer le JSON de l’offre de fragment de contenu par emplacement d’activité.

   Lors de la configuration du SDK web, deux ID sont requis :

   + `edgeConfigId` qui est l’[ID du train de données](#datastream-id).
   + `orgId` l’ID de l’organisation d’Adobe as a Cloud Service/Target disponible sous __Experience Cloud > Profil > Informations sur le compte > ID de l’organisation actuelle__.

   Lors de l’appel du SDK web, l’emplacement de l’activité Adobe Target (dans notre exemple, `wknd-adventure-promo`) doit être défini comme la valeur du tableau `decisionScopes`.

   ```javascript
   import { createInstance } from "@adobe/alloy";
   const alloy = createInstance({ name: "alloy" });
   ...
   alloy("config", { ... });
   alloy("sendEvent", { ... });
   ```

### Implémentation

1. Créez un composant React `AdobeTargetActivity.js` pour transmettre les activités Adobe Target.

   __src/components/AdobeTargetActivity.js__

   ```javascript
   import React, { useEffect } from 'react';
   import { createInstance } from '@adobe/alloy';
   
   const alloy = createInstance({ name: 'alloy' });
   
   alloy('configure', { 
     'edgeConfigId': 'e3db252d-44d0-4a0b-8901-aac22dbc88dc', // AEP Datastream ID
     'orgId':'7ABB3E6A5A7491460A495D61@AdobeOrg',
     'debugEnabled': true,
   });
   
   export default function AdobeTargetActivity({ activityLocation, OfferComponent }) { 
     const [offer, setOffer] = React.useState();
   
     useEffect(() => {
       async function sendAlloyEvent() {
         // Get the activity offer from Adobe Target
         const result = await alloy('sendEvent', {
           // decisionScopes is set to an array containing the Adobe Target activity location
           'decisionScopes': [activityLocation],
         });
   
         if (result.propositions?.length > 0) {
           // Find the first proposition for the active activity location
           var proposition = result.propositions?.filter((proposition) => { return proposition.scope === activityLocation; })[0];
   
           // Get the Content Fragment Offer JSON from the Adobe Target response
           const contentFragmentOffer = proposition?.items[0]?.data?.content || { status: 'error', message: 'Personalized content unavailable'};
   
           if (contentFragmentOffer?.data) {
             // Content Fragment Offers represent a single Content Fragment, hydrated by
             // the byPath GraphQL query, we must traverse the JSON object to retrieve the 
             // Content Fragment JSON representation
             const byPath = Object.keys(contentFragmentOffer.data)[0];
             const item = contentFragmentOffer.data[byPath]?.item;
   
             if (item) {
               // Set the offer to the React state so it can be rendered
               setOffer(item);
   
               // Record the Content Fragment Offer as displayed for Adobe Target Activity reporting
               // If this request is omitted, the Target Activity's Reports will be blank
               alloy("sendEvent", {
                   xdm: {
                       eventType: "decisioning.propositionDisplay",
                       _experience: {
                           decisioning: {
                               propositions: [proposition]
                           }
                       }
                   }
               });          
             }
           }
         }
       };
   
       sendAlloyEvent();
   
     }, [activityLocation, OfferComponent]);
   
     if (!offer) {
       // Adobe Target offer initializing; we render a blank component (which has a fixed height) to prevent a layout shift
       return (<OfferComponent></OfferComponent>);
     } else if (offer.status === 'error') {
       // If Personalized content could not be retrieved either show nothing, or optionally default content.
       console.error(offer.message);
       return (<></>);
     }
   
     console.log('Activity Location', activityLocation);
     console.log('Content Fragment Offer', offer);
   
     // Render the React component with the offer's JSON
     return (<OfferComponent content={offer} />);
   };
   ```

   Le composant d’actvité React d’Adobe Target est appelé comme suit :

   ```jsx
   <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   ```

1. Un composant React `AdventurePromo.js` est créé pour effectuer le rendu de l’aventure sur les serveurs JSON Adobe Target.

   Ce composant React prend le fichier JSON entièrement hydraté qui représente un fragment de contenu d’aventure et s’affiche de manière promotionnelle. Les composants React qui affichent le JSON fourni à partir des offres de fragments de contenu Adobe Target peuvent être aussi variés et complexes que nécessaire en fonction des fragments de contenu exportés vers Adobe Target.

   __src/components/AdventurePromo.js__

   ```javascript
   import React from 'react';
   
   import './AdventurePromo.scss';
   
   /**
   * @param {*} content is the fully hydrated JSON data for a WKND Adventure Content Fragment
   * @returns the Adventure Promo component
   */
   export default function AdventurePromo({ content }) {
       if (!content) {
           // If content is still loading, then display an empty promote to prevent layout shift when Target loads the data
           return (<div className="adventure-promo"></div>)
       }
   
       const title = content.title;
       const description = content?.description?.plaintext;
       const image = content.primaryImage?._publishUrl;
   
       return (
           <div className="adventure-promo">
               <div className="adventure-promo-text-wrapper">
                   <h3 className="adventure-promo-eyebrow">Promoted adventure</h3>
                   <h2 className="adventure-promo-title">{title}</h2>
                   <p className="adventure-promo-description">{description}</p>
               </div>
               <div className="adventure-promo-image-wrapper">
                   <img className="adventure-promo-image" src={image} alt={title} />
               </div>
           </div>
       )
   }
   ```

   __src/components/AdventurePromo.scss__

   ```css
   .adventure-promo {
       display: flex;
       margin: 3rem 0;
       height: 400px;
   }
   
   .adventure-promo-text-wrapper {
       background-color: #ffea00;
       color: black;
       flex-grow: 1;
       padding: 3rem 2rem;
       width: 55%;
   }
   
   .adventure-promo-eyebrow {
       font-family: Source Sans Pro,Helvetica Neue,Helvetica,Arial,sans-serif;
       font-weight: 700;
       font-size: 1rem;
       margin: 0;
       text-transform: uppercase;
   }
   
   .adventure-promo-description {
       line-height: 1.75rem;
   }
   
   .adventure-promo-image-wrapper {
       height: 400px;
       width: 45%;
   }
   
   .adventure-promo-image {
       height: 100%;
       object-fit: cover;
       object-position: center center;
       width: 100%;
   }
   ```

   Ce composant React est appelé comme suit :

   ```jsx
   <AdventurePromo adventure={adventureJSON}/>
   ```

1. Le composant d’activité Adobe Target est ajouté au composant React `Home.js` de l’application en haut de la liste des aventures.

   __src/components/Home.js__

   ```javascript
   import AdventurePromo from './AdventurePromo';
   import AdobeTargetActivity from './AdobeTargetActivity';
   ... 
   export default function Home() {
       ...
       return(
           <div className="Home">
   
             <AdobeTargetActivity activityLocation={"wknd-adventure-promo"} OfferComponent={AdventurePromo}/>
   
             <h2>Current Adventures</h2>
             ...
       )
   }
   ```

1. Si l’application React n’est pas en cours d’exécution, redémarrez avec `npm run start`.

   Ouvrez l’application React dans deux navigateurs différents afin de permettre au test A/B de proposer les différentes expériences à chaque navigateur. Si les deux navigateurs affichent la même offre d’aventure, essayez de fermer/rouvrir l’un des navigateurs jusqu’à ce que l’autre expérience s’affiche.

   L’image ci-dessous présente les deux offres de fragments de contenu différentes qui s’affichent pour l’activité `wknd-adventure-promo`, selon la logique d’Adobe Target.

   ![Offres d’expérience.](./assets/target/offers-in-app.png)

## Félicitations.

Maintenant que nous avons configuré AEM as a Cloud Service pour exporter des fragments de contenu vers Adobe Target, nous avons utilisé les offres de fragments de contenu dans une activité Adobe Target et nous avons affiché cette activité dans une application AEM Headless, en personnalisant l’expérience.
