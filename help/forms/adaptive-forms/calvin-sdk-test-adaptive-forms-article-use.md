---
title: 'Utilisation de tests automatisés avec AEM Forms adaptatif '
seo-title: 'Utilisation de tests automatisés avec AEM Forms adaptatif '
description: Test automatisé de Forms adaptatif à l’aide du SDK Calvin
seo-description: Test automatisé de Forms adaptatif à l’aide du SDK Calvin
feature: formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
uuid: 3ad4e6d6-d3b1-4e4d-9169-847f74ba06be
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '461'
ht-degree: 5%

---


# Utilisation de tests automatisés avec AEM Forms adaptatif {#using-automated-tests-with-aem-adaptive-forms}

Test automatisé de Forms adaptatif à l’aide du SDK Calvin

Calvin SDK est une API utilitaire pour les développeurs de formulaires adaptatifs pour les tester. Le SDK Calvin est construit sur la structure de test [Hobbes.js](https://docs.adobe.com/docs/fr/aem/6-3/develop/ref/test-api/index.html). Calvin SDK est disponible avec AEM Forms 6.3 et versions ultérieures.

Dans ce didacticiel, vous allez créer les éléments suivants :

* Suite de tests
* La suite de tests contient un ou plusieurs cas de test
* Les cas de test contiennent une ou plusieurs actions

## Prise en main {#getting-started}

[Téléchargez et installez les ressources à l’aide de Package ](assets/testingadaptiveformsusingcalvinsdk1.zip)ManagerLe package contient des exemples de scripts et plusieurs Forms adaptatif.Ces Forms adaptatifs sont créés à l’aide de AEM Forms version 6.3. Il est recommandé de créer de nouveaux formulaires spécifiques à votre version d’AEM Forms si vous testez cette application sur AEM Forms 6.4 ou version ultérieure. Les exemples de scripts montrent différentes API du SDK Calvin disponibles pour tester Forms adaptatif. Les étapes générales pour tester AEM Adaptive Forms sont les suivantes :

* Accédez au formulaire à tester.
* Définir la valeur du champ
* Envoyer le formulaire adaptatif
* Vérifier les messages d&#39;erreur

Les exemples de scripts du package montrent toutes les actions ci-dessus.
Examinons le code de `mortgageForm.js`

```javascript
var mortgageFormTS = new hobs.TestSuite("Mortgage Form Test", {
        path: '/etc/clientlibs/testingAFUsingCalvinSDK/mortgageForm.js',
        register: true
})
```

Le code ci-dessus crée une nouvelle suite de tests.

* Dans ce cas, le nom de TestSuite est &quot; `Mortgage Form Test`&quot;.
* Fourni est le chemin absolu dans AEM fichier js qui contient la suite de tests.
* Le paramètre register lorsqu&#39;il est défini sur &quot; `true`&quot; rend la suite de tests disponible dans l&#39;interface utilisateur de test.

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
>Si vous testez cette fonctionnalité sur AEM Forms 6.4 ou version ultérieure, créez un formulaire adaptatif et utilisez-le pour effectuer vos tests.Il n&#39;est pas recommandé d&#39;utiliser le formulaire adaptatif fourni avec le package.

Les cas de test peuvent être ajoutés à la suite de tests à exécuter sur un formulaire adaptatif.

* Pour ajouter un cas de test à la suite de tests, utilisez la méthode `addTestCase` de l&#39;objet TestSuite.
* La méthode `addTestCase` utilise un objet TestCase comme paramètre.
* Pour créer TestCase, utilisez la méthode `hobs.TestCase(..)`.
* Remarque : Le premier paramètre est le nom du cas de test qui apparaîtra dans l’interface utilisateur.
* Une fois que vous avez créé un cas de test, vous pouvez ajouter des actions à votre cas de test.
* Les actions `navigateTo`, `asserts.isTrue` peuvent être ajoutées en tant qu&#39;actions au cas de test.

## Exécution des tests automatisés {#running-the-automated-tests}

[](http://localhost:4502/libs/granite/testing/hobbes.html)Openthe estsuiteDéveloppez la suite de tests et exécutez les tests. Si tout s’exécute correctement, vous verrez la sortie suivante.

![calvinsdk](assets/calvinimage.png)

## Testez les exemples de suites de tests {#try-out-the-sample-test-suites}

Dans l&#39;exemple de package, trois suites de tests supplémentaires sont disponibles. Vous pouvez les tester en incluant les fichiers appropriés dans le fichier js.txt de la bibliothèque cliente, comme indiqué ci-dessous :

```javascript
#base=.

scriptingTest.js
validationTest.js
prefillTest.js
mortgageForm.js
```
