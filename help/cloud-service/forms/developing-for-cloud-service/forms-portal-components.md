---
title: Activation des composants AEM Forms Portal
description: Création d’un portail AEM Forms à l’aide de composants principaux
solution: Experience Manager
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Core Components
kt: 10373
exl-id: ab01573a-e95f-4041-8ccf-16046d723aba
source-git-commit: eecc275e38390b9330464c8ac0750efa2c702c82
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 15%

---

# Composants du portail Forms

AEM Forms fournit les composants de portail suivants prêts à l’emploi :

**Search &amp; Lister**: Ce composant vous permet de répertorier les formulaires du référentiel de formulaires sur votre page de portail et fournit des options de configuration pour répertorier les formulaires selon des critères spécifiés.

**Brouillons et envois**: Tandis que le composant Search &amp; Lister affiche les formulaires rendus publics par l’auteur Forms, le composant Drafts &amp; Submissions (Brouillons et envois) affiche les formulaires enregistrés en tant que brouillons en vue de les compléter ultérieurement et les formulaires envoyés. Ce composant fournit une expérience personnalisée à tout utilisateur connecté.

**Lien**: Ce composant permet de créer un lien vers un formulaire n’importe où sur la page.

## Activer les composants du Portail Formulaires

Lancez IntelliJ et ouvrez le projet BankingApplication créé dans la [étape précédente.](./getting-started.md) Développez ui.apps->src->main->content->jcr_root->apps.bankingapplication->components .

Pour utiliser n’importe quel composant principal (y compris les composants de portail prêts à l’emploi) dans un site Adobe Experience Manager (AEM), vous devez créer un composant proxy et l’activer pour votre site.
Le nouveau composant proxy doit pointer vers le composant de formulaires prêts à l’emploi, de sorte qu’il hérite de tout d’eux. Pour ce faire, modifiez le paramètre resourceSuperType dans le fichier content.xml du composant proxy. Dans le fichier content.xml, nous spécifions également le titre et le groupe de composants.
>[!NOTE]
>
> Vous pouvez créer un super-type de ressource pour chacun des [ces composants d’ici](https://github.com/adobe/aem-core-forms-components/tree/master/ui.apps/src/main/content/jcr_root/apps/core/fd/components/formsportal)


### Brouillons et envois

Effectuez une copie d’un composant existant (par exemple `button`) et donnez-lui le nom _brouillons et envois_.
![brouillons et envois](assets/forms-portal-components2.png)
Remplacez le contenu de la fonction `.content.xml` avec le code XML suivant :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Drafts And Submissions"
          sling:resourceSuperType="core/fd/components/formsportal/draftsandsubmissions/v1/draftsandsubmissions"
          componentGroup="BankingApplication - Content"/>
```

### Recherche et énumérateur

Effectuez une copie du composant Bouton et renommez-le en _searchandlister_.
Remplacez le contenu de la fonction `.content.xml` avec le code XML suivant :


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Search And Lister"
          sling:resourceSuperType="core/fd/components/formsportal/searchlister/v1/searchlister"
          componentGroup="BankingApplication - Content"/>
```

### Composant Link

Effectuez une copie du composant Bouton et renommez-le en _link_.
Remplacez le contenu de la fonction `.content.xml` avec le code XML suivant :


```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
          jcr:primaryType="cq:Component"
          jcr:title="Link to Adaptive Form"
          sling:resourceSuperType="core/fd/components/formsportal/link/v2/link"
          componentGroup="BankingApplication - Content"/>
```

Une fois votre projet déployé, vous devez pouvoir utiliser ces composants dans votre page AEM pour créer Forms Portal.

## Étapes suivantes

[Inclure la configuration des services cloud](./azure-storage-fdm.md)
