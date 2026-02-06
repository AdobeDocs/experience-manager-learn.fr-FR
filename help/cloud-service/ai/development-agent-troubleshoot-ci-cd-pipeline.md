---
title: Dépannage du pipeline CI/CD à l’aide de l’agent de développement AEM
description: Découvrez comment dépanner et corriger un pipeline CI/CD en échec à l’aide de l’agent de développement AEM.
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 3%

---


# Dépannage du pipeline CI/CD à l’aide de l’agent de développement AEM

Découvrez comment dépanner et corriger un pipeline CI/CD en échec à l’aide de l’agent de développement AEM.

L’agent de développement AEM aide les équipes techniques, notamment les développeurs, les ingénieurs DevOps et les administrateurs, à **accélérer leurs workflows** en fournissant des conseils et des actions optimisés par l’IA _IA_.

>[!TIP]
>
> Consultez également la section [Présentation des agents dans AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) pour obtenir la liste complète des agents disponibles dans AEM as a Cloud Service, leurs fonctionnalités et la manière dont vous pouvez y accéder.


## Vue d’ensemble

L’agent de développement AEM offre plusieurs fonctionnalités, notamment la possibilité de répertorier, de dépanner et de corriger les pipelines CI/CD en échec. Vous pouvez appeler l’agent de développement AEM par le biais de l’assistant d’IA pour résoudre vos cas d’utilisation spécifiques.

Ce tutoriel utilise le [projet Sites WKND](https://github.com/adobe/aem-guides-wknd) pour démontrer comment dépanner et corriger un pipeline CI/CD en échec à l’aide de l’agent de développement AEM. Les mêmes principes s’appliquent à tout projet AEM.

Pour plus de simplicité, ce tutoriel présente un échec de test unitaire dans le fichier `BylineImpl.java` pour présenter les fonctionnalités de dépannage du pipeline de l’agent de développement AEM.

## Conditions préalables

Les éléments suivants sont requis afin de réaliser ce tutoriel :

- Assistant AI et agents dans AEM activés. Voir [Configurer l’IA dans AEM](./setup.md) pour plus d’informations et notez que les terrains de jeu mentionnés dans cet article ne disposent pas des fonctionnalités de l’agent de développement AEM.
- Accès à Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) avec un rôle de développeur ou de responsable de programme. Voir [Définitions des rôles](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions) pour plus d’informations.
- Un environnement AEM as a Cloud Service
- Accès aux agents dans AEM via le programme [Beta](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)
- Le [projet Sites WKND](https://github.com/adobe/aem-guides-wknd) cloné sur votre ordinateur local

### Fonctionnalités actuelles de l’agent de développement AEM

Avant de passer au tutoriel, examinons les fonctionnalités actuelles de l’agent de développement AEM :

- Répertorier les pipelines CI/CD et leur état
- Dépannage et correction des pipelines **full stack** ayant échoué, y compris les types _Qualité du code_ et _Déploiement_.
- Les étapes _Build_ (compilation du code pour produire un artefact déployable) et _Qualité du code_ (analyse du code statique via les règles SonarQube) des pipelines **full-stack** sont prises en charge.

Les fonctionnalités de l’agent de développement AEM sont constamment étendues et mises à jour régulièrement. Pour tout commentaire ou suggestion, envoyez un e-mail à l&#39;adresse [aem-devagent@adobe.com](mailto:aem-devagent@adobe.com).

## Configuration

Pour suivre ce tutoriel, procédez comme suit :

1. Clonez le [projet Sites WKND](https://github.com/adobe/aem-guides-wknd) et envoyez-le à votre référentiel Git Cloud Manager
2. Création et configuration d’un pipeline de qualité du code
3. Exécuter le pipeline et observer l’échec de l’exécution
4. Utilisez l’agent de développement AEM pour résoudre les problèmes et corriger le pipeline en échec

Passons en revue chaque étape en détail.

### Utilisation du projet WKND Sites en tant que projet de démonstration

Ce tutoriel utilise la branche `tutorial/dev-agent/unit-test-failure` du projet WKND Sites pour démontrer comment utiliser l’agent de développement AEM. Les mêmes principes peuvent être appliqués à n’importe quel projet AEM.

- Un échec de test unitaire a été introduit dans le fichier `BylineImpl.java` comme suit. Si vous utilisez votre propre projet AEM, vous pouvez introduire un échec de test unitaire similaire.

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- Clonez le [projet Sites WKND](https://github.com/adobe/aem-guides-wknd) sur votre ordinateur local, accédez au répertoire du projet et passez à la branche `tutorial/dev-agent/unit-test-failure`.

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- Créez un référentiel Git Cloud Manager pour le projet WKND Sites et ajoutez-le en tant que référentiel distant à votre référentiel Git local :

   - Accédez à Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) et sélectionnez votre programme.
   - Cliquez sur **Référentiels** dans la barre latérale gauche.
   - Cliquez sur **Ajouter un référentiel** dans le coin supérieur droit.
   - Saisissez un **Nom du référentiel** (par exemple, « wknd-site-tutorial ») et cliquez sur **Enregistrer**. Attendez que le référentiel soit créé.

     ![Ajouter un référentiel](./assets/dev-agent/add-repository.png)

   - Cliquez sur **Accéder aux informations sur le référentiel** dans le coin supérieur droit, puis copiez l’URL du référentiel.

     ![Accéder aux informations sur le référentiel](./assets/dev-agent/access-repo-info.png)

   - Ajoutez le référentiel Git Cloud Manager que vous venez de créer en tant que référentiel distant à votre référentiel Git local :

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- Envoyez votre référentiel Git local au référentiel Git de Cloud Manager :

  ```shell
  git push adobe
  ```

  Lorsqu’on vous invite à fournir des informations d’identification, indiquez le **Nom d’utilisateur** et le **Mot de passe** de la boîte de dialogue modale **Informations sur le référentiel** de Cloud Manager.

### Création et configuration d’un pipeline de qualité du code

Ce tutoriel utilise un pipeline de qualité du code (hors production) pour déclencher l’échec du pipeline à des fins de dépannage. Voir [Présentation des pipelines CI/CD](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction) pour plus d’informations sur les pipelines de qualité du code.

- Dans Cloud Manager, accédez à la section **Pipelines** et sélectionnez **Ajouter** > **Ajouter un pipeline hors production**.
- Dans la boîte de dialogue **Ajouter un pipeline hors production**, configurez les éléments suivants :

   - Étape **Configuration** :
      - Conservez les valeurs par défaut telles que **Type de pipeline** en tant que `Code Quality Pipeline` et **Déclencheur de déploiement** en tant que `Manual`.
      - Pour **Nom du pipeline hors production**, saisissez `Code Quality::Fullstack`

     ![Ajouter une configuration de pipeline hors production](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - **Code Source** étape :
      - Sélectionnez **Code de pile complète**
      - Pour **Référentiel**, sélectionnez le référentiel Git de Cloud Manager que vous venez de créer
      - Pour **Branche Git**, sélectionnez `tutorial/dev-agent/unit-test-failure`
      - Cliquez sur **Enregistrer**.

     ![Ajout de code Source de pipeline hors production](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- Exécutez le nouveau pipeline de qualité du code en cliquant sur **Exécuter** dans le menu à trois points de l’entrée du pipeline.

  ![Exécution du pipeline de qualité du code](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> Le pipeline de déploiement n’est pas abordé dans ce tutoriel. Cependant, vous pouvez suivre les mêmes principes pour résoudre les problèmes et corriger un pipeline de déploiement en échec.


### Observer l’échec de l’exécution du pipeline

Le pipeline Qualité du code échoue à l’étape **Préparation des artefacts** avec une erreur :

![Échec de l’exécution du pipeline](./assets/dev-agent/failed-pipeline-execution.png)

Sans l’agent de développement AEM, cet échec du pipeline nécessite un dépannage manuel. L’équipe de développement doit alors vérifier les journaux et réviser le code, ce qui constitue un processus fastidieux et long.

Ensuite, vous verrez comment l’IA dédiée aux agences peut résoudre les problèmes et corriger l’échec d’exécution du pipeline.

## Utilisation de l’agent de développement AEM pour dépanner et corriger le pipeline en échec

Vous pouvez appeler l’agent de développement AEM à l’aide de l’assistant AI dans AEM en décrivant l’échec du pipeline en langage naturel.

- Cliquez sur l’icône **Assistant IA** dans le coin supérieur droit.

- Saisissez les détails de l’échec du pipeline en langage naturel ou **Invite**. Par exemple :

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![Appeler l’agent de développement AEM](./assets/dev-agent/invoke-aem-development-agent.png)

  L’**agent de développement AEM** est appelé pour résoudre les problèmes et corriger l’échec de l’exécution du pipeline.

  >[!NOTE]
  >
  > Si l’invite saisie n’est pas claire, l’assistant AI demande des éclaircissements et fournit des informations pour vous aider à affiner l’invite.

- Une fois le raisonnement terminé, cliquez sur l’icône **Ouvrir en plein écran** pour afficher le processus de dépannage détaillé.

  ![Ouvrir en plein écran](./assets/dev-agent/open-in-full-screen.png)

  Les résultats contiennent des informations précieuses, notamment les détails de l’erreur, le fichier source, le numéro de ligne, ainsi qu’une section **Comment corriger** qui décrit clairement les étapes à suivre pour résoudre le problème.

- Dans ce cas, l’agent a correctement suggéré de modifier l’implémentation (méthode `getName()`) ou de mettre à jour le test unitaire (méthode `getNameTest()`) pour résoudre le problème. Il évitait les hallucinations et utilisait une approche humaine dans la boucle tout en fournissant des changements de code exploitables pour le développeur.

  ![Copier les modifications de code](./assets/dev-agent/copy-code-changes.png)

- Mettez à jour le fichier `BylineImpl.java` avec les modifications de code suggérées, puis validez et transmettez les modifications au référentiel Git de Cloud Manager.

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- Exécutez à nouveau le pipeline et observez la réussite de l’exécution.

## Exemples supplémentaires

Le projet Sites WKND comprend d’autres exemples de code rompu et de problèmes de configuration, tels que des dépendances manquantes et une configuration incorrecte. Vous pouvez explorer ces exemples en consultant les branches [&#x200B; qui commencent par `tutorial/dev-agent/`](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview). Pour afficher les modifications avec rupture, vous pouvez comparer la branche `tutorial/dev-agent/unit-test-failure` à la branche `main` en cliquant sur le bouton **Comparer**. Recherchez ensuite la section _fichier modifié_.

![Comparer les branches](./assets/dev-agent/compare-branches.png)

Consultez également le [Exemples d’invites](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts) pour obtenir des idées supplémentaires sur l’utilisation de l’agent de développement AEM.

## Résumé

Dans ce tutoriel, vous avez appris à utiliser l’agent de développement AEM pour résoudre les problèmes et corriger un pipeline CI/CD en échec à l’aide de l’assistant AI. Vous avez également appris comment l’IA dédiée aux agences accélère les workflows techniques en fournissant des informations exploitables et en apportant des modifications au code.

Commencez à utiliser l’agent de développement AEM et d’autres agents dans AEM pour accélérer vos workflows. Voir [Présentation des agents dans AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) pour plus d’informations.

## Ressources supplémentaires

- [L’IA dans Experience Manager](./overview.md)
- [Présentation des agents dans AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [Présentation de l’agent de développement](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [Présentation des agents dans AEM](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
