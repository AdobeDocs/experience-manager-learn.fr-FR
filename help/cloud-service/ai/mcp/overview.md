---
title: Serveurs MCP dans AEM
description: Découvrez comment utiliser les serveurs MCP (Model Context Protocol) AEM à partir de vos applications IDE optimisées par l’IA ou basées sur le chat afin de rationaliser et d’accélérer votre travail de contenu AEM.
version: Experience Manager as a Cloud Service
role: Leader, User, Developer
level: Beginner
doc-type: Article
duration: 0
last-substantial-update: 2026-03-04T00:00:00Z
jira: KT-20473
source-git-commit: c5f1c7f57181b1e9de6dd91aa2428f2fe1a04893
workflow-type: tm+mt
source-wordcount: '881'
ht-degree: 0%

---

# Serveurs MCP dans AEM

Découvrez comment utiliser les serveurs AEM _Model Context Protocol (MCP)_ à partir de vos applications IDE optimisées par l’IA ou basées sur le chat pour rationaliser et accélérer votre travail de contenu AEM. Vous décrivez ce que vous souhaitez dans un langage naturel au lieu d’écrire du code API de bas niveau ou de naviguer dans l’interface utilisateur d’AEM.

## Liste des serveurs AEM MCP

Tous les serveurs AEM MCP sont disponibles sous `https://mcp.adobeaemcloud.com/adobe/mcp/`. Voir [Utilisation de MCP avec AEM as a Cloud Service](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service) pour plus d’informations.

- **Contenu** (`/content`) : accès complet pour créer, lire, mettre à jour et supprimer des pages, des fragments et des ressources.
- **Contenu (lecture seule)** (`/content-readonly`) — En lecture seule pour répertorier et obtenir des pages, des fragments et des ressources (aucune modification).
- **Cloud Manager** (`/cloudmanager`) — Pour gérer les programmes, environnements, référentiels et pipelines Adobe Cloud Manager.

>[!TIP]
>
>Les outils exposés par chaque serveur peuvent changer au fil du temps. Pour afficher les fonctionnalités disponibles maintenant, demandez à votre IA de répertorier tous les outils de MCP d’AEM (par exemple, `List all AEM MCP tools available from this server and describe what they do`) ou saisissez l’invite `tools/list` dans votre IDE.

## Modèles d’utilisation du serveur MCP

Avant de commencer à utiliser les serveurs MCP AEM, nous allons examiner les deux principaux schémas d’utilisation des serveurs MCP :

- **Centré sur l&#39;humain** - Vous êtes aux commandes. Vous demandez, l’IA suggère ou exécute des outils pour vous dans l’IDE.
- **Agentic** — Une application agentic (agent ou sous-agent) appelle le serveur seule, en choisissant des outils et en travaillant vers un objectif avec peu d&#39;entrée humaine.

Voici comment ces deux schémas d’utilisation se comparent :

| Aspect | Centré sur l&#39;humain | Agentic |
| ------ | ------------- | ------- |
| **Qui mène les actions** | Toi. <br> L’IA propose ou exécute des outils pour vous dans l’application basée sur l’IDE ou le chat. | L’IA. <br> Il sélectionne les outils à utiliser et continue de le faire avec un minimum de conseils. |
| **Autorité de décision** | Vous gardez le contrôle. Vous approuvez ou déclenchez chaque étape. | L&#39;IA a plus de liberté. Les actions à fort impact peuvent nécessiter des mécanismes de sécurisation ou des approbations. |
| **Modèle d’utilisation standard** | **Par développeur**, vous pouvez l’utiliser à partir de votre propre application basée sur un IDE ou un chat, un développeur par session, ce qui est utile pour le travail de développement quotidien. | **Partagé** via une application agentic, comme services partagés et passerelles pour de nombreux utilisateurs ou agents. |
| **Convient le mieux à** | Réviser le contenu, effectuer des mises à jour guidées, explorer ou répéter des tâches tout en restant dans la boucle. | Workflows d’agent, traitements par lots, pipelines et objectifs pour lesquels le système doit s’exécuter avec une intervention minimale. |

### Lors de l’utilisation de MCP dans des systèmes Agentic

Les serveurs MCP sont conçus pour les **clients MCP à commande humaine** avec une supervision humaine et une expérience utilisateur interactive. La spécification des outils MCP recommande _un humain dans la boucle_ qui peut approuver ou refuser les appels à l’outil.

Si vous utilisez des serveurs MCP dans un système autonome ou autonome, traitez-le comme un niveau de compatibilité distinct. Placer sur la liste autorisée N’utilisez **pas de code en dur** les noms d’outil dans _invites_, __ ou _logique de routage_. Dans MCP, le _nom de l’outil_ est un identifiant programmatique, le _description_ est l’indice associé au modèle pour le LLM. Privilégiez les invites et la sélection basées sur la fonctionnalité ou la description.

Implémentez la découverte d’exécution via `tools/list`, gérez les modifications de liste d’outils (`notifications/tools/list_changed`) et alignez-vous sur le fournisseur de serveur MCP pour l’intégration et le contrôle de version si vous avez besoin de garanties de stabilité au-delà de la ligne de base du protocole.

## Entités MCP et leur mappage

MCP s’articule autour de trois entités : **hôte**, **client** et **serveur**. La [spécification MCP](https://modelcontextprotocol.io/docs/getting-started/intro) les définit formellement. Cependant, le tableau ci-dessous explique chacun en termes simples et son mappage lors de l’utilisation de serveurs MCP AEM.

| Composant | Définition standard | Lors de l’utilisation de serveurs MCP AEM |
| --------- | ------------------- | ---------------- |
| **Hôte** | L’application qui exécute tout, rassemble le contexte, parle à l’IA, gère les autorisations et crée des clients. | Votre application **IDE** (curseur) ou basée sur un chat est l’hôte. Il exécute le client MCP et décide quels outils et serveurs votre session peut utiliser. |
| **Client** | Connexion unique de l’hôte à un serveur. Il transmet les messages entre eux et maintient l’accès de ce serveur à l’écart des autres. | Le **client MCP** réside dans votre application basée sur l’IDE ou le chat. Lorsque vous ajoutez le serveur AEM Content MCP dans les paramètres, l’application basée sur l’IDE ou le chat crée un client qui communique avec ce serveur. Vos invites et appels d&#39;outils passent par ce client. |
| **Serveur** | Un service qui expose les outils, les données et les invites via MCP. Il peut être exécuté sur votre ordinateur ou à distance. | Les **serveurs AEM MCP hébergés par Adobe** offrent des outils de création, de lecture, de mise à jour et de suppression de pages, de fragments de contenu et de ressources afin que l’IA de votre IDE ou de votre application basée sur une conversation puisse fonctionner avec votre environnement AEM. |

En d’autres termes, **Hôte** est votre application basée sur un IDE ou un chat, **Client** est la connexion de l’application basée sur un IDE ou un chat à AEM, **Serveur** est le serveur AEM MCP hébergé par Adobe qui effectue le travail.

## Configuration

Les serveurs AEM MCP sont conçus pour fonctionner avec un ensemble défini d’applications compatibles avec MCP. Les applications suivantes sont officiellement prises en charge :

- [Anthropique Claude](https://claude.com/product/overview)
- [Curseur](https://www.cursor.com/)
- [OpenAI ChatGPT](https://chatgpt.com/)
- [Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)

Voir [Présentation de la configuration](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/ai-in-aem/using-mcp-with-aem-as-a-cloud-service#setup-overview) pour plus d’informations.

## Cas d’utilisation

<!-- CARDS
{target = _self}

* ./accelerate-content-operations-with-aem-mcp-server.md    
  {title = Accelerate Content Operations with AEM MCP Server}
  {description = Learn how to use the AEM Content MCP Server from Cursor IDE to streamline and accelerate your AEM content work.}
  {image = ../assets/content-mcp-server/update-adventure-price-prompt-response.png}
  {cta = Learn Content MCP Server}

* ./cloud-manager.md
  {title = Cloud Manager MCP Server}
  {description = Learn how to use the AEM Cloud Manager MCP Server from Cursor IDE to streamline and accelerate your AEM cloud manager work.}
  {image = ../assets/cm-mcp-server/start-pipeline.png}
  {cta = Learn Cloud Manager MCP Server}
-->
<!-- START CARDS HTML - DO NOT MODIFY BY HAND -->
<div class="columns">
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Accelerate Content Operations with AEM MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./accelerate-content-operations-with-aem-mcp-server.md" title="Accélérer les opérations de contenu avec le serveur AEM MCP" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/content-mcp-server/update-adventure-price-prompt-response.png" alt="Accélérer les opérations de contenu avec le serveur AEM MCP"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" title="Accélérer les opérations de contenu avec le serveur AEM MCP">Accélérer les opérations de contenu avec le serveur AEM MCP</a>
                    </p>
                    <p class="is-size-6">Découvrez comment utiliser le serveur AEM Content MCP à partir de l’IDE du curseur pour rationaliser et accélérer votre travail de contenu AEM.</p>
                </div>
                <a href="./accelerate-content-operations-with-aem-mcp-server.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus sur le serveur Content MCP</span>
                </a>
            </div>
        </div>
    </div>
    <div class="column is-half-tablet is-half-desktop is-one-third-widescreen" aria-label="Cloud Manager MCP Server">
        <div class="card" style="height: 100%; display: flex; flex-direction: column; height: 100%;">
            <div class="card-image">
                <figure class="image x-is-16by9">
                    <a href="./cloud-manager.md" title="Serveur MCP Cloud Manager" target="_self" rel="referrer">
                        <img class="is-bordered-r-small" src="../assets/cm-mcp-server/start-pipeline.png" alt="Serveur MCP Cloud Manager"
                             style="width: 100%; aspect-ratio: 16 / 9; object-fit: cover; overflow: hidden; display: block; margin: auto;">
                    </a>
                </figure>
            </div>
            <div class="card-content is-padded-small" style="display: flex; flex-direction: column; flex-grow: 1; justify-content: space-between;">
                <div class="top-card-content">
                    <p class="headline is-size-6 has-text-weight-bold">
                        <a href="./cloud-manager.md" target="_self" rel="referrer" title="Serveur MCP Cloud Manager">Serveur Cloud Manager MCP</a>
                    </p>
                    <p class="is-size-6">Découvrez comment utiliser le serveur AEM Cloud Manager MCP à partir de l’IDE Cursor pour rationaliser et accélérer votre travail dans AEM Cloud Manager.</p>
                </div>
                <a href="./cloud-manager.md" target="_self" rel="referrer" class="spectrum-Button spectrum-Button--outline spectrum-Button--primary spectrum-Button--sizeM" style="align-self: flex-start; margin-top: 1rem;">
                    <span class="spectrum-Button-label has-no-wrap has-text-weight-bold">En savoir plus sur le serveur MCP Cloud Manager</span>
                </a>
            </div>
        </div>
    </div>
</div>
<!-- END CARDS HTML - DO NOT MODIFY BY HAND -->
