---
title: Protection CSRF
description: Découvrez comment générer et ajouter des jetons CSRF AEM aux requêtes POST, PUT et DELETE autorisées à AEM pour les personnes authentifiées.
version: Cloud Service
feature: Security
topic: Development, Security
role: Developer
level: Intermediate
doc-type: Code Sample
last-substantial-update: 2023-07-14T00:00:00Z
jira: KT-13651
thumbnail: KT-13651.jpeg
exl-id: 747322ed-f01a-48ba-a4a0-483b81f1e904
source-git-commit: 097ff8fd0f3a28f3e21c10e03f6dc28695cf9caf
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 100%

---

# Protection CSRF

Découvrez comment générer et ajouter des jetons CSRF AEM aux requêtes POST, PUT et DELETE autorisées à AEM pour les personnes authentifiées.

AEM nécessite l’envoi d’un jeton CSRF valide pour les requêtes HTTP __authentifiées__ __POST__, PUT ou __DELETE__ aux services de création et de publication AEM.

Le jeton CSRF n’est pas requis pour les requêtes __GET__ ou __anonymes__.

Si un jeton CSRF n’est pas envoyé avec une requête POST, PUT ou DELETE, AEM renvoie une réponse 403 Forbidden (accès interdit) et AEM consigne l’erreur suivante :

```log
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] isValidRequest: empty CSRF token - rejecting
[INFO][POST /path/to/aem/endpoint HTTP/1.1][com.adobe.granite.csrf.impl.CSRFFilter] doFilter: the provided CSRF token is invalid
```

Consultez la [documentation pour plus d’informations sur la protection CSRF d’AEM](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/csrf-protection.html?lang=fr).


## Bibliothèque cliente CSRF

AEM fournit une bibliothèque cliente qui peut être utilisée pour générer et ajouter des requêtes XHR de jetons CSRF et POST de formulaire, en appliquant les fonctions de prototype principales. La fonctionnalité est fournie par la catégorie de bibliothèque cliente `granite.csrf.standalone`.

Pour utiliser cette approche, ajoutez `granite.csrf.standalone` en tant que dépendance à la bibliothèque cliente qui se charge sur votre page. Par exemple, si vous utilisez la catégorie de bibliothèque cliente `wknd.site`, ajoutez `granite.csrf.standalone` en tant que dépendance à la bibliothèque cliente qui se charge sur votre page.

## Envoyer un formulaire personnalisé avec protection CSRF

Si l’utilisation de bibliothèque cliente [`granite.csrf.standalone`](#csrf-client-library) n’est pas compatible avec votre cas d’utilisation, vous pouvez ajouter manuellement un jeton CSRF à un envoi de formulaire. L’exemple suivant montre comment ajouter un jeton CSRF à un envoi de formulaire.

Cet extrait de code montre comment, lors de l’envoi d’un formulaire, le jeton CSRF peut être récupéré à partir d’AEM et ajouté à une entrée de formulaire nommée `:cq_csrf_token`. Étant donné que le jeton CSRF a une courte durée de vie, il est préférable de récupérer et de définir le jeton CSRF juste avant l’envoi du formulaire, afin de s’assurer de sa validité.

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
        form.insertAdjacentHTML('beforeend', `<input type="hidden" name=":cq_csrf_token" value="${json.token}">`);
    } else {
        // If the form already has a CSRF token input, update the value.
        csrfTokenInput.value = json.token;
    }
    // Submit the form with the hidden input containing the CSRF token
    form.submit();
});
```

## Récupérer avec la protection CSRF

Si l’utilisation de la bibliothèque cliente [`granite.csrf.standalone`](#csrf-client-library) n’est pas compatible avec votre cas d’utilisation, vous pouvez ajouter manuellement un jeton CSRF à des requêtes XHR ou de récupération. L’exemple suivant montre comment ajouter un jeton CSRF à une requête XHR réalisé avec récupération.

Cet extrait de code explique comment récupérer un jeton CSRF à partir d’AEM et l’ajouter à l’en-tête d’une requête HTTP `CSRF-Token` d’une requête de récupération. Étant donné que le jeton CSRF a une courte durée de vie, il est préférable de récupérer et de définir le jeton CSRF immédiatement avant que la requête de récupération ne soit effectuée, afin de s’assurer de sa validité.

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

Lors de l’utilisation de jetons CSRF sur le service de publication AEM, la configuration de Dispatcher doit être mise à jour afin d’autoriser les requêtes GET au point d’entrée de jeton CSRF. La configuration suivante permet d’envoyer des requêtes GET au point d’entrée du jeton CSRF sur le service de publication AEM. Si cette configuration n’est pas ajoutée, le point d’entrée du jeton CSRF renvoie une réponse 404 Introuvable.

* `dispatcher/src/conf.dispatcher.d/filters/filters.any`

```
...
/0120 { /type "allow" /method "GET" /url "/libs/granite/csrf/token.json" }
...
```
