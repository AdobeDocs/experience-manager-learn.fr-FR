---
title: Développement de périmètres OAuth dans AEM
description: Adobe Experience Manager des étendues OAuth extensibles permet un contrôle d’accès des ressources provenant d’une application cliente autorisée par un utilisateur final. Le diagramme ci-dessous illustre le flux de requêtes dans le contexte d’AEM.
version: 6.3, 6.4, 6.5
feature: Utilisateurs et groupes
topic: Développement
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---


# Développement des périmètres OAuth

Adobe Experience Manager des portées OAuth extensibles permettent un contrôle d’accès des ressources provenant d’une application cliente autorisée par un utilisateur final. Le diagramme ci-dessous illustre le flux de requêtes dans le contexte d’AEM.

![Flux de périmètres Oauth](./assets/oauth-code-sample-develop/oauth-scopes-flow.png)

AEM fournit trois portées :

* Profil
* Accès hors ligne
* Répliquer

AEM les portées OAuth extensibles permettent de définir d’autres portées personnalisées. Par exemple, une portée personnalisée peut être développée et déployée vers AEM qui permet à une application mobile autorisée via OAuth d’être limitée à la lecture, mais pas à l’écriture de ressources.

OAuth est la méthode privilégiée pour autoriser une application cliente, car elle utilise un jeton d’accès au lieu d’exiger que les informations d’identification d’un utilisateur AEM soient fournies à cette application.

* [Afficher le code](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/legacy/bundle/src/main/java/com/adobe/acs/samples/authentication/oauth/impl/SampleScopeWithPrivileges.java)
