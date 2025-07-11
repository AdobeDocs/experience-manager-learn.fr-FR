---
title: Créer un composant de page
description: Créer un composant de page
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 7469aa7f-1794-40dd-990c-af5d45e85223
duration: 67
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '273'
ht-degree: 100%

---

# Composant de page

Un composant de page est un composant normal qui se charge du rendu d’une page. Nous allons créer un composant de page et nous allons associer ce composant de page à un nouveau modèle de formulaire adaptatif. Cela garantit que notre code ne sera exécuté que lorsqu’un formulaire adaptatif est basé sur ce modèle particulier.

## Créer un composant de page

Connectez-vous à votre instance AEM Forms locale compatible avec le cloud. Créez la structure suivante sous le dossier des applications.
![page-component](./assets/page-component1.png)

1. Cliquez avec le bouton droit sur le dossier de la page et créez un nœud appelé storeandfetch de type cq:Component.
1. Enregistrez les modifications.
1. Ajoutez les propriétés suivantes au nœud `storeandfetch` et enregistrez.

| **Nom de la propriété** | **Type de propriété** | **Valeur de la propriété** |
|-------------------------|-------------------|----------------------------------------|
| componentGroup | Chaîne | Masqué |
| jcr:description | Chaîne | Type de page de modèle de formulaire adaptatif |
| jcr:title | Chaîne | Page de modèle de formulaire adaptatif |
| sling:resourceSuperType | Chaîne | `fd/af/components/page2/aftemplatedpage` |

Copiez le `/libs/fd/af/components/page2/aftemplatedpage/aftemplatedpage.jsp` et collez-le sous le nœud `storeandfetch`. Renommez le `aftemplatedpage.jsp` en tant que `storeandfetch.jsp`.

Ouvrez `storeandfetch.jsp` et ajoutez la ligne suivante :

```jsp
<cq:include script="azureportal.jsp"/>
```

sous

```jsp
<cq:include script="fallbackLibrary.jsp"/>
```

Le code final doit ressembler à ce qui suit :

```jsp
<cq:include script="fallbackLibrary.jsp"/>
<cq:include script="azureportal.jsp"/>
```

Créez un fichier appelé azureportal.jsp sous le nœud storeandfetch, copiez le code suivant dans le fichier azureportal.jsp et enregistrez les modifications.

```jsp
<%@page session="false" %>
<%@include file="/libs/fd/af/components/guidesglobal.jsp" %>
<%@ page import="org.apache.commons.logging.Log" %>
<%@ page import="org.apache.commons.logging.LogFactory" %>
<%
    if(request.getParameter("guid")!=null) {
            logger.debug( "Got Guid in the request" );
            String BlobId = request.getParameter("guid");
            java.util.Map paraMap = new java.util.HashMap();
            paraMap.put("BlobId",BlobId);
            slingRequest.setAttribute("paramMap",paraMap);
    } else {
            logger.debug( "There is no Guid in the request " );
    }            
%>
```

Dans ce code, nous obtenons la valeur du paramètre de requête **guid** et nous la stockons dans une variable appelée BlobId. Ce BlobId est ensuite transmis dans la requête sling à l’aide de l’attribut paramMap. Pour que ce code fonctionne, il est supposé que vous disposez d’un formulaire basé sur un modèle de données de formulaire pris en charge par Azure Storage et que le service de lecture du modèle de données de formulaire est lié à un attribut de requête appelé BlobId comme illustré dans la capture d’écran ci-dessous.

![fdm-request-attribute](./assets/fdm-request-attribute.png)

### Étapes suivantes

[Associer le composant de page au modèle](./associate-page-component.md)
