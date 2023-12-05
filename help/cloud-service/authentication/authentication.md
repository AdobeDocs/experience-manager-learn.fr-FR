---
title: Authentification dans AEM as a Cloud Service
description: Découvrez l’authentification dans AEM as a Cloud Service.
version: Cloud Service
feature: Security
topic: Development, Integrations, Security
role: Architect, Developer
level: Intermediate
jira: KT-10436
thumbnail: KT-10436.png
last-substantial-update: 2022-10-14T00:00:00Z
exl-id: 4dba6c09-2949-4153-a9bc-d660a740f8f7
duration: 58
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 100%

---

# Authentification dans AEM as a Cloud Service

AEM as a Cloud Service prend en charge plusieurs types d’authentification, en fonction du service choisi.

|                       | Création AEM | Publication AEM |
|-----------------------|:----------:|:-----------:|
| [Adobe IMS](../accessing/overview.md) | ✔ | ✘ |
| • [SAML 2.0 via Adobe IMS](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/security/ims-support.html?lang=fr#how-to-set-up) | ✔ | ✘ |
| [SAML 2.0](./saml-2-0.md) | ✘ | ✔ |
| [Authentification unique (SSO)](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html?lang=fr#integration-with-an-idp) | ✘ | ✔ |
| [Oauth](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/authoring/personalization/user-and-group-sync-for-publish-tier.html?lang=fr#integration-with-an-idp) | ✘ | ✔ |
| [Authentification par jeton](../../headless-tutorial/authentication/overview.md) | ✔ | ✔ |

## Options d’authentification

Cliquez sur le lien correspondant ci-dessous pour obtenir plus d’informations sur la configuration et l’utilisation de l’approche d’authentification.

<table>
  <tr>
   <td>
      <a  href="../accessing/overview.md"><img alt="Adobe IMS" src="./assets/card--adobe-ims.png"/></a>
      <div><strong><a href="../accessing/overview.md">Adobe IMS</a></strong></div>
      <p>
          Gérez l’accès à l’instance de création AEM à l’aide d’Adobe IMS via Adobe Admin Console.
      </p>
    </td>   
   <td>
      <a  href="./saml-2-0.md"><img alt="SAML 2.0" src="./assets/card--saml-2-0.png"/></a>
      <div><strong><a href="./saml-2-0.md">SAML 2.0</a></strong></div>
      <p>
        Authentifiez l’utilisateur ou l’utilisatrice de votre site web auprès d’un fournisseur d’identité à l’aide de l’intégration SAML 2.0 du service de publication AEM.
      </p>
    </td>   
   <td>
      <a  href="../../headless-tutorial/authentication/overview.md"><img alt="Jeton" src="./assets/card--token.png"/></a>
      <div><strong><a href="../../headless-tutorial/authentication/overview.md">Authentification par jeton</a></strong></div>
      <p>
        Permet aux applications et aux middleware de s’authentifier auprès d’AEM à l’aide d’un jeton de service API.
      </p>
    </td>   
  </tr>
</table>
