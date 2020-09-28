---
title: Utilisation de CAPTCHA avec AEM Forms adaptatif
seo-title: Utilisation de CAPTCHA avec AEM Forms adaptatif
description: ajouter et utiliser un CAPTCHA avec AEM Adaptive Forms.
seo-description: ajouter et utiliser un CAPTCHA avec AEM Adaptive Forms.
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 11%

---


# Utilisation de CAPTCHA avec AEM Forms adaptatif{#using-captchas-with-aem-adaptive-forms}

ajouter et utiliser un CAPTCHA avec AEM Adaptive Forms.

Consultez la page d&#39;exemples [](https://forms.enablementadobe.com/content/samples/samples.html?query=0) AEM Forms pour obtenir un lien vers une démonstration en direct de cette fonctionnalité.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Cette vidéo décrit le processus d&#39;ajout d&#39;un CAPTCHA à un formulaire adaptatif AEM à l&#39;aide du service intégré AEM CAPTCHA ainsi que du service reCAPTCHA de Google.*

>[!NOTE]
>
>Cette fonction est disponible uniquement à partir de AEM version 6.3.

>[!NOTE]
>
>**Pour configurer reCaptcha sur l&#39;instance de publication, suivez les étapes ci-dessous**
>
>Configuration de reCaptach sur l’instance d’auteur
>
>ouverture de la console [](http://localhost:4502/system/console/bundles) web felix sur l’instance d’auteur
>
>recherchez le lot com.adobe.granite.crypto.file
>
>Notez l’ID d’assemblage. Sur mon cas, il est 20.
>
>Accédez à l’ID d’assemblage sur le système de fichiers de votre instance d’auteur.
>
>* &lt;rép-install-aem-création>/crx-quickstart/launchpad/felix/bundle20/data
* Copiez les fichiers HMAC et les fichiers principaux

Ouvrez la console [Web](http://localhost:4502/system/console/bundles) felix sur votre instance de publication. Recherchez le lot com.adobe.granite.crypto.file. Notez l’ID du lot
Accédez à l’ID d’assemblage sur le système de fichiers de votre instance de publication.
* &lt;rép-install-aem-publication>/crx-quickstart/launchpad/felix/bundle20/data
* supprimez les fichiers HMAC et master existants.
* coller les fichiers HMAC et originaux copiés à partir de l’instance d’auteur

Redémarrez votre serveur de publication AEM

## Documents de support {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

