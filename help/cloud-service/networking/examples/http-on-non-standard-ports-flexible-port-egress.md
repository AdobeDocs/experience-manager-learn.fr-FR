---
title: Connexions HTTP/HTTPS sur les ports non standard pour une sortie de port flexible
description: Découvrez comment effectuer des requêtes HTTP/HTTPS d’AEM as a Cloud Service à des services web externes s’exécutant sur des ports non standard pour une sortie de port flexible.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: c8cc0385-9e94-4120-9fb1-aeccbfcc8aa4
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '239'
ht-degree: 100%

---

# Connexions HTTP/HTTPS sur les ports non standard pour une sortie de port flexible

Les connexions HTTP/HTTPS sur les ports non standard (autres que 80/443) doivent être traitées par proxy en dehors d’AEM as a Cloud Service, mais elles n’ont pas besoin de règles `portForwards` spéciales et peuvent utiliser l’`AEM_PROXY_HOST` de la mise en réseau avancée d’AEM et un port proxy réservé `AEM_HTTP_PROXY_PORT` ou `AEM_HTTPS_PROXY_PORT` selon que la destination est HTTP/HTTPS.

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancée suivantes.

Assurez-vous que la configuration de mise en réseau avancée [appropriée](../advanced-networking.md#advanced-networking) a été définie avant de suivre ce tutoriel.

| Aucune mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP de sortie dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel (VPN)](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✘ | ✘ |

>[!CAUTION]
>
> Cet exemple de code concerne uniquement la [sortie de port flexible](../flexible-port-egress.md). Un exemple de code similaire, mais différent, est disponible pour les [connexions HTTP/HTTPS sur des ports non standard pour les adresses IP de sortie dédiée et le VPN](./http-dedicated-egress-ip-vpn.md).

## Exemple de code

Cet exemple de code Java™ illustre un service OSGi qui peut s’exécuter dans AEM as a Cloud Service et qui établit une connexion HTTP à un serveur web externe sur le port 8080. Les connexions aux serveurs web HTTPS utilisent les variables d’environnement `AEM_PROXY_HOST` et `AEM_HTTPS_PROXY_PORT` (`proxy.tunnel:3128` par défaut dans les versions AEM &lt; 6094).

>[!NOTE]
> Il est recommandé d’utiliser les [API HTTP Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) pour effectuer des appels HTTP/HTTPS à partir d’AEM.

+ `core/src/com/adobe/aem/wknd/examples/connections/impl/HttpExternalServiceImpl.java`

```java
package com.adobe.aem.wknd.examples.core.connections.impl;

import com.adobe.aem.wknd.examples.core.connections.ExternalService;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.IOException;
import java.net.InetSocketAddress;
import java.net.ProxySelector;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    @Override
    public boolean isAccessible() {
        HttpClient client;

        // Use System.getenv("AEM_PROXY_HOST") and proxy port System.getenv("AEM_HTTP_PROXY_PORT") 
        // or System.getenv("AEM_HTTPS_PROXY_PORT"), depending on if the destination requires HTTP/HTTPS

        if (System.getenv("AEM_PROXY_HOST") != null) {
            // Create a ProxySelector that uses to AEM's provided AEM_PROXY_HOST, with a fallback of proxy.tunnel, and proxy port using the AEM_HTTP_PROXY_PORT variable. 
            // If the destination requires HTTPS, then use the variable AEM_HTTPS_PROXY_PORT instead of AEM_HTTP_PROXY_PORT.
 
            ProxySelector proxySelector = ProxySelector.of(new InetSocketAddress(
                System.getenv().getOrDefault("AEM_PROXY_HOST", "proxy.tunnel"), 
                Integer.parseInt(System.getenv().get("AEM_HTTP_PROXY_PORT"))));

            client = HttpClient.newBuilder().proxy(proxySelector).build();
            log.debug("Using HTTPClient with AEM_PROXY_HOST");
        } else {
            client = HttpClient.newBuilder().build();
            // If no proxy is set up (such as local dev)
            log.debug("Using HTTPClient without AEM_PROXY_HOST");
        }

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, and NOT in Cloud Manager portForwards rules.
        URI uri = URI.create("http://api.example.com:8080/test.json");

        // Prepare the HttpRequest
        HttpRequest request = HttpRequest.newBuilder().uri(uri).timeout(Duration.ofSeconds(2)).build();

        // Send the HttpRequest using the configured HttpClient
        HttpResponse<String> response = null;
        try {
            // Request the URL
            response = client.send(request, HttpResponse.BodyHandlers.ofString());

            log.debug("HTTP response body: {} ", response.body());

            // Our simple example returns true is response is successful! (200 status code)
            return response.statusCode() == 200;
        } catch (IOException e) {
            return false;
        } catch (InterruptedException e) {
            return false;
        }
    }
}
```
