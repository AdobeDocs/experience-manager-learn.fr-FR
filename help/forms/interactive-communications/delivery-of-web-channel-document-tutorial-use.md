---
title: Diffuser un document de communication interactive - Canal web AEM Forms
description: Diffuser un document de canal web par le biais d’un lien dans un e-mail
feature: Interactive Communication
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
last-substantial-update: 2019-07-07T00:00:00Z
duration: 85
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 100%

---

# Diffusion par e-mail d’un document de canal web

Une fois que vous avez défini et testé votre document de communication interactive de canal web, vous avez besoin d’un mécanisme de diffusion pour diffuser le document de canal web à la personne destinataire.

Cet article se penche sur l’e-mail comme mécanisme de diffusion pour les documents de canal web. La personne destinataire obtient un lien vers le document de canal web par e-mail. En cliquant sur le lien, elle est invitée à s’authentifier et le document de canal web est renseigné avec les données propres à la personne utilisatrice connectée.

Regardons l’extrait de code suivant. Ce code fait partie de GET.jsp, qui est déclenché lorsque l’utilisateur ou l’utilisatrice clique sur le lien dans l’e-mail afin d’afficher le document de canal web. La personne utilisatrice connectée est récupérée à l’aide de Jackrabbit UserManager. Une fois la personne utilisatrice connectée obtenue, la valeur de la propriété accountNumber associée au profil de cette personne est récupérée.

La valeur accountNumber peut ensuite être associée à une clé appelée accountnumber dans le mappage. La clé **accountnumber** est définie dans le modèle de données de formulaire sous la forme d’un attribut de requête. La valeur de cet attribut est transmise en tant que paramètre d’entrée à la méthode de service de lecture du modèle de données de formulaire.

Ligne 7 : la requête reçue est transmise à un autre servlet, en fonction du type de ressource identifié par l’URL du document de communication interactive. La réponse renvoyée par ce deuxième servlet est incluse dans la réponse du premier servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![Approche de la méthode d’inclusion.](assets/includemethod.jpg)

Représentation visuelle du code à la ligne 7

![Configuration du paramètre de requête.](assets/requestparameter.png)

Attribut de requête défini pour le service de lecture du modèle de données de formulaire.

[Exemple de package AEM](assets/webchanneldelivery.zip).
