---
title: Installation des bibliothèques de réaction de formulaire adaptatif requises
description: Ajout des dépendances requises à votre projet de réaction
feature: Adaptive Forms
version: 6.5
kt: 13285
topic: Development
role: User
level: Intermediate
source-git-commit: c6e83a627743c40355559d9cdbca2b70db7f23ed
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---


# Installation des dépendances requises

Pour commencer à utiliser des formulaires adaptatifs sans interface dans votre projet de réaction, installez les dépendances suivantes dans votre projet de réaction.

* @aemforms/af-response-components
* @aemforms/af-response-renderer

Mettez à jour le fichier package.json pour inclure les dépendances suivantes. Au moment de l&#39;écriture 0.22.41 était la version actuelle

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

## Configurer le proxy

Le partage des ressources cross-origin (CORS) est un mécanisme de sécurité qui empêche les navigateurs web d’effectuer des requêtes vers un domaine différent de celui sur lequel l’application est hébergée. Des erreurs CORS peuvent se produire lorsque vous essayez de récupérer des données d’une API hébergée sur un autre domaine. En configurant un proxy, vous pouvez contourner les restrictions CORS et envoyer des requêtes à l’API à partir de votre application React. J’ai utilisé le code suivant dans un fichier appelé setUpProxy.js dans le dossier src. **Veillez à modifier la cible pour qu’elle pointe vers votre instance de publication.**

```
const { createProxyMiddleware } = require('http-proxy-middleware');
const proxy = {
    target: 'https://mypublishinstance:4503/',
    changeOrigin: true
}
module.exports = function(app) {
  app.use(
    '/adobe',
    createProxyMiddleware(proxy)
  ),
  app.use(
    '/content',
    createProxyMiddleware(proxy)
  );
};
```

Vous devez également installer et ajouter le **http-proxy-middleware** à votre projet.

## Étapes suivantes

[Récupérer le formulaire à incorporer](./fetch-the-form.md)