---
title: Certification de document dans AEM Forms
description: Utilisation du service Docassurance pour certifier des documents PDF dans AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Développement
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '403'
ht-degree: 2%

---


# Certification de document dans AEM Forms

Un document certifié fournit au document PDF et aux destinataires de formulaires des garanties supplémentaires d’authenticité et d’intégrité.

Pour certifier un document, vous pouvez utiliser Acrobat DC sur le bureau ou AEM Forms Document Services dans le cadre d’un processus automatisé sur un serveur.

Cet article fournit un exemple de lot OSGI pour certifier des documents pdf à l’aide d’AEM Forms Document Services. Le code utilisé dans l’exemple est [disponible ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Pour certifier des documents à l’aide d’AEM Forms, les étapes suivantes doivent être suivies :

## Ajout d’un certificat à Trust Store {#adding-certificate-to-trust-store}

Suivez les étapes mentionnées ci-dessous pour ajouter le certificat au KeyStore dans AEM

* [Initialisation du Trust Store global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Recherchez fd-](http://localhost:4502/security/users.html) serviceuser
* **Vous devrez faire défiler la page de résultats pour charger tous les utilisateurs afin de trouver l’utilisateur fd-service.**
* Double-cliquez sur l’utilisateur fd-service pour ouvrir la fenêtre des paramètres utilisateur.
* Cliquez sur &quot;Ajouter une clé privée à partir du fichier de stockage de clés&quot;. Indiquez l’alias et le mot de passe propres à votre certificat.
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Enregistrez vos modifications

## Création du service OSGI

Vous pouvez créer votre propre lot OSGi et utiliser le SDK client AEM Forms pour mettre en oeuvre un service de certification des documents PDF. Les liens suivants seraient utiles pour écrire votre propre lot OSGi.

* [Création de votre premier lot OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Utilisation de l’API Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Vous pouvez également utiliser l’exemple de lot inclus dans les ressources de ce tutoriel.

>[!NOTE]
>
>L’exemple de lot utilise un alias appelé &quot;ares&quot; pour certifier les documents. Assurez-vous donc que votre alias est appelé &quot;ares&quot; lors de l’utilisation de ce lot.

## Test de l’échantillon sur votre système local

* Télécharger et installer [Offre groupée de services de document personnalisée](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Télécharger et installer [Développement avec le Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Assurez-vous d’avoir ajouté l’entrée suivante dans le service de mappeur d’utilisateur du service Apache Sling](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-** service comme illustré dans la capture d’écran ci-dessous
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importer un exemple de formulaire adaptatif](assets/certify-pdf-af.zip)
* [Importation et installation de l’envoi personnalisé](assets/custom-submit-certify.zip)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Télécharger un document PDF qui doit être certifié
   **facultatif**  : spécifiez le champ de signature à utiliser pour certifier le document.
* Cliquez sur Envoyer.
* Le PDF certifié doit vous être renvoyé.


