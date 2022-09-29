---
title: Ajout de colonnes personnalisées
description: Ajouter des colonnes personnalisées pour afficher les données additionnelles du workflow
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
kt: 5830
topic: Development
role: Developer
level: Experienced
exl-id: 0b141b37-6041-4f87-bd50-dade8c0fee7d
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 8%

---

# Ajout de colonnes personnalisées 

Pour afficher les données de workflow dans la boîte de réception, nous devons définir et renseigner les variables dans le workflow. La valeur de la variable doit être définie avant qu’une tâche ne soit affectée à un utilisateur. Pour démarrer, nous vous avons fourni un exemple de workflow prêt à être déployé sur votre serveur AEM.

* [Connexion à AEM](http://localhost:4502/crx/de/index.jsp)
* [Importation du processus de révision](assets/review-workflow.zip)
* [Vérification du workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Deux variables sont définies pour ce workflow (isMarié et revenu) et ses valeurs sont définies à l’aide du composant de variable définie. Ces variables sont disponibles sous forme de colonnes à ajouter à AEM boîte de réception

## Créer un service

Pour chaque colonne que nous devons afficher dans notre boîte de réception, nous devons écrire un service. Le service suivant nous permet d’ajouter une colonne pour afficher la valeur de la variable isMarry .

```java
import com.adobe.cq.inbox.ui.column.Column;
import com.adobe.cq.inbox.ui.column.provider.ColumnProvider;

import com.adobe.cq.inbox.ui.InboxItem;
import org.osgi.service.component.annotations.Component;

import java.util.Map;

/**
 * This provider does not require any sightly template to be defined.
 * It is used to display the value of 'ismarried' workflow variable as a column in inbox
 */
@Component(service = ColumnProvider.class, immediate = true)
public class MaritalStatusProvider implements ColumnProvider {@Override
public Column getColumn() {
return new Column("married", "Married", Boolean.class.getName());
}

// Return True or False if 'ismarried' is set. Else returns null
private Boolean isMarried(InboxItem inboxItem) {
Boolean ismarried = null;

Map metaDataMap = inboxItem.getWorkflowMetadata();
if (metaDataMap != null) {
if (metaDataMap.containsKey("isMarried")) {
    ismarried = (Boolean) metaDataMap.get("isMarried");
}
}

return ismarried;
}

@Override
public Object getValue(InboxItem inboxItem) {
return isMarried(inboxItem);
}
}
```

>[!NOTE]
>
>Vous devez inclure AEM 6.5.5 Uber.jar dans votre projet pour que le code ci-dessus fonctionne.

![uber-jar](assets/uber-jar.PNG)

## Test sur votre serveur

* [Connexion à AEM console web](http://localhost:4502/system/console/bundles)
* [Déploiement et démarrage du lot de personnalisation de la boîte de réception](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Ouvrir votre boîte de réception](http://localhost:4502/aem/inbox)
* Ouvrez le contrôle d’administration en cliquant sur _Mode Liste_ en regard de _Créer_ button
* Ajouter une colonne Marié(e) à la boîte de réception et enregistrer vos modifications
* [Accédez à l’interface utilisateur FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importation de l’exemple de formulaire](assets/snap-form.zip) en sélectionnant _Téléchargement du fichier_ de _Créer_ menu
* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Sélectionnez la _état civil_ et envoyer le formulaire
   [Boîte de réception d’affichage](http://localhost:4502/aem/inbox)

L’envoi du formulaire déclenche le workflow et une tâche est assignée à l’utilisateur &quot;administrateur&quot;. Une valeur devrait s’afficher sous la colonne Marié(e) , comme illustré dans cette capture d’écran.

![marié-colonne](assets/married-column.PNG)
