---
title: Services utilitaires utiles
description: Quelques services utiles pour les développeurs AEM Forms
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '160'
ht-degree: 5%

---


# Services utilitaires utiles

Cet exemple de lot fournit des services utilitaires utiles qui peuvent être utilisés par un développeur AEM Forms. Les services suivants sont disponibles.


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

Voici le code qui a été utilisé dans la page JSP pour créer org.w3c.dom.Document à partir d’une chaîne et convertir le document et le stocker dans le référentiel CRX, comme le montre le fragment de code suivant.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Conditions préalables


Vous devez déployer [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) et début le lot.


Si vous souhaitez enregistrer des documents dans le référentiel CRX à l’aide de ce service d’utilitaire, suivez l’[développement avec l’utilisateur du service ](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=en#adaptive-forms). Assurez-vous de fournir à l’utilisateur du service fd les autorisations [requises](http://localhost:4502/useradmin) sur les dossiers CRX appropriés.

