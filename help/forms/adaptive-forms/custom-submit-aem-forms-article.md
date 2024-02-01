---
title: Écrire un envoi personnalisé dans AEM Forms
description: Découvrez une méthode rapide et simple pour créer votre propre action d’envoi personnalisée pour le formulaire adaptatif.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 64b586a6-e9ef-4a3d-8528-55646ab03cc4
last-substantial-update: 2021-04-09T00:00:00Z
duration: 59
source-git-commit: b1734f75bdda174788d880be28fa19f8e787af0a
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 88%

---

# Écrire un envoi personnalisé dans AEM Forms {#writing-a-custom-submit-in-aem-forms}

Découvrez une méthode rapide et simple pour créer votre propre action d’envoi personnalisée pour le formulaire adaptatif.

>[!NOTE]
>Le code de cet article ne fonctionne pas avec le formulaire adaptatif basé sur les composants principaux.
>[L’article équivalent pour le formulaire adaptatif basé sur les composants principaux est disponible ici](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/custom-submit-headless-forms/custom-submit-service.html?lang=en)


Cet article décrit comment créer une action d’envoi personnalisée pour gérer l’envoi d’un formulaire adaptatif.

* Ouvrez une session dans CRX.
* Créez un nœud de type « sling:folder » sous « apps ». Appelez ce nœud CustomSubmitHelpx.
* Enregistrez le nouveau nœud.
* Ajoutez les trois propriétés suivantes au nœud.

| Nom de la propriété | Valeur de la propriété |
|----------------    | ---------------------------------|
| guideComponentType | fd/af/components/guidesubmittype |
| guideDataModel | xfa,xsd,basic |
| jcr:description | CustomSubmitHelpx |


* Enregistrez les modifications.
* Créez un fichier nommé post.POST.jsp sous le nœud CustomSubmitHelpx. Lorsqu’un formulaire adaptatif est envoyé, ce JSP est appelé. Vous pouvez écrire le code JSP selon vos besoins dans ce fichier. Le code suivant transfère la demande au servlet.

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

* Créez un fichier appelé addfields .jsp sous le nœud CustomSubmitHelpx. Ce fichier permet d’accéder au document signé.
* Ajoutez le code suivant au fichier :

```java
    <%@include file="/libs/fd/af/components/guidesglobal.jsp"%>

    <%@page import="org.slf4j.LoggerFactory" %>

    <%@page import="org.slf4j.Logger" %>

    <input type="hidden" id="useSignedPdf" name="_useSignedPdf" value=""/>;
```

* Enregistrez vos modifications.

« CustomSubmitHelpx » apparaît désormais dans les actions d’envoi de votre formulaire adaptatif, comme illustré dans cette image.

![Formulaire adaptatif avec envoi personnalisé](assets/capture-2.gif).
