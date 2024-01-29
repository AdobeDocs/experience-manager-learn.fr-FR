---
title: Installer les bibliothèques React de formulaire adaptatif requises
description: Ajouter les dépendances requises à votre projet React
feature: Adaptive Forms
version: 6.5
jira: KT-13285
topic: Development
role: User
level: Intermediate
exl-id: 0ed44016-d52a-4980-a0b1-06da149c3cb1
duration: 56
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '210'
ht-degree: 100%

---

# Installer les dépendances requises

Pour commencer à utiliser des formulaires adaptatifs découplés dans votre projet React, installez les dépendances suivantes dans votre projet React :

* @aemforms/af-react-components
* @aemforms/af-react-renderer

Mettez à jour le fichier package.json pour inclure les dépendances suivantes. Au moment de l’écriture, la version 0.22.41 était la version actuelle.

```json
"@aemforms/af-react-components": "^0.22.41",
"@aemforms/af-react-renderer": "^0.22.41",
```

>[!NOTE]
>
>La liste déroulante et la disposition de carte de ce tutoriel ont été créées à l’aide de la [bibliothèque de Material UI](https://mui.com/). Vous devrez télécharger les packages de Material UI appropriés pour que le code fonctionne sur votre système.

## Configurer le proxy

Le partage des ressources entre origines multiples (CORS) est un mécanisme de sécurité qui empêche les navigateurs web d’effectuer des requêtes vers un domaine différent de celui sur lequel l’application est hébergée. Des erreurs CORS peuvent se produire lorsque vous essayez de récupérer des données d’une API hébergée sur un autre domaine. En configurant un proxy, vous pouvez contourner les restrictions CORS et envoyer des requêtes à l’API à partir de votre application React. J’ai utilisé le code suivant dans un fichier appelé setUpProxy.js dans le dossier src. **Veillez à modifier la cible pour qu’elle pointe vers votre instance de publication.**

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

Vous devez également installer et ajouter le module **http-proxy-middleware** à votre projet.

## Étapes suivantes

[Récupérer le formulaire à incorporer](./fetch-the-form.md)
