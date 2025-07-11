---
title: Créer un modèle Sling pour le composant
description: Créer un modèle Sling pour le composant
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: f4a18f02-61a2-4fa3-bfbb-41bf696cd2a8
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '384'
ht-degree: 100%

---

# Créer un modèle Sling pour le composant

Un modèle Sling dans AEM est un cadre basé sur Java, utilisé pour simplifier le développement de la logique de back-end pour les composants. Il permet aux développeurs et développeuses de mapper des données provenant de ressources AEM (nœuds JCR) avec des objets Java à l’aide d’annotations, offrant ainsi un moyen net et efficace de gérer les données dynamiques pour les composants.
Cette classe, CountriesDropDownImpl, est une implémentation de l’interface CountriesDropDown dans un projet AEM (Adobe Experience Manager). Elle alimente un composant déroulant dans lequel il est possible de sélectionner un pays en fonction du continent sélectionné. Les données de liste déroulante sont chargées dynamiquement à partir d’un fichier JSON stocké dans la gestion des actifs numériques AEM (AEM DAM).

**Champs dans la classe**

* **multiSelect** : indique si la liste déroulante autorise plusieurs sélections.
Injecté à partir des propriétés du composant à l’aide de @ValueMapValue avec la valeur par défaut false.
* **request** : représente la requête HTTP actuelle. Utile pour accéder à des informations spécifiques au contexte.
* **continent** : stocke le continent sélectionné pour la liste déroulante (par exemple, « asie », « europe »).
Injecté à partir de la boîte de dialogue de propriétés du composant, avec la valeur par défaut « asia » si aucune n’est fournie.
* **resourceResolver** : utilisé pour accéder à des ressources dans le référentiel AEM et les manipuler.
* **jsonData** : objet JSONObject qui stocke les données analysées du fichier JSON correspondant au continent sélectionné.

**Méthodes de la classe**

* **getContinent()** Méthode simple pour renvoyer la valeur du champ continent.
Consigne la valeur renvoyée à des fins de débogage.
* **init()** Méthode de cycle de vie annotée avec @PostConstruct, exécutée après la création de la classe et l’injection des dépendances. Construit dynamiquement le chemin d’accès au fichier JSON en fonction de la valeur de continent.
Récupère le fichier JSON à partir de la gestion des actifs numériques AEM à l’aide de resourceResolver.
Adapte le fichier à une ressource, lit son contenu et l’analyse dans un objet JSONObject.
Consigne les erreurs ou les avertissements pendant ce processus.
* **getEnums()** Récupère toutes les clés (codes pays) à partir des données JSON analysées.
Trie les clés par ordre alphabétique et les renvoie sous la forme d’un tableau.
Consigne le nombre de codes pays renvoyés.
* **getEnumNames()** Extrait tous les noms de pays à partir des données JSON analysées.
Trie les noms par ordre alphabétique et les renvoie sous la forme d’un tableau.
Consigne le nombre total de pays et chaque nom de pays récupéré.
* **isMultiSelect()** Renvoie la valeur du champ multiSelect pour indiquer si la liste déroulante autorise plusieurs sélections.



```java
package com.aem.customcorecomponent.core.models.impl;
import com.adobe.cq.export.json.ComponentExporter;
import com.adobe.cq.export.json.ExporterConstants;
import com.adobe.cq.forms.core.components.internal.form.ReservedProperties;
import com.adobe.cq.forms.core.components.util.AbstractOptionsFieldImpl;
import com.aem.customcorecomponent.core.models.CountriesDropDown;
import com.day.cq.dam.api.Asset;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.resource.ResourceResolver;
import org.apache.sling.models.annotations.Default;
import org.apache.sling.models.annotations.Exporter;
import org.apache.sling.models.annotations.Model;
import org.apache.sling.models.annotations.injectorspecific.InjectionStrategy;
import org.apache.sling.models.annotations.injectorspecific.ValueMapValue;
import org.json.JSONObject;
import javax.annotation.PostConstruct;
import javax.inject.Inject;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.*;
import java.util.stream.Collectors;

//@Model(adaptables = SlingHttpServletRequest.class,adapters = CountriesDropDown.class,defaultInjectionStrategy = DefaultInjectionStrategy.OPTIONAL)

@Model(
        adaptables = { SlingHttpServletRequest.class, Resource.class },
        adapters = { CountriesDropDown.class,
                ComponentExporter.class },
        resourceType = { "corecomponent/components/adaptiveForm/countries" })
@Exporter(name = ExporterConstants.SLING_MODEL_EXPORTER_NAME, extensions = ExporterConstants.SLING_MODEL_EXTENSION)

public class CountriesDropDownImpl extends AbstractOptionsFieldImpl implements CountriesDropDown {
    @ValueMapValue(injectionStrategy = InjectionStrategy.OPTIONAL, name = ReservedProperties.PN_MULTISELECT)
    @Default(booleanValues = false)
    protected boolean multiSelect;

    private static final Logger logger = LoggerFactory.getLogger(CountriesDropDownImpl.class);
    @Inject
    SlingHttpServletRequest request;

    @Inject
    @Default(values = "asia")
    public String continent;
    @Inject
    private ResourceResolver resourceResolver;
    private JSONObject jsonData;
    public String getContinent()
    {
        logger.info("Returning continent");
        return continent;
    }
    @PostConstruct
    public void init() {

        String jsonPath = "/content/dam/corecomponent/" + getContinent() + ".json"; // Update path as needed
        logger.info("Initializing JSON data for continent: {}", getContinent());

        try {
            // Fetch the JSON resource
            Resource jsonResource = resourceResolver.getResource(jsonPath);
            if (jsonResource != null) {
                // Adapt the resource to an Asset
                Asset asset = jsonResource.adaptTo(Asset.class);
                if (asset != null) {
                    // Get the original rendition and parse it as JSON
                    try (InputStream inputStream = asset.getOriginal().adaptTo(InputStream.class);
                         BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))) {

                        String jsonString = reader.lines().collect(Collectors.joining());
                        jsonData = new JSONObject(jsonString);
                        logger.info("Successfully loaded JSON data for path: {}", jsonPath);
                    }
                } else {
                    logger.warn("Failed to adapt resource to Asset at path: {}", jsonPath);
                }
            } else {
                logger.warn("Resource not found at path: {}", jsonPath);
            }
        } catch (Exception e) {
            logger.error("An error occurred while initializing JSON data for path: {}", jsonPath, e);
        }
    }

    @Override
    public Object[] getEnums() {
        Set<String> keySet = jsonData.keySet();


// Convert the set of keys to a sorted array
        String[] countryCodes = keySet.toArray(new String[0]);
        Arrays.sort(countryCodes);

        logger.debug("Returning sorted country codes: " + countryCodes.length);

        return countryCodes;


    }

    @Override
    public String[] getEnumNames() {
        String[] countries = new String[jsonData.keySet().size()];
        logger.info("Fetching countries - Total number: " + countries.length);

// Populate the array with country names
        int index = 0;
        for (String code : jsonData.keySet()) {
            String country = jsonData.getString(code);
            logger.debug("Retrieved country: " + country);
            countries[index++] = country;
        }

// Sort the array alphabetically
        Arrays.sort(countries);
        logger.debug("Returning " + countries.length + " sorted countries");

        return countries;

    }

    @Override
    public Boolean isMultiSelect() {
        return multiSelect;
    }
}
```

## Étapes suivantes

[Créer, déployer et tester](./build.md)
