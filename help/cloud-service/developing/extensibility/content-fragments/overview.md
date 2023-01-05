---
title: AEM des extensions de la console Fragment de contenu
description: Découvrez comment créer et déployer AEM extensions de la console de fragments de contenu as a Cloud Service
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: fbc8c11841f5b5e04a99ba74fac6f01dc3e3a2da
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 4%

---


# Extension AEM Content Fragments Console

[AEM la console Fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=fr) les extensions peuvent être ajoutées via deux points d’extension : un bouton dans la fonction [de la console de fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=fr) menu d’en-tête ou barre d’actions. Les extensions sont écrites en JavaScript et s’exécutent en tant qu’applications App Builder. Elles peuvent également mettre en oeuvre une interface utilisateur web personnalisée et des actions Adobe I/O Runtime sans serveur afin d’effectuer des tâches plus intensives et de longue durée.

![Extension AEM Content Fragments Console](./assets/overview/example.png){align="center"}

| Type d’extension | Description | Paramètre(s) |
| :--- | :--- | :--- |
| Menu En-tête | Ajoute un bouton à l’en-tête qui s’affiche lorsque __zero__ Les fragments de contenu sont sélectionnés. | Aucun. |
| Barre d’actions | Ajoute un bouton à la barre d’actions qui s’affiche lorsque __un ou plusieurs__ Les fragments de contenu sont sélectionnés. | Tableau des chemins d’accès aux fragments de contenu sélectionnés. |

Une seule extension de la console de fragments de contenu AEM peut inclure zéro ou un menu d’en-tête et zéro ou un type d’extension de barre d’actions. Si plusieurs types d’extension du même type sont requis, plusieurs extensions de la console Fragments de contenu AEM doivent être créées.

AEM des extensions de la console de fragments de contenu, une [Projet de la console Adobe Developer](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) et un [Application App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) en utilisant la variable `@adobe/aem-cf-admin-ui-ext-tpl` modèle, associé au projet de console Adobe Developer.

Effectuez une sélection parmi les fonctionnalités suivantes lors de la génération de l’application App Builder, en fonction de ce que fait l’extension. Toutes les combinaisons d’options peuvent être utilisées dans une extension.

|  | Ajouter un bouton à [Menu En-tête](./header-menu.md) | Ajouter un bouton à [Barre d’actions](./action-bar.md) | Afficher [Modal](./modal.md) | Ajouter [gestionnaire côté serveur](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponible lorsque les fragments de contenu ne sont pas sélectionnés | ✔ |  |  |  |
| Disponible lorsqu’un ou plusieurs fragments de contenu sont sélectionnés |  | ✔ |  |  |
| Collecte des entrées personnalisées de l’utilisateur |  |  | ✔️ |  |
| Affiche des commentaires personnalisés à l’intention de l’utilisateur |  |  | ✔️ |  |
| Appelle les requêtes HTTP vers AEM |  |  |  | ✔ |
| Appelle des requêtes HTTP vers des services tiers/Adobe |  |  |  | ✔ |


## Documentation Adobe Developer

Adobe Developer contient des informations détaillées sur les développeurs concernant AEM extensions de la console de fragments de contenu. Veuillez consulter la section [Contenu Adobe Developer pour plus de détails techniques](https://developer.adobe.com/uix/docs/).

## Développement d’une extension

Suivez les étapes décrites ci-dessous pour savoir comment générer, développer et déployer une extension AEM Content Fragment Console pour AEM as a Cloud Service.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Création d’un projet Adobe Developer" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Création d’un projet Adobe Developer">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Création d’un projet</p>
                    <p class="is-size-6">Créez un projet de console Adobe Developer qui définit son accès aux autres services Adobe et gère ses déploiements.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Création d’un projet Adobe Developer</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Generate an Extension app -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Generate an Extension app">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./app-initialization.md" title="Génération d’une application d’extension" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Initialisation d’une application d’extension">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Initialisation d’une application d’extension</p>
                    <p class="is-size-6">Initialisez une application du générateur d’applications de la console de fragments de contenu AEM qui définit l’emplacement d’affichage de l’extension et le travail qu’elle effectue.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Initialisation d’une application d’extension</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension registration -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension registration">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./extension-registration.md" title="Enregistrement d’une extension" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/extension-registration/card.png" alt="Enregistrement d’une extension">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">3. Enregistrement de l’extension</p>
                    <p class="is-size-6">Enregistrez l’extension dans la console de fragments de contenu AEM en tant que menu d’en-tête ou type d’extension de barre d’actions.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Enregistrement de l’extension</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Header Menu -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Header menu">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./header-menu.md" title="Menu En-tête" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menu En-tête">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menu d’en-tête</p>
                    <p class="is-size-6">Découvrez comment créer une extension de menu d’en-tête de la console de fragments de contenu AEM.</p>
                    <a href="./header-menu.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Étendre le menu d’en-tête</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Action Bar -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Action Bar">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./action-bar.md" title="Barre d’actions" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/action-bar/card.png" alt="Barre d’actions">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4b. Barre d’actions</p>
                    <p class="is-size-6">Découvrez comment créer une extension de barre d’actions de la console de fragments de contenu AEM.</p>
                    <a href="./action-bar.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Étendre la barre d’actions</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Modal -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Modal">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./modal.md" title="Modal" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Modal">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Modal</p>
                    <p class="is-size-6">Ajoutez un modal personnalisé à l’extension qui peut être utilisé pour créer des expériences personnalisées pour les utilisateurs. Les modèles collectent souvent les entrées des utilisateurs et affichent les résultats d’une opération.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ajouter un modal</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Adobe I/O Runtime action -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Adobe I/O Runtime action">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./runtime-action.md" title="Action Adobe I/O Runtime" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Action Adobe I/O Runtime">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Action Adobe I/O Runtime</p>
                    <p class="is-size-6">Ajoutez une action Adobe I/O Runtime sans serveur que l’extension peut appeler pour interagir avec des fragments de contenu et AEM pour effectuer des opérations commerciales personnalisées.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ajout d’une action Adobe I/O Runtime</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Test -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Test">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./test.md" title="Testez" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Testez">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Test</p>
                    <p class="is-size-6">Testez les extensions pendant le développement et partagez les extensions terminées avec les testeurs AQ ou UAT à l’aide d’une URL spéciale.</p>
                    <a href="./test.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Tester l’extension</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Extension deployment -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Extension deployment">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./deploy.md" title="Déploiement d’extensions" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/deploy/card.png" alt="Déploiement d’extensions">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">8. Déploiement en production</p>
                    <p class="is-size-6">Déployez l’extension pour l’Adobe I/O afin de la rendre disponible pour AEM utilisateurs. Les extensions peuvent également être mises à jour et supprimées.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Déploiement en production</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
</div>

## Exemples d’extensions

Exemple d’AEM d’extensions de la console de fragments de contenu.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Extension Bulk property update" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Extension Bulk property update">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Extension Bulk property update</p>
                    <p class="is-size-6">Explorez un exemple d’extension de barre d’actions qui met à jour en masse une propriété sur les fragments de contenu sélectionnés.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explorer l’exemple d’extension</span>
                    </a>
                </div>
            </div>
        </div>
    </div>
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Génération d’images et chargement vers l’extension AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Génération d’images et chargement vers l’extension AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Génération d’images et chargement vers l’extension AEM</p>
                    <p class="is-size-6">Explorez un exemple d’extension de barre d’actions qui génère une image à l’aide d’OpenAI, la charge dans AEM et met à jour la propriété de l’image sur le fragment de contenu sélectionné.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
                        <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explorer l’exemple d’extension</span>
                    </a>
                </div>
            </div>
        </div>
    </div>



</div>
