---
title: aem forms avec Schéma et données JSON[Partie 1]
seo-title: aem forms avec Schéma et données JSON[Part1]
description: Ce didacticiel en plusieurs parties vous explique comment créer un formulaire adaptatif avec le schéma JSON et interroger les données envoyées.
seo-description: Ce didacticiel en plusieurs parties vous explique comment créer un formulaire adaptatif avec le schéma JSON et interroger les données envoyées.
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 2%

---


# Créer un formulaire adaptatif basé sur le Schéma JSON


La possibilité de créer un Forms adaptatif basé sur le schéma JSON a été introduite avec la version AEM Forms 6.3. Les détails de la création d’une Forms adaptative avec le schéma JSON sont expliqués en détail dans cet [article](https://helpx.adobe.com/fr/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Une fois que vous avez créé un formulaire adaptatif basé sur le schéma JSON, l’étape suivante consiste à stocker les données envoyées dans la base de données. À cette fin, nous utiliserons le nouveau type de données JSON introduit par divers fournisseurs de bases de données. Aux fins de cet article, nous utiliserons la base de données MySql 8 pour stocker les données envoyées.

La base de données MySQL 8 a été utilisée pour cet article. MySQL a introduit un nouveau type de données appelé [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Cela facilite le stockage et la requête des objets JSON. Nous stockerons les données envoyées dans une colonne de type JSON dans notre base de données.

La capture d’écran suivante montre les données de formulaire envoyées stockées dans le type de données JSON. La colonne &quot;formdata&quot; est de type JSON. Nous avons également stocké le nom du formulaire associé aux données dans le nom du formulaire de colonne.

>[!NOTE]
>
>Assurez-vous que votre fichier schéma json porte le nom approprié. Par exemple, il doit être nommé au format suivant &lt;name>schéma.json. Ainsi, votre fichier de schéma peut être prêt hypothécaire.schéma.json ou crédit.schéma.json.


![datastored](assets/datastored.gif)


[Exemples de Schémas JSON pouvant être utilisés pour créer un Forms adaptatif.](assets/samplejsonschemas.zip). Téléchargez et décompressez le fichier zip pour obtenir les schémas JSON.

