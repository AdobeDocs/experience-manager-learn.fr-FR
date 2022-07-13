---
title: Connexions HTTP/HTTPS pour les adresses IP sortantes dédiées et les VPN
description: Découvrez comment effectuer des requêtes HTTP/HTTPS d’AEM as a Cloud Service à des services web externes s’exécutant pour une adresse IP Egress dédiée et un VPN
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9354
thumbnail: KT-9354.jpeg
exl-id: a565bc3a-675f-4d5e-b83b-c14ad70a800b
source-git-commit: bdce84fdcc949c8f8d0690ee7110238d8e8d3e42
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 0%

---

# Connexions HTTP/HTTPS pour les adresses IP sortantes dédiées et les VPN

Les connexions HTTP/HTTPS sont automatiquement traitées par proxy en dehors des AEM as a Cloud Service avec une adresse IP de sortie dédiée ou un VPN, et elles n’ont pas besoin de `portForwards` règles.

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancées suivantes.

Assurez-vous que la variable [adresse IP de sortie dédiée ou VPN](../advanced-networking.md#advanced-networking) une configuration réseau avancée a été configurée avant de suivre ce tutoriel.

| Pas de mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP sortante dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✘ | ✔ | ✔ |

>[!CAUTION]
>
> Cet exemple de code concerne uniquement [Adresse IP Egress dédiée](../dedicated-egress-ip-address.md) et [VPN](../vpn.md). Un exemple de code similaire, mais différent est disponible pour [Connexions HTTP/HTTPS sur les ports non standard pour une sortie de port flexible](./http-on-non-standard-ports-flexible-port-egress.md).

## Exemple de code

Cet exemple de code Java™ illustre un service OSGi qui peut s’exécuter dans AEM as a Cloud Service qui établit une connexion HTTP à un serveur web externe sur 8080. Les connexions HTTPS (ou HTTP) sont automatiquement traitées par proxy hors d’AEM as a Cloud Service et ne nécessitent pas de développement spécial.

>[!NOTE]
> Il est recommandé de [API HTTP Java™ 11](https://docs.oracle.com/en/java/javase/11/docs/api/java.net.http/java/net/http/package-summary.html) sont utilisés pour effectuer des appels HTTP/HTTPS depuis AEM.

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
