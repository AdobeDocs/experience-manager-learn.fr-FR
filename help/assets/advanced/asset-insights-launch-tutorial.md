---
title: Configurer des insights sur les ressources avec AEM Assets et Adobe Launch
description: Dans cette série de vidéos en cinq volets, nous présentons la configuration des insights sur les ressources pour Experience Manager déployé via Launch by Adobe.
feature: Asset Insights
version: 6.4, 6.5
topic: Integrations
role: Developer
level: Intermediate
last-substantial-update: 2022-06-04T00:00:00Z
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Assets as a Cloud Service, AEM Assets 6.5" before-title="false"
exl-id: 00125fe1-3bb9-4b1a-a83c-61c413403ae2
source-git-commit: 420dbb7bab84c0f3e79be0cc6b5cff0d5867f303
workflow-type: ht
source-wordcount: '826'
ht-degree: 100%

---

# Configurer des insights sur les ressources avec AEM Assets et Adobe Experience Platform Launch

Dans cette série de vidéos en cinq volets, nous présentons la configuration des insights sur les ressources pour Experience Manager déployé via Adobe Launch.

## Partie 1 : vue d’ensemble des insights sur les ressources {#overview}

Vue d’ensemble des insights sur les ressources. Installez les composants principaux, l’exemple de composant d’image et d’autres packages de contenu pour préparer votre environnement.

>[!VIDEO](https://video.tv.adobe.com/v/25943?quality=12&learn=on)

### Diagramme d’architecture {#architecture-diagram}

![Diagramme d’architecture.](./assets/asset-insights-launch-tutorial/diagram.png)

>[!CAUTION]
>
>Veillez à télécharger la [dernière version des composants principaux](https://github.com/adobe/aem-core-wcm-components) pour votre implémentation.

La vidéo utilise les composants principaux v2.2.2 qui ne sont plus la dernière version. Veillez à utiliser la dernière version avant de passer à la section suivante.

* Télécharger [l’exemple de contenu d’image pour les insights sur les ressources](./assets/asset-insights-launch-tutorial/aem-assets-insights-sample.zip)
* Télécharger [les derniers composants principaux de la gestion de contenu web AEM](https://github.com/adobe/aem-core-wcm-components/releases)

## Partie 2 : activer le tracking des insights sur les ressources pour un exemple de composant d’image {#sample-image-component-asset-insights}

Améliorations apportées aux composants principaux et utilisation du composant proxy (exemple de composant Image) pour les insights sur les ressources. Modification des stratégies de modèle de page de contenu afin d’activer un exemple de composant d’image pour le site de référence.

>[!VIDEO](https://video.tv.adobe.com/v/25944?quality=12&learn=on)

>[!NOTE]
>
>Le composant principal Image comprend la possibilité de désactiver le suivi UUID en désactivant le suivi de l’UUID de la ressource (valeur d’identifiant unique pour un nœud créé dans JCR).

Le composant principal Image utilise l’attribut ***data-asset-id*** dans le parent &lt;div> d’une balise d’image pour activer/désactiver cette fonction. Le composant proxy remplace le composant principal par les modifications suivantes.

* Supprime ***data-asset-id*** de la balise div parente d’un élément &lt;img> dans le fichier image.html.
* Ajoute ***data-aem-asset-id*** directement à l’élément &lt;img> dans image.html.
* Ajoute la valeur ***data-trackable=&#39;true&#39;*** à l’élément &lt;img> dans image.html.
* ***data-aem-asset-id*** et ***data-trackable=&#39;true&#39;*** sont conservés au même niveau de nœud.

>[!NOTE]
>
>*data-aem-asset-id=&#39;image.UUID&#39;* et *data-trackable=&#39;true&#39;* sont les attributs clés qui doivent être présents pour les impressions de ressources. Pour les insights de clics sur les ressources, en plus des attributs de données ci-dessus présents dans la balise &lt;img>, la balise parente &lt;a> doit avoir une valeur href valide.

## Partie 3 : Adobe Analytics - Créer des suites de rapports, activer la collecte de données en temps réel et la création de rapports AEM Assets {#adobe-analytics-asset-insights}

La suite de rapports avec collecte de données en temps réel est créée pour le suivi des ressources. La configuration des insights d’AEM Assets est effectuée à l’aide des informations d’identification Adobe Analytics.

>[!VIDEO](https://video.tv.adobe.com/v/25945?quality=12&learn=on)

>[!NOTE]
>
La collecte de données en temps réel et la création de rapports de ressources AEM doivent être activées pour votre suite de rapports Adobe Analytics. L’activation du reporting des ressources AEM réserve des variables d’analyse pour le suivi des insights sur les ressources.

Pour la configuration des insights sur les ressources AEM, vous avez besoin des informations d’identification suivantes :

* Centre de données
* Nom de la société Analytics
* Nom d’utilisateur ou d’utilisatrice Analytics
* Secret partagé (peut être obtenu à partir de *Adobe Analytics > Admin > Paramètres de la société > Service Web*).
* Suite de rapports (veillez à sélectionner la suite de rapports appropriée utilisée pour les rapports de ressources)

## Partie 4 : utiliser Adobe Experience Platform Launch pour l’ajout d’une extension Adobe Analytics {#part-using-launch-by-adobe-for-adding-adobe-analytics-extension}

Ajout de l’extension Adobe Analytics, création de règles de chargement de page et intégration d’AEM à Launch avec le compte technique Adobe IMS.

>[!VIDEO](https://video.tv.adobe.com/v/25946?quality=12&learn=on)

>[!NOTE]
>
Veillez à répliquer toutes vos modifications de l’instance de création vers l’instance de publication.

### Règle 1 : outil de suivi de page (pagetracker.js) {#rule-page-tracker-pagetracker-js}

```javascript
//For AEM 6.3
<script type="text/javascript" src="http://localhost:4503/etc/clientlibs/foundation/assetinsights/pagetracker.js"></script>
```

```javascript
//For AEM 6.4
<script type="text/javascript" src="http://localhost:4503/etc.clientlibs/dam/clientlibs/assetinsights/pagetracker.js"></script>
```

L‘outil de suivi de page met en œuvre deux rappels (enregistrés dans le code incorporé de la ressource).

* **\&lt;code>assetAnalytics.core.assetLoaded\&lt;code>** : appelé lorsque l’événement « load » est distribué pour asset-DOM-element.
* **\&lt;code>assetAnalytics.core.assetClicked\&lt;code>** : appelé lorsque l’événement « click » est distribué pour asset-DOM-element. Cela n’est pertinent que si asset-DOM-element comporte une balise d’ancrage en tant que parent avec un attribut « href » externe valide.

Enfin, l’outil de suivi de page met en œuvre une fonction d’initialisation.

* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : appelée pour initialiser le composant de l’outil de suivi de page. Elle DOIT être appelée avant que l’un des asset-insights-events (Impressions et/ou Clics) ne soit généré à partir de la page web.
* **\&lt;code>assetAnalytics.dispatcher.init()\&lt;code>** : accepte éventuellement un objet AppMeasurement. S’il est fourni, elle ne tente pas de créer une nouvelle instance de l’objet AppMeasurement.

### Règle 2 : suivi d’images - Action 1 (asset-insights.js) {#rule-image-tracker-action-asset-insights-js}

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

### Règle 2 : suivi d’image - Action 2 (image-tracker.js) {#rule-image-tracker-action-image-tracker-js}

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

* assetAnalytics.core.assetLoaded() : est appelée à la fin du chargement de la page et déclenche les impressions de ressources pour toutes les images pouvant faire l’objet d’un suivi.
* Variable Analytics qui contient la liste des ressources chargées : **contextData[&#39;c.a.assets.idList&#39;]**.
* assetAnalytics.core.assetClicked() : est appelée lorsque l’élément DOM de la ressource comporte une balise d’ancrage avec une valeur href valide. Lorsqu’un utilisateur ou une utilisatrice clique sur une ressource, un cookie est créé avec l’ID de la ressource sur laquelle l’utilisateur ou l’utilisatrice a cliqué comme valeur.**(Nom du cookie : a.assets.clickedid.)**
* Variable Analytics qui contient la liste des ressources chargées : **contextData[&#39;c.a.assets.clickedid&#39;]**.
* Source d’origine : **contextData[&#39;c.a.assets.source&#39;]**.

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

Deux extensions de navigateur Google Chrome sont abordées dans la vidéo pour déboguer Analytics. Des extensions similaires sont également disponibles pour d’autres navigateurs.

* [Extension Launch Switch Chrome](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=fr)
* [Adobe Experience Cloud Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)

Il est également possible de passer la gestion dynamique des balises en mode de débogage avec l’extension Chrome suivante : [Launch and DTM Switch](https://chrome.google.com/webstore/detail/launch-and-dtm-switch/nlgdemkdapolikbjimjajpmonpbpmipk?hl=fr). Cela facilite la vérification des erreurs liées au déploiement de la gestion dynamique des balises. En outre, vous pouvez passer manuellement la gestion dynamique des balises au mode de débogage via *Outils de développement -> Console JS* sur n’importe quel navigateur en ajoutant l’extrait de code suivant :

## Partie 5 : tester le suivi Analytics et synchroniser des données d’insight{#analytics-tracking-asset-insights}

Configurer le planificateur de tâche de synchronisation de rapports AEM Asset et du rapport d’insights Assets

>[!VIDEO](https://video.tv.adobe.com/v/25947?quality=12&learn=on)
