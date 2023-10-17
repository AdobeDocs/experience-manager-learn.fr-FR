---
title: Utiliser des tests automatisés avec les formulaires adaptatifs d’AEM
description: Tests automatisés des formulaires adaptatifs à l’aide du SDK Calvin
feature: Adaptive Forms
doc-type: article
activity: develop
version: 6.4,6.5
topic: Development
role: Developer
level: Beginner
exl-id: 5a1364f3-e81c-4c92-8972-4fdc24aecab1
last-substantial-update: 2020-09-10T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '443'
ht-degree: 100%

---

# Utiliser des tests automatisés avec les formulaires adaptatifs d’AEM {#using-automated-tests-with-aem-adaptive-forms}

Tests automatisés des formulaires adaptatifs à l’aide du SDK Calvin

Calvin SDK est une API utilitaire pour les développeurs de formulaires adaptatifs pour les tester. Calvin SDK est construit sur la [structure de test Hobbes.js](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr). Le SDK Calvin est disponible avec AEM Forms 6.3 et les versions ultérieures.

Dans ce tutoriel, vous allez créer les éléments suivants :

* Suite de tests
* La suite de tests contient un ou plusieurs cas de test.
* Les cas de test contiennent une ou plusieurs actions.

## Prise en main {#getting-started}

[Téléchargement et installation des ressources à l’aide du gestionnaire de packages](assets/testingadaptiveformsusingcalvinsdk1.zip) Le package contient des exemples de scripts et plusieurs formulaires adaptatifs. Ces formulaires adaptatifs sont créés à l’aide de la version AEM Forms 6.3. Il est recommandé de créer des formulaires spécifiques à votre version d’AEM Forms si vous testez ce type de formulaire sur AEM Forms 6.4 ou une version ultérieure. Les exemples de scripts montrent les différentes API du SDK Calvin disponibles pour tester les formulaires adaptatifs. Les étapes générales pour tester les formulaires adaptatifs d’AEM sont les suivantes :

* accédez au formulaire à tester ;
* définissez la valeur du champ ;
* envoyez le formulaire adaptatif ;
* vérifiez les messages d’erreur.

Les exemples de scripts dans le package montrent toutes les actions ci-dessus.
Explorons le code de `mortgageForm.js`.

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Le code ci-dessus crée une suite de tests.

* Dans ce cas, le nom de la suite de tests est « `Mortgage Form Test` ».
* Le chemin fourni est le chemin d’accès absolu dans AEM vers le fichier js qui contient la suite de tests.
* Lorsque le paramètre de registre est défini sur « `true` », il rend la suite de tests disponible dans l’interface utilisateur de test.

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
>Si vous testez cette fonctionnalité dans AEM Forms 6.4 ou une version ultérieure, créez un formulaire adaptatif et utilisez-le pour effectuer vos tests. Il n’est pas recommandé d’utiliser le formulaire adaptatif fourni avec le package.

Des cas de test peuvent être ajoutés à la suite de tests à exécuter sur un formulaire adaptatif.

* Pour ajouter un cas de test à une suite de tests, utilisez la méthode `addTestCase` de l’objet TestSuite.
* La méthode `addTestCase` prend un objet TestCase comme paramètre.
* Pour créer TestCase, utilisez la méthode `hobs.TestCase(..)`.
* Remarque : le premier paramètre est le nom du cas de test qui s’affiche dans l’interface utilisateur.
* Une fois que vous avez créé un cas de test, vous pouvez ajouter des actions à votre cas de test.
* Les actions telles que `navigateTo` ou `asserts.isTrue` peut être ajoutées en tant qu’actions au cas de test.

## Exécuter les tests automatisés {#running-the-automated-tests}

[Openthetestsuite](http://localhost:4502/libs/granite/testing/hobbes.html) Développez la suite de tests et exécutez les tests. Si tout s’exécute correctement, la sortie suivante s’affiche.

![calvinsdk](assets/calvinimage.png)

## Tester les exemples de suites de tests {#try-out-the-sample-test-suites}

Dans le cadre de l’exemple de package, il existe trois suites de test supplémentaires. Vous pouvez les essayer en incluant les fichiers appropriés dans le fichier js.txt de la bibliothèque cliente, comme illustré ci-dessous :

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
