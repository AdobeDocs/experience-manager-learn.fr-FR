---
title: AEM Forms avec schéma et données JSON [Partie 4]
description: Il s’agit d’un tutoriel en plusieurs parties consacré à la création de formulaires adaptatifs avec les schémas JSON et à l’interrogation des données envoyées.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: a8d8118d-f4a1-483f-83b4-77190f6a42a4
duration: 135
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '432'
ht-degree: 100%

---

# Interroger les données envoyées


L’étape suivante consiste à interroger les données envoyées et à afficher les résultats sous forme de tableau. Pour ce faire, nous utilisons les logiciels suivants :

[QueryBuilder](https://querybuilder.js.org/) : composant d’interface utilisateur pour créer des requêtes.

[DataTables](https://datatables.net/) : pour afficher les résultats de requête sous forme de tableau.

L’interface utilisateur suivante a été créée pour permettre l’interrogation des données envoyées. Seuls les éléments marqués comme obligatoires dans le schéma JSON sont disponibles pour les requêtes. Dans la copie d’écran ci-dessous, nous demandons toutes les soumissions dont la deliverypref est un SMS.

L’exemple d’interface utilisateur pour interroger les données envoyées n’utilise pas toutes les fonctionnalités avancées disponibles dans QueryBuilder. Nous vous encourageons à les essayer vous-même.

![QueryBuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La version actuelle de ce tutoriel ne prend pas en charge l’interrogation de plusieurs colonnes.

Lorsque vous sélectionnez un formulaire pour effectuer votre requête, un appel GET est effectué à **/bin/getdatakeysfromschema**. Cet appel GET renvoie les champs obligatoires associés au schéma des formulaires. Les champs obligatoires sont ensuite renseignés dans la liste déroulante de QueryBuilder pour que vous puissiez créer la requête.

L’extrait de code suivant appelle la méthode getRequiredColumnsFromSchema du service JSONSchemaOperations. Nous transmettons les propriétés et les éléments requis du schéma à cet appel de méthode. Le tableau renvoyé par cet appel de fonction est ensuite utilisé pour remplir la liste déroulante de QueryBuilder.

```java
public JSONArray getData(String formName) throws SQLException, IOException {

  org.json.JSONArray arrayOfDataKeys = new org.json.JSONArray();
  JSONObject jsonSchema = jsonSchemaOperations.getJSONSchemaFromDataBase(formName);
  Map<String, String> refKeys = new HashMap<String, String>();

  try {
   JSONObject properties = jsonSchema.getJSONObject("properties");
   JSONArray requiredFields = jsonSchema.has("required") ? jsonSchema.getJSONArray("required") : null;
   jsonSchemaOperations.getRequiredColumnsFromSchema(properties, arrayOfDataKeys, "", jsonSchema, refKeys,
     requiredFields);
  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }
  return arrayOfDataKeys;

 }
```

Lorsque vous cliquez sur le bouton GetResult, un appel Get est envoyé à **« /bin/querydata »**. Nous transmettons la requête créée par l’interface utilisateur de QueryBuilder au servlet via le paramètre de requête. Le servlet convertit ensuite cette requête en requête SQL qui peut être utilisée pour interroger la base de données. Par exemple, si vous recherchez tous les produits nommés « Mouse » (souris), la chaîne de requête de Query Builder est `$.productname = 'Mouse'`. Cette requête sera ensuite convertie en :

SELECT &#42; d’aemformswithjson .  formsubmissions où JSON_EXTRACT( formsubmissions .formdata,&quot;$.productName &quot;)= &#39;Mouse&#39;

Le résultat de cette requête est alors renvoyé pour renseigner le tableau dans l’interface utilisateur.

Pour que cet exemple s’exécute sur votre système local, procédez comme suit :

1. [Assurez-vous d’avoir suivi toutes les étapes mentionnées ici.](part2.md)
1. [Importez le dossier Dashboardv2.zip à l’aide du gestionnaire de packages AEM.](assets/dashboardv2.zip)Ce package contient tous les lots nécessaires, les paramètres de configuration, l’envoi personnalisé et l’exemple de page pour interroger les données.
1. Créez un formulaire adaptatif à l’aide de l’exemple de schéma json.
1. Configurez le formulaire adaptatif pour envoyer une action d’envoi personnalisée « customsubmithelpx ».
1. Remplissez le formulaire et envoyez-le.
1. Pointez votre navigateur sur [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html).
1. Sélectionnez le formulaire et effectuez une requête simple.
