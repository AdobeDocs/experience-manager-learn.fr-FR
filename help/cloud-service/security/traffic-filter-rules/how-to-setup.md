---
title: Comment configurer des règles de filtrage du trafic, y compris des règles WAF
description: Découvrez comment configurer pour créer, déployer, tester et analyser les résultats des règles de filtrage du trafic, y compris les règles WAF.
version: Cloud Service
feature: Security
topic: Security, Administration, Architecture
role: Admin, Architect
level: Intermediate
doc-type: Tutorial
last-substantial-update: 2023-10-20T00:00:00Z
jira: KT-13148
thumbnail: KT-13148.jpeg
source-git-commit: bca52c7543b35fc20a782dfd3f2b2dc81bee4cde
workflow-type: tm+mt
source-wordcount: '563'
ht-degree: 3%

---


# Comment configurer des règles de filtrage du trafic, y compris des règles WAF

Formation **configuration** Règles de filtre de trafic, y compris les règles WAF. Découvrez comment créer, déployer, tester et analyser les résultats.

## Configuration

Le processus de configuration comprend les éléments suivants :

- _création de règles_ avec une structure AEM projet et un fichier de configuration appropriés.
- _règles de déploiement_ à l’aide du pipeline de configuration d’Adobe Cloud Manager.
- _règles de test_ utilisation de divers outils pour générer le trafic
- _analyse des résultats_ à l’aide des journaux de réseau de diffusion de contenu AEM et de l’outil de tableau de bord.

### Création de règles dans votre projet AEM

Pour créer des règles, procédez comme suit :

1. Au niveau supérieur de votre projet AEM, créez un dossier . `config`.

1. Dans le `config` créer un dossier appelé `cdn.yaml`.

1. Ajoutez les métadonnées suivantes au `cdn.yaml` fichier :

```yaml
kind: CDN
version: '1'
metadata:
  envTypes:
    - dev
    - stage
    - prod
data:
  trafficFilters:
    rules:
```

Voir un exemple de la fonction `cdn.yaml` dans le projet AEM Guides WKND Sites :

![Fichier et dossier des règles de projet AEM WKND](./assets/wknd-rules-file-and-folder.png)

### Déploiement de règles via Cloud Manager {#deploy-rules-through-cloud-manager}

Pour déployer des règles, procédez comme suit :

1. Connectez-vous à Cloud Manager à l’adresse [my.cloudmanager.adobe.com](https://my.cloudmanager.adobe.com/) et sélectionnez l’organisation et le programme appropriés.

1. Accédez au _Pipelines_ de la carte _Aperçu du programme_ et cliquez sur le bouton **+Ajouter** et sélectionnez le type de pipeline souhaité.

   ![Carte Pipelines de Cloud Manager](./assets/cloud-manager-pipelines-card.png)

   Dans l’exemple ci-dessus, à des fins de démonstration _Ajout d’un pipeline hors production_ est sélectionné, car un environnement de développement est utilisé.

1. Dans le _Ajout d’un pipeline hors production_ , sélectionnez et saisissez les détails suivants :

   1. Étape de configuration :

      - **Type**: pipeline de déploiement
      - **Nom du pipeline**: Dev-Config

      ![Boîte de dialogue de pipeline de configuration de Cloud Manager](./assets/cloud-manager-config-pipeline-step1-dialog.png)

   2. Étape Code source :

      - **Code à déployer**: déploiement ciblé
      - **Inclure**: Config
      - **Environnement de déploiement**: nom de votre environnement, par exemple wknd-program-dev.
      - **Référentiel**: référentiel Git à partir duquel le pipeline doit récupérer le code ; par exemple, `wknd-site`
      - **Branche Git**: nom de la branche du référentiel Git.
      - **Emplacement du code**: `/config`, correspondant au dossier de configuration de niveau supérieur créé à l’étape précédente.

      ![Boîte de dialogue de pipeline de configuration de Cloud Manager](./assets/cloud-manager-config-pipeline-step2-dialog.png)

### Test de règles en générant du trafic

Pour tester des règles, divers outils tiers sont disponibles et votre entreprise peut disposer d’un outil préféré. À des fins de démonstration, nous allons utiliser les outils suivants :

- [Curl](https://curl.se/) pour les tests de base, comme appeler une URL et vérifier le code de réponse.

- [Vegeta](https://github.com/tsenart/vegeta) pour effectuer un déni de service (DOS). Suivez les instructions d’installation de la section [Vegeta GitHub](https://github.com/tsenart/vegeta#install).

- [Nikto](https://github.com/sullo/nikto/wiki) pour rechercher des problèmes potentiels et des vulnérabilités de sécurité telles que XSS, l’injection SQL, etc. Suivez les instructions d’installation de la section [Nikto GitHub](https://github.com/sullo/nikto).

- Vérifiez que les outils sont installés et disponibles dans votre terminal en exécutant les commandes ci-dessous :

  ```shell
  # Curl version check
  $ curl --version
  
  # Vegeta version check
  $ vegeta -version
  
  # Nikto version check
  $ cd <PATH-OF-CLONED-REPO>/program
  ./nikto.pl -Version
  ```

### Analyse des résultats à l’aide de l’outil de tableau de bord

Après avoir créé, déployé et testé les règles, vous pouvez analyser les résultats à l’aide de la fonction **Elasticsearch, Logstash et Kibana (ELK)** Outils du tableau de bord. Il peut analyser les journaux de réseau de diffusion de contenu AEMCS, ce qui vous permet de visualiser les résultats sous la forme de plusieurs graphiques.

Les outils des tableaux de bord peuvent être clonés directement à partir de la [Référentiel GitHub AEMCS-CDN-Log-Analysis-ELK-Tool](https://github.com/adobe/AEMCS-CDN-Log-Analysis-ELK-Tool) et suivez les étapes pour installer et charger le **Règles de filtre de trafic (y compris WAF)** tableau de bord.

- Après avoir chargé l’exemple de tableau de bord, la page d’outils du tableau de bord Elastic doit se présenter comme suit :

  ![Tableau de bord des règles de filtrage de trafic ELK](./assets/elk-dashboard.png)

>[!NOTE]
>
>    Comme aucun journal de réseau de diffusion de contenu AEM n’est encore ingéré, le tableau de bord est vide.


## Étape suivante

Découvrez comment déclarer des règles de filtre de trafic y compris des règles WAF dans la variable [Exemples et analyse des résultats](./examples-and-analysis.md) à l’aide du projet de sites WKND AEM.
