---
title: Capturer des messages d’erreur dans le service de modèle de données de formulaire en tant qu’étape du workflow
description: À partir d’AEM Forms 6.5.1, nous pouvons capturer les messages d’erreur générés lors de l’appel du service de modèle de données de formulaire comme étape du workflow AEM. Workflow.
feature: Workflow
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 8cae155c-c393-4ac3-a412-bf14fc411aac
last-substantial-update: 2020-06-09T00:00:00Z
duration: 59
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 100%

---

# Capturer les messages d’erreur dans l’étape Appeler le service de modèle de données de formulaire

À partir d’AEM Forms 6.5.1, nous avons la possibilité de capturer les messages d’erreur et de spécifier les options de validation. L’étape Appeler le service de modèle de données de formulaire a été améliorée afin de fournir les fonctionnalités suivantes.

* Ajout d’une option pour la validation à 3 niveaux (« OFF », « BASIC » et « FULL ») afin de gérer les exceptions rencontrées lors de l’appel du service de modèle de données de formulaire. Les 3 options indiquent une version de plus en plus stricte de la vérification des exigences spécifiques à la base de données.
  ![validation-levels](assets/validation-level.PNG)

* Ajout d’une case à cocher pour personnaliser l’exécution du workflow. Par conséquent, l’utilisateur ou l’utilisatrice dispose désormais de la possibilité de poursuivre l’exécution du workflow, même si l’étape Appeler le modèle de données de formulaire renvoie des exceptions.

* Stockage d’informations importantes sur les erreurs dues à des exceptions de validation. Trois sélecteurs de variable de type Autocomplete ont été ajoutés pour sélectionner les variables appropriées afin de stocker les variables ErrorCode(String), ErrorMessage(String) et ErrorDetails(JSON). ErrorDetails est toutefois défini sur null si l’exception n’est pas une exception DermisValidationException.
  ![Capture des messages d’erreur.](assets/fdm-error-details.PNG)

Grâce à ces modifications, l’étape Appeler le service de modèle de données de formulaire s’assure que les valeurs d’entrée respectent les contraintes de données spécifiées dans le fichier Swagger. Par exemple, le message d’erreur suivant est généré lorsque les valeurs accountId et balance ne sont pas conformes aux contraintes de données spécifiées dans le fichier Swagger.

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
