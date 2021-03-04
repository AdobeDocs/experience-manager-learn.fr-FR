---
title: Personnalisation de la boîte de réception
description: Ajouter des colonnes personnalisées pour afficher des données supplémentaires du flux de travail à l’aide d’un modèle proche
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Développement
role: Développeur
level: Expérience
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '296'
ht-degree: 9%

---

# Utilisation d’un modèle de rapport pour afficher les données de la boîte de réception

Vous pouvez utiliser un modèle de rapport afin de formater les données à afficher dans les colonnes de la boîte de réception. Dans cet exemple, nous allons afficher des icônes de suie-corail en fonction de la valeur de la colonne de revenu. La capture d&#39;écran suivante montre l&#39;utilisation d&#39;icônes dans la colonne de revenu
![icônes de revenu](assets/income-column.PNG)

[Le ](assets/sightly-template.zip) modèle de rapport serré utilisé pour afficher les icônes d’interface de corail personnalisées est fourni dans le cadre de cet article.

## Modèle direct

Voici le modèle de rapport. Le code du modèle affiche une icône en fonction du revenu. Les icônes sont disponibles dans le cadre de la [bibliothèque d’icônes d’interface utilisateur de corail](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) fournie avec AEM.

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

## Mise en oeuvre du service

Le code suivant est la mise en oeuvre du service pour l&#39;affichage de la colonne de revenu.

La ligne 12 associe la colonne au modèle de rapport

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
>Cet article suppose que vous avez installé l&#39;[exemple de flux de travaux](assets/review-workflow.zip) et [exemple de formulaire](assets/snap-form.zip) de l&#39;[article précédent](https://docs.adobe.com/content/help/en/experience-manager-learn/forms/inbox-customization/add-married-column.md) de cette série.

* [Connexion à crx en tant qu’utilisateur administrateur](http://localhost:4502/crx/de/index.jsp)
* [importer un modèle de rapport](assets/sightly-template.zip)
* [Connexion à AEM console Web](http://localhost:4502/system/console/bundles)
* [Déploiement et début du lot de personnalisation de la boîte de réception](assets/income-column-customization.jar)
* [Ouvrez votre boîte de réception](http://localhost:4502/aem/inbox)
* Ouvrez le contrôle d’administration en cliquant sur la Vue de Liste en regard du bouton Créer
* Ajouter la colonne de revenu dans la boîte de réception et enregistrer vos modifications
* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Sélectionnez l&#39;_état civil_ et envoyez le formulaire.
* [Boîte de réception vue](http://localhost:4502/aem/inbox)

L’envoi du formulaire déclenche le processus et une tâche est attribuée à l’utilisateur &quot;administrateur&quot;. Vous devriez voir l&#39;icône appropriée sous la colonne de revenu
