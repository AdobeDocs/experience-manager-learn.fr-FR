---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: AEM utilise des paires de clés publiques/privées pour communiquer en toute sécurité avec Adobe I/O et d’autres services web. Ce court tutoriel explique comment générer des clés et des KeyStore compatibles à l’aide de l’outil de ligne de commande openssl qui fonctionne avec AEM et Adobe I/O.
version: 6.4, 6.5
feature: User and Groups
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
topic: Development
role: Developer
level: Experienced
exl-id: 62ed9dcc-6b8a-48ff-8efe-57dabdf4aa66
last-substantial-update: 2022-07-17T00:00:00Z
thumbnail: KT-2450.jpg
source-git-commit: a156877ff4439ad21fb79f231d273b8983924199
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 0%

---

# Configuration de clés publiques et privées à utiliser avec Adobe I/O

AEM utilise des paires de clés publiques/privées pour communiquer en toute sécurité avec Adobe I/O et d’autres services web. Ce court tutoriel explique comment générer des clés et des KeyStore compatibles à l’aide de la variable [!DNL openssl] outil de ligne de commande qui fonctionne avec AEM et Adobe I/O.

>[!CAUTION]
>
>Ce guide crée des clés autosignées utiles pour le développement et l’utilisation dans des environnements inférieurs. Dans les scénarios de production, les clés sont généralement générées et gérées par l’équipe de sécurité informatique d’une entreprise.

## Génération de la paire de clés publique/privée {#generate-the-public-private-key-pair}

Le [[!DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) outil de ligne de commande [[!DNL req] command](https://www.openssl.org/docs/man1.0.2/man1/req.html) peut être utilisé pour générer une paire de clés compatible avec Adobe I/O et Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Pour terminer la [!DNL openssl generate] , fournissez les informations sur le certificat si nécessaire. L’Adobe I/O et l’AEM ne se soucient pas de ces valeurs, mais ils doivent s’aligner sur et décrire votre clé.

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

## Ajout d’une paire de clés à un nouveau KeyStore {#add-key-pair-to-a-new-keystore}

Des paires de clés peuvent être ajoutées à une nouvelle [!DNL PKCS12] keystore. Dans [[!DNL openssl]'s [!DNL pcks12] Commande,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) le nom du fichier de stockage des clés (via `-  caname`), le nom de la clé (via `-name`) et le mot de passe du KeyStore (via `-  passout`) sont définies.

Ces valeurs sont requises pour charger le KeyStore et les clés dans AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

La sortie de cette commande est une `keystore.p12` fichier .

>[!NOTE]
>
>Les valeurs de paramètre de **[!DNL my-keystore]**, **[!DNL my-key]** et **[!DNL my-password]** doivent être remplacées par vos propres valeurs.

## Vérification du contenu du KeyStore {#verify-the-keystore-contents}

Le Java™ [[!DNL keytool] outil de ligne de commande](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) fournit une visibilité sur un fichier de stockage de clés pour s’assurer que les clés sont correctement chargées dans le fichier de stockage de clés ([!DNL keystore.p12]).

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

AEM utilise la variable **clé privée** pour communiquer en toute sécurité avec Adobe I/O et d’autres services web. Pour que la clé privée soit accessible à AEM, elle doit être installée dans le KeyStore d’un utilisateur AEM.

Accédez à **AEM > [!UICONTROL Outils] > [!UICONTROL Sécurité] > [!UICONTROL Utilisateurs]** et **modifier l’utilisateur ;** la clé privée doit être associée à .

### Création d’un KeyStore d’AEM {#create-an-aem-keystore}

![Création de KeyStore dans AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)
*AEM > [!UICONTROL Outils] > [!UICONTROL Sécurité] > [!UICONTROL Utilisateurs] > Modifier l’utilisateur*

Lorsque vous êtes invité à créer un fichier de stockage de clés, procédez comme suit : Ce KeyStore existe uniquement dans AEM et n’est PAS le KeyStore créé via openssl. Le mot de passe peut être n’importe quoi et ne doit pas nécessairement être le même que le mot de passe utilisé dans la variable [!DNL openssl] .

### Installation de la clé privée via le KeyStore {#install-the-private-key-via-the-keystore}

![Ajout d’une clé privée dans AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)
*[!UICONTROL Utilisateur] > [!UICONTROL Keystore] > [!UICONTROL Ajout d’une clé privée à partir du KeyStore]*

Dans la console KeyStore de l’utilisateur, cliquez sur **[!UICONTROL Ajout d’un fichier KeyStore de formulaire de clé privée]** et ajoutez les informations suivantes :

* **[!UICONTROL Nouvel alias]**: l’alias de la clé dans AEM. Il peut s’agir de n’importe quoi et ne doit pas nécessairement correspondre au nom du KeyStore créé avec la commande openssl.
* **[!UICONTROL Fichier KeyStore]**: sortie de la commande openssl pkcs12 (keystore.p12)
* **[!UICONTROL Mot de passe du fichier KeyStore]**: Mot de passe défini dans la commande openssl pkcs12 via `-passout` argument .
* **[!UICONTROL Alias de clé privée]**: La valeur fournie à la variable `-name` dans la commande openssl pkcs12 ci-dessus (c’est-à-dire `my-key`).
* **[!UICONTROL Mot de passe de la clé privée]**: Mot de passe défini dans la commande openssl pkcs12 via `-passout` argument .

>[!CAUTION]
>
>Le mot de passe du fichier KeyStore et le mot de passe de la clé privée sont identiques pour les deux entrées. La saisie d’un mot de passe non compatible entraîne l’importation de la clé.

### Vérifiez que la clé privée est chargée dans le KeyStore AEM {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Vérification de la clé privée dans AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)
*[!UICONTROL Utilisateur] > [!UICONTROL Keystore]*

Lorsque la clé privée est correctement chargée du fichier de stockage des clés fourni dans le fichier de stockage des clés d’AEM, les métadonnées de la clé privée s’affichent dans la console du fichier de stockage des clés de l’utilisateur.

## Ajout de la clé publique à l’Adobe I/O {#adding-the-public-key-to-adobe-i-o}

La clé publique correspondante doit être chargée dans Adobe I/O pour permettre à l’utilisateur du service AEM, qui dispose de la clé publique privée correspondante de communiquer en toute sécurité.

### Création d’une intégration Adobe I/O {#create-a-adobe-i-o-new-integration}

![Création d’une intégration Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Création d’une intégration Adobe I/O]](https://developer.adobe.com/console/) > [!UICONTROL Nouvelle intégration]*

La création d’une intégration dans Adobe I/O nécessite le chargement d’un certificat public. Téléchargez le **certificate.crt** généré par le `openssl req` .

### Vérification du chargement des clés publiques dans Adobe I/O {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Vérification des clés publiques dans Adobe I/O](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Les clés publiques installées et leurs dates d’expiration sont répertoriées dans la variable [!UICONTROL Intégrations] sur Adobe I/O. Plusieurs clés publiques peuvent être ajoutées via le **[!UICONTROL Ajout d’une clé publique]** bouton .

Désormais, AEM contient la clé privée et l’intégration de l’Adobe I/O contient la clé publique correspondante, ce qui permet à AEM de communiquer en toute sécurité avec l’Adobe I/O.
