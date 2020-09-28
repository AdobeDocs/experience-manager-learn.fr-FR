---
title: Développement avec des utilisateurs de services dans AEM Forms
seo-title: Développement avec des utilisateurs de services dans AEM Forms
description: Cet article vous guide tout au long du processus de création d’un utilisateur de service en AEM Forms.
seo-description: Cet article vous guide tout au long du processus de création d’un utilisateur de service en AEM Forms.
uuid: 996f30df-3fc5-4232-a104-b92e1bee4713
feature: adaptive-forms
topics: development,administration
audience: implementer,developer
doc-type: article
activity: setup
discoiquuid: 65bd4695-e110-48ba-80ec-2d36bc53ead2
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 2%

---


# Développement avec des utilisateurs de services dans AEM Forms

Cet article vous guide tout au long du processus de création d’un utilisateur de service en AEM Forms.

Dans les versions précédentes de Adobe Experience Manager (AEM), le résolveur de ressources d&#39;administration était utilisé pour le traitement principal qui nécessitait l&#39;accès au référentiel. L&#39;utilisation du résolveur de ressources d&#39;administration est abandonnée dans AEM 6.3. À la place, un utilisateur système disposant d&#39;autorisations spécifiques dans le référentiel est utilisé.

Cet article décrit la création d’un utilisateur système et la configuration des propriétés du mappeur utilisateur.

1. Accédez à [http://localhost:4502/crx/explorer/index.jsp](http://localhost:4502/crx/explorer/index.jsp)
1. Se connecter en tant que &#39; admin &#39;
1. Cliquez sur &#39; Administration utilisateur &#39;
1. Cliquez sur &#39; Créer un utilisateur système &#39;
1. Définissez le type d&#39;utilisateur comme &quot; données &quot; et cliquez sur l&#39;icône verte pour terminer le processus de création de l&#39;utilisateur système.
1. [Ouvrir configMgr](http://localhost:4502/system/console/configMgr)
1. Recherchez &quot;Service Apache Sling User Mapper Service&quot; et cliquez sur pour ouvrir les propriétés.
1. Cliquez sur l’icône *+* (plus) pour ajouter le mappage de services suivant.

   * DevelopingWithServiceUser.core:getresourceresolver=data
   * DevelopingWithServiceUser.core:getformsresourceresolver=fd-service

1. Cliquez sur &quot; Enregistrer &quot;

Dans la configuration ci-dessus, le paramètre DevelopingWithServiceUser.core est le nom symbolique du lot. getresourceresolver est le nom de sous-service.data est l&#39;utilisateur système créé à l&#39;étape précédente.

Nous pouvons également obtenir le résolveur de ressources pour le compte de l&#39;utilisateur du service fd. Cet utilisateur de service est utilisé pour les services de document. Par exemple, si vous souhaitez certifier/appliquer des droits d’utilisation, etc., nous utiliserons le résolveur de ressources de l’utilisateur du service fd pour effectuer les opérations.

1. [Téléchargez et décompressez le fichier zip associé à cet article.](assets/developingwithserviceuser.zip)
1. Navigate to [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles)
1. Téléchargement et début du lot OSGi
1. Vérifier que le lot est à principal état
1. Vous avez maintenant créé un utilisateur ** système et déployé le groupe *d’utilisateurs du* service.

   Pour fournir l&#39;accès à /content, accordez à l&#39;utilisateur système (&#39; data &#39;) des autorisations de lecture sur le noeud de contenu.

   1. Accédez à [http://localhost:4502/useradmin](http://localhost:4502/useradmin)
   1. Recherchez les données de l&#39;utilisateur &quot;&quot;. Il s’agit du même utilisateur système que celui que vous avez créé à l’étape précédente.
   1. Doublon-clic sur l&#39;utilisateur, puis sur l&#39;onglet &#39; Permissions &#39;
   1. Donnez à &#39; read &#39; l&#39;accès au dossier &#39;content &#39;.
   1. Pour utiliser l’utilisateur du service pour accéder au dossier /content, utilisez le code suivant :

   ```java
   com.mergeandfuse.getserviceuserresolver.GetResolver aemDemoListings = sling.getService(com.mergeandfuse.getserviceuserresolver.GetResolver.class);
   
   resourceResolver = aemDemoListings.getServiceResolver();
   
   // get the resource. This will succeed because we have given ' read ' access to the content node
   
   Resource contentResource = resourceResolver.getResource("/content/forms/af/sandbox/abc.pdf");
   ```

   Si vous souhaitez accéder au fichier /content/dam/data.json dans votre lot, vous utiliserez le code suivant. Ce code suppose que vous avez donné des autorisations de lecture à l’utilisateur &quot;data&quot; sur le noeud /content/dam/.

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

