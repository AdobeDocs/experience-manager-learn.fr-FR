---
title: Installer IDE GraphiQL sur AEM 6.5
description: Découvrez comment installer et configurer l’IDE GraphiQL sur AEM 6.5.
version: Experience Manager 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
jira: KT-11614
thumbnail: KT-10253.jpeg
exl-id: 04fcc24c-7433-4443-a109-f01840ef1a89
duration: 41
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '208'
ht-degree: 100%

---

# Installer IDE GraphiQL sur AEM 6.5

Dans AEM 6.5, l’outil IDE GraphiQL doit être installé manuellement.

1. Accédez au **[Portail de distribution logicielle](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?lang=fr)** > **AEM as a Cloud Service**.
1. Recherchez « GraphiQL » (veillez à inclure le **i** dans **GraphiQL**).
1. Téléchargez la dernière version du **Package de contenu GraphiQL v.x.x.x**.

   ![Télécharger le package GraphiQL](assets/graphiql/software-distribution.png)

   Le fichier zip est un package AEM qui peut être installé directement.

1. Dans le menu Accueil AEM, accédez à **Outils** > **Déploiement** > **Packages**
1. Cliquez sur **Télécharger le package** et sélectionnez le package téléchargé à l’étape précédente. Cliquez sur **Installer** pour installer le package.

   ![Installer le package GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Accédez à **CRXDE Lite** > **Panneau Référentiel** > sélectionnez le nœud `/content/graphiql` (par exemple, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. Sous l’onglet **Propriétés**, modifiez la valeur de la propriété `endpoint` sur `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Modification de la valeur de la propriété du point d’entrée](assets/graphiql/endpoint-prop-value-change.png)

1. Accédez à l’interface utilisateur de la **Configuration de la console web** et recherchez la configuration du **Filtre CSRF** (par exemple, <http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. Dans le champ du nom de la propriété `Excluded Paths`, ajoutez le chemin du point d’entrée GraphQL WKND suivant : `/content/cq:graphql/wknd-shared/endpoint`.

![Modifier la valeur de la propriété des chemins exclus](assets/graphiql/exclude-paths-value-change.png)

1. Accédez à l’éditeur GraphiQL via l’adresse `//HOST:PORT/content/graphiql.html` et vérifiez que vous pouvez créer une requête ou exécuter une requête existante. (par exemple, <http://localhost:4502/content/graphiql.html>).

![Éditeur GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Pour prendre en charge le schéma GraphQL et l’exécution des requêtes propres à votre projet, vous devez apporter les modifications correspondantes aux valeurs des propriétés `endpoint` et `Excluded Paths` dans les étapes ci-dessus.
