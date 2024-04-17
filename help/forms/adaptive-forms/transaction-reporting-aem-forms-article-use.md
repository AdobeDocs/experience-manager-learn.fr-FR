---
title: Utiliser les rapports de transaction dans AEM Forms
description: Les rapports de transaction dans AEM Forms vous permettent de tenir le compte de toutes les transactions effectuées depuis une date spécifiée sur votre déploiement AEM Forms.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 36c38cb6-6f6a-4328-abf5-7a30059b66ce
last-substantial-update: 2019-03-20T00:00:00Z
duration: 81
source-git-commit: 0f853040911ad92aea0ebac57011e659d7f0b7c0
workflow-type: ht
source-wordcount: '343'
ht-degree: 100%

---

# Utiliser les rapports de transaction dans AEM Forms{#using-transaction-reporting-in-aem-forms}

La création de rapports de transaction pour capturer le nombre d’envois de formulaires, le rendu des documents à l’aide des services de document et le rendu des communications interactives (canaux web et d’impression) ont été introduits avec AEM Forms 6.4.1. Cette fonctionnalité est actuellement disponible avec AEM Forms 6.4.1 pour la pile OSGi et 6.5.20 pour la pile JEE.

## Activer les rapports de transaction {#enabling-transaction-reporting}

Par défaut, l’enregistrement des transactions est désactivé. Pour activer l’enregistrement des transactions, procédez comme suit :

* [Ouvrez configMgr.](http://localhost:4502/system/console/configMgr)
* Recherchez « rapports de transaction Forms ».
* Cochez la case « Enregistrer les transactions ».
* Enregistrez vos modifications.

Une fois que la création de rapports de transaction est activée, vous pouvez envoyer des formulaires adaptatifs, générer des documents à l’aide des services de document ou générer des documents de communication interactive pour voir la création de rapports de transaction en action.

## Affichage du rapport de transaction {#viewing-transaction-report}

Pour afficher le rapport de transaction, connectez-vous à AEM Forms en tant qu’administrateur ou administratrice. Seules les personnes membres du groupe fd-Administrator peuvent voir les rapports de transaction.

Sélectionnez Outils | Forms | Afficher le rapport de transaction

ou affichez le rapport de transaction en cliquant [ici](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html).

![TransctionReporting](assets/transactionreporting.gif)

Dans la capture d’écran ci-dessus, Documents traités correspond au nombre de documents générés à l’aide des services de document. Documents rendus correspond au nombre de documents de communication interactive (web et d’impression) rendus. Formulaires envoyés correspond au nombre d’envois de formulaire adaptatif.

Une transaction reste dans la mémoire tampon pendant une période spécifiée (durée de la mémoire tampon de purge + temps de réplication inverse). Par défaut, le comptage des transactions prend environ 90 secondes pour figurer dans le rapport de transaction.

Les actions telles que l’envoi d’un formulaire de PDF, l’utilisation de l’interface utilisateur de l’agent pour prévisualiser une communication interactive ou l’utilisation de méthodes d’envoi de formulaire non standard ne sont pas comptabilisées comme des transactions. AEM Forms fournit une API pour enregistrer ces transactions. Appelez l’API à partir de vos implémentations personnalisées pour enregistrer une transaction.

Si vous affichez le rapport de transaction sur l’instance de création, assurez-vous que la réplication inverse est configurée sur toutes les instances de publication.

Pour en savoir plus sur les rapports de transaction, [cliquez ici](https://helpx.adobe.com/fr/experience-manager/6-4/forms/using/transaction-reports-overview.html).
