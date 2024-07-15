---
title: Créer un composant de kit de bienvenue
description: Créez une page AEM Sites avec des liens pour télécharger des ressources en fonction des données de formulaire envoyées.
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 66496f0e-c121-4b6d-b371-084393ece3ca
duration: 20
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '74'
ht-degree: 100%

---

# Composant du kit de bienvenue

Un composant de page a été créé pour répertorier les ressources de la page qui peuvent être téléchargées par la personne finale. Les chemins d’accès aux ressources individuelles sont enregistrés dans une propriété appelée **paths**. Les données de formulaire envoyées déterminent les ressources à inclure.

Le code suivant répertorie les ressources sur la page :

```html
   <p class="cmp-press-kit__press-kit-size">
        Welcome kit contains ${pressKit.assets.size} assets.
    </p>
<ul class="cmp-press-kit__asset-list" data-sly-list.asset="${pressKit.assets}">
    <li class="cmp-press-kit__asset-item">
        <div class="cmp-press-kit__asset " >
            <div class="cmp-press-kit__asset-content">
                <img class="cmp-press-kit__asset-image" src="${asset.path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png" alt="${asset.name}"/>
                <p class="cmp-press-kit__asset-title">${asset.title}</p>
            </div>
            <div class="cmp-press-kit__asset-actions">
                <a class="cmp-press-kit__asset-download-button" href="${asset.path}">Download</a>
            </div>
        </div>
    </li>
</ul>
</sly>
<sly data-sly-call="${placeholderTemplate.placeholder @ isEmpty=!ready}"></sly>
```
