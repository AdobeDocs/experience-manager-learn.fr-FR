---
title: aem forms avec Marketo (partie 2)
seo-title: aem forms avec Marketo (partie 2)
description: Didacticiel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
seo-description: Didacticiel pour intégrer AEM Forms à Marketo à l’aide du modèle de données de formulaire AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---


# Service d’authentification du marketing

Les API REST de Marketo sont authentifiées avec OAuth 2.0 à 2 pattes. Nous devons créer une authentification personnalisée pour nous authentifier auprès de Marketo. Cette authentification personnalisée est généralement écrite dans un lot OSGI. Le code suivant affiche l’authentificateur personnalisé qui a été utilisé dans le cadre de ce didacticiel.

## Service d’authentification personnalisée

Le code suivant crée l&#39;objet AuthenticationDetails qui possède le jeton access_token nécessaire pour l&#39;authentification auprès de Marketo.

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

MarketoAuthenticationService implémente l&#39;interface IAuthentication. Cette interface fait partie du SDK client AEM Forms. Le service obtient le jeton d&#39;accès et insère le jeton dans HttpHeader de AuthenticationDetails. Une fois que HttpHeaders de l’objet AuthenticationDetails est renseigné, l’objet AuthenticationDetails est renvoyé à la couche Dermis du modèle de données de formulaire.

Veuillez prêter attention à la chaîne renvoyée par la méthode getAuthenticationType. Cette chaîne sera utilisée lorsque vous configurez votre source de données.

### Obtenir le Jeton d&#39;accès

Une interface simple est définie avec une méthode qui renvoie access_token. Le code de la classe qui implémente cette interface est répertorié plus loin dans la page.

```java
package com.marketoandforms.core;
public interface MarketoService {
    String getAccessToken();
}
```

Le code suivant est celui du service qui renvoie le jeton access_token à utiliser pour les appels d&#39;API REST. Le code de ce service accède aux paramètres de configuration nécessaires pour effectuer l’appel de GET. Comme vous pouvez le constater, nous transmettons client_id, client_secret dans l’URL du GET pour générer le jeton access_token. Cet access_token est ensuite renvoyé à l’application appelante.

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

La capture d’écran ci-dessous montre les propriétés de configuration qui doivent être définies. Ces propriétés de configuration sont lues dans le code répertorié ci-dessus pour obtenir access_token

![config](assets/marketoconfig.jfif)

### Configuration

Le code suivant a été utilisé pour créer les propriétés de configuration. Ces propriétés sont spécifiques à votre instance de marketing.

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
1. [Pointez votre navigateur sur configMgr](http://localhost:4502/system/console/configMgr) et recherchez &quot;Configuration du service d’identification de Marketo&quot;.
1. Spécifiez les propriétés appropriées spécifiques à votre instance de marketing.
