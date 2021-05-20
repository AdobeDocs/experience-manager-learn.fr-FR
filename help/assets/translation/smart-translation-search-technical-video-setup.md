---
title: Configuration de la recherche de traduction dynamique avec AEM Assets
description: La recherche de traduction intelligente permet d’utiliser des termes de recherche non anglais pour résoudre du contenu en anglais. Pour configurer AEM pour la recherche de traduction intelligente, le lot OSGi de traduction automatique de recherche Apache Oak doit être installé et configuré, ainsi que les modules de langue Apache Joshua (logiciels libres et open source) pertinents qui contiennent les règles de traduction.
version: 6.3, 6.4, 6.5
feature: Rechercher
topic: Gestion de contenu
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 1%

---


# Configuration de la recherche de traduction dynamique avec AEM Assets{#set-up-smart-translation-search-with-aem-assets}

La recherche de traduction intelligente permet d’utiliser des termes de recherche non anglais pour résoudre du contenu en anglais. Pour configurer AEM pour la recherche de traduction intelligente, le lot OSGi de traduction automatique de recherche Apache Oak doit être installé et configuré, ainsi que les modules de langue Apache Joshua (logiciels libres et open source) pertinents qui contiennent les règles de traduction.

>[!VIDEO](https://video.tv.adobe.com/v/21291/?quality=9&learn=on)

>[!NOTE]
>
>La recherche de traduction intelligente doit être configurée sur chaque instance AEM qui la requiert.

1. Téléchargez et installez le lot OSGi Oak Search Machine Translation
   * [Téléchargez le ](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22) lot OSGi de traduction automatique de recherche Oak qui correspond à AEM version Oak.
   * Installez le lot OSGi de traduction automatique de recherche Oak téléchargé dans AEM via [ `/system/console/bundles`](http://localhost:4502/system/console/bundles).

2. Télécharger et mettre à jour les modules de langue Apache Joshua
   * Téléchargez et décompressez les [packs de langue Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs) souhaités.
   * Modifiez le fichier `joshua.config` et commentez les 2 lignes qui commencent par :

      ```
      feature-function = LanguageModel ...
      ```

   * Déterminez et enregistrez la taille du dossier de modèles du module de langue, car cela influence l’AEM d’espace de tas supplémentaire nécessaire.
   * Déplacez le dossier Apache Joshua language pack décompressé (avec les `joshua.config` modifications) vers

      ```
      .../crx-quickstart/opt/<source_language-target_language>
      ```

      Par exemple :

      ```
       .../crx-quickstart/opt/es-en
      ```

3. Redémarrez l’AEM avec une allocation de mémoire de tas mise à jour.
   * Arrêtez AEM
   * Déterminer la nouvelle taille de tas requise pour AEM

      * AEM la taille du tas avant la langue + la taille du répertoire de modèle arrondie à 2 Go
      * Par exemple : Si l’installation des packs de langue d’AEM nécessite 8 Go de tas pour être exécutée et que le dossier de modèle du pack de langue est de 3,8 Go décompressé, la nouvelle taille du tas est la suivante :

         Le `8GB` + ( `3.75GB` arrondi au `2GB` le plus proche, c’est-à-dire `4GB`) pour un total de `12GB`
   * Vérifiez que la machine dispose de cette quantité supplémentaire de mémoire disponible.
   * Mettre à jour AEM scripts de démarrage pour ajuster la nouvelle taille du tas

      * Ex. `java -Xmx12g -jar cq-author-p4502.jar`
   * Redémarrez l’AEM avec la taille de tas augmentée.

   >[!NOTE]
   >
   >L’espace de tas requis pour les packs de langue peut s’agrandir, en particulier lorsque plusieurs packs de langue sont utilisés.
   >
   >
   >Veillez toujours à ce que **l’instance dispose de suffisamment de mémoire** pour s’adapter à l’augmentation de l’espace de tas alloué.
   >
   >
   >Le **segment de base doit toujours être calculé pour prendre en charge des performances acceptables sans aucun module de langue** installé.

4. Enregistrez les modules de langue via les configurations OSGi du fournisseur de termes de requête en texte intégral de traduction automatique Apache Jackrabbit Oak

   * Pour chaque module de langue, [créez une configuration OSGi Apache Jackrabbit Oak Machine Translation Full-text Query Terms Provider ](http://localhost:4502/system/console/configMgr/org.apache.jackrabbit.oak.plugins.index.mt.MTFulltextQueryTermsProviderFactory) via le gestionnaire de configuration de la console web d’AEM.

      * `Joshua Config Path` est le chemin d’accès absolu au fichier joshua.config . Le processus AEM doit pouvoir lire tous les fichiers du dossier du module de langue.
      * `Node types` sont les types de noeuds candidats dont la recherche de texte intégral engage ce module de langue pour traduction.
      * `Minimum score` est le score de confiance minimal pour un terme traduit à utiliser.

         * Par exemple, hombre (en espagnol pour &quot;man&quot;) peut se traduire par le mot anglais &quot;man&quot; avec un score de confiance de `0.9` et se traduire également par le mot anglais &quot;humain&quot; avec un score de confiance `0.2`. En définissant le score minimum sur `0.3`, vous conservez la traduction &quot;hombre&quot; à &quot;man&quot;, mais ignorez la traduction &quot;hombre&quot; à &quot;humain&quot;, car ce score de traduction de `0.2` est inférieur au score minimum de `0.3`.

5. Exécution d’une recherche de texte intégral sur des ressources
   * Comme dam:Asset est le type de noeud que ce pack de langue est enregistré à nouveau, nous devons rechercher AEM Assets à l’aide de la recherche de texte intégral pour le valider.
   * Accédez à AEM > Ressources et ouvrez Omni-recherche. Recherchez un terme dans la langue dans laquelle le pack de langue a été installé.
   * Au besoin, ajustez le score minimum dans les configurations OSGi pour garantir la précision des résultats.

6. Mise à jour des packs de langue
   * Les packs de langues Apache Joshua sont entièrement conservés par le projet Apache Joshua, et leur mise à jour ou correction est la discrétion du projet Apache Joshua.
   * Si un module de langue est mis à jour, pour installer les mises à jour dans AEM, les étapes 2 à 4 ci-dessus doivent être suivies, en ajustant la taille du tas vers le haut ou vers le bas, selon les besoins.

      * Notez que lorsque vous déplacez le module de langue décompressé vers le dossier crx-quickstart/opt, déplacez tout dossier existant de module de langue avant de le copier dans le nouveau.
   * Si AEM ne nécessite pas de redémarrage, la configuration OSGi appropriée Apache Jackrabbit Oak Machien Translation Fulltext Query Terms Provider pour le(s) pack(s) de langue mis à jour doit être réenregistrée afin AEM traiter les fichiers mis à jour.


## Mise à jour de l’index damAssetLucene {#updating-damassetlucene-index}

Pour que [AEM les balises intelligentes](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html) soient affectées par AEM traduction dynamique, l’index `/oak   :index  /damAssetLucene` d’AEM doit être mis à jour afin de marquer les balises prédites (nom système des &quot;balises intelligentes&quot;) comme partie de l’index Lucene agrégé de la ressource.

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

* [Groupe OSGi de traduction automatique de recherche Apache Oak](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.apache.jackrabbit%22%20AND%20a%3A%22oak-search-mt%22)
* [Packs de langues Apache Joshua](https://cwiki.apache.org/confluence/display/JOSHUA/Language+Packs)
* [AEM balises intelligentes](https://helpx.adobe.com/experience-manager/6-3/assets/using/touch-ui-smart-tags.html)
* [Bonnes pratiques relatives aux requêtes et à l’indexation](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/best-practices-for-queries-and-indexing.html)