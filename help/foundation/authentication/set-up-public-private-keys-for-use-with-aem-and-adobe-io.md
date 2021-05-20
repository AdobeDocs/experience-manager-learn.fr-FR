---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'AEM utilise des paires de clés publiques/privées pour communiquer en toute sécurité avec Adobe I/O et d’autres services web. Ce court tutoriel explique comment générer des clés et des KeyStore compatibles à l’aide de l’outil de ligne de commande openssl qui fonctionne avec AEM et Adobe I/O. '
version: 6.4, 6.5
feature: 'Utilisateurs et groupes '
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '772'
ht-degree: 0%

---


# Configuration de clés publiques et privées à utiliser avec Adobe I/O

AEM utilise des paires de clés publiques/privées pour communiquer en toute sécurité avec Adobe I/O et d’autres services web. Ce court tutoriel explique comment générer des clés et des KeyStore compatibles à l’aide de l’outil de ligne de commande [!DNL openssl] qui fonctionne avec AEM et Adobe I/O.

>[!CAUTION]
>
>Ce guide crée des clés autosignées utiles pour le développement et l’utilisation dans des environnements inférieurs. Dans les scénarios de production, les clés sont généralement générées et gérées par l’équipe de sécurité informatique d’une entreprise.

## Générer la paire de clés publique/privée {#generate-the-public-private-key-pair}

La [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) ligne de commande [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) de l’outil de ligne de commande peut être utilisée pour générer une paire de clés compatible avec Adobe I/O et Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Pour terminer la commande [!DNL openssl generate], fournissez les informations sur le certificat lorsque cela est demandé. L’Adobe I/O et l’AEM ne se soucient pas de ces valeurs, mais ils doivent s’aligner sur et décrire votre clé.

```
Generating a 2048 bit RSA private key
...........................................................+++
...+++
writing new private key to 'private.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) []:US
State or Province Name (full name) []:CA
Locality Name (eg, city) []:San Jose
Organization Name (eg, company) []:Example Co
Organizational Unit Name (eg, section) []:Digital Marketing
Common Name (eg, fully qualified host name) []:com.example
Email Address []:me@example.com
```

## Ajoutez une paire de clés au nouveau KeyStore {#add-key-pair-to-a-new-keystore}

Vous pouvez ajouter des paires de clés à un nouveau fichier [!DNL PKCS12] KeyStore. Dans le cadre de la commande [[!DNL openssl]'s [!DNL pcks12] ,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) le nom du KeyStore (via `-  caname`), le nom de la clé (via `-name`) et le mot de passe du KeyStore (via `-  passout`) sont définis.

Ces valeurs sont requises pour charger le KeyStore et les clés dans AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

La sortie de cette commande est un fichier `keystore.p12`.

>[!NOTE]
>
>Les valeurs des paramètres **[!DNL my-keystore]**, **[!DNL my-key]** et **[!DNL my-password]** doivent être remplacées par vos propres valeurs.

## Vérifiez le contenu du KeyStore {#verify-the-keystore-contents}

L’outil de ligne de commande Java [[!DNL keytool] ](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) offre une visibilité dans un fichier de stockage de clés pour s’assurer que les clés sont correctement chargées dans le fichier de stockage de clés ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Vérification du magasin de clés dans Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## Ajout du KeyStore à AEM {#adding-the-keystore-to-aem}

AEM utilise la **clé privée** générée pour communiquer en toute sécurité avec Adobe I/O et d’autres services Web. Pour que la clé privée soit accessible à AEM, elle doit être installée dans le KeyStore d’un utilisateur AEM.

Accédez à **AEM > [!UICONTROL Outils] > [!UICONTROL Sécurité] > [!UICONTROL Utilisateurs]** et **modifiez l’utilisateur** auquel la clé privée doit être associée.

### Création d’un fichier de stockage de clés d’AEM {#create-an-aem-keystore}

![Créer KeyStore dans ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM >  [!UICONTROL Outils]  >  [!UICONTROL Sécurité]  >  [!UICONTROL Utilisateurs]  > Modifier l’utilisateur*

Si vous êtes invité à créer un fichier de stockage de clés, procédez de la sorte. Ce KeyStore existe uniquement dans AEM et n’est PAS le KeyStore créé via openssl. Le mot de passe peut être n’importe quoi et ne doit pas nécessairement être le même que le mot de passe utilisé dans la commande [!DNL openssl].

### Installez la clé privée via le KeyStore {#install-the-private-key-via-the-keystore}

![Ajouter une clé privée dans ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL AEMUser]  >  [!UICONTROL Keystore]  >  [!UICONTROL Ajouter une clé privée à partir du KeyStore]*

Dans la console KeyStore de l’utilisateur, cliquez sur **[!UICONTROL Ajouter le fichier KeyStore de la clé privée]** et ajoutez les informations suivantes :

* **[!UICONTROL Nouvel alias]** : l’alias de la clé dans AEM. Il peut s’agir de n’importe quoi et ne doit pas nécessairement correspondre au nom du KeyStore créé avec la commande openssl.
* **[!UICONTROL Fichier KeyStore]** : sortie de la commande openssl pkcs12 (keystore.p12)
* **[!UICONTROL KeyStore File Password]** : Mot de passe défini dans la commande openssl pkcs12 via l’ `-passout` argument .
* **[!UICONTROL Alias de clé privée]** : La valeur fournie à l’ `-name` argument dans la commande openssl pkcs12 ci-dessus (c.-à-d.  `my-key`).
* **[!UICONTROL Mot de passe]** de la clé privée : Mot de passe défini dans la commande openssl pkcs12 via l’ `-passout` argument .

>[!CAUTION]
>
>Le mot de passe du fichier KeyStore et le mot de passe de la clé privée sont identiques pour les deux entrées. La saisie d’un mot de passe non compatible entraîne l’importation de la clé.

### Vérifiez que la clé privée est chargée dans le KeyStore AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Vérifier la clé privée dans ](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL AEMUser]  >  [!UICONTROL Keystore]*

Lorsque la clé privée est correctement chargée du fichier de stockage des clés fourni dans le fichier de stockage des clés d’AEM, les métadonnées de la clé privée s’affichent dans la console du fichier de stockage des clés de l’utilisateur.

## Ajout de la clé publique pour l’Adobe I/O {#adding-the-public-key-to-adobe-i-o}

La clé publique correspondante doit être chargée dans Adobe I/O pour permettre à l’utilisateur du service AEM, qui dispose de la clé publique privée correspondante de communiquer en toute sécurité.

### Créer une intégration d’Adobe I/O {#create-a-adobe-i-o-new-integration}

![Création d’une intégration Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Créer une intégration Adobe I/O]](https://console.adobe.io/)  >  [!UICONTROL Nouvelle intégration]*

La création d’une intégration dans Adobe I/O nécessite le chargement d’un certificat public. Téléchargez le **certificate.crt** généré par la commande `openssl req`.

### Vérifiez que les clés publiques sont chargées dans l’Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Vérification des clés publiques dans Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Les clés publiques installées et leurs dates d’expiration sont répertoriées dans la console [!UICONTROL Intégrations] de l’Adobe I/O. Plusieurs clés publiques peuvent être ajoutées à partir du bouton **[!UICONTROL Ajouter une clé publique]** .

Désormais, AEM contient la clé privée et l’intégration de l’Adobe I/O détient la clé publique correspondante, ce qui permet à AEM de communiquer en toute sécurité avec l’Adobe I/O.
