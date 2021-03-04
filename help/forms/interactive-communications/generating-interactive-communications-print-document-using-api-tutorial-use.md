---
title: Génération d'un Document de communications interactives pour un canal d'impression à l'aide du mécanisme du dossier de contrôle
seo-title: Génération d'un Document de communications interactives pour un canal d'impression à l'aide du mécanisme du dossier de contrôle
description: Utiliser le dossier de contrôle pour générer des documents de canal d’impression
seo-description: Utiliser le dossier de contrôle pour générer des documents de canal d’impression
feature: Communication interactive
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '487'
ht-degree: 1%

---


# Génération d&#39;un Document de communications interactives pour un canal d&#39;impression à l&#39;aide du mécanisme du dossier de contrôle

Après avoir conçu et testé votre document de canal d’impression, vous devrez généralement générer le document en effectuant un appel REST ou en générant des documents d’impression à l’aide du mécanisme de dossier de contrôle.

Cet article explique comment générer des documents de canal d’impression à l’aide du mécanisme de dossier de contrôle.

Lorsque vous déposez un fichier dans le dossier de contrôle, un script associé au dossier de contrôle est exécuté. Ce script est expliqué dans l&#39;article ci-dessous.

Le fichier déposé dans le dossier de contrôle possède la structure suivante. Le code génère des instructions pour tous les numéros de comptes répertoriés dans le document XML.

&lt;accountnumbers>

&lt;accountnumber>509840&lt;/accountnumber>

&lt;accountnumber>948576&lt;/accountnumber>

&lt;accountnumber>398762&lt;/accountnumber>

&lt;accountnumber>291723&lt;/accountnumber>

&lt;/accountnumbers>

Le code ci-dessous répertorie les éléments suivants :

Ligne 1 - Chemin d&#39;accès au document InteractiveCommunications

Lignes 15 à 20 : Obtenir la liste des numéros de compte du document XML déposée dans le dossier de contrôle

Lignes 24 à 25 : Obtenez les Canaux PrintChannelService et Print associés au document.

Ligne 30 : Transmettez le numéro de compte en tant qu’élément clé au modèle de données de formulaire.

Lignes 32 à 36 : Définissez les options de données pour le Document à générer.

Ligne 38 : Générer le document.

Lignes 39 à 40 - Enregistre le document généré dans le système de fichiers.

Le point de terminaison REST du modèle de données de formulaire attend un id comme paramètre d’entrée. cet ID est associé à un attribut de requête appelé numéro de compte, comme illustré dans la capture d’écran ci-dessous.

![request](assets/requestattributeprintchannel.gif)

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


**Pour tester cette fonctionnalité sur votre système local, suivez les instructions suivantes :**

* Configurez Tomcat comme décrit dans cet [article.](/help/forms/ic-print-channel-tutorial/set-up-tomcat.md) Tomcat possède le fichier war qui génère les données d&#39;exemple.
* Configurez le service comme utilisateur système tel que décrit dans cet [article](/help/forms/adaptive-forms/service-user-tutorial-develop.md).
Assurez-vous que cet utilisateur système dispose des autorisations de lecture sur le noeud suivant. Pour ouvrir une session d&#39;autorisation pour [l&#39;utilisateur admin](https://localhost:4502/useradmin) et rechercher l&#39;utilisateur système &quot;data&quot; et donner les autorisations de lecture sur le noeud suivant en appuyant sur l&#39;onglet permissions
   * /content/dam/formsanddocuments
   * /content/dam/formsanddocuments-fdm
   * /content/forms/af
* Importez le ou les packages suivants dans AEM à l’aide du gestionnaire de packages. Ce package contient les éléments suivants :


* [Exemple de Document de communications interactives](assets/retirementstatementprint.zip)
* [Script du dossier de contrôle](assets/printchanneldocumentusingwatchedfolder.zip)
* [Configuration de la source de données](assets/datasource.zip)

* Ouvrez le fichier /etc/fd/watchfolder/scripts/PrintPDF.ecma. Assurez-vous que le chemin d’accès à interactiveCommunicationsDocument à la ligne 1 pointe vers le bon document d’impression.

* Modifiez saveLocation en fonction de vos préférences sur la ligne 2.

* Créez le fichier accountnuméros.xml avec le contenu suivant

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


* Déposez le fichier accountnuméros.xml dans le dossier C:\RenderPrintChannel\input folder.

* Les fichiers PDF générés sont écrits dans saveLocation comme spécifié dans le script ecma.

>[!NOTE]
>
>Si vous prévoyez d&#39;utiliser cette méthode sur un système d&#39;exploitation autre que Windows, accédez à
>
>/etc/fd/watchfolder /config/PrintChannelDocument et modifiez folderPath en fonction de vos préférences

