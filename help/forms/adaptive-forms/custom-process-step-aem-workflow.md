---
title: Implémentation d’une étape de processus personnalisée
seo-title: Implémentation d’une étape de processus personnalisée
description: Ecriture de pièces jointes de formulaire adaptatif dans un système de fichiers à l’aide de l’étape de processus personnalisée
seo-description: Ecriture de pièces jointes de formulaire adaptatif dans un système de fichiers à l’aide de l’étape de processus personnalisée
feature: workflow
topics: development
audience: developer
doc-type: tutorial
activity: understand
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 2%

---


# Étape de processus personnalisée

Ce didacticiel est destiné aux clients AEM Forms qui doivent implémenter une étape de processus personnalisée. Une étape de processus peut exécuter un script ECMA ou appeler du code Java personnalisé pour effectuer des opérations. Ce didacticiel explique les étapes nécessaires à l’implémentation de WorkflowProcess qui est exécuté par l’étape du processus.

La principale raison de l’implémentation de l’étape de processus personnalisé est d’étendre le flux de travail AEM. Par exemple, si vous utilisez des composants AEM Forms dans votre modèle de processus, vous pouvez effectuer les opérations suivantes :

* Enregistrer les pièces jointes du formulaire adaptatif dans le système de fichiers
* Manipuler les données envoyées

Pour accomplir le cas d’utilisation ci-dessus, vous écrivez généralement un service OSGi qui est exécuté par l’étape de processus.

## Créer un projet expert

La première étape consiste à créer un projet d&#39;expert à l&#39;aide de l&#39;archétype d&#39;expert d&#39;Adobe approprié. Les étapes détaillées sont répertoriées dans cet [article](https://helpx.adobe.com/experience-manager/using/maven_arch13.html). Une fois votre projet d&#39;expert importé dans eclipse, vous êtes prêt à début d&#39;écrire votre premier composant OSGi qui peut être utilisé dans votre étape de processus.


### Créer une classe qui implémente WorkflowProcess

Ouvrez le projet maven dans votre environnement de développement de l&#39;éclipse. Développez le dossier **projectname** > **core**. Développez le dossier src/main/java. Vous devriez voir un paquet qui se termine par &quot;core&quot;. Créez une classe Java qui implémente WorkflowProcess dans ce package. Vous devez remplacer la méthode execute. La signature de la méthode execute est la suivante :
public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)renvoie WorkflowException
La méthode execute donne accès aux 3 variables suivantes.

**WorkItem** : La variable workItem donne accès aux données liées au processus. La documentation de l&#39;API publique est disponible [ici.](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**WorkflowSession** : Cette variable workflowSession vous permet de contrôler le flux de travaux. La documentation de l’API publique est disponible [ici](https://helpx.adobe.com/experience-manager/6-3/sites/developing/using/reference-materials/diff-previous/changes/com.adobe.granite.workflow.WorkflowSession.html)

**MetaDataMap** : Toutes les métadonnées associées au processus. Tous les arguments de processus transmis à l&#39;étape de processus sont disponibles à l&#39;aide de l&#39;objet MetaDataMap.[Documentation d’API](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/javadoc/com/adobe/granite/workflow/metadata/MetaDataMap.html)

Dans ce didacticiel, nous allons écrire les pièces jointes ajoutées au formulaire adaptatif dans le système de fichiers dans le cadre du processus AEM.

Pour ce faire, la classe Java suivante a été écrite :

Examinons ce code

```
@Component(property = { Constants.SERVICE_DESCRIPTION + "=Write Adaptive Form Attachments to File System",
        Constants.SERVICE_VENDOR + "=Adobe Systems",
        "process.label" + "=Save Adaptive Form Attachments to File System" })
public class WriteFormAttachmentsToFileSystem implements WorkflowProcess {
     private static final Logger log = LoggerFactory.getLogger(WriteFormAttachmentsToFileSystem.class);
     @Override
    public void execute(WorkItem workItem, WorkflowSession workflowSession, MetaDataMap processArguments)
            throws WorkflowException {
        // TODO Auto-generated method stub
        log.debug("The string I got was ..." + processArguments.get("PROCESS_ARGS", "string").toString());
        String[] params = processArguments.get("PROCESS_ARGS", "string").toString().split(",");
        String attachmentsPath = params[0];
        String saveToLocation = params[1];
        log.debug("The seperator is" + File.separator);
        String payloadPath = workItem.getWorkflowData().getPayload().toString();
 
        String attachmentsFilePath = payloadPath + "/" + attachmentsPath + "/attachments";
        log.debug("The data file path is " + attachmentsFilePath);
 
        ResourceResolver resourceResolver = workflowSession.adaptTo(ResourceResolver.class);
 
        Resource attachmentsNode = resourceResolver.getResource(attachmentsFilePath);
        Iterator<Resource> attachments = attachmentsNode.listChildren();
        while (attachments.hasNext()) {
            Resource attachment = attachments.next();
            String attachmentPath = attachment.getPath();
            String attachmentName = attachment.getName();
 
            log.debug("The attachmentPath is " + attachmentPath + " and the attachmentname is " + attachmentName);
            com.adobe.aemfd.docmanager.Document attachmentDoc = new com.adobe.aemfd.docmanager.Document(attachmentPath,
                    attachment.getResourceResolver());
            try {
                File file = new File(saveToLocation + File.separator + workItem.getId());
                if (!file.exists()) {
                    file.mkdirs();
                }
 
                attachmentDoc.copyToFile(new File(file + File.separator + attachmentName));
 
                log.debug("Saved attachment" + attachmentName);
                attachmentDoc.close();
 
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
 
        }
 
    }
```

Ligne 1 - définit les propriétés de notre composant. La propriété process.label est ce que vous verrez lors de l’association du composant OSGi à l’étape du processus, comme le montre l’une des captures d’écran ci-dessous.

Lignes 13 à 15 - Les arguments de processus transmis à ce composant OSGi sont fractionnés à l&#39;aide du séparateur &quot;,&quot;. Les valeurs de attachementPath et saveToLocation sont ensuite extraites du tableau de chaînes.

* attachementPath : il s&#39;agit du même emplacement que celui que vous avez spécifié dans le formulaire adaptatif lorsque vous avez configuré l&#39;action d&#39;envoi du formulaire adaptatif pour appeler AEM Workflow. Il s’agit du nom du dossier dans lequel vous souhaitez que les pièces jointes soient enregistrées dans AEM par rapport à la charge utile du flux de travail.

* saveToLocation : emplacement où vous souhaitez que les pièces jointes soient enregistrées sur le système de fichiers de votre serveur AEM.

Ces deux valeurs sont transmises en tant qu’arguments de processus, comme le montre la capture d’écran ci-dessous.

![ProcessStep](assets/implement-process-step.gif)


Ligne 19 : Nous construisons ensuite la pièce jointe FilePath. Le chemin d’accès au fichier de pièce jointe est semblable à

    /var/fd/tableau de bord/payload/server0/2018-11-19/3EF6ENASOQTHCPLNDYVNAM7OKA_7/Pièces jointes/pièces jointes

* Les &quot;pièces jointes&quot; sont le nom du dossier par rapport à la charge utile du flux de travail qui a été spécifiée lorsque vous avez configuré l’option d’envoi du formulaire adaptatif.

   ![submitoptions](assets/af-submit-options.gif)

Lignes 24-26 - Get ResourceResolver et ensuite la ressource pointant vers la pièce jointe FilePath.

Le reste du code crée des objets de Document en effectuant une itération sur l&#39;objet enfant de la ressource pointant vers attachementFilePath à l&#39;aide de l&#39;API. Cet objet document est spécifique à AEM Forms. Nous utilisons ensuite la méthode copyToFile de l&#39;objet document pour enregistrer l&#39;objet document.

>[!NOTE]
>
>Puisque nous utilisons un objet Document spécifique à AEM Forms, vous devez inclure la dépendance aemfd-client-sdk dans votre projet maven. L’ID de groupe est com.adobe.aemfd et l’ID artefact est aemfd-client-sdk.

#### Création et déploiement

[Créez le lot comme décrit ](https://helpx.adobe.com/experience-manager/using/maven_arch13.html#BuildtheOSGibundleusingMaven)
[iciAssurez-vous que le lot est déployé et en principal état](http://localhost:4502/system/console/bundles)

Créer un modèle de processus. Faites glisser et déposez l’étape du processus dans le modèle de processus. Associez l’étape de processus à &quot;Enregistrer les pièces jointes de formulaire adaptatif dans le système de fichiers&quot;.

Fournissez les arguments de processus nécessaires séparés par une virgule. Par exemple, Pièces jointes,c:\\scrappp\\. Le premier argument est le dossier dans lequel vos pièces jointes de formulaire adaptatif seront stockées par rapport à la charge utile du flux de travail. Il doit s’agir de la même valeur que celle spécifiée lors de la configuration de l’action d’envoi du formulaire adaptatif. Le deuxième argument est l&#39;emplacement où les pièces jointes doivent être stockées.

Créez un formulaire adaptatif. Faites glisser le composant Pièces jointes vers le formulaire. Configurez l’action d’envoi du formulaire pour appeler le processus créé lors des étapes précédentes. Indiquez le chemin d’accès de pièce jointe approprié.

Enregistrez les paramètres.

Prévisualiser le formulaire. Ajoutez quelques pièces jointes et envoyez le formulaire. Les pièces jointes doivent être enregistrées dans le système de fichiers à l’emplacement indiqué par vous dans le flux de travail.

