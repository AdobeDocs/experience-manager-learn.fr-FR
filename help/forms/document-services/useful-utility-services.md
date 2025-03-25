---
title: Services utilitaires utiles
description: Quelques services utilitaires utiles pour les développeurs et développeuses d’AEM Forms
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: add06b73-18bb-4963-b91f-d8e1eb144842
last-substantial-update: 2020-07-07T00:00:00Z
duration: 35
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Services utilitaires utiles

Cet exemple de lot fournit des services utilitaires qui peuvent être utilisés par un développeur ou une développeuse d’AEM Forms. Les services suivants sont disponibles :


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

L’exemple de lot peut être [téléchargé ici](assets/aemformsutilityfunctions.aemformsutilityfunctions.core-1.0-SNAPSHOT.jar).

## Exemple de code pour utiliser le ou les service(s) utilitaire(s)

Voici le code utilisé dans la page JSP pour créer org.w3c.dom.Document à partir d’une chaîne et convertir le document et le stocker dans le référentiel CRX tel qu’illustré dans l’extrait de code suivant.

```java
 aemformsutilityfunctions.core.AemFormsUtilities aemFormsUtilities = sling.getService(aemformsutilityfunctions.core.AemFormsUtilities.class);
com.adobe.aemfd.docmanager.Document xmlStringDoc = aemFormsUtilities.orgw3cDocumentToAEMFDDocument(aemFormsUtilities.w3cDocumentFromStrng("<data><fname>Girish</fname></data>"));
aemFormsUtilities.saveDocumentInCrx("/content/xmlfiles",".xml",xmlStringDoc);
```

## Conditions préalables


Vous devez déployer [DevelopingWithServiceUserBundle](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/DevelopingWithServiceUser.jar) et démarrer le lot.


Si vous souhaitez enregistrer des documents dans le référentiel CRX à l’aide de ce service utilitaire, veuillez suivre l’[article sur le développement avec un utilisateur ou une utilisatrice de service](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=fr#adaptive-forms). Veillez à fournir les [autorisations requises](http://localhost:4502/useradmin) sur les dossiers CRX appropriés à l’utilisateur ou l’utilisatrice du service fd.
