---
title: Conseils et astuces sur la maintenance du site
description: Conseils et astuces sur la maintenance du site
hide: true
hidefromtoc: true
source-git-commit: 3eb429039589ae26a81bc6d24f020a77517133e8
workflow-type: tm+mt
source-wordcount: '1070'
ht-degree: 8%

---


# Conseils et astuces sur la maintenance du site

Il existe trois options pour installer et gérer une instance AEM

* AEMaaCS (service cloud) : le système est toujours activé, à jour et s’adapte dynamiquement selon vos besoins.
* Adobe Managed Services où les ingénieurs du service client Adobe effectuent toutes les opérations de maintenance quotidienne, hebdomadaire et mensuelle, et s’assurent que tous les Service Packs sont installés et que le système est toujours sécurisé et s’exécute correctement
* l’exécuter sur site, où vous devez prendre en charge l’ensemble du système, y compris les sauvegardes, les mises à niveau et la sécurité.

Si vous choisissez de mettre en oeuvre votre propre système sur site, vous devez tenir compte de quelques points pour vous assurer que vous disposez d’un système performant et sécurisé. Outre les éléments &quot;Assistance et alimentation&quot;, cet article mentionnera également plusieurs éléments que AEM les développeurs devraient garder à l’esprit pour aider à maintenir le bon fonctionnement du système.

## Administrateurs

Sauvegardes : assurez-vous d’effectuer des sauvegardes complètes et/ou partielles à intervalles réguliers :

* chaque jour
* chaque semaine
* mensuel

De nombreux clients effectuent des sauvegardes instantanées, qui ne prennent que quelques minutes en supposant que le système d’exploitation sous-jacent prenne en charge ces sauvegardes. Assurez-vous que ces sauvegardes sont correctement stockées (hors du système AEM). Assurez-vous que les sauvegardes sont fonctionnelles et qu’elles peuvent être utilisées pour recréer un système de travail périodiquement. Il n’y a rien de pire que d’avoir un plantage du système et vos sauvegardes sont endommagées pour une raison quelconque !

Vous devez surveiller plusieurs éléments pour garantir un fonctionnement sans problème :

### Maintenance courante

#### [maintenance d&#39;index](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=fr)

Les index permettent aux requêtes de s’exécuter aussi rapidement que possible, libérant ainsi des ressources pour d’autres opérations. Assurez-vous que vos index sont en forme de pointe ! AEM annule les requêtes qui se déplacent au lieu d’utiliser un index pour empêcher une mauvaise requête d’affecter les performances globales d’AEM.

#### [Compression tar/nettoyage de révision](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=en)

Chaque mise à jour du référentiel crée une nouvelle révision de contenu. Par conséquent, à chaque mise à jour, la taille du référentiel augmente. Pour éviter une croissance incontrôlée du référentiel, les anciennes révisions doivent être nettoyées pour libérer des ressources de disque.

#### [Nettoyage des binaires Lucene](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-dashboard.html?lang=en#automated-maintenance-tasks)

Purge des fichiers binaires Lucene et réduction de la taille d’entrepôt de données en cours d’exécution.

#### [Nettoyage de l’entrepôt de données](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/data-store-garbage-collection.html?lang=en)

Lorsqu’une ressource dans AEM est supprimée, la référence à l’enregistrement de l’entrepôt de données sous-jacent peut être supprimée de la hiérarchie des noeuds, mais l’enregistrement de l’entrepôt de données lui-même reste. Cet enregistrement d’entrepôt de données non référencé devient de la &quot;mémoire&quot; qui ne doit pas être conservée. Dans les cas où plusieurs ressources non référencées existent, il est préférable de les supprimer, de préserver de l’espace, d’optimiser la sauvegarde et les performances de maintenance du système de fichiers.

#### [Purge du workflow](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/workflows-administering.html?lang=en)

Réduire le nombre d’instances de workflow améliore les performances du moteur de workflows. Vous pouvez donc purger régulièrement les instances de workflow terminées ou en cours d’exécution du référentiel.

#### [Maintenance des journaux d’audit](https://experienceleague.adobe.com/docs/experience-manager-64/administering/operations/operations-audit-log.html?lang=en)

Les événements AEM qui remplissent les critères de la journalisation d’audit génèrent de nombreuses données archivées. Ces données peuvent rapidement s’agrandir au fil du temps en raison des réplications, des téléchargements de ressources et autres activités du système.

#### [Sécurité](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=fr)

Assurez-vous que les bonnes pratiques relatives à la liste de contrôle de sécurité sont suivies de près pour garantir l’instance d’AEM la plus sécurisée.

#### Diskspace

Surveillez l’espace disque pour vous assurer que vous disposez de suffisamment pour le référentiel JCR, plus environ la moitié de plus d’espace - la compression tar utilise de l’espace supplémentaire lors de son exécution. L&#39;absence d&#39;espace de disque est la raison numéro un de la corruption JCR !

## Développeur

Essayez de ne pas utiliser de composants personnalisés - utilisez [composants principaux](https://www.aemcomponents.dev/). Votre objectif doit être d’utiliser les composants principaux 80 à 90 % du temps et les composants personnalisés uniquement avec modération. Cela nécessite souvent une nouvelle manière d’examiner les composants sur une page. Vous devez vous rendre compte que les composants peuvent être facilement reconfigurés par un développeur front-end à l’aide de CSS. Gardez également à l’esprit que ces composants principaux peuvent être incorporés les uns dans les autres pour obtenir des résultats assez complexes. Soyez créatif !

### [Systèmes de style](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=en)

Les systèmes de style permettent aux composants principaux, et même aux composants personnalisés, de changer d’aspect et d’aspect à la discrétion des auteurs pour créer des composants entièrement nouveaux. Ces modifications stylistiques impliquent généralement uniquement un concepteur front-end et un auteur expérimenté (souvent appelé &quot;super-auteur&quot;).

### [Lancements](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=en)

Les lancements permettent de terminer les travaux pour un nouveau déploiement de promotion, de vente ou de site web sans affecter les pages actuellement déployées. En outre, ils peuvent être programmés pour être mis en ligne automatiquement, sans présence ni supervision, ce qui permet aux auteurs de faire le travail de la semaine prochaine (ou du trimestre prochain) aujourd&#39;hui et de ne pas se précipiter dans le développement de pages la veille de sa mise en ligne - c&#39;est vraiment le cadeau de TIME !)

### [Fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-64/assets/fragments/content-fragments.html?lang=en)

Les fragments de contenu sont des &quot;blocs&quot; d’informations personnalisables qui peuvent être facilement réutilisés sur tout le site. Si vous avez besoin d’une modification, vous modifiez simplement le bloc d’origine et la mise à jour est visible partout où elle est utilisée - immédiatement !

### [Fragments d’expérience](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=en)

Bien que les fragments de contenu semblent presque identiques, les fragments d’expérience sont de petits éléments visibles d’une page. Elles peuvent également être largement réutilisées sur l’ensemble de votre site et gérées dans un emplacement central au sein d’AEM afin de faciliter la tâche consistant à apporter des modifications potentiellement globales sur votre site en quelques secondes, et non en plusieurs jours ou semaines.

Pensez-y et voyez ce qui pourrait être réutilisé. Un pied de page ? Une clause exonératoire de responsabilité ? Un en-tête ? Certains types de contenu ? Tous ces éléments peuvent être partagés sur l’ensemble d’un site tout en conservant la maintenance au minimum. Vous devez mettre à jour une date dans une clause de non-responsabilité, mais elle se trouve sur 1 000 pages de votre site ? Si vous avez utilisé un fragment d’expérience, il s’agit d’une opération de 5 secondes.

## Général

Tenez-vous au courant des changements AEM par l&#39;apprentissage continu - ne vous retrouvez pas coincés dans le passé. Utilisation [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=en) et [Services d’apprentissage numérique Adobe (ADLS)](https://learning.adobe.com/) pour perfectionner vos compétences.

## Conclusion

AEM peut être un grand système, et il faut beaucoup de gens pour le faire &quot;chanter&quot;. Des administrateurs aux développeurs (développeurs front-end et noyau dur de Java) en passant par les auteurs, il y a quelque chose pour tout le monde ! Et si vous n&#39;avez pas envie de gérer l&#39;administration au jour le jour, il y a toujours AMS et AEM as a Cloud Service.
