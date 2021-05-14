---
title: Créer une configuration OSGi personnalisée
description: Configuration OSGi personnalisée pour capturer des détails spécifiques à document cloud
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: développement
thumbnail: 7818.jpg
kt: 7818
source-git-commit: 84499d5a7c8adac87196f08c6328e8cb428c0130
workflow-type: tm+mt
source-wordcount: '64'
ht-degree: 3%

---

# Présentation

Créez une configuration OSGi personnalisée pour capturer les informations d’identification de votre compte document cloud.


Pour créer une configuration OSGi personnalisée, nous devons d&#39;abord créer une interface dont les méthodes publiques représenteront les champs de la configuration.

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
