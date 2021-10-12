---
title: AEM Forms avec Marketo (partie 2)
description: Tutoriel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: f8ba3d5c-0b9f-4eb7-8609-3e540341d5c2
source-git-commit: 020852f16de0cdb1e17e19ad989dabf37b7f61f5
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 1%

---

# Service d’authentification Marketo

Les API REST de Marketo sont authentifiées avec OAuth 2.0 à 2 jambes. Nous devons créer une authentification personnalisée pour nous authentifier auprès de Marketo. Cette authentification personnalisée est généralement écrite dans un lot OSGI. Le code suivant affiche l’authentificateur personnalisé utilisé dans le cadre de ce tutoriel.

## Service d’authentification personnalisé

Le code suivant crée l’objet AuthenticationDetails dont l’attribut access_token est nécessaire pour l’authentification par rapport à Marketo.

```java
package com.marketoandforms.core;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
 
import com.adobe.aemfd.dermis.authentication.api.IAuthentication;
import com.adobe.aemfd.dermis.authentication.exception.AuthenticationException;
import com.adobe.aemfd.dermis.authentication.model.AuthenticationDetails;
import com.adobe.aemfd.dermis.authentication.model.Configuration;
@Component(service={IAuthentication.class}, immediate=true)
public class MarketoAuthenticationService implements IAuthentication {
@Reference
MarketoService marketoService;
    @Override
    public AuthenticationDetails getAuthDetails(Configuration arg0) throws AuthenticationException
    {
        AuthenticationDetails auth = new AuthenticationDetails();
        auth.addHttpHeader("Cache-Control", "no-cache");
        auth.addHttpHeader("Authorization", "Bearer " + marketoService.getAccessToken());
        return auth
    }
 
    @Override
    public String getAuthenticationType() {
        // TODO Auto-generated method stub
        return "AemForms With Marketo";
    }
}
```

MarketoAuthenticationService implémente l’interface IAuthentication. Cette interface fait partie du SDK client AEM Forms. Le service récupère le jeton d’accès et insère le jeton dans le HttpHeader de AuthenticationDetails. Une fois que les HttpHeaders de l’objet AuthenticationDetails sont renseignés, l’objet AuthenticationDetails est renvoyé à la couche Dermis du modèle de données de formulaire.

Faites attention à la chaîne renvoyée par la méthode getAuthenticationType. Cette chaîne sera utilisée lors de la configuration de votre source de données.

### Obtenir le jeton d’accès

Une interface simple est définie avec une méthode qui renvoie le jeton d’accès. Le code de la classe qui implémente cette interface est répertorié plus bas dans la page.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

Le code suivant est celui du service qui renvoie le jeton d’accès qui doit être utilisé pour effectuer les appels de l’API REST. Le code de ce service accède aux paramètres de configuration nécessaires pour effectuer l’appel de GET. Comme vous pouvez le constater, nous transmettons le client_id, client_secret dans l’URL du GET pour générer le jeton d’accès. Ce jeton d’accès est ensuite renvoyé à l’application appelante.

```java
package com.marketoandforms.core.impl;
import java.io.IOException;
import org.apache.http.HttpEntity;
import org.apache.http.HttpResponse;
import org.apache.http.ParseException;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.HttpClient;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.HttpClientBuilder;
import org.apache.http.util.EntityUtils;
import org.json.JSONException;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.marketoandforms.core.*; 
@Component(service=MarketoService.class,immediate = true)
public class MarketoServiceImpl implements MarketoService {
    private final Logger log = LoggerFactory.getLogger(getClass());
@Reference
MarketoConfigurationService config;
    @Override
    public String getAccessToken()
    {
        String AUTH_URL = config.getAUTH_URL();
        String CLIENT_ID = config.getCLIENT_ID();
        String CLIENT_SECRET = config.getCLIENT_SECRET();
        String AUTH_PATH = config.getAUTH_PATH();
        HttpClient httpClient = HttpClientBuilder.create().build();
        String getURL = AUTH_URL+AUTH_PATH+"&client_id="+CLIENT_ID+"&client_secret="+CLIENT_SECRET;
        log.debug("The url to get the access token is "+getURL);
        HttpGet httpGet = new HttpGet(getURL);
        httpGet.addHeader("Cache-Control","no-cache");
        try {
            HttpResponse httpResponse = httpClient.execute(httpGet);
            HttpEntity httpEntity = httpResponse.getEntity();
            org.json.JSONObject responseJSON = new org.json.JSONObject(EntityUtils.toString(httpEntity))
            return (String)responseJSON.get("access_token");
        } catch (ClientProtocolException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        } catch (JSONException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return null;
    }
}
```

La capture d’écran ci-dessous montre les propriétés de configuration qui doivent être définies. Ces propriétés de configuration sont lues dans le code répertorié ci-dessus pour obtenir access_token.

![config](assets/configuration-settings.png)

### Configuration

Le code suivant a été utilisé pour créer les propriétés de configuration. Ces propriétés sont spécifiques à votre instance Marketo.

```java
package com.marketoandforms.core;
 
import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;
 
@ObjectClassDefinition(name="Marketo Credentials Service Configuration", description = "Connect Form With Marketo")
public @interface MarketoConfiguration {
     @AttributeDefinition(name="Identity Endpoint", description="URL of Marketo Identity Endpoint")
     String identityEndpoint() default "";
      @AttributeDefinition(name="Authentication path", description="Marketo authentication path")
      String authPath() default "";
      @AttributeDefinition(name="Client ID", description="Client ID")
      String clientID() default "";
      @AttributeDefinition(name="Client Secret", description="Client Secret")
      String clientSecret() default "";
}
```

Le code suivant lit les propriétés de configuration et renvoie la même valeur via les méthodes getter.

```java
package com.marketoandforms.core;
 
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
@Component(immediate=true, service={MarketoConfigurationService.class})
@Designate(ocd=MarketoConfiguration.class)
public class MarketoConfigurationService {
    private final Logger log = LoggerFactory.getLogger(getClass());
    private MarketoConfiguration config;
    private String AUTH_URL;
    private String  AUTH_PATH;
    private String CLIENT_ID ;
    private String CLIENT_SECRET;
     @Activate
      protected final void activate(MarketoConfiguration config) {
        System.out.println("####In my marketo activating auth service");
        AUTH_URL = config.identityEndpoint();
        AUTH_PATH = config.authPath();
        CLIENT_ID = config.clientID();
        CLIENT_SECRET = config.clientSecret();
        log.info("clientID:" + CLIENT_ID);
        System.out.println("The client id is "+CLIENT_ID+"AUTH PATH"+AUTH_PATH);
      }
    public String getAUTH_URL() {
        return AUTH_URL;
    }
   public String getAUTH_PATH() {
        return AUTH_PATH;
    }
    public String getCLIENT_ID() {
        return CLIENT_ID;
    }

    public String getCLIENT_SECRET() {
        return CLIENT_SECRET;
    }
}
```

1. Créez et déployez le lot sur votre serveur AEM.
1. [Pointez votre navigateur sur ](http://localhost:4502/system/console/configMgr) configMgrand pour rechercher &quot;Configuration du service Marketo Credentials&quot;.
1. Spécifiez les propriétés appropriées spécifiques à votre instance Marketo.
