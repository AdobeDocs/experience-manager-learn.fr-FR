---
title: Paramétrage de modèles Sling à partir de HTL
description: Découvrez comment transmettre des paramètres à partir de HTL vers un modèle Sling dans AEM.
version: Experience Manager as a Cloud Service
topic: Development
feature: Sling Model
role: Developer
jira: KT-15923
level: Intermediate, Experienced
exl-id: 5d852617-720a-4a00-aecd-26d0ab77d9b3
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '264'
ht-degree: 100%

---

# Paramétrage de modèles Sling à partir de HTL

Adobe Experience Manager (AEM) offre une infrastructure robuste pour la création d’applications web dynamiques et adaptables. L’une de ses puissantes fonctionnalités est la possibilité de paramétrer des modèles Sling, ce qui améliore leur flexibilité et leur capacité de réutilisation. Ce tutoriel vous guide tout au long de la création d’un modèle Sling paramétré et de son utilisation dans du HTL (HTML Template Language) pour effectuer du rendu de contenu dynamique.

## Script HTL

Dans cet exemple, le script HTL envoie deux paramètres au modèle Sling `ParameterizedModel`. Le modèle manipule ces paramètres dans sa méthode `getValue()` et renvoie le résultat pour l’affichage.

Cet exemple transmet deux paramètres de chaîne, mais vous pouvez transmettre n’importe quel type de valeur ou d’objet au modèle Sling, à condition que le type de champ [Modèle Sling, annoté avec `@RequestAttribute`](#sling-model-implementation), corresponde au type d’objet ou de valeur transmis à partir du HTL.

```html
<sly data-sly-use.myModel="${'com.example.core.models.ParameterizedModel' @ myParameterOne='Hello', myParameterTwo='World'}"
     data-sly-test.isReady="${myModel.isReady()}">

    <p>
        ${myModel.value}
    </p>

</sly>

<sly data-sly-use.placeholderTemplate="core/wcm/components/commons/v1/templates.html"
     data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!isReady}">
</sly>
```

- **Paramétrisation :** l’instruction `data-sly-use` crée une instance de `ParameterizedModel` avec `myParameterOne` et `myParameterTwo`.
- **Rendu conditionnel :** `data-sly-test` vérifie si le modèle est prêt avant d’afficher le contenu.
- **Appel d’espace réservé :** `placeholderTemplate` gère les cas où le modèle n’est pas prêt.

## Implémentation de modèles Sling

Voici comment implémenter le modèle Sling :

```java
package com.example.core.models.impl;

import org.apache.commons.lang3.StringUtils;
import org.apache.sling.api.SlingHttpServletRequest;
import org.osgi.service.component.annotations.Model;
import org.osgi.service.component.annotations.RequestAttribute;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Model(
    adaptables = { SlingHttpServletRequest.class },
    adapters = { ParameterizedModel.class }
)
public class ParameterizedModelImpl implements ParameterizedModel {
    private static final Logger log = LoggerFactory.getLogger(ParameterizedModelImpl.class);

    // Use the @RequestAttribute annotation to inject the parameter set in the HTL.
    // Note that the Sling Model field can be any type, but must match the type of object or value passed from HTL.
    @RequestAttribute
    private String myParameterOne;

    // If the HTL parameter name is different from the Sling Model field name, use the name attribute to specify the HTL parameter name
    @RequestAttribute(name = "myParameterTwo")
    private String mySecondParameter;

    // Do some work with the parameter values
    @Override
    public String getValue() {
        return myParameterOne + " " + mySecondParameter + ", from the parameterized Sling Model!";
    }

    @Override
    public boolean isReady() {
        return StringUtils.isNotBlank(myParameterOne) && StringUtils.isNotBlank(mySecondParameter);
    }
}
```

- **Annotation de modèle :** l’annotation `@Model` désigne cette classe en tant que modèle Sling, adaptable à partir de `SlingHttpServletRequest` et implémentant l’interface `ParameterizedModel`.
- **Attributs de requête :** l’annotation `@RequestAttribute` injecte des paramètres HTL dans le modèle.
- **Méthodes :** `getValue()` concatène les paramètres et `isReady()` vérifie que les paramètres ne sont pas vides.

L’interface `ParameterizedModel` est définie comme suit :

```java
package com.example.core.models;

import org.osgi.annotation.versioning.ConsumerType;

@ConsumerType
public interface ParameterizedModel {
    /**
     * Get an example string value from the model. This value is the concatenation of the two parameters.
     * 
     * @return the value of the model
     */
    String getValue();

    /**
     * Check if the model is ready to be used.
     *
     * @return {@code true} if the model is ready, {@code false} otherwise
     */
    boolean isReady();
}
```

## Exemple de sortie

Avec les paramètres `"Hello"` et `"World"`, le script HTL génère la sortie suivante :

```html
<p>
    Hello World, from the parameterized Sling Model!
</p>
```

Cela illustre comment les modèles Sling paramétrés dans AEM peuvent être affectés par des paramètres d’entrée fournis via HTL.
