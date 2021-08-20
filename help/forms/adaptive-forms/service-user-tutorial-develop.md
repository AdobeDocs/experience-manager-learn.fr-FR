---
title: Développement avec des utilisateurs de services dans AEM Forms
description: Cet article vous guide tout au long du processus de création d’un utilisateur de service dans AEM Forms
feature: Formulaires adaptatifs
topic: Développement
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 3%

---


# Développement avec des utilisateurs de services dans AEM Forms

Cet article vous guide tout au long du processus de création d’un utilisateur de service dans AEM Forms

Dans les versions précédentes d’Adobe Experience Manager (AEM), le résolveur de ressources d’administration était utilisé pour le traitement principal qui nécessitait l’accès au référentiel. L’utilisation du résolveur de ressources d’administration est obsolète dans AEM 6.3. À la place, un utilisateur système disposant d’autorisations spécifiques dans le référentiel est utilisé.

Cet article décrit la création d’un utilisateur système et la configuration des propriétés du mappeur utilisateur.

1. Accédez à [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Connectez-vous en tant qu’&quot;admin&quot;
1. Cliquez sur &quot;Administration des utilisateurs&quot;
1. Cliquez sur &quot;Créer un utilisateur système&quot;.
1. Définissez le type userid sur &quot;data&quot; et cliquez sur l’icône verte pour terminer le processus de création de l’utilisateur système.
1. [Ouvrez configMgr](http://localhost:4502/system/console/configMgr)
1. Recherchez &quot;Service de mappage des utilisateurs du service Apache Sling&quot; et cliquez pour ouvrir les propriétés.
1. Cliquez sur l’icône *+* (plus) pour ajouter le mappage de service suivant :

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Cliquez sur Enregistrer .

Dans la configuration ci-dessus, le paramètre DevelopingWithServiceUser.core est le nom symbolique du lot. getresourceresolver est le nom du sous-service. data est l’utilisateur système créé à l’étape précédente.

Nous pouvons également obtenir le résolveur de ressources au nom de l’utilisateur fd-service. Cet utilisateur de service est utilisé pour les services de document. Par exemple, si vous souhaitez certifier/appliquer des droits d’utilisation, etc., nous utiliserons le résolveur de ressources de l’utilisateur fd-service pour effectuer les opérations.

1. [Téléchargez et décompressez le fichier zip associé à cet article.](assets/developingwithserviceuser.zip)
1. Accédez à [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Chargement et démarrage du lot OSGi
1. Assurez-vous que le lot est à principal état
1. Vous avez maintenant créé un *utilisateur système* et déployé le *lot utilisateur de service*.

   Pour permettre l’accès à /content, attribuez à l’utilisateur système (&#39; data &#39;) des autorisations de lecture sur le noeud de contenu.

   1. Accédez à [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Recherchez les données de l’utilisateur. Il s’agit du même utilisateur système que celui que vous avez créé à l’étape précédente.
   1. Double-cliquez sur l’utilisateur, puis cliquez sur l’onglet &quot;Autorisations&quot;.
   1. Accédez au dossier &#39;content&#39; en lecture.
   1. Pour utiliser l’utilisateur du service pour accéder au dossier /content , utilisez le code suivant :

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Si vous souhaitez accéder au fichier /content/dam/data.json de votre lot, vous utiliserez le code suivant. Ce code suppose que vous avez donné des autorisations de lecture à l’utilisateur &quot;data&quot; sur le noeud /content/dam/

   ```java
   @Reference
   GetResolver getResolver;
   .
   .
   .
   ResourceResolver serviceResolver = getResolver.getServiceResolver();
   // to get resource resolver specific to fd-service user. This is for Document Services
   ResourceResolver fdserviceResolver = getResolver.getFormsServiceResolver();
   Node resNode = getResolver.getServiceResolver().getResource("/content/dam/data.json").adaptTo(Node.class);
   ```

   Le code complet de la mise en oeuvre est indiqué ci-dessous.

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
     HashMap<String, Object> param = new HashMap<String, Object>();
     param.put(ResourceResolverFactory.SUBSERVICE, "getresourceresolver");
     ResourceResolver resolver = null;
     try {
      resolver = resolverFactory.getServiceResourceResolver(param);
     } catch (LoginException e) {
      // TODO Auto-generated catch block
      e.printStackTrace();
     }
     return resolver;
    }
   ```

