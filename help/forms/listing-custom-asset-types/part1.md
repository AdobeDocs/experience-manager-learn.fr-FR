---
title: Enregistrement des types de ressources personnalisés
seo-title: Enregistrement des types de ressources personnalisés
description: Activation des types de ressources personnalisés pour une liste dans AEM Forms Portal
seo-description: Activation des types de ressources personnalisés pour une liste dans AEM Forms Portal
uuid: eaf29eb0-a0f6-493e-b267-1c5c4ddbe6aa
feature: Formulaires adaptatifs
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Développement
role: Developer
level: Experienced
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 3%

---


# Enregistrement des types de ressources personnalisés {#registering-custom-asset-types}

Activation des types de ressources personnalisés pour une liste dans AEM Forms Portal

>[!NOTE]
>
>Assurez-vous d’avoir AEM 6.3 avec SP1 et l’AEM Forms Add On Installed correspondant. Cette fonctionnalité fonctionne uniquement avec AEM Forms 6.3 SP1 et versions ultérieures.

## Spécifier le chemin de base {#specify-base-path}

Le chemin d’accès de base est le chemin d’accès de référentiel de niveau supérieur qui comprend toutes les ressources qu’un utilisateur peut vouloir répertorier dans le composant Search &amp; Lister. Si vous le souhaitez, l’utilisateur peut également configurer des emplacements spécifiques dans le chemin de base à partir de la boîte de dialogue de modification des composants, de sorte que la recherche soit déclenchée à des emplacements spécifiques plutôt que de rechercher tous les noeuds dans le chemin de base. Par défaut, le chemin de base est utilisé comme critère de chemin de recherche pour récupérer les ressources, sauf si l’utilisateur configure un ensemble de chemins spécifiques à partir de cet emplacement. Il est important de disposer d’une valeur optimale de ce chemin pour effectuer une recherche performante. La valeur par défaut du chemin de base reste **_/content/dam/formsanddocuments_**, car toutes les ressources AEM Forms résident dans **_/content/dam/formsanddocuments._**

Étapes de configuration du chemin de base

1. Connexion à crx
1. Accédez à **/libs/fd/fp/extensions/querybuilder/basepath**

1. Cliquez sur &quot;Noeud de recouvrement&quot; dans la barre d’outils.
1. Assurez-vous que l’emplacement de la superposition est &quot;/apps/&quot;
1. Cliquez sur Ok
1. Cliquez sur Enregistrer
1. Accédez à la nouvelle structure créée à l’adresse **/apps/fd/fp/extensions/querybuilder/basepath**

1. Remplacez la valeur de la propriété path par **&quot;/content/dam&quot;**
1. Cliquez sur Enregistrer

En spécifiant la propriété de chemin sur **&quot;/content/dam&quot;**, vous définissez simplement le chemin de base sur /content/dam. Vous pouvez le vérifier en ouvrant le composant Search and Lister.

![basepath](assets/basepath.png)

## Enregistrement des types de ressources personnalisés {#register-custom-asset-types}

Nous avons ajouté un nouvel onglet (Liste des ressources) dans le composant de recherche et de liste. Cet onglet répertorie les types de ressources prêts à l’emploi et les types de ressources supplémentaires que vous configurez. Par défaut, les types de ressources suivants sont répertoriés :

1. Formulaires adaptatifs
1. Modèles de formulaire
1. Formulaires PDF
1. Document(PDF statiques)

**Étapes d’enregistrement d’un type de ressource personnalisé**

1. Créez un noeud de recouvrement de **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Définissez l’emplacement de la superposition sur &quot;/apps&quot;.
1. Accédez à la nouvelle structure créée dans **/apps/fd/fp/extensions/querybuilder/assettypes **

1. Sous cet emplacement, créez un noeud &#39;nt:unstructured&#39; pour le type à enregistrer, nommez le noeud **mp4files. Ajoutez les deux propriétés suivantes à ce noeud mp4files**

   1. Ajoutez la propriété jcr:title pour spécifier le nom d’affichage du type de ressource. Définissez la valeur de jcr:title sur &quot;Fichiers Mp4&quot;.
   1. Ajoutez la propriété &quot;type&quot; et définissez sa valeur sur &quot;videos&quot;. Il s’agit de la valeur que nous utilisons dans notre modèle pour répertorier les ressources de type vidéo. Enregistrez vos modifications.

1. Créez un noeud de type &quot;nt:unstructured&quot; sous les fichiers mp4. Nommez ce noeud &quot;critères de recherche&quot;.
1. Ajoutez un ou plusieurs filtres sous les critères de recherche. Supposons que l’utilisateur souhaite avoir un filtre de recherche pour répertorier les fichiers mp4Files dont le type MIME est &quot;video/mp4&quot;. Vous pouvez le faire ici.
1. Créez un noeud de type &quot;nt:unstructured&quot; sous les critères de recherche de noeud. Nommez ce noeud &quot;filetypes&quot;.
1. Ajoutez les 2 propriétés suivantes à ce noeud &quot;filetypes&quot;.

   1. name: ./jcr:content/metadata/dc:format
   1. value: video/mp4

1. Cela signifie que les ressources dont la propriété dc:format est égale à video/mp4 sont considérées comme un type de ressource &quot;Vidéos Mp4&quot;. Vous pouvez utiliser n’importe quelle propriété répertoriée sur le noeud &quot;jcr:content/metadata&quot; pour les critères de recherche.

1. **Veillez à enregistrer votre travail.**

Après avoir effectué les étapes ci-dessus, le nouveau type de ressource (Fichiers Mp4) commence à s’afficher dans la liste déroulante Types de ressources du composant Search and Lister, comme illustré ci-dessous.

![mp4files](assets/mp4files.png)

[Si vous rencontrez des problèmes pour que cela fonctionne, vous pouvez importer le package suivant.](assets/assettypeskt1.zip) Deux types de ressources personnalisés sont définis pour le package. Fichiers Mp4 et documents Word. Nous vous recommandons de consulter la section **/apps/fd/fp/extensions/querybuilder/assettypes**

[Installez le package](assets/customportalpage.zip) customeportal. Ce module contient un exemple de page de portail. Cette page sera utilisée dans la partie 2 de ce tutoriel.

