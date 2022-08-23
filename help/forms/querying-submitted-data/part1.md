---
title: AEM Forms avec schéma et données JSON [Partie 1]
seo-title: AEM Forms with JSON Schema and Data[Part1]
description: Tutoriel en plusieurs parties pour vous guider tout au long des étapes nécessaires à la création d’un formulaire adaptatif avec un schéma JSON et à l’interrogation des données envoyées.
seo-description: Multi-Part tutorial to walk you through the steps involved in creating Adaptive Form with JSON schema and querying the submitted data.
feature: Adaptive Forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '266'
ht-degree: 0%

---

# Création d’un formulaire adaptatif basé sur un schéma JSON


La possibilité de créer un Forms adaptatif basé sur un schéma JSON a été introduite avec la version 6.3 d’AEM Forms. Les détails de la création d’un Forms adaptatif avec un schéma JSON sont expliqués en détail dans cette section [article](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html).

Une fois que vous avez créé un formulaire adaptatif basé sur un schéma JSON, l’étape suivante consiste à stocker les données envoyées dans la base de données. À cette fin, nous utiliserons le nouveau type de données JSON introduit par divers fournisseurs de base de données. Pour les besoins de cet article, nous utiliserons la base de données MySql 8 pour stocker les données envoyées.

La base de données MySql 8 a été utilisée pour cet article. MySQL a introduit un nouveau type de données appelé [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Cela facilite le stockage et la requête d’objets JSON. Nous stockerons les données envoyées dans une colonne de type JSON dans notre base de données.

La capture d’écran suivante montre les données de formulaire envoyées stockées dans le type de données JSON. La colonne &quot;formdata&quot; est de type JSON. Nous avons également stocké le nom du formulaire associé aux données dans la colonne nom du formulaire.

>[!NOTE]
>
>Assurez-vous que votre fichier de schéma json est nommé correctement. Par exemple, il doit être nommé au format suivant : &lt;name>schema.json. Votre fichier de schéma peut donc être prêt immobilier.schema.json ou crédit.schema.json.


![datastored](assets/datastored.gif)


[Exemples de schémas JSON pouvant être utilisés pour créer le Forms adaptatif.](assets/samplejsonschemas.zip). Téléchargez et décompressez le fichier zip pour obtenir les schémas JSON.
