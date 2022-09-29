---
title: Configuration d’OKTA avec AEM
description: Présentation des différents paramètres de configuration pour l’utilisation de l’authentification unique à l’aide des données d’identification
feature: Adaptive Forms
version: 6.5
topic: Administration
role: Admin
level: Experienced
exl-id: 85c9b51e-92bb-4376-8684-57c9c3204b2f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 2%

---

# Authentification à l’auteur AEM à l’aide d’OKTA

La première étape consiste à configurer votre application sur le portail OKTA. Une fois votre application approuvée par votre administrateur OKTA, vous avez accès au certificat IdP et à l’URL de connexion unique. Voici les paramètres généralement utilisés pour enregistrer une nouvelle application.

* **Nom de l’application :** Il s’agit du nom de votre application. Veillez à attribuer un nom unique à votre application.
* **Destinataire SAML :** Après l’authentification à partir d’OKTA, il s’agit de l’URL qui sera atteinte sur votre instance AEM avec la réponse SAML. Le gestionnaire d’authentification SAML intercepte normalement toutes les URL de avec / saml_login, mais il est préférable de l’ajouter après la racine de votre application.
* **Audience SAML**: Il s’agit de l’URL du domaine de votre application. N’utilisez pas le protocole (http ou https) dans l’URL du domaine.
* **ID de nom SAML :** Sélectionnez Email dans la liste déroulante.
* **Environnement**: Choisissez l’environnement approprié.
* **Attributs**: Il s’agit des attributs que vous obtenez sur l’utilisateur dans la réponse SAML. Indiquez-les selon vos besoins.


![okta-application](assets/okta-app-settings-blurred.PNG)


## Ajoutez le certificat OKTA (IdP) au Trust Store d’AEM

Puisque les assertions SAML sont chiffrées, nous devons ajouter le certificat IdP (OKTA) au trust store d’AEM, afin de permettre une communication sécurisée entre OKTA et AEM.
[Initialisation de Trust Store](http://localhost:4502/libs/granite/security/content/truststore.html), s’il n’est pas déjà initialisé.
Souvenez-vous du mot de passe du Trust Store. Nous devrons utiliser ce mot de passe plus tard dans ce processus.

* Accédez à [Trust Store mondial](http://localhost:4502/libs/granite/security/content/truststore.html).
* Cliquez sur &quot;Ajouter un certificat à partir du fichier CER&quot;. Ajoutez le certificat IdP fourni par OKTA et cliquez sur Envoyer.

   >[!NOTE]
   >
   >Ne mappez le certificat à aucun utilisateur.

Lors de l’ajout du certificat au Trust Store, vous devez obtenir un alias de certificat comme illustré dans la capture d’écran ci-dessous. Le nom d’alias peut être différent dans votre cas.

![Certificate-alias](assets/cert-alias.PNG)

**Notez l’alias du certificat. Vous en aurez besoin lors des étapes suivantes.**

### Configuration du gestionnaire d’authentification SAML

Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
Recherchez et ouvrez &quot;Adobe Granite SAML 2.0 Authentication Handler&quot;.
Indiquez les propriétés suivantes, comme indiqué ci-dessous. Voici les propriétés clés à spécifier :

* **path** - Il s’agit du chemin d’accès où le gestionnaire d’authentification est déclenché.
* **IdP Url**: il s’agit de votre URL IdP fournie par OKTA.
* **Alias de certificat IDP**: alias obtenu lorsque vous avez ajouté le certificat IdP au Trust Store d’AEM
* **Identifiant d’entité du fournisseur de services**: il s’agit du nom de votre serveur AEM.
* **Mot de passe du KeyStore**: mot de passe du trust store que vous avez utilisé
* **Redirection par défaut**: URL vers laquelle rediriger une authentification réussie
* **Attribut UserID**:uid
* **Utiliser le chiffrement**:false
* **Création automatique d’utilisateurs CRX**:true
* **Ajouter aux groupes**:true
* **Groupes par défaut**:ktausers(groupe auquel les utilisateurs sont ajoutés. Vous pouvez fournir n’importe quel groupe existant dans AEM)
* **NamedIDPolicy**: Spécifie les contraintes sur l’identifiant de nom à utiliser pour représenter l’objet demandé. Copiez et collez la chaîne mise en surbrillance suivante **urn:oasis:names:tc:SAML:2.0:nameidformat:emailAddress**
* **Attributs synchronisés** - Il s’agit des attributs stockés à partir de l’assertion SAML dans AEM profil.

![saml-authentication-handler](assets/saml-authentication-settings-blurred.PNG)

### Configuration du filtre de référent Apache Sling

Accédez à [configMgr](http://localhost:4502/system/console/configMgr).
Recherchez et ouvrez &quot;Apache Sling Referrer Filter&quot;. Définissez les propriétés suivantes comme indiqué ci-dessous :

* **Autoriser vide**: false
* **Autorisation des hôtes**: Nom d’hôte IdP (différent dans votre cas)
* **Autoriser l’hôte Regexp**: Nom d’hôte IdP (différent dans votre cas) Copie d’écran des propriétés du référent du filtre de référent Sling

![referrer-filter](assets/okta-referrer.png)

#### Configuration de la journalisation DEBUG pour l’intégration OKTA

Lors de la configuration de l’intégration OKTA sur AEM, il peut s’avérer utile de consulter les journaux DEBUG pour AEM gestionnaire d’authentification SAML. Pour définir le niveau de journalisation sur DEBUG, créez une configuration Sling Logger via la console web OSGi AEM.

N’oubliez pas de supprimer ou de désactiver cet enregistreur dans les environnements intermédiaire et de production pour réduire le bruit de journal.

Lors de la configuration de l’intégration OKTA sur AEM, il peut s’avérer utile de consulter les journaux DEBUG pour AEM gestionnaire d’authentification SAML. Pour définir le niveau de journalisation sur DEBUG, créez une configuration Sling Logger via la console web OSGi AEM.
**N’oubliez pas de supprimer ou de désactiver cet enregistreur dans les environnements intermédiaire et de production pour réduire le bruit de journal.**
* Accédez à [configMgr](http://localhost:4502/system/console/configMgr)

* Recherchez et ouvrez &quot;Configuration de l’enregistreur de journalisation Apache Sling&quot;.
* Créez un enregistreur avec la configuration suivante :
   * **Niveau de journal**: Déboguer
   * **Fichier journal**: logs/saml.log
   * **Enregistreur**: com.adobe.granite.auth.saml
* Cliquez sur Enregistrer pour enregistrer vos paramètres.

#### Test de votre configuration OKTA

Déconnectez-vous de votre instance AEM. Essayez d’accéder au lien. Vous devriez voir OKTA SSO en action.
