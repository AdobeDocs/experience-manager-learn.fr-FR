---
title: Utilisation du Rapports de transactions dans AEM Forms
seo-title: Utilisation du Rapports de transactions dans AEM Forms
description: Les rapports de transactions à AEM Forms vous permettent de conserver le décompte de toutes les transactions effectuées depuis une date spécifiée dans votre déploiement AEM Forms.
seo-description: Les rapports de transactions à AEM Forms vous permettent de conserver le décompte de toutes les transactions effectuées depuis une date spécifiée dans votre déploiement AEM Forms.
uuid: e6133f7e-c79c-4006-89e7-3bebf7b8229e
feature: adaptive-forms
topics: developing
audience: administrator
doc-type: article
activity: setup
version: 6.4.1,6.5
discoiquuid: 1abdf07a-b9f0-4c58-a1c6-08ae57db2014
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 2%

---


# Utilisation du Rapports de transactions dans AEM Forms{#using-transaction-reporting-in-aem-forms}

Le rapports des transactions pour capturer le nombre d’envois de formulaires, le rendu de documents utilisant les services de document et le rendu de communications interactives (canaux Web et d’impression) a été introduit avec AEM Forms 6.4.1. Cette fonctionnalité s’adresse principalement aux clients qui souhaitent obtenir une licence du logiciel en fonction du nombre d’envois de formulaires et/ou de documents rendus. Cette fonctionnalité est actuellement disponible sur la pile AEM Forms OSGI uniquement.

## Activation du Rapports de transactions {#enabling-transaction-reporting}

Par défaut, l&#39;enregistrement des transactions est désactivé. Pour activer l&#39;enregistrement des transactions, procédez comme suit :

* [Ouvrez configMgr](http://localhost:4502/system/console/configMgr)
* Rechercher &quot;Forms Transaction Rapports&quot;
* Cochez la case &quot;Enregistrer les transactions&quot;.
* Enregistrez vos modifications

Une fois le rapports de transaction activé, vous pouvez envoyer un Forms adaptatif, générer des documents à l’aide des services de document ou générer des documents de communication interactive pour voir le rapports de transaction en action.

## Affichage de l&#39;état des transactions {#viewing-transaction-report}

Pour vue du rapport de transactions, connectez-vous à AEM Forms en tant qu&#39;administrateur. Seuls les membres du groupe fd-Administrator peuvent vue l&#39;état de transaction.

Sélectionner les outils | Forms | Rapport des transactions de Vue

ou vue du rapport de transaction en cliquant [ici](http://localhost:4502/mnt/overlay/fd/transaction/gui/content/report.html)

![TransmissionReporting](assets/transactionreporting.gif)

Dans la capture d’écran ci-dessus, Document traité indique le nombre de documents générés à l’aide des services de document. Les documents rendus correspondent au nombre de documents de communication interactive (Web et Impression) générés. Forms Submitted indique le nombre d’envois de formulaire adaptatif.

Une transaction reste dans la mémoire tampon pendant une période spécifiée (durée de la mémoire tampon de vidage + temps de réplication inverse). Par défaut, il faut environ 90 secondes pour que le nombre de transactions se reflète dans l&#39;état de la transaction.

Les actions telles que l’envoi d’un formulaire PDF, l’utilisation de l’interface utilisateur de l’agent pour prévisualisation une communication interactive ou l’utilisation de méthodes d’envoi de formulaire non standard ne sont pas comptabilisées comme des transactions. aem forms fournit une API pour enregistrer ces transactions. Appelez l’API depuis vos implémentations personnalisées pour enregistrer une transaction.

Si vous consultez le rapport de transaction sur l’instance d’auteur, assurez-vous que la réplication inversée est configurée sur toutes les instances de publication.

Pour en savoir plus sur le rapports de transactions, [cliquez ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/transaction-reports-overview.html)

