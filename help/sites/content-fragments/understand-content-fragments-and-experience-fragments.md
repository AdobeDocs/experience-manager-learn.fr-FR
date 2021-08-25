---
title: Présentation des fragments de contenu et des fragments d’expérience
description: Les fragments de contenu Adobe Experience Manager et les fragments d’expérience peuvent sembler similaires à la surface, mais chacun joue un rôle clé dans différents cas d’utilisation. Découvrez comment les fragments de contenu et d’expérience sont similaires, différents et quand et comment les utiliser.
sub-product: assets, sites, content services
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.3, 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1037'
ht-degree: 1%

---


# Présentation des fragments de contenu et des fragments d’expérience

Les fragments de contenu Adobe Experience Manager et les fragments d’expérience peuvent sembler similaires à la surface, mais chacun joue un rôle clé dans différents cas d’utilisation. Découvrez comment les fragments de contenu et d’expérience sont similaires, différents et quand et comment les utiliser.

## Comparaison des fragments de contenu et des fragments d’expérience

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragments de contenu (CF)</strong></td>
<td><strong>Fragments d’expérience (XF)</strong></td>
</tr><tr><td><strong>Définition</strong></td>
<td><ul>
<li>Réutilisable <strong>content</strong> indépendant de la présentation, composé d’éléments de données structurés (texte, dates, références, etc.)</li>
</ul>
</td>
<td><ul>
<li>Un composite réutilisable d’un ou plusieurs composants AEM définissant le contenu et la présentation qui forme une <strong>expérience</strong> logique propre</li>
</ul>
</td>
</tr><tr><td><strong>Tenants principaux</strong></td>
<td><ul>
<li>Content-centric</li>
<li>Défini par un modèle de données <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">structuré, basé sur les formulaires.</a></li>
<li>Indépendante de la conception et de la mise en page.</li>
<li>Le canal est responsable de la présentation du contenu du fragment de contenu (mise en page et conception).</li>
</ul>
</td>
<td><ul>
<li>Axé sur la présentation</li>
<li>Défini par la composition non structurée des composants AEM</li>
<li>Définit la conception et la mise en page du contenu</li>
<li>Utilisé "en l’état" dans les canaux</li>
</ul>
</td>
</tr><tr><td><strong>Détails techniques</strong></td>
<td><ul>
<li>Mise en oeuvre en tant que <strong>dam:Asset</strong></li>
<li>Défini par un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modèle de fragment de contenu</a></li>
</ul>
</td>
<td><ul>
<li>Mise en oeuvre en tant que <strong>cq:Page</strong></li>
<li>Défini par les modèles modifiables</li>
<li>Rendu HTML natif</li>
</ul>
</td>
</tr><tr><td><strong>Variations</strong></td>
<td><ul>
<li>La variation de Principal est la variation canonique.</li>
<li>Les variations sont spécifiques à un cas d’utilisation, qui peut s’aligner sur les canaux.</li>
</ul>
</td>
<td><ul>
<li>Les variations sont spécifiques au canal ou au contexte.</li>
<li>Les variations sont synchronisées via AEM Live Copy</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Réutilisation du contenu </a> de diffusion de bloc sur plusieurs variantes</li>
</ul>
</td>
</tr><tr><td><strong>Fonctions</strong></td>
<td><ul>
<li>Variations</li>
<li>Versions</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank"></a> Synchronisation du contenu entre les variations</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Versions </a> de fragments de contenu différées visuelles</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank"></a> Annotations d’éléments de texte multiligne</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">synthèse</a> intelligente des éléments de texte multiligne.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/creating-translation-projects-for-content-fragments.html?lang=en" target="_blank">Traduction/localisation</a></li>
</ul>
</td>
<td><ul>
<li>Variations</li>
<li>Variations en tant que Live Copies</li>
<li>Versions</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en#building-blocks" target="_blank">Blocs de création</a></li>
<li>Annotations</li>
<li>Mise en page et aperçu réactifs</li>
<li>Traduction/localisation</li>
</ul>
</td>
</tr><tr><td><strong>Utilisez</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM composant de fragment de contenu des composants principaux </a> à utiliser dans AEM Sites, AEM Screens ou dans des fragments d’expérience.</li>
<li>Exportation JSON via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> pour une consommation tierce</li>
<li>JSON via les API Assets HTTP AEM pour une consommation tierce.</li>
</ul>
</td>
<td><ul>
<li>AEM composant Fragment d’expérience à utiliser dans AEM Sites, AEM Screens ou d’autres fragments d’expérience.</li>
<li>Exporter en <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML brut</a> pour une utilisation par des systèmes tiers</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Exportation HTML vers Adobe </a> Target pour les offres ciblées</li>
<li>Exportation JSON vers Adobe Target pour les offres ciblées</li>
</ul>
</td>
</tr><tr><td><strong>Cas d’utilisation courants</strong></td>
<td><ul>
<li>Contenu basé sur des formulaires/saisie de données hautement structuré</li>
<li>Contenu éditorial de longue forme (élément multi-lignes)</li>
<li>Contenu géré en dehors du cycle de vie des canaux qui le diffusent</li>
</ul>
</td>
<td><ul>
<li>Gestion centralisée des supports promotionnels multicanaux à l’aide de variations par canal.</li>
<li>Contenu réutilisé sur plusieurs pages d’un site Web.</li>
<li>Chrome du site web (ex. en-tête et pied de page)</li>
<li>Expérience gérée en dehors du cycle de vie des canaux qui la diffusent</li>
</ul>
</td>
</tr><tr><td><strong>Documentation</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guide de l’utilisateur des fragments de contenu AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en" target="_blank">Utilisation de fragments de contenu dans AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">Adobe de la documentation sur les fragments d’expérience</a></li>
</ul>
</td>
</tr></tbody></table>

## Architecture des fragments de contenu

Le diagramme suivant illustre l’architecture globale des fragments de contenu AEM

!![Architecture des fragments de contenu](./assets/content-fragments-architecture.png)

+ **Les** modèles de fragment de contenu définissent les éléments (ou champs) qui définissent le contenu que le fragment de contenu peut capturer et exposer.
+ Le **fragment de contenu** est une instance d’un modèle de fragment de contenu qui représente une entité de contenu logique.
+ Les variations **de fragment de contenu** se conforment au modèle de fragment de contenu, mais présentent des variations de contenu.
+ Les fragments de contenu peuvent être exposés/consommés par :
   + Utilisation de fragments de contenu sur **AEM Sites** (ou AEM Screens) via le composant Fragment de contenu des composants principaux de la gestion de contenu web AEM.
   + Incorporation d’un fragment de contenu dans un **fragment d’expérience** via le composant Fragment de contenu des composants principaux de la gestion de contenu web AEM, à utiliser dans n’importe quel cas d’utilisation de fragment d’expérience.
   + Exposition d’un contenu de variations de fragment de contenu au format JSON via **AEM Content Services** et des pages d’API pour des cas d’utilisation en lecture seule.
   + Exposition directe du contenu du fragment de contenu (toutes les variations) au format JSON via des appels directs vers AEM Assets via l’**API HTTP AEM Assets** pour les cas d’utilisation CRUD.

## Architecture des fragments d’expérience

!![Architecture des fragments d’expérience](./assets/experience-fragments-architecture.png)

+ **Les modèles modifiables**, qui sont à leur tour définis par des  **modèles** modifiables et une mise en oeuvre de composant de page  **AEM**, définissent les composants AEM autorisés qui peuvent être utilisés pour composer un fragment d’expérience.
+ Le **fragment d’expérience** est une instance d’un modèle modifiable qui représente une expérience logique.
+ Les variations **du fragment d’expérience** se conforment au modèle modifiable, mais ont des variations d’expérience (contenu et conception).
+ Les fragments d’expérience peuvent être exposés/consommés par :
   + Utilisation de fragments d’expérience sur AEM Sites (ou AEM Screens) via le composant Fragment d’expérience AEM.
   + Exposition du contenu d’un fragment d’expérience au format JSON (avec du code HTML incorporé) via **AEM Content Services** et les pages d’API.
   + Exposition directe d’une variation de fragment d’expérience sous la forme **&quot;HTML brut&quot;**.
   + Exportation de fragments d’expérience vers **Adobe Target** sous la forme d’offres HTML ou JSON.
   + AEM Sites prend en charge les offres HTML en mode natif. Toutefois, les offres JSON nécessitent un développement personnalisé.

## Matériel de prise en charge des fragments de contenu

+ [Guide de l’utilisateur des fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Utilisation de fragments de contenu dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM composant de fragment de contenu des composants principaux WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html)
+ [Utilisation de fragments de contenu et AEM sans affichage](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=en)
+ [Prise en main d’AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Matériel de prise en charge des fragments d’expérience

+ [Adobe de la documentation sur les fragments d’expérience](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Présentation des fragments d’expérience AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilisation de fragments d’expérience AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilisation de fragments d’expérience AEM avec Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
