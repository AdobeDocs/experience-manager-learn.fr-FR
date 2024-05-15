---
title: Autorisations pilotées par les métadonnées dans AEM Assets
description: Les autorisations pilotées par les métadonnées sont une fonctionnalité utilisée pour restreindre l’accès en fonction des propriétés de métadonnées des ressources plutôt que de la structure de dossiers.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
doc-type: Tutorial
last-substantial-update: 2024-05-03T00:00:00Z
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
duration: 158
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '738'
ht-degree: 54%

---

# Autorisations pilotées par les métadonnées{#metadata-driven-permissions}

Les autorisations pilotées par les métadonnées sont une fonctionnalité utilisée pour permettre aux décisions de contrôle d’accès sur l’instance de création AEM Assets d’être basées sur les propriétés de métadonnées des ressources plutôt que sur la structure de dossiers. Grâce à cette fonctionnalité, vous pouvez définir des politiques de contrôle d’accès qui évaluent des attributs tels que le statut et le type de ressource, ou toute propriété de métadonnées personnalisée que vous définissez.

Prenons un exemple. Les personnes membres de l’équipe créative chargent leur travail dans AEM Assets vers le dossier associé à la campagne. Il peut s’agir d’une ressource en cours qui n’a pas été approuvée pour utilisation. Nous voulons nous assurer que les personnes spécialisées dans le marketing ne voient que les ressources approuvées pour cette campagne. Nous pouvons utiliser la propriété de métadonnées pour indiquer qu’une ressource a été approuvée et peut être utilisée par les personnes spécialisées dans le marketing.

## Fonctionnement

L’activation des autorisations pilotées par les métadonnées implique de définir quelles propriétés de métadonnées de ressource vont générer des restrictions d’accès, telles que « statut » ou « marque ». Ces propriétés peuvent ensuite être utilisées pour créer des entrées de contrôle d’accès qui spécifient quels groupes d’utilisateurs et d’utilisatrices ont accès aux ressources avec des valeurs de propriété spécifiques.

## Conditions préalables

L’accès à un environnement AEM as a Cloud Service mis à jour vers la dernière version est requis pour configurer des autorisations pilotées par les métadonnées.

## Configuration OSGi {#configure-permissionable-properties}

Pour mettre en oeuvre des autorisations pilotées par les métadonnées, un développeur doit déployer une configuration OSGi AEM as a Cloud Service, qui active des propriétés de métadonnées de ressources spécifiques pour alimenter les autorisations pilotées par les métadonnées.

1. Déterminez quelles propriétés de métadonnées de ressource seront utilisées pour le contrôle d’accès. Les noms des propriétés sont les noms des propriétés JCR sur le `jcr:content/metadata` ressource. Dans notre cas, il s’agira d’une propriété appelée `status`.
1. Créer une configuration OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` dans votre projet Maven AEM.
1. Collez le fichier JSON suivant dans le fichier créé :

   ```json
   {
     "restrictionPropertyNames":[
       "status",
       "brand"
     ],
     "enabled":true
   }
   ```

1. Remplacez les noms des propriétés par les valeurs requises.

## Réinitialisation des autorisations des ressources de base

Avant d’ajouter des entrées de contrôle d’accès basées sur des restrictions, une nouvelle entrée de niveau supérieur doit être ajoutée pour refuser l’accès en lecture à tous les groupes soumis à une évaluation des autorisations pour les ressources (par exemple, « contributeurs et contributrices » ou similaire) :

1. Accédez au __Outils → Sécurité → autorisations__ écran
1. Sélectionnez la variable __Contributeurs__ groupe (ou autre groupe personnalisé auquel tous les groupes d’utilisateurs appartiennent)
1. Cliquez sur __Ajouter ACE__ dans le coin supérieur droit de l’écran
1. Sélectionner `/content/dam` pour __Chemin__
1. Entrée `jcr:read` pour __Privilèges__
1. Sélectionner `Deny` pour __Type d’autorisation__
1. Sous Restrictions, sélectionnez `rep:ntNames` et saisissez `dam:Asset` comme la propriété __Valeur de restriction__
1. Cliquez sur __Enregistrer__

![Refuser l’accès](./assets/metadata-driven-permissions/deny-access.png)

## Accorder l’accès aux ressources par métadonnées

Il est désormais possible d’ajouter des entrées de contrôle d’accès pour accorder un accès en lecture aux groupes d’utilisateurs en fonction de la variable [valeurs de propriété de métadonnées de ressource configurées](#configure-permissionable-properties).

1. Accédez au __Outils → Sécurité → autorisations__ écran
1. Sélectionner les groupes d’utilisateurs qui doivent avoir accès aux ressources
1. Cliquez sur __Ajouter ACE__ dans le coin supérieur droit de l’écran
1. Sélectionner `/content/dam` (ou un sous-dossier) pour __Chemin__
1. Entrée `jcr:read` pour __Privilèges__
1. Sélectionner `Allow` pour __Type d’autorisation__
1. Sous __Restrictions__, sélectionnez l’une des [noms de propriétés de métadonnées de ressource configurés dans la configuration OSGi](#configure-permissionable-properties)
1. Saisissez la valeur de propriété de métadonnées requise dans la variable __Valeur de restriction__ field
1. Cliquez sur le bouton __+__ pour ajouter la restriction à l’entrée de contrôle d’accès
1. Cliquez sur __Enregistrer__

![Autorisation d’accès](./assets/metadata-driven-permissions/allow-access.png)

## Autorisations pilotées par les métadonnées en vigueur

Le dossier d’exemple contient quelques ressources.

![Vue d’administration](./assets/metadata-driven-permissions/admin-view.png)

Une fois que vous avez configuré les autorisations et défini les propriétés de métadonnées de ressource en conséquence, les utilisateurs et utilisatrices (dans notre cas, les personnes spécialisées dans le marketing) ne verront que la ressource approuvée.

![Vue des personnes spécialisées dans le marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Avantages et considérations

Les avantages des autorisations gérées par les métadonnées incluent :

- Contrôle précis de l’accès aux ressources en fonction d’attributs spécifiques.
- Découplage des politique de contrôle d’accès de la structure de dossiers, ce qui permet une organisation des ressources plus flexible.
- Possibilité de définir des règles de contrôle d’accès complexes basées sur plusieurs propriétés de métadonnées.

>[!NOTE]
>
> Il est important de noter :
> 
> - Les propriétés de métadonnées sont évaluées par rapport aux restrictions à l’aide de __Égalité des chaînes__ (`=`) (les autres types de données ou opérateurs ne sont pas encore pris en charge, pour les opérateurs supérieurs à (`>`) ou Propriétés de date)
> - Pour autoriser plusieurs valeurs pour une propriété de restriction, vous pouvez ajouter des restrictions supplémentaires à l’entrée de contrôle d’accès en sélectionnant la même propriété dans la liste déroulante « Sélectionner un type » et en saisissant une nouvelle valeur de restriction (par exemple, `status=approved`, `status=wip`) et cliquez sur « + » pour ajouter la restriction à l’entrée.
> ![Autoriser plusieurs valeurs](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - __Restrictions ET__ sont pris en charge, via plusieurs restrictions dans une seule entrée de contrôle d’accès avec différents noms de propriété (par exemple, `status=approved`, `brand=Adobe`) sera évalué en tant que condition ET, c’est-à-dire que le groupe d’utilisateurs sélectionné se verra accorder un accès en lecture aux ressources comportant la variable `status=approved AND brand=Adobe`
> ![Autoriser plusieurs restrictions](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - __Restrictions OR__ sont pris en charge en ajoutant une nouvelle entrée de contrôle d’accès avec une restriction de propriété de métadonnées permet d’établir une condition OU pour les entrées, par exemple une entrée unique avec restriction. `status=approved` et une seule entrée avec `brand=Adobe` est évalué comme `status=approved OR brand=Adobe`
> ![Autoriser plusieurs restrictions](./assets/metadata-driven-permissions/allow-multiple-aces.png)
