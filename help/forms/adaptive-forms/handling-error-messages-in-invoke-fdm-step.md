---
title: Capturer les messages d’erreur dans le service de modèle de données de formulaire en tant qu’étape du processus
seo-title: Capturer les messages d’erreur dans le service de modèle de données de formulaire en tant qu’étape du processus
description: À compter de AEM Forms 6.5.1, nous pouvons désormais capturer les messages d’erreur générés lors de l’utilisation du service de modèle de données de formulaire d’appel comme étape dans AEM Workflow. Workflow.
seo-description: À compter de AEM Forms 6.5.1, nous pouvons désormais capturer les messages d’erreur générés lors de l’utilisation du service de modèle de données de formulaire d’appel comme étape dans AEM Workflow. Workflow.
feature: workflow
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.5.1,6.5.2
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '286'
ht-degree: 0%

---


# Capturer les messages d’erreur dans l’étape de service du modèle de données de formulaire d’appel

Depuis AEM Forms 6.5.1, nous avons désormais la possibilité de capturer des messages d’erreur et de spécifier des options de validation. L’étape Invoke Form Data Model Service a été améliorée pour offrir les fonctionnalités suivantes.

* Fourniture d’une option de validation à trois niveaux (&quot;OFF&quot;, &quot;BASIC&quot; et &quot;FULL&quot;) pour gérer les exceptions survenues lors de l’appel du service de modèle de données de formulaire. Les 3 options indiquent successivement une version plus stricte de la vérification des exigences spécifiques à la base de données.
   ![niveaux de validation](assets/validation-level.PNG)

* Fourniture d’une case à cocher pour personnaliser l’exécution du flux de travaux. Par conséquent, l’utilisateur dispose désormais de la souplesse nécessaire pour procéder à l’exécution du flux de travail, même si l’étape Appeler un modèle de données de formulaire génère des exceptions.

* Stockage d’informations importantes sur les erreurs survenant en raison d’exceptions de validation. Trois sélecteurs de variables de type Fin automatique ont été incorporés pour sélectionner les variables appropriées pour stocker les variables ErrorCode(String), ErrorMessage(String) et ErrorDetails(JSON). ErrorDetails sera toutefois défini sur null si l&#39;exception n&#39;est pas DermisValidationException.
   ![capture de messages d’erreur](assets/fdm-error-details.PNG)

Grâce à ces modifications, l’étape Appeler le service de modèle de données de formulaire s’assure que les valeurs d’entrée respectent les contraintes de données fournies dans le fichier swagger. Par exemple, le message d’erreur suivant est généré lorsque les valeurs accountId et balance ne sont pas conformes aux contraintes de données spécifiées dans le fichier swagger.

```json
{

"errorCode": "AEM-FDM-001-049"

"errorMessage": "Input validations failed during operation execution"

"violations": {

"/accountId": ["numeric instance is greater than the required maximum (maximum: 20, found: 97)"],

"/newAccount/balance": ["instance type (string) does not match any allowed primitive type (allowed: [\"integer\",\"number\"])"]

}

}
```


