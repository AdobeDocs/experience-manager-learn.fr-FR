---
title: Prévisualiser une extension de l’éditeur universel
description: Découvrez comment prévisualiser une extension de l’éditeur universel exécutée localement pendant le développement.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner, Intermediate, Experienced
doc-type: Tutorial
jira: KT-18658
exl-id: 27b6626d-b291-4b88-9350-a32cdbd0cf63
source-git-commit: 88b8808c4f8e767578d04ecf3e7c05b37a9e5dcf
workflow-type: ht
source-wordcount: '306'
ht-degree: 100%

---

# Prévisualiser une extension locale de l’éditeur universel

>[!TIP]
> Découvrez comment [créer une extension de l’éditeur universel](https://developer.adobe.com/uix/docs/services/aem-universal-editor/).

Pour prévisualiser une extension de l’éditeur universel pendant le développement, vous devez :

1. Exécuter l’extension localement.
2. Accepter le certificat auto-signé.
3. Ouvrir une page dans l’éditeur universel.
4. Mettre à jour l’URL de l’emplacement pour charger l’extension locale.

## Exécuter l’extension localement

Cela implique que vous avez déjà créé une [extension de l’éditeur universel](https://developer.adobe.com/uix/docs/services/aem-universal-editor/) et que vous souhaitez la prévisualiser pendant les tests et le développement en local.

Démarrez votre extension de l’éditeur universel avec :

```bash
$ aio app run
```

Vous verrez une sortie du type :

```
To view your local application:
  -> https://localhost:9080
To view your deployed application in the Experience Cloud shell:
  -> https://experience.adobe.com/?devMode=true#/custom-apps/?localDevUrl=https://localhost:9080
```

Votre extension est exécutée à l’adresse `https://localhost:9080` par défaut.


## Accepter le certificat auto-signé

L’éditeur universel requiert HTTPS pour charger les extensions. Étant donné que le développement local utilise un certificat auto-signé, votre navigateur doit l’approuver explicitement.

Ouvrez un nouvel onglet de navigateur et accédez à la sortie de l’URL de l’extension locale à l’aide de la commande `aio app run` :

```
https://localhost:9080
```

Votre navigateur affiche un avertissement de certificat. Acceptez le certificat pour continuer.

![Acceptation du certificat auto-signé](./assets/local-extension-preview/accept-certificate.png)

Une fois accepté, la page d’espace réservé de l’extension locale s’affiche :

![L’extension est accessible](./assets/local-extension-preview/extension-accessible.png)


## Ouvrir une page dans l’éditeur universel

Ouvrez l’éditeur universel via la [console de l’éditeur universel](https://experience.adobe.com/#/@myOrg/aem/editor/canvas/) ou en modifiant une page dans AEM Sites qui utilise l’éditeur universel :

![Ouverture d’une page dans l’éditeur universel](./assets/local-extension-preview/open-page-in-ue.png)


## Charger l’extension

Dans l’éditeur universel, recherchez le champ **Emplacement** au milieu en haut de l’interface. Développez-le et mettez à jour l’**URL dans le champ Emplacement**, **et non dans la barre d’adresse du navigateur**.

Ajoutez les paramètres de requête suivants :

* `devMode=true` : active le mode de développement pour l’éditeur universel.
* `ext=https://localhost:9080` : charge votre extension exécutée localement.

Exemple :

```
https://author-pXXX-eXXX.adobeaemcloud.com/content/aem-ue-wknd/index.html?devMode=true&ext=https://localhost:9080
```

![Mise à jour de l’URL de l’emplacement de l’éditeur universel](./assets/local-extension-preview/update-location-url.png)


## Prévisualiser l’extension

**Chargez à nouveau** le navigateur pour vous assurer que l’URL mise à jour est utilisée.

L’éditeur universel charge désormais votre extension locale, dans votre session de navigateur uniquement.

Toutes les modifications de code que vous apportez localement sont répercutées immédiatement.

![Extension locale chargée](./assets/local-extension-preview/extension-loaded.png)
