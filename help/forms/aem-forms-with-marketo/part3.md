---
title: AEM Forms avec Marketo (partie 3)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 14%

---

# Configuration de la source de données

L’intégration de données AEM Forms permet de configurer des sources de données disparates et de s’y connecter. La prise en charge est assurée par défaut pour les types suivants. Cependant, avec une petite personnalisation, vous pouvez également intégrer d’autres sources de données.

1. Bases de données relationnelles : MySQL, Microsoft SQL Server, IBM DB2 et Oracle RDBMS
1. Profil utilisateur AEM
1. Services web RESTful
1. Services web SOAP
1. Services OData 

Pour l’intégration d’AEM Forms à Marketo, nous utilisons les services web RESTful. La première étape de l’intégration consiste à configurer une [source de données.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Utilisez le fichier swagger fourni dans le cadre de ce tutoriel. La capture d’écran suivante présente les propriétés importantes à spécifier lors de la configuration de la source de données.
![datasource](assets/datasource.jfif)

&quot;marketo.json&quot; est le fichier swagger qui vous est fourni dans le cadre des ressources de ce tutoriel.
L’hôte de propriété est spécifique à votre instance Marketo.
Le type d’authentification est personnalisé et l’implémentation de l’authentification doit correspondre à &quot;AemForms avec Marketo&quot;. (Sauf si vous avez modifié cette variable dans votre code).

## Création d’un modèle de données de formulaire

Ensuite, lors de la configuration de la source de données, l’étape suivante consiste à créer un modèle de données de formulaire basé sur la source de données configurée à l’étape précédente. Pour créer un modèle de données de formulaire, procédez comme suit :

Pointez votre navigateur sur la [intégrations de données .](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Cette section répertorie toutes les intégrations de données créées sur votre instance AEM.

1. Cliquez sur Créer | Modèle de données de formulaire
1. Fournissez un titre significatif tel que FormsAndMarketo et cliquez sur Suivant
1. Sélectionnez la source de données qui a été configurée à l’étape précédente et cliquez sur Créer et modifier pour ouvrir le modèle de données de formulaire en mode d’édition.
1. Développez le noeud &quot;FormsAndMarketo&quot;. Développez le noeud Services .
1. Sélectionner la première opération &quot;Get&quot;
1. Cliquez sur Ajouter la sélection .
1. Cliquez sur &quot;Tout sélectionner&quot; dans la boîte de dialogue &quot;Ajouter des objets de modèle associés&quot;, puis cliquez sur Ajouter .
1. Enregistrez votre modèle de données de formulaire en cliquant sur le bouton Enregistrer
1. Onglet Services
1. Sélectionnez le seul service répertorié, puis cliquez sur le service de test.
1. Fournissez un leadId valide et cliquez sur Test. Si tout se passe bien, vous devriez récupérer les détails de la piste comme illustré dans la capture d’écran ci-dessous.
   ![testresults](assets/testresults.jfif)
