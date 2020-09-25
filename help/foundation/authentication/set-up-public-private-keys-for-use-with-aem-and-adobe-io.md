---
seo: Set up public and private keys for use with AEM and Adobe I/O
description: 'aem utilise des paires clé publique/clé privée pour communiquer en toute sécurité avec les E/S d''Adobe et d''autres services Web. Ce court didacticiel illustre comment générer des clés et des fichiers de stockage de clés compatibles à l''aide de l''outil de ligne de commande openssl qui fonctionne avec les E/S AEM et Adobe. '
version: 6.4, 6.5
feature: authentication
topics: authentication, integrations
activity: setup
audience: architect, developer, implementer
doc-type: tutorial
kt: 2450
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 0%

---


# Configuration de clés publiques et privées à utiliser avec les E/S d&#39;Adobe

aem utilise des paires clé publique/clé privée pour communiquer en toute sécurité avec les E/S d&#39;Adobe et d&#39;autres services Web. Ce court didacticiel illustre comment générer des clés et des fichiers de stockage de clés compatibles à l&#39;aide de l&#39;outil de ligne de [!DNL openssl] commande qui fonctionne avec les E/S AEM et Adobe.

>[!CAUTION]
>
>Ce guide crée des clés autosignées utiles pour le développement et l’utilisation dans les environnements inférieurs. Dans les scénarios de production, les clés sont généralement générées et gérées par l’équipe de sécurité informatique d’une entreprise.

## Générer la paire de clés publique/privée {#generate-the-public-private-key-pair}

La [commande](https://www.openssl.org/docs/man1.0.2/man1/openssl.html) de l&#39;outil de ligne de commande [[!DNL req] [ !DNL openssl]](https://www.openssl.org/docs/man1.0.2/man1/req.html) peut être utilisée pour générer une paire de clés compatible avec les E/S d&#39;Adobe et Adobe Experience Manager.

```shell
$ openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate.crt
```

Pour exécuter la [!DNL openssl generate] commande, fournissez les informations relatives au certificat lorsque vous en faites la demande. Les E/S et AEM Adobes ne se soucient pas de ces valeurs, mais elles doivent s&#39;aligner sur et décrire votre clé.

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

## ajouter la paire de clés à un nouveau fichier de stockage de clés {#add-key-pair-to-a-new-keystore}

Vous pouvez ajouter des paires de clés à un nouveau fichier de stockage de [!DNL PKCS12] clés. Dans le cadre de la [[!DNL openssl]'s [!DNL pcks12] commande,](https://www.openssl.org/docs/man1.0.2/man1/pkcs12.html) le nom du fichier de stockage des clés (via `-  caname`), le nom de la clé (via `-name`) et le mot de passe du fichier de stockage des clés (via `-  passout`) sont définis.

Ces valeurs sont requises pour charger le fichier de stockage des clés et les clés dans AEM.

```shell
$ openssl pkcs12 -export -caname my-keystore -in certificate.crt -name my-key -inkey private.key -out keystore.p12 -passout pass:my-password
```

La sortie de cette commande est un `keystore.p12` fichier.

>[!NOTE]
>
>Les valeurs de paramètre de **[!DNL my-keystore]**, **[!DNL my-key]** et **[!DNL my-password]** doivent être remplacées par vos propres valeurs.

## Vérification du contenu du fichier de stockage des clés {#verify-the-keystore-contents}

L’outil [[!DNL keytool] de ligne de](https://docs.oracle.com/middleware/1213/wls/SECMG/keytool-summary-appx.htm#SECMG818) commande Java permet de visualiser les clés d’un fichier de stockage de clés afin de s’assurer qu’elles sont correctement chargées dans ce fichier ([!DNL keystore.p12]).

```shell
$ keytool -keystore keystore.p12 -list

Enter keystore password: my-password

Keystore type: jks
Keystore provider: SUN

Your keystore contains 1 entry

my-key, Feb 5, 2019, PrivateKeyEntry,
Certificate fingerprint (SHA1): 7C:6C:25:BD:52:D3:3B:29:83:FD:A2:93:A8:53:91:6A:25:1F:2D:52
```

![Vérifier le stockage des clés dans les E/S d&#39;Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

## ajouter le fichier de stockage des clés à AEM {#adding-the-keystore-to-aem}

aem utilise la clé **** privée générée pour communiquer en toute sécurité avec les E/S d&#39;Adobe et d&#39;autres services Web. Pour que la clé privée soit accessible à AEM, elle doit être installée dans le fichier de stockage des clés d’un utilisateur AEM.

Accédez à **AEM >[!UICONTROL Outils]>[!UICONTROL Sécurité]>[!UICONTROL Utilisateurs]** et modifiez l’utilisateur la clé privée doit être associée.****

### Création d’un fichier de stockage de clés AEM {#create-an-aem-keystore}

![Créer KeyStore dans AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--create-keystore.png)*AEM >[!UICONTROL Outils]>[!UICONTROL Sécurité]>[!UICONTROL Utilisateurs > Modifier l’utilisateur]*

Si vous êtes invité à créer un fichier de stockage de clés, faites-le. Ce fichier de stockage de clés n&#39;existera qu&#39;en AEM et n&#39;est PAS le fichier de stockage de clés créé par openssl. Le mot de passe peut être n’importe quoi et n’a pas besoin d’être identique au mot de passe utilisé dans la [!DNL openssl] commande.

### Installation de la clé privée via le fichier de stockage des clés {#install-the-private-key-via-the-keystore}

![ajouter la clé privée dans AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--add-private-key.png)*utilisateur[!UICONTROL >]Keystore[!UICONTROL >Ajouter la clé privée à partir du keystore]*

Dans la console de stockage des clés de l’utilisateur, cliquez sur **[!UICONTROL Ajouter une clé privée à partir du fichier]** KeyStore et ajoutez les informations suivantes :

* **[!UICONTROL Nouvel alias]**: l’alias de la clé en AEM. Il peut s’agir de n’importe quoi et ne doit pas nécessairement correspondre au nom du fichier de stockage de clés créé avec la commande openssl.
* **[!UICONTROL Fichier]** de stockage de clés : sortie de la commande openssl pkcs12 (keystore.p12)
* **[!UICONTROL Alias]** de clé privée : Mot de passe défini dans la commande openssl pkcs12 par `-  passout` argument.

* **[!UICONTROL Mot de passe]** de clé privée : Mot de passe défini dans la commande openssl pkcs12 par `-  passout` argument.

### Vérifiez que la clé privée est chargée dans le fichier de stockage des clés AEM. {#verify-the-private-key-is-loaded-into-the-aem-keystore}

![Vérification de la clé privée dans AEM](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/aem--keystore.png)*utilisateur[!UICONTROL >Keystore]*

Une fois que la clé privée a été chargée à partir du fichier de stockage de clés fourni dans le fichier de stockage de clés AEM, les métadonnées de la clé privée s’affichent dans la console de stockage de clés de l’utilisateur.

## ajouter la clé publique à l&#39;Adobe des E/S {#adding-the-public-key-to-adobe-i-o}

La clé publique correspondante doit être téléchargée sur les E/S d&#39;Adobe pour permettre à l&#39;utilisateur du service AEM, qui dispose de la clé publique privée correspondante pour communiquer en toute sécurité.

### Créer une nouvelle intégration d&#39;E/S d&#39;Adobe {#create-a-adobe-i-o-new-integration}

![Créer une nouvelle intégration d&#39;E/S d&#39;Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--create-new-integration.png)

*[[!UICONTROL Créer une intégration]](https://console.adobe.io/)E/S Adobe >[!UICONTROL Nouvelle intégration]*

La création d&#39;une nouvelle intégration dans les E/S d&#39;Adobe nécessite le transfert d&#39;un certificat public. Téléchargez le **certificat.crt** généré par la `openssl req` commande.

### Vérifier que les clés publiques sont chargées dans les E/S d&#39;Adobe {#verify-the-public-keys-are-loaded-in-adobe-i-o}

![Vérifier les clés publiques dans les E/S d&#39;Adobe](assets/set-up-public-private-keys-for-use-with-aem-and-adobe-io/adobe-io--public-keys.png)

Les clés publiques installées et leurs dates d&#39;expiration sont répertoriées dans la console [!UICONTROL Integrations] sur les E/S d&#39;Adobe. Vous pouvez ajouter plusieurs clés publiques par l’intermédiaire du bouton **[!UICONTROL Ajouter une clé]** publique.

Désormais, AEM conservez la clé privée et l&#39;intégration E/S de l&#39;Adobe contient la clé publique correspondante, ce qui permet aux AEM de communiquer en toute sécurité avec les E/S de l&#39;Adobe.
