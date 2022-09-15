---
title: Correction des erreurs de délai d’expiration lors de la fusion de fichiers de données XML volumineux avec le modèle xdp
description: Fusion de fichiers xml volumineux avec modèle dans AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
source-git-commit: 164741ce5ae7d00f904365589438c2eaaf1e05db
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 7%

---

# Comment activer la création de fichiers pdf en fusionnant les fichiers de données xml volumineux avec des modèles xdp

Lors de la fusion de fichiers de données xml volumineux avec le modèle xdp, l’erreur suivante peut s’afficher dans le fichier journal :

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Pour corriger l’erreur ci-dessus, procédez comme suit :

## Modification du délai d’expiration des fichiers

* Arrêter AEM serveur
* Créez un dossier appelé **install** sous le dossier crx-quickstart de votre installation AEM
* Créez un fichier appelé **org.apache.aries.transaction.config** avec le contenu suivant aries.transaction.timeout=&quot;1200&quot; sous le dossier d’installation. Vous pouvez modifier la valeur du délai d’expiration en fonction de vos besoins. La valeur du délai d’expiration est en secondes.

>[!NOTE]
> Une fois que vous avez créé la configuration org.apache.aries.transaction , vous pouvez modifier les valeurs du délai d’expiration de la transaction à partir du [configMgr](http://localhost:4502/system/console/configMgr) au lieu de modifier le fichier


## Modification des paramètres du fournisseur ORB Jacorb

* [Ouvrez OSGi ConfigMgr .](http://localhost:4502/system/console/configMgr)
* Rechercher **Fournisseur ORB Jacorb**
* Ajoutez l’entrée suivante jacorb.connection.client.pending_response_timeout=600000 Le paramètre ci-dessus définit le délai d’attente de réponse (également appelé délai d’attente du client CORBA) sur 600 secondes.
* Enregistrez vos modifications
