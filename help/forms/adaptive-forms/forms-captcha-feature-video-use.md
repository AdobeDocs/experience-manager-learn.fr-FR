---
title: Utiliser Captcha avec les formulaires adaptatifs AEM
description: Ajoutez et utilisez un Captcha avec les formulaires adaptatifs AEM.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 272
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 100%

---

# Utiliser Captcha avec les formulaires adaptatifs AEM{#using-captchas-with-aem-adaptive-forms}

Ajoutez et utilisez un Captcha avec les formulaires adaptatifs AEM.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Cette vidéo décrit le processus d’ajout d’un Captcha à un formulaire adaptatif AEM à l’aide du service intégré Captcha d’AEM ainsi que du service reCaptcha de Google.*

>[!NOTE]
>
>Cette fonctionnalité est disponible uniquement avec AEM version 6.3 et ultérieure.

>[!NOTE]
>
>**Pour configurer reCaptcha sur l’instance de publication, procédez comme suit :**
>
>Configurer reCaptcha sur l’instance de création
>
>Ouvrez la [console web](http://localhost:4502/system/console/bundles) Felix sur l’instance de création.
>
>Recherchez le lot com.adobe.granite.crypto.file.
>
>Notez l’ID du lot. Mon instance indique 20.
>
>Accédez à l’ID du lot sur le système de fichiers de votre instance de création.
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* Copier les fichiers HMAC et maîtres
>
>Ouvrez la [console web Felix](http://localhost:4502/system/console/bundles) sur votre instance de publication. Recherchez le lot com.adobe.granite.crypto.file. Notez l’ID du lot.
>
>Accédez à l’ID du lot sur le système de fichiers de votre instance de publication.
>
>* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
>* Supprimez les fichiers HMAC et maîtres existants.
>* Collez les fichiers HMAC et maîtres copiés à partir de l’instance de création.
>
>Redémarrez votre serveur de publication AEM.

## Documents annexes {#supporting-materials}

* [reCAPTCHA de Google](https://www.google.com/recaptcha)
