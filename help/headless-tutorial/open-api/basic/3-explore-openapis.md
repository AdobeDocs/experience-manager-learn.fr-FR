---
title: Explorer la documentation OpenAPI | AEM Headless Partie 3
description: Prise en main de Adobe Experience Manager (AEM) et OpenAPI. Explorez les API de diffusion de fragments de contenu basées sur OpenAPI d’AEM à l’aide de la documentation d’API intégrée. Découvrez comment AEM génère automatiquement des schémas OpenAPI en fonction de modèles de fragment de contenu. Testez la création de requêtes de base à l’aide de la documentation de l’API.
version: Experience Manager as a Cloud Service
mini-toc-levels: 1
feature: Content Fragments
topic: Headless, Content Management
role: Developer
level: Beginner
duration: 400
source-git-commit: c6213dd318ec4865375c57143af40dbe3f3990b1
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 0%

---


# Explorer les API de diffusion de fragments de contenu basées sur OpenAPI AEM

La [diffusion de fragments de contenu AEM avec des API OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) dans AEM offre un moyen puissant de diffuser du contenu structuré à n’importe quelle application ou canal. Dans ce chapitre, nous explorons la manière d’utiliser les API OpenAPI pour récupérer des fragments de contenu via la fonctionnalité **Essayer** de la documentation.

## Prérequis {#prerequisites}

Il s’agit d’un tutoriel en plusieurs parties qui suppose que les étapes décrites dans la section [Création de fragments de contenu](./2-author-content-fragments.md) ont été terminées.

Veillez à disposer des éléments suivants :

* Le nom d’hôte du service de publication AEM (par exemple, `https://publish-<PROGRAM_ID>-e<ENVIRONMENT_ID >.adobeaemcloud.com/`) vers lequel [les fragments de contenu sont publiés](./2-author-content-fragments.md#publish-content-fragments). Si vous publiez un service d’aperçu AEM, indiquez ce nom d’hôte (par exemple, `https://preview-<PROGRAM_ID>-e<ENVIRONMENT_ID>.adobeaemcloud.com/`).

## Objectifs {#objectives}

* Familiarisez-vous avec la diffusion de fragments de contenu [AEM avec les API OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/).
* Appelez les API à l’aide de la fonctionnalité **Essayer** des documents d’API.

## API de diffusion

La diffusion de fragments de contenu AEM avec les API OpenAPI fournissent une interface RESTful pour récupérer les fragments de contenu. Les API décrites dans ce tutoriel sont disponibles uniquement sur les services de publication et de prévisualisation d’AEM, et non sur le service de création. Il existe d’autres OpenAPI pour [interagir avec des fragments de contenu sur le service de création AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/sites/).

## Explorer les API

[La documentation Diffusion de fragments de contenu AEM avec les API OpenAPI](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) dispose d’une fonctionnalité « À essayer » qui vous permet d’explorer les API et de les tester directement depuis le navigateur. Il s’agit d’un excellent moyen de vous familiariser avec les points d’entrée d’API et leurs fonctionnalités.

Ouvrez la [documentation de l’API AEM Sites](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) dans votre navigateur.

Les API sont répertoriées dans le volet de navigation de gauche sous la section **Diffusion de fragments**. Vous pouvez développer cette section pour afficher les API disponibles. La sélection d’une API affiche les détails de l’API dans le panneau principal, ainsi qu’une section **Essayer** dans le rail droit, qui vous permet de tester et d’explorer l’API directement depuis le navigateur.

![Documentation sur la diffusion de fragments de contenu AEM avec les API OpenAPI](./assets/3/docs-overview.png)

## Liste des fragments de contenu

1. Ouvrez la [documentation sur la diffusion de fragments de contenu AEM avec OpenAPI destinée aux développeurs](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/stable/contentfragments/delivery/) dans votre navigateur.
1. Dans le volet de navigation de gauche, développez la section **Diffusion de fragments** et sélectionnez l’API **Répertorier tous les fragments de contenu**

Cette API vous permet de récupérer une liste paginée de tous les fragments de contenu d’AEM par dossier. Le moyen le plus simple d’utiliser cette API consiste à fournir le chemin d’accès au dossier contenant les fragments de contenu.

1. Sélectionnez **Essayer** dans la partie supérieure du rail de droite.
1. Saisissez l’identifiant du service AEM auquel l’API se connectera pour récupérer les fragments de contenu. Le compartiment est la première partie de l’URL du service de publication (ou de prévisualisation) AEM, généralement au format `publish-p<PROGRAM_ID>-e<ENVIRONMENT_ID>` ou `preview-p<PROGRAM_ID>-e<ENVIRONMENT_ID>`.

Puisque nous utilisons le service de publication AEM, définissez le compartiment sur l’identifiant du service de publication AEM. Par exemple :

* **compartiment** : `publish-p138003-e1400351`

![Compartiment Essayer](./assets/3/try-it-bucket.png)

Lorsque le compartiment est défini, le champ **Serveur Target** est automatiquement mis à jour vers l’URL d’API complète du service de publication AEM, par exemple : `https://publish-p138003-e1400351.adobeaemcloud.com/adobe/contentFragments`

1. Développez la section **Sécurité** et définissez **Schéma de sécurité** sur **Aucun**. En effet, le service de publication AEM (et le service de prévisualisation) ne nécessite pas d’authentification pour la diffusion de fragments de contenu AEM avec les API OpenAPI.

1. Développez la section **Paramètres** pour fournir les détails du fragment de contenu à obtenir.

* **cursor** : laissez ce champ vide. Il est utilisé pour la pagination et il s’agit d’une demande initiale.
* **limit** : laissez ce champ vide. Il est utilisé pour limiter le nombre de résultats renvoyés par page de résultats.
* **path** : `/content/dam/my-project/en`

  >[!TIP]
  > Lors de la saisie d’un chemin d’accès, assurez-vous que son préfixe est `/content/dam/` et ne se termine **pas** par une barre `/`.

  ![Paramètres d’essai](assets/3/try-it-parameters.png)

1. Sélectionnez le bouton **Envoyer** pour exécuter l’appel API.
1. Sous l’onglet **Réponse** du panneau **Essayer**, vous devriez voir une réponse JSON contenant une liste de fragments de contenu dans le dossier spécifié. La réponse ressemblera à ce qui suit :

   ![Essayer la réponse](./assets/3/try-it-response.png)

1. La réponse contient tous les fragments de contenu sous le dossier `path` du paramètre `/content/dam/my-project`, y compris les sous-dossiers, y compris les fragments de contenu **Personne** et **Équipe**.
1. Cliquez sur le tableau de `items` et recherchez la valeur de `Team Alpha` de l’élément de `id`. L’identifiant est utilisé dans la section suivante pour récupérer les détails d’un fragment de contenu unique.
1. Sélectionnez **Modifier la requête** en haut du panneau **L’essayer** et les différents paramètres de l’appel API pour voir comment la réponse change. Par exemple, vous pouvez modifier le chemin d’accès vers un autre dossier contenant des fragments de contenu ou vous pouvez ajouter des paramètres de requête pour filtrer les résultats. Par exemple, définissez `path` paramètre sur `/content/dam/my-project/teams` uniquement aux fragments de contenu de ce dossier (et de ses sous-dossiers).

## Obtenir les détails du fragment de contenu

Tout comme l’API **Répertorier tous les fragments de contenu**, l’API **Obtenir un fragment de contenu** récupère un fragment de contenu unique par son identifiant, ainsi que toutes les références facultatives. Pour explorer cette API, nous demanderons le fragment de contenu d’équipe qui fait référence à plusieurs fragments de contenu de personne.

1. Développez la section **Diffusion de fragment** dans le rail de gauche, puis sélectionnez l’API **Obtenir un fragment de contenu**.
1. Sélectionnez **Essayer** dans la partie supérieure du rail de droite.
1. Vérifiez que le `bucket` pointe vers votre service de publication ou d’aperçu AEM as a Cloud Service.
1. Développez la section **Sécurité** et définissez **Schéma de sécurité** sur **Aucun**. En effet, le service de publication AEM ne nécessite pas d’authentification pour la diffusion de fragments de contenu AEM avec les API OpenAPI.
1. Développez la section **Paramètres** pour fournir les détails du fragment de contenu à obtenir :

Dans cet exemple, utilisez l’identifiant du fragment de contenu d’équipe récupéré dans la section précédente. Par exemple, pour cette réponse de fragment de contenu dans **Répertorier tous les fragments de contenu**, utilisez la valeur dans le champ `id` de `b954923a-0368-4fa2-93ea-2845f599f512`. (Votre `id` sera différente de la valeur utilisée dans le tutoriel.)

```json
{
    "path": "/content/dam/my-project/teams/team-alpha",
    "name": "",
    "title": "Team Alpha",
    "id": "50f28a14-fec7-4783-a18f-2ce2dc017f55", // This is the Content Fragment ID
    "description": "",
    "model": {},
    "fields": {} 
}
```

* **fragmentId** : `50f28a14-fec7-4783-a18f-2ce2dc017f55`
* **références** : `none`
* **depth** : laissez ce champ vide. Le paramètre **references** détermine la profondeur des fragments référencés.
* **hydraté** : laissez ce champ vide. Le paramètre **references** va dicter l&#39;hydratation des fragments référencés.
* **If-None-Match** : laisser vide

1. Sélectionnez le bouton **Envoyer** pour exécuter l’appel API.
1. Examinez la réponse dans l’onglet **Réponse** du panneau **Essayer**. Vous devriez voir une réponse JSON contenant les détails du fragment de contenu, y compris ses propriétés et toutes les références dont il dispose.
1. Sélectionnez **Modifier la requête** dans la partie supérieure du panneau **Essayer** et dans les sections **Paramètres**, définissez le paramètre `references` sur `all-hydrated`, de sorte que tout le contenu du fragment de contenu référencé soit inclus dans l’appel API.

   * **fragmentId** : `50f28a14-fec7-4783-a18f-2ce2dc017f55`
   * **références** : `all-hydrated`
   * **depth** : laissez ce champ vide. Le paramètre **references** détermine la profondeur des fragments référencés.
   * **hydraté** : laissez ce champ vide. Le paramètre **references** va dicter l&#39;hydratation des fragments référencés.
   * **If-None-Match** : laisser vide

1. Sélectionnez le bouton **Renvoyer** pour exécuter à nouveau l’appel API.
1. Examinez la réponse dans l’onglet **Réponse** du panneau **Essayer**. Vous devriez voir une réponse JSON contenant les détails du fragment de contenu, y compris ses propriétés et celles des fragments de contenu de personne référencés.

Notez que le tableau de `teamMembers` comprend désormais les détails des fragments de contenu de personne référencés. L’hydratation des références vous permet de récupérer toutes les données nécessaires dans un seul appel API, ce qui est particulièrement utile pour réduire le nombre de requêtes effectuées par les applications clientes.

## Félicitations.

Félicitations, vous avez créé et exécuté plusieurs diffusions de fragments de contenu AEM avec des appels d’API OpenAPI à l’aide de la fonctionnalité **Essayer** de la documentation AEM.

## Étapes suivantes

Dans le chapitre suivant, [Créer une application React](./4-react-app.md), vous explorez comment une application externe peut interagir avec la diffusion de fragments de contenu AEM à l’aide des API OpenAPI.

