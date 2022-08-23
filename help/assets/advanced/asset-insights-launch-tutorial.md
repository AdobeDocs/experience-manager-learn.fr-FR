---
title: Configuration des statistiques sur les ressources avec AEM Assets et Adobe Launch
description: Dans cette série de vidéos en 5 parties, nous examinons la configuration et la configuration des statistiques sur les ressources pour le Experience Manager déployé via Launch by Adobe.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '821'
ht-degree: 1%

---

# Configuration des statistiques sur les ressources avec AEM Assets et Adobe Experience Platform Launch

Dans cette série de vidéos en 5 parties, nous examinons la configuration et la configuration des statistiques sur les ressources pour les Experience Manager déployés via Adobe Launch.

## Partie 1 : Présentation des statistiques sur les ressources {#overview}

Présentation des statistiques sur les ressources. Installez les composants principaux, l’exemple de composant d’image et d’autres modules de contenu pour préparer votre environnement.

>[!VIDEO](https://video.tv.adobe.com/v/25943/?quality=12&learn=on)

### Schéma d’architecture {#architecture-diagram}

![Schéma d’architecture](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Veillez à télécharger le [dernière version des composants principaux](https://github.com/adobe/aem-core-wcm-components) pour votre mise en oeuvre.

La vidéo utilise les composants principaux v2.2.2 qui ne sont plus la dernière version ; veillez à utiliser la dernière version avant de passer à la section suivante.

* Télécharger [Exemple de contenu d’image pour Asset Insights](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Télécharger [les derniers composants principaux de la gestion de contenu web AEM](https://github.com/adobe/aem-core-wcm-components/releases)

## Partie 2 : Activation du suivi des statistiques sur les ressources pour un exemple de composant d’image {#sample-image-component-asset-insights}

Améliorations apportées aux composants principaux et utilisation du composant proxy (exemple de composant Image) pour les statistiques sur les ressources. Modification des stratégies de modèle de page de contenu afin d’activer un exemple de composant d’image pour le site de référence.

>[!VIDEO](https://video.tv.adobe.com/v/25944/?quality=12&learn=on)

>[!NOTE]
>
>Le composant principal Image comprend la possibilité de désactiver le suivi UUID en désactivant le suivi de l’UUID de la ressource (valeur d’identifiant unique pour un noeud créé dans JCR).

Le composant Image principale utilise les ***data-asset-id*** dans le parent &lt;div> d’une balise d’image pour activer/désactiver cette fonction. Le composant proxy remplace le composant principal par les modifications suivantes.

* Supprime la variable ***data-asset-id*** à partir de la balise div parente d’un  &lt;img> élément dans le fichier image.html
* Ajouts ***data-aem-asset-id*** directement à l’ &lt;img> élément dans image.html
* Ajouts ***data-trackable=&#39;true&#39;*** à l’ &lt;img> élément dans image.html.
* ***data-aem-asset-id*** et ***data-trackable=&#39;true&#39;*** sont conservées au même niveau de noeud.

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* et *data-trackable=&#39;true&#39;* sont les attributs clés qui doivent être présents pour les impressions de ressources. Pour Asset Click Insights, en plus des attributs de données ci-dessus présents dans la  &lt;img> balise , la balise parente doit avoir une valeur href valide.

## Partie 3 : Adobe Analytics : création de suites de rapports, activation de la collecte de données en temps réel et de la création de rapports AEM Assets {#adobe-analytics-asset-insights}

La suite de rapports avec collecte de données en temps réel est créée pour le suivi des ressources. La configuration d’AEM Assets Insights est configurée à l’aide des informations d’identification Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945/?quality=12&learn=on)

>[!NOTE]
La collecte de données en temps réel et la création de rapports de ressources AEM doivent être activées pour votre suite de rapports Adobe Analytics. L’activation du reporting d’AEM ressources réserve des variables d’analyse pour le suivi des informations sur les ressources.

Pour la configuration d’AEM Assets Insights, vous avez besoin des informations d’identification suivantes :

* Centre de données
* Nom de la société Analytics
* Nom d’utilisateur Analytics
* Secret partagé (peut être obtenu à partir de *Adobe Analytics > Admin > Paramètres de la société > Service Web*).
* Suite de rapports (veillez à sélectionner la suite de rapports appropriée utilisée pour les rapports de ressources)

## Partie 4 : Utilisation d’Adobe Experience Platform Launch pour l’ajout d’une extension Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Ajout de l’extension Adobe Analytics, création de règles de chargement de page et intégration d’AEM à Launch avec le compte technique Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946/?quality=12&learn=on)

>[!NOTE]
Veillez à répliquer toutes vos modifications de l’instance de création vers l’instance de publication.

### Règle 1 : Suivi de page (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

Le dispositif de suivi de page met en oeuvre deux rappels (enregistrés dans le code incorporé de la ressource).

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : appelé lorsque l’événement &quot;load&quot; est distribué pour l’élément asset-DOM.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : appelé lorsque l’événement &quot;click&quot; est distribué pour l’élément asset-DOM, cela n’est pertinent que lorsque l’élément asset-DOM-element a une balise d’ancrage parent avec un attribut &quot;href&quot; externe valide

Enfin, Pagetracker met en oeuvre une fonction d’initialisation en tant que .

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : appelé pour initialiser le composant Pagetracker. Il DOIT être appelé avant que l’un des événements-insights-de-ressource (impressions et/ou clics) ne soit généré à partir de la page web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : accepte éventuellement un objet AppMeasurement ; s’il est fourni, il ne tente pas de créer une instance de l’objet AppMeasurement.

### Règle 2 : Suivi d’image — Action 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

```javascript
/*
 * AEM Asset Insights
 */

var sObj = window.s;
_satellite.notify('in assetAnalytics customInit');
(function initializeAssetAnalytics() {
 if ((!!window.assetAnalytics) && (!!assetAnalytics.dispatcher)) {
 _satellite.notify('assetAnalytics ready');
 /** NOTE:
  Copy over the call to 'assetAnalytics.dispatcher.init()' from Assets Pagetracker
  Be mindful about changing the AppMeasurement object as retrieved above.
  */
 assetAnalytics.dispatcher.init(
                                "",  /** RSID to send tracking-call to */
                                "",  /** Tracking Server to send tracking-call to */
                                "",  /** Visitor Namespace to send tracking-call to */
                                "",  /** listVar to put comma-separated-list of Asset IDs for Asset Impression Events in tracking-call, e.g. 'listVar1' */
                                "",  /** eVar to put Asset ID for Asset Click Events in, e.g. 'eVar3' */
                                "",  /** event to include in tracking-calls for Asset Impression Events, e.g. 'event8' */
                                "",  /** event to include in tracking-calls for Asset Click Events, e.g. 'event7' */
                                sObj  /** [OPTIONAL] if the webpage already has an AppMeasurement object, please include the object here. If unspecified, Pagetracker Core shall create its own AppMeasurement object */
                                );
 sObj.usePlugins = true;
 sObj.doPlugins = assetAnalytics.core.updateContextData;
}
 else {
 _satellite.notify('assetAnalytics not available. Consider updating the Custom Page Code', 4);
 }
})();
```

### Règle 2 : Suivi d’image — Action 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

```javascript
/*
 * AEM Asset Insights
 */

document.querySelectorAll('[data-aem-asset-id]').forEach(function(element) {
    assetAnalytics.core.assetLoaded(element);
    var parent = element.parentElement;
    if (parent.nodeName == "A") {
        parent.addEventListener("click", function() {
            assetAnalytics.core.assetClicked(this)
        });
    }
});
```

* assetAnalytics.core.assetLoaded() : est appelé à la fin du chargement de la page et déclenche les impressions de ressources pour toutes les images pouvant faire l’objet d’un suivi.
* Variable Analytics qui contient la liste des ressources chargées : **contextData[&quot;c.a.assets.idList&quot;]**
* assetAnalytics.core.assetClicked() : est appelé lorsque l’élément DOM de la ressource comporte une balise d’ancrage avec une valeur href valide. Lorsqu’un utilisateur clique sur une ressource, un cookie est créé avec comme valeur l’ID de ressource sur lequel l’utilisateur a cliqué.**(Nom du cookie : a.assets.clickedid)**
* Variable Analytics qui contient la liste des ressources chargées : **contextData[&#39;c.a.assets.clickedid&#39;]**
* Source de l’origine : **contextData[&#39;c.a.assets.source&#39;]**

### Instructions de débogage de la console {#console-debug-statements}

```javascript
//Launch Build Info
_satellite.buildInfo

//Enables debug messages
_satellite.setDebug(true);

//Asset Insight JS Object
assetAnalytics

//List of trackable images
document.querySelectorAll(".cmp-image__image");
```

Deux extensions de navigateur Google Chrome sont référencées dans la vidéo pour déboguer Analytics. Des extensions similaires sont également disponibles pour d’autres navigateurs.

* [Extension Launch Switch Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en)
* [Débogueur Adobe Experience Cloud](https://chrome.google.com/webstore/detail/adobe-experience-cloud-de/ocdmogmohccmeicdhlhhgepeaijenapj?hl=en)

Il est également possible de passer la gestion dynamique des balises en mode de débogage avec l’extension Chrome suivante : [Launch et DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=en). Cela facilite la vérification des erreurs liées au déploiement de la gestion dynamique des balises. En outre, vous pouvez basculer manuellement la gestion dynamique des balises vers le mode de débogage via n’importe quel navigateur. *Outils de développement -> Console JS* en ajoutant le fragment de code suivant :

## Partie 5 : Test du suivi Analytics et synchronisation des données Insight{#analytics-tracking-asset-insights}

Configuration AEM rapport Planificateur de tâche de synchronisation de rapports de ressources et Statistiques sur les ressources

>[!VIDEO](https://video.tv.adobe.com/v/25947/?quality=12&learn=on)
