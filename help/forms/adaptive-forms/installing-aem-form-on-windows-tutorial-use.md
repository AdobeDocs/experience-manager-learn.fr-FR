---
title: Procédure simplifiée d’installation d’AEM Forms sous Windows
description: Procédure rapide et simple d’installation d’AEM Forms sous Windows
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
last-substantial-update: 2020-06-09T00:00:00Z
duration: 158
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 100%

---

# Procédure simplifiée d’installation d’AEM Forms sous Windows

>[!NOTE]
>
>Si vous envisagez d’utiliser AEM Forms, ne double-cliquez jamais sur le fichier jar de démarrage rapide AEM.
>
>Assurez-vous également qu’il n’y a aucun espace dans le chemin du dossier d’installation d’AEM Forms.
>
>Par exemple, n’installez pas AEM Forms dans le dossier c:\jack and jill\AEM Forms.

>[!NOTE]
>
>Si vous installez AEM Forms 6.5, assurez-vous d’avoir installé les redistribuables Microsoft Visual C++ 32 bits suivants.
>
>* Redistribuable Microsoft Visual C++ 2008
>* Redistribuable Microsoft Visual C++ 2010
>* Redistribuable Microsoft Visual C++ 2012
>* Redistribuable Microsoft Visual C++2013 (à partir de la version 6.5)

Nous recommandons de suivre la [documentation officielle](https://helpx.adobe.com/fr/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) pour installer AEM Forms. Pour installer et configurer AEM Forms dans un environnement Windows, procédez comme suit :

* Assurez-vous que le JDK approprié est installé.
   * Pour AEM 6.2, vous avez besoin de : Oracle SE 8 JDK 1.8.x (64 bits)
   * Pour AEM 6.3 et AEM 6.4, vous avez besoin de : Oracle SE 8 JDK 1.8.x (64 bits)
   * Pour AEM 6.5, vous avez besoin de : JDK 8 ou JDK 11
   * Les [exigences JDK officielles](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=fr) sont mentionnées ici.
* Assurez-vous que JAVA_HOME est défini pour pointer vers le JDK que vous avez installé.
   * Pour créer la variable JAVA_HOME dans Windows, procédez comme suit :
      * Cliquez avec le bouton droit de la souris sur Mon ordinateur et sélectionnez Propriétés.
      * Dans l’onglet Avancées, sélectionnez Variables d’environnement et créez une variable système appelée JAVA_HOME.
      * Définissez la valeur de la variable pour qu’elle pointe vers le JDK installé sur votre système. Par exemple, c:\program files\java\jdk1.8.0_25.

* Créez un dossier nommé AEMForms sur votre lecteur C.
* Localisez le fichier AEMQuickStart.Jar et déplacez-le dans le dossier AEMForms.
* Copiez le fichier license.properties dans le dossier AEMForms.
* Créez un fichier batch appelé « StartAemForms.bat » avec le contenu suivant :
   * `java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui`
      * Ici AEM_6.5_Quickstart.jar est le nom de mon fichier jar de démarrage rapide AEM.
   * Vous pouvez renommer votre fichier jar sous n’importe quel nom, mais assurez-vous que le nom apparaît dans le fichier de commandes. Enregistrez le fichier de commandes dans le dossier AEMForms.

* Ouvrez une nouvelle invite de commande et accédez à _c:\aemforms_.

* Exécutez le fichier StartAemForms.bat à partir de l’invite de commande.

* Vous devriez obtenir une petite boîte de dialogue indiquant la progression du démarrage.

* Une fois le démarrage terminé, ouvrez le fichier sling.properties. Il se trouve dans le dossier c:\AEMForms\crx-quickstart\conf.

* Copiez les 2 lignes suivantes vers le bas du fichier.
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;****sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Ces deux propriétés sont requises pour que Document Services fonctionne.
* Enregistrez le fichier sling.properties.
* [Télécharger le package complémentaire de Forms approprié](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=fr)
* Installez le package complémentaire de Forms à l’aide du [gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp).
* Une fois que vous avez installé le package complémentaire, effectuez les étapes suivantes :

   * **Assurez-vous que tous les lots sont actifs. (à l’exception du lot Signatures d’AEMFD).**
   * **5 minutes ou plus sont généralement nécessaires pour que tous les lots obtiennent le statut actif.**

   * **Une fois que tous les lots sont actifs (sauf le lot Signatures d’AEMFD), redémarrez votre système pour terminer l’installation d’AEM Forms.**

## Package sun.util.calendar dans la liste autorisée.

1. Ouvrez la console web Felix dans la [fenêtre du navigateur](http://localhost:4502/system/console/configMgr).
1. Recherchez et ouvrez la configuration du pare-feu de désérialisation : `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
1. Ajoutez `sun.util.calendar` comme nouvelle entrée sous `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`.
1. Enregistrez les modifications.

Félicitations. Vous avez maintenant installé et configuré AEM Forms sur votre système.
Selon vos besoins, vous pouvez configurer [Reader Extensions](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=fr) ou [PDFG](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=fr) sur votre serveur.
