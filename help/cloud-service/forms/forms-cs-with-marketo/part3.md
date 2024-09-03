---
title: Intégration d’AEM Forms Cloud Service et de Marketo (partie 3)
description: Découvrez comment intégrer AEM Forms et Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Form Data Model,Integration
version: Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Intégration" type="positive"
badgeVersions: label="dʼAEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 43737765-b1ea-4594-853a-d78f41136b5e
source-git-commit: ba744f95f8d1f0b982cd5430860f0cb0945a4cda
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 77%

---

# Créer un modèle de données de formulaire

Après avoir configuré la source de données, l’étape suivante consiste à créer un modèle de données de formulaire basé sur la source de données configurée à l’étape précédente. Pour créer un modèle de données de formulaire, veuillez procéder comme suit :

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
   ![testresults](assets/testresults.png)

Vous pouvez désormais créer un formulaire adaptatif basé sur ce modèle de données de formulaire pour insérer et récupérer des objets Marketo.
