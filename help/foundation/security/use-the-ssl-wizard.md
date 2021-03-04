---
title: Utilisation de l’Assistant SSL dans AEM
description: Adobe Experience Manager de l’assistant de configuration SSL pour faciliter la configuration d’une instance AEM à exécuter via HTTPS.
seo-description: Adobe Experience Manager de l’assistant de configuration SSL pour faciliter la configuration d’une instance AEM à exécuter via HTTPS.
version: 6.3, 6,4, 6.5
topics: security, operations
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Sécurité
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '216'
ht-degree: 1%

---


# Utilisation de l’Assistant SSL dans AEM

Adobe Experience Manager de l’assistant de configuration SSL pour faciliter la configuration d’une instance AEM à exécuter via HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993/?quality=12&learn=on)

>[!NOTE]
>
>Pour les environnements gérés, il est préférable que le service informatique fournisse des certificats et clés de confiance CA.
>
>Les certificats auto-signés doivent uniquement être utilisés à des fins de développement.

## Clé privée et téléchargement de certificat autosigné

Le fichier compressé suivant contient les fichiers [!DNL DER] et [!DNL CRT] nécessaires à la configuration AEM SSL sur localhost et destiné uniquement à des fins de développement local.

Les fichiers [!DNL DER] et [!DNL CERT] sont fournis à titre pratique et générés en suivant les étapes décrites dans la section Générer une clé privée et un certificat auto-signé ci-dessous.

Si nécessaire, la phrase de passe de certificat est **admin**.

localhost - clé privée et fichier certificate.zip autosigné (expire en juillet 2028)

[Téléchargement du fichier de certificat](assets/use-the-ssl-wizard/certificate.zip)

## Génération de clés privées et de certificats autosignés

La vidéo ci-dessus illustre la configuration et la configuration de SSL sur une instance d’auteur AEM à l’aide de certificats auto-signés. Les commandes ci-dessous utilisant [[!DNL OpenSSL]](https://www.openssl.org/) peuvent générer une clé privée et un certificat à utiliser à l&#39;étape 2 de l&#39;Assistant.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
