---
title: Recherche et suppression des API obsolètes dans AEM as a Cloud Service
description: Découvrez comment rechercher et supprimer des API obsolètes dans AEM as a Cloud Service.
version: Experience Manager as a Cloud Service
role: Developer, Architect
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20288
thumbnail: KT-20288.png
last-substantial-update: 2026-02-09T00:00:00Z
source-git-commit: 6c5b911d1d59573338dd1a30eb95289bc1339f19
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 3%

---


# Recherche et suppression des API obsolètes dans AEM as a Cloud Service

Découvrez comment rechercher et supprimer des API obsolètes dans AEM as a Cloud Service.

## Vue d’ensemble

AEM as a Cloud Service **Centre d’action** vous informe de l’existence d’_API obsolètes_ dans votre projet. Pour obtenir les dernières fonctionnalités, mises à jour de sécurité et déploiements fluides de votre code vers AEM as a Cloud Service à l’aide des pipelines Cloud Manager, supprimez les API obsolètes de votre projet.

Dans ce tutoriel, vous apprendrez à rechercher et à supprimer des API obsolètes dans votre environnement AEM as a Cloud Service à l’aide du plug-in Maven [AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

## Comment trouver des API obsolètes

Pour rechercher des API obsolètes dans votre projet AEM as a Cloud Service, procédez comme suit.

1. **Utiliser le dernier plug-in Maven d’AEM Analyzer**

   Dans votre projet AEM, utilisez la dernière version du plug-in Maven [AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md).

   - Dans la `pom.xml` principale, la version du plug-in est généralement déclarée. Comparez votre version à la dernière [version publiée](https://mvnrepository.com/artifact/com.adobe.aem/aemanalyser-maven-plugin).

     ```xml
     ...
     <aemanalyser.version>1.6.14</aemanalyser.version> <!-- Latest released version as of 09-Feb-2026 -->
     ...
     <!-- AEM Analyser Plugin -->
     <plugin>
         <groupId>com.adobe.aem</groupId>
         <artifactId>aemanalyser-maven-plugin</artifactId>
         <version>${aemanalyser.version}</version>
         <extensions>true</extensions>
     </plugin>
     ...
     ```

   - Le plug-in vérifie la dernière SDK AEM disponible. Utilisez la dernière version d’AEM SDK dans le fichier `pom.xml` de votre projet. Cela permet de faire apparaître les API obsolètes en tant qu’avertissements IDE.

     ```xml
     ...
     <aem.sdk.api>2026.2.24288.20260204T121510Z-260100</aem.sdk.api> <!-- Latest available AEM SDK version as of 09-Feb-2026 -->
     ...
     ```

   - Assurez-vous que le module `all` exécute le module externe dans la phase `verify`.

     ```xml
     ...
     <build>
         <plugins>
             ...
             <plugin>
                 <groupId>com.adobe.aem</groupId>
                 <artifactId>aemanalyser-maven-plugin</artifactId>
                 <extensions>true</extensions>
                 <executions>
                     <execution>
                         <id>analyse-project</id>
                         <phase>verify</phase>
                         <goals>
                             <goal>project-analyse</goal>
                         </goals>
                     </execution>
                 </executions>
             </plugin>
             ...
         </plugins>
     </build>
     ...
     ```

2. **Exécutez une version et recherchez des avertissements**

   Lorsque vous exécutez `mvn clean install`, l’analyseur signale les API obsolètes en tant que messages **[AVERTISSEMENT]** dans la sortie. Par exemple :

   ```shell
   ...
   [WARNING] The analyser found the following warnings for author and publish :
   [WARNING] [region-deprecated-api] com.adobe.aem.guides:aem-guides-wknd.core:4.0.5-SNAPSHOT: Usage of deprecated package found : org.apache.commons.lang : Commons Lang 2 is in maintenance mode. Commons Lang 3 should be used instead. Deprecated since 2021-04-30 For removal : 2021-12-31 (com.adobe.aem.guides:aem-guides-wknd.all:4.0.5-SNAPSHOT)
   ...
   ```

   Il est facile d’ignorer ces messages lorsque vous vous concentrez sur la réussite ou l’échec de la création.

3. **Obtenez une liste claire des API obsolètes**

   L’étape ci-dessus fournit également les mêmes informations. Toutefois, exécutez la phase `verify` sur le module `all` pour afficher tous les messages **[AVERTISSEMENT]** au même endroit. Par exemple :

   ```shell
   $ mvn clean verify -pl all
   ```

   Les messages **[AVERTISSEMENT]** de la sortie de build répertorient les API obsolètes de votre projet.

## Comment supprimer des API obsolètes

L’analyseur d’AEM signale **quoi** est obsolète et fournit la **recommandation** sur la manière de corriger le problème. Toutefois, utilisez le tableau ci-dessous pour choisir l’action appropriée et suivez la documentation associée si vous avez besoin de plus de détails.

### Stratégie de remédiation d’API obsolète

| Type d’avertissement de l’analyseur | Ce qu’il indique | Action recommandée | Référence |
| --------------------- | ----------------- | ------------------ | --------- |
| API AEM obsolète | API à supprimer d’AEM as a Cloud Service | Remplacer l’utilisation par l’API publique prise en charge | [Guide de suppression des API](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Package ou classe AEM obsolète | Le package ou la classe n’est plus pris en charge | Refactoriser le code pour utiliser l’alternative recommandée | [&#x200B; API obsolètes &#x200B;](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#aem-apis) |
| Bibliothèque tierce obsolète | La bibliothèque ne sera pas prise en charge dans les futurs SDK | Mise à niveau de la dépendance et utilisation de la refactorisation | [Directives générales](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Modèles Sling/OSGi obsolètes | Annotations ou API héritées détectées | Migration vers les API Sling et OSGi modernes | [Suppression des modèles Sling/OSGi](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance) |
| Suppression prévue (date ultérieure) | L’API fonctionne toujours, mais la suppression est appliquée ultérieurement | Planifier le nettoyage avant l’application du pipeline | [Notes de mise à jour](https://experienceleague.adobe.com/fr/docs/experience-manager-cloud-service/content/release-notes/home) |

### Conseils pratiques

- Traiter les avertissements de l’analyseur comme **futurs échecs de pipeline**, et non comme des messages facultatifs.
- Corrigez localement les API obsolètes à l’aide de la **dernière version d’AEM SDK**.
- Gardez la sortie de l’analyseur propre pour éviter des problèmes lors des futures mises à niveau d’AEM.

Une correction précoce des API obsolètes permet de conserver votre projet **sûr pour la mise à niveau et prêt pour le déploiement**.

## Ressources supplémentaires

- [Plug-In Maven AEM Analyzer](https://github.com/adobe/aemanalyser-maven-plugin/blob/main/aemanalyser-maven-plugin/README.md)
- [Fonctionnalités et API obsolètes et supprimées](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/deprecated-removed-features#api-removal-guidance)

