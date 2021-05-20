---
title: AEM Forms avec schéma et données JSON [Partie4]
seo-title: AEM Forms avec schéma et données JSON [Partie4]
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
source-wordcount: '478'
ht-degree: 0%

---


# Requête sur les données envoyées


L’étape suivante consiste à interroger les données envoyées et à afficher les résultats sous forme de tableau. Pour ce faire, nous utiliserons les logiciels suivants :

[QueryBuilder](https://querybuilder.js.org/)  : composant d’interface utilisateur pour créer des requêtes.

[Tableaux de données](https://datatables.net/) : pour afficher les résultats de la requête sous forme de tableau.

L’interface utilisateur suivante a été créée pour permettre l’interrogation des données envoyées. Seuls les éléments marqués comme requis dans le schéma JSON sont disponibles pour la requête sur . Dans la capture d&#39;écran ci-dessous, nous demandons que toutes les soumissions pour lesquelles le deliverypref est un SMS soient envoyées.

L’exemple d’interface utilisateur pour interroger les données envoyées n’utilise pas toutes les fonctionnalités avancées disponibles dans QueryBuilder. Nous vous encourageons à les essayer vous-même.

![querybuilder](assets/querybuilderui.gif)

>[!NOTE]
>
>La version actuelle de ce tutoriel ne prend pas en charge l’interrogation de plusieurs colonnes.

Lorsque vous sélectionnez un formulaire pour effectuer votre requête, un appel de GET est effectué à **/bin/getdatakeysfromschema**. Cet appel de GET renvoie les champs obligatoires associés au schéma des formulaires. Les champs obligatoires sont ensuite renseignés dans la liste déroulante de QueryBuilder pour que vous puissiez créer la requête.

Le fragment de code suivant effectue un appel à la méthode getRequiredColumnsFromSchema du service JSONSchemaOperations. Nous transmettons les propriétés et les éléments requis du schéma à cet appel de méthode. Le tableau renvoyé par cet appel de fonction est ensuite utilisé pour remplir la liste déroulante Query Builder

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

Lorsque l’utilisateur clique sur le bouton GetResult, un appel Get est effectué à **&quot;/bin/querydata&quot;**. Nous transmettons la requête créée par l’interface utilisateur de QueryBuilder au servlet via le paramètre de requête . Le servlet convertit ensuite cette requête en requête SQL qui peut être utilisée pour interroger la base de données. Par exemple, si vous recherchez tous les produits nommés &quot;Souris&quot;, la chaîne de requête de Query Builder sera $.productname = &quot;Souris&quot;. Cette requête sera ensuite convertie en :

SELECT * depuis aemformswithjson .  formenvois où JSON_EXTRACT( formsubmission .formdata,&quot;$.productName &quot;)= &#39;Souris&#39;

Le résultat de cette requête est alors renvoyé pour renseigner le tableau dans l’interface utilisateur.

Pour que cet exemple s’exécute sur votre système local, procédez comme suit :

1. [Assurez-vous d’avoir suivi toutes les étapes mentionnées ici](part2.md)
1. [Importez le tableau de bord v2.zip à l’aide d’AEM Gestionnaire de modules.](assets/dashboardv2.zip) Ce package contient tous les lots nécessaires, les paramètres de configuration, l’envoi personnalisé et des exemples de page pour interroger les données.
1. Création d’un formulaire adaptatif à l’aide de l’exemple de schéma json
1. Configuration du formulaire adaptatif pour l’envoi d’une action d’envoi personnalisée &quot;customsubmithelpx&quot;
1. Remplissez le formulaire et envoyez-le.
1. Pointez votre navigateur sur [dashboard.html](http://localhost:4502/content/AemForms/dashboard.html)
1. Sélectionnez le formulaire et effectuez une requête simple.

