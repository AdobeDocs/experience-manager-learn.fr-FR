---
title: Diffusion d’un document de communication interactive - AEM Forms de canal web
description: Diffusion du document du canal web via un lien dans un email
feature: Interactive Communication
audience: developer
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 50858100-3d0c-42bd-87b8-f626212669e2
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '277'
ht-degree: 0%

---

# Diffusion par e-mail d’un document de canal web

Une fois que vous avez défini et testé votre document de communication interactive de canal web, vous avez besoin d’un mécanisme de diffusion pour diffuser le document de canal web au destinataire.

Dans cet article, nous examinons l’email comme mécanisme de diffusion pour le document de canal web. Le destinataire obtient un lien vers le document de canal web par courrier électronique. En cliquant sur le lien, l’utilisateur est invité à s’authentifier et le document de canal web est renseigné avec les données propres à l’utilisateur connecté.

Regardons le fragment de code suivant. Ce code fait partie de GET.jsp qui est déclenché lorsque l’utilisateur clique sur le lien dans le courrier électronique pour afficher le document du canal web. Nous obtenons l’utilisateur connecté à l’aide de jackrabbit UserManager. Une fois que nous obtenons l’utilisateur connecté, nous obtenons la valeur de la propriété accountNumber associée au profil de l’utilisateur.

Nous associons ensuite la valeur accountNumber à une clé appelée account number dans le mappage. La clé **accountnumber** est défini dans le modal de données de formulaire sous la forme d’un attribut de requête. La valeur de cet attribut est transmise en tant que paramètre d’entrée à la méthode de service de lecture du modèle de données de formulaire.

Ligne 7 : Nous envoyons la demande reçue à un autre servlet, en fonction du type de ressource identifié par l’URL du document de communication interactive. La réponse renvoyée par ce deuxième servlet est incluse dans la réponse du premier servlet.

```java
org.apache.jackrabbit.api.security.user.UserManager um = ((org.apache.jackrabbit.api.JackrabbitSession) session).getUserManager();
org.apache.jackrabbit.api.security.user.Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
String accountNumber = loggedinUser.getProperty("profile/accountNumber")[0].getString();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,"GET");
wrapperRequest.getRequestDispatcher("/content/forms/af/401kstatement/irastatement/channels/web.html").include(wrapperRequest, response);
```

![Approche de la méthode d’inclusion](assets/includemethod.jpg)

Représentation visuelle du code Ligne 7

![Configuration du paramètre de requête](assets/requestparameter.png)

Attribut de requête défini pour le service de lecture du modal de données de formulaire.

[Exemple de module AEM](assets/webchanneldelivery.zip).
