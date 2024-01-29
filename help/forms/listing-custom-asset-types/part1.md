---
title: Enregistrer des types de ressources personnalisés
description: Activez des types de ressources personnalisés pour une liste dans le portail AEM Forms.
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 99944f44-0985-4320-b437-06c5adfc60a1
topic: Development
role: Developer
level: Experienced
exl-id: da613092-e03b-467c-9b9e-668142df4634
last-substantial-update: 2019-07-11T00:00:00Z
duration: 165
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '644'
ht-degree: 100%

---

# Enregistrer des types de ressources personnalisés {#registering-custom-asset-types}

Activez des types de ressources personnalisés pour une liste dans le portail AEM Forms.

>[!NOTE]
>
>Assurez-vous que vous disposez de la version d’AEM 6.3 avec SP1 et que les modules complémentaires d’AEM Forms sont bien installés. Cette fonctionnalité fonctionne uniquement avec AEM Forms 6.3 SP1 et versions ultérieures.

## Spécifier le chemin d’accès de base {#specify-base-path}

Le chemin d’accès de base est le chemin d’accès du référentiel de niveau supérieur qui comprend toutes les ressources qu’un utilisateur ou une utilisatrice peut vouloir répertorier dans le composant Search &amp; Lister. Si vous le souhaitez, l’utilisateur ou l’utilisatrice peut également configurer des emplacements spécifiques dans le chemin d’accès de base à partir de la boîte de dialogue de modification des composants, de sorte que la recherche soit déclenchée à des emplacements spécifiques plutôt que de rechercher tous les nœuds dans le chemin d’accès de base. Par défaut, le chemin d’accès de base est utilisé comme critère de chemin d’accès de recherche pour récupérer les ressources, sauf si l’utilisateur ou l’utilisatrice configure un ensemble de chemins d’accès spécifiques à partir de cet emplacement. Il est important de disposer d’une valeur optimale de ce chemin d’accès pour effectuer une recherche performante. La valeur par défaut du chemin d’accès de base reste **_/content/dam/formsanddocuments_** car toutes les ressources AEM Forms résident dans **_/content/dam/formsanddocuments._**

Étapes à suivre pour configurer le chemin de base

1. connectez-vous à CRX ;
1. Accédez à **/libs/fd/fp/extensions/querybuilder/basepath**

1. Cliquez sur « nœud de recouvrement » dans la barre d’outils.
1. Assurez-vous que l’emplacement de recouvrement est « /apps/ ».
1. Cliquez sur OK.
1. Cliquez sur Enregistrer.
1. Accédez à la nouvelle structure créée à l’adresse **/apps/fd/fp/extensions/querybuilder/basepath**

1. Remplacez la valeur de la propriété du chemin d’accès par **« /content/dam »**.
1. Cliquez sur Enregistrer.

En spécifiant la propriété du chemin d’accès sur **« /content/dam »**, vous définissez simplement le chemin d’accès de base sur /content/dam. Vous pouvez le vérifier en ouvrant le composant Search &amp; Lister.

![basepath](assets/basepath.png)

## Enregistrer des types de ressources personnalisés {#register-custom-asset-types}

Nous avons ajouté un nouvel onglet (Liste des ressources) dans le composant Search &amp; Lister. Cet onglet répertorie les types de ressources prêts à l’emploi et les types de ressources supplémentaires que vous configurez. Par défaut, les types de ressources suivants sont répertoriés :

1. Formulaires adaptatifs
1. Modèles de formulaire
1. Formulaires PDF
1. Document (PDF statiques)

**Étapes à suivre pour enregistrer un type de ressource personnalisé**

1. Créez un nœud de recouvrement de **/libs/fd/fp/extensions/querybuilder/assettypes**

1. Définissez l’emplacement du recouvrement sur « /apps ».
1. Accédez à la nouvelle structure créée à l’adresse `/apps/fd/fp/extensions/querybuilder/assettypes`.

1. Sous cet emplacement, créez un nœud « nt:unstructured » pour le type à enregistrer, et nommez le nœud **mp4files. Ajoutez les deux propriétés suivantes à ce nœud mp4files**.

   1. Ajoutez la propriété jcr:title pour spécifier le nom d’affichage du type de ressource. Définissez la valeur de jcr:title sur « Fichiers Mp4 ».
   1. Ajoutez la propriété « type » et définissez sa valeur sur « vidéos ». Il s’agit de la valeur que nous utilisons dans notre modèle pour répertorier les ressources de type vidéo. Enregistrez vos modifications.

1. Créez un nœud de type « nt:unstructured » sous mp4files. Nommez ce nœud « searchcriteria ».
1. Ajoutez un ou plusieurs filtres sous les critères de recherche. Supposons que l’utilisateur ou l’utilisatrice souhaite avoir un filtre de recherche pour répertorier les fichiers mp4 dont le type MIME est « video/mp4 ». Vous pouvez le faire ici.
1. Créez un nœud de type « nt:unstructured » sous le nœud searchcriteria. Nommez ce noeud « filetypes ».
1. Ajoutez les 2 propriétés suivantes à ce nœud « filetypes »

   1. Nom :/jcr:content/metadata/dc:format
   1. Valeur : video/mp4

1. Cela signifie que les ressources dont la propriété dc:format est égale à video/mp4 sont considérées comme un type de ressource « Vidéos Mp4 ». Vous pouvez utiliser n’importe quelle propriété répertoriée sur le noeud « jcr:content/metadata » comme critère de recherche.

1. **Veillez à enregistrer votre travail.**

Une fois les étapes ci-dessus effectuées, le nouveau type de ressource (Fichiers Mp4) s’affiche dans la liste déroulante Types de ressources du composant Search &amp; Lister, comme illustré ci-dessous.

![mp4files](assets/mp4files.png)

[Si vous rencontrez des problèmes, importez le package suivant.](assets/assettypeskt1.zip)Deux types de ressources personnalisés composent le package. Fichiers Mp4 et documents Word. Consultez **/apps/fd/fp/extensions/querybuilder/assettypes**

[Installez le package customeportal](assets/customportalpage.zip). Il contient un exemple de page de portail. Cette page est utilisée dans la 2ème partie du tutoriel.
