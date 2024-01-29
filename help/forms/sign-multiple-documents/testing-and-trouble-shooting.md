---
title: Dépanner les problèmes liés à la signature de plusieurs documents
description: Tester et dépanner les problèmes liés à la solution
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 99cba29e-4ae3-4160-a4c7-a5b6579618c0
duration: 98
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '382'
ht-degree: 100%

---

# Tester et dépanner


## Prévisualiser le formulaire de refinancement

Le cas d’utilisation est déclenché lorsque le service clientèle remplit et envoie le [formulaire de refinancement](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Le processus Signer plusieurs formulaires obtient des déclencheurs lors de l’envoi de ce formulaire. La personne reçoit alors une notification par e-mail, contenant un lien pour démarrer le processus de remplissage et de signature du formulaire.

## Remplir des formulaires dans le package

La personne doit remplir et signer le premier formulaire du package. Une fois la signature du formulaire effectuée, la personne peut accéder au formulaire suivant dans le package. Une fois que tous les formulaires sont remplis et signés, la personne reçoit le formulaire **« Alldone »**.

## Résolution des problèmes

### La notification par e-mail n’est pas générée

La notification par e-mail est envoyée par le composant Envoyer un e-mail du workflow Signer plusieurs formulaires. Si l’une des étapes de ce workflow échoue, l’e-mail de notification est envoyé. Assurez-vous que l’étape de processus personnalisée du workflow crée des lignes dans votre base de données MySQL. Si les lignes sont en cours de création, vérifiez les paramètres de configuration du service de messagerie Day CQ.

### Le lien de la notification par e-mail ne fonctionne pas

Les liens contenus dans les notifications par e-mail sont générés dynamiquement. Si votre serveur AEM n’est pas exécuté sur localhost:4502, indiquez le nom et le port corrects du serveur dans les arguments de l’étape Stocker les formulaires à signer du workflow Signer plusieurs formulaires.

### Impossible de signer le formulaire

Cette situation peut se produire si le formulaire n’a pas été rempli correctement lors de la récupération des données de la source de données. Vérifiez les journaux stdout de votre serveur. Le fichier fetchformdata.jsp écrit des messages utiles au fichier stdout.

### Impossible d’accéder au formulaire suivant dans le package

Une fois la signature d’un formulaire du package terminée, le workflow Mettre à jour le statut de la signature est déclenché. La première étape du workflow met à jour le statut de signature du formulaire dans la base de données. Vérifiez si le statut du formulaire est mis à jour de 0 à 1.

### Pas de formulaire AllDone

Lorsqu’il n’y a plus de formulaires à signer dans le package, le formulaire AllDone s’affiche. Si vous ne voyez pas le formulaire AllDone, veuillez vérifier l’URL utilisée à la ligne 33 du fichier GetNextFormToSign.js de la bibliothèque cliente **getnextform**.
