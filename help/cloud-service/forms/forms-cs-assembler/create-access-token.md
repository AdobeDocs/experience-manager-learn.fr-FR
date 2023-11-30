---
title: Créer un jeton d’accès
description: Échangez le jeton web JSON (JWT) avec les API Adobe IMS contre un jeton d’accès AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-9980
exl-id: 0e4fd0a0-eaa8-490d-b036-713b25974d60
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '86'
ht-degree: 100%

---

# Exchanger un jeton JWT contre un jeton d’accès


Le jeton JWT créé à l’étape précédente est échangé avec les API Adobe IMS contre un jeton d’accès, qui peut ensuite être utilisé pour accéder à AEM as a Cloud Service. Pour demander un jeton d’accès, envoyez une requête POST contenant le jeton JWT, le client_id et le client_secret au service d’authentification IMS.

Le code suivant a été utilisé pour générer l’échange du jeton JWT contre le jeton d’accès.

```java
public String getAccessToken() {
        
        String jwtToken = getJWTToken();
        GetServiceCredentials getCredentials = new GetServiceCredentials();
        System.out.println("Getting Access Token");

        try {
                HttpClient httpClient = HttpClientBuilder.create().build();
                HttpHost authServer = new HttpHost(getCredentials.getIMS_ENDPOINT(), 443, "https");
                HttpPost authPostRequest = new HttpPost("/ims/exchange/jwt");
                List < NameValuePair > nameValuePairs = new ArrayList < NameValuePair > ();
                nameValuePairs.add(new BasicNameValuePair("jwt_token", jwtToken));
                nameValuePairs.add(new BasicNameValuePair("client_id", getCredentials.getCLIENT_ID()));
                nameValuePairs.add(new BasicNameValuePair("client_secret", getCredentials.getCLIENT_SECRET()));
                authPostRequest.setEntity(new UrlEncodedFormEntity(nameValuePairs, Consts.UTF_8));
                HttpResponse response;
                response = httpClient.execute(authServer, authPostRequest);
                StatusLine statusLine = response.getStatusLine();
                System.out.println("The status code is " + statusLine.getStatusCode());
                HttpEntity result = response.getEntity();
                String jsonResponseStr = EntityUtils.toString(result);
                System.out.println(jsonResponseStr);
                JsonReader jsonReader = new JsonReader(new StringReader(jsonResponseStr));
                JsonObject jsonObject = JsonParser.parseReader(jsonReader).getAsJsonObject();
                
                System.out.println("Returning access_token " + jsonObject.get("access_token").getAsString());
                return jsonObject.get("access_token").getAsString();

        } catch (Exception e) {
                System.out.print("Error: " + e.getMessage());
        }
        return "null";

}
```
