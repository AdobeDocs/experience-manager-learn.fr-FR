---
title: Utilisation de ldap avec Aem Forms Workflow
seo-title: Utilisation de ldap avec Aem Forms Workflow
description: Affectation d’une tâche de processus AEM Forms au responsable de l’émetteur
seo-description: Affectation d’une tâche de processus AEM Forms au responsable de l’émetteur
feature: Adaptive Forms,Workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
uuid: 3e32c3a7-387f-4652-8a94-4e6aa6cd5ab8
discoiquuid: 671872b3-3de0-40da-9691-f8b7e88a9443
topic: Development
role: Administrator
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 1%

---


# Utilisation du protocole LDAP avec le flux de travaux AEM Forms

Affectation d’une tâche de processus AEM Forms au responsable de l’émetteur.

Lors de l’utilisation de formulaires adaptatifs dans AEM processus, vous souhaitez affecter de manière dynamique une tâche au gestionnaire de l’auteur du formulaire. Pour ce faire, nous devrons configurer AEM avec Ldap.

Les étapes nécessaires pour configurer AEM avec LDAP sont décrites en [détails ici.](https://helpx.adobe.com/experience-manager/6-5/sites/administering/using/ldap-config.html)

Aux fins de cet article, je joins les fichiers de configuration utilisés pour configurer AEM avec Adobe Ldap. Ces fichiers sont inclus dans le package qui peut être importé à l&#39;aide du gestionnaire de packages.

Dans la capture d&#39;écran ci-dessous, nous récupérons tous les utilisateurs appartenant à un centre de coûts particulier. Si vous souhaitez récupérer tous les utilisateurs de votre protocole LDAP, vous ne pouvez pas utiliser le filtre supplémentaire.

![Configuration du protocole LDAP](assets/costcenterldap.gif)

Dans la capture d’écran ci-dessous, nous affectons les groupes aux utilisateurs récupérés de LDAP dans AEM. Notez le groupe d’utilisateurs de formulaires affecté aux utilisateurs importés. L’utilisateur doit être membre de ce groupe pour pouvoir interagir avec AEM Forms. Nous stockons également la propriété manager sous le noeud profil/manager dans AEM.

![Synchandler](assets/synchandler.gif)

Une fois que vous avez configuré le protocole LDAP et importé des utilisateurs dans AEM, nous pouvons créer un processus qui affectera la tâche au gestionnaire des auteurs d’envoi. Aux fins de cet article, nous avons développé un processus d&#39;approbation simple en une seule étape.

La première étape du processus définit la valeur de l’étape initiale sur Non. La règle de fonctionnement du formulaire adaptatif désactive le panneau &quot;Détails de l’émetteur&quot; et affiche le panneau &quot;Approuvé par&quot; en fonction de la valeur de l’étape initiale.

La deuxième étape affecte la tâche au responsable de l&#39;émetteur. Nous obtenons le gestionnaire de l’expéditeur à l’aide du code personnalisé.

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

Le fragment de code est chargé de récupérer l&#39;identifiant de gestionnaire et d&#39;affecter la tâche au gestionnaire.

Nous obtenons la personne qui a lancé le processus. Nous obtenons ensuite la valeur de la propriété manager.

Selon la manière dont la propriété manager est stockée dans votre LDAP, vous devrez peut-être manipuler les chaînes pour obtenir l’identifiant manager.

Veuillez lire cet article pour implémenter votre propre [ ParticipantChooser .](https://helpx.adobe.com/experience-manager/using/dynamic-steps.html)

Pour le tester sur votre système (pour les employés d&#39;Adobe, vous pouvez utiliser cet exemple prêt à l&#39;emploi)

* [Téléchargez et déployez le lot](/help/forms/assets/common-osgi-bundles/SetValueApp.core-1.0-SNAPSHOT.jar) setvalue. Il s’agit du lot OSGI personnalisé permettant de définir la propriété du responsable.
* [Téléchargement et installation de DevelopingWithServiceUserBundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Importez les ressources associées à cet article dans AEM à l’aide du gestionnaire](assets/aem-forms-ldap.zip) de packages. Les fichiers de configuration LDAP, le flux de travail et un formulaire adaptatif sont inclus dans ce pack.
* Configurez AEM avec votre protocole LDAP à l’aide des informations d’identification LDAP appropriées.
* Connectez-vous à AEM à l’aide de vos informations d’identification LDAP.
* Ouvrez le formulaire [timeoffrequest](http://localhost:4502/content/dam/formsanddocuments/helpx/timeoffrequestform/jcr:content?wcmmode=disabled).
* Remplissez le formulaire et envoyez-le.
* Le responsable de l&#39;émetteur doit obtenir le formulaire pour révision.

>[!NOTE]
>
>Ce code personnalisé permettant d’extraire le nom du gestionnaire a été testé par rapport au protocole LDAP Adobe. Si vous exécutez ce code sur un autre LDAP, vous devrez modifier ou écrire votre propre implémentation getParticipant pour obtenir le nom du responsable.
