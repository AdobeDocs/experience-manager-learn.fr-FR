---
title: Génération d’un document de communications interactives pour le canal d’impression à l’aide d’un mécanisme de dossier de contrôle
description: Utilisez un dossier de contrôle pour générer des documents du canal d’impression.
feature: Interactive Communication
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: f5ab4801-cde5-426d-bfe4-ce0a985e25e8
last-substantial-update: 2019-07-07T00:00:00Z
duration: 161
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 100%

---

# Génération d’un document de communications interactives pour le canal d’impression à l’aide d’un mécanisme de dossier de contrôle

Après avoir conçu et testé votre document de canal d’impression, vous devez généralement générer le document en effectuant un appel REST ou en générant des documents d’impression à l’aide du mécanisme de dossier de contrôle.

Cet article explique le cas d’utilisation de la génération de documents de canal d’impression à l’aide du mécanisme de dossier de contrôle.

Lorsque vous déposez un fichier dans le dossier de contrôle, un script associé au dossier de contrôle est exécuté. Ce script est expliqué dans l’article ci-dessous.

Le fichier déposé dans le dossier de contrôle possède la structure suivante. Le code génère des instructions pour tous les numéros de compte répertoriés dans le document XML.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

La liste de code ci-dessous effectue les opérations suivantes :

Ligne 1 : chemin d’accès au document de communications interactives

Lignes 15 à 20 : obtenir la liste des numéros de compte à partir du document XML déposé dans le dossier de contrôle

Lignes 24 à 25 : obtenir le service de canal d’impression et le canal d’impression associés au document.

Ligne 30 : transmettre le numéro de compte comme élément clé au modèle de données de formulaire.

Lignes 32 à 36 : définir les Options de données du document à générer.

Ligne 38 : effectuer le rendu du document.

Lignes 39 à 40 : enregistrer le document généré dans le système de fichiers.

Le point d’entrée REST du modèle de données de formulaire attend un identifiant comme paramètre d’entrée. Cet identifiant est mappé à un attribut de requête appelé numéro de compte, comme illustré dans la copie d’écran ci-dessous.

![requestatattribute](assets/requestattributeprintchannel.gif)

```java
var interactiveCommunicationsDocument = "/content/forms/af/retirementstatementprint/channels/print/";
var saveLocation =  new Packages.java.io.File("c:\\scrap\\loadtesting");

if(!saveLocation.exists())
{
 saveLocation.mkdirs();
}

var inputMap = processorContext.getInputMap();
var entry = inputMap.entrySet().iterator().next();
var inputDocument = inputMap.get(entry.getKey());
var aemDemoListings = sling.getService(Packages.com.mergeandfuse.getserviceuserresolver.GetResolver);
var resourceResolver = aemDemoListings.getServiceResolver();
var resourceResolverHelper = sling.getService(Packages.com.adobe.granite.resourceresolverhelper.ResourceResolverHelper);
var dbFactory = Packages.javax.xml.parsers.DocumentBuilderFactory.newInstance();
var dBuilder = dbFactory.newDocumentBuilder();
var xmlDoc = dBuilder.parse(inputDocument.getInputStream());
var nList = xmlDoc.getElementsByTagName("accountnumber");
for(var i=0;i<nList.getLength();i++)
{
 var accountnumber = nList.item(i).getTextContent();
resourceResolverHelper.callWith(resourceResolver, {call: function()
       {
   var printChannelService = sling.getService(Packages.com.adobe.fd.ccm.channels.print.api.service.PrintChannelService);
   var printChannel = printChannelService.getPrintChannel(interactiveCommunicationsDocument);
   var options = new Packages.com.adobe.fd.ccm.channels.print.api.model.PrintChannelRenderOptions();
   options.setMergeDataOnServer(true);
   options.setRenderInteractive(false);
   var map = new Packages.java.util.HashMap();
   map.put("accountnumber",accountnumber);
    // Required Data Options
   var dataOptions = new Packages.com.adobe.forms.common.service.DataOptions(); 
   dataOptions.setServiceName(printChannel.getPrefillService()); 
   dataOptions.setExtras(map); 
   dataOptions.setContentType(Packages.com.adobe.forms.common.service.ContentType.JSON);
   dataOptions.setFormResource(resourceResolver.resolve(interactiveCommunicationsDocument));
            options.setDataOptions(dataOptions); 
    var printDocument = printChannel.render(options);
   var statement = new Packages.com.adobe.aemfd.docmanager.Document(printDocument.getInputStream());
            statement.copyToFile(new Packages.java.io.File(saveLocation+"\\"+accountnumber+".pdf"));

      }
   });
}
```


**Pour le tester sur votre système local, suivez les instructions suivantes :**

* Configurez Tomcat comme décrit dans cet [article.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat possède le fichier war qui génère les données d’exemple.
* Configurez le service, c’est-à-dire la personne utilisatrice système, comme décrit dans cet [article](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Assurez-vous que cette personne utilisatrice système dispose des autorisations de lecture sur le nœud suivant. Pour accorder les autorisations, connectez-vous à la [personne en charge de l’administration](https://localhost:4502/useradmin) et recherchez les « données » de la personne utilisatrice système et attribuez les autorisations de lecture sur le nœud suivant en accédant à l’onglet Autorisations.
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importez le ou les packages suivants dans AEM à l’aide du gestionnaire de modules. Ce package contient les éléments suivants :


* [Exemple de document de communications interactives](assets/retirementstatementprint.zip)
* [Script de dossier de contrôle](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configuration de la source de données](assets/datasource.zip)

* Ouvrez le fichier /etc/fd/watchfolder/scripts/PrintPDF.ecma. Assurez-vous que le chemin d’accès au document de communications interactives dans la ligne 1 pointe vers le document correct que vous souhaitez imprimer.

* Modifiez l’emplacement d’enregistrement en fonction de vos préférences sur la ligne 2.

* Créez un fichier accountnumbers.xml avec le contenu suivant.

```xml
<accountnumbers>
<accountnumber>1</accountnumber>
<accountnumber>100</accountnumber>
<accountnumber>101</accountnumber>
<accountnumber>1009</accountnumber>
<accountnumber>10009</accountnumber>
<accountnumber>11990</accountnumber>
</accountnumbers>
```


* Déposez le fichier accountnuméros.xml dans le dossier C:\RenderPrintChannel\input.

* Les fichiers PDF générés sont écrits dans l’emplacement d’enregistrement comme spécifié dans le script ecma.

>[!NOTE]
>
>Si vous prévoyez de l’utiliser sur un système d’exploitation autre que Windows, accédez à
>
>/etc/fd/watchfolder /config/PrintChannelDocument et modifiez le chemin du dossier en fonction de vos préférences.
