---
title: Déploiement de SPA pour AEM GraphQL
description: Découvrez SPA options de déploiement en rapport avec AEM GraphQL, sans affichage.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10587
thumbnail: KT-10587.jpg
mini-toc-levels: 2
source-git-commit: c7d2e69a9039cfbaa43d6d9b65b9fa6f69378716
workflow-type: tm+mt
source-wordcount: '528'
ht-degree: 0%

---


# Déploiement d’un SPA

Dans cette section, nous allons passer en revue une approche de déploiement de SPA (React, Vue, Angular, etc.) qui appelle AEM API GraphQL pour charger les données, cependant, avant de comprendre les artefacts de haut niveau qu’il faut déployer.

**SPA Créer des artefacts d’application :**

Les fichiers générés par la structure SPA, généralement **HTML, CSS, JS**, c’est-à-dire les artefacts de version statique. Dans le cas de l’application React, les artefacts de la variable `build` pour les artefacts Vue du répertoire `dist` répertoire .
La demande adressée à votre SPA (par exemple, https://HOST/my-aem-spa.html) sera diffusée à l’aide de ces artefacts de version.

**AEM API GraphQL :**

Clairement, ce point d’entrée de l’API GraphQL (`/graphql/execute.json/<PROJECT-CONFIG>/<PERSISTED-QUERY-NAME>`) doit être hébergé sur le domaine AEM.

En résumé SPA l’architecture de déploiement comporte deux parties *1. SPA 2. Couche de l’API GraphQL AEM*, examinons donc les options de déploiement pour ces deux parties.


## Options de déploiement

| Option de déploiement | URL SPA | URL de l’API GraphQL AEM | Configuration de la norme CORS requise ? |
| ---------|---------- | ---------|---------- |
| **Même domaine** | https://**HÔTE**/my-aem-spa.html | https://**HÔTE**/graphql/execute.json/... | ✘ |
| **Domaine différent** | https://**SPA-HOST**/my-aem-spa.html | https://**AEM-HOST**/graphql/execute.json/... | ✔ |

**Même domaine :**\
Les *Couche de l’API GraphQL SPA et AEM* dans cette option sont déployées sur le **Même domaine**. Signification d’une requête SPA URI `/my-aem-spa.html` Couche de l’API &amp; GraphQL `/graphql/execute.json/` sont diffusés à partir du même domaine.

**Différent de domaines :**\
Les *Couche de l’API GraphQL SPA et AEM* dans cette option, déployez sur **Domaine différent**. Signification d’une requête SPA URI `/my-aem-spa.html` est diffusé à partir d’un **domaine différent** couche de l’API GraphQL `/graphql/execute.json/` requêtes. Notez que dans le cadre de cette option de déploiement, vous devez [configuration de CORS](cors.md) sur l’instance AEM.

>[!NOTE]
>
>Vous DEVEZ correctement configurer CORS sur l’instance AEM, [voir les étapes ici](cors.md).

### Déploiement sur un même domaine

Lors du déploiement sur le même domaine, le domaine peut être une **Principal domaine AEM** (Sur le domaine AEM) ou **Principal domaine SPA** (Hors domaine d’AEM) et les requêtes d’API SPA entrantes et GraphQL peuvent être fractionnées dans n’importe quel composant de déploiement tel que CDN ([Fastly](https://docs.fastly.com/en/guides/routing-assets-to-different-origins), Akamai, [CloudFront](https://aws.amazon.com/premiumsupport/knowledge-center/cloudfront-distribution-serve-content/)), [HTTPD avec proxy inverse](https://httpd.apache.org/docs/2.4/howto/reverse_proxy.html). En d’autres termes, vous déployez toujours les artefacts de SPA version et les API AEM GraphQL sur différents serveurs, mais pour les utilisateurs finaux, ceux-ci sont fournis à partir d’un seul domaine et derrière la scène acheminés vers un autre serveur de destination ou d’origine.

En outre, SPA artefacts de création peuvent être hébergés dans AEM *mais ne sont pas recommandées.*

| Même déploiement de domaine | Partage CDN | HTTPD + Reverse Proxy | AEM d’artefacts SPA hébergés |
| ---------|---------- | ---------|---------- |
| **SUR Domaine AEM** | ✔ | ✔ | ✔ |
| **Hors domaine AEM** | ✔ | ✔ | **S/O** |


**HTTPD + Reverse Proxy**

Voici un exemple de configuration :

>[!TIP]
>
> Les configurations suivantes sont des exemples. Veillez à les ajuster pour qu’elles s’alignent sur les exigences de votre projet.

SUR LE DOMAINE AEM

    &quot;
    ProxyPass &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    ProxyPassReverse &quot;/${YOUR-SPA-URI}&quot; &quot;http://${SPA-HOST}/&quot;
    &quot;

Hors domaine AEM

    &quot;
    ProxyPass &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    ProxyPassReverse &quot;/graphql/execute.json/&quot; &quot;http://${AEM-HOST}/&quot;
    &quot;




### Déploiement sur différents domaines

Dans ce scénario, SPA création d’artefacts sont déployés sur un domaine différent du domaine des API GraphQL AEM. Pour les utilisateurs finaux, ils sont fournis à partir de deux domaines distincts. [Configuration CORS](cors.md) est MUST sur AEM.

**Demandes d’application SPA via différents domaines**

![Différents domaines SPA diffusion](assets/spa/different-domain-spa-delivery.png)


**En-tête de réponse CORS sur l’API AEM GraphQL**

![API d’en-tête de réponse CORS AEM GraphQL](assets/spa/CORS-response-header-aem-graphql-api.png)


