---
title: Résolution des problèmes de signature de la solution de plusieurs Documents
description: Tester et résoudre les problèmes tirer la solution
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6960
thumbnail: 6960.jpg
topic: Development
role: Developer
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 2%

---


# Test et dépannage


## Prévisualisation du formulaire de refinancement

Le cas d’utilisation est déclenché lorsque l’agent du service à la clientèle remplit et envoie [le formulaire de refinancement](http://localhost:4502/content/dam/formsanddocuments/formsandsigndemo/refinanceform/jcr:content?wcmmode=disabled).

Le processus Signer plusieurs Forms obtient des déclencheurs lors de l’envoi de ce formulaire et le client reçoit une notification par courrier électronique contenant un lien vers le début du processus de remplissage et de signature du formulaire.

## Remplir des formulaires dans le package

Le client est invité à remplir et signer le premier formulaire du pack. Une fois le formulaire signé, le client peut accéder au formulaire suivant dans le pack. Une fois que tous les formulaires sont remplis et signés, le client reçoit le formulaire &quot;**AllDone**&quot;.

## Résolution des problèmes liés à

### La notification par courrier électronique n&#39;est pas générée

La notification par courrier électronique est envoyée par le composant Envoyer un courrier électronique dans le processus Signer plusieurs formulaires. Si l&#39;une des étapes de ce processus échoue, la notification par courrier électronique est envoyée. Assurez-vous que l’étape de processus personnalisée du flux de travail crée des lignes dans votre base de données MySQL. Si des lignes sont en cours de création, vérifiez les paramètres de configuration du service de messagerie Day CQ.

### Le lien de la notification par courrier électronique ne fonctionne pas

Les liens dans les notifications par courrier électronique sont générés dynamiquement. Si votre serveur d&#39;AEM n&#39;est pas exécuté sur localhost:4502, indiquez le nom et le port corrects du serveur dans les arguments de l&#39;étape Stocker le Forms à signer du processus Sign Multiple Forms.

### Impossible de signer le formulaire

Cela peut se produire si le formulaire n’a pas été renseigné correctement en récupérant les données de la source de données. Vérifiez les journaux stdout de votre serveur. Le fichier fetchformdata.jsp écrit quelques messages utiles à l&#39;archive stdout.

### Impossible d&#39;accéder au formulaire suivant dans le package

Lors de la signature réussie d’un formulaire dans le package, le processus Mettre à jour l’état de la signature est déclenché. La première étape du processus met à jour l’état de signature du formulaire dans la base de données. Vérifiez si l’état du formulaire est mis à jour de 0 à 1.

### Impossible de voir le formulaire ToutTerminé

S&#39;il n&#39;y a plus de formulaires à signer dans le package, le formulaire AllDone est présenté à l&#39;utilisateur.Si vous ne voyez pas le formulaire AllDone, veuillez vérifier l&#39;URL utilisée à la ligne 33 du fichier GetNextFormToSign.js qui fait partie du **getnextform** client lib.











