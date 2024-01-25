---
title: Certifier des documents dans AEM Forms
description: Utiliser le service Docassurance pour certifier des documents PDF dans AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
duration: 84
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 100%

---

# Certifier des documents dans AEM Forms

Un document certifié fournit aux personnes destinataires d’un document et de formulaires PDF des garanties supplémentaires sur leur authenticité et leur intégrité.

Pour certifier un document, vous pouvez utiliser Acrobat DC sur le bureau ou AEM Forms Document Services dans le cadre d’un processus automatisé sur un serveur.

Cet article fournit un exemple de lot OSGI pour certifier des documents PDF à l’aide d’AEM Forms Document Services. Le code utilisé dans l’exemple est [disponible ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html).

Pour certifier des documents à l’aide d’AEM Forms, procédez comme suit :

## Ajouter un certificat à Trust Store {#adding-certificate-to-trust-store}

Suivez les étapes mentionnées ci-dessous pour ajouter le certificat au KeyStore dans AEM

* [Initialiser un Trust Store global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Rechercher l’utilisateur ou l’utilisatrice fd-service](http://localhost:4502/security/users.html)
* **Vous devrez faire défiler la page de résultats pour charger tous les utilisateurs et utilisatrices afin de trouver l’utilisateur ou l’utilisatrice fd-service.**
* Double-cliquez sur l’utilisateur ou l’utilisatrice fd-service pour ouvrir la fenêtre des paramètres utilisateur.
* Cliquez sur « Ajouter une clé privée à partir du fichier de stockage de clés ». Indiquez l’alias et le mot de passe propres à votre certificat.
  ![add-certificate](assets/adding-certificate-keystore.PNG)
* Enregistrez vos modifications.

## Créer le service OSGI

Vous pouvez créer votre propre lot OSGi et utiliser le SDK client AEM Forms pour mettre en œuvre un service de certification des documents PDF. Les liens suivants peuvent être utiles pour écrire votre propre lot OSGi.

* [Créer votre premier lot OSGi](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html?lang=fr)
* [Utiliser l’API Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Vous pouvez également utiliser l’exemple de lot inclus dans les ressources de ce tutoriel.

>[!NOTE]
>
>L’exemple de lot utilise un alias appelé « ares » pour certifier les documents. Assurez-vous donc que votre alias est nommé « ares » lors de l’utilisation de ce lot.

## Tester le modèle sur votre système local

* Téléchargez et installez le [lot Document Services personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar).
* Téléchargez et installez le [lot Developing with Service User](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar).
* [Assurez-vous d’avoir ajouté l’entrée suivante dans le service de mappage des utilisateurs et utilisatrices du service Apache Sling](http://localhost:4502/system/console/configMgr) :
  **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** comme illustré dans la capture d’écran ci-dessous.
  ![User-Mapper](assets/user-mapper-service.PNG)
* [Importer un exemple de formulaire adaptatif](assets/certify-pdf-af.zip)
* [Importer et installer l’envoi personnalisé](assets/custom-submit-certify.zip)
* [Ouvrez le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled).
* Chargez un document PDF devant être certifié.
  **Facultatif** : spécifiez le champ de signature à utiliser pour certifier le document.
* Cliquez sur Envoyer.
* Le PDF certifié doit vous être renvoyé.
