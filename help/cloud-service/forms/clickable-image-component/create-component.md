---
title: Création d’un composant Image cliquable
description: Création d’un composant d’image cliquable dans AEM Forms Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 1%

---

# Créer un composant

Cet article suppose que vous avez une certaine expérience du développement pour AEM Forms CS. Il est également supposé que vous avez créé un projet d’archétype AEM Forms.

Ouvrez votre projet AEM Forms dans IntelliJ ou tout autre IDE de votre choix. Créez un noeud appelé svg sous

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent`` est l’appId fourni lors de la création du projet Maven. Cet appId peut être différent dans votre environnement.


## Créer un fichier .content.xml

Créez un fichier nommé .content.xml sous le noeud svg . Ajoutez le contenu suivant au fichier nouvellement créé. Vous pouvez modifier jcr:description, jcr:title et componentGroup selon vos besoins.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## Création de svg.html

Créez un fichier appelé svg.html. Ce fichier va rendre le SVG de USA map.Copiez le contenu de [svg.html](assets/svg.html) dans le fichier nouvellement créé. Ce que vous avez copié est la carte du SVG des États-Unis. Enregistrez le fichier.

## Déploiement du projet

Déployez le projet sur votre instance locale prête pour le cloud afin de tester le composant.

Pour déployer le projet, vous devez accéder au dossier racine du projet dans la fenêtre d’invite de commande et exécuter la commande suivante.

```
mvn clean install -PautoInstallSinglePackage
```

Le projet sera alors déployé sur votre instance AEM Forms locale et le composant pourra être inclus dans votre formulaire adaptatif.

![usa-map](./assets/usa-map.png)