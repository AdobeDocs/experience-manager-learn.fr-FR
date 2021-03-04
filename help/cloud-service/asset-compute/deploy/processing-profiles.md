---
title: Intégrer les employés d'Asset compute aux Profils de traitement AEM
description: AEM en tant que Cloud Service s’intègre aux travailleurs d’Asset compute déployés à Adobe I/O Runtime par le biais de Profils de traitement AEM Assets. Les Profils de traitement sont configurés dans le service Auteur pour traiter des ressources spécifiques à l’aide de travailleurs personnalisés et pour stocker les fichiers générés par les travailleurs en tant que rendus de ressources.
feature: Microservices Asset compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6287
thumbnail: KT-6287.jpg
topic: Intégrations, développement
role: Développeur
level: Intermédiaire, expérimenté
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '638'
ht-degree: 2%

---


# Intégration des Profils de traitement AEM

Pour que les travailleurs d’Asset compute puissent générer des rendus personnalisés en AEM en tant que Cloud Service, ils doivent être enregistrés en AEM en tant que service d’auteur Cloud Service via les Profils de traitement. Tous les actifs soumis à ce Profil de traitement font appel au programme de travail lors du transfert ou du retraitement, et le rendu personnalisé est généré et rendu disponible via les rendus de l’actif.

## Définir un Profil de traitement

Créez d’abord un nouveau Profil de traitement qui appellera le programme de travail avec les paramètres configurables.

![Profil de traitement](./assets/processing-profiles/new-processing-profile.png)

1. Connectez-vous à AEM en tant que service d’auteur Cloud Service en tant qu’__AEM Administrator__. Comme il s’agit d’un tutoriel, nous vous recommandons d’utiliser un environnement Dev ou un environnement dans un Sandbox.
1. Accédez à __Outils > Ressources > Profils de traitement__
1. Appuyez sur le bouton __Créer__
1. Nommez le Profil de traitement `WKND Asset Renditions`.
1. Appuyez sur l’onglet __Personnalisé__, puis sur __Ajouter nouveau__.
1. Définir le nouveau service
   + __Nom du rendu:__ `Circle`
      + Le rendu du nom de fichier qui sera utilisé pour identifier ce rendu en AEM Assets
   + __Extension :__ `png`
      + Extension du rendu qui sera généré. Définissez cette valeur sur `png`, car il s’agit du format de sortie pris en charge par le service Web du collaborateur, et le résultat est un arrière-plan transparent derrière le cercle découpé.
   + __Point de terminaison :__ `https://...adobeioruntime.net/api/v1/web/wkndAemAssetCompute-0.0.1/worker`
      + Il s&#39;agit de l&#39;URL du travailleur obtenu via `aio app get-url`. Assurez-vous que l’URL pointe vers l’espace de travail approprié en fonction de l’AEM en tant qu’environnement Cloud Service.
      + Assurez-vous que l’URL du collaborateur pointe vers l’espace de travail approprié. AEM en tant que scène de Cloud Service doit utiliser l’URL de l’espace de travail de scène et AEM en tant que production de Cloud Service doit utiliser l’URL de l’espace de travail de production.
   + __Paramètres de service__
      + Appuyez sur __Paramètre d&#39;Ajoute__.
         + Clé: `size`
         + Valeur : `1000`
      + Appuyez sur __Paramètre d&#39;Ajoute__.
         + Clé: `contrast`
         + Valeur : `0.25`
      + Appuyez sur __Paramètre d&#39;Ajoute__.
         + Clé: `brightness`
         + Valeur : `0.10`
      + Ces paires clé/valeur qui sont transmises au programme de travail de l’Asset compute et disponibles via l’objet JavaScript `rendition.instructions`.
   + __Types MIME__
      + __Inclut:__ `image/jpeg`,  `image/png`,  `image/gif`,  `image/bmp`,  `image/tiff`
         + Ces types MIME sont les seuls modules npm du travailleur. Cette liste limite les ressources qui seront traitées par le programme de travail personnalisé.
      + __Exclure :__ `Leave blank`
         + Ne traitez jamais les ressources avec ces types MIME à l’aide de cette configuration de service. Dans ce cas, nous n&#39;utilisons qu&#39;une liste autorisée.
1. Appuyez sur __Enregistrer__ en haut à droite.

## Appliquer et appeler un Profil de traitement

1. Sélectionnez le nouveau Profil de traitement, `WKND Asset Renditions`
1. Appuyez sur __Appliquer le Profil au dossier(s)__ dans la barre d’actions supérieure.
1. Sélectionnez un dossier auquel appliquer le Profil de traitement, par exemple `WKND`, puis appuyez sur __Appliquer__.
1. Accédez au dossier auquel le Profil de traitement n’a pas été appliqué par __AEM > Ressources > Fichiers__ et appuyez sur `WKND`.
1. Téléchargez de nouveaux fichiers d’images ([sample-1.jpg](../assets/samples/sample-1.jpg), [sample-2.jpg](../assets/samples/sample-2.jpg) et [sample-3.jpg](../assets/samples/sample-3.jpg)) dans n’importe quel dossier situé sous le dossier où le Profil de traitement est appliqué et attendez que la ressource téléchargée soit traitée.
1. Appuyez sur la ressource pour ouvrir ses détails
   + Les rendus par défaut peuvent générer et apparaître plus rapidement dans les AEM que les rendus personnalisés.
1. Ouvrez la vue __Rendus__ à partir de la barre latérale gauche.
1. Appuyez sur la ressource `Circle.png` et passez en revue le rendu généré.

   ![Rendu généré](./assets/processing-profiles/rendition.png)

## Terminé!

Félicitations ! Vous avez terminé le [tutoriel](../overview.md) sur la façon d&#39;étendre l&#39;AEM en tant que microservices d&#39;Asset compute Cloud Service ! Vous devez désormais être en mesure de configurer, de développer, de tester, de déboguer et de déployer des agents d’Asset compute personnalisés pour une utilisation par votre AEM en tant que service d’auteur Cloud Service.

### Examiner le code source complet du projet sur Github

Le projet d&#39;Asset compute final est disponible sur Github à l&#39;adresse suivante :

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_Github contient l&#39;état final du projet, entièrement renseigné avec les cas de travail et de test, mais ne contient aucune information d&#39;identification, c&#39;est-à-dire. `.env`, `.config.json` ou `.aio`._

## Résolution des incidents

+ [Rendu personnalisé absent de la ressource dans AEM](../troubleshooting.md#custom-rendition-missing-from-asset)
+ [Échec du traitement des ressources dans AEM](../troubleshooting.md#asset-processing-fails)
