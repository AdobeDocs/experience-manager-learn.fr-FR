---
title: Configurer la recherche de traduction intelligente avec AEM Assets
description: La recherche de traduction intelligente permet d’utiliser des termes de recherche non anglais pour résoudre du contenu en anglais. Pour configurer AEM pour la recherche de traduction intelligente, le lot OSGi Search Machine Translation Apache Oak doit être installé et configuré, ainsi que les modules linguistiques Apache Joshua gratuits et open source pertinents qui contiennent les règles de traduction.
version: 6.4, 6.5
feature: Search
topic: Content Management
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7be8c3d5-b944-4421-97b3-bd5766c1b1b5
duration: 664
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 100%

---

# Configurer la recherche de traduction intelligente avec AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La recherche de traduction intelligente permet d’utiliser des termes de recherche non anglais pour résoudre du contenu en anglais. Pour configurer AEM pour la recherche de traduction intelligente, le lot OSGi Search Machine Translation Apache Oak doit être installé et configuré, ainsi que les modules linguistiques Apache Joshua gratuits et open source pertinents qui contiennent les règles de traduction.

>[!VIDEO](https://video.tv.adobe.com/v/21291?quality=12&learn=on)

>[!NOTE]
>
>La recherche de traduction intelligente doit être configurée sur chaque instance AEM qui la requiert.

1. Téléchargez et installez le lot OSGi Search Machine Translation Oak
   * [Téléchargez le lot OSGi Search Machine Translation Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) qui correspond à la version Oak d’AEM.
   * Installez le lot OSGi Search Machine Translation Oak téléchargé dans AEM via [`/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Télécharger et mettre à jour les modules linguistiques Apache Joshua
   * Téléchargez et décompressez les [modules linguistiques Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) souhaités.
   * Modifiez le fichier `joshua.config` et mettez en commentaire les 2 lignes qui commencent par :

     ```
     feature-function = LanguageModel ...
     ```

   * Déterminez et enregistrez la taille du dossier de modèles du module linguistique, car cela influence la quantité d’espace de tas supplémentaire nécessaire pour AEM.
   * Déplacez le dossier décompressé de module linguistique Apache Joshua (avec les modifications `joshua.config`) vers

     ```
     .../crx-quickstart/opt/<source_language-target_language>
     ```

     Par exemple :

     ```
      .../crx-quickstart/opt/es-en
     ```

3. Redémarrer AEM avec une attribution de mémoire de tas mise à jour
   * Arrêtez AEM
   * Déterminez la nouvelle taille de tas requise pour AEM.

      * La taille du tas d’AEM avant la module linguistique + la taille du répertoire de modèle arrondie à 2 Go.
      * Par exemple : si l’installation AEM des modules linguistiques nécessite 8 Go de tas pour s’exécuter et que le dossier de modèle du module linguistique est de 3,8 Go décompressé, la nouvelle taille du tas est la suivante :

        Les `8GB` originaux + (`3.75GB` arrondi à `2GB`, ce qui donne `4GB`) pour un total de `12GB`.

   * Vérifiez que la machine dispose de cette quantité supplémentaire de mémoire disponible.
   * Mettez à jour les scripts de démarrage d’AEM pour ajuster à la nouvelle taille du tas.

      * Par exemple : `java -Xmx12g -jar cq-author-p4502.jar`.

   * Redémarrez AEM avec la taille de tas augmentée.

   >[!NOTE]
   >
   >L’espace de tas requis pour les modules linguistiques peut s’agrandir, en particulier lorsque plusieurs modules sont utilisés.
   >
   >
   >Assurez-vous toujours que **l’instance dispose de suffisamment de mémoire** pour supporter l’augmentation de l’espace de tas attribué.
   >
   >
   >Le **tas de base doit toujours être calculé pour prendre en charge des performances acceptables sans aucun module linguistique** installé.

4. Enregistrer les modules linguistiques via les configurations OSGi du fournisseur de termes de requête de texte intégral de traduction automatique Apache Jackrabbit Oak

   * Pour chaque module linguistique, [créez une nouvelle configuration OSGi du fournisseur de termes de requête de texte intégral de traduction automatique Apache Jackrabbit Oak](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) via le gestionnaire de configuration de la console web AEM.

      * `Joshua Config Path` est le chemin d’accès absolu au fichier joshua.config. Le processus AEM doit pouvoir lire tous les fichiers du dossier du module linguistique.
      * Les `Node types` sont les types de nœuds candidats dont la recherche de texte intégral engage ce module linguistique pour la traduction.
      * Le `Minimum score` est le score de confiance minimal pour un terme traduit à utiliser.

         * Par exemple, hombre (« homme » en espagnol) peut se traduire par « man » en anglais avec un score de confiance de `0.9` et traduire aussi en anglais le mot « human » (« humain ») avec un score de confiance de `0.2`. Régler le score minimal sur `0.3` garderait la traduction « man» pour « hombre », mais ignorerait la traduction « human » pour « hombre » comme ce score de traduction de `0.2` est inférieur au score minimal de `0.3`.

5. Exécuter une recherche de texte intégral sur des ressources
   * Comme dam:Asset est le type de nœud avec lequel ce module linguistique est enregistré, nous devons rechercher les ressources AEM à l’aide de la recherche de texte intégral pour le valider.
   * Accédez à AEM > Ressources, puis ouvrez Omnisearch. Recherchez un terme dans la langue dans laquelle le module linguistique a été installé.
   * Au besoin, ajustez le score minimal dans les configurations OSGi pour garantir la précision des résultats.

6. Mettre à jour les modules linguistiques
   * Les modules linguistiques Apache Joshua sont entièrement gérés par le projet Apache Joshua et leur mise à jour ou correction est à la discrétion du projet Apache Joshua.
   * Si un module linguistique est mis à jour, pour installer les mises à jour dans AEM, les étapes 2 à 4 ci-dessus doivent être suivies, en ajustant le tas à la bonne taille, selon les besoins.

      * Notez que lorsque vous déplacez le module linguistique décompressé vers le dossier crx-quickstart/opt, déplacez tout dossier existant de module linguistique avant de le copier dans le nouveau.

   * Si AEM ne nécessite pas de redémarrage, les configurations appropriées OSGi du fournisseur de termes de requête de texte intégral de traduction automatique Apache Jackrabbit Oak qui se rapportent aux modules linguistiques mis à jour doivent être réenregistrées afin qu’AEM traite les fichiers mis à jour.

## Mettre à jour l’index damAssetLucene {#updating-damassetlucene-index}

Pour que les [balises intelligentes AEM](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) soient concernées par la traduction dynamique AEM, l’index AEM `/oak   :index  /damAssetLucene` doit être mis à jour afin de marquer predictedTags (le nom système des « balises intelligentes ») comme faisant partie de l’index Lucene agrégé de la ressource.

Sous `/oak:index/damAssetLucene/indexRules/dam:Asset/properties/predicatedTags`, vérifiez que la configuration est la suivante :

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

* [Lot OSGi Search Machine Translation Apache Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Modules linguistiques Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [Balises intelligentes AEM](https://helpx.adobe.com/fr/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Bonnes pratiques relatives aux interrogations et à l’indexation](https://helpx.adobe.com/fr/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)
