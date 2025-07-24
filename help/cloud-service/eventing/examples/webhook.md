---
title: Webhooks et événements AEM
description: Découvrez comment recevoir des événements AEM sur un webhook et consulter les détails de l’événement tels que la payload, les en-têtes et les métadonnées.
version: Experience Manager as a Cloud Service
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
source-git-commit: 00301753eae983a17160b783a9b166537baf5ee0
workflow-type: tm+mt
source-wordcount: '523'
ht-degree: 67%

---

# Webhooks et événements AEM

Découvrez comment recevoir des événements AEM sur un webhook et consulter les détails de l’événement tels que la payload, les en-têtes et les métadonnées.


>[!VIDEO](https://video.tv.adobe.com/v/3427051?quality=12&learn=on)


>[!IMPORTANT]
>
>La vidéo fait référence à un point d’entrée webhook hébergé par Glitch. Depuis que Glitch a arrêté son service d’hébergement, le webhook a été migré vers Azure App Service.
>
>La fonctionnalité reste la même : seule la plateforme d’hébergement a changé.


Au lieu d’utiliser l’exemple de webhook fourni par Adobe, vous pouvez également utiliser votre propre point d’entrée webhook pour recevoir les événements AEM.

## Prérequis

Les éléments suivants sont requis afin de terminer ce tutoriel :

- Environnement AEM as a Cloud Service avec [AEM Eventing activé](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#enable-aem-events-on-your-aem-cloud-service-environment).

- [Un projet Adobe Developer Console configuré pour les événements AEM](https://developer.adobe.com/experience-cloud/experience-manager-apis/guides/events/#how-to-subscribe-to-aem-events-in-the-adobe-developer-console).


## Accéder à un webhook

Pour accéder à l’exemple de webhook fourni par Adobe, procédez comme suit :

- Vérifiez que vous pouvez accéder à l’exemple de webhook fourni par [Adobe](https://aemeventing-webhook.azurewebsites.net/) dans un nouvel onglet du navigateur.

  ![Exemple de webhook fourni par Adobe](../assets/examples/webhook/adobe-provided-webhook.png)

- Saisissez un nom unique pour votre webhook, par exemple, `<YOUR_PETS_NAME>-aem-eventing`, et cliquez sur **Connecter**. Le message `Connected to: ${YOUR-WEBHOOK-URL}` doit s’afficher à l’écran.

  ![Créer votre point d’entrée webhook](../assets/examples/webhook/create-webhook-endpoint.png)

- Notez l’**URL du webhook**. Vous en aurez besoin plus loin dans ce tutoriel.

## Configurer un webhook dans un projet Adobe Developer Console

Pour recevoir des événements AEM sur l’URL du webhook ci-dessus, procédez comme suit :

- Dans [Adobe Developer Console](https://developer.adobe.com), accédez à votre projet et cliquez pour l’ouvrir.

- Sous la section **Produits et services**, cliquez sur les points de suspension `...` en regard de la carte d’événements de votre choix qui doit envoyer des événements AEM au webhook et sélectionnez **Modifier**.

  ![Modification d’un projet Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-edit.png)

- Dans la boîte de dialogue **Configurer l’enregistrement d’événement** qui vient de s’ouvrir, cliquez sur **Suivant** pour passer à l’étape **Comment recevoir des événements**.

  ![Configuration d’un projet Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-configure.png)

- Dans l’étape **Comment recevoir des événements**, sélectionnez l’option **Webhook** et collez l’URL **Webhook** que vous avez copiée précédemment à partir de l’exemple de webhook fourni par Adobe, puis cliquez sur **Enregistrer les événements configurés**.

  ![Webhook de projet Adobe Developer Console](../assets/examples/webhook/adobe-developer-console-project-webhook.png)

- Dans l’exemple de page webhook fourni par Adobe, vous devriez voir une requête GET ; il s’agit d’une requête de défi envoyée par le Adobe I/O Events pour vérifier l’URL webhook.

  ![Webhook - demande de défi](../assets/examples/webhook/webhook-challenge-request.png)


## Déclencher des événements AEM

Pour déclencher des événements AEM à partir de votre environnement AEM as a Cloud Service qui a été enregistré dans le projet Adobe Developer Console ci-dessus, procédez comme suit :

- Accédez à votre environnement de création AEM as a Cloud Service via [Cloud Manager](https://my.cloudmanager.adobe.com/) et connectez-vous.

- Selon vos **Événements abonnés**, créez, mettez à jour, supprimez, publiez ou annulez la publication d’un fragment de contenu.

## Vérifier les détails de l’événement

Après avoir effectué les étapes ci-dessus, vous devriez voir les événements AEM diffusés au webhook. Recherchez la requête POST dans l’exemple de page webhook fourni par Adobe.

![Webhook - requête POST](../assets/examples/webhook/webhook-post-request.png)

Voici les détails clés de la requête POST :

- chemin : `/webhook/${YOUR-WEBHOOK-URL}`, par exemple `/webhook/AdobeTM-aem-eventing`

- en-têtes : en-têtes de requête envoyés par les événements Adobe I/O, par exemple :

```json
{
  "host": "aemeventing-webhook.azurewebsites.net",
  "user-agent": "Adobe/1.0",
  "accept-encoding": "deflate,compress,identity",
  "max-forwards": "10",
  "x-adobe-public-key2-path": "/prod/keys/pub-key-kruhWwu4Or.pem",
  "x-adobe-delivery-id": "25c36f70-9238-4e4c-b1d8-4d9a592fed9d",
  "x-adobe-provider": "aemsites_7ABB3E6A5A7491460A495D61@AdobeOrg_acct-aem-p63947-e1249010@adobe.com",
  "x-adobe-public-key1-path": "/prod/keys/pub-key-lyTiz3gQe4.pem",
  "x-adobe-event-id": "b555a1b1-935b-4541-b410-1915775338b5",
  "x-adobe-event-code": "aem.sites.contentFragment.modified",
  "x-adobe-digital-signature-2": "Lvw8+txbQif/omgOamJXJaJdJMLDH5BmPA+/RRLhKG2LZJYWKiomAE9DqKhM349F8QMdDq6FXJI0vJGdk0FGYQa6JMrU+LK+1fGhBpO98LaJOdvfUQGG/6vq8/uJlcaQ66tuVu1xwH232VwrQOKdcobE9Pztm6UX0J11Uc7vtoojUzsuekclKEDTQx5vwBIYK12bXTI9yLRsv0unBZfNRrV0O4N7KA9SRJFIefn7hZdxyYy7IjMdsoswG36E/sDOgcnW3FVM+rhuyWEizOd2AiqgeZudBKAj8ZPptv+6rZQSABbG4imOa5C3t85N6JOwffAAzP6qs7ghRID89OZwCg==",
  "x-adobe-digital-signature-1": "ZQywLY1Gp/MC/sXzxMvnevhnai3ZG/GaO4ThSGINIpiA/RM47ssAw99KDCy1loxQyovllEmN0ifAwfErQGwDa5cuJYEoreX83+CxqvccSMYUPb5JNDrBkG6W0CmJg6xMeFeo8aoFbePvRkkDOHdz6nT0kgJ70x6mMKgCBM+oUHWG13MVU3YOmU92CJTzn4hiSK8o91/f2aIdfIui/FDp8U20cSKKMWpCu25gMmESorJehe4HVqxLgRwKJHLTqQyw6Ltwy2PdE0guTAYjhDq6AUd/8Fo0ORCY+PsS/lNxim9E9vTRHS7TmRuHf7dpkyFwNZA6Au4GWHHS87mZSHNnow==",
  "x-arr-log-id": "881073f0-7185-4812-9f17-4db69faf2b68",
  "client-ip": "52.37.214.82:46066",
  "disguised-host": "aemeventing-webhook.azurewebsites.net",
  "x-site-deployment-id": "aemeventing-webhook",
  "was-default-hostname": "aemeventing-webhook.azurewebsites.net",
  "x-forwarded-proto": "https",
  "x-appservice-proto": "https",
  "x-arr-ssl": "2048|256|CN=Microsoft Azure RSA TLS Issuing CA 03, O=Microsoft Corporation, C=US|CN=*.azurewebsites.net, O=Microsoft Corporation, L=Redmond, S=WA, C=US",
  "x-forwarded-tlsversion": "1.3",
  "x-forwarded-for": "52.37.214.82:46066",
  "x-original-url": "/webhook/AdobeTechMarketing-aem-eventing",
  "x-waws-unencoded-url": "/webhook/AdobeTechMarketing-aem-eventing",
  "x-client-ip": "52.37.214.82",
  "x-client-port": "46066",
  "content-type": "application/cloudevents+json; charset=UTF-8",
  "content-length": "1178"
}
```

- corps/payload : corps de la requête envoyé par les événements Adobe I/O, par exemple :

```json
{
  "specversion": "1.0",
  "id": "83b0eac0-56d6-4499-afa6-4dc58ff6ac7f",
  "source": "acct:aem-p63947-e1249010@adobe.com",
  "type": "aem.sites.contentFragment.modified",
  "datacontenttype": "application/json",
  "dataschema": "https://ns.adobe.com/xdm/aem/sites/events/content-fragment-modified.json",
  "time": "2025-07-24T13:53:23.994109827Z",
  "eventid": "b555a1b1-935b-4541-b410-1915775338b5",
  "event_id": "b555a1b1-935b-4541-b410-1915775338b5",
  "recipient_client_id": "606d4074c7ea4962aaf3bc2a5ac3b7f9",
  "recipientclientid": "606d4074c7ea4962aaf3bc2a5ac3b7f9",
  "data": {
    "user": {
      "imsUserId": "ims-933E1F8A631CAA0F0A495E53@80761f6e631c0c7d495fb3.e",
      "principalId": "xx@adobe.com",
      "displayName": "Sachin Mali"
    },
    "path": "/content/dam/wknd-shared/en/adventures/beervana-portland/beervana-in-portland",
    "sourceUrl": "https://author-p63947-e1249010.adobeaemcloud.com",
    "model": {
      "id": "L2NvbmYvd2tuZC1zaGFyZWQvc2V0dGluZ3MvZGFtL2NmbS9tb2RlbHMvYWR2ZW50dXJl",
      "path": "/conf/wknd-shared/settings/dam/cfm/models/adventure"
    },
    "id": "9e1e9835-64c8-42dc-9d36-fbd59e28f753",
    "tags": [
      "wknd-shared:region/nam/united-states",
      "wknd-shared:activity/social",
      "wknd-shared:season/fall"
    ],
    "properties": [
      {
        "name": "price",
        "changeType": "modified"
      }
    ]
  }
}
```

Vous constatez que les détails de l’événement AEM disposent de toutes les informations nécessaires pour traiter l’événement dans le webhook. Par exemple, le type d’événement (`type`), la source de l’événement (`source`), l’identifiant de l’événement (`event_id`), l’heure de l’événement (`time`) et les données de l’événement (`data`).

## Ressources supplémentaires

- Le code source [AEM-Eventing Webhook](../assets/examples/webhook/aemeventing-webhook.tgz) est disponible à titre de référence.
