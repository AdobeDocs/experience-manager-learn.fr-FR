---
title: Déployer les exemples de ressources sur votre serveur
description: Obtenir le cas d’utilisation sur votre serveur local
feature: Adaptive Forms,Acrobat Sign
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
kt: 13099
last-substantial-update: 2023-04-13T00:00:00Z
exl-id: 44f4261b-d6fe-42ad-a3aa-2a36ca897b5e
source-git-commit: 137f7166a6a10ecd95a85114b27a1a3bd608b965
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 4%

---

# Déploiement des ressources

Les ressources/configurations suivantes ont été déployées sur un serveur de publication AEM Forms.

* [Groupe de wrapper Adobe Sign](assets/AcrobatSign.core-1.0.0-SNAPSHOT.jar)

* [Exemple de modèle de communication interactive](assets/waiver-interactive-communication.zip)
* [Déploiement du lot DevelopingWithServiceUser](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip)
* Ajoutez l’entrée suivante dans le service Apache Sling Service User Mapper à l’aide de la configurationMgr OSGi.
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service**

## Déploiement de l’exemple d’application de réaction

* [Téléchargez l’exemple d’application de réaction](assets/mult-step-form1.zip)
* Décompressez le contenu de l’application de réaction dans un nouveau dossier.
* Accédez au dossier et exécutez les commandes suivantes

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

Pour activer l’envoi d’appels POST vers le point de terminaison AEM à partir de votre application REACT, vous devez spécifier les entrées appropriées dans le champ Origines autorisées dans la configuration Adobe Granite Cross-Origin Resource Sharing Policy.

![définition des cordes](assets/cors-settings.png)

Voir [Présentation de la norme CORS avec AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/foundation/security/understand-cross-origin-resource-sharing.html?lang=fr) pour plus d’informations sur les options de configuration CORS.
