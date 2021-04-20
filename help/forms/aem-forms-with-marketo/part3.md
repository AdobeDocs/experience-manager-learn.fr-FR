---
title: AEM Forms avec Marketo (partie 3)
seo-title: AEM Forms avec Marketo (partie 3)
description: Didacticiel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
seo-description: Didacticiel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 13%

---


# Configurer la source de données

L’intégration de données AEM Forms permet de configurer des sources de données disparates et de s’y connecter. La prise en charge est assurée par défaut pour les types suivants. Cependant, avec une petite personnalisation, vous pouvez également vous intégrer à d’autres sources de données.

1. Bases de données relationnelles : MySQL, Microsoft SQL Server, IBM DB2 et Oracle RDBMS
1. Profil utilisateur AEM
1. Services web RESTful
1. Services web SOAP
1. Services OData

Pour l&#39;intégration de AEM Forms avec Marketo, nous utiliserons les services web RESTful. La première étape de l&#39;intégration consiste à configurer une source de données [.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Veuillez utiliser le fichier swagger fourni dans le cadre de ce tutoriel. La capture d’écran suivante présente les propriétés importantes à spécifier lors de la configuration de la source de données.
![datasource](assets/datasource.jfif)

Le fichier &quot;marketo.json&quot; est le fichier swagger et vous est fourni dans le cadre des ressources de ce didacticiel.
L’hôte de propriété est spécifique à votre instance de Marketo.
Le type d’authentification est personnalisé et l’implémentation de l’authentification doit correspondre à &quot;AemForms avec Marketo&quot;. (sauf si vous avez modifié ceci dans votre code).

## Créer un modèle de données de formulaire

Ensuite, la configuration de la source de données passe par la création d’un modèle de données de formulaire basé sur la source de données configurée à l’étape précédente. Pour créer un modèle de données de formulaire, procédez comme suit :

Pointez votre navigateur sur la page [intégrations de données.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Cette liste toutes les intégrations de données créées sur votre instance AEM.

1. Cliquez sur Créer | Modèle de données de formulaire
1. Fournissez un titre significatif, tel que FormsAndMarketo, puis cliquez sur Suivant.
1. Sélectionnez la source de données qui a été configurée à l’étape précédente, puis cliquez sur Créer et modifier pour ouvrir le modèle de données de formulaire en mode d’édition.
1. Développez le noeud &quot;FormsAndMarketo&quot;. Développez le noeud Services.
1. Sélectionner la première opération &quot;Get&quot;
1. Cliquez sur Ajouter sélectionné
1. Cliquez sur &quot;Sélectionner tout&quot; dans la boîte de dialogue &quot;Ajouter les objets de modèle associés&quot;, puis cliquez sur Ajouter.
1. Enregistrez votre modèle de données de formulaire en cliquant sur le bouton Enregistrer
1. Onglet de l’onglet Services
1. Sélectionnez le seul service répertorié et cliquez sur le service de test.
1. Fournissez un prospectId valide et cliquez sur Test. Si tout se passe bien, vous devriez récupérer les informations de piste comme le montre la capture d&#39;écran ci-dessous.
   ![testrésultats](assets/testresults.jfif)
