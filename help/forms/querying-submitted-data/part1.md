---
title: AEM Forms avec schéma et données JSON [Partie 1]
seo-title: AEM Forms avec schéma JSON et données [Partie1]
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes nécessaires à la création d’un formulaire adaptatif avec un schéma JSON et à l’interrogation des données envoyées.
seo-description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes nécessaires à la création d’un formulaire adaptatif avec un schéma JSON et à l’interrogation des données envoyées.
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 3%

---


# Création d’un formulaire adaptatif basé sur un schéma JSON


La possibilité de créer un Forms adaptatif basé sur un schéma JSON a été introduite avec la version 6.3 d’AEM Forms. Les détails de la création d’un Forms adaptatif avec le schéma JSON sont expliqués en détail dans cet [article](https://helpx.adobe.com/fr/experience-manager/6-3/forms/using/adaptive-form-json-schema-form-model.html).

Une fois que vous avez créé un formulaire adaptatif basé sur un schéma JSON, l’étape suivante consiste à stocker les données envoyées dans la base de données. À cette fin, nous utiliserons le nouveau type de données JSON introduit par divers fournisseurs de base de données. Pour les besoins de cet article, nous utiliserons la base de données MySql 8 pour stocker les données envoyées.

La base de données MySql 8 a été utilisée pour cet article. MySQL a introduit un nouveau type de données appelé [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Cela facilite le stockage et la requête d’objets JSON. Nous stockerons les données envoyées dans une colonne de type JSON dans notre base de données.

La capture d’écran suivante montre les données de formulaire envoyées stockées dans le type de données JSON. La colonne &quot;formdata&quot; est de type JSON. Nous avons également stocké le nom du formulaire associé aux données dans la colonne nom du formulaire.

>[!NOTE]
>
>Assurez-vous que votre fichier de schéma json est nommé correctement. Par exemple, il doit être nommé au format suivant &lt;name>schema.json. Votre fichier de schéma peut donc être prêt immobilier.schema.json ou crédit.schema.json.


![datastored](assets/datastored.gif)


[Exemples de schémas JSON pouvant être utilisés pour créer le Forms adaptatif.](assets/samplejsonschemas.zip). Téléchargez et décompressez le fichier zip pour obtenir les schémas JSON.

