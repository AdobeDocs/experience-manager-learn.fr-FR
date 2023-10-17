---
title: Corriger des erreurs de délai d’expiration lors de la fusion de fichiers de données XML volumineux avec le modèle xdp
description: Fusionner des fichiers XML volumineux avec modèle dans AEM Forms
type: Troubleshooting
role: Admin
level: Intermediate
version: 6.5
feature: Output Service,Forms Service
topic: Administration
kt: 11091
exl-id: 933ec5f6-3e9c-4271-bc35-4ecaf6dbc434
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: ht
source-wordcount: '185'
ht-degree: 100%

---

# Comment activer la création de fichiers PDF en fusionnant des fichiers de données XML volumineux avec des modèles XDP

Lors de la fusion de fichiers de données XML volumineux avec le modèle XDP, l’erreur suivante peut s’afficher dans le fichier journal :

```txt
POST /services/OutputService/GeneratePdfOutput HTTP/1.1] com.adobe.fd.output.internal.exception.OutputServiceException AEM_OUT_001_003:Unexpected Exception: client timeout reached org.omg.CORBA.TIMEOUT: client timeout reached
```

Pour corriger l’erreur ci-dessus, procédez comme suit :

## Modifiez le délai d’expiration Aries.

* Arrêtez le serveur AEM.
* Créez un dossier appelé **install** sous le dossier crx-quickstart de votre installation AEM.
* Créez un fichier appelé **org.apache.aries.transaction.config** avec le contenu suivant
aries.transaction.timeout=&quot;1200&quot;
sous le dossier d’installation. Vous pouvez modifier la valeur du délai d’expiration en fonction de vos besoins. La valeur du délai d’expiration est en secondes.

>[!NOTE]
> Une fois que vous avez créé la configuration org.apache.aries.transaction, vous pouvez modifier les valeurs du délai d’expiration de la transaction à partir de [configMgr](http://localhost:4502/system/console/configMgr) au lieu de modifier le fichier.


## Modifiez les paramètres du fournisseur ORB Jacorb.

* [Ouvrez OSGi ConfigMgr.](http://localhost:4502/system/console/configMgr)
* Recherchez le **fournisseur ORB Jacorb.**
* Ajoutez l’entrée suivante :
jacorb.connection.client.pending_response_timeout=600000
Le paramètre ci-dessus définit le délai d’expiration de réponse en attente (également appelé délai d’attente du client CORBA) sur 600 secondes.
* Enregistrez vos modifications.
