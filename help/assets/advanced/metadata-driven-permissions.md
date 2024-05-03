---
title: Autorisations pilotées par les métadonnées dans AEM Assets
description: Les autorisations pilotées par les métadonnées sont une fonctionnalité utilisée pour restreindre l’accès en fonction des propriétés de métadonnées des ressources plutôt que de la structure de dossiers.
version: Cloud Service
topic: Administration, Security
feature: User and Groups
role: Admin
level: Intermediate
jira: KT-13757
thumbnail: xx.jpg
doc-type: Tutorial
exl-id: 57478aa1-c9ab-467c-9de0-54807ae21fb1
source-git-commit: 03cb7ef0cf79a21ec1b96caf6c11e6f5119f777c
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 98%

---

# Autorisations pilotées par les métadonnées{#metadata-driven-permissions}

Les autorisations pilotées par les métadonnées sont une fonctionnalité utilisée pour permettre aux décisions de contrôle d’accès sur l’instance de création AEM Assets d’être basées sur les propriétés de métadonnées des ressources plutôt que sur la structure de dossiers. Grâce à cette fonctionnalité, vous pouvez définir des politiques de contrôle d’accès qui évaluent des attributs tels que le statut et le type de ressource, ou toute propriété de métadonnées personnalisée que vous définissez.

Prenons un exemple. Les personnes membres de l’équipe créative chargent leur travail dans AEM Assets vers le dossier associé à la campagne. Il peut s’agir d’une ressource en cours qui n’a pas été approuvée pour utilisation. Nous voulons nous assurer que les personnes spécialisées dans le marketing ne voient que les ressources approuvées pour cette campagne. Nous pouvons utiliser la propriété de métadonnées pour indiquer qu’une ressource a été approuvée et peut être utilisée par les personnes spécialisées dans le marketing.

## Fonctionnement

L’activation des autorisations pilotées par les métadonnées implique de définir quelles propriétés de métadonnées de ressource vont générer des restrictions d’accès, telles que « statut » ou « marque ». Ces propriétés peuvent ensuite être utilisées pour créer des entrées de contrôle d’accès qui spécifient quels groupes d’utilisateurs et d’utilisatrices ont accès aux ressources avec des valeurs de propriété spécifiques.

## Conditions préalables

L’accès à un environnement AEM as a Cloud Service mis à jour vers la dernière version est requis pour configurer des autorisations pilotées par les métadonnées.


## Étapes de développement

Pour implémenter les autorisations pilotées par les métadonnées :

1. Déterminez quelles propriétés de métadonnées de ressource seront utilisées pour le contrôle d’accès. Dans notre cas, il s’agira d’une propriété appelée `status`.
1. Créez une configuration OSGi `com.adobe.cq.dam.assetmetadatarestrictionprovider.impl.DefaultRestrictionProviderConfiguration.cfg.json` dans votre projet.
1. Collez le JSON suivant dans le fichier créé.

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


Avant d’ajouter des entrées de contrôle d’accès basées sur des restrictions, une nouvelle entrée de niveau supérieur doit être ajoutée pour refuser l’accès en lecture à tous les groupes soumis à une évaluation des autorisations pour les ressources (par exemple, « contributeurs et contributrices » ou similaire) :

1. Accédez à l’écran Outils → Sécurité → Autorisations.
1. Sélectionnez le groupe « Contributeurs et contributrices » (ou tout autre groupe personnalisé auquel tous les groupes d’utilisateurs et d’utilisatrices appartiennent).
1. Cliquez sur « Ajouter une entrée de contrôle d’accès » dans le coin supérieur droit de l’écran.
1. Sélectionnez /content/dam pour le chemin.
1. Saisissez jcr:read pour les privilèges.
1. Sélectionnez Refuser pour le type d’autorisation.
1. Sous Restrictions, sélectionnez rep:ntNames et saisissez dam:Asset comme valeur de restriction.
1. Cliquez sur Enregistrer.

![Refuser l’accès](./assets/metadata-driven-permissions/deny-access.png)

Il est désormais possible d’ajouter des entrées de contrôle d’accès afin d’accorder un accès en lecture aux groupes d’utilisateurs et d’utilisatrices en fonction des valeurs des propriétés de métadonnées de ressource.

1. Accédez à l’écran Outils → Sécurité → Autorisations.
1. Sélectionnez le groupe souhaité.
1. Cliquez sur « Ajouter une entrée de contrôle d’accès » dans le coin supérieur droit de l’écran.
1. Sélectionnez /content/dam (ou un sous-dossier) pour le chemin.
1. Saisissez jcr:read pour les privilèges.
1. Sélectionnez Autoriser pour le type d’autorisation.
1. Sous Restrictions, sélectionnez l’un des noms de propriétés de métadonnées de ressource configurés (les propriétés définies dans la configuration OSGi seront incluses ici).
1. Saisissez la valeur de propriété de métadonnées requise dans le champ Valeur de restriction.
1. Cliquez sur l’icône « + » pour ajouter la restriction à l’entrée de contrôle d’accès.
1. Cliquez sur Enregistrer.

![Autorisation d’accès](./assets/metadata-driven-permissions/allow-access.png)

Le dossier d’exemple contient quelques ressources.

![Vue d’administration](./assets/metadata-driven-permissions/admin-view.png)

Une fois que vous avez configuré les autorisations et défini les propriétés de métadonnées de ressource en conséquence, les utilisateurs et utilisatrices (dans notre cas, les personnes spécialisées dans le marketing) ne verront que la ressource approuvée.

![Vue des personnes spécialisées dans le marketing](./assets/metadata-driven-permissions/marketeer-view.png)

## Avantages et considérations

Les avantages des autorisations pilotées par les métadonnées sont les suivants :

- Contrôle précis de l’accès aux ressources en fonction d’attributs spécifiques.
- Découplage des politique de contrôle d’accès de la structure de dossiers, ce qui permet une organisation des ressources plus flexible.
- Possibilité de définir des règles de contrôle d’accès complexes basées sur plusieurs propriétés de métadonnées.

>[!NOTE]
>
> Il est important de noter :
> 
> - Les propriétés des métadonnées sont évaluées par rapport aux restrictions à l’aide de l’égalité des chaînes (autres types de données pas encore pris en charge, par exemple : date).
> - Pour autoriser plusieurs valeurs pour une propriété de restriction, vous pouvez ajouter des restrictions supplémentaires à l’entrée de contrôle d’accès en sélectionnant la même propriété dans la liste déroulante « Sélectionner un type » et en saisissant une nouvelle valeur de restriction (par exemple, `status=approved`, `status=wip`) et cliquez sur « + » pour ajouter la restriction à l’entrée.
> ![Autoriser plusieurs valeurs](./assets/metadata-driven-permissions/allow-multiple-values.png)
> - Plusieurs restrictions dans une seule entrée de contrôle d’accès avec différents noms de propriété (par exemple, `status=approved`, `brand=Adobe`) seront évaluées en tant que condition AND, c’est-à-dire que le groupe d’utilisateurs et d’utilisatrices sélectionné se verra accorder un accès en lecture aux ressources avec `status=approved AND brand=Adobe`.
> ![Autoriser plusieurs restrictions](./assets/metadata-driven-permissions/allow-multiple-restrictions.png)
> - L’ajout d’une nouvelle entrée de contrôle d’accès avec une restriction de propriété de métadonnées établit une condition OR pour les entrées, par exemple une seule entrée avec la restriction `status=approved` et une seule entrée avec `brand=Adobe` seront évaluées comme `status=approved OR brand=Adobe`.
> ![Autoriser plusieurs restrictions](./assets/metadata-driven-permissions/allow-multiple-aces.png)
