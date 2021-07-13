---
title: Créer un formulaire adaptatif
description: Créer et configurer un formulaire adaptatif pour utiliser le service de préremplissage du modèle de données de formulaire
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5813
thumbnail: kt-5813.jpg
topic: Développement
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '609'
ht-degree: 3%

---


# Créer un formulaire adaptatif

Jusqu’à présent, nous avons créé les éléments suivants :

* Base de données avec 2 tables - `newhire` et `beneficiaries`
* Source de données en pool de la connexion Apache Sling configurée
* Modèle de données de formulaire basé sur RDBMS

L’étape suivante consiste à créer et configurer un formulaire adaptatif pour utiliser le modèle de données de formulaire.  Pour démarrer rapidement, vous pouvez [télécharger et importer ](assets/fdm-demo-af.zip) exemple de formulaire. L’exemple de formulaire comporte une section pour afficher les détails de l’employé et une autre section pour répertorier les bénéficiaires de l’employé.

## Association d’un formulaire à un modèle de données de formulaire

L’exemple de formulaire fourni avec ce cours n’est associé à aucun modèle de données de formulaire. Pour configurer le formulaire de sorte qu’il utilise le modèle de données de formulaire, procédez comme suit :

* Sélectionnez le formulaire DMDemo
* Cliquez sur _Propriétés_->_Modèle de formulaire_
* Sélectionnez Modèle de données de formulaire dans la liste déroulante
* Recherchez et sélectionnez votre modèle de données de formulaire créé dans la leçon précédente.
* Cliquez sur _Enregistrer et fermer_

## Configurer le service de préremplissage

La première étape consiste à associer le service de préremplissage au formulaire. Pour associer le service de préremplissage, procédez comme suit :

* Sélectionnez le formulaire `FDMDemo`
* Cliquez sur _Modifier_ pour ouvrir le formulaire en mode d’édition.
* Sélectionnez Form Container dans la hiérarchie de contenu, puis cliquez sur l’icône en forme de clé à molette pour ouvrir sa feuille de propriétés.
* Sélectionnez _Service de préremplissage de modèle de données de formulaire_ dans la liste déroulante Service de préremplissage .
* Cliquez sur bleu ☑ pour enregistrer vos modifications.

* ![prefill-service](assets/fdm-prefill.png)

## Configuration des détails des employés

L’étape suivante consiste à lier les champs de texte du formulaire adaptatif aux éléments du modèle de données de formulaire. Vous devrez ouvrir la feuille de propriétés des champs suivants et définir son bindRef comme illustré ci-dessous.


| Nom du champ | Liaison Ref |
|------------|--------------------|
| Prénom | /newhire/FirstName |
| Nom de famille | /newhire/lastName |

>[!NOTE]
>
>N’hésitez pas à ajouter des champs de texte supplémentaires et à les lier aux éléments de modèle de données de formulaire appropriés.

## Configurer le tableau des bénéficiaires

L’étape suivante consiste à afficher les bénéficiaires de l’employé sous forme de tableau. L’exemple de formulaire fourni comporte un tableau de 4 colonnes et une seule ligne. Nous devons configurer la table pour qu&#39;elle s&#39;agrandisse en fonction du nombre de bénéficiaires.

* Ouvrez le formulaire en mode de modification.
* Développer le panneau racine -> Vos bénéficiaires -> Tableau
* Sélectionnez Row1 et cliquez sur l’icône de clé à molette pour ouvrir la feuille de propriétés correspondante.
* Définissez la référence de liaison sur **/newhire/GetemployeeBénéficiaries**
* Définissez Paramètres de répétition - Nombre minimum sur 1 et Nombre maximum sur 5.
* La configuration de la ligne 1 doit ressembler à la capture d’écran ci-dessous.
   ![row-configure](assets/configure-row.PNG)
* Cliquez sur le ☑ bleu pour enregistrer vos modifications.

## Liaison de cellules de rangée

Enfin, nous devons lier les cellules de ligne aux éléments de modèle de données de formulaire.

* Développer le panneau racine -> Vos bénéficiaires -> Tableau -> Ligne 1
* Définissez la référence de liaison de chaque cellule de ligne conformément au tableau ci-dessous.

| Cellule de ligne | Référence de liaison |
|------------|----------------------------------------------|
| Prénom | /newhire/GetEmployeesBénéficiaries/firstname |
| Nom | /newhire/GetEmployeesBénéficiaries/lastname |
| Relation | /newhire/GetemployeeBénéficiaries/relation |
| Pourcentage | /newhire/GetemployeeBénéficiaries/pourcentage |

* Cliquez sur le ☑ bleu pour enregistrer vos modifications.

## Tester votre formulaire

Nous devons maintenant ouvrir le formulaire avec l’empID approprié dans l’URL. Les 2 liens suivants renseigneront les formulaires avec des informations de la base de données
[Formulaire avec empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207)
[Formulaire avec empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208)

## Résolution des problèmes

Mon formulaire est vide et ne contient aucune donnée.

* Assurez-vous que le modèle de données de formulaire renvoie les résultats corrects.
* Le formulaire est associé au modèle de données de formulaire approprié.
* Vérifier les liaisons de champ
* Consultez le fichier journal stdout. empID devrait normalement être écrit dans le fichier. Si vous ne voyez pas cette valeur, votre formulaire peut ne pas utiliser le modèle personnalisé fourni.

Le tableau n’est pas renseigné

* Vérification de la liaison Row1
* Assurez-vous que les paramètres de répétition pour Row1 sont correctement définis (Min =1 et Max = 5 ou plus).

