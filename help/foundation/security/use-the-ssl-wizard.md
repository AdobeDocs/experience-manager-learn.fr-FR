---
title: Utiliser l’assistant SSL dans AEM
description: L’assistant de configuration SSL d’Adobe Experience Manager facilite la configuration d’une instance d’AEM pour qu’elle fonctionne via HTTPS.
seo-description: Adobe Experience Manager's SSL setup wizard to make it easier to set up an AEM instance to run over HTTPS.
version: 6.5, Cloud Service
jira: KT-13839
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
last-substantial-update: 2023-08-08T00:00:00Z
source-git-commit: 1a8e3f37554f98c1366a1a06cb4a7b867866dd1b
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 31%

---

# Utiliser l’assistant SSL dans AEM

Découvrez comment configurer SSL dans Adobe Experience Manager pour qu’il s’exécute via HTTPS à l’aide de l’assistant SSL intégré.

>[!VIDEO](https://video.tv.adobe.com/v/17993?quality=12&learn=on)


>[!NOTE]
>
>Pour les environnements gérés, il est préférable que le service informatique fournisse des certificats et des clés approuvés par une autorité de certification.
>
>Les certificats auto-signés doivent uniquement être utilisés à des fins de développement.

## Utilisation de l’assistant de configuration SSL

Accédez à __AEM Auteur > Outils > Sécurité > Configuration SSL__, puis ouvrez le __Assistant de configuration SSL__.

![Assistant de configuration SSL](assets/use-the-ssl-wizard/ssl-config-wizard.png)

### Création d’informations d’identification de magasin

Pour créer une _Key Store_ associé à la propriété `ssl-service` utilisateur système et une variable globale _Trust Store_, utilisez le __Informations d’identification de magasin__ étape de l’assistant.

1. Saisissez le mot de passe et confirmez le mot de passe pour la variable __Key Store__ associé à la propriété `ssl-service` utilisateur système.
1. Saisissez le mot de passe et confirmez le mot de passe pour le __Trust Store__. Notez qu’il s’agit d’un Trust Store à l’échelle du système et, s’il est déjà créé, le mot de passe saisi est ignoré.

   ![Configuration SSL - Informations d’identification de magasin](assets/use-the-ssl-wizard/store-credentials.png)

### Téléchargement d’une clé privée et d’un certificat

Pour charger la variable _clé privée_ et _Certificat SSL_, utilisez le __Clé et certificat__ étape de l’assistant.

En règle générale, votre service informatique fournit le certificat et la clé approuvés par l’autorité de certification, mais le certificat auto-signé peut être utilisé pour __development__ et __test__ d’ .

Pour créer ou télécharger le certificat auto-signé, voir [Clé privée et certificat auto-signé](#self-signed-private-key-and-certificate).

1. Téléchargez le __Clé privée__ au format DER (Distinguished Encoding Rules). Contrairement à PEM, les fichiers codés DER ne contiennent pas d’instructions de texte brut telles que `-----BEGIN CERTIFICATE-----`
1. Charger le __Certificat SSL__ dans le `.crt` format.

   ![Configuration SSL - Clé privée et certificat](assets/use-the-ssl-wizard/privatekey-and-certificate.png)

### Mise à jour des détails du connecteur SSL

Pour mettre à jour la variable _hostname_ et _port_ utilisez la méthode __Connecteur SSL__ étape de l’assistant.

1. Mettez à jour ou vérifiez le __HTTPS Hostname__ , elle doit correspondre à la variable `Common Name (CN)` du certificat.
1. Mettez à jour ou vérifiez le __Port HTTPS__ .

   ![Configuration SSL - détails du connecteur SSL](assets/use-the-ssl-wizard/ssl-connector-details.png)

### Vérification de la configuration SSL

1. Pour vérifier le protocole SSL, cliquez sur le bouton __Accéder à l’URL HTTPS__ bouton .
1. Si vous utilisez un certificat auto-signé, vous voyez `Your connection is not private` erreur.

   ![Configuration SSL - Vérification de l’AEM via HTTPS](assets/use-the-ssl-wizard/verify-aem-over-ssl.png)

## Clé privée et certificat auto-signé

Le fichier zip suivant contient [!DNL DER] et [!DNL CRT] fichiers requis pour configurer AEM SSL localement et destinés uniquement à des fins de développement local.

Les fichiers [!DNL DER] et [!DNL CERT] sont fournis pour des raisons de commodité et sont générés en suivant les étapes décrites dans la section Générer une clé privée et un certificat auto-signé ci-dessous.

Si nécessaire, le mot de passe du certificat est **admin**.

Cet hôte local : clé privée et certificat auto-signé.zip (expiration en juillet 2028)

[Télécharger le fichier de certificat](assets/use-the-ssl-wizard/certificate.zip)

### Génération de la clé privée et du certificat auto-signé

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
