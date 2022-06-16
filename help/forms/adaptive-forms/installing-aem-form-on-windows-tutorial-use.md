---
title: Procédure simplifiée d’installation d’AEM Forms sous Windows
description: Procédure rapide et simple d’installation d’AEM Forms sous Windows
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
topic: Development
role: Admin
level: Beginner
exl-id: 80288765-0b51-44a9-95d3-3bdb2da38615
source-git-commit: 5c53919dd038c0992e1fe5dd85053f26c03c5111
workflow-type: tm+mt
source-wordcount: '586'
ht-degree: 9%

---

# Procédure simplifiée d’installation d’AEM Forms sous Windows

>[!NOTE]
>
>Si vous envisagez d’utiliser AEM Forms, ne double-cliquez jamais sur le fichier AEM Quick Start jar.
>
>Assurez-vous également qu’il n’y a aucun espace dans le chemin du dossier d’installation d’AEM Forms.
>
>Par exemple, n’installez pas AEM Forms dans c:\jack and jill\AEM Forms folder

>[!NOTE]
>
>Si vous installez AEM Forms 6.5, assurez-vous d’avoir installé les redistribuables Microsoft Visual C++ 32 bits suivants.
>
>* Redistribuable Microsoft Visual C++ 2008
>* Redistribuable Microsoft Visual C++ 2010
>* Redistribuable Microsoft Visual C++ 2012
>* Redistribuable Microsoft Visual C++ 2013 (à partir de la version 6.5)


Bien que nous recommandions de suivre la [documentation officielle](https://helpx.adobe.com/fr/experience-manager/6-3/forms/using/installing-configuring-aem-forms-osgi.html) pour installer AEM Forms. Pour installer et configurer AEM Forms dans un environnement Windows, procédez comme suit :

* Assurez-vous que le JDK approprié est installé.
   * AEM 6.2 vous devez : Oracle SE 8 JDK 1.8.x (64 bits)
* 
   * AEM 6.3 et AEM 6.4, vous avez besoin des éléments suivants : Oracle SE 8 JDK 1.8.x (64 bits)
* AEM 6.5 vous avez besoin de JDK 8 ou JDK 11
* [Exigences JDK officielles](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/introduction/technical-requirements.html?lang=en) sont répertoriées ici
* Assurez-vous que JAVA_HOME est défini pour pointer vers le JDK que vous avez installé.
   * Pour créer la variable JAVA_HOME dans Windows, procédez comme suit :
      * Cliquez sur Poste de travail avec le bouton droit de la souris et sélectionnez Propriétés.
      * Dans l’onglet Avancé , sélectionnez Variables d’environnement et créez une variable système appelée JAVA_HOME.
      * Définissez la valeur de variable pour qu’elle pointe vers le JDK installé sur votre système. Par exemple, c:\program files\java\jdk1.8.0_25

* Créez un dossier nommé AEM Forms sur votre lecteur C.
* Localisez le fichier AEMuickStart.Jar et déplacez-le dans le dossier AEM Forms.
* Copiez le fichier license.properties dans ce dossier AEM Forms.
* Créez un fichier de commandes appelé &quot;StartAemForms.bat&quot; avec le contenu suivant :
   * java -d64 -Xmx2048M -jar AEM_6.5_Quickstart.jar -gui. Ici AEM_6.5_Quickstart.jar est le nom de mon fichier jar de démarrage rapide AEM.
   * Vous pouvez renommer votre fichier jar sous n’importe quel nom, mais assurez-vous que le nom est reflété dans le fichier de commandes. Enregistrez le fichier de commandes dans le dossier AEM Forms.

* Ouvrez une nouvelle invite de commande et accédez à _c:\aemforms_.

* Exécutez le fichier StartAemForms.bat à partir de l’invite de commande.

* Vous devriez obtenir une petite boîte de dialogue indiquant la progression du démarrage.

* Une fois le démarrage terminé, ouvrez le fichier sling.properties . Elle se trouve dans c:\AEMForms\crx-quickstart\conf folder.

* Copiez les 2 lignes suivantes vers le bas du fichier
   * **sling.bootdelegation.class.com.rsa.jsafe.provider.JsafeJCE=com.rsa.&#42;** **sling.bootdelegation.class.org.bouncycastle.jce.provider.BouncyCastleProvider=org.bouncycastle.&#42;**
* Ces deux propriétés sont requises pour que Document Services fonctionne.
* Enregistrez le fichier sling.properties
* [Téléchargez le module complémentaire de formulaires approprié](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/forms-updates/aem-forms-releases.html?lang=fr)
* Installez le module complémentaire de formulaires à l’aide de [gestionnaire de modules.](http://localhost:4502/crx/packmgr/index.jsp)
* Une fois que vous avez installé le module complémentaire, les étapes suivantes doivent être suivies :

       * **Assurez-vous que tous les lots sont à principal état. (Sauf pour le lot Signatures AEMFD).**
       * **Il faudrait généralement 5 minutes ou plus pour que tous les lots atteignent l’état principal.**
   
   * **Une fois que tous les lots sont principaux (sauf le lot AEMFD Signatures), redémarrez votre système pour terminer l’installation d’AEM Forms.**

## package sun.util.calendar dans la liste autorisée

1. Ouvrez la console web Felix dans votre [fenêtre du navigateur](http://localhost:4502/system/console/configMgr)
2. Recherchez et ouvrez la configuration du pare-feu de désérialisation: `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl`
3. Ajouter `sun.util.calendar` comme nouvelle entrée sous `com.adobe.cq.deserfw.impl.DeserializationFirewallImpl.firewall.deserialization.whitelist.name`
4. Enregistrez les modifications.

Félicitations!!! Vous avez maintenant installé et configuré AEM Forms sur votre système.
Selon vos besoins, vous pouvez configurer des  [Extensions de Reader](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/configuring-reader-extension-osgi.html?lang=en) ou [ PDFG](https://experienceleague.adobe.com/docs/experience-manager-64/forms/install-aem-forms/osgi-installation/install-configure-document-services.html?lang=fr) sur votre serveur
