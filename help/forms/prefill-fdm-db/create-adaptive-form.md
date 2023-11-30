---
title: Créer un formulaire adaptatif
description: Créer et configurer un formulaire adaptatif pour utiliser le service de préremplissage du modèle de données de formulaire
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5813
thumbnail: kt-5813.jpg
topic: Development
role: User
level: Beginner
exl-id: c8d4eed8-9e2b-458c-90d8-832fc9e0ad3f
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 100%

---

# Créer un formulaire adaptatif

Jusqu’à présent, nous avons créé les éléments suivants.

* Base de données avec 2 tableaux - `newhire` et `beneficiaries`.
* Configuration de la source de données mise en pool de la connexion Apache Sling.
* Modèle de données de formulaire basé sur RDBMS.

L’étape suivante consiste à créer et configurer un formulaire adaptatif pour utiliser le modèle de données de formulaire.  Pour commencer, vous pouvez [télécharger et importer](assets/fdm-demo-af.zip) un exemple de formulaire. L’exemple de formulaire comporte une section pour afficher les détails de la personne employée et une autre section pour répertorier les personnes bénéficiaires de la personne employée.

## Associer un formulaire à un modèle de données de formulaire

L’exemple de formulaire fourni avec ce cours n’est associé à aucun modèle de données de formulaire. Pour configurer le formulaire afin qu’il utilise le modèle de données de formulaire, procédez comme suit :

* Sélectionnez le formulaire DMDemo.
* Cliquez sur _Propriétés_->_Modèle de formulaire_.
* Sélectionnez Modèle de données de formulaire dans la liste déroulante.
* Recherchez et sélectionnez votre modèle de données de formulaire créé dans la leçon précédente.
* Cliquez sur _Enregistrer et fermer_.

## Configurer le service de préremplissage

La première étape consiste à associer le service de préremplissage au formulaire. Pour associer le service de préremplissage, procédez comme suit :

* Sélectionnez le formulaire `FDMDemo`.
* Cliquez sur _Modifier_ pour ouvrir la ressource en mode d’édition.
* Sélectionnez le conteneur de formulaires dans la hiérarchie de contenu, puis cliquez sur l’icône en forme de clé à molette pour ouvrir sa feuille de propriétés.
* Sélectionnez _Service de remplissage de modèle de données de formulaire_ dans la liste déroulante Service de préremplissage.
* Cliquez sur l’icône ☑ bleue pour enregistrer vos modifications.

* ![prefill-service](assets/fdm-prefill.png)

## Configurer les détails des personnes employées

L’étape suivante consiste à lier les champs de texte du formulaire adaptatif aux éléments du modèle de données de formulaire. Vous devrez ouvrir la feuille de propriétés des champs suivants et définir sa bindRef comme illustré ci-dessous.


| Nom du champ | Référence de liaison |
|------------|--------------------|
| FirstName | /newhire/FirstName |
| LastName | /newhire/lastName |

>[!NOTE]
>
>N’hésitez pas à ajouter des champs de texte supplémentaires et à les lier aux éléments de modèle de données de formulaire appropriés.

## Configurer le tableau des personnes bénéficiaires

L’étape suivante consiste à afficher les personnes bénéficiaires de la personne employée sous forme de tableau. L’exemple de formulaire fourni comporte un tableau de 4 colonnes et d’une seule ligne. Nous devons configurer le tableau pour qu’il s’agrandisse en fonction du nombre de personnes bénéficiaires.

* Ouvrez le formulaire adaptatif en mode de modification.
* Développez le Panneau racine -> Vos personnes bénéficiaires -> Tableau.
* Sélectionnez Ligne 1 et cliquez sur l’icône de clé à molette pour ouvrir sa feuille de propriétés.
* Définissez la référence de liaison sur **/newhire/GetEmployeesBeneficiaries**
* Définissez les paramètres de répétition : nombre minimum sur 1 et nombre maximum sur 5.
* La configuration de la Ligne 1 doit ressembler à la capture d’écran ci-dessous.
  ![row-configure](assets/configure-row.PNG)
* Cliquez sur l’icône ☑ bleue pour enregistrer vos modifications.

## Lier les cellules de ligne

Enfin, nous devons lier les cellules de ligne aux éléments de modèle de données de formulaire.

* Développez le Panneau racine -> Vos personnes bénéficiaires -> Tableau -> Ligne 1.
* Définissez la référence de liaison de chaque cellule de ligne conformément au tableau ci-dessous.

| Cellule de ligne | Référence de liaison |
|------------|----------------------------------------------|
| Prénom | /newhire/GetEmployeeBeneficiaries/firstname |
| Nom | /newhire/GetEmployeeBeneficiaries/lastname |
| Relation | /newhire/GetEmployeeBeneficiaries/relation |
| Pourcentage | /newhire/GetEmployeeBeneficiaries/percentage |

* Cliquez sur l’icône ☑ bleue pour enregistrer vos modifications.

## Tester votre formulaire

Nous devons maintenant ouvrir le formulaire avec l’empID approprié dans l’URL. Les 2 liens suivants renseigneront les formulaires avec des informations de la base de données :
[Formulaire avec empID=207](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=207).
[Formulaire avec empID=208](http://localhost:4502/content/dam/formsanddocuments/fdmdemo/jcr:content?wcmmode=disabled&amp;empID=208).

## Résolution des problèmes

Mon formulaire est vide et ne contient aucune donnée.

* Assurez-vous que le modèle de données de formulaire renvoie les bons résultats.
* Le formulaire est associé au modèle de données de formulaire approprié.
* Vérifiez les liaisons de champ.
* Consultez le fichier journal stdout. Vous devriez normalement voir empID écrit dans le fichier. Si vous ne voyez pas cette valeur, votre formulaire peut ne pas utiliser le modèle personnalisé fourni.

Le tableau n’est pas renseigné.

* Vérifiez la liaison de Ligne 1.
* Assurez-vous que les paramètres de répétition pour Ligne 1 sont correctement définis (Min = 1 et Max = 5 ou plus).
