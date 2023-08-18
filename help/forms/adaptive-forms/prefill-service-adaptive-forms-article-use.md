---
title: Service de préremplissage des formulaires adaptatifs
description: Préremplir les formulaires adaptatifs en récupérant des données à partir de sources de données principales.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2021-11-27T00:00:00Z
source-git-commit: cf37afeb9bea65b540c9cfde75070d4106a01976
workflow-type: tm+mt
source-wordcount: '459'
ht-degree: 6%

---

# Utilisation du service de préremplissage dans Forms adaptatif

Vous pouvez préremplir les champs d’un formulaire adaptatif à l’aide de données existantes. Lorsqu’un utilisateur ou une utilisatrice ouvre un formulaire, les valeurs de ces champs sont préremplies. Il existe plusieurs façons de préremplir des champs de formulaires adaptatifs. Dans cet article, nous allons examiner le préremplissage d’un formulaire adaptatif à l’aide du service de préremplissage AEM Forms.

Pour en savoir plus sur les différentes méthodes de préremplissage de formulaires adaptatifs, [veuillez suivre cette documentation](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Pour préremplir un formulaire adaptatif à l’aide du service de préremplissage, vous devez créer une classe qui implémente la fonction `com.adobe.forms.common.service.DataXMLProvider` . La méthode `getDataXMLForDataRef` disposera de la logique nécessaire pour créer et renvoyer les données que le formulaire adaptatif utilisera pour préremplir les champs. Dans cette méthode, vous pouvez récupérer les données de n’importe quelle source et renvoyer le flux d’entrée du document de données. L’exemple de code suivant récupère les informations de profil utilisateur de l’utilisateur connecté et crée un document XML dont le flux d’entrée est renvoyé pour être utilisé par les formulaires adaptatifs.

Dans le fragment de code ci-dessous, nous avons une classe qui implémente l’interface DataXMLProvider . Nous avons accès à l’utilisateur connecté, puis nous récupérons les informations de profil de l’utilisateur connecté. Nous créons ensuite un document XML avec un élément de noeud racine appelé &quot;data&quot; et ajoutons les éléments appropriés à ce noeud de données. Une fois le document XML construit, le flux d’entrée du document XML est renvoyé.

Cette classe est ensuite transformée en lot OSGi et déployée dans AEM. Une fois le lot déployé, ce service de préremplissage est alors disponible pour être utilisé comme service de préremplissage de votre formulaire adaptatif.

>[!NOTE]
>
>Vous pouvez préremplir un formulaire à l’aide de données xml ou json en appliquant l’approche répertoriée dans cet article .

```java
package com.aem.prefill.core;
import com.adobe.forms.common.service.DataXMLOptions;
import com.adobe.forms.common.service.DataXMLProvider;
import com.adobe.forms.common.service.FormsException;
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.FileOutputStream;
import java.io.InputStream;
import javax.jcr.Session;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;
import javax.xml.transform.Transformer;
import javax.xml.transform.TransformerFactory;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.stream.StreamResult;
import org.apache.jackrabbit.api.JackrabbitSession;
import org.apache.jackrabbit.api.security.user.Authorizable;
import org.apache.jackrabbit.api.security.user.UserManager;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.w3c.dom.Document;
import org.w3c.dom.Element;

@Component
public class PrefillAdaptiveForm implements DataXMLProvider {
  private static final Logger log = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

  @Override
  public String getServiceDescription() {

    return "Custom AEM Forms PreFill Service";
  }

  @Override
  public String getServiceName() {

    return "CustomAemFormsPrefillService";
  }

  @Override
  public InputStream getDataXMLForDataRef(DataXMLOptions dataXmlOptions) throws FormsException {
    InputStream xmlDataStream = null;
    Resource aemFormContainer = dataXmlOptions.getFormResource();
    ResourceResolver resolver = aemFormContainer.getResourceResolver();
    Session session = (Session) resolver.adaptTo(Session.class);
    try {
      UserManager um = ((JackrabbitSession) session).getUserManager();
      Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
      log.debug("The path of the user is" + loggedinUser.getPath());
      DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
      DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
      Document doc = docBuilder.newDocument();
      Element rootElement = doc.createElement("data");
      doc.appendChild(rootElement);

      if (loggedinUser.hasProperty("profile/givenName")) {
        Element firstNameElement = doc.createElement("fname");
        firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
        rootElement.appendChild(firstNameElement);
        log.debug("Created firstName Element");
      }

      if (loggedinUser.hasProperty("profile/familyName")) {
        Element lastNameElement = doc.createElement("lname");
        lastNameElement.setTextContent(loggedinUser.getProperty("profile/familyName")[0].getString());
        rootElement.appendChild(lastNameElement);
        log.debug("Created lastName Element");

      }
      if (loggedinUser.hasProperty("profile/email")) {
        Element emailElement = doc.createElement("email");
        emailElement.setTextContent(loggedinUser.getProperty("profile/email")[0].getString());
        rootElement.appendChild(emailElement);
        log.debug("Created email Element");

      }

      TransformerFactory transformerFactory = TransformerFactory.newInstance();
      ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
      Transformer transformer = transformerFactory.newTransformer();
      DOMSource source = new DOMSource(doc);
      StreamResult outputTarget = new StreamResult(outputStream);
      TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
      if (log.isDebugEnabled()) {
        FileOutputStream output = new FileOutputStream("afdata.xml");
        StreamResult result = new StreamResult(output);
        transformer.transform(source, result);
      }

      xmlDataStream = new ByteArrayInputStream(outputStream.toByteArray());
      return xmlDataStream;
    } catch (Exception e) {
      log.error("The error message is " + e.getMessage());
    }
    return null;

  }

}
```

Pour tester cette fonctionnalité sur votre serveur, procédez comme suit :

* Assurez-vous d’être connecté [profil de l’utilisateur](http://localhost:4502/security/users.html) Les informations sont renseignées. L’exemple recherche les propriétés FirstName, LastName et Email de l’utilisateur connecté.
* [Téléchargez et extrayez le contenu du fichier zip sur votre ordinateur.](assets/prefillservice.zip)
* Déployez le lot prefill.core-1.0.0-SNAPSHOT à l’aide du [AEM console web](http://localhost:4502/system/console/bundles)
* Importez le formulaire adaptatif à l’aide de l’option Créer | Téléchargement du fichier depuis le [Section FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* Assurez-vous que la variable [formulaire](http://localhost:4502/editor.html/content/forms/af/prefill.html) est utilisé **&quot;Service AEM Forms PreFill personnalisé&quot;** comme service de préremplissage. Cela peut être vérifié à partir des propriétés de configuration de la variable **Conteneur de formulaires** .
* [Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/prefill/jcr:content?wcmmode=disabled). Le formulaire doit être renseigné avec les valeurs correctes.

>[!NOTE]
>
>Si vous avez activé le débogage pour com.aem.prefill.core.PrefillAdaptiveForm, le fichier de données XML généré est écrit dans le dossier d’installation du serveur AEM.

