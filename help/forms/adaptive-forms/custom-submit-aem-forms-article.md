---
title: Ecriture d’un envoi personnalisé en AEM Forms
seo-title: Ecriture d’un envoi personnalisé en AEM Forms
description: Méthode simple et rapide de création de votre propre action d’envoi personnalisée pour le formulaire adaptatif
seo-description: Méthode simple et rapide de création de votre propre action d’envoi personnalisée pour le formulaire adaptatif
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '227'
ht-degree: 2%

---


# Ecriture d’un envoi personnalisé en AEM Forms {#writing-a-custom-submit-in-aem-forms}

Méthode simple et rapide de création de votre propre action d’envoi personnalisée pour le formulaire adaptatif

Cet article décrit les étapes nécessaires à la création d’une action d’envoi personnalisée pour la gestion de l’envoi de Forms adaptatif.

* Connexion à crx
* Créez un noeud de type &quot;sling:folder&quot; sous applications. Appelons ce noeud CustomSubmitHelpx.
* Enregistrez le nouveau noeud.
* ajouter les deux propriétés suivantes au noeud que vous venez de créer
* PropertyName       | Valeur de propriété
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* Enregistrez les modifications
* Créez un nouveau fichier nommé post.POST.jsp sous le noeud CustomSubmitHelpx.Lorsqu’un formulaire adaptatif est envoyé, ce JSP est appelé. Vous pouvez écrire le code JSP en fonction de vos besoins dans ce fichier. Le code suivant transfère la demande à la servlet.

```java
<%
%><%@include file="/libs/foundation/global.jsp"%>
<%@taglib prefix="cq" uri="http://www.day.com/taglibs/cq/1.0"%>
<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode,com.adobe.forms.common.submitutils.CustomParameterRequest,com.adobe.aemds.guide.submitutils.*" %>

<%@ page import="org.apache.sling.api.request.RequestParameter,com.day.cq.wcm.api.WCMMode" %>
<%@page session="false" %>
<%

   com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,"/bin/storeafsubmission",null,null);

%>
```

* Créez un fichier appelé addfields .jsp sous le noeud CustomSubmitHelpx. Ce fichier vous permet d&#39;accéder au document signé.
* ajouter le code suivant dans ce fichier

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Enregistrez vos modifications

Vous allez maintenant début de voir &quot;CustomSubmitHelpx&quot; dans les actions d&#39;envoi de votre formulaire adaptatif comme illustré dans cette image.

![Formulaire adaptatif avec envoi personnalisé](assets/capture-2.gif)

