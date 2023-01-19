---
title: Installation de l’IDE GraphiQL sur AEM 6.5.X
description: Découvrez comment installer et configurer l’IDE GraphiQL sur AEM version 6.5.X
version: 6.5
topic: Headless
feature: GraphQL API
role: Developer
level: Intermediate
kt: 11614
thumbnail: KT-10253.jpeg
source-git-commit: ae27cbc50fc5c4c2e8215d7946887b99d480d668
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 24%

---


# Installation de l’IDE GraphiQL sur AEM 6.5.X

Dans AEM 6.5, l’outil IDE GraphiQL doit être installé manuellement, suivez les étapes ci-dessous pour l’installation et la configuration.

1. Accédez au **[Portail de distribution de logiciels](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html)** > **AEM as a Cloud Service**.
1. Recherchez « GraphiQL » (veillez à inclure le **i** dans **GraphiQL**.
1. Télécharger la dernière version du **Package de contenu GraphiQL v.x.x.x**

   ![Téléchargement du package GraphiQL](assets/graphiql/software-distribution.png)

   Le fichier zip est un package AEM qui peut être installé directement.

1. Dans le menu AEM Démarrer , accédez à **Outils** > **Déploiement** > **Packages**.
1. Cliquez sur **Télécharger le package** et sélectionnez le package téléchargé à l’étape précédente. Cliquez sur **Installer** pour installer le package.

   ![Installation du package GraphiQL](assets/graphiql/install-graphiql-package.png)

1. Accédez à **CRXDE Lite** > **Panneau Référentiel** > sélectionner `/content/graphiql` noeud (par exemple, <http://localhost:4502/crx/de/index.jsp#/content/graphiql>).
1. Dans le **Propriétés** valeur de modification de l’onglet `endpoint` de `/content/_cq_graphql/wknd-shared/endpoint.json`.
   ![Modification de la valeur de la propriété Endpoint](assets/graphiql/endpoint-prop-value-change.png)

1. Accédez au **Configuration de la console web** Interface utilisateur > Rechercher **Filtre CSRF** configuration (par exemple,<http://localhost:4502/system/console/configMgr/com.adobe.granite.csrf.impl.CSRFFilter)>
1. Dans le `Excluded Paths` mise à jour du champ de nom de propriété, chemin d’accès du point de terminaison WKND GraphQL vers `/content/cq:graphql/wknd-shared/endpoint`.
   ![Modification de la valeur de la propriété Exclure les chemins](assets/graphiql/exclude-paths-value-change.png)

1. Accédez à l’éditeur GraphiQL à l’aide de `//HOST:PORT/content/graphiql.html`et vérifiez que vous pouvez créer une requête ou exécuter une requête existante. (par exemple, <http://localhost:4502/content/graphiql.html>)

![Éditeur GraphiQL](assets/graphiql/graphiql-editor.png)

>[!TIP]
>
>Pour prendre en charge l’exécution de requêtes et de schémas GraphQL spécifiques à votre projet, vous devez apporter les modifications correspondantes au `endpoint` et `Excluded Paths` dans les étapes ci-dessus.
