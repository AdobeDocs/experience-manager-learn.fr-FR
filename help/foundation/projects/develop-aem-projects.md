---
title: Développer des projets dans AEM
description: Tutoriel de développement expliquant comment développer des projets AEM.  Dans ce tutoriel, nous allons créer un modèle de projet personnalisé qui pourra être utilisé pour créer des projets dans AEM afin de gérer les workflows et les tâches de création de contenu.
version: 6.4, 6.5
feature: Projects, Workflow
topics: collaboration, development, governance
activity: develop
audience: developer, implementer, administrator
doc-type: tutorial
topic: Development
role: Developer
level: Beginner
exl-id: 9bfe3142-bfc1-4886-85ea-d1c6de903484
source-git-commit: 481b8877e252b885da307fcf4d96f8a50f026fa6
workflow-type: ht
source-wordcount: '4571'
ht-degree: 100%

---

# Développer des projets dans AEM

Ce tutoriel de développement explique comment développer des [!DNL AEM Projects].  Dans ce tutoriel, nous allons créer un modèle de projet personnalisé qui pourra être utilisé pour créer des projets dans AEM afin de gérer les workflows et les tâches de création de contenu.

>[!VIDEO](https://video.tv.adobe.com/v/16904?quality=12&learn=on)

*Cette vidéo présente une brève démonstration du workflow terminé créé dans le tutoriel ci-dessous.*

## Présentation {#introduction}

[[!DNL AEM Projects]](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/projects.html?lang=fr) est une fonctionnalité d’AEM conçue pour faciliter la gestion et le regroupement de tous les workflows et tâches associés à la création de contenu dans le cadre d’une mise en œuvre d’AEM Sites ou Assets.

Les projets AEM sont fournis avec plusieurs [modèles de projet prêts à l’emploi](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/projects.html?lang=fr#ProjectTemplates). Lors de la création d’un projet, les créateurs et créatrices peuvent choisir parmi ces modèles disponibles. Les mises en œuvre d’AEM volumineuses avec des besoins commerciaux uniques voudront créer des modèles de projet personnalisés et adaptés à leurs besoins. En créant un modèle de projet personnalisé, les développeurs et développeuses peuvent configurer le tableau de bord du projet, se connecter aux workflows personnalisés et créer des rôles professionnels supplémentaires pour un projet. Nous allons examiner la structure d’un modèle de projet et en créer un exemple.

![Carte de projet personnalisée.](./assets/develop-aem-projects/custom-project-card.png)

## Configuration

Ce tutoriel décrit le code nécessaire à la création d’un modèle de projet personnalisé. Vous pouvez télécharger et installer le [package joint](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip) dans un environnement local à suivre avec le tutoriel. Vous pouvez également accéder au projet Maven complet hébergé sur [GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide).

* [Package de tutoriel terminé](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Référentiel de code complet sur GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)

Ce tutoriel suppose d’avoir des connaissances de base sur les [bonnes pratiques de développement AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/the-basics.html) et une certaine familiarité avec la [configuration de projet Maven AEM](https://helpx.adobe.com/fr/experience-manager/6-5/sites/developing/using/ht-projects-maven.html). Tout le code mentionné est destiné à être utilisé comme référence et ne doit être déployé que sur une [instance AEM de développement local](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=frl#GettingStarted).

## Structure d’un modèle de projet

Les modèles de projet doivent être placés sous contrôle de code source et doivent se trouver sous votre dossier d’application sous /apps. Idéalement, ils doivent être placés dans un sous-dossier avec la convention de nommage de **&#42;/projects/templates/**&lt;my-template>. En suivant cette convention de nommage, les nouveaux modèles personnalisés deviennent automatiquement disponibles pour les créateurs et créatrices lors de la création d’un projet. La configuration des modèles de projet disponibles est définie au niveau du nœud **/content/projects/jcr:content** par la propriété **cq:allowedTemplates**. Par défaut, il s’agit d’une expression régulière : **/(apps|libs)/.&#42;/projects/templates/.&#42;**

Le nœud racine d’un modèle de projet aura un **jcr:primaryType** de **cq:Template**. Sous le nœud racine, il existe 3 nœuds : **gadgets**, **roles**, et **workflows**. Ces nœuds sont tous **nt:unstructured**. Sous le nœud racine peut également se trouver un fichier thumbnail.png qui s’affiche lors de la sélection du modèle dans l’assistant de création de projets.

La structure de nœud complète est la suivante :

```shell
/apps/<my-app>
    + projects (nt:folder)
         + templates (nt:folder)
              + <project-template-root> (cq:Template)
                   + gadgets (nt:unstructured)
                   + roles (nt:unstructured)
                   + workflows (nt:unstructured)
```

### Racine du modèle de projet

Le nœud racine du modèle de projet est de type **cq:Template**. Sur ce nœud, vous pouvez configurer des propriétés **jcr:title** et **jcr:description** qui s’affichent dans l’assistant de création de projets. Il existe également une propriété appelée **wizard** qui pointe vers un formulaire qui renseigne les propriétés du projet. La valeur par défaut de **/libs/cq/core/content/projects/wizard/steps/defaultproject.html** devrait fonctionner normalement dans la plupart des cas, car cela permet à l’utilisateur ou utilisatrice de renseigner les propriétés de base du projet et d’ajouter des personnes membres au groupe.

*&#42;Notez que l’assistant de création de projets n’utilise pas le servlet POST Sling. À la place, les valeurs sont publiées sur un servlet personnalisé :**com.adobe.cq.projects.impl.servlet.ProjectServlet**. Cela doit être pris en compte lors de l’ajout de champs personnalisés.*

Vous trouverez un exemple d’assistant personnalisé pour le modèle de projet de traduction : **/libs/cq/core/content/projects/wizard/translationproject/defaultproject**.

### Gadgets {#gadgets}

Il n’existe aucune propriété supplémentaire sur ce nœud, mais les enfants du nœud gadgets contrôlent quelles mosaïques de projet renseignent le tableau de bord du projet lorsqu’un nouveau projet est créé. Les [mosaïques du projet](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/projects.html?lang=fr#ProjectTiles) (également appelés gadgets ou capsules) sont des cartes simples qui renseignent l’espace de travail d’un projet. Vous trouverez une liste complète des mosaïques prêtes à l’emploi sous : **/libs/cq/gui/components/projects/admin/pod. **Les personnes propriétaires de projet peuvent toujours ajouter/supprimer des mosaïques après la création d’un projet.

### Rôles {#roles}

Il existe 3 [rôles par défaut](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/projects.html?lang=fr#UserRolesinaProject) pour chaque projet : **Observateurs**, **Éditeurs**, et **Propriétaires**. En ajoutant des nœuds enfants sous le nœud roles, vous pouvez ajouter des rôles de projet spécifiques à l’entreprise supplémentaires pour le modèle. Vous pouvez ensuite lier ces rôles à des workflows spécifiques associés au projet.

### Workflows {#workflows}

L’une des raisons les plus intéressantes de la création d’un modèle de projet personnalisé est qu’il vous donne la possibilité de configurer les workflows disponibles à utiliser avec le projet. Il peut s’agir de workflows prêts à l’emploi ou personnalisés. Sous le nœud **workflows**, il doit y avoir un nœud **models** (également `nt:unstructured`) et les nœuds enfants spécifient les modèles de workflow disponibles. La propriété **modelId **pointe vers le modèle de workflow sous /etc/workflow et la propriété **wizard** pointe vers la boîte de dialogue utilisée lors du démarrage du workflow. L’avantage des projets est la possibilité d’ajouter une boîte de dialogue personnalisée (assistant) pour capturer des métadonnées spécifiques à l’entreprise au démarrage du workflow, ce qui peut déclencher d’autres actions dans le workflow.

```shell
<projects-template-root> (cq:Template)
    + workflows (nt:unstructured)
         + models (nt:unstructured)
              + <workflow-model> (nt:unstructured)
                   - modelId = points to the workflow model
                   - wizard = dialog used to start the workflow
```

## Créer un modèle de projet {#creating-project-template}

Puisque nous copions/configurons principalement des nœuds, nous utiliserons CRXDE Lite. Dans votre instance AEM locale, ouvrez [CRXDE Lite](http://localhost:4502/crx/de/index.jsp).

1. Commencez par créer un dossier sous `/apps/&lt;your-app-folder&gt;` nommé `projects`. Créez un autre dossier sous le précédent nommé `templates`.

   ```shell
   /apps/aem-guides/projects-tasks/
                       + projects (nt:folder)
                                + templates (nt:folder)
   ```

1. Pour faciliter les choses, nous allons démarrer notre modèle personnalisé à partir du modèle de projet simple existant.

   1. Copiez et collez le nœud **/libs/cq/core/content/projects/templates/default** sous le dossier *templates* créé à l’étape 1.

   ```shell
   /apps/aem-guides/projects-tasks/
                + templates (nt:folder)
                     + default (cq:Template)
   ```

1. Vous devriez maintenant avoir un chemin qui ressemble à **/apps/aem-guides/projects-tasks/projects/templates/authoring-project**.

   1. Remplacez les propriétés **jcr:title** et **jcr:description** du nœud author-project par des valeurs de titre et de description personnalisées.

      1. Laissez la propriété **wizard** pointer vers les propriétés par défaut du projet.

   ```shell
   /apps/aem-guides/projects-tasks/projects/
            + templates (nt:folder)
                 + authoring-project (cq:Template)
                      - jcr:title = "Authoring Project"
                      - jcr:description = "A project to manage approval and publish process for AEM Sites or Assets"
                      - wizard = "/libs/cq/core/content/projects/wizard/steps/defaultproject.html"
   ```

1. Pour ce modèle de projet, nous voulons utiliser des tâches.
   1. Ajouter un nouveau nœud **nt:unstructured** sous authoring-project/gadgets nommé **tasks**.
   1. Ajoutez des propriétés de chaîne au nœud tasks pour **cardWeight** = &quot;100&quot;, **jcr:title** = &quot;Tasks&quot; et **sling:resourceType** = &quot;cq/gui/components/projects/admin/pod/taskpod&quot;.

   Désormais, la [mosaïque des tâches](https://experienceleague.adobe.com/docs/?lang=fr#Tasks) s’affiche par défaut lors de la création d’un projet.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
            + team (nt:unstructured)
            + asset (nt:unstructured)
            + work (nt:unstructured)
            + experiences (nt:unstructured)
            + projectinfo (nt:unstructured)
            ..
            + tasks (nt:unstructured)
                 - cardWeight = "100"
                 - jcr:title = "Tasks"
                 - sling:resourceType = "cq/gui/components/projects/admin/pod/taskpod"
   ```

1. Nous ajouterons un rôle Approbateur personnalisé à notre modèle de projet.

   1. Sous le nœud de modèle de projet (authoring-project), ajoutez un nouveau nœud **nt:unstructured** étiqueté **roles**.
   1. Ajoutez un autre nœud **nt:unstructured** étiqueté « approvers » en tant qu’enfant du nœud roles.
   1. Ajoutez des propriétés de chaîne **jcr:title** = &quot;**Approvers**&quot;, **roleclass** = &quot;**owner**&quot;, **roleid** = &quot;**approvers**&quot;.
      1. Le nom du nœud des approbateurs et approbatrices, ainsi que jcr:title et roleid, peuvent être n’importe quelle valeur de chaîne (tant que roleid est unique).
      1. **roleclass** détermine les autorisations appliquées à ce rôle en fonction des [3 rôles prêts à l’emploi](https://experienceleague.adobe.com/docs/?lang=fr#User%20Roles%20in%20a%20Project) : **propriétaire**, **éditeur**, et **observateur**.
      1. En général, si le rôle personnalisé est davantage un rôle de gestion, la roleclass peut être **propriétaire** ; s’il s’agit d’un rôle de création plus spécifique comme photographe ou designer, la roleclass **éditeur** devrait suffire. La grande différence entre les rôles **propriétaire** et **éditeur** est que les personnes propriétaires d’un projet peuvent mettre à jour les propriétés du projet et ajouter de nouveaux utilisateurs et utilisatrices au projet.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
           + approvers (nt:unstructured)
                - jcr:title = "Approvers"
                - roleclass = "owner"
                - roleid = "approver"
   ```

1. En copiant le modèle Projet simple, vous obtiendrez 4 workflows prêts à l’emploi configurés. Chaque nœud sous workflows/models pointe vers un workflow spécifique et un assistant de démarrage de boîte de dialogue pour ce workflow. Plus loin dans ce tutoriel, nous allons créer un workflow personnalisé pour ce projet. Pour l’instant, supprimez les nœuds sous workflow/models :

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
            + models (nt:unstructured)
               - (remove ootb models)
   ```

1. Afin que les créateurs et créatrices de contenu puissent facilement identifier le modèle de projet, vous pouvez ajouter une miniature personnalisée. La taille recommandée est de 319x319 pixels.
   1. Dans CRXDE Lite, créez un fichier en tant qu’objet apparenté aux gadgets, aux rôles et aux nœuds de workflows nommé **thumbnail.png**.
   1. Enregistrez, puis accédez au nœud `jcr:content` et double-cliquez sur la propriété `jcr:data` (évitez de cliquer sur « afficher »).
      1. Cela devrait vous inviter à modifier la boîte de dialogue de fichier `jcr:data` et vous pouvez alors charger une miniature personnalisée.

   ```shell
   ../projects/templates/authoring-project
       + gadgets (nt:unstructured)
       + roles (nt:unstructured)
       + workflows (nt:unstructured)
       + thumbnail.png (nt:file)
   ```

Représentation XML terminée du modèle de projet :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:description="A project to manage approval and publish process for AEM Sites or Assets"
    jcr:primaryType="cq:Template"
    jcr:title="Authoring Project"
    ranking="{Long}1"
    wizard="/libs/cq/core/content/projects/wizard/steps/defaultproject.html">
    <jcr:content
        jcr:primaryType="nt:unstructured"
        detailsHref="/projects/details.html"/>
    <gadgets jcr:primaryType="nt:unstructured">
        <team
            jcr:primaryType="nt:unstructured"
            jcr:title="Team"
            sling:resourceType="cq/gui/components/projects/admin/pod/teampod"
            cardWeight="60"/>
        <tasks
            jcr:primaryType="nt:unstructured"
            jcr:title="Tasks"
            sling:resourceType="cq/gui/components/projects/admin/pod/taskpod"
            cardWeight="100"/>
        <work
            jcr:primaryType="nt:unstructured"
            jcr:title="Workflows"
            sling:resourceType="cq/gui/components/projects/admin/pod/workpod"
            cardWeight="80"/>
        <experiences
            jcr:primaryType="nt:unstructured"
            jcr:title="Experiences"
            sling:resourceType="cq/gui/components/projects/admin/pod/channelpod"
            cardWeight="90"/>
        <projectinfo
            jcr:primaryType="nt:unstructured"
            jcr:title="Project Info"
            sling:resourceType="cq/gui/components/projects/admin/pod/projectinfopod"
            cardWeight="100"/>
    </gadgets>
    <roles jcr:primaryType="nt:unstructured">
        <approvers
            jcr:primaryType="nt:unstructured"
            jcr:title="Approvers"
            roleclass="owner"
            roleid="approvers"/>
    </roles>
    <workflows
        jcr:primaryType="nt:unstructured"
        tags="[]">
        <models jcr:primaryType="nt:unstructured">
        </models>
    </workflows>
</jcr:root>
```

## Tester le modèle de projet personnalisé

Nous pouvons maintenant tester notre modèle de projet en créant un nouveau projet.

1. Vous devriez voir le modèle personnalisé comme l’une des options de création d’un projet.

   ![Choix d’un modèle.](./assets/develop-aem-projects/choose-template.png)

1. Après avoir sélectionné le modèle personnalisé, cliquez sur « Suivant » et notez que lorsque vous renseignez les personnes membres du projet, vous pouvez les ajouter en tant que rôle d’approbateur ou d’approbatrice.

   ![Approuver.](./assets/develop-aem-projects/user-approver.png)

1. Cliquez sur « Créer » pour terminer la création du projet à partir du modèle personnalisé. Vous remarquerez sur le tableau de bord du projet que la mosaïque Tâches et les autres mosaïques configurées sous les gadgets apparaissent automatiquement.

   ![Mosaïque Tâches.](./assets/develop-aem-projects/tasks-tile.png)


## Pourquoi un workflow ?

Traditionnellement, les workflows AEM qui se centrent autour d’un processus d’approbation ont suivi les étapes de workflow de participant ou participante. La boîte de réception AEM contient des détails sur les tâches et les workflows et une intégration améliorée avec les projets AEM. Ces fonctionnalités rendent l’utilisation des étapes du processus de création de tâche des projets plus attrayante.

### Pourquoi des tâches ?

L’utilisation d’une étape de création de tâche par rapport aux étapes de participant et ou participante traditionnelles offre plusieurs avantages :

* **Date de début et d’échéance** : facilite la gestion du temps par les créateurs et les créatrices. La nouvelle fonction Calendrier utilise ces dates.
* **Priorité** : les priorités intégrées Faible, Normale et Élevée permettent aux créateurs et aux créatrices de hiérarchiser les travaux.
* **Commentaires threads** : lorsque les créateurs et les créatrices travaillent sur une tâche, ils ont la possibilité de laisser des commentaires afin d’améliorer la collaboration.
* **Visibilité** : les affichages et les mosaïques de tâches dans les projets permettent aux personnes responsables de déterminer comment le temps est utilisé.
* **Intégration de projets** : les tâches sont déjà intégrées aux rôles des projets et aux tableaux de bord.

À l’instar des étapes de participant ou participante, les tâches peuvent être affectées et acheminées dynamiquement. Les métadonnées de tâche telles que Titre et Priorité peuvent également être définies dynamiquement en fonction des actions précédentes, comme nous le verrons dans le tutoriel suivant.

Bien que les tâches présentent certains avantages par rapport aux étapes de participant ou participante, elles sont plus gourmandes et ne sont pas aussi utiles en dehors d’un projet. En outre, tous les comportements dynamiques des tâches doivent être codés à l’aide de scripts ecma ayant leurs propres limites.

## Conditions requises pour les exemples de cas d’utilisation : {#goals-tutorial}

![Diagramme de processus de workflow.](./assets/develop-aem-projects/workflow-process-diagram.png)

Le diagramme ci-dessus décrit les conditions requises de haut niveau pour notre workflow d’approbation des exemples.

La première étape consiste à créer une tâche pour terminer la modification d’un élément de contenu. Nous autoriserons l’initiateur ou l’initiatrice du workflow à choisir la personne désignée pour cette première tâche.

Une fois la première tâche terminée, la personne désignée dispose de trois options pour le routage du workflow :

**Normal** : le routage normal crée une tâche affectée au groupe d’approbation du projet pour sa révision et son approbation. La priorité de la tâche est Normale et la date d’échéance est fixée à 5 jours à compter de sa création.

**Rapide** : le routage rapide crée également une tâche affectée au groupe d’approbation du projet. La priorité de la tâche est Haute et la date d’échéance n’est que d’un jour.

**Contournement** : dans cet exemple de workflow, la personne initiale a la possibilité de contourner le groupe d’approbation. (cela peut effectivement rendre nul l’objectif d’un workflow d’« Approbation », mais cela nous permet d’illustrer des fonctionnalités de routage supplémentaires).

Le groupe d’approbation peut approuver le contenu ou le renvoyer à la personne désignée initiale pour le retravail. Dans le cas d’un renvoi en vue du retravail, une nouvelle tâche est créée et correctement étiquetée « Renvoyé pour le retravail ».

La dernière étape du workflow utilise l’étape de processus Activer la page/ressource prête à l’emploi et réplique la payload.

## Créer un modèle de workflow

1. Dans le menu Démarrage d’AEM, accédez à Outils > Workflow > Modèles. Cliquez sur « Créer » dans le coin supérieur droit pour créer un modèle de workflow.

   Attribuez un titre au nouveau modèle : « Processus d’approbation du contenu » et un nom d’URL : « content-approval-workflow ».

   ![Boîte de dialogue de création de workflow.](./assets/develop-aem-projects/workflow-create-dialog.png)

   Vous trouverez plus d’informations sur la [création de workflows ici](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/extending-workflows/workflows-models.html?lang=fr).

1. Il est recommandé de regrouper les workflows personnalisés dans leur propre dossier sous /etc/workflow/models. Dans CRXDE Lite, créez un dossier « **nt:folder** » sous /etc/workflow/models nommé **« aem-guides »**. L’ajout d’un sous-dossier permet de s’assurer que les workflows personnalisés ne sont pas écrasés accidentellement lors des mises à niveau ou des installations du pack de services.

   &#42;Notez qu’il est important de ne jamais placer le dossier ou les workflows personnalisés dans des sous-dossiers prêts à l’emploi tels que /etc/workflow/models/dam ou /etc/workflow/models/projects, car le sous-dossier entier peut également être écrasé par des mises à niveau ou des packs de services.

   ![Emplacement du modèle de workflow dans la version 6.3.](./assets/develop-aem-projects/custom-workflow-subfolder.png)

   Emplacement du modèle de workflow dans la version 6.3

   >[!NOTE]
   >
   >Si vous utilisez AEM version 6.4 ou ultérieure, l’emplacement du workflow a changé. [Pour plus d’informations, rendez-vous ici.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/extending-workflows/workflows-best-practices.html?lang=fr)

   Si vous utilisez AEM version 6.4 ou ultérieure, le modèle de workflow est créé sous `/conf/global/settings/workflow/models`. Répétez les étapes ci-dessus avec le répertoire /conf et ajoutez un sous-dossier nommé `aem-guides` et déplacez le `content-approval-workflow` dans ce dossier.

   ![Emplacement de définition de workflow moderne.](./assets/develop-aem-projects/modern-workflow-definition-location.png)
Emplacement du modèle de workflow dans la version 6.4 et ultérieure

1. La possibilité d’ajouter des étapes de processus à un workflow donné est introduite dans AEM 6.3. Les étapes s’affichent pour l’utilisateur ou l’utilisatrice à partir de la boîte de réception dans l’onglet Informations sur le workflow. Elle montre à l’utilisateur ou l’utilisatrice l’étape actuelle du workflow ainsi que les étapes précédentes et suivantes.

   Pour configurer les étapes, ouvrez la boîte de dialogue Propriétés de la page dans le Sidekick. Le quatrième onglet est intitulé « Étapes ». Ajoutez les valeurs suivantes pour paramétrer les trois étapes de ce workflow :

   1. Modifier le contenu
   1. Approbation
   1. Publier

   ![Configuration des étapes de workflow.](./assets/develop-aem-projects/workflow-model-stage-properties.png)

   Configurez les étapes de workflow dans la boîte de dialogue Propriétés de la page.

   ![Barre de progression du workflow.](./assets/develop-aem-projects/workflow-info-progress.png)

   La barre de progression du workflow telle qu’elle s’affiche dans la boîte de réception AEM.

   Vous pouvez éventuellement charger une **image** dans les Propriétés de page qui servira de miniature du workflow lorsque les utilisateurs et utilisatrices le sélectionnent. Les dimensions de l’image doivent être de 319x319 pixels. L’ajout d’une **Description** aux Propriétés de page apparaîtra également lorsqu’un utilisateur ou une utilisatrice sélectionne le workflow.

1. Le processus de workflow Créer une tâche de projet est conçu pour créer une tâche en tant qu’étape dans le workflow. Ce n’est qu’après avoir terminé la tâche que le workflow progresse. L’étape Créer une tâche de projet présente un aspect intéressant : elle peut lire les valeurs de métadonnées de workflow et les utiliser pour créer la tâche de manière dynamique.

   Commencez par supprimer l’étape de participant ou participante créée par défaut. Dans le menu Composants du Sidekick, développez le sous en-tête **Projets** et faites glisser et déposez l’étape **Créer une tâche de projet** sur le modèle.

   Double-cliquez sur l’étape Créer une tâche de projet pour ouvrir la boîte de dialogue du workflow. Configurez les propriétés suivantes :

   Cet onglet est commun à toutes les étapes du processus de workflow et nous définirons le Titre et la Description (ceux-ci ne seront pas visibles par la personne finale). La propriété importante que nous définirons est l’étape du workflow sur **Modifier le contenu** dans le menu déroulant.

   ```shell
   Common Tab
   -----------------
       Title = "Start Task Creation"
       Description = "This the first task in the Workflow"
       Workflow Stage = "Edit Content"
   ```

   Le processus de workflow Créer une tâche de projet est conçu pour créer une tâche en tant qu’étape dans le workflow. L’onglet Tâche permet de définir toutes les valeurs de la tâche. Dans notre cas, nous voulons que la personne désignée soit dynamique, nous la laissons donc vide. Le reste des valeurs de propriété :

   ```shell
   Task Tab
   -----------------
       Name* = "Edit Content"
       Task Priority = "Medium"
       Description = "Edit the content and finalize for approval. Once finished submit for approval."
       Due In - Days = "2"
   ```

   L’onglet Routage est une boîte de dialogue facultative qui peut spécifier les actions disponibles pour l’utilisateur ou l’utilisatrice qui effectue la tâche. Ces actions ne sont que des valeurs de chaîne et sont enregistrées dans les métadonnées du workflow. Ces valeurs peuvent être lues par des scripts et/ou des étapes de processus plus tard dans le workflow pour « acheminer » dynamiquement le workflow. Selon les [objectifs du workflow](#goals-tutorial), nous ajoutons trois actions à cet onglet :

   ```shell
   Routing Tab
   -----------------
       Actions =
           "Normal Approval"
           "Rush Approval"
           "Bypass Approval"
   ```

   Cet onglet nous permet de configurer un script de création préalable de tâche dans lequel nous pouvons choisir par programmation différentes valeurs de la tâche avant qu’elle ne soit créée. Nous avons la possibilité de pointer le script vers un fichier externe ou d’incorporer un script court directement dans la boîte de dialogue. Dans notre cas, nous pointons le script de création préalable de tâche vers un fichier externe. À l’étape 5, nous allons créer ce script.

   ```shell
   Advanced Settings Tab
   -----------------
      Pre-Create Task Script = "/apps/aem-guides/projects/scripts/start-task-config.ecma"
   ```

1. À l’étape précédente, nous avons référencé un script de création préalable de tâche. Nous allons créer ce script dans lequel nous définirons la personne désignée de la tâche en fonction de la valeur d’une valeur de métadonnées de workflow « **assignee** ». La valeur **assignee** est définie au déclenchement du workflow. Nous allons également lire les métadonnées de workflow pour choisir dynamiquement la priorité de la tâche en lisant la valeur **taskPriority** des métadonnées du workflow, ainsi que la taskdueDate (date d’échéance de la tâche) pour définir dynamiquement le moment où la première tâche doit être effectuée.

   À des fins d’organisation, nous avons créé un dossier sous notre dossier d’application destiné à contenir tous les scripts liés au projet : **/apps/aem-guides/projects-tasks/projects/scripts**. Créez un fichier sous ce dossier nommé « **start-task-config.ecma** ». &#42;Notez que le chemin d’accès à votre fichier start-task-config.ecma correspond au chemin d’accès défini dans l’onglet Paramètres avancés de l’étape 4.

   Ajoutez les éléments suivants comme contenu du fichier :

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   ```

1. Revenez au workflow d’approbation du contenu. Faites glisser et déposez le composant **Division OU** (situé dans le Sidekick sous la catégorie « Workflow ») sous l’étape **Démarrer la tâche**. Dans la boîte de dialogue commune, sélectionnez le bouton radio correspondant à 3 branches. La Division OU lit la valeur des métadonnées du workflow « **lastTaskAction** » pour déterminer l’itinéraire du workflow. La propriété « **lastTaskAction** » est définie sur l’une des valeurs de l’onglet Routage configuré à l’étape 4. Pour chacun des onglets Branche, renseignez la zone de texte **Script** avec les valeurs suivantes :

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Normal Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Rush Approval") {
       return true;
   }
   
   return false;
   }
   ```

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Bypass Approval") {
       return true;
   }
   
   return false;
   }
   ```

   &#42;Notez que nous effectuons une correspondance de chaîne directe pour déterminer l’itinéraire. Il est donc important que les valeurs définies dans les scripts Branch correspondent aux valeurs d’itinéraire définies à l’étape 4.

1. Faites glisser et déposez une autre étape **Créer une tâche de projet** sur le modèle le plus à gauche (Branch 1) sous la Division OU. Renseignez la boîte de dialogue avec les propriétés suivantes :

   ```
   Common Tab
   -----------------
       Title = "Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is Medium."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Approve Content for Publish"
       Task Priority = "Medium"
       Description = "Approve this content for publication."
       Days = "5"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Puisqu’il s’agit de l’itinéraire de validation normal, la priorité de la tâche est définie sur Moyenne. De plus, nous donnons au groupe d’approbation 5 jours pour terminer la tâche. La personne désignée reste vide dans l’onglet Tâche, car nous l’affecterons de manière dynamique dans l’onglet Paramètres avancés. Nous donnons au groupe d’approbation deux itinéraires possibles lors de l’exécution de cette tâche : **« Approuver et publier »** si les personnes du groupe approuvent le contenu et qu’il peut être publié et **« Envoyer pour révision »** si des problèmes doivent être corrigés par l’éditeur ou éditrice d’origine. La personne chargée de l’approbation peut laisser des commentaires que l’éditeur ou éditrice d’origine verra si le workflow lui est renvoyé.

Plus tôt dans ce tutoriel, nous avons créé un modèle de projet qui incluait un rôle Approbateurs. Chaque fois qu’un nouveau projet est créé à partir de ce modèle, un groupe spécifique au projet est créé pour le rôle Approbateurs. Tout comme une étape de participant ou participante, une tâche ne peut être affectée qu’à un utilisateur ou une utilisatrice, ou à un groupe. Nous souhaitons affecter cette tâche au groupe de projets correspondant au groupe d’approbation. Tous les workflows lancés à partir d’un projet disposeront de métadonnées qui mappent les rôles de projet au groupe spécifique au projet.

Copiez et collez le code suivant dans la zone de texte **Script** de l’onglet Paramètres avancés. Ce code lit les métadonnées du workflow et affecte la tâche au groupe d’approbation du projet. S’il ne trouve pas la valeur du groupe d’approbation, il attribue la tâche au groupe d’administration.

```
var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");

task.setCurrentAssignee(projectApproverGrp);
```

1. Faites glisser et déposez une autre étape **Créer une tâche de projet** sur le modèle à la branche centrale (Branch 2) sous la Division OU. Renseignez la boîte de dialogue avec les propriétés suivantes :

   ```
   Common Tab
   -----------------
       Title = "Rush Approval Task Creation"
       Description = "Create a an approval task for Project Approvers. Priority is High."
       Workflow Stage = "Approval"
   
   Task Tab
   ------------
       Name* = "Rush Approve Content for Publish"
       Task Priority = "High"
       Description = "Rush approve this content for publication."
       Days = "1"
   
   Routing Tab - Actions
   ----------------------------
       "Approve and Publish"
       "Send Back for Revision"
   ```

   Puisqu’il s’agit de l’itinéraire d’approbation rapide, la priorité de la tâche est définie sur Élevée. De plus, nous ne donnons au groupe d’approbation qu’un seul jour pour terminer la tâche. La personne désignée reste vide dans l’onglet Tâche, car nous l’affecterons de manière dynamique dans l’onglet Paramètres avancés.

   Nous pouvons réutiliser le même fragment de script qu’à l’étape 7 pour renseigner la zone de texte **Script** dans l’onglet Paramètres avancés. Copiez et collez le code ci-dessous :

   ```
   var projectApproverGrp = workflowData.getMetaDataMap().get("project.group.approvers","administrators");
   
   task.setCurrentAssignee(projectApproverGrp);
   ```

1. Faites glisser et déposez un composant Aucune opération vers la branche la plus à droite (Branch 3). Le composant Aucune opération n’effectue aucune action et progresse immédiatement, ce qui représente la volonté de l’éditeur ou éditrice d’origine de contourner l’étape d’approbation. Techniquement, nous pourrions laisser cette branche sans aucune étape de workflow, mais nous allons ajouter une étape Aucune opération en tant que bonne pratique. Cela permet aux autres développeurs et développeuses de déterminer l’objectif de la Branch 3.

   Double-cliquez sur l’étape du workflow et configurez le Titre et la Description :

   ```
   Common Tab
   -----------------
       Title = "Bypass Approval"
       Description = "Placeholder step to indicate that the original editor decided to bypass the approver group."
   ```

   ![Division OU du modèle de workflow.](./assets/develop-aem-projects/workflow-stage-after-orsplit.png)

   Le modèle de workflow doit ressembler à ceci une fois que les trois branches de la Division OU ont été configurées.

1. Puisque le groupe d’approbation a la possibilité de renvoyer le workflow à l’éditeur ou l’éditrice d’origine pour d’autres révisions, l’étape **Atteindre** est nécessaire pour lire la dernière action effectuée et acheminer le workflow vers le début ou le laisser continuer.

   Faites glisser et déposez le composant Étape Atteindre (situé dans le Sidekick, sous Workflow) sous la Division OU où il se joint à nouveau. Double-cliquez et configurez les propriétés suivantes dans la boîte de dialogue :

   ```
   Common Tab
   ----------------
       Title = "Goto Step"
       Description = "Based on the Approver groups action route the workflow to the beginning or continue and publish the payload."
   
   Process Tab
   ---------------
       The step to go to. = "Start Task Creation"
   ```

   Le dernier élément que nous allons configurer est le script, qui fait partie de l’étape du processus Atteindre. La valeur du Script peut être incorporée via la boîte de dialogue ou configurée pour renvoyer vers un fichier externe. Le script Atteindre doit contenir une **fonction check()** et renvoyer la valeur « true » si le workflow doit atteindre l’étape spécifiée. Le renvoi de résultats « false » fait avancer le workflow.

   Si le groupe d’approbation choisit l’action **Renvoyer pour révision** (configurée aux étapes 7 et 8), le workflow doit retourner à l’étape **Commencer la création de la tâche**.

   Sous l’onglet Processus, ajoutez le fragment de code suivant à la zone de texte Script :

   ```
   function check() {
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   if(lastAction == "Send Back for Revision") {
       return true;
   }
   
   return false;
   }
   ```

1. Pour publier la payload, nous utiliserons l’étape du processus **Activer la page/ressource** prête à l’emploi. Cette étape de processus nécessite peu de configuration et ajoute la payload du workflow à la file d’attente de réplication pour activation. Nous allons ajouter l’étape sous l’étape Atteindre. Elle ne sera ainsi atteinte que si le groupe d’approbation a approuvé le contenu à publier ou si l’éditeur ou l’éditrice d’origine a choisi l’itinéraire Contourner l’approbation.

   Faites glisser l’étape de processus **Activer la page/ressource** (qui se trouve dans le Sidekick, sous le workflow de gestion de contenu web) dans l’étape Atteindre du modèle.

   ![Modèle de workflow terminé](assets/develop-aem-projects/workflow-model-final.png).

   Le modèle de workflow doit ressembler à ceci après l’ajout des étapes Atteindre et Activer la page/ressource.

1. Si le groupe d’approbation renvoie le contenu pour révision, la personne éditrice d’origine doit en être informée. Pour ce faire, nous pouvons modifier dynamiquement les propriétés de création de tâche. Nous modifierons la valeur de la propriété lastActionTaken de **Renvoyer pour révision**. Si cette valeur est présente, nous modifierons le titre et la description afin d’indiquer que cette tâche est le résultat du contenu renvoyé pour révision. Nous définirons également la priorité sur **Élevée**, afin que l’éditeur ou l’éditrice s’attelle à cette tâche en priorité. Enfin, nous définirons la date d’échéance de la tâche à « un jour » à partir du moment où le workflow a été renvoyé pour révision.

   Remplacez le script de démarrage `start-task-config.ecma` (créé à l’étape 5) par les éléments suivants :

   ```
   // start-task-config.ecma
   // Populate the task using values stored as workflow metadata originally posted by the start workflow wizard
   
   // set the assignee based on start workflow wizard
   var assignee = workflowData.getMetaDataMap().get("assignee", Packages.java.lang.String);
   task.setCurrentAssignee(assignee);
   
   //Set the due date for the initial task based on start workflow wizard
   var dueDate = workflowData.getMetaDataMap().get("taskDueDate", Packages.java.util.Date);
   if (dueDate != null) {
       task.setProperty("taskDueDate", dueDate);
   }
   
   //Set the priority based on start workflow wizard
   var taskPriority = workflowData.getMetaDataMap().get("taskPriority", "Medium");
   task.setProperty("taskPriority", taskPriority);
   
   var lastAction = workflowData.getMetaDataMap().get("lastTaskAction","");
   
   //change the title and priority if the approver group sent back the content
   if(lastAction == "Send Back for Revision") {
     var taskName = "Review and Revise Content";
   
     //since the content was rejected we will set the priority to High for the revison task
     task.setProperty("taskPriority", "High"); 
   
     //set the Task name (displayed as the task title in the Inbox) 
     task.setProperty("name", taskName);
     task.setProperty("nameHierarchy", taskName);
   
     //set the due date of this task 1 day from current date
     var calDueDate = Packages.java.util.Calendar.getInstance();
     calDueDate.add(Packages.java.util.Calendar.DATE, 1);
     task.setProperty("taskDueDate", calDueDate.getTime());
   
   }
   ```

## Créer l’assistant « Démarrer le workflow » {#start-workflow-wizard}

Lorsque vous démarrez un workflow au sein d’un projet, vous devez spécifier un assistant pour le lancer. L’assistant par défaut : `/libs/cq/core/content/projects/workflowwizards/default_workflow` permet à l’utilisateur ou l’utilisatrice de saisir un titre de workflow, un commentaire de démarrage et un chemin de payload pour l’exécution du workflow. Vous trouverez également plusieurs autres exemples ci-dessous : `/libs/cq/core/content/projects/workflowwizards`.

La création d’un assistant personnalisé peut s’avérer très efficace, car vous pouvez collecter des informations essentielles avant le démarrage du workflow. Les données sont stockées dans le cadre des métadonnées du workflow. Les processus du workflow peuvent les lire et modifier dynamiquement le comportement en fonction des valeurs renseignées. Nous allons créer un assistant personnalisé pour affecter dynamiquement la première tâche du workflow en fonction d’une valeur de l’assistant de démarrage.

1. Dans CRXDE-Lite, nous allons créer un sous-dossier dans le dossier `/apps/aem-guides/projects-tasks/projects` appelé « wizards » (assistants). Copiez l’assistant par défaut à partir de : `/libs/cq/core/content/projects/workflowwizards/default_workflow` sous le dossier « wizards » nouvellement créé et renommez-le en **content-approval-start**. Le chemin complet est le suivant : `/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start`.

   L’assistant par défaut est un assistant à 2 colonnes. La première colonne indique le titre, la description et la miniature du modèle de workflow sélectionné. La deuxième colonne comprend des champs pour le titre du workflow, les commentaires de démarrage et le chemin d’accès à la payload. L’assistant est un formulaire d’interface utilisateur tactile standard et utilise les [Composants de formulaire de l’IU Granite](https://experienceleague.adobe.com/docs/?lang=fr) pour renseigner les champs.

   ![Assistant de workflow d’approbation du contenu](./assets/develop-aem-projects/content-approval-start-wizard.png).

1. Nous ajouterons un champ supplémentaire à l’assistant qui est utilisé pour définir la personne désignée de la première tâche dans le workflow (consultez l’étape 5 de la section [Créer un modèle de workflow](#create-workflow-model)).

   Sous `../content-approval-start/jcr:content/items/column2/items`, créez un nœud de type `nt:unstructured` et nommez-le **« assign »**. Nous utiliserons le composant Sélecteur d’utilisateur ou d’utilisatrice de projets (basé sur le [Composant de sélecteur d’utilisateur ou d’utilisatrice Granite](https://experienceleague.adobe.com/docs/?lang=fr)). Ce champ de formulaire permet de limiter la sélection des utilisateurs et utilisatrices et des groupes à ceux qui appartiennent au projet en cours.

   Vous trouverez ci-dessous la représentation XML du nœud **assign** :

   ```xml
   <assign
       granite:class="js-cq-project-user-picker"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="cq/gui/components/projects/admin/userpicker"
       fieldLabel="Assign To"
       hideServiceUsers="{Boolean}true"
       impersonatesOnly="{Boolean}true"
       showOnlyProjectMembers="{Boolean}true"
       name="assignee"
       projectPath="${param.project}"
       required="{Boolean}true"/>
   ```

1. Nous ajouterons également un champ de sélection des priorités qui déterminera la priorité de la première tâche du workflow (consultez l’étape 5 de la section [Créer un modèle de workflow](#create-workflow-model)).

   Sous `/content-approval-start/jcr:content/items/column2/items`, créez un nœud de type `nt:unstructured` et nommez-le **priority**. Nous utiliserons le [composant Sélection de l’IU Granite](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr) pour remplir le champ de formulaire.

   Sous le nœud **priority**, nous allons ajouter un nœud **items** de **nt:unstructured**. Sous le nœud **items**, ajoutez 3 nœuds supplémentaires pour remplir les options de sélection pour High, Medium et Low (Élevé, Moyen et Faible). Chaque nœud est de type **nt:unstructured** et doit avoir une propriété **text** et **value**. Les valeurs de ces 2 propriétés doivent être identiques :

   1. Élevée
   1. Moyenne
   1. Faible

   Pour le nœud Medium, ajoutez une propriété booléenne supplémentaire nommée « **selected »** et définissez la valeur suivante : **true**. Cela permet de s’assurer que Medium est la valeur par défaut dans le champ de sélection.

   Vous trouverez ci-dessous une représentation XML de la structure et des propriétés du nœud :

   ```xml
   <priority
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/select"
       fieldLabel="Task Priority"
       name="taskPriority">
           <items jcr:primaryType="nt:unstructured">
               <high
                   jcr:primaryType="nt:unstructured"
                   text="High"
                   value="High"/>
               <medium
                   jcr:primaryType="nt:unstructured"
                   selected="{Boolean}true"
                   text="Medium"
                   value="Medium"/>
               <low
                   jcr:primaryType="nt:unstructured"
                   text="Low"
                   value="Low"/>
               </items>
   </priority>
   ```

1. Nous autorisons à présent la personne à l’origine du workflow à définir la date d’échéance de la tâche initiale. Le champ de formulaire [Sélecteur de date de l’IU Granite](https://experienceleague.adobe.com/docs/?lang=fr) permet de capturer cette entrée. Le champ masqué [TypeHint](https://sling.apache.org/documentation/bundles/manipulating-content-the-slingpostservlet-servlets-post.html#typehint) est ajouté pour s’assurer que l’entrée est stockée en tant que propriété de type Date dans JCR.

   Ajoutez deux nœuds **nt:unstructured** avec les propriétés suivantes représentées ci-dessous en code XML :

   ```xml
   <duedate
       granite:rel="project-duedate"
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/datepicker"
       displayedFormat="YYYY-MM-DD HH:mm"
       fieldLabel="Due Date"
       minDate="today"
       name="taskDueDate"
       type="datetime"/>
   <duedatetypehint
       jcr:primaryType="nt:unstructured"
       sling:resourceType="granite/ui/components/coral/foundation/form/hidden"
       name="taskDueDate@TypeHint"
       type="datetime"
       value="Calendar"/>
   ```

1. Pour consulter le code complet de la boîte de dialogue de l’assistant de démarrage, rendez-vous [ici](https://github.com/Adobe-Marketing-Cloud/aem-guides/blob/master/projects-tasks-guide/ui.apps/src/main/content/jcr_root/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start/.content.xml).

## Connecter le workflow et le modèle de projet {#connecting-workflow-project}

Il ne reste plus qu’à s’assurer que le modèle de workflow est disponible pour être lancé à partir de l’un des projets. Pour ce faire, nous devons revoir le modèle de projet que nous avons créé dans la partie 1 du tutoriel.

La configuration de workflow est une zone d’un modèle de projet qui indique les workflows disponibles pour le projet. La configuration indique également l’assistant de démarrage du workflow lors du lancement du workflow (que nous avons créé dans les [étapes précédentes)](#start-workflow-wizard). La configuration du workflow d’un modèle de projet est « active », ce qui signifie que la mise à jour de la configuration de workflow affectera les nouveaux projets créés ainsi que les projets existants qui utilisent le modèle.

1. Dans CRXDE-Lite, accédez au modèle de projet de création créé précédemment dans `/apps/aem-guides/projects-tasks/projects/templates/authoring-project/workflows/models`.

   Sous le nœud models, ajoutez un nouveau nœud nommé **contentapproval** avec le type de nœud **nt:unstructured**. Ajoutez les propriétés suivantes au nœud :

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/etc/workflow/models/aem-guides/content-approval-workflow/jcr:content/model"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

   >[!NOTE]
   >
   >Si vous utilisez AEM 6.4, l’emplacement du workflow a été modifié. Pointez la propriété `modelId` sur l’emplacement du modèle de workflow d’exécution sous `/var/workflow/models/aem-guides/content-approval-workflow`.
   >
   >
   >Rendez-vous [ici pour en savoir plus sur le nouvel emplacement du workflow.](https://experienceleague.adobe.com/docs/experience-manager-65/developing/extending-aem/extending-workflows/workflows-best-practices.html?lang=fr)

   ```xml
   <contentapproval
       jcr:primaryType="nt:unstructured"
       modelId="/var/workflow/models/aem-guides/content-approval-workflow"
       wizard="/apps/aem-guides/projects-tasks/projects/wizards/content-approval-start.html"
   />
   ```

1. Une fois que le workflow d’approbation de contenu a été ajouté au modèle de projet, il doit pouvoir être lancé à partir de la mosaïque Workflow du projet. Après l’effort, le réconfort. Lancez-vous et essayez les différents itinéraires que nous avons créés.

## Documents annexes

* [Télécharger le package de tutoriel terminé](./assets/develop-aem-projects/projects-tasks-guide.ui.apps-0.0.1-SNAPSHOT.zip)
* [Référentiel de code complet sur GitHub](https://github.com/Adobe-Marketing-Cloud/aem-guides/tree/feature/projects-tasks-guide)
* [Documentation des projets AEM](https://experienceleague.adobe.com/docs/experience-manager-65/authoring/projects/projects.html?lang=fr)
