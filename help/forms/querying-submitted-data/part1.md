---
title: AEM Forms avec schéma et données JSON [Partie 1]
description: Il s’agit d’un tutoriel en plusieurs parties consacré à la création de formulaires adaptatifs avec les schémas JSON et à l’interrogation des données envoyées.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: c588bdca-b8a8-4de2-97e0-ba08b195699f
duration: 62
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 100%

---

# Créer un formulaire adaptatif basé sur un schéma JSON


La possibilité de créer des formulaires adaptatifs basés sur un schéma JSON a été introduite avec la version 6.3 d’AEM Forms. Les informations sur la création de formulaires adaptatifs avec un schéma JSON sont expliquées en détail dans cet [article](https://experienceleague.adobe.com/docs/experience-manager-65/forms/adaptive-forms-advanced-authoring/adaptive-form-json-schema-form-model.html?lang=fr).

Une fois que vous avez créé un formulaire adaptatif basé sur un schéma JSON, l’étape suivante consiste à stocker les données envoyées dans la base de données. À cette fin, nous utiliserons le nouveau type de données JSON introduit par divers fournisseurs de base de données. Pour les besoins de cet article, nous utiliserons la base de données MySql 8 pour stocker les données envoyées.

La base de données MySql 8 a été utilisée pour cet article. MySQL a introduit un nouveau type de données appelé [JSON](https://dev.mysql.com/doc/refman/8.0/en/json.html). Cela facilite le stockage et la requête d’objets JSON. Nous stockons les données envoyées dans une colonne de type JSON dans notre base de données.

La copie d’écran suivante montre les données de formulaire envoyées stockées dans le type de données JSON. La colonne « formdata » est de type JSON. Nous avons également stocké le nom du formulaire associé aux données dans la colonne formname.

>[!NOTE]
>
>Assurez-vous que votre fichier de schéma json est nommé correctement. Par exemple, il doit être nommé au format suivant : &lt;name>schema.json. Votre fichier de schéma peut donc être mortgage.schema.json ou credit.schema.json.


![datastored](assets/datastored.gif)


[Exemples de schémas JSON pouvant être utilisés pour créer des formulaires adaptatifs.](assets/samplejsonschemas.zip). Téléchargez et décompressez le fichier zip pour obtenir les schémas JSON.
