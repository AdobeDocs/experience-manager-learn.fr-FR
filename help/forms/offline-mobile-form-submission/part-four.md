---
title: Déclencher le processus AEM sur l’envoi de formulaires HTML5
seo-title: Déclencher le flux de travail AEM sur l’envoi de formulaire HTML5
description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
seo-description: Continuer à remplir le formulaire pour périphériques mobiles en mode hors ligne et envoyer le formulaire pour périphériques mobiles pour déclencher AEM processus
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 1%

---


# Utilisation de ce cas d&#39;utilisation sur votre système

>[!NOTE]
>
>Pour que les exemples de ressources fonctionnent sur votre système, il est supposé que les instances d’auteur et de publication AEM s’exécutent respectivement sur les ports 4502 et 4503. Il est également supposé que l’auteur AEM est accessible via `admin`/`admin`. Si les numéros de port ou le mot de passe d’administrateur ont été modifiés, ces exemples de ressources ne fonctionneront pas. Vous devrez créer vos propres ressources à l’aide de l’exemple de code fourni.

Pour que ce cas d&#39;utilisation fonctionne sur votre système local, procédez comme suit :

* Installez l’instance d’auteur AEM sur le port 4502 et l’instance de publication AEM sur le port 4503.
* [Suivez les instructions indiquées dans le développement avec un utilisateur de service en AEM Forms](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html). Veillez à créer l’utilisateur du service et à déployer le lot sur vos instances d’auteur et de publication AEM.
* [Ouvrez la configuration osgi  ](http://localhost:4503/system/console/configMgr).
* Recherchez **Apache Sling Parrain Filter**. Assurez-vous que la case à cocher Autoriser les champs vides est activée.
* [Déployez le lot](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar) AEMFormDocumentService personnalisé.Ce lot doit être déployé sur votre instance de publication AEM. Ce lot comporte le code permettant de générer un PDF interactif à partir d’un formulaire pour périphériques mobiles.
* [Téléchargez et décompressez les ressources liées à cet article.](assets/offline-pdf-submission-assets.zip) Vous obtiendrez les éléments suivants :
   * **offline-submission-profil.zip**  - Ce package AEM contient le profil personnalisé qui vous permet de télécharger le fichier pdf interactif sur votre système de fichiers local. Déployez ce package sur votre instance de publication AEM.
   * **xdp-form-and-workflow.zip**  - Ce package AEM contient XDP, exemple de flux de travail, lanceur configuré sur le contenu de noeud/pdfSubmissions. Déployez ce package sur vos instances d’auteur et de publication AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar**  - Il s&#39;agit du lot AEM qui effectue la majeure partie du travail. Ce lot contient la servlet montée sur `/bin/startworkflow`. Cette servlet enregistre les données de formulaire envoyées sous le noeud `/content/pdfsubmissions` dans le référentiel AEM. Déployez ce lot sur vos instances d’auteur et de publication AEM.
* [Prévisualisation du formulaire mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Renseignez plusieurs champs, puis cliquez sur le bouton de la barre d’outils pour télécharger le PDF interactif.
* Renseignez le PDF téléchargé à l’aide d’Acrobat et cliquez sur le bouton Envoyer.
* Vous devriez recevoir un message de réussite.
* Connexion à l’instance Auteur AEM en tant qu’administrateur
* [Cochez la boîte de réception AEM](http://localhost:4502/aem/inbox)
* Vous devez disposer d’un élément de travail pour réviser le PDF envoyé.

>[!NOTE]
>
>Au lieu d’envoyer le PDF à la servlet s’exécutant sur l’instance de publication, certains clients ont déployé la servlet dans le conteneur de servlet tel que Tomcat. Tout dépend de la topologie avec laquelle le client se sent à l’aise.Dans ce didacticiel, nous allons utiliser la servlet déployée sur l’instance de publication pour gérer les envois pdf.

