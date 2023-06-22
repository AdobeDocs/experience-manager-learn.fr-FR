---
title: Créer un service d’envoi personnalisé pour gérer l’envoi de formulaire adaptatif sans tête
description: Renvoie une réponse personnalisée basée sur les données envoyées
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
kt: 13520
source-git-commit: 2dceb4dd4ee1079c100c9cbca94332d61d17ef57
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 1%

---


# Création d’un envoi personnalisé

AEM Forms fournit un certain nombre d’options d’envoi prêtes à l’emploi qui répondent à la plupart des cas d’utilisation. Outre ces actions d’envoi prédéfinies, AEM Forms vous permet d’écrire votre propre gestionnaire d’envoi personnalisé pour traiter l’envoi du formulaire selon vos besoins.

Pour créer un service d’envoi personnalisé, les étapes suivantes ont été suivies :

## Création d’un projet AEM

Si vous disposez déjà d’un projet AEM Forms Cloud Service, vous pouvez [saut à l’écriture d’un service d’envoi personnalisé](#Write-the-custom-submit-service)

* Créez un dossier appelé cloudmanager sur votre disque c.
* Accédez à ce dossier nouvellement créé.
* Copiez et collez le contenu de [ce fichier texte](./assets/creating-maven-project.txt) dans la fenêtre de l’invite de commande. Vous devrez peut-être modifier DarchetypeVersion=41 en fonction du [dernière version](https://github.com/adobe/aem-project-archetype/releases). La dernière version était 41 au moment de la rédaction de cet article.
* Exécutez la commande en appuyant sur la touche Entrée. Si tout se passe correctement, un message de réussite de création s’affiche.

## Écrire le service d’envoi personnalisé{#Write-the-custom-submit-service}

Lancez IntelliJ et ouvrez AEM projet. Créez une nouvelle classe java appelée **HandleRegistrationFormSubmission** comme illustré dans la capture d’écran ci-dessous
![custom-submit-service](./assets/custom-submit-service.png)

Le code suivant a été écrit pour implémenter le service.

```java
package com.aem.bankingapplication.core;
import java.util.HashMap;
import java.util.Map;
import com.google.gson.Gson;
import org.osgi.service.component.annotations.Component;
import com.adobe.aemds.guide.model.FormSubmitInfo;
import com.adobe.aemds.guide.service.FormSubmitActionService;
import com.adobe.aemds.guide.utils.GuideConstants;
import com.google.gson.JsonObject;
import org.slf4j.*;

@Component(
        service=FormSubmitActionService.class,
        immediate = true
)
public class HandleRegistrationFormSubmission implements FormSubmitActionService {
    private static final String serviceName = "Core Custom AF Submit";
    private static Logger logger = LoggerFactory.getLogger(HandleRegistrationFormSubmission.class);



    @Override
    public String getServiceName() {
        return serviceName;
    }

    @Override
    public Map<String, Object> submit(FormSubmitInfo formSubmitInfo) {
        logger.error("in my custom submit service");
        Map<String, Object> result = new HashMap<>();
        logger.error("in my custom submit service");
        String data = formSubmitInfo.getData();
        JsonObject formData = new Gson().fromJson(data,JsonObject.class);
        logger.error("The form data is "+formData);
        JsonObject jsonObject = new JsonObject();
        jsonObject.addProperty("firstName",formData.get("firstName").getAsString());
        jsonObject.addProperty("lastName",formData.get("lastName").getAsString());
        result.put(GuideConstants.FORM_SUBMISSION_COMPLETE, Boolean.TRUE);
        result.put("json",jsonObject.toString());
        return result;
    }

}
```

## Création d’un noeud crx sous apps

Développez le noeud ui.apps pour créer un nouveau module appelé **HandleRegistrationFormSubmission** sous le noeud apps , comme illustré dans la capture d’écran ci-dessous.
![crx-node](./assets/crx-node.png)
Créez un fichier appelé .content.xml sous le **HandleRegistrationFormSubmission**. Copiez et collez le code suivant dans le fichier .content.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="Handle Registration Form Submission"
    jcr:primaryType="sling:Folder"
    guideComponentType="fd/af/components/guidesubmittype"
    guideDataModel="xfa,xsd,basic"
    submitService="Core Custom AF Submit"/>
```

La valeur de la variable **submitService** L’élément doit correspondre  **serviceName = &quot;Core Custom AF Submit&quot;** dans l’implémentation de FormSubmitActionService.

## Déployez le code sur votre instance AEM Forms locale.

Avant d’envoyer les modifications au référentiel de cloud manager, il est recommandé de déployer le code sur votre instance d’auteur locale prête pour le cloud afin de tester le code. Assurez-vous que l’instance d’auteur est en cours d’exécution.
Pour déployer le code vers votre instance d’auteur prête pour le cloud, accédez au dossier racine de votre projet AEM et exécutez la commande suivante :

```
mvn clean install -PautoInstallSinglePackage
```

Cela déploie le code sous la forme d’un package unique sur votre instance de création.

## Envoyez le code à Cloud Manager et déployez le code.

Après avoir vérifié le code sur votre instance locale, poussez-le vers votre instance cloud.
Envoyez les modifications à votre référentiel git local, puis au référentiel de cloud manager. Vous pouvez consulter la section  [Configuration Git](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/setup-git.html), [transfert d’AEM projet dans le référentiel de cloud manager](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git.html) et [déploiement dans l’environnement de développement](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/deploy-to-dev-environment.html) articles.

Une fois que le pipeline s’est exécuté correctement, vous devriez être en mesure d’associer l’action d’envoi de votre formulaire au gestionnaire d’envoi personnalisé, comme illustré dans la capture d’écran ci-dessous.
![submit-action](./assets/configure-submit-action.png)

## Étapes suivantes

[Affichage de la réponse personnalisée dans votre application de réaction](./handle-response-react-app.md)












