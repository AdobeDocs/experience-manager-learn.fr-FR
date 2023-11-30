---
title: Connexions HTTP/HTTPS pour l’adresse IP de sortie dédiée et le VPN
description: Découvrez comment effectuer des requêtes HTTP/HTTPS d’AEM as a Cloud Service vers des services web externes s’exécutant pour une adresse IP de sortie dédiée et un VPN.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
jira: KT-9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 100%

---

# Connexions HTTP/HTTPS pour l’adresse IP de sortie dédiée et le VPN

Les connexions HTTP/HTTPS sont automatiquement traitées par proxy en dehors d’AEM as a Cloud Service avec une adresse IP de sortie dédiée ou un VPN, et elles n’ont pas besoin de règles `portForwards` spéciales.

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancée suivantes.

Assurez-vous qu’une configuration réseau avancée de l’[adresse IP de sortie dédiée ou du VPN](../advanced-networking.md#advanced-networking) a été configurée avant de suivre ce tutoriel.

| Aucune mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP de sortie dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel (VPN)](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Cet exemple de code concerne uniquement l’[adresse IP de sortie dédiée](../dedicated-egress-ip-address.md) et le [VPN](../vpn.md). Un exemple de code similaire, mais différent, est disponible pour les [connexions HTTP/HTTPS sur les ports non standard pour une sortie de port flexible](./http-on-non-standard-ports-flexible-port-egress.md).

## Exemple de code

Cet exemple de code Java™ illustre un service OSGi qui peut s’exécuter dans AEM as a Cloud Service et qui établit une connexion HTTP à un serveur web externe sur le port 8080. Les connexions HTTPS (ou HTTP) sont automatiquement traitées par proxy hors d’AEM as a Cloud Service et ne nécessitent pas de développement spécial.

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
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.time.Duration;

@Component
public class HttpExternalServiceImpl implements ExternalService {
    private static final Logger log = LoggerFactory.getLogger(HttpExternalServiceImpl.class);

    // client with connection pool reused for all requests
    private HttpClient client = HttpClient.newBuilder().build();

    @Override
    public boolean isAccessible() {

        // Prepare the full URI to request, note this will have the
        // - Scheme (http/https)
        // - External host name
        // - External port
        // The external service URI, including the scheme/host/port, is defined in code, rather than in Cloud Manager portForwards rules.
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
