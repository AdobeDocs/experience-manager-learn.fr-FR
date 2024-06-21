---
title: Création du composant Adresse
description: Création d’un composant principal d’adresse dans AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# Créer un composant Adresse

Connectez-vous à CRXDE de votre instance locale prête pour le cloud d’AEM Forms.

Effectuez une copie de la fonction ``/apps/bankingapplication/components/adaptiveForm/button`` et renommez-le en address block. Sélectionnez le noeud address block et définissez ses propriétés comme illustré ci-dessous.

>[!NOTE]
>
> ``bankingapplication`` est l’appId fourni lors de la création du projet Maven. Cet appId peut être différent dans votre environnement. Vous pouvez faire une copie de n&#39;importe quel composant. Il se trouve que je fais une copie du composant de bouton.


![address-bloc](assets/address-properties.png)

## Propriétés du noeud cq-template

Sélectionnez la variable ``cq-template`` sous ``addressblock`` et définissez ses propriétés comme illustré ci-dessous. Notez que le champType est défini sur panneau.
![cq-template](assets/cq-template.png)

## Ajout de noeuds sous cq-template

Ajoutez les noeuds suivants de type ``nt:unstructured`` under ``cq-template``

* streetaddress
* city
* zip
* state

Ces noeuds représentent les champs du composant de bloc d’adresse. Les champs streetaddress, city et zip seront un champ de saisie de texte et le champ state un champ de liste déroulante.

## Définir les propriétés du noeud streetaddress

>[!NOTE]
>
> La variable **_application bancaire_** dans le chemin d’accès fait référence à l’appId du projet Maven. Cela peut être différent dans votre environnement.

Sélectionnez la variable ``streetaddress`` et définissez ses propriétés comme illustré ci-dessous.
![street-address](assets/streetaddress.png)

## Définition des propriétés du noeud city

Sélectionnez la variable ``city`` et définissez ses propriétés comme illustré ci-dessous.
![city](assets/city.png)

## Définition des propriétés du noeud zip

Sélectionnez la variable ``zip`` et définissez ses propriétés comme illustré ci-dessous.
![zip](assets/zip.png)

## Définition des propriétés du noeud d’état

Sélectionnez la variable ``state`` et définissez ses propriétés comme illustré ci-dessous. Remarquez le champType d’état : il est défini comme une liste déroulante.
![state](assets/state.png)

Le composant de bloc d’adresse final ressemblera à ceci :

![final-address](assets/crx-address-block.png)

## Étapes suivantes

[Déploiement du projet](./deploy-your-project.md)




