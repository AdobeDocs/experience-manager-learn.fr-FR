---
title: Utilisation des rapports de transaction dans AEM Forms
seo-title: Utilisation des rapports de transaction dans AEM Forms
description: Les rapports de transaction dans AEM Forms vous permettent de conserver un décompte de toutes les transactions effectuées depuis une date spécifiée sur votre déploiement AEM Forms.
seo-description: Les rapports de transaction dans AEM Forms vous permettent de conserver un décompte de toutes les transactions effectuées depuis une date spécifiée sur votre déploiement AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: Formulaires adaptatifs
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '401'
ht-degree: 2%

---


# Utilisation des rapports de transaction dans AEM Forms{#using-transaction-reporting-in-aem-forms}

La création de rapports de transaction pour capturer le nombre d’envois de formulaire, le rendu des documents à l’aide de Document Services et le rendu des communications interactives (canaux Web et d’impression) a été introduite avec AEM Forms 6.4.1. Cette fonctionnalité est principalement destinée aux clients qui souhaitent acquérir sous licence le logiciel en fonction du nombre d’envois de formulaire et/ou de documents rendus. Cette fonctionnalité est actuellement disponible sur la pile AEM Forms OSGI uniquement.

## Activation des rapports de transaction {#enabling-transaction-reporting}

Par défaut, l’enregistrement des transactions est désactivé. Pour activer l’enregistrement des transactions, procédez comme suit :

* [Ouvrez configMgr](http://localhost:4502/system/console/configMgr)
* Recherche de &quot;rapports de transaction Forms&quot;
* Cochez la case &quot;Enregistrer les transactions&quot;.
* Enregistrez vos modifications

Une fois que la création de rapports de transaction est activée, vous pouvez envoyer Adaptive Forms, générer des documents à l’aide de Document Services ou générer des documents de communication interactive pour voir la création de rapports de transaction en action.

## Affichage du rapport des transactions {#viewing-transaction-report}

Pour afficher le rapport de transaction, connectez-vous à AEM Forms en tant qu’administrateur. Seuls les membres du groupe fd-Administrator peuvent afficher le rapport de transaction.

Sélectionner les outils | Forms | Afficher le rapport de transaction

ou afficher le rapport de transaction en cliquant [ici](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransmissionReporting](assets/transactionreporting.gif)

Dans la capture d’écran ci-dessus, le nombre de documents générés à l’aide de Document Services est indiqué. Documents rendus est le nombre de documents de communication interactive (Web et Impression) rendus. Forms envoyée est le nombre d’envois de formulaire adaptatif.

Une transaction reste dans la mémoire tampon pendant une période spécifiée (durée de la mémoire tampon de vidage + temps de réplication inverse). Par défaut, le comptage des transactions prend environ 90 secondes dans le rapport de transaction.

Les actions telles que l’envoi d’un formulaire PDF, l’utilisation de l’interface utilisateur de l’agent pour prévisualiser une communication interactive ou l’utilisation de méthodes d’envoi de formulaire non standard ne sont pas comptabilisées comme des transactions. AEM Forms fournit une API pour enregistrer ces transactions. Appelez l’API à partir de vos implémentations personnalisées pour enregistrer une transaction.

Si vous affichez le rapport de transaction sur l’instance d’auteur, assurez-vous que la réplication inverse est configurée sur toutes les instances de publication.

Pour en savoir plus sur les rapports de transaction [veuillez cliquer ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

