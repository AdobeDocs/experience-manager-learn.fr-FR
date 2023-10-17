---
title: Configurer un modèle de données de formulaire
description: Créer un modèle de données de formulaire basé sur la source de données RDBMS
feature: Adaptive Forms
version: 6.4,6.5
kt: 5812
thumbnail: kt-5812.jpg
topic: Development
role: Developer
level: Beginner
exl-id: 5fa4638f-9faa-40e0-a20d-fdde3dbb528a
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: ht
source-wordcount: '498'
ht-degree: 100%

---

# Configurer un modèle de données de formulaire

## Source de données mise en pool de la connexion Apache Sling

La première étape de la création d’un modèle de données de formulaire soutenu par RDBMS est de configurer la source de données mise en pool de la connexion Apache Sling. Pour configurer la source de données, procédez comme suit :

* Pointez votre navigateur sur [configMgr](http://localhost:4502/system/console/configMgr).
* Recherchez **Source de données mise en pool de la connexion Apache Sling**.
* Ajoutez une nouvelle entrée et fournissez les valeurs comme illustré dans la capture d’écran.
* ![data-source](assets/data-source.png)
* Enregistrez vos modifications.

>[!NOTE]
>L’URI de connexion JDBC, le nom d’utilisateur ou d’utilisatrice et le mot de passe changent en fonction de la configuration de votre base de données MySQL.


## Créer un modèle de données de formulaire

* Pointez votre navigateur sur [Intégrations de données](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm).
* Cliquez sur _Créer_->_Modèle de données de formulaire_.
* Fournissez un nom et un titre significatifs au modèle de données de formulaire, par exemple **Employé ou employée**.
* Cliquez sur _Suivant_.
* Sélectionnez la source de données créée dans la section précédente (forums).
* Cliquez sur _Créer_ -> Modifier pour ouvrir le modèle de données de formulaire nouvellement créé en mode d’édition.
* Développez le nœud _forums_ pour afficher le schéma des employées et employés. Développez le nœud employé pour afficher les deux tableaux.

## Ajoutez des entités à votre modèle.

* Assurez-vous que le nœud employé est développé.
* Sélectionnez les entités représentant les personnes récemment embauchées et les entités bénéficiaires et cliquez sur _Ajouter la sélection_.

## Ajoutez un service de lecture à l’entité représentant les personnes récemment embauchées.

* Sélectionner l’entité représentant les personnes récemment embauchées
* Cliquez sur _Modifier les propriétés_.
* Sélectionnez GET dans la liste déroulante du service de lecture.
* Cliquez sur l’icône + pour ajouter un paramètre au service GET.
* Spécifiez les valeurs comme illustré dans la capture d’écran.
* ![get-service](assets/get-service.png)
>[!NOTE]
> Le service GET exige qu’une valeur soit mappée à la colonne empID de l’entité représentant les personnes récemment embauchées. Il existe plusieurs façons de transmettre cette valeur. Dans ce tutoriel, l’empID est transmis par le paramètre de requête appelé empID.
* Cliquez sur _Terminé_ pour enregistrer les arguments du service GET.
* Cliquez sur _Terminé_ pour enregistrer les modifications apportées au modèle de données de formulaire.

## Ajouter une association entre 2 entités

Les associations définies entre les entités de base de données ne sont pas créées automatiquement dans le modèle de données de formulaire. Les associations entre les entités doivent être définies à l’aide de l’éditeur de modèle de données de formulaire. Chaque entité représentant les personnes récemment embauchées peut avoir un ou plusieurs bénéficiaires, nous devons définir une association d’un objet à plusieurs objets entre les entités représentant les personnes récemment embauchées et bénéficiaires.
Les étapes suivantes vous guideront tout au long du processus de création de l’association d’un objet à plusieurs objets.

* Sélectionnez l’entité représentant les personnes récemment embauchées et cliquez sur _Ajouter une association_.
* Attribuez un titre et un identifiant significatifs à l’association et aux autres propriétés, comme illustré dans la capture d’écran ci-dessous.
  ![association](assets/association-entities-1.png)

* Cliquez sur l’icône _modifier_ sous la section Arguments.

* Spécifiez les valeurs comme illustré dans cette capture d’écran.
* ![association-2](assets/association-entities.png)
* **Nous relions les deux entités à l’aide de la colonne empID des bénéficiaires et des entités représentant les personnes récemment embauchées.**
* Cliquez sur _Terminé_ pour enregistrer vos modifications.

## Tester votre modèle de données de formulaire

Notre modèle de données de formulaire comporte maintenant le service **_GET_** qui accepte empID et renvoie les détails de l’entité représentant les personnes récemment embauchées et de ses bénéficiaires. Pour tester le service GET, suivez les étapes ci-dessous.

* Sélectionner l’entité représentant les personnes récemment embauchées
* Cliquez sur _Objet de modèle de test_.
* Fournissez un empID valide et cliquez sur _Tester_.
* Vous devriez obtenir les résultats indiqués dans la capture d’écran ci-dessous.
* ![test-fdm](assets/test-form-data-model.png)

## Étapes suivantes

[Obtenir l’empID à partir de l’URL](./get-request-parameter.md)