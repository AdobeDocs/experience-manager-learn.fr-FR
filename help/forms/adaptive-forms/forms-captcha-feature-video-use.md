---
title: Utilisation de CAPTCHA avec AEM Forms adaptatif
description: Ajout et utilisation d’un CAPTCHA avec AEM Forms adaptatif.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 5%

---

# Utilisation de CAPTCHA avec AEM Forms adaptatif{#using-captchas-with-aem-adaptive-forms}

Ajout et utilisation d’un CAPTCHA avec AEM Forms adaptatif.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Cette vidéo décrit le processus d’ajout d’un CAPTCHA à un formulaire adaptatif AEM à l’aide du service intégré AEM CAPTCHA ainsi que du service reCAPTCHA Google.*

>[!NOTE]
>
>Cette fonctionnalité est disponible uniquement avec AEM version 6.3 et ultérieure.

>[!NOTE]
>
>**Pour configurer reCaptcha sur l’instance de publication, procédez comme suit :**
>
>Configuration de reCaptach sur l’instance d’auteur
>
>ouvrir le Felix [console web](http://localhost:4502/system/console/bundles) sur l’instance d’auteur
>
>recherchez le lot com.adobe.granite.crypto.file .
>
>Notez l’ID de lot. Sur mon instance, il est 20
>
>Accédez à l’ID de lot sur le système de fichiers de votre instance de création.
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copie des fichiers HMAC et maîtres
>
Ouvrez le [console web felix](http://localhost:4502/system/console/bundles) sur votre instance de publication. Recherchez le lot com.adobe.granite.crypto.file . Notez l’ID de bundle
Accédez à l’ID de lot sur le système de fichiers de votre instance de publication.
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* supprimez les fichiers HMAC et maîtres existants.
* coller les fichiers HMAC et maîtres copiés à partir de l’instance de création ;
>
Redémarrez votre serveur de publication AEM

## Documents annexes {#supporting-materials}

* [reCAPTCHA de Google](https://www.google.com/recaptcha)
