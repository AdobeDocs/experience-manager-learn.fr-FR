---
title: Installation d’artefacts tiers - non disponibles dans le référentiel Maven public
description: Découvrez comment installer des artefacts tiers qui ne sont *pas disponibles dans le référentiel Maven public* lors de la création et du déploiement d’un projet AEM.
version: 6.5, Cloud Service
feature: OSGI
topic: Development
role: Architect, Developer
level: Intermediate
doc-type: Tutorial
duration: 0
last-substantial-update: 2024-09-13T00:00:00Z
jira: KT-16207
thumbnail: KT-16207.jpeg
source-git-commit: 33415305f6aa183373eaef4bb4978a59325c8a32
workflow-type: tm+mt
source-wordcount: '1569'
ht-degree: 0%

---


# Installation d’artefacts tiers - non disponibles dans le référentiel Maven public

Découvrez comment installer des artefacts tiers *non disponibles dans le référentiel Maven public* lors de la création et du déploiement d’un projet AEM.

Les **artefacts tiers** peuvent être :

- [Groupe OSGi](https://www.osgi.org/resources/architecture/) : un lot OSGi est un fichier d’archive Java™ qui contient des classes Java, des ressources et un manifeste qui décrit le lot et ses dépendances.
- [Java jar](https://docs.oracle.com/javase/tutorial/deployment/jar/basicsindex.html) : fichier d’archive Java™ contenant des classes Java et des ressources.
- [Package](https://experienceleague.adobe.com/en/docs/experience-manager-65/content/sites/administering/contentmanagement/package-manager#what-are-packages) : un package est un fichier zip contenant le contenu du référentiel dans le formulaire de sérialisation du système de fichiers.

## Scénario standard

En règle générale, vous installez le bundle tiers, le package *disponible* dans le référentiel Maven public en tant que dépendance dans le fichier `pom.xml` de votre projet AEM.

Par exemple :

- [AEM WCM Core Components](https://github.com/adobe/aem-core-wcm-components) **bundle** est ajouté en tant que dépendance dans le fichier [ ](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L747-L753) `pom.xml` du projet WKND. Ici, la portée `provided` est utilisée comme le lot AEM composants principaux WCM est fourni par le composant d’exécution AEM. Si le lot n’est pas fourni par le runtime AEM, vous utiliserez la portée `compile` et il s’agit de la portée par défaut.

- [WKND Shared](https://github.com/adobe/aem-guides-wknd-shared) **package** ajouté en tant que dépendance dans le fichier [WKND project&#39;s](https://github.com/adobe/aem-guides-wknd/blob/main/pom.xml#L767-L773) `pom.xml`.



## Scénario rare

Lors de la création et du déploiement d’un projet AEM, vous devrez parfois installer un bundle tiers ou un jar ou un package **qui n’est pas disponible** dans le [ référentiel central Maven](https://mvnrepository.com/) ou le [référentiel public Adobe](https://repo.adobe.com/index.html).

Les raisons peuvent être les suivantes :

- Le lot ou le package est fourni par une équipe interne ou un fournisseur tiers et _n’est pas disponible dans le référentiel Maven public_.

- Le fichier jar Java™ _n’est pas un lot OSGi_ et peut être disponible ou non dans le référentiel Maven public.

- Vous avez besoin d’une fonctionnalité qui n’est pas encore publiée dans la dernière version du package tiers disponible dans le référentiel Maven public. Vous avez décidé d’installer la version locale de VERSION ou SNAPSHOT.

## Prérequis

Pour suivre ce tutoriel, vous devez :

- Configuration de [l’environnement de développement d’AEM local](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/local-development-environment-set-up/overview) ou [Environnement de développement rapide (RDE)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/developing/rde/overview).

- Le [projet AEM WKND](https://github.com/adobe/aem-guides-wknd) _pour ajouter le bundle tiers ou jar ou package_ et vérifier les modifications.

## Configuration

- Configurez l’environnement de développement local ou l’environnement RDE d’AEM 6.X ou AEM as a Cloud Service (AEMCS).

- Cloner et déployer le projet AEM WKND.

  ```
  $ git clone git@github.com:adobe/aem-guides-wknd.git
  $ cd aem-guides-wknd
  $ mvn clean install -PautoInstallPackage 
  ```

  Vérifiez que le rendu des pages du site WKND est correct.

## Installation d’un lot tiers dans un projet AEM{#install-third-party-bundle}

Installez et utilisez une démonstration OSGi [my-example-bundle](./assets/install-third-party-articafcts/my-example-bundle.zip) qui _n’est pas disponible dans le référentiel Maven public_ au projet AEM WKND.

Le service **my-example-bundle** exporte `HelloWorldService` OSGi, sa méthode `sayHello()` renvoie un message `Hello Earth!`.

Pour plus d’informations, reportez-vous au fichier README.md dans le fichier [my-example-bundle.zip](./assets/install-third-party-articafcts/my-example-bundle.zip) .

### Ajouter le lot au module `all`

La première étape consiste à ajouter le `my-example-bundle` au module `all` du projet AEM WKND.

- Téléchargez et extrayez le fichier [my-example-bundle.zip](./assets/install-third-party-articafcts/my-example-bundle.zip).

- Dans le module `all` du projet AEM WKND, créez la structure de répertoires `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install`. Le répertoire `/all/src/main/content` existe, il suffit de créer les répertoires `jcr_root/apps/wknd-vendor-packages/container/install`.

- Copiez le fichier `my-example-bundle-1.0-SNAPSHOT.jar` du répertoire `target` extrait dans le répertoire `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install` ci-dessus.

  ![Groupe tiers dans tous les modules](./assets/install-third-party-articafcts/3rd-party-bundle-all-module.png)

### Utiliser le service du lot

Utilisons le service OSGi `HelloWorldService` de `my-example-bundle` dans le projet AEM WKND.

- Dans le module `core` du projet AEM WKND, créez le `SayHello.java` servlet Sling @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet`.

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  import org.osgi.service.component.annotations.Reference;
  import com.example.services.HelloWorldService;
  
  @Component(service = Servlet.class, property = {
      ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
      ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
          private static final long serialVersionUID = 1L;
  
          // Injecting the HelloWorldService from the `my-example-bundle` bundle
          @Reference
          private HelloWorldService helloWorldService;
  
          @Override
          protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException, IOException {
              // Invoking the HelloWorldService's `sayHello` method
              response.getWriter().write("My-Example-Bundle service says: " + helloWorldService.sayHello());
          }
  }
  ```

- Dans le fichier `pom.xml` racine du projet WKND AEM, ajoutez `my-example-bundle` en tant que dépendance.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install/my-example-bundle-1.0-SNAPSHOT.jar</systemPath>
  </dependency>
  ...
  ```

  Ici :
   - La portée `system` indique que la dépendance ne doit pas être recherchée dans le référentiel Maven public.
   - `systemPath` est le chemin d’accès au fichier `my-example-bundle` dans le module `all` du projet AEM WKND.
   - `${maven.multiModuleProjectDirectory}` est une propriété Maven qui pointe vers le répertoire racine du projet multimodule.

- Dans le fichier `core/pom.xml` du module `core` du projet AEM WKND, ajoutez `my-example-bundle` en tant que dépendance.

  ```xml
  ...
  <!-- My Example Bundle -->
  <dependency>
      <groupId>com.example</groupId>
      <artifactId>my-example-bundle</artifactId>
  </dependency>
  ...
  ```

- Créez et déployez le projet WKND AEM à l’aide de la commande suivante :

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Vérifiez que le servlet `SayHello` fonctionne comme prévu en accédant à l’URL `http://localhost:4502/bin/sayhello` dans le navigateur.

- Validez ci-dessus les modifications apportées au référentiel du projet AEM WKND. Vérifiez ensuite les modifications apportées à l’environnement RDE ou AEM en exécutant le pipeline Cloud Manager.

  ![ Vérifiez le servlet SayHello - Bundle Service](./assets/install-third-party-articafcts/verify-sayhello-servlet-bundle-service.png)

La branche [tutoriel/install-3rd-party-bundle](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-bundle) du projet AEM WKND comporte les modifications ci-dessus à titre de référence.

### Principaux enseignements{#key-learnings-bundle}

Les lots OSGi qui ne sont pas disponibles dans le référentiel Maven public peuvent être installés dans un projet AEM en procédant comme suit :

- Copiez le lot OSGi dans le répertoire `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` du module `all`. Cette étape est nécessaire pour regrouper et déployer le lot vers l’instance AEM.

- Mettez à jour les fichiers `pom.xml` du module racine et principal pour ajouter le lot OSGi en tant que dépendance avec la portée `system` et `systemPath` pointant vers le fichier de lot. Cette étape est nécessaire pour compiler le projet.

## Installation d’un fichier jar tiers dans un projet AEM

Dans cet exemple, `my-example-jar` n’est pas un lot OSGi, mais un fichier Java jar.

Installez et utilisez une démonstration [my-example-jar](./assets/install-third-party-articafcts/my-example-jar.zip) qui _n’est pas disponible dans le référentiel Maven public_ au projet AEM WKND.

**my-example-jar** est un fichier Java jar qui contient une classe `MyHelloWorldService` avec une méthode `sayHello()` qui renvoie un message `Hello World!`.

Pour plus d’informations, reportez-vous au fichier README.md dans le fichier [my-example-jar.zip](./assets/install-third-party-articafcts/my-example-jar.zip) .

### Ajouter le jar au module `all`

La première étape consiste à ajouter le `my-example-jar` au module `all` du projet AEM WKND.

- Téléchargez et extrayez le fichier [my-example-jar.zip](./assets/install-third-party-articafcts/my-example-jar.zip).

- Dans le module `all` du projet AEM WKND, créez la structure de répertoires `all/resource/jar`.

- Copiez le fichier `my-example-jar-1.0-SNAPSHOT.jar` du répertoire `target` extrait dans le répertoire `all/resource/jar` ci-dessus.

  ![3rd-party-jar dans tous les modules](./assets/install-third-party-articafcts/3rd-party-JAR-all-module.png)

### Utiliser le service du jar

Utilisons le `MyHelloWorldService` de `my-example-jar` dans le projet AEM WKND.

- Dans le module `core` du projet AEM WKND, créez le `SayHello.java` servlet Sling @ `core/src/main/java/com/adobe/aem/guides/wknd/core/servlet`.

  ```java
  package com.adobe.aem.guides.wknd.core.servlet;
  
  import java.io.IOException;
  
  import javax.servlet.Servlet;
  import javax.servlet.ServletException;
  
  import org.apache.sling.api.SlingHttpServletRequest;
  import org.apache.sling.api.SlingHttpServletResponse;
  import org.apache.sling.api.servlets.HttpConstants;
  import org.apache.sling.api.servlets.ServletResolverConstants;
  import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
  import org.osgi.service.component.annotations.Component;
  
  import com.my.example.MyHelloWorldService;
  
  @Component(service = Servlet.class, property = {
          ServletResolverConstants.SLING_SERVLET_PATHS + "=/bin/sayhello",
          ServletResolverConstants.SLING_SERVLET_METHODS + "=" + HttpConstants.METHOD_GET
  })
  public class SayHello extends SlingSafeMethodsServlet {
  
      private static final long serialVersionUID = 1L;
  
      @Override
      protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response)
              throws ServletException, IOException {
  
          // Creating an instance of MyHelloWorldService
          MyHelloWorldService myHelloWorldService = new MyHelloWorldService();
  
          // Invoking the MyHelloWorldService's `sayHello` method
          response.getWriter().write("My-Example-JAR service says: " + myHelloWorldService.sayHello());
      }
  }    
  ```

- Dans le fichier `pom.xml` racine du projet WKND AEM, ajoutez `my-example-jar` en tant que dépendance.

  ```xml
  ...
  <!-- My Example JAR -->
  <dependency>
      <groupId>com.my.example</groupId>
      <artifactId>my-example-jar</artifactId>
      <version>1.0-SNAPSHOT</version>
      <scope>system</scope>
      <systemPath>${maven.multiModuleProjectDirectory}/all/resource/jar/my-example-jar-1.0-SNAPSHOT.jar</systemPath>
  </dependency>            
  ...
  ```

  Ici :
   - La portée `system` indique que la dépendance ne doit pas être recherchée dans le référentiel Maven public.
   - `systemPath` est le chemin d’accès au fichier `my-example-jar` dans le module `all` du projet AEM WKND.
   - `${maven.multiModuleProjectDirectory}` est une propriété Maven qui pointe vers le répertoire racine du projet multimodule.

- Dans le fichier `core/pom.xml` du module `core` du projet AEM WKND, effectuez deux modifications :

   - Ajoutez le `my-example-jar` comme dépendance.

     ```xml
     ...
     <!-- My Example JAR -->
     <dependency>
         <groupId>com.my.example</groupId>
         <artifactId>my-example-jar</artifactId>
     </dependency>
     ...
     ```

   - Mettez à jour la configuration `bnd-maven-plugin` pour inclure `my-example-jar` dans le lot OSGi (aem-guides-wknd.core) en cours de création.

     ```xml
     ...
     <plugin>
         <groupId>biz.aQute.bnd</groupId>
         <artifactId>bnd-maven-plugin</artifactId>
         <executions>
             <execution>
                 <id>bnd-process</id>
                 <goals>
                     <goal>bnd-process</goal>
                 </goals>
                 <configuration>
                     <bnd><![CDATA[
                 Import-Package: javax.annotation;version=0.0.0,*
                 <!-- Include the 3rd party jar as inline resource-->
                 -includeresource: \
                 lib/my-example-jar.jar=my-example-jar-1.0-SNAPSHOT.jar;lib:=true
                         ]]></bnd>
                 </configuration>
             </execution>
         </executions>
     </plugin>        
     ...
     ```

- Créez et déployez le projet WKND AEM à l’aide de la commande suivante :

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Vérifiez que le servlet `SayHello` fonctionne comme prévu en accédant à l’URL `http://localhost:4502/bin/sayhello` dans le navigateur.

- Validez ci-dessus les modifications apportées au référentiel du projet AEM WKND. Vérifiez ensuite les modifications apportées à l’environnement RDE ou AEM en exécutant le pipeline Cloud Manager.

  ![ Vérifiez le servlet SayHello - JAR Service](./assets/install-third-party-articafcts/verify-sayhello-servlet-jar-service.png)

La branche [tutorial/install-3rd-party-jar](https://github.com/adobe/aem-guides-wknd/compare/main...tutorial/install-3rd-party-jar) du projet AEM WKND contient les modifications ci-dessus à titre de référence.

Dans les scénarios où le fichier Java jar _est disponible dans le référentiel Maven public, mais n’est PAS un lot OSGi_, vous pouvez suivre les étapes ci-dessus, à l’exception de la portée `system` de `<dependency>` et où les éléments `systemPath` ne sont pas requis.

### Principaux enseignements{#key-learnings-jar}

Les jars Java qui ne sont pas des lots OSGi et qui peuvent être ou non disponibles dans le référentiel Maven public peuvent être installés dans un projet AEM en procédant comme suit :

- Mettez à jour la configuration `bnd-maven-plugin` dans le fichier `pom.xml` du module principal pour inclure le jar Java en tant que ressource intégrée dans le lot OSGi en cours de création.

Les étapes suivantes ne sont nécessaires que si le jar Java n’est pas disponible dans le référentiel Maven public :

- Copiez le jar Java dans le répertoire `resource/jar` du module `all`.

- Mettez à jour les fichiers `pom.xml` du module racine et principal pour ajouter le jar Java en tant que dépendance avec la portée `system` et `systemPath` pointant vers le fichier jar.

## Installation d’un package tiers dans un projet AEM

Installez la version [ACS AEM Commons](https://adobe-consulting-services.github.io/acs-aem-commons/) _SNAPSHOT_ créée localement à partir de la branche principale.

Il s’agit uniquement de démontrer les étapes d’installation d’un package AEM qui n’est pas disponible dans le référentiel Maven public.

Le package ACS AEM Commons est disponible dans le référentiel Maven public. Reportez-vous à la section [Ajout d’ACS AEM Commons à votre projet AEM Maven](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html) pour l’ajouter à votre projet AEM.

### Ajouter le package au module `all`

La première étape consiste à ajouter le package au module `all` du projet AEM WKND.

- Commentez ou supprimez la dépendance de publication ACS AEM Commons du fichier POM. Pour identifier la dépendance, reportez-vous à la section [Ajout d’ACS AEM Commons à votre projet AEM Maven](https://adobe-consulting-services.github.io/acs-aem-commons/pages/maven.html).

- Cloner la branche `master` du [référentiel ACS AEM Commons](https://github.com/Adobe-Consulting-Services/acs-aem-commons) sur votre ordinateur local.

- Créez la version ACS AEM Commons SNAPSHOT à l’aide de la commande suivante :

  ```
  $mvn clean install
  ```

- Le package créé localement est situé @ `all/target`, il existe deux fichiers .zip, l’un se terminant par `-cloud` est destiné à AEM as a Cloud Service et l’autre à AEM 6.X.

- Dans le module `all` du projet AEM WKND, créez la structure de répertoires `all/src/main/content/jcr_root/apps/wknd-vendor-packages/container/install`. Le répertoire `/all/src/main/content` existe, il suffit de créer les répertoires `jcr_root/apps/wknd-vendor-packages/container/install`.

- Copiez le fichier de package (.zip) créé localement dans le répertoire `/all/src/main/content/jcr_root/apps/mysite-vendor-packages/container/install` .

- Créez et déployez le projet WKND AEM à l’aide de la commande suivante :

  ```
  $ mvn clean install -PautoInstallPackage
  ```

- Vérifiez le package ACS AEM Commons installé :

   - Gestionnaire de modules CRX @ `http://localhost:4502/crx/packmgr/index.jsp`

     ![Package de version SNAPSHOT ACS AEM Commons](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-package.png)

   - La console OSGi @ `http://localhost:4502/system/console/bundles`

     ![Groupe de version SNAPSHOT ACS AEM Commons](./assets/install-third-party-articafcts/acs-aem-commons-snapshot-bundle.png)

- Validez ci-dessus les modifications apportées au référentiel du projet AEM WKND. Vérifiez ensuite les modifications apportées à l’environnement RDE ou AEM en exécutant le pipeline Cloud Manager.

### Principaux enseignements{#key-learnings-package}

Les packages AEM qui ne sont pas disponibles dans le référentiel Maven public peuvent être installés dans un projet AEM en procédant comme suit :

- Copiez le package dans le répertoire `jcr_root/apps/<PROJECT-NAME>-vendor-packages/container/install` du module `all`. Cette étape est nécessaire pour empaqueter et déployer le package sur l’instance AEM.


## Résumé

Dans ce tutoriel, vous avez appris à installer des artefacts tiers (bundle, Java Jar et package) qui ne sont pas disponibles dans le référentiel Maven public lors de la création et du déploiement d’un projet AEM.
