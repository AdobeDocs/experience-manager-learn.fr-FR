---
title: Intégration des objets Worker Asset compute aux profils de traitement AEM
description: AEM as a Cloud Service s’intègre aux objets Worker Asset compute déployés vers Adobe I/O Runtime via les profils de traitement AEM Assets. Les profils de traitement sont configurés dans le service Auteur pour traiter des ressources spécifiques à l’aide de programmes de travail personnalisés et pour stocker les fichiers générés par les programmes de travail en tant que rendus de ressources.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 1b398c8c-6b4e-4046-b61e-b44c45f973ef
source-git-commit: ad203d7a34f5eff7de4768131c9b4ebae261da93
workflow-type: tm+mt
source-wordcount: '630'
ht-degree: 2%

---

# Intégration aux profils de traitement AEM

Pour que les agents d’Asset compute puissent générer des rendus personnalisés dans AEM en tant que Cloud Service, ils doivent être enregistrés dans AEM en tant que service d’auteur de Cloud Service via les profils de traitement. Le programme de travail sera appelé lors du chargement ou du retraitement de toutes les ressources soumises à ce profil de traitement et le rendu personnalisé sera généré et rendu disponible via les rendus de la ressource.

## Définition d’un profil de traitement

Créez tout d’abord un profil de traitement qui appelle le programme de travail avec les paramètres configurables.

![Profil de traitement](./assets/processing-profiles/new-processing-profile.png)

1. Connectez-vous à AEM en tant que service d’auteur de Cloud Service en tant qu’ __AEM administrateur__. Comme il s’agit d’un tutoriel, nous vous recommandons d’utiliser un environnement de développement ou un environnement dans un environnement de test.
1. Accédez à __Outils > Ressources > Profils de traitement__
1. Appuyez sur le bouton __Créer__
1. Nommez le profil de traitement `WKND Asset Renditions`
1. Appuyez sur l’onglet __Personnalisé__, puis sur __Ajouter nouveau__.
1. Définir le nouveau service
   + __Nom du rendu:__ `Circle`
      + Rendu du nom de fichier qui sera utilisé pour identifier ce rendu dans AEM Assets
   + __Extension :__ `png`
      + Extension du rendu qui sera généré. Définissez cette variable sur `png`, car il s’agit du format de sortie pris en charge par le service Web du programme de travail, qui produit un arrière-plan transparent derrière la découpe du cercle.
   + __Point d’entrée :__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Il s’agit de l’URL du programme de travail obtenu via `aio app get-url`. Assurez-vous que l’URL pointe vers l’espace de travail correct en fonction de l’environnement AEM as a Cloud Service.
      + Assurez-vous que l’URL du programme de travail pointe vers l’espace de travail approprié. AEM as a Cloud Service Stage doit utiliser l’URL de l’espace de travail intermédiaire et AEM as a Cloud Service Production doit utiliser l’URL de l’espace de travail de production.
   + __Paramètres de service__
      + Appuyez sur __Ajouter un paramètre__.
         + Clé: `size`
         + Valeur : `1000`
      + Appuyez sur __Ajouter un paramètre__.
         + Clé: `contrast`
         + Valeur : `0.25`
      + Appuyez sur __Ajouter un paramètre__.
         + Clé: `brightness`
         + Valeur : `0.10`
      + Ces paires clé/valeur qui sont transmises au programme de travail de l’Asset compute et disponibles via l’objet JavaScript `rendition.instructions`.
   + __Types MIME__
      + __Inclut :__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Ces types MIME sont les seuls à être utilisés pour les modules npm du programme de travail. Cette liste limite les ressources qui seront traitées par le programme de travail personnalisé.
      + __Exclut :__ `Leave blank`
         + Ne jamais traiter les ressources avec ces types MIME à l’aide de cette configuration de service. Dans ce cas, nous n&#39;utilisons qu&#39;une liste autorisée.
1. Appuyez sur __Enregistrer__ en haut à droite.

## Application et appel d’un profil de traitement

1. Sélectionnez le profil de traitement que vous venez de créer, `WKND Asset Renditions`
1. Appuyez sur __Appliquer le profil au(x) dossier(s)__ dans la barre d’actions supérieure.
1. Sélectionnez un dossier auquel appliquer le profil de traitement, par exemple `WKND` et appuyez sur __Appliquer__.
1. Accédez au dossier auquel le profil de traitement n’a pas été appliqué via __AEM > Ressources > Fichiers__ et appuyez sur `WKND`.
1. Téléchargez de nouvelles ressources d’images ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) et [sample-3.jpg](../assets/samples/sample-3.jpg)) dans n’importe quel dossier sous lequel le profil de traitement est appliqué, et attendez que la ressource téléchargée soit traitée.
1. Appuyez sur la ressource pour ouvrir ses détails.
   + Les rendus par défaut peuvent être générés et affichés plus rapidement dans AEM que dans les rendus personnalisés.
1. Ouvrez la vue __Rendus__ dans la barre latérale gauche.
1. Appuyez sur la ressource nommée `Circle.png` et passez en revue le rendu généré.

   ![Rendu généré](./assets/processing-profiles/rendition.png)

## Terminé!

Félicitations ! Vous avez terminé le [tutoriel](../overview.md) sur la manière d’étendre AEM en tant que microservices d’Asset compute Cloud Service ! Vous devriez maintenant pouvoir configurer, développer, tester, déboguer et déployer des objets Worker d’Asset compute personnalisés à utiliser par votre AEM en tant que service de création de Cloud Service.

### Examinez le code source complet du projet sur Github.

Le projet d’Asset compute final est disponible sur Github à l’adresse :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contient l’état final du projet, entièrement renseigné avec les cas de travail et de test, mais ne contient aucune information d’identification, c’est-à-dire. `.env`, `.config.json` ou `.aio`._

## Résolution des problèmes

+ [Rendu personnalisé manquant dans la ressource dans AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Échec du traitement des ressources dans AEM](../troubleshooting.md#asset-processing-fails)
