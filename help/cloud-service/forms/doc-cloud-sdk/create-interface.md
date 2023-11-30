---
title: Créer une interface de service
description: Définissez les méthodes de l’interface que vous souhaitez afficher.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7825.jpg
jira: KT-7825
exl-id: f262013b-aaf1-43d1-84b8-6173942c3415
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '23'
ht-degree: 100%

---


# Interface

Créez une interface avec les deux définitions de méthode suivantes.

```java
package com.aemforms.doccloud.core;

import java.io.InputStream;

import com.adobe.aemfd.docmanager.Document;

public interface DocumentCloudSDKService {	
	public Document getPDF(String location,String accessToken,String fileName);
	
    public Document createPDFFromInputStream(InputStream is,String fileName);
}
```
