---
title: Procédure simplifiée d’installation d’AEM Forms sous Windows
seo-title: Procédure simplifiée d’installation d’AEM Forms sous Windows
description: Procédure rapide et simple d’installation d’AEM Forms sous Windows
seo-description: Procédure rapide et simple d’installation d’AEM Forms sous Windows
uuid: a148b8f0-83db-47f6-89d3-c8a9961be289
feature: Adaptive Forms
topics: administration
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
discoiquuid: 1182ef4d-5838-433b-991d-e24ab805ae0e
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 9%

---


# Procédure simplifiée d’installation d’AEM Forms sous Windows

>[!NOTE]
>
>Ne cliquez jamais sur l&#39;AEM JAR de Début rapide si vous avez l&#39;intention d&#39;utiliser AEM Forms.
>
>Assurez-vous également qu’il n’y a aucun espace dans le chemin d’accès du dossier d’installation AEM Forms.
>
>Par exemple, n’installez pas AEM Forms dans c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Si vous installez AEM Forms 6.5, assurez-vous d&#39;avoir installé les redistributables Microsoft Visual C++ 32 bits suivants.
>
>* Redistribuable Microsoft Visual C++ 2008
>* Redistribuable Microsoft Visual C++ 2010
>* Redistribuable Microsoft Visual C++ 2012
>* Redistribuable Microsoft Visual C++ 2013 (à partir de la version 6.5)


Bien que nous vous recommandons de suivre la [documentation officielle](https://helpx.adobe.com/fr/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) pour installer AEM Forms. Pour installer et configurer AEM Forms sur l’environnement Windows, procédez comme suit :

* Assurez-vous que le JDK approprié est installé.
   * AEM 6.2 : Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 et AEM 6.4 vous avez besoin de : Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 vous avez besoin de JDK 8 ou JDK 11
* [Les ](https://helpx.adobe.com/fr/experience-manager/6-3/sites/deploying/using/technical-requirements.html) exigences JDK officielles sont répertoriées ici
* Assurez-vous que JAVA_HOME est défini pour pointer vers le JDK que vous avez installé.
   * Pour créer la variable JAVA_HOME dans Windows, procédez comme suit :
      * Cliquez sur Poste de travail avec le bouton droit de la souris et sélectionnez Propriétés.
      * Dans l’onglet Avancé, sélectionnez Variables d’Environnement et créez une variable système nommée JAVA_HOME.
      * Définissez la valeur de la variable de sorte qu’elle pointe vers le JDK installé sur votre système. Par exemple c:\program files\java\jdk1.8.0_25

* Créez un dossier nommé AEMForms sur votre lecteur C.
* Localisez AEMQuickStart.Jar et déplacez-le dans le dossier AEMForms.
* Copier le fichier license.properties dans ce dossier AEMForms
* Créez un fichier de commandes appelé &quot;StartAemForms.bat&quot; avec le contenu suivant :
   * java -d64 -Xmx2048M -jar AEM_6.3_Quickstart.jar -gui.Ici AEM_6.3_Quickstart.jar est le nom de mon bocal de démarrage rapide AEM.
   * Vous pouvez renommer votre fichier JAR sous n’importe quel nom, mais assurez-vous que ce nom est reflété dans le fichier de commandes.Enregistrez le fichier de commandes dans le dossier AEMForms.

* Ouvrez une nouvelle invite de commande et accédez à c:\aemforms.

* Exécutez le fichier StartAemForms.bat à partir de l’invite de commande.

* Vous devriez obtenir une petite boîte de dialogue indiquant la progression du démarrage.

* Une fois le démarrage terminé, ouvrez le fichier sling.properties. Cet emplacement se trouve dans c:\AEMForms\crx-quickstart\conf folder.

* Copiez les 2 lignes suivantes vers le bas du fichier
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.*** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.***
* Ces deux propriétés sont requises pour que les services de document fonctionnent.
* Enregistrer le fichier sling.properties

* [Connexion au partage de package](http://localhost:4502/crx/packageshare/login.html)

   * Vous aurez besoin d’AdobeId pour vous connecter au partage de package
   * Recherchez l’Ajoute AEM Forms sur le package correspondant à votre version de AEM Forms et de votre système d’exploitation.
   * Ou [vous pouvez télécharger le module complémentaire de formulaires approprié](https://helpx.adobe.com/fr/aem-forms/kb/aem-forms-releases.html)
   * Une fois le package ajouté installé, les étapes suivantes doivent être suivies :

      * **Assurez-vous que tous les lots sont à principal état. (Sauf pour le lot Signatures AEMFD).**
      * **Il faudrait généralement 5 minutes ou plus pour que tous les lots atteignent l&#39;état principal.**
   * **Une fois que tous les lots sont principaux (sauf le lot AEMFD Signatures), redémarrez votre système pour terminer l’installation d’AEM Forms.**


* Ajoutez le package `sun.util.calendar` à la liste autorisée :

   1. Ouvrez la console Web Felix dans votre [fenêtre de navigateur](http://localhost:4502/system/console/configMgr)
   2. Recherchez et ouvrez la configuration du pare-feu de désérialisation: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
   3. Ajouter `sun.util.calendar` en tant que nouvelle entrée sous `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
   4. Enregistrez les modifications.

Félicitations!!! Vous avez maintenant installé et configuré AEM Forms sur votre système.
Selon vos besoins, vous pouvez configurer [les extensions de Reader](https://helpx.adobe.com/experience-manager/6-3/forms/using/configuring-document-services.html) ou [ PDFG](https://helpx.adobe.com/experience-manager/6-3/forms/using/install-configure-pdf-generator.html) sur votre serveur.
