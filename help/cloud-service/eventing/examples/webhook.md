---
title: Webhooks et événements AEM
description: Découvrez comment recevoir des événements AEM sur un webhook et consulter les détails de l’événement tels que la payload, les en-têtes et les métadonnées.
version: Cloud Service
feature: Developing, App Builder
topic: Development, Architecture, Content Management
role: Architect, Developer
level: Beginner
doc-type: Tutorial
duration: 358
last-substantial-update: 2023-01-29T00:00:00Z
jira: KT-14732
thumbnail: KT-14732.jpeg
exl-id: 00954d74-c4c7-4dac-8d23-7140c49ae31f
source-git-commit: efa0a16649c41fab8309786a766483cfeab98867
workflow-type: ht
source-wordcount: '520'
ht-degree: 100%

---

# Webhooks et événements AEM

Découvrez comment recevoir des événements AEM sur un webhook et consulter les détails de l’événement tels que la payload, les en-têtes et les métadonnées.

>[!VIDEO](https://video.tv.adobe.com/v/3427051?quality=12&learn=on)

Dans cet exemple, en utilisant un _webhook hébergé_ fourni par Adobe, vous pouvez recevoir des événements AEM sans avoir à configurer votre propre webhook. Ce webhook fourni par Adobe est hébergé sur [Glitch](https://glitch.com/), une plateforme connue pour proposer un environnement web propice à la création et au déploiement d’applications web. Cependant, vous pouvez tout de même utiliser votre propre webhook si vous le souhaitez.

## Conditions préalables

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Un projet Adobe Developer Console configuré pour les événements AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).


## Accéder à un webhook

Pour accéder au webhook fourni par Adobe, procédez comme suit :

- Vérifiez que vous pouvez accéder au [webhook hébergé sur Glitch](https://lovely-ancient-coaster.glitch.me/) dans un nouvel onglet du navigateur.

  ![Webhook hébergé sur Glitch](../assets/examples/webhook/glitch-hosted-webhook.png)

- Saisissez un nom unique pour votre webhook, par exemple, `<YOUR_PETS_NAME>-aem-eventing`, et cliquez sur **Connecter**. Le message `Connected to: ${YOUR-WEBHOOK-URL}` doit s’afficher à l’écran.

  ![Glitch – Créer un webhook](../assets/examples/webhook/glitch-create-webhook.png)

- Notez l’**URL du webhook**. Vous en aurez besoin plus loin dans ce tutoriel.

## Configurer un webhook dans un projet Adobe Developer Console

Pour recevoir des événements AEM sur l’URL du webhook ci-dessus, procédez comme suit :

- Dans [Adobe Developer Console](https://developer.adobe.com), accédez à votre projet et cliquez pour l’ouvrir.

- Sous la section **Produits et services**, cliquez sur les points de suspension `...` en regard de la carte d’événements de votre choix qui doit envoyer des événements AEM au webhook et sélectionnez **Modifier**.

  ![Modification d’un projet Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- Dans la boîte de dialogue **Configurer l’enregistrement d’événement** qui vient de s’ouvrir, cliquez sur **Suivant** pour passer à l’étape **Comment recevoir des événements**.

  ![Configuration d’un projet Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- À l’étape **Comment recevoir des événements**, sélectionnez l’option **Webhook** et collez l’**URL du webhook** que vous avez copiée précédemment à partir du webhook hébergé sur Glitch, puis cliquez sur **Enregistrer les événements configurés**.

  ![Webhook de projet Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Sur la page du webhook Glitch, une requête GET doit s’afficher. Il s’agit d’une requête de défi envoyée par les événements Adobe I/O pour vérifier l’URL du webhook.

  ![Glitch – Requête de défi](../assets/examples/webhook/glitch-challenge-request.png)


## Déclencher des événements AEM

Pour déclencher des événements AEM à partir de votre environnement AEM as a Cloud Service qui a été enregistré dans le projet Adobe Developer Console ci-dessus, procédez comme suit :

- Accédez à votre environnement de création AEM as a Cloud Service via [Cloud Manager](https://my.cloudmanager.adobe.com/) et connectez-vous.

- Selon vos **Événements abonnés**, créez, mettez à jour, supprimez, publiez ou annulez la publication d’un fragment de contenu.

## Vérifier les détails de l’événement

Après avoir effectué les étapes ci-dessus, vous devriez voir les événements AEM diffusés au webhook. Recherchez la requête POST dans la page du webhook Glitch.

![Glitch – Requête POST](../assets/examples/webhook/glitch-post-request.png)

Voici les détails clés de la requête POST :

- chemin : `/webhook/${YOUR-WEBHOOK-URL}`, par exemple `/webhook/AdobeTM-aem-eventing`

- en-têtes : en-têtes de requête envoyés par les événements Adobe I/O, par exemple :

```json
{
"connection": "close",
"x-forwarded-for": "34.205.178.127,::ffff:10.10.10.136,::ffff:10.10.84.114",
"x-forwarded-proto": "https,http,http",
"x-forwarded-port": "443,80,80",
"host": "lovely-ancient-coaster.glitch.me",
"content-length": "826",
"x-adobe-public-key2-path": "/prod/keys/pub-key-IkpzhSpTw0.pem",
"x-adobe-delivery-id": "18abfb47-d24a-4684-ade8-f442a3444033",
"x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p46652-e1074060@adobe.com",
"x-adobe-public-key1-path": "/prod/keys/pub-key-Ptc2pD9vT9.pem",
"x-adobe-event-id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
"x-adobe-event-code": "aem.sites.contentFragment.modified",
"user-agent": "Adobe/1.0",
"x-adobe-digital-signature-2": "zGLso15+6PV6X6763/x6WqgxDlEXpkv5ty8q4njaq3aUngAI9VCcYonbScEjljRluzjZ05uMJmRfNxwjj60syxEJPuc0dpmMU635gfna7I4T7IaHs496wx4m2E5mvCM+aKbNQ+NPOutyTqI8Ovq29P2P87GIgMlGhAtOaxRVGNc6ksBxc2tCWbrKUhW8hPJ0sHphU499dN4TT32xrZaiRw4akT3M/hYydsA8dcWpJ7S4dpuDS21YyDHAB8s9Dawtr3fyPEyLgZzpwZDfCqQ8gdSCGqKscE4pScwqPkKOYCHDnBvDZVe583jhcZbHGjk7Ncp/FrgQk7avWsk5XlzcuA==",
"x-adobe-digital-signature-1": "QD7THFJ1vmJqD/BatIpzO6+ACQ9cSKPR7XVaW0LI7cN/xs7ucyri6dmkerOPe9EJpjGoqCg8rxWedrIRQB3lgVskChbHH3Ujx5YG0aTQLSd1Lsn5CFbW1U0l0GqId9Cnd6MccrqSznZXcdW1rMFuRk8+gqwabBifSaLbu3r30G5hmqQd72VtiYTE4m23O3jYIMiv62pRP+a+p4NjNj1XG320uRSry+BPniTjDJ6oN/Ng7aUEKML8idZ/ZTqeh/rJSrVO95UryUolFDRwDkRn5zKonbvhSLAeXzaPhvimWUHtldq9M1WTyRMpsBk8BRzaklxlq+woJ2UjYPUIEzjotw==",
"accept-encoding": "deflate,compress,identity",
"content-type": "application/cloudevents+json; charset=UTF-8",
"x-forwarded-host": "lovely-ancient-coaster.glitch.me",
"traceparent": "00-c27558588d994f169186ca6a3c6607d4-a7e7ee36625488d4-01"
}
```

- corps/payload : corps de la requête envoyé par les événements Adobe I/O, par exemple :

```json
{
  "specversion": "1.0",
  "type": "aem.sites.contentFragment.modified",
  "source": "acct:aem-p46652-e1074060@adobe.com",
  "id": "bf922a49-9db4-4377-baf4-70e96e15c45f",
  "time": "2023-12-12T20:36:43.583228Z",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "datacontenttype": "application/json",
  "data": {
    "user": {
      "imsUserId": "933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xxx@adobe.com",
      "displayName": "First LastName",
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "model": {
      "id": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9a2d3e6a-efda-4079-a86e-0ef2ede692da",
    "properties": [
      {
        "name": "groupSize",
        "changeType": "modified"
      }
    ]
  },
  "event_id": "a0f3fb7d-b02c-4612-aac6-e472b80af793",
  "recipient_client_id": "f51ea733ba404db299fefbf285dc1c42"
}
```

Vous constatez que les détails de l’événement AEM disposent de toutes les informations nécessaires pour traiter l’événement dans le webhook. Par exemple, le type d’événement (`type`), la source de l’événement (`source`), l’identifiant de l’événement (`event_id`), l’heure de l’événement (`time`) et les données de l’événement (`data`).

## Ressources supplémentaires

- [Le code source de webhook Glitch](https://glitch.com/edit/#!/lovely-ancient-coaster) est disponible à titre de référence.
