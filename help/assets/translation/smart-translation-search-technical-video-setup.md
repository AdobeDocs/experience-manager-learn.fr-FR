---
title: Configuration de la recherche de traduction intelligente avec AEM Assets
description: Smart Translation Search permet d’utiliser des termes de recherche non anglais pour résoudre le problème en anglais. Pour configurer l'AEM pour Smart Translation Search, le lot OSGi de Apache Oak Search Machine Translation doit être installé et configuré, ainsi que les paquets de langues Apache Joshua gratuits et open source pertinents qui contiennent les règles de traduction.
version: 6.3, 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '872'
ht-degree: 1%

---


# Configuration de la recherche Smart Translation avec AEM Assets{#set-up-smart-translation-search-with-aem-assets}

Smart Translation Search permet d’utiliser des termes de recherche non anglais pour résoudre le problème en anglais. Pour configurer l&#39;AEM pour Smart Translation Search, le lot OSGi de Apache Oak Search Machine Translation doit être installé et configuré, ainsi que les paquets de langues Apache Joshua gratuits et open source pertinents qui contiennent les règles de traduction.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>La recherche de traduction intelligente doit être configurée sur chaque instance AEM qui en a besoin.

1. Téléchargement et installation du lot OSGi Oak Search Machine Translation
   * [Téléchargez le ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) lot OSGi de traduction automatique Oak Search qui correspond à AEM version Oak.
   * Installez le lot OSGi de traduction de l&#39;outil de recherche Oak téléchargé dans AEM par [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Téléchargement et mise à jour des modules linguistiques Apache Joshua
   * Téléchargez et décompressez les [packs de langue Apache Joshua ](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) souhaités.
   * Modifiez le fichier `joshua.config` et placez en commentaire les 2 lignes qui commencent par :

      ```
      feature-function = LanguageModel ...
      ```

   * Déterminez et enregistrez la taille du dossier de modèles du module linguistique, car cela a une incidence sur la quantité d’espace de mémoire supplémentaire AEM nécessaire.
   * Déplacez le dossier Apache Joshua Language Pack décompressé (avec les `joshua.config` modifications) vers

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Par exemple :

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Redémarrez AEM avec une allocation de mémoire de tas mise à jour
   * Arrêtez AEM
   * Déterminer la nouvelle taille de tas requise pour AEM

      * AEM taille du tas de pré-langue sans mémoire + la taille du répertoire de modèles arrondi à 2 Go le plus proche
      * Par exemple : Si l’installation AEM des packs de prélangue nécessite 8 Go de tas pour être exécutée et que le dossier du modèle du pack de langue est de 3,8 Go non compressé, la nouvelle taille du tas est la suivante :

         L&#39;original `8GB` + ( `3.75GB` arrondi au `2GB` le plus proche, qui est `4GB`) pour un total de `12GB`
   * Vérifiez que la machine dispose de cette quantité de mémoire disponible.
   * Mettre à jour les scripts de début-up AEM pour les adapter à la nouvelle taille du tas

      * Ex. `java -Xmx12g -jar cq-author-p4502.jar`
   * Redémarrez l&#39;AEM avec la taille du tas augmentée.

   >[!NOTE]
   >
   >L’espace de mémoire requis pour les packs de langues peut s’agrandir, en particulier lorsque plusieurs packs de langues sont utilisés.
   >
   >
   >Veillez toujours à ce que **l’instance ait suffisamment de mémoire** pour s’adapter à l’augmentation de l’espace de tas alloué.
   >
   >
   >Le tas de base **doit toujours être calculé pour prendre en charge des performances acceptables sans que des modules linguistiques** ne soient installés.

4. Enregistrement des modules linguistiques via les configurations OSGi du fournisseur Apache Jackrabbit Oak Machine Translation Termes de Requête de texte intégral

   * Pour chaque module linguistique, [créez une nouvelle configuration OSGi du fournisseur Apache Jackrabbit Oak Machine Translation Termes de Requête de texte intégral OSGi via le gestionnaire de configuration de la console Web AEM.](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory)

      * `Joshua Config Path` est le chemin d’accès absolu au fichier joshua.config. Le processus AEM doit être en mesure de lire tous les fichiers du dossier du module linguistique.
      * `Node types` sont les types de noeud candidats dont la recherche en texte intégral engage ce pack de langue pour la traduction.
      * `Minimum score` est le score de confiance minimum pour un terme traduit à utiliser.

         * Par exemple, hombre (en espagnol pour &quot;homme&quot;) peut se traduire par le mot anglais &quot;homme&quot; avec un score de confiance de `0.9` et par le mot anglais &quot;humain&quot; avec un score de confiance de `0.2`. Si vous définissez le score minimum sur `0.3`, vous conservez la traduction &quot;hombre&quot; à &quot;man&quot;, mais vous ignorez la traduction &quot;hombre&quot; à &quot;humain&quot; car ce score de traduction de `0.2` est inférieur au score minimum de `0.3`.

5. Exécution d’une recherche de texte intégral sur des fichiers
   * Etant donné que dam:Asset est le type de noeud que ce module linguistique est enregistré à nouveau, nous devons rechercher AEM Assets à l’aide de la recherche de texte intégral pour valider ce paramètre.
   * Accédez à AEM > Ressources et ouvrez Omnisearch. Recherchez un terme dans la langue dans laquelle le pack de langue a été installé.
   * Si nécessaire, ajustez le score minimum dans les configurations OSGi pour garantir la précision des résultats.

6. Mise à jour des packs de langues
   * Les modules linguistiques Apache Joshua sont entièrement conservés par le projet Apache Joshua, et leur mise à jour ou correction est laissée à la discrétion du projet Apache Joshua.
   * Si un pack de langue est mis à jour, pour installer les mises à jour dans AEM, les étapes 2 à 4 ci-dessus doivent être suivies, en ajustant la taille du tas vers le haut ou vers le bas si nécessaire.

      * Notez que lorsque vous déplacez le pack de langues décompressé vers le dossier crx-quickstart/opt, déplacez tout dossier de pack de langues existant avant de le copier dans le nouveau.
   * Si AEM ne nécessite pas de redémarrage, alors la ou les configurations OSGi du fournisseur Apache Jackrabbit Oak Machien Translation Termes de Requête de traduction complète de Apache Jackrabbit doivent être réenregistrées afin que AEM traite les fichiers mis à jour.


## Mise à jour de l&#39;index damAssetLucene {#updating-damassetlucene-index}

Pour que [AEM Balises dynamiques](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) soit affecté par AEM Traduction dynamique, l&#39;index `/oak   :index  /damAssetLucene` de l&#39;AEM doit être mis à jour afin de marquer les balises prédites (nom système des &quot;Balises dynamiques&quot;) comme faisant partie de l&#39;index Lucene de l&#39;agrégat du fichier.

Sous `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, vérifiez que la configuration est la suivante :

```xml
 <damAssetLucene jcr:primaryType="oak:QueryIndexDefinition">
        <indexRules jcr:primaryType="nt:unstructured">
            <dam:Asset jcr:primaryType="nt:unstructured">
                <properties jcr:primaryType="nt:unstructured">
                    ...
                    <predictedTags
                        jcr:primaryType="nt:unstructured"
                        isRegexp="{Boolean}true"
                        name="jcr:content/metadata/predictedTags/*/name"
                        useInSpellheck="{Boolean}true"
                        useInSuggest="{Boolean}true"
                        analyzed="{Boolean}true"
                        nodeScopeIndex="{Boolean}true"/>
```

## Ressources supplémentaires{#additional-resources}

* [Groupe OSGi de traduction automatique Apache Oak Search](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Balises dynamiques AEM](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Bonnes pratiques relatives aux requêtes et à l’indexation](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)