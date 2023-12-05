---
title: Assemblage XDP avec le service Assembler
description: Utiliser le service Assembler dans AEM Forms pour l’assemblage XDP
feature: Assembler
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-12-19T00:00:00Z
exl-id: e116038f-7d86-41ee-b1b0-7b8569121d6d
duration: 130
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 100%

---

# Assemblage XDP à l’aide du service Assembler

Cet article apporte les ressources permettant de démontrer la fonctionnalité d’assemblage des documents xdp à l’aide du service Assembler.
Le code jsp suivant a été écrit pour insérer un sous-formulaire appelé **address** du document xdp appelé address.xdp dans un point d’insertion appelé **address** dans le document master.xdp. Le fichier xdp obtenu a été enregistré dans le dossier racine de votre installation AEM.

Le service Assembler s’appuie sur un document DDX valide pour décrire la manipulation de documents PDF. Vous pouvez consulter le document de référence DDX [ici](assets/ddxRef.pdf). La page 40 contient des informations sur l’assemblage xdp.

```java
    javax.servlet.http.Part ddxFile = request.getPart("xdpstitching.ddx");
    System.out.println("Got DDX");
    java.io.InputStream ddxIS = ddxFile.getInputStream();
    com.adobe.aemfd.docmanager.Document ddxDocument = new com.adobe.aemfd.docmanager.Document(ddxIS);
    javax.servlet.http.Part masterXdpPart = request.getPart("masterxdp.xdp");
    System.out.println("Got master xdp");
    java.io.InputStream masterXdpPartIS = masterXdpPart.getInputStream();
    com.adobe.aemfd.docmanager.Document masterXdpDocument = new com.adobe.aemfd.docmanager.Document(masterXdpPartIS);

    javax.servlet.http.Part fragmentXDPPart = request.getPart("fragment.xdp");
    System.out.println("Got fragment.xdp");
    java.io.InputStream fragmentXDPPartIS = fragmentXDPPart.getInputStream();
    com.adobe.aemfd.docmanager.Document fragmentXdpDocument = new com.adobe.aemfd.docmanager.Document(fragmentXDPPartIS);

    java.util.Map < String, Object > mapOfDocuments = new java.util.HashMap < String, Object > ();
    mapOfDocuments.put("master.xdp", masterXdpDocument);
    mapOfDocuments.put("address.xdp", fragmentXdpDocument);
    com.adobe.fd.assembler.service.AssemblerService assemblerService = sling.getService(com.adobe.fd.assembler.service.AssemblerService.class);
    if (assemblerService != null)
      System.out.println("Got assembler service");

    com.adobe.fd.assembler.client.AssemblerOptionSpec aoSpec = new com.adobe.fd.assembler.client.AssemblerOptionSpec();
    aoSpec.setFailOnError(true);

    com.adobe.fd.assembler.client.AssemblerResult assemblerResult = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
    com.adobe.aemfd.docmanager.Document finalXDP = assemblerResult.getDocuments().get("stitched.xdp");
    finalXDP.copyToFile(new java.io.File("stitched.xdp"));
```

Le fichier DDX permettant d’insérer des fragments dans un autre xdp est répertorié ci-dessous. Le DDX insère le sous-formulaire **address** du fichier address.xdp dans le point d’insertion appelé **address** dans le fichier master.xdp. Le document obtenu nommé **stitched.xdp** est enregistré dans le système de fichiers.

```xml
<?xml version="1.0" encoding="UTF-8"?> 
<DDX xmlns="http://ns.adobe.com/DDX/1.0/"> 
        <XDP result="stitched.xdp"> 
           <XDP source="master.xdp"> 
            <XDPContent insertionPoint="address" source="address.xdp" fragment="address"/> 
         </XDP> 
        </XDP>         
</DDX>
```

Pour que cette fonctionnalité soit valide sur votre serveur AEM :

* Téléchargez le [package d’assemblage XDP](assets/xdp-stitching.zip) sur votre système local.
* Téléchargez et installez le package à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* [Extrayez le contenu de ce fichier zip](assets/xdp-and-ddx.zip) pour obtenir les exemples de fichiers xdp et DDX.

**Après avoir installé le package, vous devrez placer sur la liste autorisée les URL suivantes dans le filtre Adobe CSRF Granite.**

1. Suivez les étapes mentionnées ci-dessous pour placer sur la liste autorisée les chemins mentionnés ci-dessus.
1. [Se connecter à configMgr](http://localhost:4502/system/console/configMgr).
1. Recherchez un filtre CSRF Adobe Granite.
1. Ajoutez le chemin suivant dans les sections exclues et enregistrez `/content/AemFormsSamples/assemblerservice`.
1. Recherchez le filtre référent Sling (« Sling Referrer filter »).
1. Cochez la case « Autoriser les champs vides ». (Ce paramètre doit être utilisé à des fins de test uniquement.)
Il existe plusieurs façons de tester l’exemple de code. La plus rapide et la plus simple est d’utiliser l’application Postman. Postman vous permet d’envoyer des requêtes POST à votre serveur. Installez l’application Postman sur votre système.
Lancez l’application et saisissez l’URL suivante pour tester l’API d’export des données :
http://localhost:4502/content/AemFormsSamples/assemblerservice.html.

Indiquez les paramètres d’entrée suivants, comme indiqué dans la capture d’écran. Vous pouvez utiliser les exemples de documents que vous avez téléchargés précédemment,
![xdp-stitch-postman](assets/xdp-stitching-postman.png).

>[!NOTE]
>
>Assurez-vous que votre installation d’AEM Forms est terminée. Tous vos lots doivent être à l’état actif.
>
