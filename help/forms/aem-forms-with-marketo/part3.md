---
title: AEM Forms avec Marketo (Partie 3)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 97
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '379'
ht-degree: 100%

---

# Configurer la source de données

L’intégration de données AEM Forms permet de configurer des sources de données disparates et de s’y connecter. Les types suivants sont pris en charge par défaut. Toutefois, avec peu de personnalisation, vous pouvez intégrer d’autres sources de données.

1. Bases de données relationnelles : MySQL, Microsoft SQL Server, IBM DB2 et Oracle RDBMS
1. Profil utilisateur AEM
1. Services web RESTful
1. Services web SOAP
1. Services OData 

Pour l’intégration d’AEM Forms à Marketo, nous utilisons les services web RESTful. La première étape de l’intégration consiste à configurer une [source de données.](https://experienceleague.adobe.com/docs/experience-manager-64/forms/form-data-model/configure-data-sources.html?lang=fr) Veuillez utiliser le fichier Swagger fourni dans le cadre de ce tutoriel. La capture d’écran suivante présente les propriétés importantes à spécifier lors de la configuration de la source de données.
![Source de données.](assets/datasource.jfif)

« marketo.json » est le fichier Swagger qui vous est fourni dans le cadre des ressources de ce tutoriel.
L’hôte de propriété est spécifique à votre instance Marketo.
Le type d’authentification est personnalisé et l’implémentation de l’authentification doit correspondre à « AemForms avec Marketo ». (sauf si vous avez modifié cette option dans votre code).

## Créer un modèle de données de formulaire

Ensuite, lors de la configuration de la source de données, l’étape suivante consiste à créer un modèle de données de formulaire basé sur la source de données configurée à l’étape précédente. Pour créer un modèle de données de formulaire, veuillez procéder comme suit :

Pointez votre navigateur sur la [page d’intégrations de données.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Cette section répertorie toutes les intégrations de données créées sur votre instance AEM.

1. Cliquez sur Créer | Modèle de données de formulaire.
1. Fournissez un titre significatif tel que « FormsAndMarketo » et cliquez sur Suivant.
1. Sélectionnez la source de données qui a été configurée à l’étape précédente et cliquez sur Créer et Modifier pour ouvrir le modèle de données de formulaire en mode d’édition.
1. Développez le nœud « FormsAndMarketo ». Développez le nœud Services.
1. Sélectionner la première opération « GET ».
1. Cliquez sur Ajouter la sélection.
1. Cliquez sur « Tout sélectionner » dans la boîte de dialogue « Ajouter des objets de modèle associés », puis cliquez sur Ajouter.
1. Enregistrez votre modèle de données de formulaire en cliquant sur le bouton Enregistrer.
1. Accédez à l’onglet Services.
1. Sélectionnez le seul service répertorié, puis cliquez sur Tester le service.
1. Fournissez un leadId valide et cliquez sur Tester. Si tout se passe bien, vous devriez récupérer les détails du lead comme illustré dans la capture d’écran ci-dessous.
   ![testresults](assets/testresults.jfif)

## Étapes suivantes

[Assembler pour le test](./part4.md)
