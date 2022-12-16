---
title: Test de la solution de kit de bienvenue
description: Déploiement des ressources de la solution pour tester la solution
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-12-14T00:00:00Z
source-git-commit: 0e27907066c7d688549a980ccd17b3f17d74b60b
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 0%

---

# Déploiement et test des exemples de ressources

[Installer le package du kit de bienvenue](assets/welcomekit.zip). Ce package contient le modèle de page, le composant personnalisé pour répertorier les ressources, l’exemple de workflow, le modèle d’email et les exemples de documents pdf à inclure dans le kit de bienvenue.
[Installation du lot welcome-kit](assets/welcomekit.core-1.0.0-SNAPSHOT.jar). Ce lot contient le code permettant de créer la page et la classe java pour renvoyer les ressources à afficher dans la page web.
[Installation de l’exemple de formulaire adaptatif](assets/account-openeing-form.zip)
Configurez le service de messagerie Day CQ. Le workflow envoie le lien du kit de bienvenue à l’auteur du formulaire qui doit configurer correctement SMTP.
Configurez le composant Envoyer un courrier électronique du workflow selon vos besoins.
[Aperçu du formulaire](http://localhost:4502/content/dam/formsanddocuments/co-operators/accountopeningform/jcr:content?wcmmode=disabled)
Saisissez vos informations et sélectionnez un ou plusieurs fonds communs de placement et envoyez le formulaire Vous devriez recevoir un courrier électronique contenant un lien vers le kit de bienvenue.


