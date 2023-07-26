---
title: Protection CSRF
description: Découvrez comment générer et ajouter AEM jetons CSRF aux demandes POST, PUT et Supprimer autorisées à AEM pour les utilisateurs authentifiés.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
source-git-commit: b044c9982fc9309fb73509dd3117f5467903bd6a
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 0%

---


# Protection CSRF

Découvrez comment générer et ajouter AEM jetons CSRF aux demandes POST, PUT et Supprimer autorisées à AEM pour les utilisateurs authentifiés.

AEM nécessite l’envoi d’un jeton CSRF valide pour __authentifié__ __POST__, __PUT ou __DELETE__ Demandes HTTP aux services AEM Author et Publish.

Le jeton CSRF n’est pas requis pour __GET__ requêtes, ou __anonyme__ requêtes.

Si un jeton CSRF n’est pas envoyé avec une requête de POST, de PUT ou de DELETE, AEM renvoie une réponse 403 Forbidden et AEM consigne l’erreur suivante :

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Voir [documentation pour plus d’informations sur la protection AEM CSRF](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html).


## Bibliothèque cliente CSRF

AEM fournit une bibliothèque cliente qui peut être utilisée pour générer et ajouter des requêtes XHR et de POST de formulaire de jetons CSRF, via l’application de correctifs des fonctions de prototypes principales. La fonctionnalité est fournie par la fonction `granite.csrf.standalone` catégorie de bibliothèque cliente.

Pour utiliser cette approche, ajoutez `granite.csrf.standalone` en tant que dépendance à la bibliothèque cliente qui se charge sur votre page. Par exemple, si vous utilisez la variable `wknd.site` catégorie de bibliothèque cliente, ajouter `granite.csrf.standalone` en tant que dépendance à la bibliothèque cliente qui se charge sur votre page.

## Envoi de formulaire personnalisé avec protection CSRF

Si l’utilisation de [`granite.csrf.standalone` bibliothèque cliente](#csrf-client-library) n’est pas compatible avec votre cas d’utilisation, vous pouvez ajouter manuellement un jeton CSRF à un envoi de formulaire. L’exemple suivant montre comment ajouter un jeton CSRF à un envoi de formulaire.

Ce fragment de code montre comment, lors de l’envoi d’un formulaire, le jeton CSRF peut être récupéré à partir d’AEM et ajouté à une entrée de formulaire nommée `:cq_csrf_token`. Étant donné que le jeton CSRF a une courte durée de vie, il est préférable de récupérer et de définir le jeton CSRF juste avant l’envoi du formulaire, en assurant sa validité.

```javascript
// Attach submit handler event to form onSubmit
document.querySelector('form').addEventListener('submit', async (event) => {
    event.preventDefault();

    const form = event.target;
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    
    // Create a form input named ``:cq_csrf_token`` with the CSRF token.
    let csrfTokenInput = form.querySelector('input[name=":cq_csrf_token"]');
    if (!csrfTokenInput?.value) {
        // If the form does not have a CSRF token input, add one.
        form.insertAdjacentHTML('afterend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Récupération avec protection CSRF

Si l’utilisation de [`granite.csrf.standalone` bibliothèque cliente](#csrf-client-library) n’est pas compatible avec votre cas d’utilisation, vous pouvez ajouter manuellement un jeton CSRF à un XHR ou récupérer des requêtes. L’exemple suivant montre comment ajouter un jeton CSRF à un XHR réalisé avec récupération.

Ce fragment de code explique comment récupérer un jeton CSRF d’AEM et l’ajouter à une requête de récupération. `CSRF-Token` En-tête de requête HTTP. Étant donné que le jeton CSRF a une courte durée de vie, il est préférable de récupérer et de définir le jeton CSRF immédiatement avant que la requête de récupération ne soit effectuée, ce qui garantit sa validité.

```javascript
/**
 * Get CSRF token from AEM.
 * CSRF token expire after a few minutes, so it is best to get the token before each request.
 * 
 * @returns {Promise<string>} that resolves to the CSRF token.
 */
async function getCsrfToken() {
    const response = await fetch('/libs/granite/csrf/token.json');
    const json = await response.json();
    return json.token;
}

// Fetch from AEM with CSRF token in a header named 'CSRF-Token'.
await fetch('/path/to/aem/endpoint', {
    method: 'POST',
    headers: {
        'CSRF-Token': await getCsrfToken()
    },
});
```

## Configuration du Dispatcher

Lors de l’utilisation de jetons CSRF sur le service de publication AEM, la configuration de Dispatcher doit être mise à jour afin d’autoriser les requêtes de GET au point de terminaison de jeton CSRF. La configuration suivante permet d’envoyer des requêtes de GET au point de terminaison du jeton CSRF sur le service de publication AEM. Si cette configuration n’est pas ajoutée, le point de terminaison du jeton CSRF renvoie une réponse 404 Not Found.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
