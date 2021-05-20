---
title: 'Utilisation de tests automatisés avec AEM Forms adaptatif '
seo-title: 'Utilisation de tests automatisés avec AEM Forms adaptatif '
description: Tests automatisés de Forms adaptatif à l’aide du SDK Calvin
seo-description: Tests automatisés de Forms adaptatif à l’aide du SDK Calvin
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
topic: Développement
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 6%

---


# Utilisation de tests automatisés avec AEM Forms adaptatif {#using-automated-tests-with-aem-adaptive-forms}

Tests automatisés de Forms adaptatif à l’aide du SDK Calvin

Calvin SDK est une API utilitaire pour les développeurs de formulaires adaptatifs pour les tester. Le kit SDK Calvin repose sur la [structure de test Hobbes.js](https://docs.adobe.com/docs/fr/aem/6-3/develop/ref/test-api/index.html). Le kit SDK Calvin est disponible avec AEM Forms 6.3 et versions ultérieures.

Dans ce tutoriel, vous allez créer les éléments suivants :

* Suite de tests
* La suite de tests contient un ou plusieurs cas de test.
* Les cas de test contiennent une ou plusieurs actions

## Prise en main {#getting-started}

[Téléchargez et installez les ressources à l’aide du ](assets/testingadaptiveformsusingcalvinsdk1.zip)gestionnaire de modules. Le module contient des exemples de scripts et plusieurs Forms adaptatifs. Ces Forms adaptatives sont créées à l’aide de la version AEM Forms 6.3. Il est recommandé de créer des formulaires spécifiques à votre version d’AEM Forms si vous testez ce type de formulaire sur AEM Forms 6.4 ou version ultérieure. Les exemples de scripts montrent les différentes API du SDK Calvin disponibles pour tester le Forms adaptatif. Les étapes générales pour tester AEM Forms adaptatif sont les suivantes :

* Accédez au formulaire à tester.
* Définir la valeur du champ
* Envoyer le formulaire adaptatif
* Vérifier les messages d’erreur

Les exemples de scripts dans le package montrent toutes les actions ci-dessus.
Explorons le code de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Le code ci-dessus crée une suite de tests.

* Dans ce cas, le nom de la suite de tests est &quot;`Mortgage Form Test`&quot;.
* Le chemin fourni est le chemin d’accès absolu dans AEM fichier js qui contient la suite de tests.
* Le paramètre register lorsqu’il est défini sur &quot;`true`&quot; rend la suite de tests disponible dans l’interface utilisateur de test.

```javascript
.addTestCase(new hobs.TestCase("Calculate amount to borrow")
        // navigate to the mortgage form  which is to be tested
        .navigateTo("/content/forms/af/cal/mortgageform.html?wcmmode=disabled")
  .asserts.isTrue(function () {
            return calvin.isFormLoaded()
        })
```

>[!NOTE]
>
>Si vous testez cette fonctionnalité sur AEM Forms 6.4 ou version ultérieure, créez un formulaire adaptatif et utilisez-le pour effectuer vos tests. Il n’est pas recommandé d’utiliser le formulaire adaptatif fourni avec le package.

Des cas de test peuvent être ajoutés à la suite de tests à exécuter par rapport à un formulaire adaptatif.

* Pour ajouter un cas de test à la suite de tests, utilisez la méthode `addTestCase` de l’objet TestSuite.
* La méthode `addTestCase` utilise un objet TestCase comme paramètre.
* Pour créer TestCase, utilisez la méthode `hobs.TestCase(..)` .
* Remarque : Le premier paramètre est le nom du cas de test qui apparaîtra dans l’interface utilisateur.
* Une fois que vous avez créé un cas de test, vous pouvez ajouter des actions à votre cas de test.
* Les actions `navigateTo`, `asserts.isTrue` peuvent être ajoutées en tant qu’actions au cas de test.

## Exécution des tests automatisés {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)Ouvrez la suite de testsDéveloppez la suite de tests et exécutez les tests. Si tout s’exécute correctement, la sortie suivante s’affiche.

![calvinsdk](assets/calvinimage.png)

## Testez les exemples de suites de tests {#try-out-the-sample-test-suites}

Dans le cadre de l’exemple de package, il existe trois suites de test supplémentaires. Vous pouvez les essayer en incluant les fichiers appropriés dans le fichier js.txt de la bibliothèque cliente, comme illustré ci-dessous :

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
