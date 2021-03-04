---
title: Configuration d’OKTA avec AEM
description: Comprendre les différents paramètres de configuration pour l'utilisation de la connexion unique à l'aide des données
feature: administration
topics: development, authentication, security
audience: developer
doc-type: tutorial
activity: setup
version: 6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '763'
ht-degree: 3%

---


# Authentification à l’auteur AEM à l’aide d’OKTA

La première étape consiste à configurer votre application sur le portail OKTA. Une fois votre application approuvée par votre administrateur OKTA, vous aurez accès au certificat IdP et à l’URL de connexion unique. Voici les paramètres généralement utilisés pour enregistrer une nouvelle application.

* **Nom de l’application :** il s’agit du nom de l’application. Veillez à attribuer un nom unique à votre application.
* **DESTINATAIRE SAML :** après l’authentification à partir d’OKTA, il s’agit de l’URL qui sera consultée sur votre instance AEM avec la réponse SAML. Le gestionnaire d&#39;authentification SAML intercepte normalement tous les URL avec / saml_login, mais il est préférable de les ajouter après la racine de l&#39;application.
* **AUDIENCE** SAML : Il s’agit de l’URL de domaine de votre application. N’utilisez pas le protocole (http ou https) dans l’URL du domaine.
* **ID de nom SAML :** sélectionnez Courriel dans la liste déroulante.
* **Environnement** : Choisissez votre environnement approprié.
* **Attributs** : Il s’agit des attributs que vous obtenez sur l’utilisateur dans la réponse SAML. Indiquez-les en fonction de vos besoins.


![application okta](assets/okta-app-settings-blurred.PNG)


## Ajouter le certificat OKTA (IdP) au Trust Store AEM

Puisque les assertions SAML sont chiffrées, nous devons ajouter le certificat IdP (OKTA) au Trust Store de l&#39;AEM, pour permettre une communication sécurisée entre OKTA et AEM.
[Initialisez le Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html), si ce n’est déjà fait.
Souvenez-vous du mot de passe du Trust Store. Nous devrons utiliser ce mot de passe plus tard dans ce processus.

* Accédez à [Trust Store mondial](http://localhost:4502/libs/granite/security/content/truststore.html).
* Cliquez sur &quot;Ajouter le certificat du fichier CER&quot;. Ajoutez le certificat IdP fourni par OKTA et cliquez sur Envoyer.

   >[!NOTE]
   >
   >Veuillez ne pas mapper le certificat à un utilisateur.

Lors de l’ajout du certificat au Trust Store, vous devez obtenir un alias de certificat, comme illustré dans la capture d’écran ci-dessous. Le nom d’alias peut être différent dans votre cas.

![Alias du certificat](assets/cert-alias.PNG)

**Notez l’alias du certificat. Vous en aurez besoin dans les étapes suivantes.**

### Configuration du gestionnaire d’authentification SAML

Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
Recherchez et ouvrez &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Fournissez les propriétés suivantes, comme indiqué ci-dessous
Voici les propriétés clés à spécifier :

* **chemin**  : chemin d&#39;accès où le gestionnaire d&#39;authentification sera déclenché
* **URL** IdP : URL IdP fournie par OKTA
* **Alias** de certificat IDP : alias obtenu lorsque vous avez ajouté le certificat IdP dans AEM Trust Store
* **ID** d’entité prestataire : il s’agit du nom de votre serveur AEM
* **Mot de passe du magasin** de clés : mot de passe du Trust Store utilisé
* **Redirection** par défaut : il s’agit de l’URL vers laquelle rediriger une authentification réussie
* **Attribut** UserID:uid
* **Utiliser le chiffrement** : false
* **Créer automatiquement des utilisateurs** CRX:true
* **Ajouter aux groupes** : true
* **Groupes** par défaut:Navigateurs (groupe auquel les utilisateurs seront ajoutés). Vous pouvez fournir n’importe quel groupe existant dans AEM)
* **NamedIDPolicy** : Spécifie les contraintes relatives à l&#39;identifiant de nom à utiliser pour représenter l&#39;objet demandé. Copiez et collez la chaîne mise en surbrillance suivante **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Attributs**  synchronisés - Il s&#39;agit des attributs stockés à partir de l&#39;assertion SAML dans AEM profil

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configuration du filtre de Parrain Apache Sling

Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
Recherchez et ouvrez &quot;Apache Sling Parrain Filter&quot;.Définissez les propriétés suivantes comme indiqué ci-dessous :

* **Autoriser les champs vides** : true
* **Autoriser les hôtes** : Nom d’hôte IdP (différent dans votre cas)
* **Autoriser l&#39;hôte** Regexp : Nom d’hôte IdP (différent dans votre cas) Capture d’écran des propriétés du Parrain de filtre de Parrain Sling

![parrain-filtre](assets/sling-referrer-filter.PNG)

#### Configuration de la journalisation DEBUG pour l’intégration OKTA

Lors de la configuration de l’intégration OKTA sur AEM, il peut s’avérer utile de consulter les journaux DEBUG pour le gestionnaire d’authentification SAML AEM. Pour définir le niveau de journal sur DEBUG, créez une configuration Sling Logger via la console Web OSGi AEM.

Pensez à supprimer ou désactiver cette journalisation sur la scène et la production pour réduire le bruit de la journalisation.

Lors de la configuration de l&#39;intégration OKTA sur AEM, il peut s&#39;avérer utile de consulter les journaux DEBUG pour le gestionnaire d&#39;authentification SAML AEM. Pour définir le niveau de journal sur DEBUG, créez une configuration Sling Logger via la console Web OSGi AEM.
**Pensez à supprimer ou désactiver cette journalisation sur la scène et la production pour réduire le bruit de la journalisation.**
* Accédez à [configMgr](http://localhost:4502/system/console/configMgr)

* Recherchez et ouvrez &quot;Configuration du journal de journalisation Apache Sling&quot;.
* Créez un enregistreur avec la configuration suivante :
   * **Niveau** du journal : Débogage
   * **Fichier** journal : logs/saml.log
   * **Journaliseur** : com.adobe.granite.auth.saml
* Cliquez sur Enregistrer pour enregistrer vos paramètres



#### Testez votre configuration OKTA

Déconnectez-vous de votre instance AEM. Essayez d&#39;accéder au lien. Vous devriez voir OKTA SSO en action.
