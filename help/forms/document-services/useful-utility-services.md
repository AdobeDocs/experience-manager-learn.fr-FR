---
title: Services d’utilité utiles
description: Quelques services utiles pour les développeurs AEM Forms
feature: Formulaires adaptatifs
version: 6.4,6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 4%

---


# Services d’utilité utiles

Cet exemple de lot fournit des services utiles qui peuvent être utilisés par un développeur AEM Forms. Les services suivants sont disponibles.


```java
package aemformsutilityfunctions.core;
import java.util.Map;
import com.adobe.aemfd.docmanager.Document;
public interface AemFormsUtilities
{
public abstract com.adobe.aemfd.docmanager.Document createDDXFromMapOfDocuments(Map<String, com.adobe.aemfd.docmanager.Document> paramMap);
public abstract org.w3c.dom.Document w3cDocumentFromStrng(String xmlString);
public abstract com.adobe.aemfd.docmanager.Document orgw3cDocumentToAEMFDDocument(org.w3c.dom.Document xmlDocument);
public abstract String saveDocumentInCrx(String jcrPath,String fileExtension, Document documentToSave);

}
```

L’exemple de lot peut être [téléchargé ici](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar)

## Exemple de code pour utiliser le ou les services d’utilitaire

Voici le code utilisé dans la page JSP pour créer org.w3c.dom.Document à partir d’une chaîne, convertir le document et le stocker dans le référentiel CRX, comme illustré dans le fragment de code suivant.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Prérequis


Vous devez déployer [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) et démarrer le lot.


Si vous souhaitez enregistrer des documents dans le référentiel CRX à l’aide de ce service utilitaire, suivez l’ [article concernant le développement avec l’utilisateur de service](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Assurez-vous de fournir les [autorisations requises](http://localhost:4502/useradmin) sur les dossiers CRX appropriés à l’utilisateur du service fd.

