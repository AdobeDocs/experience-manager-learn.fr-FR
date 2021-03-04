---
title: Personnalisation de la boîte de réception
description: Ajouter des colonnes personnalisées pour afficher des données supplémentaires du processus
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
source-wordcount: '311'
ht-degree: 8%

---


# Ajout de colonnes personnalisées

Pour afficher les données de processus dans la boîte de réception, nous devons définir et renseigner les variables dans le processus. La valeur de la variable doit être définie avant qu’une tâche ne soit affectée à un utilisateur. Pour vous fournir un début principal, nous avons fourni un exemple de processus prêt à être déployé sur votre serveur AEM.

* [Connexion à AEM](http://localhost:4502/crx/de/index.jsp)
* [Importation du processus de révision](assets/review-workflow.zip)
* [Vérification du processus](http://localhost:4502/editor.html/conf/global/settings/workflow/models/reviewworkflow.html)

Ce flux de travaux comporte deux variables définies (isMarry et Income) et ses valeurs sont définies à l’aide du composant de variable set. Ces variables seront disponibles sous forme de colonnes à ajouter à AEM boîte de réception

## Créer un service

Pour chaque colonne que nous devons afficher dans notre boîte de réception, nous devons écrire un service. Le service suivant nous permet d’ajouter une colonne pour afficher la valeur de la variable isMarry.

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

## Tester sur votre serveur

* [Connexion à AEM console Web](http://localhost:4502/system/console/bundles)
* [Déploiement et début du lot de personnalisation de la boîte de réception](assets/inboxcustomization.inboxcustomization.core-1.0-SNAPSHOT.jar)
* [Ouvrez votre boîte de réception](http://localhost:4502/aem/inbox)
* Ouvrez le contrôle d’administration en cliquant sur l’icône _Vue de Liste_ en regard du bouton _Créer_.
* Ajouter la colonne Marié dans la boîte de réception et enregistrer vos modifications
* [Accéder à l’interface utilisateur FormsAndDocuments](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
* [Importez l’exemple de ](assets/snap-form.zip) formulaire en sélectionnant  _Fichier_ téléchargé depuis  __ Createmenu.
* [Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/snapform/jcr:content?wcmmode=disabled)
* Sélectionnez l&#39;_état civil_ et envoyez le formulaire.
   [Boîte de réception vue](http://localhost:4502/aem/inbox)

L’envoi du formulaire déclenche le processus et une tâche est attribuée à l’utilisateur &quot;administrateur&quot;. Vous devriez voir une valeur sous la colonne Marié comme illustré dans cette capture d&#39;écran.

![colonne mariée](assets/married-column.PNG)
