---
title: Service de préremplissage dans Forms adaptatif
description: Préremplir les formulaires adaptatifs en récupérant des données à partir de sources de données principales.
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f2c324a3-cbfa-4942-b3bd-dc47d8a3f7b5
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 7%

---

# Utilisation du service de préremplissage dans Forms adaptatif

Vous pouvez préremplir les champs d’un formulaire adaptatif à l’aide de données existantes. Lorsqu’un utilisateur ouvre un formulaire, les valeurs de ces champs sont préremplies. Il existe plusieurs façons de préremplir des champs de formulaires adaptatifs. Dans cet article, nous allons examiner le préremplissage d’un formulaire adaptatif à l’aide du service de préremplissage AEM Forms.

Pour en savoir plus sur les différentes méthodes de préremplissage de formulaires adaptatifs, [veuillez suivre cette documentation](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Pour préremplir un formulaire adaptatif à l’aide du service de préremplissage, vous devez créer une classe qui implémente l’interface DataProvider. La méthode getPrefillData disposera de la logique nécessaire pour créer et renvoyer les données que le formulaire adaptatif utilisera pour préremplir les champs. Dans cette méthode, vous pouvez récupérer les données de n’importe quelle source et renvoyer le flux d’entrée du document de données. L’exemple de code suivant récupère les informations de profil utilisateur de l’utilisateur connecté et construit un document XML dont le flux d’entrée est renvoyé pour être utilisé par les formulaires adaptatifs.

Dans le fragment de code ci-dessous, nous avons une classe qui implémente l’interface DataProvider. Nous avons accès à l’utilisateur connecté, puis nous récupérons les informations de profil de l’utilisateur connecté. Nous créons ensuite un document XML avec un élément de noeud racine appelé &quot;data&quot; et ajoutons les éléments appropriés à ce noeud de données. Une fois le document XML construit, le flux d’entrée du document XML est renvoyé.

Cette classe est ensuite transformée en lot OSGi et déployée dans AEM. Une fois le lot déployé, ce service de préremplissage est alors disponible pour être utilisé comme service de préremplissage de votre formulaire adaptatif.

```java
public class PrefillAdaptiveForm implements DataProvider {
 private Logger logger = LoggerFactory.getLogger(PrefillAdaptiveForm.class);

 public String getServiceName() {
  return "Default Prefill Service";
 }
 
 public String getServiceDescription() {
  return "This is default prefill service to prefill adaptive form with user data";
 }
 
 public PrefillData getPrefillData(final DataOptions dataOptions) throws FormsException {
  PrefillData prefillData = new PrefillData() {
   public InputStream getInputStream() {
    return getData(dataOptions);
   }
   
   public ContentType getContentType() {
    return ContentType.XML;
   }
  };
  return prefillData;
 }

 private InputStream getData(DataOptions dataOptions) throws FormsException {  
  try {
   Resource aemFormContainer = dataOptions.getFormResource();
   ResourceResolver resolver = aemFormContainer.getResourceResolver();
   Session session = resolver.adaptTo(Session.class);
   UserManager um = ((JackrabbitSession) session).getUserManager();
   Authorizable loggedinUser = um.getAuthorizable(session.getUserID());
   DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
   Document doc = docBuilder.newDocument();
   Element rootElement = doc.createElement("data");
   doc.appendChild(rootElement);
   Element firstNameElement = doc.createElement("fname");
   firstNameElement.setTextContent(loggedinUser.getProperty("profile/givenName")[0].getString());
     .
     .
     .
   InputStream inputStream = new ByteArrayInputStream(rootElement.getTextContent().getBytes());
   return inputStream;
  } catch (Exception e) {
   logger.error("Error while creating prefill data", e);
   throw new FormsException(e);
  }
 }
}
```

Pour tester cette fonctionnalité sur votre serveur, procédez comme suit :

* [Téléchargez et extrayez le contenu du fichier zip sur votre ordinateur.](assets/prefillservice.zip)
* Assurez-vous d’être connecté [profil de l’utilisateur](http://localhost:4502/libs/granite/security/content/useradmin) les informations sont complétées. Ceci est obligatoire pour que l’échantillon fonctionne. L’exemple ne comporte aucune vérification d’erreur pour les propriétés de profil utilisateur manquantes.
* Déployez le lot à l’aide du [AEM console web](http://localhost:4502/system/console/bundles)
* Créer un formulaire adaptatif à l’aide du schéma XSD
* Associez &quot;Service de préremplissage de formulaire Aem personnalisé&quot; comme service de préremplissage de votre formulaire adaptatif
* Faire glisser des éléments de schéma vers le formulaire
* Prévisualiser le formulaire

>[!NOTE]
>
>Si le formulaire adaptatif est basé sur XSD, assurez-vous que le document XML renvoyé par le service de préremplissage correspond au schéma XSD sur lequel votre formulaire adaptatif est basé.
>
>Si le formulaire adaptatif n’est pas basé sur XSD, vous devrez lier manuellement les champs. Par exemple, pour lier un champ de formulaire adaptatif à un élément fname dans les données XML que vous utiliserez `/data/fname`  dans la référence de liaison du champ de formulaire adaptatif.
