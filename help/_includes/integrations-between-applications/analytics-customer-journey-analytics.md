---
source-git-commit: ed53392381fa568de8230288e6b85c87540222cf
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 1%

---


# Intégration d’Adobe Analytics à Customer Journey Analytics

{{analytics-description}}

{{customer-journey-analytics-description}}

L’intégration d’Adobe Analytics à Customer Journey Analytics offre des avantages clés :

+ **insights complets** en ce qui concerne les comportements et les préférences des clients.
+ **Suivi cross-canal transparent** pour une vision holistique.
+ **Données unifiées et création de rapports** pour une analyse précise.
+ **Personnalisation améliorée** et amélioration de l’engagement des clients.
+ **Informations sur les données en temps réel** pour une prise de décision agile.

## Intégrations courantes

<table>
    <thead>
        <tr>
            <td>Applications Experience Cloud</td>
            <td>Intégrer à l’aide de</td>
            <td>Quand l’utiliser</td>
            <td>Cas d’utilisation courants</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><a href="../../integrations/tutorials/analytics-customer-journey-analytics/experience-platform-source-connector.md" target="_blank" rel="noreferrer">Analytics avec Customer Journey Analytics</a></td>
            <td>Connecteur source Experience Platform</td>
            <td>
                <ul>
                    <li>TODO : Utilisez cette intégration pour ingérer des données Analytics dans Experience Platform à partir de vos suites de rapports.</li>
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
            <td><a href="https://www.adobe.com/" target="_blank" rel="noreferrer">TODO DU LIEN : Analytics avec Customer Journey Analytics</a></td>
            <td>Experience Platform Edge</td>
            <td>
                <ul>
                    <li>Lorsque vous souhaitez mettre en oeuvre une stratégie à long terme. Cela envoie directement des données d’un appareil à l’Experience Platform à l’aide du SDK Web AEP, du SDK AEP Mobile ou de l’API Edge Network Server.</li>
                    <li>Lorsque vous êtes un nouveau client ou un client existant, qui a besoin de la disponibilité des données Analytics pour le profil client afin de prendre en charge les mêmes cas d’utilisation et les cas de personnalisation de la page suivante.</li>
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
    </tbody>          
</table>
