---
title: Création d’un jeton d’accès
description: Échangez le jeton Web JSON (JWT) avec les API IMS Adobes pour un jeton d’accès AEM.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
feature: Services de document
topic: Développement
kt: 8185
thumbnail: 8185.jpg
source-git-commit: f2a94910fbc29b705f82a66d8248cbcf54366874
workflow-type: tm+mt
source-wordcount: '89'
ht-degree: 3%

---

# Exchange JWT pour le jeton d’accès


Le jeton JWT créé à l’étape précédente est échangé avec les API IMS d’Adobe contre un jeton d’accès, qui peut ensuite être utilisé pour accéder à AEM en tant que Cloud Service. Pour demander un jeton d’accès, envoyez une demande de POST contenant le JWT, client_id, client_secret au service d’authentification IMS.

Le code suivant a été utilisé pour générer l’échange JWT pour le jeton d’accès.

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
