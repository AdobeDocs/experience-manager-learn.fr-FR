---
title: Créer une configuration OSGi personnalisée
description: Personnaliser une configuration OSGi pour capturer les détails spécifiques à Document Cloud
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
thumbnail: 7818.jpg
jira: KT-7818
exl-id: 1f34c356-6c0c-46ff-9cea-7baacfc4bb7f
duration: 22
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '63'
ht-degree: 100%

---

# Présentation

Créer une configuration OSGi personnalisée pour capturer les informations d’identification de votre compte Document Cloud


Pour créer une configuration OSGi personnalisée, nous devons d’abord créer une interface dont les méthodes publiques représenteront les champs de la configuration.

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


Créez une interface nommée DocumentCloudConfiguration et collez-y le code suivant.

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
