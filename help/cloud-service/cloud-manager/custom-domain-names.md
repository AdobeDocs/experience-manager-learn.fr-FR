---
title: Ajouter un nom de domaine personnalisé
description: Découvrez comment ajouter un nom de domaine personnalisé à AEM en tant que site hébergé par le service cloud.
version: Cloud Service
feature: Cloud Manager, Operations
topic: Administration, Architecture
role: Admin, Developer
level: Intermediate
doc-type: Tutorial
duration: null
last-substantial-update: 2024-03-12T00:00:00Z
jira: KT-15121
thumbnail: KT-15121.jpeg
source-git-commit: 8230991cebf1a9e994f0dfe96c5590d0c19ef887
workflow-type: tm+mt
source-wordcount: '701'
ht-degree: 1%

---


# Ajouter un nom de domaine personnalisé

Découvrez comment ajouter un nom de domaine personnalisé à AEM site web as a Cloud Service.

Dans ce tutoriel, la valorisation de marque de l’exemple [AEM WKND](https://github.com/adobe/aem-guides-wknd) site est amélioré en ajoutant un nom de domaine personnalisé adressable HTTPS `wknd.enablementadobe.com` avec Transport Layer Security (TLS).

>[!VIDEO](https://video.tv.adobe.com/v/3427903?quality=12&learn=on)

Les étapes de haut niveau sont les suivantes :

![Nom de domaine personnalisé élevé](./assets/add-custom-domain-name-steps.png){width="800" zoomable="yes"}

## Conditions préalables

>[!VIDEO](https://video.tv.adobe.com/v/3427909?quality=12&learn=on)

- [OpenSSL](https://www.openssl.org/) et [dig](https://www.isc.org/blogs/dns-checker/) sont installés sur votre ordinateur local.
- Accès à des services tiers :
   - Autorité de certification (CA) : pour demander le certificat signé pour votre domaine de site, comme [DigitCert](https://www.digicert.com/)
   - Service d’hébergement DNS (Domain Name System) : pour ajouter des enregistrements DNS pour votre domaine personnalisé, comme Azure DNS ou AWS Route 53.
- Accès à [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/) en tant que propriétaire d’entreprise ou responsable de déploiement.
- Exemple [AEM WKND](https://github.com/adobe/aem-guides-wknd) Le site est déployé dans l’environnement AEM de [programme de production](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/programs/introduction-production-programs) type.

Si vous n’avez pas accès à des services tiers, _collaborer avec votre équipe de sécurité ou d’hébergement pour accomplir les étapes_.

## Générer un certificat SSL

>[!VIDEO](https://video.tv.adobe.com/v/3427908?quality=12&learn=on)

Vous disposez de deux options :

- Utilisation `openssl` outil de ligne de commande : vous pouvez générer une clé privée et une demande de signature de certificat (CSR) pour votre domaine de site. Pour demander un certificat signé, envoyez la demande de signature de certificat à une autorité de certification.

- Votre équipe d’hébergement fournit la clé privée requise et le certificat signé pour votre site.

Examinons les étapes de la première option.

Pour générer une clé privée et une demande de signature de certificat, exécutez les commandes suivantes et fournissez les informations requises lorsque vous y êtes invité :

```bash
# Generate a private key and a CSR
$ openssl req -newkey rsa:2048 -keyout <YOUR-SITE-NAME>.key -out <YOUR-SITE-NAME>.csr -nodes
```

Pour demander un certificat signé, fournissez la demande de signature de certificat générée à l’autorité de certification en suivant leur documentation. Une fois que l’autorité de certification a signé la demande de signature de certificat, vous recevez le fichier de certificat signé.

### Vérification du certificat signé

Il est recommandé de consulter le certificat signé avant de l’ajouter à Cloud Manager. Vous pouvez consulter les détails du certificat à l’aide de la commande suivante :

```bash
# Review the certificate details
$ openssl crl2pkcs7 -nocrl -certfile <YOUR-SIGNED-CERT>.crt | openssl pkcs7 -print_certs -noout
```

Le certificat signé peut contenir la chaîne de certificats, qui inclut les certificats racine et intermédiaire ainsi que le certificat de l’entité finale.

Adobe Cloud Manager accepte le certificat de l’entité finale et la chaîne de certificat. _dans des champs de formulaire distincts_, vous devez donc extraire le certificat de l’entité finale et la chaîne de certificat à partir du certificat signé.

Dans ce tutoriel, la variable [DigitCert](https://www.digicert.com/) certificat signé émis contre `*.enablementadobe.com` domain est utilisé comme exemple. L’entité finale et la chaîne de certificat sont extraites en ouvrant le certificat signé dans un éditeur de texte et en copiant le contenu entre les `-----BEGIN CERTIFICATE-----` et `-----END CERTIFICATE-----` marqueurs.

## Ajout d’un certificat SSL dans Cloud Manager

>[!VIDEO](https://video.tv.adobe.com/v/3427906?quality=12&learn=on)

Pour ajouter le certificat SSL dans Cloud Manager, suivez le [Ajout d’un certificat SSL](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/manage-ssl-certificates/add-ssl-certificate) la documentation.

## Vérification des noms de domaine

>[!VIDEO](https://video.tv.adobe.com/v/3427905?quality=12&learn=on)

Pour vérifier le nom de domaine, procédez comme suit :

- Ajoutez un nom de domaine dans Cloud Manager en suivant la procédure [Ajouter un nom de domaine personnalisé](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-custom-domain-name) la documentation.
- Ajout d’une AEM spécifique [Enregistrement TXT](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/add-text-record) dans votre service d’hébergement DNS.
- Vérifiez les étapes ci-dessus en interrogeant les serveurs DNS à l’aide de la fonction `dig` .

```bash
# General syntax, the `_aemverification` is prefix provided by Adobe
$ dig _aemverification.[YOUR-DOMAIN-NAME] -t txt

# This tutorial specific example, as the subdomain `wknd.enablementadobe.com` is used
$ dig _aemverification.wknd.enablementadobe.com -t txt
```

L’exemple de réponse réussie ressemble à ceci :

```bash
; <<>> DiG 9.10.6 <<>> _aemverification.wknd.enablementadobe.com -t txt
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8636
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1220
;; QUESTION SECTION:
;_aemverification.wknd.enablementadobe.com. IN TXT

;; ANSWER SECTION:
_aemverification.wknd.enablementadobe.com. 3600    IN TXT "adobe-aem-verification=wknd.enablementadobe.com/105881/991000/bef0e843-9280-4385-9984-357ed9a4217b"

;; Query time: 81 msec
;; SERVER: 153.32.14.247#53(153.32.14.247)
;; WHEN: Tue Mar 12 15:54:25 EDT 2024
;; MSG SIZE  rcvd: 181
```

Dans ce tutoriel, Azure DNS est utilisé comme exemple. Pour ajouter l’enregistrement TXT, vous devez suivre la documentation de votre service d’hébergement DNS.

Consultez la section [Vérification de l’état du nom de domaine](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-domain-name-status) documentation en cas de problème.

## Configuration de l’enregistrement DNS

>[!VIDEO](https://video.tv.adobe.com/v/3427907?quality=12&learn=on)

Pour configurer l’enregistrement DNS pour votre domaine personnalisé, procédez comme suit :

- Déterminez le type d’enregistrement DNS (CNAME ou APEX) en fonction du type de domaine, comme le domaine racine (APEX) ou le sous-domaine (CNAME), puis suivez les [Configuration des paramètres DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/configure-dns-settings) la documentation.
- Ajoutez l’enregistrement DNS dans votre service d’hébergement DNS.
- Déclenchez la validation de l’enregistrement DNS en suivant la [Vérification de l’état des enregistrements DNS](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/custom-domain-names/check-dns-record-status) la documentation.

Dans ce tutoriel, en tant que **subdomain** `wknd.enablementadobe.com` est utilisé, le type d’enregistrement CNAME qui pointe vers `cdn.adobeaemcloud.com` est ajouté.

Cependant, si vous utilisez la variable **domaine racine**, vous devez ajouter un type d’enregistrement APEX (alias A, ALIAS ou ANAME) qui pointe vers les adresses IP spécifiques fournies par l’Adobe.

## Vérification du site

>[!VIDEO](https://video.tv.adobe.com/v/3427904?quality=12&learn=on)

Pour vérifier que le site est accessible à l’aide du nom de domaine personnalisé, ouvrez un navigateur web et accédez à l’URL du domaine personnalisé. Assurez-vous que le site est accessible et que le navigateur affiche une connexion sécurisée avec l’icône de cadenas.

## Vidéo de bout en bout

Vous pouvez également regarder la vidéo de bout en bout qui présente l’aperçu, les conditions préalables et les étapes ci-dessus pour ajouter un nom de domaine personnalisé à AEM site hébergé as a Cloud Service.

>[!VIDEO](https://video.tv.adobe.com/v/3427817?quality=12&learn=on)


