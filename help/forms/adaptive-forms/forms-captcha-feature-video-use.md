---
title: Utilisation de CAPTCHA avec AEM Forms adaptatif
description: Ajout et utilisation d’un CAPTCHA avec AEM Forms adaptatif.
feature: Forms adaptatif,Workflow
version: 6.4,6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 13%

---


# Utilisation de CAPTCHA avec AEM Forms adaptatif{#using-captchas-with-aem-adaptive-forms}

Ajout et utilisation d’un CAPTCHA avec AEM Forms adaptatif.

Consultez la page [Exemples AEM Forms](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1) pour obtenir un lien vers une démonstration en direct de cette fonctionnalité.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Cette vidéo décrit le processus d’ajout d’un CAPTCHA à un formulaire adaptatif AEM à l’aide du service intégré AEM CAPTCHA ainsi que du service reCAPTCHA de Google.*

>[!NOTE]
>
>Cette fonctionnalité est disponible uniquement avec AEM version 6.3 et ultérieure.

>[!NOTE]
>
>**Pour configurer reCaptcha sur l’instance de publication, procédez comme suit :**
>
>Configuration de reCaptach sur l’instance d’auteur
>
>ouvrez la console web Felix [web](http://localhost:4502/system/console/bundles) sur l’instance d’auteur.
>
>recherchez le lot com.adobe.granite.crypto.file .
>
>Notez l’ID de lot. Sur mon instance, il est 20
>
>Accédez à l’ID de lot sur le système de fichiers de votre instance de création.
>
>* &lt;rép-install-aem-création>/crx-quickstart/launchpad/felix/bundle20/data
* Copiez les fichiers HMAC et les fichiers principaux

Ouvrez la [console web felix](http://localhost:4502/system/console/bundles) sur votre instance de publication. Recherchez le lot com.adobe.granite.crypto.file . Notez l’ID de bundle
Accédez à l’ID de lot sur le système de fichiers de votre instance de publication.
* &lt;rép-install-aem-publication>/crx-quickstart/launchpad/felix/bundle20/data
* supprimez les fichiers HMAC et maîtres existants.
* coller les fichiers HMAC et maîtres copiés à partir de l’instance de création ;

Redémarrez votre serveur de publication AEM

## Documents annexes {#supporting-materials}

* [reCAPTCHA de Google](https://www.google.com/recaptcha)

