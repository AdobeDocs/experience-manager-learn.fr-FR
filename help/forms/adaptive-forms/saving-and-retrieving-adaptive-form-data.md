---
title: Enregistrer et récupérer des données du formulaire adaptatif
description: Enregistrez et récupérez des données du formulaire adaptatif à partir de la base de données. Cette fonctionnalité permet aux utilisateurs et aux utilisatrices d’enregistrer le formulaire et de continuer à le remplir ultérieurement.
feature: Adaptive Forms
topic: Development
role: Developer
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
last-substantial-update: 2019-06-09T00:00:00Z
duration: 711
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '600'
ht-degree: 100%

---


# Enregistrer et récupérer des données du formulaire adaptatif

Cet article décrit les étapes à suivre pour enregistrer et récupérer les données du formulaire adaptatif à partir de la base de données. Les données du formulaire adaptatif sont stockées dans une base de données MySQL. À un niveau élevé, ce cas d’utilisation requiert de suivre les étapes suivantes :

* [Configurer la source de données](#Configure-Data-Source)
* [Créer un servlet pour écrire des données dans la base de données](#create-servlet)
* [Créer un service OSGi pour récupérer les données stockées](#create-osgi-service)
* [Créer une bibliothèque cliente](#create-client-library)
* [Créer un modèle de formulaire adaptatif et un composant de page](#form-template-and-page-component)
* [Démonstration des fonctionnalités](#capability-demo)
* [Déployer sur votre serveur](#deploy-on-your-server)

## Configurer la source de données {#Configure-Data-Source}

La source de données mise en pool de la connexion Apache Sling est configurée pour pointer vers la base de données qui va être utilisée pour stocker les données du formulaire adaptatif. La capture d’écran suivante montre la configuration de mon instance. Les propriétés suivantes peuvent être copiées et collées :

* `Datasource Name:aemformstutorial` - Il s’agit du nom utilisé dans mon code.

* `JDBC Driver Class:com.mysql.jdbc.Driver`

* `JDBC Connection URL:jdbc:mysql://localhost:3306/aemformstutorial`

![connectionpool](assets/storingdata.PNG)

### Créer un servlet {#create-servlet}

Voici le code du servlet qui insère/met à jour les données du formulaire adaptatif dans la base de données. La source de données mise en pool de la connexion Apache Sling est configurée à l’aide du ConfigMgr d’AEM et elle est référencée à la ligne 26. Le reste du code est assez simple. Le code insère une nouvelle ligne dans la base de données ou met à jour une ligne existante. Les données du formulaire adaptatif stockées sont associées à un GUID. Le même GUID est ensuite utilisé pour mettre à jour les données de formulaire.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
import javax.sql.DataSource;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
                log.debug("Executing the insert statment  " + pstmt.executeUpdate());
                c.commit();
                 
      
            } catch (SQLException e) {
      
                log.error("Getting errors", e);
            } finally {
                if (pstmt != null) {
                    try {
                        pstmt.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
                if (c != null) {
                    try {
                        c.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
            return guid;
     
         
    }
     public String insertData(String afdata) {
            log.debug("### Insert Data #### The json object I got to insert was " + afdata);
            String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
            UUID uuid = UUID.randomUUID();
            String randomUUIDString = uuid.toString();
            log.debug("The query is " + insertTableSQL);
            Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(insertTableSQL);
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
                log.debug("Executing the insert statment  " + pstmt.executeUpdate());
                c.commit();
                 
      
            } catch (SQLException e) {
      
                log.error("Getting errors", e);
            } finally {
                if (pstmt != null) {
                    try {
                        pstmt.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
                if (c != null) {
                    try {
                        c.close();
                    } catch (SQLException e) {
                        // TODO Auto-generated catch block
                        e.printStackTrace();
                    }
                }
            }
            return randomUUIDString;
        }
      
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.error("not able to get connection ", e);
            }
            return null;
        }
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

} catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
 
}
```

## Créer un service OSGi pour récupérer des données {#create-osgi-service}

Le code suivant a été écrit pour récupérer les données stockées du formulaire adaptatif. Une requête simple est utilisée pour récupérer les données du formulaire adaptatif associées à un GUID donné. Les données récupérées sont ensuite renvoyées à l’application qui effectue l’appel. La source de données créée à la première étape mentionnée dans ce code.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## Créer une bibliothèque cliente {#create-client-library}

La bibliothèque cliente AEM gère tout le code JavaScript côté client. Pour cet article, j’ai créé un code JavaScript simple pour récupérer les données de formulaire adaptatif à l’aide de l’API GuideBridge. Une fois les données du formulaire adaptatif récupérées, l’appel POST est effectué au servlet pour insérer ou mettre à jour les données du formulaire adaptatif dans la base de données. La fonction getALLUrlParams renvoie les paramètres de l’URL. Elle est utilisée lorsque vous souhaitez mettre à jour les données. Le reste des fonctionnalités est géré dans le code associé à l’événement de clic de la classe .savebutton. Si le paramètre GUID est présent dans l’URL, nous devons effectuer l’opération de mise à jour. Dans le cas contraire, il s’agit d’une opération d’insertion.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## Créer un modèle de formulaire adaptatif et un composant de page {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=12&learn=on)

### Démonstration de la fonctionnalité {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=12&learn=on)

#### Déployer sur votre serveur {#deploy-on-your-server}

Pour tester cette fonctionnalité sur votre instance AEM Forms, procédez comme suit :

* [Téléchargez et décompressez le fichier DemoAssets.zip sur votre système local.](assets/demoassets.zip)
* Déployez et démarrez les lots techmarketingdemos.jar et mysqldriver.jar à l’aide de la console web Felix.
***Importez aemformstutorial.sql à l’aide de MYSQL Workbench. Vous créez ainsi le schéma et les tableaux nécessaires dans votre base de données.
* Importez StoreAndRetrieve.zip à l’aide du gestionnaire de packages AEM. Ce package contient le modèle de formulaire adaptatif, la bibliothèque cliente du composant de page et un exemple de configuration de formulaire adaptatif et de source de données.
* Connectez-vous à ConfigMgr. Recherchez « Apache Sling Connection Pooled DataSource » (source de données mise en pool de la connexion Apache Sling). Ouvrez l’entrée de la source de données associée à aemformstutorial et saisissez le nom d’utilisateur ou d’utilisatrice et le mot de passe spécifiques à votre instance de base de données.
* Ouvrez le formulaire adaptatif.
* Renseignez certains détails et cliquez sur le bouton « Enregistrer et continuer plus tard ».
* Vous devriez recevoir une URL contenant un GUID.
* Copiez l’URL et collez-la dans un nouvel onglet du navigateur.
* Le formulaire adaptatif devrait être renseigné avec les données de l’étape précédente.**
