---
title: Utilisation de LDAP avec le workflow AEM Forms
description: Affectation d’une tâche de workflow AEM Forms au responsable de l’émetteur
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 1%

---

# Utilisation de LDAP avec le workflow AEM Forms

Affectation d’une tâche de workflow AEM Forms au responsable de l’émetteur.

Lors de l’utilisation d’un formulaire adaptatif dans AEM processus, vous souhaitez affecter de manière dynamique une tâche au gestionnaire de l’auteur du formulaire. Pour réaliser ce cas d’utilisation, nous devrons configurer AEM avec Ldap.

Les étapes de configuration d’AEM avec LDAP sont expliquées dans la section [Détails ici.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Pour les besoins de cet article, je joins les fichiers de configuration utilisés pour configurer AEM avec Adobe Ldap. Ces fichiers sont inclus dans le package qui peut être importé à l’aide du gestionnaire de packages.

Dans la capture d’écran ci-dessous, nous récupérons tous les utilisateurs appartenant à un centre de coûts spécifique. Si vous souhaitez récupérer tous les utilisateurs de votre LDAP, vous ne pouvez pas utiliser le filtre supplémentaire.

![Configuration du protocole LDAP](assets/costcenterldap.gif)

Dans la capture d’écran ci-dessous, nous assignons les groupes aux utilisateurs récupérés depuis LDAP dans AEM. Notez le groupe forms-users affecté aux utilisateurs importés. L’utilisateur doit être membre de ce groupe pour interagir avec AEM Forms. Nous stockons également la propriété manager sous le noeud profile/manager dans AEM.

![Synchandler](assets/synchandler.gif)

Une fois que vous avez configuré le protocole LDAP et importé des utilisateurs dans AEM, nous pouvons créer un workflow qui assignera la tâche au responsable de l&#39;expéditeur. Pour les besoins de cet article, nous avons développé un simple processus d’approbation en une seule étape.

La première étape du workflow définit la valeur de l’étape initiale sur Non. La règle de fonctionnement du formulaire adaptatif désactive le panneau &quot;Détails de l’émetteur&quot; et affiche le panneau &quot;Approuvé par&quot; en fonction de la valeur de l’étape initiale.

La deuxième étape affecte la tâche au responsable de l’émetteur. Nous obtenons le responsable de l’expéditeur à l’aide du code personnalisé.

![Assigner une tâche](assets/assigntask.gif)

```java
public String getParticipant(WorkItem workItem, WorkflowSession wfSession, MetaDataMap arg2) throws WorkflowException{
resourceResolver = wfSession.adaptTo(ResourceResolver.class);
UserManager userManager = resourceResolver.adaptTo(UserManager.class);
Authorizable workflowInitiator = userManager.getAuthorizable(workItem.getWorkflow().getInitiator());
.
.
String managerPorperty = workflowInitiator.getProperty("profile/manager")[0].getString();
.
.

}
```

Le fragment de code est chargé de récupérer l’identifiant du responsable et d’affecter la tâche au responsable.

Nous prenons contact avec la personne qui a lancé le workflow. Nous obtenons ensuite la valeur de la propriété manager.

Selon la manière dont la propriété manager est stockée dans votre LDAP, vous devrez peut-être effectuer une manipulation de chaîne pour obtenir l’identifiant du gestionnaire.

Veuillez lire cet article pour mettre en oeuvre votre propre [  ParticipantChooser .](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=fr&amp;CID=RedirectAEMCommunityKautuk)

Pour le tester sur votre système (pour les employés d’Adobe, vous pouvez utiliser cet exemple prêt à l’emploi)

* [Télécharger et déployer le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé pour la définition de la propriété du gestionnaire.
* [Téléchargement et installation de DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importez les actifs associés à cet article dans AEM à l’aide du gestionnaire de packages.](assets/aem-forms-ldap.zip)Les fichiers de configuration LDAP, le workflow et un formulaire adaptatif sont inclus dans ce module.
* Configurez AEM avec votre LDAP à l’aide des informations d’identification LDAP appropriées.
* Connectez-vous à AEM à l’aide de vos informations d’identification LDAP.
* Ouvrez le [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled)
* Remplissez le formulaire et envoyez-le.
* Le responsable de l’émetteur doit obtenir le formulaire à réviser.

>[!NOTE]
>
>Ce code personnalisé d’extraction du nom du gestionnaire a été testé par rapport au LDAP Adobe. Si vous exécutez ce code sur un autre LDAP, vous devrez modifier ou écrire votre propre implémentation getParticipant pour obtenir le nom du responsable.
