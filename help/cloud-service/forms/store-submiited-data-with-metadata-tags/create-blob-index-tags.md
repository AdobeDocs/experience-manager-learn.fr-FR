---
title: Tutoriel pour ajouter des balises de métadonnées spécifiées par l’utilisateur ou l’utilisatrice
description: Créer un envoi personnalisé pour stocker les données de formulaire avec des balises de métadonnées dans Azure
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-14501
duration: 43
exl-id: eb9bcd1b-c86f-4894-bcf8-9c17e74dd6ec
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '115'
ht-degree: 100%

---

# Ajouter des balises d’index d’objet blob

À mesure que les jeux de données s’agrandissent, il peut s’avérer difficile de trouver un objet spécifique dans un océan de données. Les balises d’index d’objet blob offrent des fonctionnalités de gestion et de découverte de données à l’aide d’attributs de balise d’index clé-valeur. Vous pouvez classer et rechercher des objets dans un seul conteneur ou dans tous les conteneurs de votre compte de stockage. Par exemple, la balise d’index d’objet blob _**CustomerType=Platinum**_, où Platinum est la valeur du champ CustomerType.

![index-tags](assets/blob-with-index-tags1.png)
Le code suivant crée la chaîne de balises de données d’index d’objet blob avec ses valeurs correspondantes à partir des données envoyées.

```java
@Override
    public String getMetaDataTags(String submittedFormName,String formPath,Session session,String formData) {

        JsonObject jsonObject = JsonParser.parseString(formData).getAsJsonObject();
        List<String>metaDataTags = new ArrayList<String>();
        metaDataTags.add("formName="+submittedFormName);
        Map< String, String > map = new HashMap< String, String >();
        map.put("path", formPath);
        map.put("1_property", "Searchable");
        map.put("1_property.value","true");
        Query query = queryBuilder.createQuery(PredicateGroup.create(map),session);
        query.setStart(0);
        query.setHitsPerPage(20);
        SearchResult result = query.getResult();
        logger.debug("Get result hits " + result.getHits().size());
        for (Hit hit: result.getHits()) {
            try {
                    logger.debug(hit.getPath());
                    String jsonElementName = (String) hit.getProperties().get("name");
                    String fieldName = hit.getProperties().get("name").toString();
                    if(jsonObject.get(jsonElementName).isJsonArray())
                    {
                        
                        JsonArray arrayOfValues = jsonObject.get(jsonElementName).getAsJsonArray();
                        StringBuilder valuesString = new StringBuilder();
                        for(int j=0;j<arrayOfValues.size();j++)
                        {
                            valuesString.append(arrayOfValues.get(j).getAsString());
                            if(j < arrayOfValues.size() -1)
                            {
                                valuesString.append(" and ");
                            }
                        }

                        
                        metaDataTags.add(fieldName + "=" + valuesString.toString());

                    }
                    else
                    {
                        logger.debug("The searchable field name is " + fieldName + "the json element name is " + jsonElementName);
                        metaDataTags.add(fieldName + "=" + jsonObject.get(jsonElementName).getAsString());
                    }

            } catch (RepositoryException e) {
                throw new RuntimeException(e);
            }
        }
        return String.join("&",metaDataTags);
    }
```

## Étapes suivantes

[Créer un gestionnaire d’envoi personnalisé](./create-custom-submit.md)
