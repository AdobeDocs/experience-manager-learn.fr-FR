---
title: AEM Forms avec schéma et données JSON [Partie3]
seo-title: AEM Forms avec schéma et données JSON [Partie3]
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes nécessaires à la création d’un formulaire adaptatif avec un schéma JSON et à l’interrogation des données envoyées.
seo-description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes nécessaires à la création d’un formulaire adaptatif avec un schéma JSON et à l’interrogation des données envoyées.
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '285'
ht-degree: 1%

---


# Stockage du schéma JSON dans la base de données {#storing-json-schema-in-database}


Pour pouvoir interroger les données envoyées, nous devrons stocker le schéma JSON associé au formulaire envoyé. Le schéma JSON sera utilisé dans le Query Builder pour créer la requête.

Lorsqu’un formulaire adaptatif est envoyé, nous vérifions si le schéma JSON associé se trouve dans la base de données. Si le schéma JSON n’existe pas, nous récupérons le schéma JSON et le stockons dans le tableau approprié. Nous associons également le nom du formulaire au schéma JSON. La capture d’écran suivante montre le tableau dans lequel les schémas JSON sont stockés.

![jsonschema](assets/jsonschemas.gif)

```java
public String getJSONSchema(String afPath) {
  // TODO Auto-generated method stub
  afPath = afPath.replaceAll("/content/dam/formsanddocuments/", "/content/forms/af/");
  Resource afResource = getResolver.getServiceResolver().getResource(afPath + "/jcr:content/guideContainer");
  javax.jcr.Node resNode = afResource.adaptTo(Node.class);
  String schemaNode = null;
  try {
   schemaNode = resNode.getProperty("schemaRef").getString();
  } catch (ValueFormatException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (PathNotFoundException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } catch (RepositoryException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  if (schemaNode.startsWith("/content/dam")) {
   log.debug("The schema is in the dam");
   afResource = getResolver.getServiceResolver()
     .getResource(schemaNode + "/jcr:content/renditions/original/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }
  if (schemaNode.startsWith("/assets")) {
   afResource = getResolver.getServiceResolver()
     .getResource(afPath + "/jcr:content/guideContainer/" + schemaNode + "/jcr:content");
   resNode = afResource.adaptTo(Node.class);
   InputStream jsonSchemaStream = null;
   try {
    jsonSchemaStream = resNode.getProperty("jcr:data").getBinary().getStream();
    Charset charset = StandardCharsets.UTF_8;
    String jasonSchemaString = IOUtils.toString(jsonSchemaStream, charset);
    log.debug("The Schema is " + jasonSchemaString);
    return jasonSchemaString;
   } catch (ValueFormatException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (PathNotFoundException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (RepositoryException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   } catch (IOException e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
   }
  }

  return null;

 }
```

>[!NOTE]
>
>Lors de la création d’un formulaire adaptatif, vous pouvez utiliser le schéma JSON qui se trouve dans le référentiel ou charger un schéma JSON. Le code ci-dessus fonctionne pour les deux cas.

Le schéma récupéré est stocké dans la base de données à l’aide des opérations JDBC standard. Le code suivant insère le schéma dans la base de données :

```java
public void insertJsonSchema(JSONObject jsonSchema, String afForm) {
  log.debug("$$$$ in insert Schema" + afForm);
  log.debug("$$$$$ The jsonSchema is  " + jsonSchema);
  Connection con = getConnection();
  log.debug("$$$$ got connection is insertJsonSchema");
  String insertTableSQL = "INSERT INTO leads.jsonschemas(jsonschema,formname) VALUES(?,?)";
  PreparedStatement pstmt = null;
  try {

   // org.json.JSONObject jsonSchemaObj = new
   // org.json.JSONObject(jsonSchema);
   pstmt = con.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonSchema.toString());
   pstmt.setString(2, afForm);
   log.debug("Executing the insert  json schema statment  " + pstmt.executeUpdate());
   con.commit();
  } catch (SQLException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  } finally {
   if (con != null) {
    try {
     con.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }

 }
```

Pour résumer, nous avons fait ce qui suit jusqu’à présent :

* Création d’un formulaire adaptatif basé sur un schéma JSON
* Si le formulaire est envoyé la première fois, le schéma JSON associé au formulaire est stocké dans la base de données.
* Nous stockons les données liées du formulaire adaptatif dans la base de données.

Les étapes suivantes consistent à utiliser QueryBuilder pour afficher les champs à rechercher en fonction du schéma JSON.


