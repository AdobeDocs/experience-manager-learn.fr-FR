---
title: Personnalisation de la boîte de réception
description: Ajout de colonnes personnalisées pour afficher des données additionnelles du workflow à l’aide d’un modèle de rapport
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5.5
kt: 5830
topic: Développement
role: Developer
level: Experienced
source-git-commit: 7200601c1b59bef5b1546a100589c757f25bf365
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 9%

---

# Utilisation d’un modèle de rapport pour afficher les données de boîte de réception

Vous pouvez utiliser un modèle simple pour mettre en forme les données à afficher dans les colonnes de la boîte de réception. Dans cet exemple, nous allons afficher les icônes de l’interface utilisateur coral en fonction de la valeur de la colonne des revenus. La capture d’écran suivante montre l’utilisation des icônes dans la colonne des revenus
![revenu-icons](assets/income-column.PNG)

[Le ](assets/sightly-template.zip) modèle Sightly utilisé pour afficher les icônes d’interface utilisateur coral personnalisées est fourni dans le cadre de cet article.

## Modèle Sightly

Voici le modèle Sightly. Le code du modèle affiche une icône en fonction des revenus. Les icônes sont disponibles dans la [bibliothèque d’icônes de l’interface utilisateur coral](https://helpx.adobe.com/fr/experience-manager/6-3/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html#availableIcons) fournie avec AEM.

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

Le code suivant correspond à la mise en oeuvre du service pour l’affichage de la colonne des revenus.

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

## Test sur votre serveur

>[!NOTE]
>
>Cet article suppose que vous avez installé les [exemples de workflow](assets/review-workflow.zip) et [exemple de formulaire](assets/snap-form.zip) de [l’article précédent](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/inbox-customization/add-married-column.md) de cette série.

* [Connectez-vous à crx en tant qu’utilisateur administrateur.](http://localhost:4502/crx/de/index.jsp)
* [modèle d&#39;import sightly](assets/sightly-template.zip)
* [Connexion à AEM console web](http://localhost:4502/system/console/bundles)
* [Déploiement et démarrage du lot de personnalisation de la boîte de réception](assets/income-column-customization.jar)
* [Ouvrir votre boîte de réception](http://localhost:4502/aem/inbox)
* Ouvrez le contrôle d’administration en cliquant sur le bouton Afficher en regard du bouton Créer
* Ajouter une colonne de revenu à la boîte de réception et enregistrer vos modifications
* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Sélectionnez l’ _état civil_ et envoyez le formulaire.
* [Afficher la boîte de réception](http://localhost:4502/aem/inbox)

L’envoi du formulaire déclenche le workflow et une tâche est assignée à l’utilisateur &quot;administrateur&quot;. L’icône appropriée s’affiche sous la colonne de revenu.
