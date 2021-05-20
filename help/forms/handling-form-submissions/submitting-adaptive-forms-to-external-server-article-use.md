---
title: Envoi d’un formulaire adaptatif au serveur externe
seo-title: Envoi d’un formulaire adaptatif au serveur externe
description: Envoi d’un formulaire adaptatif au point de terminaison REST s’exécutant sur un serveur externe
seo-description: Envoi d’un formulaire adaptatif au point de terminaison REST s’exécutant sur un serveur externe
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: Formulaires adaptatifs
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 12%

---


# Envoi d’un formulaire adaptatif au serveur externe {#submitting-adaptive-form-to-external-server}

Utilisez l’action Envoyer vers le point de fin REST pour publier les données envoyées vers une URL REST. L’URL peut être celle d’un serveur interne (le serveur sur lequel le formulaire est rendu) ou externe.

En règle générale, les clients souhaitent envoyer les données de formulaire à un serveur externe pour un traitement ultérieur.

Pour publier des données sur un serveur interne, indiquez le chemin de la ressource. Les données sont transmises selon le chemin de la ressource. Par exemple, &lt;/content/restEndPoint> . Pour ces requêtes de publication, les informations d’authentification de la requête d’envoi sont utilisées.

Pour transmettre des données à un serveur externe, indiquez une URL. Le format d’URL est le suivant : <http://host:port/path_to_rest_end_point>. Assurez-vous que vous avez configuré le chemin d’accès pour gérer la demande du POST de manière anonyme.

Pour les besoins du présent article, j&#39;ai écrit un fichier war simple qui peut être déployé sur votre instance tomcat. En supposant que votre Tomcat s’exécute sur le port 8080, l’URL du POST sera

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

lorsque vous configurez votre formulaire adaptatif pour l’envoyer à ce point de terminaison, les données du formulaire et les pièces jointes, le cas échéant, peuvent être extraites dans le servlet à l’aide du code suivant :

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![](assets/formsubmission.gif)
formsubmissionPour tester ce contenu sur votre serveur, procédez comme suit :

1. Installez Tomcat si vous ne l&#39;avez pas déjà. [Les instructions d’installation de tomcat sont disponibles ici](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Téléchargez le [fichier zip](assets/aemformsenablement.zip) associé à cet article. Décompressez le fichier pour obtenir le fichier war.
1. Déployez le fichier war dans votre serveur tomcat.
1. Créez un formulaire adaptatif simple avec le composant de pièce jointe et configurez son action d’envoi comme illustré dans la capture d’écran ci-dessus. L’URL du POST est <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Si votre AEM et tomcat ne sont pas exécutés sur localhost, modifiez l’URL en conséquence.
1. Pour permettre l’envoi de données de formulaire en plusieurs parties à Tomcat, ajoutez l’attribut suivant à l’élément contextuel de &lt;tomcatInstallDir>\conf\context.xml et redémarrez votre serveur Tomcat.
1. **&lt;context allowCasualMultipartParsing=&quot;true&quot;>**
1. Prévisualisez votre formulaire adaptatif, ajoutez une pièce jointe et envoyez-la. Vérifiez les messages dans la fenêtre de la console tomcat.

