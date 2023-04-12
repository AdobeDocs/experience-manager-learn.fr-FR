---
title: Extensions de la console Fragments de contenu d’AEM
description: Apprenez à construire et à déployer les extensions de la console Fragments de contenu d’AEM as a Cloud Service.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay
kt: 11603
thumbnail: KT-11603.png
last-substantial-update: 2022-12-09T00:00:00Z
source-git-commit: d902eb9a8d497a43c8d4ca63767f81a35eadf139
workflow-type: ht
source-wordcount: '745'
ht-degree: 100%

---


# Extension de la console Fragments de contenu d’AEM.

[Les extensions de la console Fragments de contenu d’AEM](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=fr) peuvent être ajoutées avec deux points d’extension : un bouton dans le menu d’en-tête de la [console Fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/content-fragments/content-fragments-console.html?lang=fr) ou dans la barre d’actions. Les extensions sont écrites en JavaScript et s’exécutent en tant qu’applications App Builder. Elles peuvent également implémenter une interface utilisateur web personnalisée et des actions Adobe I/O Runtime sans serveur afin d’effectuer des tâches plus intensives et de longue durée.

![Extension de la console Fragments de contenu d’AEM.](./assets/overview/example.png){align="center"}

| Type d’extension | Description | Paramètre(s) |
| :--- | :--- | :--- |
| Menu d’en-tête | Ajoute un bouton à l’en-tête qui s’affiche si __zéro__ fragment de contenu est sélectionné. | Aucun. |
| Barre d’actions | Ajoute un bouton à la barre d’actions qui s’affiche si __un ou plusieurs__ fragment(s) de contenu est sélectionné ou sont sélectionnés. | Tableau des chemins d’accès aux fragments de contenu sélectionnés. |

Une seule extension de la console Fragments de contenu d’AEM peut inclure zéro ou un menu d’en-tête et zéro ou un type d’extension de barre d’actions. Si plusieurs types d’extension du même type sont requis, plusieurs extensions de la console Fragments de contenu d’AEM doivent être créées.

Les extensions de la console Fragments de contenu d’AEM nécessitent un projet [Adobe Developer Console](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/extension-development/#create-a-project-in-adobe-developer-console) et une application [App Builder](https://developer.adobe.com/uix/docs/services/aem-cf-console-admin/code-generation) qui utilise le modèle `@adobe/aem-cf-admin-ui-ext-tpl` associé au projet Adobe Developer Console.

Sélectionnez l’une des capacités suivantes lors de la création de l’application App Builder, selon le rôle de l’extension. Il est possible d’utiliser n’importe quelle combinaison d’options dans une extension.

|  | Ajout d’un bouton au [Menu En-tête](./header-menu.md) | Ajout d’un bouton à la [Barre d’actions](./action-bar.md) | Affichage de la [Boîte de dialogue modale](./modal.md) | Ajout d’un [gestionnaire côté serveur](./runtime-action.md) |
| ------------------------------------------ | :-----------------------: | :----------------------: | :--------: | :--------------------:  |
| Disponible si les fragments de contenu ne sont pas sélectionnés | ✔ |  |  |  |
| Disponible si un ou plusieurs fragments de contenu sont sélectionnés |  | ✔ |  |  |
| Collecte les entrées personnalisées de l’utilisateur ou de l’utilisatrice |  |  | ✔️ |  |
| Affiche des commentaires personnalisés pour l’utilisateur ou l’utilisatrice |  |  | ✔️ |  |
| Appelle les requêtes HTTP vers AEM |  |  |  | ✔ |
| Appelle des requêtes HTTP vers des services tiers ou d’Adobe |  |  |  | ✔ |


## Documentation Adobe Developer

Adobe Developer contient des détails de développement sur les extensions de la console Fragments de contenu d’AEM. Veuillez consulter la section [contenu Adobe Developer pour plus de détails techniques](https://developer.adobe.com/uix/docs/).

## Développer d’une extension

Suivez les étapes décrites ci-dessous pour savoir comment générer, développer et déployer une extension de console Fragments de contenu d’AEM pour AEM as a Cloud Service.

<div class="columns is-multiline">
    <!-- Create Adobe Developer Project -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Create Adobe Developer Project">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./adobe-developer-console-project.md" title="Créer un projet Adobe Developer" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/project/card.png" alt="Créer un projet Adobe Developer">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">1. Créer un projet</p>
                    <p class="is-size-6">Créez un projet Adobe Developer Console qui définit son accès aux autres services d’Adobe et gère ses déploiements.</p>
                    <a href="./adobe-developer-console-project.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Créer un projet Adobe Developer</span>
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
                    <a href="./app-initialization.md" title="Générer une application d’extension" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/initialize-app/card.png" alt="Initialiser une application d’extension">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">2. Initialiser une application d’extension</p>
                    <p class="is-size-6">Initialisez une application App Builder d’extension de la console Fragments de contenu d’AEM qui définit l’emplacement d’affichage de l’extension et le travail qu’elle effectue.</p>
                    <a href="./app-initialization.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Initialiser une application d’extension</span>
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
                    <p class="headline is-size-5 has-text-weight-bold">3. Enregistrer l’extension</p>
                    <p class="is-size-6">Enregistrez l’extension dans la console Fragments de contenu d’AEM en tant que menu d’en-tête ou type d’extension de barre d’actions.</p>
                    <a href="./extension-registration.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Enregistrer l’extension</span>
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
                    <a href="./header-menu.md" title="Menu d’en-tête" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/header-menu/card.png" alt="Menu d’en-tête">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">4a. Menu d’en-tête</p>
                    <p class="is-size-6">Découvrez comment créer une extension du menu d’en-tête de la console Fragments de contenu d’AEM.</p>
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
                    <p class="is-size-6">Découvrez comment créer une extension de barre d’actions de la console Fragments de contenu d’AEM.</p>
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
                    <a href="./modal.md" title="Boîte de dialogue modale" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/modal/card.png" alt="Boîte de dialogue modale">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">5. Boîte de dialogue modale</p>
                    <p class="is-size-6">Ajoutez une boîte de dialogue modale personnalisée à l’extension, qui pourra être utilisée pour créer des expériences personnalisées pour les utilisateurs et utilisatrices. Les boîtes de dialogue modales collectent souvent les entrées des utilisateurs et utilisatrices, et affichent les résultats d’une opération.</p>
                    <a href="./modal.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ajouter une boîte de dialogue modale</span>
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
                    <a href="./runtime-action.md" title="Action Adobe I/O Runtime" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/runtime-action/card.png" alt="Action Adobe I/O Runtime">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">6. Action Adobe I/O Runtime</p>
                    <p class="is-size-6">Ajoutez une action Adobe I/O Runtime sans serveur que l’extension peut appeler pour interagir avec des fragments de contenu et AEM afin d’effectuer des opérations commerciales personnalisées.</p>
                    <a href="./runtime-action.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Ajouter une action Adobe I/O Runtime</span>
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
                    <a href="./test.md" title="Tester" tabindex="-1">
                        <img class="is-bordered-r-small" src="./assets/test/card.png" alt="Tester">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">7. Test</p>
                    <p class="is-size-6">Testez les extensions pendant le développement et partagez les extensions terminées avec les testeurs et testeuses d’AQ ou d’UAT à l’aide d’une URL spéciale.</p>
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
                    <p class="headline is-size-5 has-text-weight-bold">8. Déployer en production</p>
                    <p class="is-size-6">Déployez l’extension vers Adobe I/O afin de la rendre disponible pour les utilisateurs et utilisatrices d’AEM. Les extensions peuvent également être mises à jour et supprimées.</p>
                    <a href="./deploy.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Déployer en production</span>
</a>
                </div>
            </div>
        </div>
    </div>
</div>

## Exemples d’extensions

Exemple d’extensions de la console Fragments de contenu d’AEM.

<div class="columns is-multiline">
    <!-- Bulk property update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Bulk property update extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/bulk-property-update.md" title="Extension de mise à jour en bloc des propriétés" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/bulk-property-update/card.png" alt="Extension de mise à jour en bloc des propriétés">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Extension de mise à jour en bloc des propriétés</p>
                    <p class="is-size-6">Explorez un exemple d’extension de barre d’actions qui effectue une mise à jour en bloc d’une propriété sur les fragments de contenu sélectionnés.</p>
                    <a href="./example-extensions/bulk-property-update.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explorer l’exemple d’extension</span>
</a>
                </div>
            </div>
        </div>
    </div>
    <!-- Image Generartion update extension -->
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="OpenAI-based image generation and upload to AEM extension">
        <div class="card">
            <div class="card-image">
                <figure class="image is-16by9">
                    <a href="./example-extensions/image-generation-and-image-upload.md" title="Génération d’images basée sur OpenAI et chargement vers l’extension AEM" tabindex="-1">
                        <img class="is-bordered-r-small" src="./example-extensions/assets/digital-image-generation/screenshot.png" alt="Génération d’images basée sur OpenAI et chargement vers l’extension AEM">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small">
                <div class="content">
                    <p class="headline is-size-5 has-text-weight-bold">Génération d’images basée sur OpenAI et chargement vers l’extension AEM</p>
                    <p class="is-size-6">Explorez un exemple d’extension de barre d’actions qui génère une image à l’aide d’OpenAI, la charge dans AEM et met à jour la propriété de l’image sur le fragment de contenu sélectionné.</p>
                    <a href="./example-extensions/image-generation-and-image-upload.md" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM">
<span class="spectrum-Button-label has-no-wrap has-text-weight-bold">Explorer l’exemple d’extension</span>
</a>
                </div>
            </div>
        </div>
    </div>



</div>
