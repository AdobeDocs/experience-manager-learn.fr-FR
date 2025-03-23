---
title: Affichage d’un code QR dans un formulaire adaptatif
description: Affichage d’un code QR dans un formulaire adaptatif
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
jira: KT-15603
last-substantial-update: 2024-05-28T00:00:00Z
exl-id: 0c6079f4-601e-4a82-976c-71dbb2faa671
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '203'
ht-degree: 100%

---

# Exemple de composant de code QR

L’incorporation d’un code QR dans un formulaire adaptatif peut améliorer considérablement la commodité et l’efficacité de l’accès des utilisateurs et des utilisatrices aux informations supplémentaires relatives au formulaire.

L’exemple de composant utilise [QRCode.js](https://davidshimjs.github.io/qrcodejs/).

Le fichier QRCode.js est une bibliothèque JavaScript pour créer du code QRC. Il prend en charge le mode Cross-browser avec HTML5 Canvas et la balise de tableau dans DOM.

Le composant génère le code QR en fonction de la valeur spécifiée dans la propriété de configuration du composant.
![Image](assets/qr-code-url.png)

Le code ci-après a été utilisé dans le fichier body.jsp du composant qr-code-generator.

« url » est l’URL qui doit être incorporée dans le code QR. Cette URL est spécifiée dans les propriétés de configuration du composant de code QR.

```java
<%@include file="/libs/foundation/global.jsp"%>
<body>
    <h2>Scan the QR Code for more information related to this form</h2>
    <div data-url="<%=properties.get("url")%>">
    </div>
    <div id="qrcode">
    </div>
</body>
```



Le code ci-après utilise la méthode makeCode de la bibliothèque QRCode.js dans la bibliothèque cliente du composant qr-code-generator. Le code QR généré est ajouté à la balise div identifiée par l’identifiant **« qrcode »**.

```javascript
$(document).ready(function()
  {
      var qrcode = new QRCode("qrcode");
      qrcode.makeCode(document.querySelector("[data-url]").getAttribute("data-url"));
      
 });
```

## Déploiement des ressources sur votre serveur local

* [Téléchargez et installez le composant de code QR à l’aide du gestionnaire de modules.](assets/qrcode.zip)
* [Téléchargez et installez l’exemple de formulaire adaptatif à l’aide du gestionnaire de modules.](assets/form-with-qr-code.zip)
* [Prévisualisation du formulaire](http://localhost:4502/content/dam/formsanddocuments/qrcode/w9form/jcr:content?wcmmode=disabled). La section d’aide du formulaire comporte le code QR.
