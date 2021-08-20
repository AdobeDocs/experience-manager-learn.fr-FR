---
title: Création de votre premier service OSGi avec AEM Forms
description: 'Création de votre premier service OSGi avec AEM Forms '
feature: Formulaires adaptatifs
version: 6.4,6.5
topic: Développement
role: Developer
level: Beginner
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '345'
ht-degree: 5%

---


# Service OSGi

Un service OSGi est une classe Java ou une interface de service, ainsi qu’un certain nombre de propriétés de service sous la forme de paires nom/valeur. Les propriétés du service différencient les différents fournisseurs de services qui fournissent des services avec la même interface de service.

Un service OSGi est défini sémantiquement par son interface de service et implémenté en tant qu’objet de service. La fonctionnalité d’un service est définie par les interfaces qu’il implémente. Ainsi, différentes applications peuvent mettre en oeuvre le même service. Les interfaces de service permettent aux lots d’interagir par des interfaces de liaison, et non par des implémentations. Une interface de service doit être spécifiée avec le moins de détails d’implémentation possible.

## Définition de l’interface

Interface simple avec une méthode pour fusionner les données avec le modèle <span class="x x-first x-last">XDP</span>.

```java
package com.learningaemforms.adobe.core;

import com.adobe.aemfd.docmanager.Document;

public interface MyfirstInterface {
  public Document mergeDataWithXDPTemplate(Document xdpTemplate, Document xmlDocument);
} 
```

## Mise en oeuvre de l’interface

Créez un module appelé `com.learningaemforms.adobe.core.impl` destiné à contenir l’implémentation de l’interface.

```java
package com.learningaemforms.adobe.core.impl;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.adobe.aemfd.docmanager.Document;
import com.adobe.fd.output.api.OutputService;
import com.adobe.fd.output.api.OutputServiceException;
import com.learningaemforms.adobe.core.MyfirstInterface;
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

L’annotation `@Component(...)` sur la ligne 10 marque cette classe Java en tant que composant OSGi et l’enregistre en tant que service OSGi.

L’annotation `@Reference` fait partie des services déclaratifs OSGi et est utilisée pour injecter une référence de [Outputservice](https://helpx.adobe.com/experience-manager/6-5/forms/javadocs/index.html?com/adobe/fd/output/api/OutputService.html) dans la variable `outputService`.


## Création et déploiement du lot

* Ouvrez la **fenêtre d’invite de commande**.
* Accédez à `c:\aemformsbundles\learningaemforms\core`.
* Exécutez la commande `mvn clean install -PautoInstallBundle`
* La commande ci-dessus crée et déploie automatiquement le lot vers votre instance AEM s’exécutant sur localhost:4502

Le lot sera également disponible à l’emplacement suivant `C:\AEMFormsBundles\learningaemforms\core\target`. Le lot peut également être déployé dans AEM à l’aide de la [console web Felix.](http://localhost:4502/system/console/bundles)

## Utilisation du service

Vous pouvez désormais utiliser le service dans votre page JSP. Le fragment de code suivant montre comment accéder à votre service et utiliser les méthodes implémentées par le service.

```java
MyFirstAEMFormsService myFirstAEMFormsService = sling.getService(com.learningaemforms.adobe.core.MyFirstAEMFormsService.class);
com.adobe.aemfd.docmanager.Document generatedDocument = myFirstAEMFormsService.mergeDataWithXDPTemplate(xdp_or_pdf_template,xmlDocument);
```

L’exemple de package contenant la page JSP peut être ![téléchargé ici](assets/learning-aem-forms.zip)

## Tester le module

Importez et installez le module dans AEM à l’aide du [gestionnaire de modules](http://localhost:4502/crx/packmgr/index.jsp).

Utilisez Postman pour effectuer un appel POST et fournir les paramètres d’entrée comme illustré dans la capture d’écran ci-dessous.
![postman](assets/test-service-postman.JPG)
