---
title: Écriture d’un envoi personnalisé dans AEM Forms
seo-title: Écriture d’un envoi personnalisé dans AEM Forms
description: Méthode rapide et simple pour créer votre propre action d’envoi personnalisée pour le formulaire adaptatif
seo-description: Méthode rapide et simple pour créer votre propre action d’envoi personnalisée pour le formulaire adaptatif
feature: Formulaires adaptatifs
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5
uuid: a26db0b9-7db4-4e80-813d-5c0438fabd1e
discoiquuid: 28611011-2ff9-477e-b654-e62e7374096a
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 3%

---


# Écriture d’un envoi personnalisé dans AEM Forms {#writing-a-custom-submit-in-aem-forms}

Méthode rapide et simple pour créer votre propre action d’envoi personnalisée pour le formulaire adaptatif

Cet article décrit les étapes nécessaires à la création d’une action d’envoi personnalisée pour gérer l’envoi d’un Forms adaptatif.

* Connexion à crx
* Créez un noeud de type &quot;sling:folder&quot; sous apps. Appelons ce noeud CustomSubmitHelpx.
* Enregistrez le noeud que vous venez de créer.
* Ajoutez les deux propriétés suivantes au noeud que vous venez de créer.
* PropertyName       | Valeur de la propriété
* guideComponentType | fd/af/components/guidesubmittype
* guideDataModel     | xfa,xsd,basic
* jcr:description   | CustomSubmitHelpx
* Enregistrez les modifications
* Créez un nouveau fichier nommé post.POST.jsp sous le noeud CustomSubmitHelpx . Lorsqu’un formulaire adaptatif est envoyé, ce JSP est appelé. Vous pouvez écrire le code JSP selon vos besoins dans ce fichier. Le code suivant transfère la demande au servlet.

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

* Créez un fichier appelé addfields .jsp sous le noeud CustomSubmitHelpx . Ce fichier permet d’accéder au document signé.
* Ajoutez le code suivant à ce fichier.

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Enregistrez vos modifications

Vous allez maintenant voir &quot;CustomSubmitHelpx&quot; dans les actions d’envoi de votre formulaire adaptatif comme illustré dans cette image.

![Formulaire adaptatif avec envoi personnalisé](assets/capture-2.gif)

