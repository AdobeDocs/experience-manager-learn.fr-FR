---
title: Test unitaire
description: Ce didacticiel porte sur l'implémentation d'un test unitaire qui valide le comportement du modèle Sling du composant Byline, créé dans le didacticiel Composant personnalisé.
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4089
mini-toc-levels: 1
thumbnail: 30207.jpg
translation-type: tm+mt
source-git-commit: 836ef9b7f6a9dcb2ac78f5d1320797897931ef8c
workflow-type: tm+mt
source-wordcount: '3544'
ht-degree: 0%

---


# Test unitaire {#unit-testing}

Ce didacticiel porte sur l&#39;implémentation d&#39;un test unitaire qui valide le comportement du modèle Sling du composant Byline, créé dans le [composant personnalisé](./custom-component.md) didacticiel.

## Conditions préalables {#prerequisites}

Consultez le code de ligne de base sur lequel le didacticiel s&#39;appuie :

1. Cloner le référentiel [github.com/adobe/aem-guides-wknd](https://github.com/adobe/aem-guides-wknd).
1. Consultez la branche `unit-testing/start`

```shell
$ git clone git@github.com:adobe/aem-guides-wknd.git ~/code/aem-guides-wknd
$ cd ~/code/aem-guides-wknd
$ git checkout unit-testing/start
```

Vous pouvez toujours vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd/tree/unit-testing/solution) ou vérifier le code localement en passant à la branche `unit-testing/solution`.

## Intention

1. Comprendre les principes de base des tests unitaires.
1. Découvrez les structures et les outils couramment utilisés pour tester AEM code.
1. Comprendre les options permettant de simuler ou de moquer les ressources AEM lors de la rédaction de tests unitaires.

## Arrière-plan {#unit-testing-background}

Dans ce didacticiel, nous allons explorer comment écrire [Tests d&#39;unité](https://en.wikipedia.org/wiki/Unit_testing) pour le [modèle Sling](https://sling.apache.org/documentation/bundles/models.html) de notre composant Byline (créé dans le [Création d&#39;un composant d&#39;AEM personnalisé](custom-component.md)). Les tests unitaires sont des tests de génération écrits en Java qui vérifient le comportement attendu du code Java. Chaque unité de test est généralement petite et valide la sortie d&#39;une méthode (ou d&#39;unités de travail) par rapport aux résultats attendus.

Nous utiliserons AEM pratiques exemplaires et utiliserons :

* [JUnit 5](https://junit.org/junit5/)
* [Cadre de test de Mockito](https://site.mockito.org/)
* [wcm.io Test Framework](https://wcm.io/testing/)  (qui s&#39;appuie sur  [Apache Sling Mocks](https://sling.apache.org/documentation/development/sling-mock.html))

>[!VIDEO](https://video.tv.adobe.com/v/30207/?quality=12&learn=on)

## Gestion des tests unitaires et de l’Adobe Cloud Manager {#unit-testing-and-adobe-cloud-manager}

[Adobe Cloud ](https://docs.adobe.com/content/help/fr-FR/experience-manager-cloud-manager/using/introduction-to-cloud-manager.html) Manager intègre l’exécution de tests unitaires et la couverture de  [code ](https://docs.adobe.com/content/help/en/experience-manager-cloud-manager/using/how-to-use/understand-your-test-results.html#code-quality-testing) dans son pipeline CI/CD afin d’encourager et de promouvoir les meilleures pratiques de l’AEM de test unitaire.

Bien que le code de test d’unité soit une bonne pratique pour toute base de code, il est important, lors de l’utilisation de Cloud Manager, de tirer parti de ses fonctionnalités de test de qualité de code et de rapports en fournissant des tests d’unité à exécuter par Cloud Manager.

## Inspect : dépendances du test Maven {#inspect-the-test-maven-dependencies}

La première étape est d&#39;inspecter les dépendances de Maven pour prendre en charge l&#39;écriture et l&#39;exécution des tests. Quatre dépendances sont requises :

1. JUnit5
1. Cadre de test de Mockito
1. Apache Sling Mocks
1. Cadre de test de AEM Mocks (par io.wcm)

Les dépendances de test **JUnit5**, **Mockito** et **AEM Mocks** sont automatiquement ajoutées au projet lors de la configuration à l&#39;aide de l&#39;archétype [AEM Maven](project-setup.md).

1. Pour vue de ces dépendances, ouvrez le POM du réacteur parent à l&#39;adresse **aem-guides-wknd/pom.xml**, accédez à `<dependencies>..</dependencies>` et assurez-vous que les dépendances suivantes sont définies :

   ```xml
   <dependencies>
       ...
       <!-- Testing -->
       <dependency>
           <groupId>org.junit</groupId>
           <artifactId>junit-bom</artifactId>
           <version>5.5.2</version>
           <type>pom</type>
           <scope>import</scope>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-simple</artifactId>
           <version>1.7.25</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-core</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>org.mockito</groupId>
           <artifactId>mockito-junit-jupiter</artifactId>
           <version>2.25.1</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>junit-addons</groupId>
           <artifactId>junit-addons</artifactId>
           <version>1.4</version>
           <scope>test</scope>
       </dependency>
       <dependency>
           <groupId>io.wcm</groupId>
           <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
           <!-- Prefer the latest version of AEM Mock Junit5 dependency -->
           <version>2.5.2</version>
           <scope>test</scope>
       </dependency>
       ...
   </dependencies>
   ```

1. Ouvrez **aem-guides-wknd/core/pom.xml** et vue que les dépendances de test correspondantes sont disponibles :

   ```xml
   ...
   <dependency>
       <groupId>org.junit.jupiter</groupId>
       <artifactId>junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-core</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>org.mockito</groupId>
       <artifactId>mockito-junit-jupiter</artifactId>
       <scope>test</scope>
   </dependency>
   <dependency>
       <groupId>junit-addons</groupId>
       <artifactId>junit-addons</artifactId>
   </dependency>
   <dependency>
       <groupId>io.wcm</groupId>
       <artifactId>io.wcm.testing.aem-mock.junit5</artifactId>
   </dependency>
   ...
   ```

   Un dossier source parallèle dans le projet **core** contiendra les tests unitaires et les fichiers de test pris en charge. Ce dossier **test** permet de séparer les classes de test du code source, mais permet aux tests d&#39;agir comme s&#39;ils vivaient dans les mêmes packages que le code source.

## Création du test JUnit {#creating-the-junit-test}

Les tests unitaires font généralement correspondre 1 à 1 avec les classes Java. Dans ce chapitre, nous allons écrire un test JUnit pour le **BylineImpl.java**, qui est le modèle Sling qui prend en charge le composant Byline.

![explorateur de packages de tests unitaires](assets/unit-testing/core-src-test-folder.png)

*Emplacement de stockage des tests unitaires.*

1. Pour ce faire dans Eclipse, cliquez avec le bouton droit sur la classe Java à tester, puis sélectionnez **Nouveau > Autre > Java > JUnit > JUnit Test Case**.

   ![Cliquez avec le bouton droit de la souris sur BylineImpl.java pour créer un test unitaire.](assets/unit-testing/junit-test-case-1.png)

1. Dans le premier écran de l’assistant, validez les éléments suivants :

   * Le type de test JUnit est **Nouveau test Jupiter JUnit** car il s’agit des dépendances JUnit Maven configurées dans nos **pom.xml**.
   * Le **package** est le package java de la classe en cours de test (`BylineImpl.java`)
   * Le dossier Source pointe vers le projet **core** (`aem-guides-wknd.core/src/test/java`) qui indique à Eclipse où sont stockés les fichiers de test unitaires.
   * Le stub de méthode `setUp()` sera créé manuellement ; nous verrons comment cela sera utilisé plus tard.
   * Et la classe sous test est `BylineImpl.java`, car il s&#39;agit de la classe Java que nous voulons tester.

   ![étape 2 de l&#39;assistant de test unitaire](assets/unit-testing/junit-wizard-testcase.png)

   *Assistant JUnit Test Case - étape 2*

1. Cliquez sur le bouton **Suivant** au bas de l’assistant.

   Cette étape suivante permet de générer automatiquement des méthodes de test. En règle générale, chaque méthode publique de la classe Java comporte au moins une méthode de test correspondante, ce qui valide son comportement. Souvent, un test unitaire comporte plusieurs méthodes de test qui testent une méthode publique unique, chacune représentant un ensemble d&#39;entrées ou d&#39;états différents.

   Dans l’assistant, sélectionnez toutes les méthodes sous `BylineImpl`, à l’exception de `init()` qui est une méthode utilisée en interne par le modèle Sling (via `@PostConstruct`). Nous allons tester efficacement `init()` en testant toutes les autres méthodes, car les autres méthodes reposent sur `init()` une exécution réussie.

   De nouvelles méthodes de test peuvent être ajoutées à tout moment à la classe de test JUnit. Cette page de l&#39;Assistant est simplement pour des raisons pratiques.

   ![étape 3 de l&#39;assistant de test unitaire](assets/unit-testing/junit-test-case-3.png)

   *Assistant JUnit Test Case (suite)*

1. Cliquez sur le bouton Terminer au bas de l’assistant pour générer le fichier de test JUnit5.
1. Vérifiez que le fichier de test JUnit5 a été créé dans la structure de package correspondante sur **aem-guides-wknd.core** > **/src/test/java** en tant que fichier nommé `BylineImplTest.java`.

## Vérification de BylineImplTest.java {#reviewing-bylineimpltest-java}

Notre fichier de test comporte plusieurs méthodes générées automatiquement. A ce stade, ce fichier de test JUnit n&#39;a rien d&#39;AEM spécifique.

La première méthode est `public void setUp() { .. }` qui est annotée avec `@BeforeEach`.

L&#39;annotation `@BeforeEach` est une annotation JUnit qui indique au test JUnit en cours d&#39;exécution d&#39;exécuter cette méthode avant d&#39;exécuter chaque méthode de test de cette classe.

Les méthodes suivantes sont les méthodes de test elles-mêmes et sont marquées comme telles avec l&#39;annotation `@Test`. Notez que par défaut, tous nos tests sont définis pour échouer.

Lorsque cette classe de test JUnit (également appelée Cas de test JUnit) est exécutée, chaque méthode marquée par le `@Test` s&#39;exécute comme un test qui peut réussir ou échouer.

![BylineImplTest généré](assets/unit-testing/bylineimpltest-new.png)

*`core/src/test/java/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.java`*

1. Exécutez la boîte de test JUnit en cliquant avec le bouton droit sur le nom de la classe et **Exécuter en tant que > Test JUnit**.

   ![Exécuter comme test junit](assets/unit-testing/run-as-junit-test.png)

   *Cliquez avec le bouton droit sur BylineImplTests.java > Exécuter en tant que > Test JUnit.*

1. Comme prévu, tous les tests échouent.

   ![échec des essais](assets/unit-testing/all-tests-fail.png)

   *Vue JUnit sur Eclipse > Fenêtre > Afficher la Vue > Java > JUnit*

## Révision de BylineImpl.java {#reviewing-bylineimpl-java}

Lors de la rédaction de tests unitaires, il existe deux approches Principales :

* [TDD ou Test Driven Development](https://en.wikipedia.org/wiki/Test-driven_development), qui implique la rédaction progressive des tests unitaires, immédiatement avant l&#39;élaboration de la mise en oeuvre ; écrivez un test, écrivez la mise en oeuvre pour réussir le test.
* Développement de la mise en oeuvre d&#39;abord, ce qui implique de développer d&#39;abord le code de travail, puis d&#39;écrire des tests qui valident ledit code.

Dans ce didacticiel, la dernière approche est utilisée (puisque nous avons déjà créé un **BylineImpl.java** fonctionnel dans un chapitre précédent). C&#39;est pourquoi nous devons examiner et comprendre le comportement de ses méthodes publiques, mais aussi certains détails de sa mise en oeuvre. Cela peut sembler contraire, puisqu&#39;un bon test ne devrait se préoccuper que des intrants et des extrants, cependant, lorsqu&#39;on travaille en AEM, il y a une variété de considérations d&#39;implémentation qui doivent être comprises pour construire les essais en cours.

Dans le contexte de l&#39;AEM, la TDD exige un niveau d&#39;expertise et est mieux adoptée par AEM développeurs compétents en développement de l&#39;AEM et des tests unitaires du code .

>[!VIDEO](https://video.tv.adobe.com/v/30208/?quality=12&learn=on)

## Configuration du contexte de test AEM {#setting-up-aem-test-context}

La plupart du code écrit pour l’AEM repose sur les API JCR, Sling ou AEM, ce qui nécessite à son tour le contexte d’un  en cours d’exécution pour s’exécuter correctement.

Les tests unitaires étant exécutés lors de la création, en dehors du contexte d’une instance AEM en cours d’exécution, il n’existe aucune ressource de ce type. Pour faciliter cette tâche, l&#39;AEM Mocks](https://wcm.io/testing/aem-mock/usage.html) de [wcm.io crée un contexte fictif qui permet à ces API d&#39;agir principalement comme si elles s&#39;exécutaient en AEM.

1. Créez un contexte AEM à l&#39;aide de **wcm.io&#39;s** `AemContext` dans **BylineImplTest.java** en l&#39;ajoutant en tant qu&#39;extension JUnit décorée de `@ExtendWith` au fichier **BylineImplTest.java**. L&#39;extension prend en charge toutes les tâches d&#39;initialisation et de nettoyage requises. Créez une variable de classe pour `AemContext` qui peut être utilisée pour toutes les méthodes de test.

   ```java
   import org.junit.jupiter.api.extension.ExtendWith;
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   ...
   
   @ExtendWith(AemContextExtension.class)
   class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   ```

   Cette variable, `ctx`, expose un contexte AEM fictif qui fournit un certain nombre d’abstractions AEM et Sling :

   * Le modèle BylineImpl Sling sera enregistré dans ce contexte
   * Les structures de contenu JCR fictives sont créées dans ce contexte.
   * Les services OSGi personnalisés peuvent être enregistrés dans ce contexte
   * Fournit une variété d’objets fictifs et d’assistants courants requis tels que des objets SlingHttpServletRequest, une variété de services Sling et OSGi AEM tels que ModelFactory, PageManager, Page, Template, ComponentManager, Component, TagManager, TagManager, etc.
      * *Notez que toutes les méthodes de ces objets ne sont pas implémentées !*
   * Et [beaucoup plus](https://wcm.io/testing/aem-mock/usage.html) !

   L&#39;objet **`ctx`** servira de point d&#39;entrée pour la majeure partie de notre simulacre de contexte.

1. Dans la méthode `setUp(..)`, exécutée avant chaque méthode `@Test`, définissez un état de test fictif commun :

   ```java
   @BeforeEach
   public void setUp() throws Exception {
       ctx.addModelsForClasses(BylineImpl.class);
       ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   }
   ```

   * **`addModelsForClasses`** enregistre le modèle Sling à tester, dans le contexte AEM fictif, afin qu’il puisse être instancié dans les  `@Test` méthodes.
   * **`load().json`** charge les structures de ressources dans le contexte fictif, ce qui permet au code d&#39;interagir avec ces ressources comme si elles étaient fournies par un référentiel réel. Les définitions de ressources dans le fichier **`BylineImplTest.json`** sont chargées dans le contexte simulé du JCR sous **/content**.
   * **`BylineImplTest.json`** n&#39;existe pas encore, existe, alors créons-le et définissons les structures de ressources JCR nécessaires pour le test.

1. Les fichiers JSON qui représentent les structures de ressources fictives sont stockés sous **core/src/test/resources** après le même cheminement de package que le fichier de test Java JUnit.

   Créez un fichier JSON à l’adresse **core/test/resources/com/adobe/aem/guides/wknd/core/models/impl** nommé **BylineImplTest.json** avec le contenu suivant :

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   ![BylineImplTest.json](assets/unit-testing/bylineimpltest-json.png)

   Ce fichier JSON définit une définition de ressource fictive pour le test d’unité du composant Byline. À ce stade, le JSON dispose du jeu minimum de propriétés nécessaires pour représenter une ressource de contenu de composant Byline, les `jcr:primaryType` et `sling:resourceType`.

   En règle générale, lorsque vous travaillez avec des tests unitaires, vous devez créer l’ensemble minimal de contenus fictifs, de contexte et de code requis pour satisfaire à chaque test. Evitez la tentation de créer un contexte fictif complet avant d&#39;écrire les tests, car cela donne souvent des artefacts inutiles.

   Maintenant que **BylineImplTest.json** existe, lorsque `ctx.json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content")` est exécuté, les définitions de ressources fictives sont chargées dans le contexte à l&#39;emplacement **/content.**

## Test de getName() {#testing-get-name}

Maintenant que nous disposons d&#39;une configuration de contexte fictive de base, écrivons notre premier test pour **getName()** de BylineImpl. Ce test doit s&#39;assurer que la méthode **getName()** renvoie le nom écrit correct stocké à la propriété &quot;**name&quot;** de la ressource.

1. Mettez à jour la méthode **testGetName**() dans **BylineImplTest.java** comme suit :

   ```java
   import com.adobe.aem.guides.wknd.core.components.Byline;
   import static org.junit.jupiter.api.Assertions.assertEquals;
   ...
   @Test
   public void testGetName() {
       final String expected = "Jane Doe";
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       String actual = byline.getName();
   
       assertEquals(expected, actual);
   }
   ```

   * **`String expected`** définit la valeur attendue. Nous allons définir cette valeur sur &quot;**Jane Done**&quot;.
   * **`ctx.currentResource`** définit le contexte de la ressource fictive pour évaluer le code par rapport à, de sorte qu&#39;elle soit définie sur  **/content/** bylineas où la ressource de contenu fictive est chargée.
   * **`Byline byline`** instancie le modèle Sling de signature en l’adaptant à partir de l’objet Request simulé.
   * **`String actual`** invoque la méthode que nous testons,  `getName()`sur l’objet Modèle Sling de signature.
   * **`assertEquals`** affirme que la valeur attendue correspond à la valeur renvoyée par l’objet de modèle Sling de signature. Si ces valeurs ne sont pas égales, le test échoue.

1. Exécutez le test... et échoue avec un `NullPointerException`.

   Notez que ce test n’échoue PAS car nous n’avons jamais défini de propriété `name` dans le simulateur JSON, qui provoquera l’échec du test, mais l’exécution du test n’est pas arrivée à ce point ! Ce test échoue en raison d&#39;un `NullPointerException` sur l&#39;objet de ligne jaune lui-même.

1. Dans la vidéo [Revue de BylineImpl.java](#reviewing-bylineimpl-java) ci-dessus, nous discutons de la façon dont `@PostConstruct init()` renvoie une exception qui empêche le modèle Sling d&#39;instancier, et c&#39;est ce qui se passe ici.

   ```java
   @PostConstruct
   private void init() {
       image = modelFactory.getModelFromWrappedRequest(request, request.getResource(), Image.class);
   }
   ```

   Il s&#39;avère que bien que le service OSGi ModelFactory soit fourni via `AemContext` (par le contexte Apache Sling), toutes les méthodes ne sont pas implémentées, y compris `getModelFromWrappedRequest(...)` qui est appelé dans la méthode `init()` de BylineImpl. Ceci provoque l’échec de [AbstractMethodError](https://docs.oracle.com/javase/8/docs/api/java/lang/AbstractMethodError.html), ce qui entraîne l’échec de `init()` dans le terme, et l’adaptation résultante de `ctx.request().adaptTo(Byline.class)` est un objet null.

   Puisque les moquettes fournies ne peuvent pas s&#39;adapter à notre code, nous devons mettre en oeuvre le contexte fictif nous-mêmes Pour cela, nous pouvons utiliser Mockito pour créer un objet ModelFactory fictif, qui renvoie un objet Image fictif lorsque `getModelFromWrappedRequest(...)` est appelé dessus.

   Puisque pour pouvoir instancier le modèle Byline Sling, ce contexte fictif doit être en place, nous pouvons l&#39;ajouter à la méthode `@Before setUp()`. Nous devons également ajouter `MockitoExtension.class` à l&#39;annotation `@ExtendWith` au-dessus de la classe **BylineImplTest**.

   ```java
   package com.adobe.aem.guides.wknd.core.models.impl;
   
   import org.mockito.junit.jupiter.MockitoExtension;
   import org.mockito.Mock;
   
   import com.adobe.aem.guides.wknd.core.models.Byline;
   import com.adobe.cq.wcm.core.components.models.Image;
   
   import io.wcm.testing.mock.aem.junit5.AemContext;
   import io.wcm.testing.mock.aem.junit5.AemContextExtension;
   
   import org.apache.sling.models.factory.ModelFactory;
   import org.junit.jupiter.api.BeforeEach;
   import org.junit.jupiter.api.Test;
   import org.junit.jupiter.api.extension.ExtendWith;
   
   import static org.junit.jupiter.api.Assertions.assertEquals;
   import static org.junit.jupiter.api.Assertions.fail;
   import static org.mockito.Mockito.*;
   import org.apache.sling.api.resource.Resource;
   
   @ExtendWith({ AemContextExtension.class, MockitoExtension.class })
   public class BylineImplTest {
   
       private final AemContext ctx = new AemContext();
   
       @Mock
       private Image image;
   
       @Mock
       private ModelFactory modelFactory;
   
       @BeforeEach
       public void setUp() throws Exception {
           ctx.addModelsForClasses(BylineImpl.class);
   
           ctx.load().json("/com/adobe/aem/guides/wknd/core/models/impl/BylineImplTest.json", "/content");
   
           lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()), any(Resource.class), eq(Image.class)))
                   .thenReturn(image);
   
           ctx.registerService(ModelFactory.class, modelFactory, org.osgi.framework.Constants.SERVICE_RANKING,
                   Integer.MAX_VALUE);
       }
   
       @Test
       void testGetName() { ...
   }
   ```

   * **`@ExtendWith({AemContextExtension.class, MockitoExtension.class})`** marque la classe de cas de test à exécuter avec l&#39; [extension JUnit Jupiter de ](https://www.javadoc.io/page/org.mockito/mockito-junit-jupiter/latest/org/mockito/junit/jupiter/MockitoExtension.html) Mockito qui permet l&#39;utilisation des annotations @Mock pour définir des objets fictifs au niveau de la classe.
   * **`@Mock private Image`** crée un objet fictif de type  `com.adobe.cq.wcm.core.components.models.Image`. Notez que cette variable est définie au niveau de la classe afin que, si nécessaire, les méthodes `@Test` puissent modifier son comportement en fonction des besoins.
   * **`@Mock private ModelFactory`** crée un objet fictif de type ModelFactory. Notez qu&#39;il s&#39;agit d&#39;une pure imposture de Mockito et qu&#39;aucune méthode n&#39;y est appliquée. Notez que cette variable est définie au niveau de la classe afin que, si nécessaire, les méthodes `@Test`puissent modifier son comportement en fonction des besoins.
   * **`when(modelFactory.getModelFromWrappedRequest(..)`** enregistre le comportement fictif pour le moment  `getModelFromWrappedRequest(..)` d&#39;appel sur l&#39;objet modelFactory. Le résultat défini dans `thenReturn (..)` est de renvoyer l’objet image simulé. Notez que ce comportement n’est invoqué que lorsque : le premier paramètre est égal à l&#39;objet de requête de `ctx`, le deuxième paramètre est tout objet Resource et le troisième paramètre doit être la classe Image des composants principaux. Nous acceptons toute ressource car, tout au long de nos tests, nous allons définir `ctx.currentResource(...)` sur diverses ressources fictives définies dans le **BylineImplTest.json**. Notez que nous ajoutons la rigueur **clémence()** parce que nous voudrons plus tard remplacer ce comportement de ModelFactory.
   * **`ctx.registerService(..)`.** enregistre l’objet modelFactory dans AemContext, avec le meilleur classement de service. Cela est nécessaire, car le ModelFactory utilisé dans le champ `init()` de BylineImpl est injecté via le champ `@OSGiService ModelFactory model`. Pour qu&#39;AemContext puisse injecter **notre** objet simulé, qui gère les appels à `getModelFromWrappedRequest(..)`, nous devons l&#39;enregistrer comme le service de rang le plus élevé de ce type (ModelFactory).

1. Réexécutez le test, et une fois de plus il échoue, mais cette fois le message est clair pourquoi il a échoué.

   ![assertion d&#39;échec du nom du test](assets/unit-testing/testgetname-failure-assertion.png)

   *échec de testGetName() en raison d&#39;une assertion*

   Nous recevons une erreur **AssertionError**, ce qui signifie que la condition d&#39;affirmation dans le test a échoué, et elle nous indique que la valeur **attendue est &quot;Jane Doe&quot;** mais que la valeur **réelle est nulle**. Cela est logique, car la propriété &quot;**name&quot;** n&#39;a pas été ajoutée à la définition de ressource mock **/content/byline** dans **BylineImplTest.json**, alors ajoutons-la :

1. Mettre à jour **BylineImplTest.json** pour définir `"name": "Jane Doe".`

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

1. Réexécutez le test et **`testGetName()`** réussit maintenant !

## Test de getOccupations() {#testing-get-occupations}

Très bien ! Notre premier test a réussi ! Passons à l&#39;étape suivante et testons `getOccupations()`. Puisque l&#39;initialisation du contexte fictif a été effectuée dans la méthode `@Before setUp()`, elle sera disponible pour toutes les méthodes `@Test` dans cette affaire de test, y compris `getOccupations()`.

Rappelez-vous que cette méthode doit renvoyer une liste triée par ordre alphabétique des professions (décroissantes) stockées dans la propriété des professions.

1. Mettez à jour **`testGetOccupations()`** comme suit :

   ```java
   import java.util.List;
   import com.google.common.collect.ImmutableList;
   ...
   @Test
   public void testGetOccupations() {
       List<String> expected = new ImmutableList.Builder<String>()
                               .add("Blogger")
                               .add("Photographer")
                               .add("YouTuber")
                               .build();
   
       ctx.currentResource("/content/byline");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   * **`List<String> expected`** définissez le résultat attendu.
   * **`ctx.currentResource`** définit la ressource actuelle pour évaluer le contexte par rapport à la définition de la ressource fictive dans /content/byline. Ceci garantit que le **BylineImpl.java** s&#39;exécute dans le contexte de notre fausse ressource.
   * **`ctx.request().adaptTo(Byline.class)`** instancie le modèle Sling de signature en l’adaptant à partir de l’objet Request simulé.
   * **`byline.getOccupations()`** invoque la méthode que nous testons,  `getOccupations()`sur l’objet Modèle Sling de signature.
   * **`assertEquals(expected, actual)`** affirme que la liste attendue est la même que la liste réelle.

1. N&#39;oubliez pas que, tout comme **`getName()`** ci-dessus, le **BylineImplTest.json** ne définit pas les professions, de sorte que ce test échouera si nous l&#39;exécutons, puisque `byline.getOccupations()` retournera une liste vide.

   Mettez à jour **BylineImplTest.json** pour inclure une liste de professions, et elles seront définies dans un ordre non alphabétique pour s&#39;assurer que nos tests valident que les professions sont triées par **`getOccupations()`**.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       }
   }
   ```

1. Exécutez le test, et encore une fois nous réussissons ! On dirait que les emplois triés fonctionnent !

   ![Get Occupations pass](assets/unit-testing/testgetoccupations-success.png)

   *testGetOccupations() réussit*

## Test de isEmpty() {#testing-is-empty}

Dernière méthode à tester **`isEmpty()`**.

Le test `isEmpty()` est intéressant car il nécessite des tests pour diverses conditions. En examinant la méthode **de &lt;a2/> BylineImpl.java**, les conditions suivantes doivent être testées :`isEmpty()`

* Renvoyer true si le nom est vide
* Renvoyer vrai lorsque les emplois sont nuls ou vides
* Renvoie true si l’image est nulle ou n’a pas d’URL src.
* Renvoie false lorsque le nom, les professions et l’image (avec une URL src) sont présents.

Pour ce faire, nous devons créer de nouvelles méthodes de test, chacune testant une condition spécifique ainsi que de nouvelles structures de ressources fictives dans `BylineImplTest.json` pour piloter ces tests.

Notez que cette vérification nous a permis d&#39;ignorer les tests pour déterminer si `getName()`, `getOccupations()` et `getImage()` sont vides puisque le comportement attendu de cet état est testé via `isEmpty()`.

1. Le premier test testera la condition d&#39;un composant tout nouveau, qui n&#39;a aucune propriété définie.

   Ajouter une nouvelle définition de ressource à `BylineImplTest.json`, en lui attribuant le nom sémantique &quot;**empty**&quot;

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       }
   }
   ```

   **`"empty": {...}`** définissez une nouvelle définition de ressource nommée &quot;vide&quot; qui ne comporte qu’un  `jcr:primaryType` et  `sling:resourceType`.

   Souvenez-vous que nous chargeons `BylineImplTest.json` dans `ctx` avant l&#39;exécution de chaque méthode de test dans `@setUp`. Cette nouvelle définition de ressource est donc immédiatement disponible dans les tests à **/content/empty.**

1. Mettez à jour `testIsEmpty()` comme suit, en définissant la ressource actuelle sur la nouvelle définition de ressource fictive &quot;**vide**&quot;.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   Exécutez le test et assurez-vous qu’il réussit.

1. Ensuite, créez un ensemble de méthodes pour vous assurer que si l&#39;un des points de données requis (nom, profession ou image) est vide, `isEmpty()` renvoie true.

   Pour chaque test, une définition de ressource fictive distincte est utilisée, mettez à jour **BylineImplTest.json** avec les définitions de ressources supplémentaires pour **sans nom** et **sans occupation**.

   ```json
   {
       "byline": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe",
       "occupations": ["Photographer", "Blogger", "YouTuber"]
       },
       "empty": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline"
       },
       "without-name": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "occupations": "[Photographer, Blogger, YouTuber]"
       },
       "without-occupations": {
       "jcr:primaryType": "nt:unstructured",
       "sling:resourceType": "wknd/components/content/byline",
       "name": "Jane Doe"
       }
   }
   ```

   Créez les méthodes de test suivantes pour tester chacun de ces états.

   ```java
   @Test
   public void testIsEmpty() {
       ctx.currentResource("/content/empty");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutName() {
       ctx.currentResource("/content/without-name");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutOccupations() {
       ctx.currentResource("/content/without-occupations");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImage() {
       ctx.currentResource("/content/byline");
   
       lenient().when(modelFactory.getModelFromWrappedRequest(eq(ctx.request()),
           any(Resource.class),
           eq(Image.class))).thenReturn(null);
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   
   @Test
   public void testIsEmpty_WithoutImageSrc() {
       ctx.currentResource("/content/byline");
   
       when(image.getSrc()).thenReturn("");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   **`testIsEmpty()`** teste la définition vide de la ressource fictive et affirme que  `isEmpty()` c&#39;est vrai.

   **`testIsEmpty_WithoutName()`** il s&#39;agit d&#39;un simulacre de définition des ressources qui a des emplois mais pas de nom.

   **`testIsEmpty_WithoutOccupations()`** il s&#39;agit d&#39;un simulacre de définition des ressources qui a un nom mais qui n&#39;a pas de profession.

   **`testIsEmpty_WithoutImage()`** teste une définition de ressource fictive avec un nom et des professions, mais définit la fausse image pour qu&#39;elle retourne à zéro. Notez que nous voulons remplacer le comportement `modelFactory.getModelFromWrappedRequest(..)`défini dans `setUp()` pour nous assurer que l&#39;objet Image renvoyé par cet appel est nul. La fonction des stubs Mockito est stricte et ne veut pas de code dupliqué. Par conséquent, nous avons défini la simulation avec les paramètres **`lenient`** pour noter explicitement que nous remplaçons le comportement dans la méthode `setUp()`.

   **`testIsEmpty_WithoutImageSrc()`** teste une définition de ressource fictive avec un nom et des professions, mais définit l’image fictive pour qu’elle renvoie une chaîne vide lorsqu’ `getSrc()` elle est appelée.

1. Enfin, écrivez un test pour vous assurer que **isEmpty()** renvoie false lorsque le composant est correctement configuré. Pour cette condition, nous pouvons réutiliser **/content/byline** qui représente un composant Byline entièrement configuré.

   ```java
   @Test
   public void testIsNotEmpty() {
   ctx.currentResource("/content/byline");
   when(image.getSrc()).thenReturn("/content/bio.png");
   
   Byline byline = ctx.request().adaptTo(Byline.class);
   
   assertFalse(byline.isEmpty());
   }
   ```

## Couverture du code {#code-coverage}

La couverture du code est le volume de code source couvert par les tests unitaires. Les IDE modernes fournissent des outils qui vérifient automatiquement le code source exécuté au cours des tests unitaires. Bien que la couverture du code en soi ne soit pas un indicateur de la qualité du code, il est utile de comprendre s&#39;il existe des zones importantes du code source qui ne sont pas testées par des tests unitaires.

1. Dans l&#39;Explorateur de projets d&#39;Eclipse, cliquez avec le bouton droit sur **BylineImplTest.java** et sélectionnez **Couvrir sous > Test JUnit**.

   Assurez-vous que la vue de résumé de la couverture est ouverte (Fenêtre > Afficher la Vue > Autre > Java > Couverture).

   Cette opération exécute les tests unitaires dans ce fichier et fournit un rapport indiquant la couverture du code. L&#39;exploration de la classe et des méthodes donne des indications plus claires sur les parties du fichier qui sont testées et celles qui ne le sont pas.

   ![exécuter comme couverture du code](assets/unit-testing/bylineimpl-coverage.png)

   *Résumé de la couverture du code*

   Eclipse fournit une vue rapide de la quantité de chaque classe et méthode couverte par le test unitaire. Eclipse even color code les lignes de code :

   * **Code** Greenis exécuté par au moins un test
   * **** YellowIndique une branche qui n&#39;est évaluée par aucun test.
   * **Le code** Redindique qui n&#39;est exécuté par aucun test

1. Dans le rapport de couverture, il a été identifié la branche exécutée lorsque le champ des professions est nul et renvoie une liste vide, n&#39;est jamais évaluée. Cela est indiqué par les lignes 571 et 86, en jaune, indiqué une branche de la variable if/else non exécutée, et la ligne 75, en rouge, indiquant que la ligne de code n&#39;est jamais exécutée.

   ![codage des couleurs de la couverture](assets/unit-testing/coverage-color-coding.png)

1. Il est possible de remédier à ce problème en ajoutant un test pour `getOccupations()` qui affirme qu&#39;une liste vide est renvoyée lorsqu&#39;il n&#39;y a pas de valeur pour les professions sur la ressource. Ajoutez la nouvelle méthode de test suivante sur **BylineImplTests.java**.

   ```java
   @Test
   public void testGetOccupations_WithoutOccupations() {
       List<String> expected = Collections.emptyList();
   
       ctx.currentResource("/content/empty");
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       List<String> actual = byline.getOccupations();
   
       assertEquals(expected, actual);
   }
   ```

   **`Collections.emptyList();`** définit la valeur attendue sur une liste vide.

   **`ctx.currentResource("/content/empty")`** définit la ressource actuelle sur /content/empty, dont nous savons qu&#39;il n&#39;y a pas de propriété d&#39;occupation définie.

1. Reprise de la couverture En tant que, il rapporte que **BylineImpl.java** est maintenant à 100 % de couverture, mais il y a encore une branche qui n&#39;est pas évaluée dans isEmpty() qui a encore à faire avec les professions. Dans ce cas, les occupations == null sont en cours d&#39;évaluation, cependant les occupations.isEmpty() n&#39;est pas puisqu&#39;il n&#39;y a aucune définition de ressource fictive qui définit `"occupations": []`.

   ![Couverture avec testGetOccupations_WithoutOccupations()](assets/unit-testing/getoccupations-withoutoccupations.png)

   *Couverture avec testGetOccupations_WithoutOccupations()*

1. Ceci peut être facilement résolu en créant une autre méthode de test qui est utilisée une définition de ressource fictive qui définit les occupations sur la matrice vide.

   Ajoutez une nouvelle définition de ressource fictive à **BylineImplTest.json** qui est une copie de **&quot;sans occupation&quot;** et ajoutez une propriété de profession définie sur le tableau vide, et nommez-la **&quot;sans occupation-tableau-vide&quot;**.

   ```json
   "without-occupations-empty-array": {
      "jcr:primaryType": "nt:unstructured",
      "sling:resourceType": "wknd/components/content/byline",
      "name": "Jane Doe",
      "occupations": []
    }
   ```

   Créez une nouvelle méthode **@Test** dans `BylineImplTest.java` qui utilise cette nouvelle ressource fictive, affirme que `isEmpty()` renvoie true.

   ```java
   @Test
   public void testIsEmpty_WithEmptyArrayOfOccupations() {
       ctx.currentResource("/content/without-occupations-empty-array");
   
       Byline byline = ctx.request().adaptTo(Byline.class);
   
       assertTrue(byline.isEmpty());
   }
   ```

   ![Couverture avec testIsEmpty_WithEmptyArrayOfOccupations()](assets/unit-testing/testisempty_withemptyarrayofoccupations.png)

   *Couverture avec testIsEmpty_WithEmptyArrayOfOccupations()*

1. Avec ce dernier ajout, `BylineImpl.java` bénéficie d’une couverture de code à 100 % avec tout ce qu’il a de cheminement conditionnel évalué.

   Les tests valident le comportement attendu de `BylineImpl` sans dépendre d&#39;un ensemble minimal de détails d&#39;implémentation.

## Exécution de tests unitaires dans le cadre de la génération {#running-unit-tests-as-part-of-the-build}

Les tests unitaires sont exécutés pour réussir dans le cadre de la création de l’expert. Cela permet de s’assurer que tous les tests réussissent avant le déploiement d’une application. L&#39;exécution des objectifs Maven, tels que le package ou l&#39;installation, appelle automatiquement et nécessite la réussite de tous les tests unitaires du projet.

```shell
$ mvn package
```

![succès du package mvn](assets/unit-testing/mvn-package-success.png)

```shell
$ mvn package
```

De même, si nous modifions une méthode de test pour échouer, la génération échoue et signale les échecs de test et pourquoi.

![échec du package mvn](assets/unit-testing/mvn-package-fail.png)

## Examiner le code {#review-the-code}

Vue le code terminé sur [GitHub](https://github.com/adobe/aem-guides-wknd) ou passez en revue et déployez le code localement sur la brach Git `unit-testing/solution`.
