---
title: Résolution des problèmes liés à la signature de plusieurs documents
description: Tester et résoudre les problèmes liés à la solution
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Développement
role: Developer
level: Intermediate
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '392'
ht-degree: 1%

---


# Test et dépannage


## Aperçu du formulaire de refinancement

Le cas d’utilisation est déclenché lorsque l’agent du service client remplit et envoie [le formulaire de refinancement](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Le processus Sign Multiple Forms obtient des déclencheurs lors de l’envoi de ce formulaire et le client reçoit une notification par courrier électronique avec un lien pour démarrer le processus de remplissage et de signature du formulaire.

## Remplissage des formulaires dans le module

Le client se voit alors présenter pour remplir et signer le premier formulaire du kit. Une fois la signature du formulaire effectuée, le client peut accéder au formulaire suivant dans le module. Une fois tous les formulaires remplis et signés, le client reçoit le formulaire &quot;**AllDone**&quot;.

## Résolution des problèmes

### La notification par e-mail n&#39;est pas générée

La notification par courrier électronique est envoyée par le composant Envoyer un courrier électronique dans le processus Signer plusieurs formulaires . Si l’une des étapes de ce workflow échoue, l’email de notification est envoyé. Assurez-vous que l’étape de processus personnalisée du workflow crée des lignes dans votre base de données MySQL. Si les lignes sont en cours de création, vérifiez les paramètres de configuration du service de messagerie Day CQ.

### Le lien de la notification par e-mail ne fonctionne pas

Les liens contenus dans les notifications électroniques sont générés dynamiquement. Si votre serveur AEM n’est pas exécuté sur localhost:4502, indiquez le nom et le port corrects du serveur dans les arguments de l’étape Stocker Forms à signer du processus Sign Multiple Forms .

### Impossible de signer le formulaire

Cela peut se produire si le formulaire n’a pas été rempli correctement en récupérant les données de la source de données. Vérifiez les journaux stdout de votre serveur. Le fichier fetchformdata.jsp écrit quelques messages utiles au fichier stdout.

### Impossible d’accéder au formulaire suivant dans le module

Une fois la signature d’un formulaire dans le module terminée, le workflow Mettre à jour le statut de la signature est déclenché. La première étape du workflow met à jour l’état de signature du formulaire dans la base de données. Vérifiez si l’état du formulaire est mis à jour de 0 à 1.

### Impossible de voir le formulaire AllDone

Lorsqu’il n’y a plus de formulaires à signer dans le package, le formulaire AllDone est présenté à l’utilisateur. Si vous ne voyez pas le formulaire AllDone, veuillez vérifier l’URL utilisée à la ligne 33 du fichier GetNextFormToSign.js qui fait partie de la bibliothèque cliente **getnextform**.











