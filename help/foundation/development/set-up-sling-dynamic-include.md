---
title: Configuration de l’inclusion dynamique Sling pour les AEM
description: Présentation vidéo de l’installation et de l’utilisation d’Apache Sling Dynamic Include avec AEM Dispatcher s’exécutant sur le serveur web Apache HTTP.
version: 6.4, 6.5
sub-product: foundation, sites
feature: APIs
topics: caching
activity: develop
audience: architect, developer
doc-type: technical video
topic: Development
role: Developer
level: Experienced
exl-id: 6c504710-be8f-4b44-bd8a-aaf480ae6d8a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '257'
ht-degree: 8%

---

# Configuration [!DNL Sling Dynamic Include]

Présentation vidéo de l’installation et de l’utilisation [!DNL Apache Sling Dynamic Include] avec [AEM Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=fr) en cours d’exécution [!DNL Apache HTTP Web Server].

>[!VIDEO](https://video.tv.adobe.com/v/17040/?quality=12&learn=on)

>[!NOTE]
>
> Assurez-vous que la dernière version d’AEM Dispatcher est installée localement.

1. Téléchargez et installez le [[!DNL Sling Dynamic Include] lot](https://sling.apache.org/downloads.cgi).
1. Configurer [!DNL Sling Dynamic Include] via le [!DNL OSGi Configuration Factory] at **http://&lt;host>:&lt;port>/system/console/configMgr/org.apache.sling.dynamicinclude.Configuration**.

   Ou, pour ajouter à une base de code AEM, créez la **sling:OsgiConfig** noeud à l’adresse :

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/content"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. (Facultatif) Répétez la dernière étape pour autoriser les composants sur [contenu verrouillé (initial) des modèles modifiables](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/page-templates-editable.html) à diffuser via [!DNL SDI] ainsi que . La raison de la configuration supplémentaire est que le contenu verrouillé des modèles modifiables est diffusé à partir de `/conf` au lieu de `/content`.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
       xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
       jcr:primaryType="sling:OsgiConfig"
       include-filter.config.enabled="{Boolean}true"
       include-filter.config.path="/conf"
       include-filter.config.resource-types="[my-app/components/content/highly-dynamic]"
       include-filter.config.include-type="SSI" 
       include-filter.config.add_comment="{Boolean}false"
       include-filter.config.selector="nocache"
       include-filter.config.ttl=""
       include-filter.config.required_header="Server-Agent=Communique-Dispatcher"
       include-filter.config.ignoreUrlParams="[]"
       include-filter.config.rewrite="{Boolean}true"
       />
   <!--
   * include-filter.config.include-type="SSI | ESI | JSI"
   * include-filter.config.ttl is # of seconds (requires AEM Dispatcher 4.1.11+)
   -->
   ```

1. Mettre à jour [!DNL Apache HTTPD Web server]&#39;s `httpd.conf` pour activer le fichier [!DNL Include] module .

   ```shell
   $ sudo vi .../httpd.conf
   ```

   ```shell
   LoadModule include_module libexec/apache2/mod_include.so
   ```

1. Mettez à jour le [!DNL vhost] afin de respecter les directives d’inclusion.

   ```shell
   $ sudo vi .../vhosts/aem-publish.local.conf
   ```

   ```shell
   <VirtualHost *:80>
   ...
      <Directory /Library/WebServer/docroot/publish>
         ...
         # Add Includes to enable SSI Includes used by Sling Dynamic Include
         Options FollowSymLinks Includes
   
         # Required to have dispatcher-handler process includes
         ModMimeUsePathInfo On
   
         # Set includes to process .html files
         AddOutputFilter INCLUDES .html
         ...
      </Directory>
   ...
   </VirtualHost>
   ```

1. Mettez à jour le fichier de configuration dispatcher.any pour la prise en charge (1) `nocache` et (2) activez la prise en charge TTL.

   ```shell
   $ sudo vi .../conf/dispatcher.any
   ```

   ```shell
   /rules {
     ...
     /0009 {
       /glob "*.nocache.html*"
       /type "deny"
     } 
   }
   ```

   >[!TIP]
   >
   > Quitter la fin `*` off dans glob `*.nocache.html*` règle ci-dessus, peut entraîner [problèmes dans les demandes de sous-ressources](https://github.com/AdobeDocs/experience-manager-learn.en/issues/16).

   ```shell
   /cache {
       ...
       /enableTTL "1"
   }
   ```

1. Toujours redémarrer [!DNL Apache HTTP Web Server] après avoir apporté des modifications à ses fichiers de configuration ou à l’événement `dispatcher.any`.

   ```shell
   $ sudo apachectl restart
   ```

>[!NOTE]
>
>Si vous utilisez [!DNL Sling Dynamic Includes] pour la diffusion des inclusions côté serveur (ESI), assurez-vous de mettre en cache les éléments pertinents [en-têtes de réponse dans le cache du Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#CachingHTTPResponseHeaders). Les en-têtes possibles sont les suivants :
>
>* &quot;Cache-Control&quot;
>* &quot;Content-Disposition&quot;
>* &quot;Content-Type&quot;
>* &quot;Date d’expiration&quot;
>* &quot;Last-Modified&quot;
>* &quot;ETag&quot;
>* &quot;X-Content-Type-Options&quot;
>* &quot;Last-Modified&quot;
>


## Documents annexes

* [Téléchargement du lot Sling Dynamic Include](https://sling.apache.org/downloads.cgi)
* [Documentation Apache Sling Dynamic Include](https://github.com/Cognifide/Sling-Dynamic-Include)
