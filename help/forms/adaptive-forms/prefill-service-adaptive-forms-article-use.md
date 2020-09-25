---
title: Service de préremplissage dans Forms adaptatif
seo-title: Service de préremplissage dans Forms adaptatif
description: Préremplissage des formulaires adaptatifs en récupérant des données à partir de sources de données d’arrière-plan.
seo-description: Préremplissage des formulaires adaptatifs en récupérant des données à partir de sources de données d’arrière-plan.
sub-product: formulaires
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
uuid: 26a8cba3-7921-4cbb-a182-216064e98054
discoiquuid: 936ea5e9-f5f0-496a-9188-1a8ffd235ee5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 7%

---


# Utilisation du service Prefill dans Forms adaptatif

Vous pouvez préremplir les champs d’un formulaire adaptatif à l’aide de données existantes. Lorsqu’un utilisateur ouvre un formulaire, les valeurs de ces champs sont préremplies. Il existe plusieurs façons de préremplir les champs de formulaires adaptatifs. Dans cet article, nous allons examiner le préremplissage d’un formulaire adaptatif à l’aide du service de préremplissage AEM Forms.

Pour en savoir plus sur les différentes méthodes de préremplissage des formulaires adaptatifs, [suivez cette documentation.](https://helpx.adobe.com/experience-manager/6-4/forms/using/prepopulate-adaptive-form-fields.html#AEMFormsprefillservice)

Pour préremplir un formulaire adaptatif à l’aide du service de préremplissage, vous devez créer une classe qui implémente l’interface DataProvider. La méthode getPrefillData aura la logique de créer et de renvoyer des données que le formulaire adaptatif utilisera pour préremplir les champs. Cette méthode vous permet de récupérer les données de n’importe quelle source et de renvoyer le flux d’entrée du document de données. L’exemple de code suivant récupère les informations de profil utilisateur de l’utilisateur connecté et construit un document XML dont le flux d’entrée est renvoyé pour être utilisé par les formulaires adaptatifs.

Dans le fragment de code ci-dessous, nous avons une classe qui implémente l&#39;interface DataProvider. Nous obtenons l&#39;accès à l&#39;utilisateur connecté, puis récupérons les informations de profil de l&#39;utilisateur connecté. Nous créons ensuite un document XML avec un élément de noeud racine appelé &quot;data&quot; et ajoutons les éléments appropriés à ce noeud de données. Une fois le document XML créé, le flux d’entrée du document XML est renvoyé.

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

Pour tester cette fonctionnalité sur votre serveur, effectuez les opérations suivantes :

* [Téléchargez et extrayez le contenu du fichier zip sur votre ordinateur.](assets/prefillservice.zip)
* Assurez-vous que les informations de profil [de l’](http://localhost:4502/libs/granite/security/content/useradmin) utilisateur connecté sont complètement renseignées. Il s’agit d’une condition requise pour que l’échantillon fonctionne. L’exemple ne comporte aucune vérification d’erreur des propriétés de profil d’utilisateur manquantes.
* Déployez le lot à l’aide de la console Web [AEM.](http://localhost:4502/system/console/bundles)
* Créer un formulaire adaptatif à l’aide du schéma XSD
* Associez &quot;Service de préremplissage personnalisé de formulaires Aem&quot; au service de préremplissage de votre formulaire adaptatif.
* Faire glisser des éléments de schéma vers le formulaire
* Prévisualiser le formulaire

>[!NOTE]
>
>Si le formulaire adaptatif est basé sur XSD, assurez-vous que le document XML renvoyé par le service de préremplissage correspond au schéma XSD sur lequel votre formulaire adaptatif est basé.
>
>Si le formulaire adaptatif n’est pas basé sur XSD, vous devrez lier manuellement les champs. Par exemple, pour lier un champ de formulaire adaptatif à un élément fname dans les données XML que vous utiliserez dans la référence Lier `/data/fname` du champ de formulaire adaptatif.

