---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 1%

---


# Intégration d’Adobe Analytics à Real-time Customer Data Platform

{{analytics-description}}

{{real-time-cdp-description}}

L’intégration d’Adobe Analytics à Adobe Real-time Customer Data Platform (RTCDP) peut offrir plusieurs avantages aux entreprises qui souhaitent améliorer leurs expériences client et leurs efforts marketing. Voici quelques-uns des avantages clés :

+ **Amélioration du ciblage et de la personnalisation des audiences**: Un marketing précis sur les appareils et canaux, des messages personnalisés pour un engagement optimisé.
+ **Optimisation améliorée des landing pages**: Expériences personnalisées basées sur l’appareil et le comportement, ce qui améliore la satisfaction et la conversion des utilisateurs.
+ **Activation transparente de l’audience**: Utilisez les profils client pour un ciblage efficace par le biais de canaux privilégiés, en fournissant des messages pertinents.

En combinant Adobe Analytics et Real-Time CDP, les entreprises peuvent passer leurs efforts marketing au niveau supérieur, en proposant des expériences personnalisées, en augmentant l’engagement des clients et en optimisant les conversions sur différents points de contact numériques.

<table>
    <thead>
        <tr>
            <th>Applications Experience Cloud</th>
            <th>Intégrer à l’aide de</th>
            <th>Quand l’utiliser</th>
            <th>Cas d’utilisation courants</th>
        </tr>
    </thead>
    <tr>
        <td><a href="../../integrations/tutorials/analytics-real-time-cdp/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics avec Real-Time CDP</a></td>
        <td>Connecteur source Experience Platform</td>
        <td>
            <ul>
                <li>Lorsque vous souhaitez ingérer des données Analytics dans Experience Platform à partir de vos suites de rapports.</li>
                <li>Lorsque la disponibilité des données pour Customer Profile peut se situer entre 2 et 30 minutes à compter du moment de la collecte des données, et la disponibilité pour le lac de données peut atteindre 90 minutes.</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Workflow directement initié par l’interface utilisateur.</li>
                <li>Mappage de l’interface utilisateur pour copier des props et des eVars Analytics dans de nouveaux champs XDM.</li>
                <li>Le moyen le plus rapide d’obtenir de la valeur à partir de Real-time Customer Profile et de Customer Journey Analytics.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://adobe.com" target="_blank" rel="noreferrer">TODO : Analytics avec Real-Time CDP</a></td>
        <td>Experience Platform Edge</td>
        <td>
            <ul>
                <li>Lorsque vous mettez en oeuvre une stratégie d’analyse à long terme.</li>
                <li>Lorsque vous êtes un nouveau client ou un client existant qui a besoin de la disponibilité des données Analytics pour le profil client afin de prendre en charge les mêmes cas d’utilisation de la personnalisation de la page suivante et de la même page.</li>
            </ul>
        </td>
        <td>
            <ul>
                <li>Fournit le meilleur niveau de contrôle pour les données collectées à utiliser pour prendre en charge vos cas d’utilisation.</li>
                <li>Les données côté client sont facilement mappées aux champs XDM.</li>
                <li>Disponibilité des données la plus rapide pour Real-Time Customer Profile.</li>
            </ul>
        </td>
    </tr>            
</table>
