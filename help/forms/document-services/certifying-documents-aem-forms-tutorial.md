---
title: Certification de document dans AEM Forms
description: Utilisation du service Docassurance pour certifier des documents PDF dans AEM Forms
feature: Document Security
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 1471929f-d269-4adc-88ad-2ad3682305e1
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 3%

---

# Certification de document dans AEM Forms

Un document certifié fournit au PDF un document et aux destinataires des formulaires des garanties supplémentaires d’authenticité et d’intégrité.

Pour certifier un document, vous pouvez utiliser Acrobat DC sur le bureau ou AEM Forms Document Services dans le cadre d’un processus automatisé sur un serveur.

Cet article fournit un exemple de lot OSGI pour certifier des documents pdf à l’aide d’AEM Forms Document Services. Le code utilisé dans l’exemple est [disponible ici](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Pour certifier des documents à l’aide d’AEM Forms, les étapes suivantes doivent être suivies :

## Ajout d’un certificat à Trust Store {#adding-certificate-to-trust-store}

Suivez les étapes mentionnées ci-dessous pour ajouter le certificat au KeyStore dans AEM

* [Initialisation du Trust Store global](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Recherche de fd-service](http://localhost:4502/security/users.html) user
* **Vous devrez faire défiler la page de résultats pour charger tous les utilisateurs afin de trouver l’utilisateur fd-service.**
* Double-cliquez sur l’utilisateur fd-service pour ouvrir la fenêtre des paramètres utilisateur.
* Cliquez sur &quot;Ajouter une clé privée à partir du fichier de stockage de clés&quot;. Indiquez l’alias et le mot de passe propres à votre certificat.
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Enregistrez vos modifications

## Création du service OSGI

Vous pouvez créer votre propre lot OSGi et utiliser le SDK client AEM Forms pour mettre en oeuvre un service de certification des documents du PDF. Les liens suivants seraient utiles pour écrire votre propre lot OSGi.

* [Création de votre premier lot OSGi](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/developing/aem-project-archetype.html?lang=fr)
* [Utilisation de l’API Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Vous pouvez également utiliser l’exemple de lot inclus dans les ressources de ce tutoriel.

>[!NOTE]
>
>L’exemple de lot utilise un alias appelé &quot;ares&quot; pour certifier les documents. Assurez-vous donc que votre alias est appelé &quot;ares&quot; lors de l’utilisation de ce lot.

## Test de l’échantillon sur votre système local

* Télécharger et installer [Offre groupée de services de document personnalisés](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Télécharger et installer [Développement avec le bundle d’utilisateurs du service](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Assurez-vous d’avoir ajouté l’entrée suivante dans le service de mappeur d’utilisateur du service Apache Sling](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** comme illustré dans la capture d’écran ci-dessous
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importer un exemple de formulaire adaptatif](assets/certify-pdf-af.zip)
* [Importation et installation de l’envoi personnalisé](assets/custom-submit-certify.zip)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Télécharger le document du PDF qui doit être certifié
   **facultatif** - Spécifiez le champ de signature à utiliser pour certifier le document.
* Cliquez sur Envoyer.
* Le PDF certifié doit vous être renvoyé.
