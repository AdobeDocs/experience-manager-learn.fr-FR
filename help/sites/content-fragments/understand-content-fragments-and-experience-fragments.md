---
title: Présentation des fragments de contenu et des fragments d’expérience
description: Découvrez les similitudes et les différences entre les fragments de contenu et les fragments d’expérience, ainsi que le moment et la manière d’utiliser chaque type.
sub-product: assets, sites, content services
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
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '1016'
ht-degree: 3%

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
<li>Réutilisable, agnostique en matière de présentation <strong>content</strong>, composé d’éléments de données structurés (texte, dates, références, etc.)</li>
</ul>
</td>
<td><ul>
<li>Un composite réutilisable d’un ou de plusieurs composants AEM définissant le contenu et la présentation qui forment une <strong>expérience</strong> ce qui a du sens tout seul</li>
</ul>
</td>
</tr><tr><td><strong>Tenants principaux</strong></td>
<td><ul>
<li>Content-centric</li>
<li>Défini par un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">modèle de données structuré, basé sur des formulaires.</a></li>
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
<li>Implémenté en tant que <strong>dam:Asset</strong></li>
<li>Défini par un <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-models.html?lang=en" target="_blank">Modèle de fragment de contenu</a></li>
</ul>
</td>
<td><ul>
<li>Implémenté en tant que <strong>cq:Page</strong></li>
<li>Défini par les modèles modifiables</li>
<li>Rendu de HTML natif</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html" target="_blank">Blocs de création</a> autoriser la réutilisation du contenu entre les variations</li>
</ul>
</td>
</tr><tr><td><strong>Fonctions</strong></td>
<td><ul>
<li>Variations</li>
<li>Versions</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#synchronizing-with-master" target="_blank">Synchronisation</a> du contenu entre différentes variations</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-managing.html?lang=en#comparing-fragment-versions" target="_blank">Outil de comparaison visuelle</a> des versions de fragments de contenu</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#annotating-a-content-fragment" target="_blank">Annotations</a> de plusieurs éléments de texte multiligne ;</li>
<li>Intelligent <a href="https://experienceleague.adobe.com/docs/experience-manager-65/assets/fragments/content-fragments-variations.html?lang=en#summarizing-text" target="_blank">synthèse</a> d’éléments de texte multiligne.</li>
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
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html" target="_blank">AEM composant de fragment de contenu des composants principaux</a> à utiliser dans AEM Sites, AEM Screens ou dans les fragments d’expérience.</li>
<li>Exportation JSON via <a href="https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en" target="_blank">AEM Content Services</a> pour la consommation tierce</li>
<li>JSON via les API Assets HTTP AEM pour une consommation tierce.</li>
</ul>
</td>
<td><ul>
<li>AEM composant Fragment d’expérience à utiliser dans AEM Sites, AEM Screens ou d’autres fragments d’expérience.</li>
<li>Exporter en tant que <a href="https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en" target="_blank">HTML brut</a> pour une utilisation par des systèmes tiers</li>
<li><a href="https://experienceleague.adobe.com/docs/experience-manager-65/administering/integration/experience-fragments-target.html?lang=en" target="_blank">Exportation HTML vers Adobe Target</a> pour les offres ciblées</li>
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

+ **Modèles de fragment de contenu** définir les éléments (ou champs) qui définissent le contenu que le fragment de contenu peut capturer et exposer ;
+ Le **Fragment de contenu** est une instance d’un modèle de fragment de contenu qui représente une entité de contenu logique.
+ Fragment de contenu **variations** Toutefois, la conformité au modèle de fragment de contenu comporte des variations de contenu.
+ Les fragments de contenu peuvent être exposés/consommés par :
   + Utilisation des fragments de contenu sur **AEM Sites** (ou AEM Screens) via le composant Fragment de contenu des composants principaux de la gestion de contenu web AEM.
   + Incorporation d’un fragment de contenu dans une **Fragment d’expérience** via le composant Fragment de contenu des composants principaux de la gestion de contenu web d’AEM, à utiliser dans n’importe quel cas d’utilisation de fragment d’expérience.
   + Exposition d’un contenu de variation de fragment de contenu au format JSON via **AEM Content Services** et pages d’API pour les cas d’utilisation en lecture seule.
   + Exposition directe du contenu du fragment de contenu (toutes les variations) au format JSON via des appels directs vers AEM Assets via le **API HTTP AEM Assets** pour les cas d’utilisation CRUD.

## Architecture des fragments d’expérience

!![Architecture des fragments d’expérience](./assets/experience-fragments-architecture.png)

+ **Modèles modifiables**, qui à son tour sont définis par **Types de modèle modifiables** et un **Implémentation AEM composant Page**, définissez les composants d’AEM autorisés qui peuvent être utilisés pour composer un fragment d’expérience.
+ Le **Fragment d’expérience** est une instance d’un modèle modifiable qui représente une expérience logique.
+ Fragment d’expérience **variations** Toutefois, les variations d’expérience (contenu et conception) sont conformes au modèle modifiable.
+ Les fragments d’expérience peuvent être exposés/consommés par :
   + Utilisation de fragments d’expérience sur AEM Sites (ou AEM Screens) via le composant Fragment d’expérience AEM.
   + Exposition d’un contenu de variations de fragment d’expérience au format JSON (avec HTML incorporé) via **AEM Content Services** et pages API.
   + Exposition directe d’une variation de fragment d’expérience en tant que **&quot;HTML brut&quot;**.
   + Exportation de fragments d’expérience vers **Adobe Target** comme offres par HTML ou JSON.
   + AEM Sites prend en charge les offres par HTML en mode natif. Toutefois, les offres JSON nécessitent un développement personnalisé.

## Matériel de prise en charge des fragments de contenu

+ [Guide de l’utilisateur des fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-65/assets/home.html?lang=en&amp;topic=/experience-manager/6-5/assets/morehelp/content-fragments.ug.js)
+ [Utilisation de fragments de contenu dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/content-fragments-feature-video-use.html?lang=en)
+ [AEM composant de fragment de contenu des composants principaux WCM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/components/content-fragment-component.html?lang=fr)
+ [Utilisation de fragments de contenu et AEM sans affichage](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/overview.html?lang=fr)
+ [Prise en main d’AEM Content Services](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-with-aem-headless/content-services/overview.html?lang=en)

## Matériel de prise en charge des fragments d’expérience

+ [Adobe de la documentation sur les fragments d’expérience](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/authoring/experience-fragments.html?lang=en)
+ [Présentation des fragments d’expérience AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilisation de fragments d’expérience AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)
+ [Utilisation de fragments d’expérience AEM avec Adobe Target](https://medium.com/adobetech/experience-fragments-and-adobe-target-d8d74381b9b2)
