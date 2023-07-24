---
title: Personnalisation à grande échelle
description: Faites en sorte que les expériences personnalisées soient prises en compte à chaque instant.
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 0%

---


# Personnalisation à grande échelle

Dans un paysage actuel hautement compétitif et axé sur le numérique, les clients en sont venus à s’attendre à des expériences adaptées à leurs préférences et à leurs besoins uniques. L’exploitation des fonctionnalités de Adobe Experience Cloud nous permet de collecter et d’analyser des données client complètes, ce qui nous permet de disposer d’informations précieuses sur les comportements, les intérêts et les préférences. Cette compréhension profonde facilite la diffusion d’expériences personnalisées sur différents points de contact, assurant des interactions significatives et attrayantes. L’exploitation de la puissance de Adobe Experience Cloud libère tout le potentiel de la personnalisation, favorise des relations plus étroites avec les clients, favorise la fidélité et favorise la croissance des entreprises.

<table>
  <thead>
    <tr>
      <th>Cas d’utilisation d’intégration</th>
      <th>Description de l’intégration</th>
      <th>Exemples</th>
      <th>Applications Experience Cloud</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Créer des documents de PDF personnalisés</strong></td>
      <td>
        Générer des documents de communication pour la signature en fonction des sélections/préférences des utilisateurs.
      </td>
      <td>
        <ul>
          <li>
            Présenter un NDA généré dynamiquement en fonction des données d’un envoi AEM Forms pour signature
          </li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-acrobat-sign.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Forms et signature</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Analyse et reporting des données</strong></td>
      <td>
        Analyse des données comportementales à partir d’expériences numériques <br />Utilisez les données comportementales Adobe Analytics dans Analysis Workspace en Customer Journey Analytics.
      </td>
      <td>
        <ul>
          <li>Analyse des chemins de conversion haut/bas</li>
          <li>Analyse de l’engagement et de la conversion des canaux</li>
          <li>Présentation du contenu le plus consulté</li>
          <li>Présentation des catégories de produits et des produits les plus courants</li>
          <li>
            Exécution d’une analyse de l’utilisation des outils pour optimiser les expériences en libre-service
          </li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics et Customer Journey Analytics</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Reporting pour les activités de personnalisation<br />Analysez les résultats des tests d’optimisation, y compris les tests A/B, en utilisant Adobe Target et en générant des rapports complets via Adobe Analytics.
      </td>
      <td>
        <ul>
          <li>Afficher les résultats du test A/B dans les rapports d’analyse riches</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics et Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Personnaliser des diffusions email</strong></td>
      <td>
        Personnalisez des diffusions email avec du contenu dynamique en tirant parti des fonctionnalités d’Adobe Target.
      </td>
      <td>
        <ul>
          <li>Ajout d’offres personnalisées aux e-mails des clients</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/campaign//campaign-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Campaign et Target</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2">
        <strong>Développer les audiences pour la personnalisation et les plateformes publicitaires</strong>
      </td>
      <td>
        Utilisez les segments d’Audience Manager pour créer des audiences dans Real-Time CDP à utiliser dans les tactiques de personnalisation et de remarketing.
      </td>
      <td>
        <ul>
          <li>
            Effectuez un ciblage et une personnalisation anonymes d’audience numérique sur le site web, l’application mobile ou sur les canaux publicitaires pris en charge
          </li>
          <li>
            Optimiser les expériences de page d’entrée et de pré-authentification en fonction des caractéristiques de comportement et de périphérique connues
          </li>
          <li>
            Tirez parti du réseau de données tiers d’Audience Manager pour affiner et développer davantage vos audiences en vue du ciblage.
          </li>
          <li>Partage des segments d’Audience Manager sur la plateforme RTCDP</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/aam/aam-rtcdp.md"
          target="_blank"
          rel="noopener noreferrer"
          >Audience Manager et Real-time Customer Data Platform</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Utilisez les données Analytics pour créer des audiences à utiliser dans des tactiques de personnalisation ou de remarketing.
      </td>
      <td>
        <ul>
          <li>
            Effectuez le ciblage et la personnalisation d’audience numérique sur les appareils ou les canaux publicitaires pris en charge.
          </li>
          <li>
            Optimisez les landing pages clients connues et les expériences anonymes en fonction des attributs de périphérique et de comportement.
          </li>
          <li>Activez les audiences sur les canaux connus, tels que les emails et les SMS.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/analytics/analytics-customer-journey-analytics.md"
          target="_blank"
          rel="noopener noreferrer"
          >Analytics et Real-time Customer Data Platform</a
        >
      </td>
    </tr>
    <tr>
      <td rowspan="2"><strong>Personnalisation des expériences web</strong></td>
      <td>
        Personnalisez des expériences d’application d’une seule page (SPA) en utilisant efficacement AEM sans affichage conjointement avec Adobe Target.
      </td>
      <td>
        <ul>
          <li>Personnalisation des SPA et des applications mobiles</li>
          <li>Réponses d’API personnalisées.</li>
          <li>Diffusion de contenu ciblée.</li>
          <li>Variations de test A/B.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM sans affichage et Target</a
        >
      </td>
    </tr>
    <tr>
      <td>
        Proposer des expériences web personnalisées en utilisant efficacement AEM Sites et Adobe Target pour la personnalisation.
      </td>
      <td>
        <ul>
          <li>AEM personnalisation du site web.</li>
          <li>Personnalisez le contenu du site web.</li>
          <li>Optimisez les expériences utilisateur.</li>
          <li>Variations de test A/B.</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/experience-manager/experience-manager-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >AEM Sites et  Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Personnalisation des expériences numériques</strong></td>
      <td>
        Utilisez les profils client en temps réel et les segments Platform gérés de manière centralisée pour personnaliser les messages sur les canaux web, mobiles et autres canaux numériques.
      </td>
      <td>
        <ul>
          <li>Personnalisation du contenu pour les visiteurs connus</li>
          <li>Augmenter l’inscription et la participation au programme de fidélité</li>
          <li>Identifier et impliquer les clients à risque d’attrition</li>
          <li>Personnalisation des offres en temps réel</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/rtcdp/rtcdp-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Real-time Customer Data Platform et Target</a
        >
      </td>
    </tr>
    <tr>
      <td><strong>Amélioration de la génération de pistes</strong></td>
      <td>
        Utilisez les profils client en temps réel et les segments Platform gérés de manière centralisée pour personnaliser les messages sur les canaux web, mobiles et autres canaux numériques.
      </td>
      <td>
        <ul>
          <li>Personnalisation du contenu pour les visiteurs connus</li>
        </ul>
      </td>
      <td>
        <a
          href="../integrations-between-applications/rtcdp/rtcdp-target.md"
          target="_blank"
          rel="noopener noreferrer"
          >Real-time Customer Data Platform et Target</a
        >
      </td>
    </tr>
  </tbody>
</table>
