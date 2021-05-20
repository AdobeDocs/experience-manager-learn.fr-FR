---
title: Déclencher AEM processus lors de l’envoi de formulaire HTM5
seo-title: Déclencher AEM processus lors de l’envoi d’un formulaire HTML5
description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
seo-description: Continuez à remplir le formulaire mobile en mode hors ligne et envoyez le formulaire mobile pour déclencher AEM processus.
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 1%

---


# Utilisation de ce cas pratique sur votre système

>[!NOTE]
>
>Pour que les exemples de ressources fonctionnent sur votre système, il est supposé que les instances d’auteur et de publication AEM s’exécutent respectivement sur les ports 4502 et 4503. Il est également supposé que l’auteur AEM est accessible via `admin`/`admin`. Si les numéros de port ou le mot de passe administrateur ont été modifiés, ces exemples de ressources ne fonctionneront pas. Vous devrez créer vos propres ressources à l’aide de l’exemple de code fourni.

Pour que ce cas pratique fonctionne sur votre système local, procédez comme suit :

* Installez l’instance AEM Author sur le port 4502 et l’instance AEM Publish sur le port 4503.
* [Suivez les instructions de la section Développement avec un utilisateur de service dans AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Veillez à créer l’utilisateur du service et à déployer le lot sur vos instances d’auteur et de publication AEM.
* [Ouvrez la configuration osgi  ](http://localhost:4503/system/console/configMgr).
* Recherchez **Apache Sling Referrer Filter**. Assurez-vous que la case Autoriser les champs vides est cochée.
* [Déployez le bundle AEMormDocumentService personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Ce bundle doit être déployé sur votre instance AEM Publish. Ce lot comporte le code permettant de générer un PDF interactif à partir de formulaires mobiles.
* [Téléchargez et décompressez les ressources liées à cet article.](assets/offline-pdf-submission-assets.zip) Vous obtiendrez ce qui suit :
   * **offline-submission-profile.zip**  : ce package AEM contient le profil personnalisé qui vous permet de télécharger le pdf interactif dans votre système de fichiers local. Déployez ce package sur votre instance AEM Publish.
   * **xdp-form-and-workflow.zip**  : ce package AEM contient XDP, exemple de workflow, lanceur configuré sur le noeud content/pdfenvois. Déployez ce package sur vos instances d’auteur et de publication AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  : il s’agit du lot d’AEM qui effectue la plupart du travail. Ce lot contient le servlet monté sur `/bin/startworkflow`. Ce servlet enregistre les données de formulaire envoyées sous le noeud `/content/pdfsubmissions` dans AEM référentiel. Déployez ce lot sur vos instances d’auteur et de publication AEM.
* [Aperçu du formulaire mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Renseignez plusieurs champs, puis cliquez sur le bouton de la barre d’outils pour télécharger le PDF interactif.
* Renseignez le PDF téléchargé à l’aide d’Acrobat et appuyez sur le bouton Envoyer.
* Vous devriez recevoir un message de réussite.
* Connectez-vous à l’instance d’auteur AEM en tant qu’administrateur.
* [Cocher la boîte de réception AEM](http://localhost:4502/aem/inbox)
* Vous devez disposer d’un élément de travail pour réviser le PDF envoyé.

>[!NOTE]
>
>Au lieu d’envoyer le PDF au servlet exécuté sur l’instance de publication, certains clients ont déployé le servlet dans un conteneur de servlets tel que Tomcat. Tout dépend de la topologie avec laquelle le client se sent à l’aise. Pour les besoins de ce tutoriel, nous allons utiliser le servlet déployé sur l’instance de publication pour gérer les envois pdf.

