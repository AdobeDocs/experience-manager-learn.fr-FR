---
title: Utiliser LDAP avec le workflow AEM Forms
description: Affecter une tâche de workflow AEM Forms à la personne responsable de l’auteur ou de l’autrice
feature: Adaptive Forms, Workflow
topic: Integrations
role: Developer
version: 6.4,6.5
level: Intermediate
exl-id: 2e9754ff-49fe-4260-b911-796bcc4fd266
last-substantial-update: 2021-09-18T00:00:00Z
duration: 149
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 100%

---

# Utiliser LDAP avec le workflow AEM Forms

Affecter une tâche de workflow AEM Forms à la personne responsable de l’auteur ou de l’autrice.

Lors de l’utilisation d’un formulaire adaptatif dans le workflow AEM, vous souhaitez affecter de manière dynamique une tâche à la personne responsable de l’auteur ou de l’autrice du formulaire. Pour ce cas d’utilisation, nous devons configurer AEM avec LDAP.

Les étapes de configuration d’AEM avec LDAP sont expliquées en [détail ici.](https://helpx.adobe.com/fr/experience-manager/6-5/sites/administering/using/ldap-config.html)

Pour les besoins de cet article, je joins les fichiers de configuration utilisés pour configurer AEM avec le LDAP d’Adobe. Ces fichiers sont inclus dans le package qui peut être importé à l’aide du gestionnaire de packages.

Dans la capture d’écran ci-dessous, nous récupérons tous les utilisateurs et utilisatrices appartenant à un centre de coût spécifique. Si vous souhaitez récupérer tous les utilisateurs et utilisatrices de votre LDAP, vous ne pouvez pas utiliser le filtre supplémentaire.

![Configuration LDAP.](assets/costcenterldap.gif)

Dans la capture d’écran ci-dessous, nous attribuons les groupes aux utilisateurs et utilisatrices récupérés depuis LDAP dans AEM. Prêtez attention au groupe forms-users attribué aux utilisateurs et utilisatrices importés. L’utilisateur ou l’utilisatrice doit être membre de ce groupe pour interagir avec AEM Forms. Nous stockons également la propriété manager sous le nœud profile/manager dans AEM.

![Synchandler.](assets/synchandler.gif)

Une fois que vous avez configuré le protocole LDAP et importé des utilisateurs et des utilisatrices dans AEM, nous pouvons créer un workflow qui affectera la tâche à la personne responsable de l’auteur ou de l’autrice. Pour les besoins de cet article, nous avons développé un simple workflow d’approbation en une seule étape.

La première étape du workflow définit la valeur de l’étape initiale sur Non. La règle commerciale du formulaire adaptatif désactive le panneau « Détails de l’auteur » et affiche le panneau « Approuvé par » en fonction de la valeur de l’étape initiale.

La deuxième étape affecte la tâche à la personne responsable de l’auteur ou de l’autrice. Nous obtenons la personne responsable de l’auteur ou de l’autrice à l’aide du code personnalisé.

![Affectation d’une tâche.](assets/assigntask.gif)

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

L’extrait de code est chargé de récupérer l’ID de la personne responsable et de lui affecter la tâche.

Nous contactons la personne qui a lancé le workflow. Nous obtenons ensuite la valeur de la propriété manager.

Selon la manière dont la propriété manager est stockée dans votre LDAP, vous devrez peut-être effectuer une manipulation de chaîne pour obtenir l’ID de la personne responsable.

Veuillez lire cet article pour mettre en œuvre votre propre [ParticipantChooser.](https://experienceleague.adobe.com/docs/experience-manager-learn/getting-started-wknd-tutorial-develop/overview.html?lang=en&amp;CID=RedirectAEMCommunityKautuk)

Pour le tester sur votre système (pour les employées et employés d’Adobe, vous pouvez utiliser cet exemple prêt à l’emploi) :

* [Télécharger et déployer le lot setvalue](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar). Il s’agit du lot OSGI personnalisé pour la définition de la propriété manager.
* [Télécharger et installer DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importez les ressources associées à cet article dans AEM à l’aide du gestionnaire de packages.](assets/aem-forms-ldap.zip) Les fichiers de configuration LDAP, le workflow et un formulaire adaptatif sont inclus dans ce package.
* Configurez AEM avec votre LDAP à l’aide des informations d’identification LDAP appropriées.
* Connectez-vous à AEM à l’aide de vos informations d’identification LDAP.
* Ouvrez le [timeoffrequestform](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Remplissez le formulaire et envoyez-le.
* La personne responsable de l’auteur ou de l’autrice doit recevoir le formulaire à réviser.

>[!NOTE]
>
>Ce code personnalisé d’extraction du nom de la personne responsable a été testé par rapport au LDAP d’Adobe. Si vous exécutez ce code sur un autre LDAP, vous devrez modifier ou écrire votre propre implémentation getParticipant pour obtenir le nom de la personne responsable.
