---
title: Votre guide de maintenance régulière du site
description: Que votre fonction soit d’administrer, de créer ou de développer, la maintenance du site touche tous les aspects de votre instance AEM Sites. Utilisez ce guide pour vous assurer que votre stratégie est bien configurée.
role: Admin
level: Beginner, Intermediate
topic: Administration
audience: author, marketer, developer
feature: Learn From Your Peers
exl-id: 37ee3234-f91c-4f0a-b0b7-b9167e7847a9
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: ht
source-wordcount: '1084'
ht-degree: 100%

---

# Conseils et astuces sur la maintenance du site

Il existe trois options pour installer et effectuer la maintenance d’une instance AEM.

* AEMaaCS (service cloud) : le système est toujours activé, à jour et s’adapte dynamiquement à vos besoins.
* Adobe Managed Services où les personnes chargées de l’ingénierie du service client Adobe effectuent toutes les opérations de maintenance quotidienne, hebdomadaire et mensuelle, et s’assurent que tous les packs de services sont installés et que le système fonctionne toujours correctement et en toute sécurité.
* Ou l’exécuter sur site, où vous devez prendre en charge l’ensemble du système, y compris les sauvegardes, les mises à niveau et la sécurité.

Si vous choisissez d’implémenter votre propre système sur site, vous devez tenir compte de quelques points pour vous assurer que vous disposez d’un système performant et sécurisé. Outre les éléments « Assistance et alimentation », cet article mentionnera également plusieurs éléments que les développeurs et développeuses AEM devraient garder à l’esprit pour aider à maintenir le bon fonctionnement du système.

## Administrateurs et administratrices

Sauvegardes : assurez-vous d’effectuer des sauvegardes complètes et/ou partielles à intervalles réguliers :

* Quotidien
* Hebdomadaire
* chaque mois

De nombreux clientes et clients effectuent des sauvegardes instantanées, qui ne prennent que quelques minutes en supposant que le système d’exploitation sous-jacent prenne en charge ces sauvegardes. Assurez-vous que ces sauvegardes sont correctement stockées (hors du système AEM). Assurez-vous que les sauvegardes sont fonctionnelles et qu’elles peuvent être utilisées pour recréer un système de travail périodiquement. Il n’y a rien de pire qu’un système qui tombe en panne et que vos sauvegardes soient endommagées pour une raison quelconque.

Vous devez surveiller plusieurs éléments pour garantir un fonctionnement sans problème :

### Maintenance régulière

#### [Maintenance d’index](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/practices/best-practices-for-queries-and-indexing.html?lang=fr)

Les index permettent aux requêtes de s’exécuter aussi rapidement que possible, libérant ainsi des ressources pour d’autres opérations. Assurez-vous que vos index sont en parfait état. AEM annule les requêtes qui se déplacent au lieu d’utiliser un index pour empêcher une mauvaise requête d’affecter les performances globales d’AEM.

#### [Nettoyage des révisions/compressions TAR](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/revision-cleanup.html?lang=fr)

Chaque mise à jour du référentiel crée une révision du contenu. Par conséquent, avec chaque mise à jour, la taille du référentiel augmente. Pour éviter une croissance incontrôlée du référentiel, les anciennes révisions doivent être nettoyées pour libérer des ressources de disque.

#### [Nettoyage des fichiers binaires Lucene](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-dashboard.html?lang=fr#automated-maintenance-tasks)

Purgez les fichiers binaires Lucene et réduisez la taille du magasin de données en cours d’exécution.

#### [Nettoyage du magasin de données](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/data-store-garbage-collection.html?lang=fr)

Lorsqu’une ressource dans AEM est supprimée, la référence à l’enregistrement de magasin de données sous-jacent peut être supprimée de la hiérarchie de nœud, mais l’enregistrement de magasin de données lui-même est conservé. Cet enregistrement de magasin de données non référencé est alors considéré comme faisant partie des « données à nettoyer » qu’il n’est pas utile de conserver. S’il existe plusieurs ressources non référencées, il est préférable de les supprimer, de préserver de l’espace, d’optimiser la sauvegarde et les performances de maintenance du système de fichiers.

#### [Purge du workflow](https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/workflows-administering.html?lang=fr)

Réduire le nombre d’instances de workflow améliore les performances du moteur de workflows. Vous pouvez donc purger régulièrement les instances de workflow terminées ou en cours d’exécution du référentiel.

#### [Maintenance du journal d’audit] (https://experienceleague.adobe.com/docs/experience-manager-65/administering/operations/operations-audit-log.html?lang=fr)

Les événements AEM pouvant être inclus dans la journalisation d’audit génèrent une grande quantité de données archivées. Ces données peuvent rapidement augmenter au fil du temps en raison de réplications, de chargements de ressources et d’autres activités du système.

#### [Sécurité](https://experienceleague.adobe.com/docs/experience-manager-65/administering/security/security-checklist.html?lang=fr)

Assurez-vous que les bonnes pratiques relatives à la liste de contrôle de sécurité sont suivies de près pour garantir l’instance d’AEM la plus sécurisée.

#### Espace disque

Surveillez l’espace disque pour vous assurer qu’il est suffisant pour le référentiel JCR, plus environ la moitié de cet espace, car la compression TAR utilise de l’espace supplémentaire lors de son exécution. L’absence d’espace de disque est la principale raison de la corruption du JCR.

## Développement

Essayez de ne pas utiliser de composants personnalisés. Utilisez plutôt les [composants principaux](https://www.aemcomponents.dev/). Votre objectif doit être d’utiliser les composants principaux 80 à 90 % du temps et les composants personnalisés uniquement avec modération. Vous devrez sans doute changer votre méthode d’examiner les composants sur une page. Ces derniers peuvent en effet être facilement reconfigurés par un développeur ou une développeuse front-end à l’aide de CSS. Gardez également à l’esprit que ces composants principaux peuvent être incorporés les uns dans les autres pour obtenir des résultats assez complexes. Faites appel à votre créativité.

### [Systèmes de style](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/siteandpage/style-system.html?lang=fr)

Les systèmes de style permettent aux composants principaux, et même aux composants personnalisés, de changer d’aspect à la discrétion des auteurs et autrices, qui peuvent ainsi créer des composants entièrement nouveaux. Ces modifications stylistiques impliquent généralement uniquement une personne affectée au design frontal et une personne chargée de la création expérimentée (souvent qualifiée de « super autrice »).

### [Lancements](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/launches/overview.html?lang=fr)

Les lancements permettent de terminer les travaux des nouveaux déploiements de promotion, de vente ou de site web sans affecter les pages actuellement déployées. En outre, ils peuvent être programmés pour être mis en ligne automatiquement, sans présence ni supervision, ce qui permet aux auteurs et autrices de se consacrer au travail de la semaine prochaine (ou du trimestre prochain) dès aujourd’hui et de ne pas se précipiter dans le développement de pages la veille de leur mise en ligne. Un véritable aubaine en matière de gestion du temps.

### [Fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-65/assets/content-fragments/content-fragments.html?lang=fr)

Les fragments de contenu sont des « blocs » d’informations personnalisables qui peuvent être facilement réutilisés sur tout le site. Pour effectuer une modification, modifiez simplement le bloc d’origine et la mise à jour s’affiche immédiatement partout où elle est utilisée.

### [Fragments d’expérience](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use.html?lang=fr)

Bien qu’ils semblent presque identiques aux fragments de contenu, les fragments d’expérience sont de petits éléments visibles d’une page. Ils peuvent également être largement réutilisés sur l’ensemble de votre site et gérés à partir d’un emplacement central au sein d’AEM, afin de faciliter la tâche consistant à apporter des modifications potentiellement globales sur votre site en quelques secondes, et non en plusieurs jours ou semaines.

Pensez-y et réfléchissez à ce qui pourrait être réutilisé. Un pied de page ? Une clause de non-responsabilité ? Un en-tête ? Certains types de contenu ? Tous ces éléments peuvent être partagés sur l’ensemble d’un site tout en assurant une maintenance minimale. Vous devez mettre à jour une date dans une clause de non-responsabilité, mais elle apparaît sur 1 000 pages de votre site ? Si vous avez utilisé un fragment d’expérience, cela ne vous prendra que 5 secondes.

## Général

Tenez-vous au courant des changements d’AEM par le biais de la formation continue et restez toujours à la page. Utilisez [Experience League](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/overview.html?lang=fr) et [ADLS (Adobe Digital Learning Services) ](https://learning.adobe.com/) pour perfectionner vos compétences.

## Conclusion

AEM est un système de grande ampleur, qui nécessite de nombreuses personnes pour le faire fonctionner de manière optimale. De l’administration au développement (front-end et Java pur et dur) en passant par la création de contenu, tout le monde peut y trouver son compte. Et si vous n’avez pas envie de gérer l’administration au jour le jour, vous pouvez toujours avoir recours à AMS et AEM as a Cloud Service.
