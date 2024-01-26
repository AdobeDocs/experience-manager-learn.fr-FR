---
title: Envoyer un formulaire adaptatif au serveur externe
description: Envoyer un formulaire adaptatif au point d’entrée REST s’exécutant sur un serveur externe
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 92
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 100%

---

# Envoyer un formulaire adaptatif au serveur externe {#submitting-adaptive-form-to-external-server}

Utilisez l’action Envoyer vers le point d’entrée REST pour transmettre les données envoyées à l’URL REST. L’URL peut être celle d’un serveur interne (le serveur sur lequel le formulaire est rendu) ou externe.

En règle générale, les clientes et clients souhaitent envoyer les données de formulaire à un serveur externe pour un traitement ultérieur.

Pour publier des données sur un serveur interne, indiquez le chemin de la ressource. Les données sont publiées avec le chemin de la ressource. Par exemple, &lt;/content/restEndPoint>. Pour de telles requêtes de transmission, les informations d’authentification de la requête d’envoi sont utilisées.

Pour publier des données sur un serveur externe, indiquez une URL. Le format d’URL est le suivant : <http://host:port/path_to_rest_end_point>. Assurez-vous de configurer le chemin pour que la requête POST soit traitée anonymement.

Pour les besoins du présent article, j’ai écrit un fichier WAR simple qui peut être déployé sur votre instance Tomcat. En supposant que votre Tomcat s’exécute sur le port 8080, l’URL POST sera

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

lorsque vous configurez votre formulaire adaptatif pour l’envoyer à ce point d’entrée, les données du formulaire et les pièces jointes, le cas échéant, peuvent être extraites dans le servlet à l’aide du code suivant :

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

![formsubmission](assets/formsubmission.gif)
Pour tester cela sur votre serveur, procédez comme suit :

1. Installez Tomcat si vous ne l’avez pas déjà. [Les instructions d’installation de Tomcat sont disponibles ici](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-print-channel-tutorial/set-up-tomcat.html?lang=fr).
1. Téléchargez le [fichier ZIP](assets/aemformsenablement.zip) associé à cet article. Décompressez le fichier pour obtenir le fichier WAR.
1. Déployez le fichier WAR dans votre serveur Tomcat.
1. Créez un formulaire adaptatif simple avec le composant de pièce jointe et configurez son action d’envoi comme illustré dans la copie d’écran ci-dessus. L’URL POST est <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Si vos instances AEM et Tomcat ne sont pas exécutées sur localhost, modifiez l’URL en conséquence.
1. Pour permettre l’envoi de données de formulaire en plusieurs parties à Tomcat, ajoutez l’attribut suivant à l’élément contextuel de &lt;tomcatInstallDir>\conf\context.xml et redémarrez votre serveur Tomcat.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Prévisualisez votre formulaire adaptatif, ajoutez une pièce jointe et envoyez-la. Vérifiez les messages dans la fenêtre de la console Tomcat.
