---
title: Tester la solution de kit de bienvenue
description: Déployer des ressources de la solution pour tester la solution
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
exl-id: 07a1a9fc-7224-4e2d-8b6d-d935b1125653
duration: 45
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 100%

---

# Déployer et tester des exemples de ressources

[Installez le package du kit de bienvenue](assets/welcomekit.zip). Ce package contient le modèle de page, le composant personnalisé pour répertorier les ressources, l’exemple de workflow, le modèle d’e-mail et les exemples de documents PDF à inclure dans le kit de bienvenue.
[Installez le lot welcome-kit](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Ce lot contient le code permettant de créer la page et la classe Java pour renvoyer les ressources à afficher dans la page web.
[Installez le modèle de formulaire adaptatif](assets/account-openeing-form.zip).
Configurez le service de messagerie Day CQ. Le workflow envoie le lien du kit de bienvenue à la personne qui a envoyé le formulaire, ce qui nécessite que SMTP soit configuré correctement.
Configurez le composant Envoyer un e-mail du workflow selon vos besoins.
[Prévisualiser le formulaire](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Saisissez vos informations et sélectionnez un ou plusieurs fonds communs et envoyez le formulaire.
Vous devriez recevoir un e-mail avec un lien vers le kit de bienvenue.
