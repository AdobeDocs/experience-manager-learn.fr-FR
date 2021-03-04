---
title: Utilisation de CAPTCHA avec AEM Forms adaptatif
seo-title: Utilisation de CAPTCHA avec AEM Forms adaptatif
description: Ajouter et utiliser un CAPTCHA avec AEM Adaptive Forms.
seo-description: Ajouter et utiliser un CAPTCHA avec AEM Adaptive Forms.
feature: '"Forms adaptatif,Workflow"'
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
topic: Développement
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 12%

---


# Utilisation de CAPTCHA avec AEM Forms adaptatif{#using-captchas-with-aem-adaptive-forms}

Ajouter et utiliser un CAPTCHA avec AEM Adaptive Forms.

Consultez la page [Exemples d&#39;AEM Forms](https://forms.enablementadobe.com/content/samples/samples.html?query=0) pour obtenir un lien vers une démonstration en direct de cette fonctionnalité.

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
>ouvrez la console Web felix [web ](http://localhost:4502/system/console/bundles) sur l’instance d’auteur.
>
>recherchez le lot com.adobe.granite.crypto.file
>
>Notez l’ID d’assemblage. Sur mon cas, il est 20.
>
>Accédez à l’ID d’assemblage sur le système de fichiers de votre instance d’auteur.
>
>* &lt;rép-install-aem-création>/crx-quickstart/launchpad/felix/bundle20/data
* Copiez les fichiers HMAC et les fichiers principaux

Ouvrez la [console Web felix](http://localhost:4502/system/console/bundles) sur votre instance de publication. Recherchez le lot com.adobe.granite.crypto.file. Notez l’ID du lot
Accédez à l’ID d’assemblage sur le système de fichiers de votre instance de publication.
* &lt;rép-install-aem-publication>/crx-quickstart/launchpad/felix/bundle20/data
* supprimez les fichiers HMAC et master existants.
* coller les fichiers HMAC et originaux copiés à partir de l’instance d’auteur

Redémarrez votre serveur de publication AEM

## Documents d&#39;appui {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

