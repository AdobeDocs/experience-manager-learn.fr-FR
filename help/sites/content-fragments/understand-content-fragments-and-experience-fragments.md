---
title: Fragments de contenu et fragments d’expérience
description: Découvrez les similitudes et les différences entre les fragments de contenu et les fragments d’expérience, ainsi que leur utilisation et finalité.
sub-product: Experience Manager Assets, Experience Manager Sites
feature: Content Fragments, Experience Fragments
topics: headless
version: 6.4, 6.5
doc-type: article
activity: understand
audience: all
topic: Content Management
role: User
level: Beginner
exl-id: ccbc68d1-a83e-4092-9a49-53c56c14483e
source-git-commit: 84fdbaa173a929ae7467aecd031cacc4ce73538a
workflow-type: ht
source-wordcount: '1044'
ht-degree: 100%

---

# Fragments de contenu et fragments d’expérience

Les fragments de contenu et les fragments d’expérience d’Adobe Experience Manager peuvent sembler similaires de prime abord, mais chacun prouve son utilité dans des cas d’utilisation précis. Découvrez les différences et similitudes entre les fragments de contenu et d’expérience et leurs différentes utilisations et finalités.

## Comparaison

<table>
<tbody><tr><td><strong> </strong></td>
<td><strong>Fragments de contenu (CF)</strong></td>
<td><strong>Fragments d’expérience (XF)</strong></td>
</tr><tr><td><strong>Définition</strong></td>
<td><ul>
<li>Il s’agit d’un <strong>contenu</strong> réutilisable, agnostique sur le plan de la présentation, composé d’éléments de données structurés (texte, dates, références, etc.).</li>
</ul>
</td>
<td><ul>
<li>Ce composite réutilisable d’un ou de plusieurs composants AEM définit le contenu et la présentation constitutifs d’une <strong>expérience</strong> et est significatif en soi.</li>
</ul>
</td>
</tr><tr><td><strong>Clients principaux</strong></td>
<td><ul>
<li>Axé sur le contenu</li>
<li>Défini par un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=fr" target="_blank">modèle de données structuré, basé sur des formulaires.</a></li>
<li>Ne dépend pas de la conception et de la disposition.</li>
<li>Le canal est responsable de la présentation du contenu du fragment de contenu (disposition et conception).</li>
</ul>
</td>
<td><ul>
<li>Axé sur la présentation</li>
<li>Défini par la composition non structurée des composants AEM</li>
<li>Définit la conception et la disposition du contenu</li>
<li>Utilisé « en l’état » dans les canaux</li>
</ul>
</td>
</tr><tr><td><strong>Détails techniques</strong></td>
<td><ul>
<li>Implémenté en tant que <strong>dam:Asset</strong>.</li>
<li>Défini par un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=fr" target="_blank">Modèle de fragment de contenu</a>.</li>
</ul>
</td>
<td><ul>
<li>Implémenté en tant que <strong>cq:Page</strong>.</li>
<li>Défini par les modèles modifiables.</li>
<li>Rendu HTML natif</li>
</ul>
</td>
</tr><tr><td><strong>Variations</strong></td>
<td><ul>
<li>La variation principale est la variation canonique.</li>
<li>Les variations sont spécifiques aux cas d’utilisation, qui peuvent s’aligner sur les canaux.</li>
</ul>
</td>
<td><ul>
<li>Les variations sont spécifiques au canal ou au contexte.</li>
<li>Les variations sont synchronisées via AEM Live Copy.</li>
<li>Les <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=fr" target="_blank">Blocs de création</a> permettent de réutiliser le contenu dans toutes les variations.</li>
</ul>
</td>
</tr><tr><td><strong>Fonctions</strong></td>
<td><ul>
<li>Variations</li>
<li>Versions</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=fr#synchronizing-with-master" target="_blank">Synchronisation</a> du contenu entre plusieurs variations</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-managing.html?lang=fr#comparing-fragment-versions" target="_blank">Outil de comparaison</a> des versions de fragments de contenu</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-variations.html?lang=fr#annotating-a-content-fragment" target="_blank">Annotations</a> de plusieurs éléments de texte multiligne</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments-variations.html?lang=fr#summarizing-text" target="_blank">Récapitulation</a> intelligente d’éléments de texte multiligne.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/creating-translation-projects-for-content-fragments.html?lang=fr" target="_blank">Traduction/localisation</a></li>
</ul>
</td>
<td><ul>
<li>Variations</li>
<li>Variations comme Live Copies</li>
<li>Versions</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=fr#building-blocks" target="_blank">Blocs de création</a></li>
<li>Annotations</li>
<li>Disposition et prévisualisation réactives</li>
<li>Traduction/localisation</li>
<li>Modèle de données complexe via les références de fragments de contenu</li>
<li>Prévisualisation in-app</li>
</ul>
</td>
</tr><tr><td><strong>Utilisez</strong></td>
<td><ul>
<li>Export JSON via les <a href="https://experienceleague.adobe.com/landing/experience-manager/headless/developer.html?lang=fr">API GraphQL AEM Headless</a>.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr" target="_blank">Composants principaux AEM : composant de fragment de contenu</a> à utiliser dans AEM Sites, AEM Screens ou dans les fragments d’expérience.</li>
<li>Export JSON via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=fr" target="_blank">AEM Content Services</a> pour la consommation tierce.</li>
<li>Export JSON vers Adobe Target pour les offres ciblées.</li>
<li>JSON via les API Assets HTTP AEM pour une consommation tierce.</li>
</ul>
</td>
<td><ul>
<li>Composant Fragment d’expérience AEM à utiliser dans AEM Sites, AEM Screens ou d’autres fragments d’expérience.</li>
<li>Exporter en tant que <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=fr" target="_blank">HTML brut</a> pour une utilisation par des systèmes tiers.</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=fr" target="_blank">Export HTML vers Adobe Target</a> pour les offres ciblées.</li>
<li>Export JSON vers Adobe Target pour les offres ciblées.</li>
</ul>
</td>
</tr><tr><td><strong>Cas d’utilisation courants :</strong></td>
<td><ul>
<li>Optimisation des cas d’utilisation headless sur GraphQL.</li>
<li>Contenu structuré basé sur la saisie de données/les formulaires.</li>
<li>Contenu éditorial à la forme longue (élément multi-lignes).</li>
<li>Contenu géré en dehors du cycle de vie des canaux qui le diffusent.</li>
</ul>
</td>
<td><ul>
<li>Gestion centralisée des supports promotionnels multicanaux à l’aide de variations par canal.</li>
<li>Contenu réutilisé sur plusieurs pages d’un site Web.</li>
<li>Chrome du site Web (par exemple, en-tête et pied de page).</li>
<li>Expérience gérée en dehors du cycle de vie des canaux qui la diffusent.</li>
</ul>
</td>
</tr><tr><td><strong>Documentation</strong></td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=fr&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js" target="_blank">Guide d’utilisation des fragments de contenu AEM</a></li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=fr" target="_blank">Utiliser des fragments de contenu dans AEM</a></li>
</ul>
</td>
<td><ul>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=fr" target="_blank">Documentation d’Adobe sur les fragments d’expérience.</a></li>
</ul>
</td>
</tr></tbody></table>

## Architecture des fragments de contenu

Le diagramme suivant illustre l’architecture globale des fragments de contenu d’AEM.

![Architecture des fragments de contenu](./assets/content-fragments-architecture.png)

+ Les **modèles de fragment de contenu** définissent les éléments (ou champs) qui définissent le contenu que le fragment de contenu peut capturer et exposer.
+ Le **fragment de contenu** est une instance d’un modèle de fragment de contenu qui représente une entité de contenu logique.
+ Les **variations** des fragments de contenu adhèrent au modèle de fragment de contenu ; cependant, la conformité au modèle de fragment de contenu comporte des variations de contenu.
+ Les fragments de contenu peuvent être exposés/consommés par les éléments suivants :
   + Utilisation des fragments de contenu sur **AEM Sites** (ou AEM Screens) via le composant Fragment de contenu des composants principaux de la gestion de contenu web d’AEM.
   + Consommation d’un **fragment de contenu** depuis les applications Headless à l’aide des API GraphQL AEM Headless.
   + Exposition d’un contenu de variation de fragment de contenu au format JSON via **AEM Content Services** et les pages d’API pour les cas d’utilisation en lecture seule.
   + Exposition directe du contenu du fragment de contenu (toutes les variations) au format JSON via des appels directs vers AEM Assets via l’**API HTTP AEM Assets** pour les cas d’utilisation CRUD.

## Architecture des fragments d’expérience

![Architecture des fragments d’expérience](./assets/experience-fragments-architecture.png)

+ Les **modèles modifiables**, qui à leur tour sont définis par des **types de modèles modifiables** et une **mise en œuvre d’un composant de page AEM**, définissent les composants AEM autorisés pouvant être utilisés pour composer un fragment d’expérience.
+ Le **fragment d’expérience** est une instance d’un modèle modifiable qui représente une expérience logique.
+ Les **variations** des fragments d’expérience adhèrent au modèle modifiable ; elles ont cependant une expérience avec des variations (contenu et conception).
+ Les fragments d’expérience peuvent être exposés/consommés par les éléments suivants :
   + Utilisation de fragments d’expérience sur AEM Sites (ou AEM Screens) via le composant Fragment d’expérience d’AEM.
   + Exposition d’un contenu de variations de fragment d’expérience au format JSON (avec HTML incorporé) via **AEM Content Services** et des pages API.
   + Exposition directe d’une variation de fragment d’expérience en tant que **« HTML brut »**.
   + Export de fragments d’expérience vers **Adobe Target** en tant qu’offres HTML ou JSON.
   + AEM Sites prend nativement en charge les offres HTML. Toutefois, les offres JSON nécessitent un développement personnalisé.

## Ressource de prise en charge des fragments de contenu

+ [Guide d’utilisation des fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=fr&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Présentation d’Adobe Experience Manager en tant que CMS découplé](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/introduction.html?lang=fr)
+ [Utiliser les fragments de contenu dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=fr)
+ [Composant de fragment de contenu des composants principaux WCM d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr)
+ [Utiliser les fragments de contenu et AEM Headless](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=fr)
+ [Prise en main d’AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=fr)

## Ressource de prise en charge des fragments d’expérience

+ [Documentation d’Adobe sur les fragments d’expérience](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=fr)
+ [Comprendre les fragments d’expérience d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=fr)
+ [Utiliser les fragments d’expérience d’AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=fr)
+ [Utiliser les fragments d’expérience d’AEM avec Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
