---
title: Création de votre premier service OSGi avec AEM Forms
description: Création de votre premier service OSGi avec AEM Forms
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 2f15782e-b60d-40c6-b95b-6c7aa8290691
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '354'
ht-degree: 4%

---

# Service OSGi

Un service OSGi est une classe Java ou une interface de service, ainsi qu’un certain nombre de propriétés de service sous la forme de paires nom/valeur. Les propriétés du service différencient les différents fournisseurs de services qui fournissent des services avec la même interface de service.

Un service OSGi est défini sémantiquement par son interface de service et implémenté en tant qu’objet de service. La fonctionnalité d’un service est définie par les interfaces qu’il implémente. Ainsi, différentes applications peuvent mettre en oeuvre le même service. Les interfaces de service permettent aux lots d’interagir par des interfaces de liaison, et non par des implémentations. Une interface de service doit être spécifiée avec le moins de détails d’implémentation possible.

## Définition de l’interface

Une interface simple avec une méthode pour fusionner les données avec la variable <span class="x x-first x-last">XDP</span> modèle.

```java
package com.mysite.samples;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface
{
    public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
}
 
```

## Mise en oeuvre de l’interface

Créez un module appelé `com.mysite.samples.impl` pour contenir l’implémentation de l’interface.

```java
package com.mysite.samples.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.mysite.samples.MyfirstInterface;
@Component(service = MyfirstInterface.class)
public class MyfirstInterfaceImpl implements MyfirstInterface {
  @Reference
  OutputService outputService;

  private static final Logger log = LoggerFactory.getLogger(MyfirstInterfaceImpl.class);

  @Override
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument) {
    com.adobe.fd.output.api.PDFOutputOptions pdfOptions = new com.adobe.fd.output.api.PDFOutputOptions();
    pdfOptions.setAcrobatVersion(com.adobe.fd.output.api.AcrobatVersion.Acrobat_11);
    try {
      return outputService.generatePDFOutput(xdpTemplate, xmlDocument, pdfOptions);

    } catch (OutputServiceException e) {

      log.error("Failed to merge data with XDP Template", e);

    }

    return null;
  }

}
```

Annotation `@Component(...)` La ligne 10 marque cette classe Java en tant que composant OSGi et l’enregistre en tant que service OSGi.

Le `@Reference` annotation fait partie des services déclaratifs OSGi et est utilisé pour injecter une référence de la variable [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) dans la variable `outputService`.


## Création et déploiement du lot

* Ouvrir **fenêtre d&#39;invite de commande**
* Accédez à `c:\aemformsbundles\mysite\core`.
* Exécutez la commande `mvn clean install -PautoInstallBundle`
* La commande ci-dessus crée et déploie automatiquement le lot vers votre instance AEM s’exécutant sur localhost:4502

Le lot sera également disponible à l’emplacement suivant : `C:\AEMFormsBundles\mysite\core\target`. Le lot peut également être déployé dans AEM à l’aide de la variable [Console web Felix.](http://localhost:4502/system/console/bundles)

## Utilisation du service

Vous pouvez désormais utiliser le service dans votre page JSP. Le fragment de code suivant montre comment accéder à votre service et utiliser les méthodes implémentées par le service.

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.mysite.samples.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

L’exemple de package contenant la page JSP peut être [téléchargé ici](assets/learning_aem_forms.zip)

[Le lot complet est disponible pour le téléchargement.](assets/mysite.core-1.0.0-SNAPSHOT.jar)

## Tester le module

Importez et installez le package dans AEM à l’aide de la méthode [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp)

Utilisez Postman pour effectuer un appel POST et fournir les paramètres d’entrée comme illustré dans la capture d’écran ci-dessous.
![postman](assets/test-service-postman.JPG)

## Étapes suivantes

[Création d’un servlet Sling](./create-servlet.md)

