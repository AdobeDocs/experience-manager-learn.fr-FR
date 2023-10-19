---
title: Service de messagerie
description: Découvrez comment configurer AEM as a Cloud Service pour se connecter à un service de messagerie à l’aide des ports de sortie.
version: Cloud Service
feature: Security
topic: Development, Security
role: Architect, Developer
level: Intermediate
kt: 9353
thumbnail: KT-9353.jpeg
exl-id: 5f919d7d-e51a-41e5-90eb-b1f6a9bf77ba
source-git-commit: c34c27955dbc084620ac4dd811ba4051ea83f447
workflow-type: ht
source-wordcount: '367'
ht-degree: 100%

---

# Service de messagerie

Envoyez des e-mails depuis AEM as a Cloud Service en configurant le `DefaultMailService` d’AEM pour utiliser les ports de sortie réseau avancés.

Comme les services de messagerie ne s’exécutent en général pas sur HTTP/HTTPS, les connexions aux services de messagerie d’AEM as a Cloud Service doivent être traitées par proxy.

+ `smtp.host` est défini sur la variable d’environnement OSGi `$[env:AEM_PROXY_HOST;default=proxy.tunnel]`, il est donc acheminé par la sortie.
   + `$[env:AEM_PROXY_HOST]` est une variable réservée qu’AEM as a Cloud Service mappe sur l’hôte interne `proxy.tunnel`.
   + Ne tentez PAS de définir `AEM_PROXY_HOST` via Cloud Manager.
+ `smtp.port` est défini sur le port `portForward.portOrig` qui mappe sur l’hôte et le port du service de messagerie de destination. Cet exemple utilise le mappage : `AEM_PROXY_HOST:30465` → `smtp.sendgrid.com:465`.
   + Le `smpt.port` est défini sur le port `portForward.portOrig` et NON le port réel du serveur SMTP. Le mappage entre le `smtp.port` et le port `portForward.portOrig` est établi par la règle `portForwards` Cloud Manager (comme illustré ci-dessous).

Puisque les secrets ne doivent pas être stockés dans le code, le nom d’utilisateur ou d’utilisatrice et le mot de passe du service de messagerie sont mieux fournis à l’aide des [variables de configuration OSGi secrètes](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/deploying/configuring-osgi.html?lang=fr#secret-configuration-values), définies avec l’interface de ligne de commande AIO ou l’API Cloud Manager.

En règle générale, la [sortie de port flexible](../flexible-port-egress.md) est utilisée pour réaliser l’intégration à un service de messagerie, sauf s’il est nécessaire de placer l’adresse IP Adobe sur une `allowlist`, auquel cas l’[adresse IP de sortie dédiée](../dedicated-egress-ip-address.md) peut être utilisée.

Consultez également la documentation AEM sur l’[envoi d’un e-mail](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=fr#sending-email).

## Prise en charge de la mise en réseau avancée

L’exemple de code suivant est pris en charge par les options de mise en réseau avancée suivantes.

Assurez-vous que la configuration de mise en réseau avancée [appropriée](../advanced-networking.md#advanced-networking) a été définie avant de suivre ce tutoriel.

| Aucune mise en réseau avancée | [Sortie de port flexible](../flexible-port-egress.md) | [Adresse IP de sortie dédiée](../dedicated-egress-ip-address.md) | [Réseau privé virtuel (VPN)](../vpn.md) |
|:-----:|:-----:|:------:|:---------:|
| ✘ | ✔ | ✔ | ✔ |

## Configuration OSGi

Cet exemple de configuration OSGi configure le service OSGi de messagerie d’AEM pour utiliser un service de messagerie externe, au moyen de la règle `portForwards` Cloud Manager suivante de l’opération [enableEnvironmentAdvancedNetworkingConfiguration](https://www.adobe.io/experience-cloud/cloud-manager/reference/api/#operation/enableEnvironmentAdvancedNetworkingConfiguration).

```json
...
"portForwards": [{
    "name": "smtp.mymail.com",
    "portDest": 465,
    "portOrig": 30465
}]
...
```

+ `ui.config/src/jcr_root/apps/wknd-examples/osgiconfig/config/com.day.cq.mailer.DefaultMailService.cfg.json`

Configurez le [DefaultMailService](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/developing/development-guidelines.html?lang=fr#sending-email) d’AEM selon les besoins de votre fournisseur de messagerie (par exemple, `smtp.ssl`, etc.).

```json
{
    "smtp.host": "$[env:AEM_PROXY_HOST;default=proxy.tunnel]",
    "smtp.port": "30465",
    "smtp.user": "$[env:EMAIL_USERNAME;default=myApiKey]",
    "smtp.password": "$[secret:EMAIL_PASSWORD]",
    "from.address": "noreply@wknd.site",
    "smtp.ssl": true,
    "smtp.starttls": false, 
    "smtp.requiretls": false,
    "debug.email": false,
    "oauth.flow": false
}
```

La variable OSGi et le secret `EMAIL_USERNAME` et `EMAIL_PASSWORD` peuvent être définis selon l’environnement de l’une des manières suivantes :

+ [Configuration de l’environnement Cloud Manager](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=fr)
+ ou en utilisant la commande `aio CLI`.

  ```shell
  $ aio cloudmanager:set-environment-variables --programId=<PROGRAM_ID> <ENVIRONMENT_ID> --secret EMAIL_USERNAME "myApiKey" --secret EMAIL_PASSWORD "password123"
  ```
