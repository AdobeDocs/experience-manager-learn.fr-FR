---
title: Contenu protégé dans AEM sans affichage
description: Découvrez comment protéger le contenu dans AEM sans affichage.
version: Cloud Service
topic: Headless
feature: GraphQL API
role: Developer, Architect
level: Intermediate
jira: KT-15233
last-substantial-update: 2024-04-01T00:00:00Z
source-git-commit: c498783aceaf3bb389baaeaeefbe9d8d0125a82e
workflow-type: tm+mt
source-wordcount: '992'
ht-degree: 0%

---


# Protection du contenu dans AEM sans affichage

Il est essentiel d’assurer l’intégrité et la sécurité de vos données lors de la diffusion AEM contenu sans affichage à partir d’AEM Publish lors de la diffusion de contenu sensible. Cette procédure explique comment sécuriser le contenu fourni par AEM points d’entrée de l’API GraphQL sans affichage.

Ce mode d’emploi ne couvre pas :

- Sécuriser directement les points de fin, mais se concentre plutôt sur la sécurisation du contenu qu’ils diffusent.
- Authentification pour AEM Publication ou obtention de jetons de connexion. Les méthodes d’authentification et la transmission des informations d’identification dépendent de cas d’utilisation et d’implémentations individuels.

## Groupes d’utilisateurs

Tout d&#39;abord, nous devons définir une [groupe d’utilisateurs](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/accessing/aem-users-groups-and-permissions) contenant les utilisateurs qui doivent avoir accès au contenu protégé.

![AEM groupe d’utilisateurs de contenu protégé sans affichage](./assets/protected-content/user-groups.png){align="center"}

Les groupes d’utilisateurs attribuent l’accès au contenu AEM sans affichage, y compris aux fragments de contenu ou à d’autres ressources référencées.

1. Connectez-vous à AEM Auteur en tant que **administrateur utilisateur**.
1. Accédez à **Outils** > **Sécurité** > **Groupes**.
1. Sélectionner **Créer** dans le coin supérieur droit.
1. Dans le **Détails** , spécifiez la variable **ID de groupe** et **Nom du groupe**.
   - L’ID de groupe et le nom du groupe peuvent être de n’importe quel type, mais dans cet exemple, utilise le nom . **AEM utilisateurs d’API sans affichage**.
1. Sélectionnez **Enregistrer et fermer**.
1. Sélectionnez le groupe nouvellement créé, puis choisissez **Activer** dans la barre d’actions.

Si plusieurs niveaux d’accès sont requis, créez plusieurs groupes d’utilisateurs pouvant être associés à un contenu différent.

### Ajout d’utilisateurs à des groupes d’utilisateurs

Pour accorder AEM aux demandes d’API GraphQL sans affichage l’accès au contenu protégé, vous pouvez associer la demande sans affichage à un utilisateur appartenant à un groupe d’utilisateurs spécifique. Voici deux approches courantes :

1. **AEM as a Cloud Service [comptes techniques](https://experienceleague.adobe.com/en/docs/experience-manager-learn/getting-started-with-aem-headless/authentication/service-credentials):**
   - Créez un compte technique dans AEM Developer Console as a Cloud Service.
   - Connectez-vous à AEM Auteur une fois avec le compte technique .
   - Ajoutez le compte technique au groupe d’utilisateurs via **Outils > Sécurité > Groupes > AEM utilisateurs d’API sans affichage > Membres**.
   - **Activer** l’utilisateur du compte technique et le groupe d’utilisateurs sur AEM Publier.
   - Cette méthode nécessite que le client sans interface utilisateur n’expose pas les informations d’identification du service à l’utilisateur, car il s’agit des informations d’identification d’un utilisateur spécifique et ne doit pas être partagé.

   ![AEM Gestion des groupes du compte technique](./assets/protected-content/group-membership.png){align="center"}

2. **Utilisateurs nommés :**
   - Authentifiez les utilisateurs nommés et ajoutez-les directement au groupe d’utilisateurs sur AEM Publier.
   - Cette méthode nécessite que le client headless authentifie les informations d’identification de l’utilisateur avec AEM Publier, obtienne un jeton de connexion ou d’accès AEM et utilise ce jeton pour les demandes ultérieures à l’utilisateur. Les détails de cette procédure ne sont pas abordés dans cette section pratique et dépendent de l’implémentation.

## Protection des fragments de contenu

La protection des fragments de contenu est essentielle pour la protection de votre contenu AEM sans affichage et est réalisée en associant le contenu à un groupe d’utilisateurs fermé (CUG). Lorsqu’un utilisateur envoie une requête à l’API GraphQL AEM sans affichage, le contenu renvoyé est filtré en fonction des groupes d’utilisateurs fermés de l’utilisateur.

![AEM CUG sans affichage](./assets/protected-content/cugs.png){align="center"}

Pour ce faire, procédez comme suit : [Groupes d’utilisateurs fermés (CUG)](https://experienceleague.adobe.com/en/docs/experience-manager-learn/assets/advanced/closed-user-groups).

1. Connectez-vous à AEM Auteur en tant que **Utilisateur DAM**.
2. Accédez à **Ressources > Fichiers** et sélectionnez la variable **folder** contenant les fragments de contenu à protéger. Les CUG sont appliqués de manière hiérarchique et affectent les sous-dossiers sauf s’ils sont remplacés par un autre CUG.
   - Assurez-vous que les utilisateurs appartenant à d’autres canaux utilisant le contenu des dossiers sont inclus dans ce groupe d’utilisateurs. Vous pouvez également inclure les groupes d’utilisateurs associés à ces canaux dans la liste des groupes d’utilisateurs fermés. Dans le cas contraire, le contenu ne sera pas accessible pour ces canaux.
3. Sélectionnez le dossier et choisissez **Propriétés** dans la barre d’outils.
4. Sélectionnez la variable **Autorisations** .
5. Saisissez dans la variable **Nom du groupe** et sélectionnez la variable **Ajouter** pour ajouter le nouveau CUG.
6. **Enregistrer** pour appliquer le groupe d’utilisateurs fermé.
7. **Sélectionner** le dossier de ressources et sélectionnez **Publier** pour envoyer le dossier avec les CUG appliqués à AEM Publier, où il sera évalué en tant qu’autorisation.

Effectuez les mêmes étapes pour tous les dossiers contenant des fragments de contenu qui doivent être protégés, en appliquant les CUG corrects à chaque dossier.

Désormais, lorsqu’une requête HTTP est envoyée au point de terminaison de l’API GraphQL sans affichage d’AEM, seuls les fragments de contenu accessibles par les groupes d’utilisateurs fermés spécifiés de l’utilisateur requérant sont inclus dans le résultat. Si l’utilisateur n’a pas accès à un fragment de contenu, le résultat sera vide, tout en renvoyant un code d’état HTTP 200.

### Protection du contenu référencé

Les fragments de contenu font souvent référence à d’autres contenus AEM tels que des images. Pour sécuriser ce contenu référencé, appliquez des groupes d’utilisateurs fermés aux dossiers de ressources dans lesquels les ressources référencées sont stockées. Notez que les ressources référencées sont généralement demandées à l’aide de méthodes différentes de celles des API GraphQL sans affichage d’AEM. Par conséquent, la manière dont les jetons d’accès sont transmis dans les requêtes à ces ressources référencées peut différer.

Selon l’architecture du contenu, il peut être nécessaire d’appliquer des CUG à plusieurs dossiers pour s’assurer que tout le contenu référencé est protégé.

## Empêcher la mise en cache du contenu protégé

AEM as a Cloud Service [met en cache les réponses HTTP par défaut.](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish) pour améliorer les performances. Cela peut toutefois entraîner des problèmes lors de la diffusion de contenu protégé. Pour empêcher la mise en cache de ce contenu, [suppression des en-têtes de cache pour des points de fin spécifiques](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/caching/publish#how-to-customize-cache-rules-1) dans la configuration Apache de l’instance de publication AEM.

Ajoutez la règle suivante au fichier de configuration Apache de votre projet Dispatcher afin de supprimer les en-têtes de cache pour des points de terminaison spécifiques :

```xml
# dispatcher/src/conf.d/available_vhosts/example.vhost

<VirtualHost *:80>
    ...
    # Replace `example` with the name of your GraphQL endpoint's configuration name.
    <LocationMatch "^/graphql/execute.json/example/.*$">
        # Remove cache headers for protected endpoints so they are not cached
        Header unset Cache-Control
        Header unset Surrogate-Control
        Header set Age 0
    </LocationMatch>
    ...
</VirtualHost>
```

Notez que cela entraîne une pénalité de performances, car le contenu ne sera pas mis en cache par le Dispatcher ou le réseau de diffusion de contenu. Il s’agit d’un compromis entre performance et sécurité.

## Protection des points d’entrée de l’API GraphQL sans affichage AEM

Ce guide ne traite pas de la protection de la variable [AEM points d’entrée de l’API GraphQL sans affichage](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/headless/graphql-api/graphql-endpoint) mais se concentre plutôt sur la sécurisation du contenu qu&#39;ils diffusent. Tous les utilisateurs, y compris les utilisateurs anonymes, peuvent accéder aux points de terminaison contenant du contenu protégé. Seul le contenu accessible par les groupes d’utilisateurs fermés de l’utilisateur est renvoyé. Si aucun contenu n’est accessible, la réponse de l’API AEM sans affichage comporte toujours un code d’état de réponse HTTP 200, mais les résultats seront vides. En règle générale, la sécurisation du contenu est suffisante, car les points de terminaison eux-mêmes n’exposent pas par nature les données sensibles. Si vous devez sécuriser les points de fin, appliquez-leur des listes de contrôle d’accès lors de l’AEM de publication via [Sling Repository Initialization (repoinit) scripts](https://sling.apache.org/documentation/bundles/repository-initialization.html#repoinit-parser-test-scenarios).

