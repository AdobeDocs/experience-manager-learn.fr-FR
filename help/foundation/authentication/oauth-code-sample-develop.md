---
title: Développer des portées OAuth dans AEM
description: Les portées OAuth extensibles d’Adobe Experience Manager permettent un contrôle d’accès des ressources provenant d’une application cliente autorisée par une personne utilisatrice finale. Le diagramme ci-dessous illustre le flux de requêtes dans le contexte d’AEM.
version: Experience Manager 6.4, Experience Manager 6.5
feature: User and Groups
topic: Development
role: Developer
level: Experienced
doc-type: Article
exl-id: dd37355e-cfc7-4581-ac22-d89c951c22cf
duration: 27
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '162'
ht-degree: 100%

---

# Développer des portées OAuth

Les portées OAuth extensibles d’Adobe Experience Manager permettent un contrôle d’accès des ressources provenant d’une application cliente autorisée par une personne utilisatrice finale. Le diagramme ci-dessous illustre le flux de requêtes dans le contexte d’AEM.

![Flux de portées Oauth.](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fournit trois portées :

* Profil
* Accès hors connexion
* Réplication

Les portées OAuth extensibles d’AEM permettent de définir d’autres portées personnalisées. Par exemple, une portée personnalisée peut être développée et déployée vers AEM, permettant à une application mobile autorisée via OAuth d’être limitée à la lecture, mais pas à l’écriture de ressources.

OAuth est la méthode privilégiée pour autoriser une application cliente, car elle utilise un jeton d’accès au lieu d’exiger que les informations d’identification d’un utilisateur ou d’une utilisatrice AEM soient fournies à cette application.

* [Afficher le code](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
