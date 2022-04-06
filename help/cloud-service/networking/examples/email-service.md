---
title: Service Email
description: Découvrez comment configurer AEM as a Cloud Service pour se connecter à un service de messagerie à l’aide des ports de sortie.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: 4d3256cee67183803692cccc7f17ca1a0820e05d
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 1%

---

# Service Email

Envoyer des emails depuis AEM as a Cloud Service en configurant AEM `DefaultMailService` pour utiliser les ports de sortie réseau avancés.

Comme (la plupart) des services de messagerie ne s’exécutent pas sur HTTP/HTTPS, les connexions aux services de messagerie d’AEM as a Cloud Service doivent être traitées par proxy.

+ `smtp.host` est défini sur la variable d’environnement OSGi. `$[env:AEM_PROXY_HOST;default=proxy.tunnel]` il est donc acheminé par la sortie.
+ `smtp.port` est défini sur la valeur `portForward.portOrig` port qui mappe sur l’hôte et le port du service de messagerie de destination. Cet exemple utilise le mapping : `AEM_PROXY_HOST:30002` → `smtp.sendgrid.com:465`.

Puisque les secrets ne doivent pas être stockés dans le code, le nom d’utilisateur et le mot de passe du service de messagerie sont mieux fournis à l’aide de [variables de configuration OSGi secrètes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/deploying/configuring-osgi.html#secret-configuration-values), défini à l’aide de l’interface de ligne de commande AIO ou de l’API Cloud Manager.

En règle générale, [sortie de port flexible](../flexible-port-egress.md) est utilisé pour répondre à l’intégration à un service de messagerie, sauf s’il est nécessaire de `allowlist` l’adresse IP de l’Adobe, auquel cas [adresse ip de sortie dédiée](../dedicated-egress-ip-address.md) peut être utilisé.

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancées suivantes.

| Pas de mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP sortante dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuration OSGi

Cet exemple de configuration OSGi configure AEM Mail OSGi Service pour utiliser un service de messagerie externe, au moyen de Cloud Manager suivant : `portForwards` de la règle [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration) opération.

```json
...
"portForwards": [{
    "name": "smtp.sendgrid.com",
    "portDest": 465,
    "portOrig": 30002
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configuration d’AEM [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html#sending-email) selon les besoins de votre fournisseur de messagerie (par exemple, `smtp.ssl`, etc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30002",
    "smtp.user": "$[env:EMAIL_USERNAME;default=emailapikey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

Les éléments suivants `aio CLI` peut être utilisée pour définir les secrets OSGi par environnement :

```shell
$ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "apikey" --secret EMAIL_PASSWORD "password123"
```
