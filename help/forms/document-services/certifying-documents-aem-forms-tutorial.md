---
title: Document de certification en AEM Forms
seo-title: Document de certification en AEM Forms
description: Utilisation du service Docassurance pour certifier des documents PDF dans AEM Forms
seo-description: Utilisation du service Docassurance pour certifier des documents PDF dans AEM Forms
uuid: ecb1f9b6-bbb3-43a3-a0e0-4c04411acc9f
feature: document-security
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: ca4a8f02ea9ec5db15dbe6f322731748da90be6b
workflow-type: tm+mt
source-wordcount: '416'
ht-degree: 1%

---


# Document de certification en AEM Forms

Un Document certifié fournit aux destinataires de document PDF et de formulaires des garanties supplémentaires d’authenticité et d’intégrité.

Pour certifier un document, vous pouvez utiliser Acrobat DC sur le bureau ou AEM Forms Document Services dans le cadre d’un processus automatisé sur un serveur.

Cet article fournit un exemple de lot OSGI pour certifier des documents pdf à l&#39;aide des services de Document AEM Forms. Le code utilisé dans l&#39;exemple est [disponible ici.](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Pour certifier des documents à l’aide d’AEM Forms, les étapes suivantes doivent être suivies :

## ajouter un certificat au Trust Store {#adding-certificate-to-trust-store}

Suivez les étapes mentionnées ci-dessous pour ajouter le certificat au fichier de stockage des clés dans AEM

* [Initialisation de la Trust Store globale](http://localhost:4502/libs/granite/security/content/truststore.html)
* [Rechercher un utilisateur de service](http://localhost:4502/security/users.html) fd
* **Vous devez faire défiler la page de résultats pour charger tous les utilisateurs afin de trouver l’utilisateur du service fd.**
* Doublon de cliquer sur l’utilisateur du service fd pour ouvrir la fenêtre des paramètres utilisateur
* Cliquez sur &quot;Ajouter la clé privée à partir du fichier de stockage de clés&quot;.Spécifiez l&#39;alias et le mot de passe propres à votre certificat.
   ![add-certificate](assets/adding-certificate-keystore.PNG)
* Enregistrez vos modifications

## Création du service OSGI

Vous pouvez créer votre propre lot OSGi et utiliser le SDK client AEM Forms pour mettre en oeuvre un service de certification des documents PDF. Les liens suivants seraient utiles pour écrire votre propre offre OSGi

* [Création de votre premier lot OSGi](https://helpx.adobe.com/experience-manager/using/maven_arch13.html)
* [Utiliser l&#39;API Document Service](https://helpx.adobe.com/experience-manager/6-4/forms/using/aem-document-services-programmatically.html)

Vous pouvez également utiliser l’exemple de lot inclus dans les ressources de ce didacticiel.
>[!NOTE]
L’exemple d’assemblage utilise un alias appelé &quot;ares&quot; pour certifier les documents. Assurez-vous donc que votre alias s’appelle &quot;ares&quot; lors de l’utilisation de ce lot.

## Test de l&#39;exemple sur votre système local

* Téléchargement et installation de l&#39;offre groupée [Custom Document Services](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Télécharger et installer [Developing with Service User Bundle](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* [Assurez-vous d’avoir ajouté l’entrée suivante dans le service Apache Sling User Mapper Service.](http://localhost:4502/system/console/configMgr)

   **DevelopingWithServiceUser.core:getformsresourceresolver=fd-service** comme illustré dans la capture d’écran ci-dessous
   ![User-Mapper](assets/user-mapper-service.PNG)
* [Importer un exemple de formulaire adaptatif](assets/certify-pdf-af.zip)
* [Importer et installer l’envoi personnalisé](assets/custom-submit-certify.zip)
* [Ouvrir le formulaire adaptatif](http://localhost:4502/content/dam/formsanddocuments/certifypdf/jcr:content?wcmmode=disabled)
* Télécharger un document PDF qui doit être certifié
   **facultatif** - Indiquez le champ de signature à utiliser pour la certification du document.
* Cliquez sur Envoyer.
* Un PDF certifié doit vous être renvoyé.


