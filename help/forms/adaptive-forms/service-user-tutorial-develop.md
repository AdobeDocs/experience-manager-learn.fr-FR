---
title: Développer avec des profils utilisateurs de service dans AEM Forms
description: Cet article vous guide tout au long du processus de création d’un profil utilisateur de service dans AEM Forms.
feature: Adaptive Forms
topic: Development
role: Developer
level: Experienced
exl-id: 5fa3d52a-6a71-45c4-9b1a-0e6686dd29bc
last-substantial-update: 2020-09-09T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: ht
source-wordcount: '445'
ht-degree: 100%

---

# Développer avec des profils utilisateurs de service dans AEM Forms

Cet article vous guide tout au long du processus de création d’un profil utilisateur de service dans AEM Forms.

Dans les versions précédentes d’Adobe Experience Manager (AEM), le résolveur de ressources d’administration était utilisé pour le traitement back-end qui nécessitait l’accès au référentiel. L’utilisation du résolveur de ressources d’administration est obsolète dans AEM 6.3 ; il est remplacé par un profil utilisateur système disposant d’autorisations spécifiques dans le référentiel.

En savoir plus sur la [création et l’utilisation des utilisateurs et utilisatrices de service dans AEM](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/developing/advanced/service-users.html?lang=fr).

Cet article décrit la création d’un profil utilisateur système et la configuration des propriétés du service de mappage des utilisateurs et des utilisatrices.

1. Accédez à [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp).
1. Connectez-vous en tant qu’« admin ».
1. Cliquez sur « Administration des utilisateurs et utilisatrices ».
1. Cliquez sur « Créer un profil utilisateur système ».
1. Définissez le type userid sur « data » et cliquez sur l’icône verte pour terminer le processus de création du profil utilisateur système.
1. [Ouvrir configMgr](http://localhost:4502/system/console/configMgr)
1. Recherchez _Service de mappage des profils utilisateurs de services Apache Sling_ et cliquez dessus pour ouvrir les propriétés.
1. Cliquez sur l’icône *+* (plus) pour ajouter le mappage de service suivant.

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Cliquez sur « Enregistrer ».

Dans la configuration ci-dessus, le paramètre DevelopingWithServiceUser.core est le nom symbolique du lot. « getresourceresolver » est le nom du sous-service. « data » est l’utilisateur système créé à l’étape précédente.

Nous pouvons également obtenir le résolveur de ressources au nom de l’utilisateur fd-service. Cet utilisateur de service est utilisé pour les services de document. Par exemple, si vous souhaitez certifier/appliquer des droits d’utilisation, etc., nous utiliserons le résolveur de ressources de l’utilisateur fd-service pour effectuer les opérations.

1. [Téléchargez et décompressez le fichier zip associé à cet article.](assets/developingwithserviceuser.zip)
1. Accédez à [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles).
1. Charger et démarrer le lot OSGi
1. Vérifiez que le lot est en état actif.
1. Vous avez maintenant créé un profil *Utilisateur système* et déployé le *lot Utilisateur de service*.

   Pour permettre l’accès à /content, attribuez au profil utilisateur système (« data ») des autorisations de lecture sur le nœud de contenu.

   1. Accédez à [http://localhost:4502/useradmin](http://localhost:4502/useradmin).
   1. Recherchez le profil utilisateur « data ». Il s’agit du profil utilisateur système que vous avez créé à l’étape précédente.
   1. Double-cliquez sur le profil utilisateur, puis cliquez sur l’onglet « Autorisations ».
   1. Accordez l’accès « read » au dossier « content ».
   1. Pour utiliser le profil utilisateur de service pour accéder au dossier /content, utilisez le code suivant :



```java
com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
resourceResolver = aemDemoListings.getServiceResolver();
   
// get the resource. This will succeed because we have given ' read ' access to the content node
   
Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
```

Si vous souhaitez accéder au fichier /content/dam/data.json de votre lot, vous utiliserez le code suivant. Ce code suppose que vous avez accordé des autorisations de lecture au profil utilisateur « data » sur le nœud /content/dam/.

```java
@Reference
GetResolver getResolver;
.
.
.
try {
   ResourceResolver serviceResolver = getResolver.getServiceResolver();

   // To get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();

   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
} catch(LoginException ex) {
   // Unable to get the service user, handle this exception as needed
} finally {
  // Always close all ResourceResolvers you open!
  
  if (serviceResolver != null( { serviceResolver.close(); }
  if (fdserviceResolver != null) { fdserviceResolver.close(); }
}
```

Le code complet de la mise en œuvre est fourni ci-dessous.

```java
package com.mergeandfuse.getserviceuserresolver.impl;
import java.util.HashMap;
import org.apache.sling.api.resource.LoginException;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.api.resource.ResourceResolverFactory;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import com.mergeandfuse.getserviceuserresolver.GetResolver;
@Component(service = GetResolver.class)
public class GetResolverImpl implements GetResolver {
        @Reference
        ResourceResolverFactory resolverFactory;

        @Override
        public ResourceResolver getServiceResolver() {
                System.out.println("#### Trying to get service resource resolver ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {

                        System.out.println("Login Exception " + e.getMessage());
                }
                return resolver;

        }

        @Override
        public ResourceResolver getFormsServiceResolver() {

                System.out.println("#### Trying to get Resource Resolver for forms ....  in my bundle");
                HashMap < String, Object > param = new HashMap < String, Object > ();
                param.put(ResourceResolverFactory.SUBSERVICE, "getformsresourceresolver");
                ResourceResolver resolver = null;
                try {
                        resolver = resolverFactory.getServiceResourceResolver(param);
                } catch (LoginException e) {
                        System.out.println("Login Exception ");
                        System.out.println("The error message is " + e.getMessage());
                }
                return resolver;
        }

}
```
