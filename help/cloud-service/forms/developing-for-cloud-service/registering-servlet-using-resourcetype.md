---
title: Enregistrer un servlet à l’aide du type de ressource
description: Mapper un servlet à un type de ressource dans AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 108
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '378'
ht-degree: 100%

---

# Présentation

La liaison de servlets par chemins d’accès présente plusieurs inconvénients par rapport à la liaison par types de ressources, à savoir :

* Les servlets liés au chemin d’accès ne peuvent pas faire l’objet d’un contrôle d’accès à l’aide des listes ACL par défaut du référentiel JCR.
* Les servlets liés au chemin d’accès ne peuvent être enregistrés que dans un chemin et non dans un type de ressource (c’est-à-dire qu’il n’y a pas de gestion des suffixes).
* Si un servlet lié au chemin d’accès n’est pas actif, par exemple si le bundle est manquant ou n’est pas démarré, une requête POST peut entraîner des résultats inattendus. En général, cela crée un nœud à l’adresse `/bin/xyz` qui chevauche ensuite la liaison du chemin des servlets.
Le mappage n’est pas transparent pour un développeur ou une développeuse qui ne regarde que le référentiel.
Compte tenu de ces inconvénients, il est fortement recommandé de lier les servlets aux types de ressources plutôt qu’aux chemins d’accès.

## Créer un servlet

Lancez votre projet aem-banking dans IntelliJ. Créez un servlet appelé GetFieldChoices sous le dossier servlets, comme illustré dans la copie d’écran ci-dessous.
![choices](assets/fetchchoices.png)

## Exemple de servlet

Le servlet suivant est lié au type de ressource Sling : _**azure/fetchchoice**_.



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## Créer des ressources dans CRX

* Connectez-vous à votre SDK AEM local.
* Créez une ressource nommée `fetchchoices` (vous pouvez nommer ce nœud quand vous le souhaitez) de type `cq:Page` sous le nœud de contenu.
* Enregistrez vos modifications.
* Créez un nœud appelé `jcr:content` de type `cq:PageContent` et enregistrez les modifications.
* Ajoutez les propriétés suivantes au nœud `jcr:content` :

| Nom de la propriété | Valeur de la propriété |
|--------------------|--------------------|
| jcr:title | Servlets utilitaires |
| sling:resourceType | `azure/fetchchoices` |


La valeur `sling:resourceType` doit correspondre à resourceTypes=&quot;azure/fetchchoice spécifié dans le servlet.

Vous pouvez désormais appeler votre servlet en demandant la ressource avec `sling:resourceType` = `azure/fetchchoices` à son chemin d’accès complet, avec tous les sélecteurs ou extensions enregistrés dans le servlet Sling.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

Le chemin `/content/fetchchoices/jcr:content` est le chemin d’accès de la ressource et l’extension `.json` correspond à ce qui est spécifié dans le servlet.

## Synchroniser votre projet AEM

1. Ouvrez le projet AEM dans votre éditeur favori. J’ai choisi intelliJ.
1. Créez un fichier appelé `fetchchoices` sous `\aem-banking-application\ui.content\src\main\content\jcr_root\content`.
1. Cliquez avec le bouton droit sur le dossier `fetchchoices` et sélectionnez `repo | Get Command` (cet élément de menu est configuré dans un chapitre précédent de ce tutoriel).

Cela devrait synchroniser ce nœud d’AEM à votre projet AEM local.

La structure de votre projet AEM doit ressembler à celle-ci :
![resource-resolver](assets/mapping-servlet-resource.png)
Mettez à jour le fichier filter.xml dans le dossier aem-banking-application\ui.content\src\main\content\META-INF\vault avec l’entrée suivante :

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

Vous pouvez désormais transmettre vos modifications à un environnement AEM as a Cloud Service à l’aide de Cloud Manager.

## Étapes suivantes

[Activer les composants du portail Formulaires](./forms-portal-components.md)
