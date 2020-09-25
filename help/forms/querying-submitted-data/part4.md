---
title: aem forms avec Schéma et données JSON[Part4]
seo-title: aem forms avec Schéma et données JSON[Part4]
description: Ce didacticiel en plusieurs parties vous explique comment créer un formulaire adaptatif avec le schéma JSON et interroger les données envoyées.
seo-description: Ce didacticiel en plusieurs parties vous explique comment créer un formulaire adaptatif avec le schéma JSON et interroger les données envoyées.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '475'
ht-degree: 0%

---


# Interrogation des données envoyées


L’étape suivante consiste à requête des données envoyées et à afficher les résultats sous forme de tableaux. Pour ce faire, nous utiliserons les logiciels suivants :

[QueryBuilder](https://querybuilder.js.org/) - Composant d&#39;interface utilisateur pour créer des requêtes

[Tableaux](https://datatables.net/)de données : pour afficher les résultats de la requête sous forme de tableaux.

L’interface utilisateur suivante a été créée pour permettre l’interrogation des données envoyées. Seuls les éléments marqués comme requis dans le schéma JSON sont mis à la disposition de la requête. Dans la capture d&#39;écran ci-dessous, nous demandons toutes les soumissions pour lesquelles le pref de livraison est SMS.

L’exemple d’interface utilisateur pour requête des données envoyées n’utilise pas toutes les fonctionnalités avancées disponibles dans QueryBuilder. On vous encourage à les essayer par vous-même.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La version actuelle de ce didacticiel ne prend pas en charge l’interrogation de plusieurs colonnes.

Lorsque vous sélectionnez un formulaire pour exécuter votre requête, un appel de GET est effectué à **/bin/getdatakeysfromschema**. Cet appel de GET renvoie les champs obligatoires associés au schéma des formulaires. Les champs obligatoires sont ensuite renseignés dans la liste déroulante de QueryBuilder pour que vous puissiez créer la requête.

Le fragment de code suivant effectue un appel à la méthode getRequiredColumnsFromSchema du service JSONSchemaOperations. Nous transmettons les propriétés et les éléments requis du schéma à cet appel de méthode. Le tableau renvoyé par cet appel de fonction est ensuite utilisé pour remplir la liste déroulante du créateur de requêtes.

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

Lorsque l&#39;utilisateur clique sur le bouton GetResult, un appel Get est effectué à **&quot;/bin/querydata&quot;**. Nous transmettons la requête générée par l’interface utilisateur de QueryBuilder à la servlet via le paramètre de requête. La servlet transforme ensuite cette requête en requête SQL qui peut être utilisée pour requête de la base de données. Par exemple, si vous recherchez tous les produits nommés &quot;Souris&quot;, la chaîne de requête du créateur de Requêtes sera $.productname = &#39;Souris&#39;. Cette requête sera ensuite convertie en :

SÉLECTIONNEZ * dans aemformswithjson .  formSubmissions où JSON_EXTRACT( formSubmissions .formdata,&quot;$.productName &quot;)= &#39;Souris&#39;

Le résultat de cette requête est ensuite renvoyé pour remplir le tableau dans l’interface utilisateur.

Pour que cet exemple s&#39;exécute sur votre système local, procédez comme suit :

1. [Assurez-vous d&#39;avoir suivi toutes les étapes mentionnées ici.](part2.md)
1. [Importez le fichier Dashboardv2.zip à l’aide d’AEM Package Manager.](assets/dashboardv2.zip) Ce package contient tous les lots nécessaires, les paramètres de configuration, l’envoi personnalisé et les exemples de données de page vers la requête.
1. Création d’un formulaire adaptatif à l’aide de l’exemple de schéma json
1. Configurez le formulaire adaptatif à envoyer à l’action d’envoi personnalisée &quot;customsubmithelpx&quot;.
1. Remplir le formulaire et envoyer
1. Pointez votre navigateur sur [tableau de bord.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Sélectionnez le formulaire et effectuez une requête simple

