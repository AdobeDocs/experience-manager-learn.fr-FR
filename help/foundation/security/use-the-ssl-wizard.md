---
title: Utiliser l’assistant SSL dans AEM
description: L’assistant de configuration SSL d’Adobe Experience Manager facilite la configuration d’une instance d’AEM pour qu’elle fonctionne via HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.4, 6.5
topics: security, operations
feature: Security
activity: use
audience: administrator
doc-type: technical video
uuid: 82a6962e-3658-427a-bfad-f5d35524f93b
discoiquuid: 9e666741-0f76-43c9-ab79-1ef149884686
topic: Security
role: Developer
level: Beginner
exl-id: 4e69e115-12a6-4a57-90da-b91e345c6723
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 100%

---

# Utiliser l’assistant SSL dans AEM

L’assistant de configuration SSL d’Adobe Experience Manager facilite la configuration d’une instance d’AEM pour qu’elle fonctionne via HTTPS.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)

Ouvrez l’__assistant de configurationSSL__ qui peut être ouvert directement en accédant à __Création AEM > Outils > Sécurité > Configuration SSL__.

>[!NOTE]
>
>Pour les environnements gérés, il est préférable que le service informatique fournisse des certificats et des clés approuvés par une autorité de certification.
>
>Les certificats auto-signés doivent uniquement être utilisés à des fins de développement.

## Téléchargement de clé privée et de certificat auto-signé

Le fichier zip suivant contient les fichiers [!DNL DER] et [!DNL CRT] requis pour la configuration SSL d’AEM sur localhost et destinés uniquement à des fins de développement local.

Les fichiers [!DNL DER] et [!DNL CERT] sont fournis pour des raisons de commodité et sont générés en suivant les étapes décrites dans la section Générer une clé privée et un certificat auto-signé ci-dessous.

Si nécessaire, le mot de passe du certificat est **admin**.

localhost : fichier .zip de clé privée et de certificat autosigné (expire en juillet 2028)

[Télécharger le fichier de certificat](assets/use-the-ssl-wizard/certificate.zip)

## Génération de la clé privée et du certificat auto-signé

La vidéo ci-dessus illustre la configuration SSL sur une instance de création AEM à l’aide de certificats auto-signés. Les commandes ci-dessous utilisant [[!DNL OpenSSL]](https://www.openssl.org/) peuvent générer une clé privée et un certificat à utiliser à l’étape 2 de l’assistant.

```shell
### Create Private Key
$ openssl genrsa -aes256 -out localhostprivate.key 4096

### Generate Certificate Signing Request using private key
$ openssl req -sha256 -new -key localhostprivate.key -out localhost.csr -subj '/CN=localhost'

### Generate the SSL certificate and sign with the private key, will expire one year from now
$ openssl x509 -req -extfile <(printf "subjectAltName=DNS:localhost") -days 365 -in localhost.csr -signkey localhostprivate.key -out localhost.crt

### Convert Private Key to DER format - SSL wizard requires key to be in DER format
$ openssl pkcs8 -topk8 -inform PEM -outform DER -in localhostprivate.key -out localhostprivate.der -nocrypt
```
