---
title: Intégrer des programmes de travail Asset Compute aux profils de traitement AEM
description: AEM as a Cloud Service s’intègre aux programmes de travail Asset Compute déployés vers Adobe I/O Runtime via les profils de traitement AEM Assets. Les profils de traitement sont configurés dans le service de création pour traiter des ressources spécifiques à l’aide de programmes de travail personnalisés et pour stocker les fichiers générés par les programmes de travail en tant que rendus de ressources.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
duration: 150
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '622'
ht-degree: 100%

---

# Intégrer aux profils de traitement AEM

Pour que les programmes de travail Asset Compute puissent générer des rendus personnalisés dans AEM as a Cloud Service, ils doivent être enregistrés dans le service de création d’AEM as a Cloud Service via les profils de traitement. Pour toutes les ressources soumises à ce profil de traitement, le programme de travail sera appelé lors du chargement ou du retraitement et le rendu personnalisé sera généré et mis à disposition via les rendus de la ressource.

## Créer un profil de traitement

Créez d’abord un profil de traitement qui appelle le programme de travail avec les paramètres configurables.

![Profil de traitement.](./assets/processing-profiles/new-processing-profile.png)

1. Connectez-vous au service de création d’AEM as a Cloud Service en tant qu’__Administrateur ou administratrice AEM__. Comme il s’agit d’un tutoriel, nous vous recommandons d’utiliser un environnement de développement ou un environnement dans une sandbox.
1. Accédez à __Outils > Ressources > Profils de traitement__.
1. Appuyez sur le bouton __Créer__.
1. Nommez le profil de traitement, `WKND Asset Renditions`.
1. Appuyez sur l’onglet __Personnalisé__ et appuyez sur __Ajouter nouveau__.
1. Définir le nouveau service
   + __Nom du rendu :__ `Circle`
      + Nom de fichier de rendu utilisé pour identifier ce rendu dans AEM Assets
   + __Extension :__ `png`
      + Extension du rendu généré. Définissez sur `png`, car il s’agit du format de sortie pris en charge par le service web du programme de travail, qui produit un arrière-plan transparent derrière la découpe de cercle.
   + __Point d’entrée :__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Il s’agit de l’URL du programme de travail obtenue via `aio app get-url`. Assurez-vous que l’URL pointe vers le bon espace de travail en fonction de l’environnement AEM as a Cloud Service.
      + Assurez-vous que l’URL du programme de travail pointe versle bon espace de travail. L’instance d’évaluation d’AEM as a Cloud Service doit utiliser l’URL de l’espace de travail d’évaluation et l’instance de production d’AEM as a Cloud Service doit utiliser l’URL de l’espace de travail de production.
   + __Paramètres de service__
      + Appuyez sur __Ajouter un paramètre__.
         + Clé : `size`
         + Valeur : `1000`
      + Appuyez sur __Ajouter un paramètre__.
         + Clé : `contrast`
         + Valeur : `0.25`
      + Appuyez sur __Ajouter un paramètre__.
         + Clé : `brightness`
         + Valeur : `0.10`
      + Ces paires clé/valeur sont transmises au programme de travail Asset Compute et disponibles via l’objet JavaScript `rendition.instructions`.
   + __Types MIME__
      + __Inclut :__ `image/jpeg`, `image/png`, `image/gif`, `image/bmp`, `image/tiff`
         + Ces types MIME sont les seuls à être utilisés pour les modules npm du programme de travail. Cette liste limite ceux qui sont traités par le programme de travail personnalisé.
      + __Exclut :__ `Leave blank`
         + N’utilisez jamais cette configuration de service pour traiter les ressources avec ces types MIME. Dans ce cas, nous utilisons uniquement une liste autorisée.
1. Appuyz sur __Enregistrer__ en haut à droite.

## Appliquer et appeler un profil de traitement

1. Sélectionnez le profil de traitement nouvellement créé, `WKND Asset Renditions`.
1. Appuyez sur __Appliquer le profil au(x) dossier(s)__ dans la barre d’actions supérieure
1. Sélectionnez un dossier auquel appliquer le profil de traitement, par exemple `WKND`, et appuyez sur __Appliquer__.
1. Accédez au dossier auquel le profil de traitement n’a pas été appliqué via __AEM > Ressources > Fichiers__ et appuyez sur `WKND`.
1. Chargez certaines nouvelles ressources d’images ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) et [sample-3.jpg](../assets/samples/sample-3.jpg)) dans n’importe quel dossier situé sous le dossier auquel est appliqué le profil de traitement, et attendez que la ressource chargée soit traitée.
1. Appuyez sur la ressource pour ouvrir ses détails.
   + Les rendus par défaut peuvent être générés et affichés plus rapidement dans AEM que dans les rendus personnalisés.
1. Ouvrez la vue __Rendus__ depuis la barre latérale gauche.
1. Appuyez sur la ressource nommée `Circle.png` et consultez le rendu généré.

   ![Rendu généré.](./assets/processing-profiles/rendition.png)

## Terminé.

Félicitations. Vous avez terminé le [tutoriel](../overview.md) sur la manière d’étendre les microservices d’Asset Compute d’AEM as a Cloud Service. Vous pouvez maintenant configurer, développer, tester, déboguer et déployer des programmes de travail d’Asset Compute personnalisés à utiliser par votre service de création d’AEM as a Cloud Service.

### Examinez le code source complet du projet sur GitHub.

Le projet Asset Compute final est disponible sur GitHub à l’adresse :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_GitHub contient le projet dans son état final, entièrement renseigné avec les cas de test et les programmes de travail, mais ne contient aucune information d’identification, c’est-à-dire `.env`, `.config.json` ou `.aio`._

## Résolution des problèmes

+ [Rendu personnalisé manquant dans la ressource dans AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Échec du traitement des ressources dans AEM](../troubleshooting.md#asset-processing-fails)
