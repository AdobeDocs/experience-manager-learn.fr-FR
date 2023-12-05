---
title: Déployer les exemples de ressources sur votre serveur
description: Faire fonctionner le cas d’utilisation sur votre serveur local
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
jira: KT-13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: f12f83fa-673a-454c-aa52-6ea769a182b7
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 100%

---

# Déployer les ressources

Les ressources/configurations suivantes ont été déployées sur un serveur de publication AEM Forms.

* [Lot de Wrappers Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Exemple de modèle de communication interactive](assets/waiver-interactive-communication.zip)
* [Déployez le lot Developingwithserviceuser.](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Ajoutez l’entrée suivante dans le mappage des utilisateurs et utilisatrices de serveur Apache Sling à l’aide du configMgr OSGi.
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## Déployer l’exemple d’application React

* [Téléchargez l’exemple d’application React.](assets/mult-step-form1.zip)
* Décompressez le contenu de l’application React dans un nouveau dossier.
* Accédez au dossier et exécutez les commandes suivantes :

```java
npm install
npm start
```

Ouvrez le fichier EmergencyContact.js et modifiez l’URL dans la méthode de récupération pour qu’elle corresponde à votre environnement.


```javascript
 const getWebForm=async()=>
     {
        setSpinner(true)
        console.log("inside widgetURL function emergency contact");
        // NOTE: replace the `aemforms.azure.com:4503` with your AEM FORM server
        let res = await fetch("http://aemforms.azure.com:4503/bin/getwidgeturl",
          {
            method: "POST",
            body: JSON.stringify({"icTemplate":"/content/forms/af/waiver/waiver/channels/print","waiver":formData})
                     
         })
 
```

Pour activer l’envoi d’appels POST vers le point d’entrée AEM à partir de votre application REACT, vous devez spécifier les entrées appropriées dans le champ Origines autorisées dans la configuration de la politique de partage des ressources entre origines multiples Adobe Granite.

![cors-setting](assets/cors-settings.png)

Voir [Présentation de la norme CORS avec AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=fr) pour plus d’informations sur les options de configuration CORS.
