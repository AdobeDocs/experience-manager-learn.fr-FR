---
title: 'Déclencher le workflow AEM lors de la soumission du formulaire HTM5 : appliquer ce cas d’utilisation'
seo-title: Trigger AEM Workflow on HTML5 Form Submission
description: Continuez à remplir le formulaire mobile en mode hors ligne et soumettez-le pour déclencher le workflow AEM.
seo-description: Continue filling mobile form in offline mode and submit mobile form to trigger AEM workflow
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
source-git-commit: 012850e3fa80021317f59384c57adf56d67f0280
workflow-type: ht
source-wordcount: '452'
ht-degree: 100%

---

# Appliquer ce cas d’utilisation sur votre système

>[!NOTE]
>
>Pour que les exemples de ressources fonctionnent sur votre système, les instances de création et de publication AEM doivent s’exécuter sur les ports 4502 et 4503, respectivement. L’instance de création AEM doit également être accessible via `admin`/`admin`. Si les numéros de port ou le mot de passe d’administration ont été modifiés, ces exemples de ressources ne fonctionneront pas. Vous devrez alors créer vos propres ressources à l’aide de l’exemple de code fourni.

Pour que ce cas d’utilisation fonctionne sur votre système local, procédez comme suit :

* Installez l’instance de création AEM sur le port 4502 et l’instance de publication AEM sur le port 4503.
* [Suivez les instructions indiquées dans la section Développer avec une personne utilisatrice de service dans AEM Forms](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/adaptive-forms/service-user-tutorial-develop.html?lang=fr). Assurez-vous de créer la personne utilisatrice de service et de déployer le lot sur vos instances de création et de publication AEM.
* [Ouvrez la configuration OSGi](http://localhost:4503/system/console/configMgr).
* Recherchez **Filtre référent Apache Sling**. Assurez-vous que la case Autoriser les champs vides est cochée.
* [Déployez le lot AEMFormDocumentService personnalisé](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar). Il doit être déployé sur votre instance de publication AEM. Le lot intègre le code permettant de générer un PDF interactif à partir d’un formulaire mobile.
* [Téléchargez et décompressez les ressources décrites dans cet article.](assets/offline-pdf-submission-assets.zip)Vous obtiendrez les éléments suivants :
   * **offline-submission-profile.zip** : ce package AEM contient le profil personnalisé qui vous permet de télécharger le PDF interactif sur votre système de fichiers local. Déployez ce package sur votre instance de publication AEM.
   * **xdp-form-and-workflow.zip** : ce package AEM contient XDP, l’exemple de workflow, le lanceur configuré sur le nœud content/pdfsubmissions. Déployez ce package sur vos instances de création et de publication AEM.
   * **HandlePDFSubmission.HandlePDFSubmission.core-1.0-SNAPSHOT.jar** : il s’agit du lot AEM responsable de la majeure partie du travail. Ce lot contient le servlet monté sur `/bin/startworkflow`. Ce servlet enregistre les données de formulaire soumises sous le nœud `/content/pdfsubmissions` dans le référentiel d’AEM. Déployez ce lot sur vos instances de création et de publication AEM.
* [Prévisualiser le formulaire mobile](http://localhost:4503/content/dam/formsanddocuments/testsubmision.xdp/jcr:content)
* Renseignez plusieurs champs, puis cliquez sur le bouton de la barre d’outils pour télécharger le PDF interactif.
* Complétez le PDF téléchargé à l’aide d’Acrobat et appuyez sur le bouton Envoyer.
* Un message indiquant la réussite de l’opération s’affiche.
* Connectez-vous à l’instance de création AEM en tant que personne administratrice.
* [Consulter la boîte de réception AEM](http://localhost:4502/aem/inbox)
* Vous devez disposer d’un élément de travail pour examiner le PDF envoyé.

>[!NOTE]
>
>Au lieu d’envoyer le PDF au servlet exécuté sur l’instance de publication, certains clientes et clients ont choisi de déployer le servlet dans un conteneur de servlets tel que Tomcat. Chaque client et cliente a ses préférences en matière de topologie. Pour les besoins de ce tutoriel, nous allons utiliser le servlet déployé sur l’instance de publication pour traiter les envois de formulaires PDF.
