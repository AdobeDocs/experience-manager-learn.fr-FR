---
title: Utiliser un modèle Sightly pour afficher les données de boîte de réception
description: Ajouter des colonnes personnalisées pour afficher des données supplémentaires du workflow à l’aide d’un modèle Sightly
feature: Adaptive Forms
doc-type: article
version: 6.5
jira: KT-5830
topic: Development
role: Developer
level: Experienced
exl-id: d09b46ed-3516-44cf-a616-4cb6e9dfdf41
duration: 103
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '256'
ht-degree: 100%

---

# Utiliser un modèle Sightly pour afficher les données de boîte de réception

Vous pouvez utiliser un modèle Sightly pour formater les données à afficher dans les colonnes de la boîte de réception. Dans cet exemple, nous allons afficher les icônes de l’interface utilisateur Coral UI en fonction de la valeur de la colonne des revenus. La copie d’écran suivante montre l’utilisation des icônes dans la colonne des revenus.
![income-icons](assets/income-column.PNG)

[Le modèle Sightly](assets/sightly-template.zip) utilisé pour afficher les icônes d’interface utilisateur Coral UI personnalisées est fourni dans cet article.

## Modèle Sightly

Voici le modèle Sightly. Le code du modèle affiche une icône en fonction des revenus. Les icônes sont disponibles dans la [bibliothèque d’icônes de l’interface utilisateur Coral UI](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) fournie avec AEM.

```java
<template data-sly-template.incomeTemplate="${@ item}>">
    <td is="coral-table-cell" class="payload-income-cell">
         <div data-sly-test="${(item.workflowMetadata && item.workflowMetadata.income)}" data-sly-set.income ="${item.workflowMetadata.income}">
                 <coral-icon icon="confidenceOne" size="M" data-sly-test="${income >=0 && income <10000}"></coral-icon>
                 <coral-icon icon="confidenceTwo" size="M" data-sly-test="${income >=10000 && income <100000}"></coral-icon>
                 <coral-icon icon="confidenceThree" size="M" data-sly-test="${income >=100000 && income <500000}"></coral-icon>
                 <coral-icon icon="confidenceFour" size="M" data-sly-test="${income >=500000}"></coral-icon>
          </div>
    </td>
</template>
```

## Mise en œuvre du service

Le code suivant correspond à la mise en œuvre du service pour l’affichage de la colonne des revenus.

La ligne 12 associe la colonne au modèle Sightly.

```java
import java.util.Map;
import org.osgi.service.component.annotations.Component;
import com.adobe.cq.inbox.ui.InboxItem;
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

@Component(service = ColumnProvider.class, immediate = true)
public class IncomeProvider implements ColumnProvider {
@Override
public Column getColumn() {

return new Column("income", "Income", String.class.getName(),"inbox/customization/column-templates.html", "incomeTemplate");
}

@Override
public Object getValue(InboxItem inboxItem) {
Object val = null;

Map workflowMetadata = inboxItem.getWorkflowMetadata();

if (workflowMetadata != null && workflowMetadata.containsKey("income"))
    val = workflowMetadata.get("income");

return val;
}
}
```

## Tester sur votre serveur

>[!NOTE]
>
>Cet article suppose que vous avez installé l’[exemple de workflow](assets/review-workflow.zip) et l’[exemple de formulaire](assets/snap-form.zip) de l’[article précédent](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.html?lang=fr) dans cette série.

* [Connectez-vous à CRX en tant qu’administrateur ou administratrice](http://localhost:4502/crx/de/index.jsp).
* [Importez le modèle Sightly.](assets/sightly-template.zip)
* [Connectez-vous à la console web AEM](http://localhost:4502/system/console/bundles).
* [Déployez et démarrez le lot de personnalisation de la boîte de réception.](assets/income-column-customization.jar)
* [Ouvrez votre boîte de réception](http://localhost:4502/aem/inbox).
* Ouvrez le contrôle d’administration en cliquant sur la vue Liste en regard du bouton Créer.
* Ajoutez une colonne des revenus à la boîte de réception et enregistrez vos modifications.
* [Prévisualisez le formulaire](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled).
* Sélectionnez l’_état civil_ et envoyez le formulaire.
* [Affichez la boîte de réception](http://localhost:4502/aem/inbox).

L’envoi du formulaire déclenche le workflow et une tâche est assignée à l’administrateur ou à l’administratrice. L’icône appropriée devrait s’afficher sous la colonne des revenus.
