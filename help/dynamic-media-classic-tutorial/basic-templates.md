---
title: Présentation des modèles de base
description: Découvrez les modèles de base dans Dynamic Media Classic, les modèles basés sur des images appelés à partir du serveur d’images et composés d’images et de texte rendu. Un modèle peut être modifié dynamiquement à partir de l’URL qui suit la publication du modèle. Vous apprendrez comment charger un PSD Photoshop vers Dynamic Media Classic pour l’utiliser comme base d’un modèle. Créez un modèle de base de marchandisage simple constitué de calques d’image. Ajoutez des calques de texte et rendez-les variables à l’aide de paramètres. Créez une URL de modèle et manipulez l’image de manière dynamique par le biais du navigateur web.
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: d4e16b45-0095-44b4-8c16-89adc15e0cf9
source-git-commit: f0c6e6cd09c1a2944de667d9f14a2d87d3e2fe1d
workflow-type: tm+mt
source-wordcount: '6260'
ht-degree: 0%

---

# Présentation des modèles de base {#basic-templates}

En termes Dynamic Media Classic, un modèle est un document qui peut être modifié dynamiquement via l’URL après la publication du modèle. Dynamic Media Classic propose des modèles de base, des modèles basés sur des images appelés à partir du serveur d’images et composés d’images et de texte rendu.

L’un des aspects les plus puissants des modèles est qu’ils comportent des points d’intégration directs qui vous permettent de les lier à votre base de données. Ainsi, non seulement vous pouvez diffuser une image et la redimensionner, mais vous pouvez également interroger votre base de données pour rechercher des articles nouveaux ou vendus et les faire apparaître comme une superposition sur l’image. Vous pouvez demander une description de l’élément et le faire apparaître sous forme de libellé dans une police choisie et mise en page. Les possibilités sont illimitées.

Les modèles de base peuvent être implémentés de différentes manières, de simple à complexe. Par exemple :

- Marchandisage de base. Utilise des étiquettes telles que &quot;livraison gratuite&quot; si ce produit dispose d’une livraison gratuite. Ces libellés sont configurés par l’équipe de marchandisage de Photoshop. Le web utilise la logique pour savoir quand les appliquer à l’image.
- Marchandisage avancé. Chaque modèle comporte plusieurs variables et peut afficher plusieurs options en même temps. Utilise une base de données, un inventaire et des règles de fonctionnement pour déterminer quand afficher un produit sous la forme &quot;Just In&quot;, &quot;Clearance&quot; ou &quot;Srevente&quot;. Vous pouvez également utiliser la transparence derrière le produit pour l’afficher sur différents arrière-plans, par exemple dans différentes pièces. Les mêmes modèles et/ou ressources peuvent être réutilisés sur la page des détails du produit pour afficher une version plus grande ou agrandie du même produit sur différents arrière-plans.

Il est important de comprendre que Dynamic Media Classic ne fournit que la partie visuelle de ces applications basées sur des modèles. Les sociétés Dynamic Media Classic ou leurs partenaires d’intégration doivent fournir les règles commerciales, la base de données et les compétences de développement nécessaires pour créer les applications. Il n’existe pas d’application de modèle &quot;intégrée&quot; ; les concepteurs configurent le modèle dans Dynamic Media Classic et les développeurs utilisent des appels d’URL pour modifier les variables du modèle.

À la fin de cette section du tutoriel, vous saurez comment :

- Téléchargez un PSD Photoshop vers Dynamic Media Classic pour l’utiliser comme base d’un modèle.
- Créez un modèle de base de marchandisage simple constitué de calques d’image.
- Ajoutez des calques de texte et rendez-les variables à l’aide de paramètres.
- Créez une URL de modèle et manipulez l’image de manière dynamique par le biais du navigateur web.

>[!NOTE]
>
>Toutes les URL de ce chapitre sont proposées à titre d’illustration uniquement ; ce ne sont pas des liens en direct.

## Présentation des modèles de base

La définition d’un modèle de base (ou simplement &quot;modèle&quot;, en abrégé) est une image superposée pouvant contenir des URL. Le résultat final est une image, mais qui peut être modifiée par l’URL. Il peut se composer de photos, de texte ou d’images, quelle que soit la combinaison de ressources de TIFF P dans Dynamic Media Classic.

Les modèles sont très similaires aux fichiers de PSD Photoshop, car ils disposent d’un workflow similaire et de fonctionnalités similaires.

- Les deux se composent de couches qui sont comme des feuilles d&#39;Acétate empilé. Vous pouvez composer des images partiellement transparentes et les voir à travers les zones transparentes d’un calque jusqu’aux calques ci-dessous.
- Les calques peuvent être déplacés et pivotés pour repositionner le contenu. L’opacité et les modes de fusion peuvent être modifiés pour rendre le contenu partiellement transparent.
- Vous pouvez créer des calques basés sur du texte. La qualité peut être très élevée, car le serveur d’images utilise le même moteur de texte que Photoshop et Illustrator.
- Des styles de calque simples peuvent être appliqués à chaque calque pour créer des effets spéciaux tels que des ombres portées ou des lueurs.

Cependant, contrairement aux PSDs Photoshop, les calques peuvent être entièrement dynamiques et contrôlés via une URL sur le serveur d’images.

- Vous pouvez ajouter des variables à toutes les propriétés du modèle, ce qui facilite la modification à la volée de sa composition.
- Les variables appelées paramètres vous permettent d’afficher uniquement la partie du modèle que vous souhaitez modifier.

Il vous suffit d’ajouter un espace réservé pour chaque calque qui varie au lieu de placer tous les calques dans un seul fichier comme vous le faites dans Photoshop, de les afficher et de les masquer (mais vous pouvez également le faire si vous le souhaitez).

À l’aide d’un espace réservé, vous pouvez remplacer dynamiquement le contenu d’un calque par une autre ressource publiée, et il récupère automatiquement les mêmes propriétés (telles que la taille et la rotation) du calque qu’il a remplacé.

Comme les modèles de base sont généralement conçus dans Photoshop mais déployés via une URL, un projet de modèle nécessite un mélange de compétences techniques et de conception. Nous supposons généralement que la personne qui effectue le travail de modèle créatif est un concepteur Photoshop et que la personne qui le met en oeuvre est un développeur web. Les équipes de création et de développement doivent travailler en étroite collaboration pour que le modèle soit couronné de succès.

Les projets de modèle peuvent être relativement simples ou extrêmement complexes en fonction des règles métier et des besoins de l’application. Les modèles de base sont appelés à partir du serveur d’images. Cependant, en raison de la flexibilité de l’environnement Dynamic Media Classic, vous pouvez même imbriquer des modèles dans d’autres modèles, ce qui vous permet de créer des images assez complexes qui peuvent être liées par des variables communément nommées.

- En savoir plus sur [Concepts de base des modèles](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/quick-start-template-basics.html).
- Découvrez comment créer une [Modèle de base](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html#creating_a_template).

## Création d’un modèle de base

Lorsque vous utilisez un modèle de base, vous suivez généralement les étapes du workflow dans le diagramme ci-dessous. Les étapes marquées de lignes pointillées sont facultatives si vous utilisez des calques de texte dynamiques et sont indiquées dans les instructions ci-dessous sous la forme &quot;Processus de texte&quot;. Si vous n’utilisez pas de texte, suivez le chemin principal uniquement.

![image](assets/basic-templates/basic-templates-1.png)

_Workflow Modèle de base ._

1. Concevez et créez vos ressources. La plupart des utilisateurs effectuent cette opération dans Adobe Photoshop. Concevez des ressources à la taille exacte dont vous avez besoin : s’il s’agit d’une image de 200 pixels pour une page miniature, concevez-la à 200 pixels. Si vous devez effectuer un zoom dessus, concevez-le à une taille d’environ 2 000 pixels. Utilisez Photoshop (et/ou Illustrator enregistré en tant qu’image bitmap) pour créer les ressources et utilisez Dynamic Media Classic pour assembler les parties, gérer les calques et ajouter des variables.
2. Après avoir conçu des ressources graphiques, chargez-les dans Dynamic Media Classic. Plutôt que de charger des ressources individuelles à partir du PSD, nous vous recommandons de charger l’intégralité de votre fichier de PSD superposé et de faire en sorte que Dynamic Media Classic crée un fichier par couche, en utilisant la variable **Conserver les calques** lors du téléchargement (voir ci-dessous pour plus d’informations). _Processus de texte : Si vous créez du texte dynamique, chargez également vos polices. Le texte dynamique est variable et contrôlé via l’URL. Si votre texte est statique ou ne comporte que quelques phrases courtes qui ne changent pas (par exemple, des balises qui indiquent &quot;Nouveau&quot; ou &quot;Vente&quot; plutôt que &quot;X % de réduction&quot;, le X étant un numéro variable), nous vous recommandons de pré-restituer le texte dans Photoshop et de le transférer sous forme d’images sous forme de calques pixellisés. C’est plus facile et vous pouvez mettre le texte en forme exactement comme vous le souhaitez._
3. Créez le modèle dans Dynamic Media Classic à l’aide de l’éditeur Concepts de base des modèles du menu Créer et ajoutez des calques d’image. Processus de texte : Créez des calques de texte dans le même éditeur. Cette étape est requise lors de la création manuelle d’un modèle dans Dynamic Media Classic. Sélectionnez une taille de zone de travail correspondant à votre conception, faites glisser et déposez les images sur la zone de travail, puis définissez les propriétés du calque (taille, rotation, opacité, etc.). Vous ne placez pas tous les calques possibles sur votre modèle, un seul espace réservé par calque d’image. _Processus de texte : Vous créez des calques de texte à l’aide de l’outil Texte, comme vous le faites pour créer des calques de texte dans Photoshop. Vous pouvez choisir une police et la mettre en forme à l’aide des mêmes options que celles disponibles avec l’outil Type de Photoshop._ Un autre workflow consiste à charger un PSD et à faire en sorte que Dynamic Media Classic génère un modèle &quot;gratuit&quot;, voire à recréer des calques de texte. Ceci est abordé plus en détail ultérieurement.
4. Une fois les calques créés, ajoutez des paramètres (variables) à n’importe quelle propriété de calque que vous souhaitez contrôler via l’URL, y compris la source du calque (l’image elle-même ). _Processus de texte : Vous pouvez également ajouter des paramètres aux calques de texte, à la fois pour contrôler le contenu du texte, la taille et la position du calque lui-même, ainsi que toutes les options de formatage telles que la couleur de police, la taille de police, le suivi horizontal, etc._
5. Créez un paramètre d’image prédéfini correspondant à la taille de votre modèle. Nous vous recommandons de le faire de sorte que le modèle soit toujours appelé à une taille 1:1, et également d’ajouter de l’accentuation à tous les calques d’image de grande taille qui sont redimensionnés pour s’adapter au modèle. Si vous créez un modèle sur lequel effectuer un zoom, cette étape n’est pas nécessaire.
6. Publiez, copiez l’URL à partir de l’aperçu Dynamic Media Classic et testez-la dans un navigateur.

## Préparation et téléchargement des ressources de modèle vers Dynamic Media Classic

Avant de charger vos ressources de modèle vers Dynamic Media Classic, vous devez effectuer quelques étapes préparatoires.

### Préparation du PSD pour le téléchargement

Avant de charger votre fichier Photoshop vers Dynamic Media Classic, simplifiez les calques de Photoshop afin de faciliter l’utilisation et la compatibilité avec le serveur d’images. Votre fichier de PSD se compose souvent d’éléments que Dynamic Media Classic ne reconnaît pas et vous pouvez également obtenir de nombreux petits éléments difficiles à gérer. Veillez à enregistrer une sauvegarde de votre PSD maître si vous devez modifier l’original ultérieurement. Vous téléchargerez la copie simplifiée, et non le gabarit.

![image](assets/basic-templates/basic-templates-2.jpg)

1. Simplifiez la structure des calques en fusionnant/aplatissant les calques associés qui doivent être activés/désactivés ensemble en un seul calque. Par exemple, le libellé &quot;NEW&quot; et la bannière bleue sont fusionnés en un seul calque afin que vous puissiez les afficher ou les masquer en un seul clic.
   ![image](assets/basic-templates/basic-templates-3.jpg)
2. Certains types de calques et effets de calque ne sont pas pris en charge par Dynamic Media Classic ou le serveur d’images et doivent être pixellisés avant le téléchargement. Sinon, les effets peuvent être ignorés ou les calques ignorés. La pixellisation d’un calque consiste à effectuer une conversion si vous ne pouvez pas être modifié en non modifiable. Pour pixelliser les effets de calque ou les calques de texte, créez un calque vide, sélectionnez les deux et fusionnez à l’aide de **Calques > Fusionner les calques** ou Ctrl + E/CMD + E.

   - Dynamic Media Classic ne peut pas regrouper ni lier des calques. Tous les calques d’un groupe ou d’un ensemble lié sont convertis en calques distincts qui ne sont plus regroupés/liés.
   - Les masques de calque sont convertis en transparence lors du transfert.
   - Les calques de réglage ne sont pas pris en charge et sont ignorés.
   - Les calques de remplissage, tels que les calques de couleur unie, sont pixellisés.
   - Les calques d’objet dynamique et les calques vectoriels sont pixellisés en images normales lors du téléchargement et les filtres intelligents sont appliqués et pixellisés.
   - Les calques de texte seront également pixellisés, sauf si vous utilisez l’option Extract Text (Extraire le texte). Pour plus d’informations, reportez-vous à la section ci-dessous.
   - La plupart des effets de calque sont ignorés et seuls quelques modes de fusion sont pris en charge. En cas de doute, ajoutez des effets simples dans Dynamic Media Classic (ombres intérieures ou extérieures, lueurs intérieure ou extérieure, par exemple) ou utilisez une couche vierge pour fusionner et pixelliser l’effet dans Photoshop.

### Utilisation des polices

Vous pouvez également charger et publier vos polices si vous devez générer du texte dynamique. La seule police incluse avec Dynamic Media Classic est Arial.

Chaque entreprise a la responsabilité d&#39;obtenir une licence d&#39;utilisation d&#39;une police sur le web — le simple fait qu&#39;une police soit installée sur son ordinateur ne vous donne pas le droit de l&#39;utiliser à des fins commerciales sur le web, et votre entreprise pourrait faire face à une action en justice de l&#39;éditeur de la police si elle est utilisée sans autorisation. En outre, les termes de licence varient : il se peut que vous ayez besoin de licences distinctes pour l’affichage d’impression et d’écran, par exemple.

Dynamic Media Classic prend en charge les polices OpenType standard (OTF), TrueType (TTF) et PostScript Type 1. Les polices Mac uniquement pour les valises, les fichiers de collection, les polices système Windows et les polices machine propriétaires (comme les polices utilisées par les machines de gravure ou de broderie) ne sont pas prises en charge. Vous devrez les convertir dans l’un des formats de police standard ou remplacer une police similaire à utiliser dans Dynamic Media Classic et sur le serveur d’images.

Une fois les polices chargées dans Dynamic Media Classic, comme toute autre ressource, elles doivent également être publiées sur le serveur d’images. Une erreur de modèle très courante est d’oublier de publier vos polices, ce qui entraînera une erreur d’image : le serveur d’images ne remplacera pas une autre police à sa place. En outre, si vous souhaitez utiliser la variable **Extraction de texte** lors du téléchargement, vous devez charger vos fichiers de polices avant de charger le PSD qui les utilise. Le **Extraction de texte** tente de recréer votre texte en tant que calque de texte modifiable et de le placer dans un modèle Dynamic Media Classic. Ceci est abordé dans la rubrique suivante, Options de PSD.

Gardez à l’esprit que les polices possèdent plusieurs noms internes souvent différents de leur nom de fichier externe. Vous pouvez voir tous leurs noms différents sur la page Détails de cette ressource dans Dynamic Media Classic. Voici les noms de la police Adobe Caslon Pro Semibold, répertoriés sous l’onglet Métadonnées dans Dynamic Media Classic :

![image](assets/basic-templates/basic-templates-4.jpg)

_Onglet Métadonnées de la page Détails d’une police dans Dynamic Media Classic._

Dynamic Media Classic utilise le nom de fichier de cette police (ACaslonPro-Semibold) comme identifiant de ressource, mais il ne s’agit pas du nom utilisé par le modèle. Le modèle utilise le nom RTF (Rich Text Format), répertorié en bas. RTF est la &quot;langue&quot; native du moteur de texte Image Server.

Si vous devez modifier les polices via l’URL, vous devez appeler le nom RTF de la police (et non l’identifiant de ressource), sinon vous obtiendrez une erreur. Dans ce cas, le nom approprié pour cette police serait &quot;Adobe Caslon Pro&quot;. Nous discuterons plus des polices et de la RTF dans la rubrique Paramètres RTF et de texte, ci-dessous.

Les formats de fichiers de polices les plus courants trouvés sur les systèmes Windows et Mac sont OpenType et TrueType. OpenType possède une extension .OTF, tandis que TrueType est .TTF. Les deux formats fonctionnent également bien dans Dynamic Media Classic.

### Sélection des options lors du téléchargement de votre PSD

Vous n’avez pas besoin de charger un fichier Photoshop (PSD) pour créer un modèle ; un modèle peut être créé à partir de n’importe quelle ressource d’image dans Dynamic Media Classic. Cependant, le chargement d’un PSD peut faciliter la création, car ces ressources se trouvent généralement déjà dans un PSD à plusieurs niveaux. En outre, Dynamic Media Classic génère automatiquement un modèle lorsque vous téléchargez un PSD superposé.

- **Conserver les calques.** Il s’agit de l’option la plus importante. Cela indique à Dynamic Media Classic de créer une ressource image par calque Photoshop. Si cette option n’est pas cochée, toutes les autres options sont désactivées et le PSD est aplati dans une seule image.
- **Créer** **Modèle.** Cette option récupère les différents calques générés et crée automatiquement un modèle en les combinant. L’inconvénient de l’utilisation du modèle généré automatiquement est que Dynamic Media Classic place tous les calques dans un fichier, alors que nous n’avons besoin que d’un seul espace réservé par calque. Il est facile de supprimer les calques supplémentaires, mais s’ils sont nombreux, il est plus rapide de les recréer. Veillez à renommer le nouveau modèle ; dans le cas contraire, il est remplacé la prochaine fois que vous chargez à nouveau le même PSD.
- **Extraire du texte.** Les calques de texte du PSD sont ainsi recréés sous forme de calques de texte dans le modèle à l’aide de la police que vous avez téléchargée. Cette étape est requise si votre texte se trouve sur un chemin d’accès dans Photoshop et que vous souhaitez conserver ce chemin d’accès dans le modèle. Cette fonctionnalité requiert que vous utilisiez la méthode **Créer un modèle** , car le texte extrait ne peut être créé que par un modèle généré lors du téléchargement.
- **Etendre les calques à la taille de l’arrière-plan.** Ce paramètre donne à chaque calque la même taille que le canevas de PSD global. Cela s’avère très utile pour les calques qui resteront toujours en position fixe : sinon, lorsque vous permutez des images dans le même calque, vous devrez peut-être les repositionner.
- **Affectation de nom de calque.** Cela indique à Dynamic Media Classic comment nommer chaque ressource générée par calque. Nous vous recommandons de **Photoshop** **et calque** **Nom** ou Photoshop et **Calque** **Nombre**. Les deux options utilisent le nom du PSD comme première partie du nom et ajoutent le nom du calque ou le numéro à la fin. Par exemple, si vous disposez d’un PSD nommé &quot;shirt.psd&quot; et qu’il comporte des calques nommés &quot;front&quot;, &quot;manches&quot; et &quot;collier&quot;, si vous effectuez un téléchargement à l’aide de la variable **Photoshop et** Calque **Nom** , Dynamic Media Classic génère les identifiants de ressource &quot;shirt_front&quot;, &quot;shirt_manches&quot; et &quot;shirt_collar&quot;. L’utilisation de l’une de ces options permet de s’assurer que le nom est unique dans Dynamic Media Classic.

## Création d’un modèle avec des calques d’image

Même si Dynamic Media Classic peut créer automatiquement un modèle à partir d’un PSD superposé, vous devez savoir comment le créer manuellement. Comme expliqué ci-dessus, il peut arriver que vous ne souhaitiez pas utiliser le modèle créé par Dynamic Media Classic.

### Interface utilisateur des concepts de base des modèles

Commençons par nous familiariser avec l’interface d’édition.

Dans le centre gauche se trouve votre zone de travail qui affiche un aperçu de votre modèle final. Sur le côté droit se trouvent les panneaux Calques et Propriétés du calque . C&#39;est là que vous faites le plus de travail.

![image](assets/basic-templates/basic-templates-5.jpg)

_Créer la page Concepts de base des modèles ._

- **Zone de travail/aperçu.** Voici la fenêtre principale. Vous pouvez y déplacer, redimensionner et faire pivoter les calques à l’aide de la souris. Les contours des calques s’affichent sous la forme de lignes en pointillés.
- **Calques.** Cette opération est similaire au panneau Calques Photoshop . Lorsque vous ajoutez des calques à votre modèle, ils s’affichent ici. Les calques sont empilés de haut en bas : le calque supérieur du panneau Calques est visible au-dessus des autres calques de la liste.
- **Propriétés du calque.** Vous pouvez y ajuster toutes les propriétés d’un calque à l’aide de commandes numériques. Sélectionnez tout d’abord un calque, puis ajustez ses propriétés.
- **Composite** **URL.** La zone URL composite se trouve au bas de l’interface utilisateur. Cela ne sera pas abordé dans cette section du tutoriel. Cependant, ici, votre modèle sera déconstruit sous la forme d’une série de modificateurs d’URL de diffusion d’images. Cette zone est modifiable : si vous connaissez bien les commandes du serveur d’images, vous pouvez modifier manuellement le modèle ici. Cependant, vous pouvez également le briser. Comme Photoshop, la numérotation des calques commence à 0. Le Canevas est le calque 0, et le premier calque que vous ajoutez vous-même est le calque 1. Les modes de fusion déterminent la manière dont les pixels d’un calque se fondent en pixels. Vous pouvez créer divers effets spéciaux à l’aide des modes de fusion.

#### Utilisation de l’éditeur de concepts de base des modèles

Voici les étapes de workflow pour démarrer votre modèle de base :

1. Dans Dynamic Media Classic, accédez à **Créer > Concepts de base des modèles**. Vous pouvez sélectionner n’avoir rien sélectionné ou commencer par sélectionner une image, qui devient le premier calque de votre modèle.
2. Choisissez une taille et appuyez sur **OK**. Cette taille doit correspondre à la taille que vous avez conçue dans Photoshop. L’éditeur de modèles se charge.
3. Si aucune image n’a été sélectionnée à l’étape 1, recherchez ou accédez à une image dans le panneau Ressource à gauche et faites-la glisser sur la zone de travail.

   - L’image est automatiquement redimensionnée pour s’adapter à la taille de la zone de travail. Si vous prévoyez de permuter vos images haute résolution, vous devez généralement importer l’une de vos images grand TIFF P (2 000 pixels) et l’utiliser comme espace réservé.
   - Il doit s’agir du calque le plus en bas de votre modèle. Vous pouvez toutefois réorganiser les calques ultérieurement.

4. Redimensionnez ou repositionnez le calque directement dans la zone de travail ou en réglant les paramètres du panneau Propriétés du calque .
5. Faites glisser d’autres calques d’image si nécessaire. Ajoutez des effets de calques si vous le souhaitez. Voir la rubrique _Ajout d’effets de calque_, ci-dessous.
6. Cliquez sur **Enregistrer**, choisissez un emplacement et attribuez un nom au modèle. Vous pouvez prévisualiser, mais à ce stade, votre modèle ressemblera exactement à une image Photoshop aplatie : il n’est pas encore modifiable.

### Ajout d’effets de calque

Le serveur d’images prend en charge quelques effets de calque programmatiques, c’est-à-dire des effets spéciaux qui modifient l’aspect du contenu d’un calque. Elles fonctionnent de la même manière que les effets de calque dans Photoshop. Ils sont attachés à un calque mais contrôlés indépendamment du calque. Vous pouvez les ajuster ou les supprimer sans apporter de modification permanente au calque lui-même.

- **Ombre portée**. Applique une ombre au-delà des limites du calque, positionnée par un décalage de pixel x et y.
- **Ombre interne**. Applique une ombre à l’intérieur des limites du calque, positionnée par un décalage de pixel x et y.
- **Luminosité extérieure**. Applique un effet d’éclat uniformément sur tous les bords du calque.
- **Luminosité interne**. Applique un effet d’éclat uniformément à l’intérieur de tous les bords du calque.

![image](assets/basic-templates/basic-templates-6.jpg)

_Un calque avec et sans ombre portée_

Pour ajouter un effet, cliquez sur **Ajouter un effet**, puis choisissez un effet dans le menu. Comme les calques normaux, vous pouvez sélectionner un effet dans le panneau Calques et utiliser le panneau Propriétés du calque pour en ajuster les paramètres.

Les effets d’ombre sont décalés horizontalement ou verticalement par rapport au calque, tandis que les effets de lueur sont appliqués uniformément dans toutes les directions. Les effets internes agissent sur les parties opaques du calque, tandis que les effets externes n’affectent que les zones transparentes.

En savoir plus sur[Ajout d’effets de calque](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html#using-shadow-and-glow-effects-on-layers).

### Ajout de paramètres

Si vous combinez simplement des calques et les enregistrez, le résultat net n’est pas différent d’une image Photoshop aplatie. Ce qui rend les modèles spéciaux est la possibilité d’ajouter des paramètres aux propriétés de chaque calque afin qu’elles puissent être modifiées dynamiquement via l’URL.

En termes Dynamic Media Classic, un paramètre est une variable qui peut être liée à une propriété de modèle afin de pouvoir être manipulée via une URL. Lorsque vous ajoutez un paramètre à un calque, Dynamic Media Classic expose cette propriété dans l’URL en ajoutant en préfixe le nom de votre paramètre avec un signe dollar ($) — par exemple, si vous créez un paramètre appelé &quot;taille&quot; pour modifier la taille d’un calque, Dynamic Media Classic renommera votre paramètre $size.

Si vous n’ajoutez pas de paramètre pour une propriété, celle-ci reste masquée dans la base de données Dynamic Media Classic et ne s’affiche pas dans l’URL.

![image](assets/basic-templates/parameters.png)

Sans paramètres, vos URL sont généralement beaucoup plus longues, en particulier si vous utilisez également du texte dynamique. Le texte ajoute plusieurs dizaines de caractères supplémentaires à chaque URL.

Enfin, votre jeu initial de paramètres devient les valeurs par défaut des propriétés du modèle. Si vous créez votre modèle, ajoutez des paramètres, puis appelez l’URL sans ses paramètres, le serveur d’images crée l’image avec tous les paramètres par défaut que vous avez enregistrés dans le modèle. Les paramètres ne sont nécessaires que si vous souhaitez modifier une propriété. Si une propriété n’a pas besoin d’être modifiée, il n’est pas nécessaire de définir un paramètre.

#### Création de paramètres

Il s’agit du workflow de création de paramètres :

1. Cliquez sur le bouton **Paramètres** en regard du nom du calque pour lequel vous souhaitez créer des paramètres. L’écran Paramètres s’affiche. Il répertorie chaque propriété du calque et sa valeur.
1. Sélectionnez la **Activé** en regard du nom de chaque propriété que vous souhaitez transformer en paramètre. Un nom de paramètre par défaut s’affiche. Vous pouvez uniquement ajouter des paramètres aux propriétés qui ont changé de leur état par défaut.

   - Par exemple, si vous ajoutez un calque et que vous le conservez à sa position de xy par défaut de 0,0, Dynamic Media Classic n’expose pas un **Position** . Pour corriger, déplacez le calque d’au moins un pixel. Maintenant Dynamic Media Classic va exposer **Position** en tant que propriété que vous pouvez paramétrer.
   - Pour ajouter un paramètre à la propriété d’affichage/masquage (qui active et désactive le calque), cliquez sur le bouton **Afficher** ou **Masquer le calque** pour désactiver le calque (si vous le souhaitez, vous pouvez l’activer à nouveau par la suite). Dynamic Media Classic expose désormais une **Masquer** qui peuvent être paramétrées.

1. Renommez les noms de paramètre par défaut en un nom plus facile à identifier dans l’URL. Par exemple, si vous souhaitez ajouter un paramètre pour modifier le calque de bannière au-dessus d’une image, remplacez le nom par défaut &quot;layer_2_src&quot; par &quot;banner&quot;.
1. Press **Fermer** pour quitter l’écran Paramètres .
1. Répétez cette opération pour les autres calques en cliquant sur le bouton **Paramètres** et ajouter et renommer des paramètres.
1. Une fois les modifications effectuées, enregistrez-les.

>[!TIP]
>
>Renommez vos paramètres pour qu’ils aient un sens et développez une convention d’affectation des noms pour normaliser ces noms. Assurez-vous que la convention d’affectation des noms a été préalablement convenue par les équipes de conception et de développement.
>
>Impossible d’ajouter un paramètre, car la propriété n’apparaît pas ? Il vous suffit de modifier la propriété du calque à partir de sa valeur par défaut (en le déplaçant, le redimensionnement, le masquage, etc.). Vous devriez maintenant voir cette propriété exposée.

En savoir plus sur [Paramètres de modèle](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html).

## Création d’un modèle avec des calques de texte

Vous allez maintenant apprendre à créer un modèle de base qui comprend des calques de texte.

### Compréhension du texte dynamique

Vous savez maintenant comment créer un modèle de base à l’aide de calques d’image. Pour de nombreuses applications, il ne vous reste plus qu’à en avoir besoin. Comme vous l’avez vu dans l’exercice précédent, les calques dont le texte est simple (comme &quot;Soldes&quot; et &quot;Nouveau&quot;) peuvent être pixellisés et traités comme des images, car leur texte n’a pas besoin d’être modifié.

Toutefois, que se passe-t-il si vous avez besoin de :

- Ajoutez un libellé pour indiquer &quot;25 % de désactivation&quot;, la valeur de 25 % étant variable.
- Ajoutez un libellé de texte avec le nom du produit au-dessus de l’image.
- Localisez vos calques dans différentes langues en fonction du pays dans lequel votre modèle est affiché.

Dans ce cas, vous souhaitez ajouter des calques de texte dynamiques avec des paramètres pour contrôler le texte et/ou la mise en forme.

Pour créer du texte, vous devez charger certaines polices. Sinon, Dynamic Media Classic prend la valeur Arial par défaut. Les polices doivent également être publiées sur le serveur d’images, sinon elles génèrent une erreur dès qu’elles tentent d’effectuer le rendu de tout texte qui utilise cette police.

### Paramètres RTF et texte

Pour ajouter des variables au texte à l’aide de l’outil Concepts de base des modèles , vous devez comprendre comment le texte est rendu. Le serveur d’images génère du texte à l’aide du moteur de texte Adobe, le même moteur que celui utilisé par Photoshop et Illustrator, et le compose en tant que calque dans l’image finale. Pour communiquer avec le moteur, le serveur d’images utilise le format RTF (Rich Text Format).

RTF est une spécification de format de fichier développée par Microsoft pour spécifier la mise en forme des documents. Il s’agit d’un langage de balisage standard utilisé par la plupart des logiciels de traitement de texte et de messagerie électronique. Si vous avez écrit dans une URL &amp;text=\b1 Hello, le serveur d’images génère une image avec le mot &quot;Hello&quot; en gras, car \b1 est la commande RTF permettant de mettre le texte en gras.

La bonne nouvelle est que Dynamic Media Classic génère le RTF pour vous. Chaque fois que vous saisissez du texte dans un modèle et que vous ajoutez une mise en forme, Dynamic Media Classic écrit automatiquement le code RTF dans le modèle. La raison pour laquelle nous la mentionnons est que vous ajoutez des paramètres directement à la RTF elle-même, il est donc important que vous en soyez un peu familier.

#### Création de calques de texte

Vous pouvez créer des calques de texte dans un modèle dans Dynamic Media Classic de deux manières :

1. Outil Texte dans Dynamic Media Classic. Nous discuterons de cette méthode ci-dessous. L’éditeur Concepts de base des modèles dispose d’un outil qui vous permet de créer une zone de texte, de saisir du texte et de le mettre en forme. Dynamic Media Classic génère le RTF selon les besoins et le place dans un calque distinct.
2. Extract Text (lors du transfert). L’autre méthode consiste à créer le calque de texte dans Photoshop et à l’enregistrer dans le PSD en tant que calque de texte normal (au lieu de le pixelliser en tant que calque d’image). Vous téléchargez ensuite le fichier vers Dynamic Media Classic et utilisez le **Extraction de texte** . Dynamic Media Classic convertit chaque calque de texte Photoshop en calque de texte de diffusion d’images à l’aide de commandes RTF. Si vous utilisez cette méthode, assurez-vous d’abord de charger vos polices vers Dynamic Media Classic, sinon Dynamic Media Classic remplacera une police par défaut lors du chargement, et il n’y a aucun moyen facile de remplacer la police appropriée.

### Éditeur de texte

Vous saisissez du texte à l’aide de l’éditeur de texte. L’éditeur de texte est une interface WYSIWYG qui vous permet de saisir et de mettre en forme votre texte à l’aide de commandes de formatage similaires à celles de Photoshop ou Illustrator.

![image](assets/basic-templates/basic-templates-9.jpg)

_Éditeur de texte de base des modèles._

Vous ferez la plus grande partie de votre travail dans la **Aperçu** qui vous permet de saisir du texte et de le voir affiché tel qu’il apparaîtra dans le modèle. Il y a également un **Source** , qui est utilisé pour modifier manuellement le RTF, si nécessaire.

Le workflow général consiste à utiliser la variable **Aperçu** pour saisir du texte.

Sélectionnez ensuite le texte et choisissez une mise en forme, telle que la couleur de la police, la taille de la police ou la justification, à l’aide des commandes situées en haut. Une fois que le texte est stylisé comme vous le souhaitez, cliquez sur **Appliquer** pour voir la mise à jour dans l’aperçu de la zone de travail. Vous fermez ensuite l’éditeur de texte pour revenir à la fenêtre principale Concepts de base des modèles .

#### Utilisation de l’éditeur de texte

Voici les étapes de workflow pour ajouter du texte dans la page de création Concepts de base des modèles :

1. Cliquez sur le bouton **Texte** dans la partie supérieure de la page de création.
2. Faites glisser une zone de texte dans laquelle le texte doit s’afficher. La fenêtre Éditeur de texte s’ouvre dans une fenêtre modale. En arrière-plan, le modèle s’affiche, mais il n’est pas modifiable tant que vous n’avez pas terminé de le modifier.
3. Saisissez l’exemple de texte que vous souhaitez afficher lors du premier chargement du modèle. Par exemple, si vous créez une zone de texte pour une image d’email personnalisée, votre texte peut indiquer &quot;Bonjour nom. C&#39;est maintenant le moment d&#39;économiser !&quot; Vous ajouteriez ensuite un paramètre de texte pour remplacer Nom par une valeur que vous envoyez sur l’URL. Votre texte n’apparaîtra pas dans le modèle sous la fenêtre tant que vous n’aurez pas cliqué sur **Appliquer**.
4. Pour mettre en forme votre texte, sélectionnez-le en faisant glisser votre souris et choisissez un contrôle de mise en forme dans l’interface utilisateur.

   - Il existe de nombreuses options de formatage. Les plus courantes sont la police (face), la taille et la couleur de la police, ainsi que la justification gauche/centre/droite.
   - N&#39;oubliez pas de sélectionner d&#39;abord le texte. Sinon, vous ne pourrez appliquer aucune mise en forme.
   - Pour choisir une autre police, veillez à sélectionner le texte et à ouvrir le menu Police . L’éditeur affiche la liste de toutes les polices chargées dans Dynamic Media Classic. Si une police est également installée sur votre ordinateur, elle s’affiche en noir. S’il n’est pas installé sur votre ordinateur, il s’affiche en rouge. Toutefois, le rendu sera toujours effectué dans la fenêtre de prévisualisation lorsque vous cliquerez sur **Appliquer**. Il vous suffit de charger des polices dans Dynamic Media Classic pour les rendre disponibles à tous les utilisateurs de Dynamic Media Classic. Une fois que vous avez publié, le serveur d’images utilise ces polices pour générer le texte. Vos utilisateurs n’ont pas besoin d’installer de polices pour voir le texte que vous créez, car il fait partie d’une image.
   - Contrairement à Photoshop et Illustrator, le serveur d’images peut aligner votre texte verticalement dans la zone de texte. La valeur par défaut est l’alignement supérieur. Pour le modifier, sélectionnez votre texte et choisissez **Moyen** ou **Bas** de la **Alignement vertical** .
   - Si vous agrandissez le texte de la zone (ou si la zone de texte est trop petite), tout ou partie de ce texte sera tronqué et disparaîtra. Réduisez la taille de la police ou agrandissez la zone.

5. Cliquez sur **Appliquer** pour que vos modifications prennent effet dans la fenêtre de la zone de travail. Vous devez cliquer sur **Appliquer** ou vous perdez vos modifications.
6. Lorsque vous avez terminé, cliquez sur **Fermer**. Si vous souhaitez revenir au mode de modification, double-cliquez sur le calque de texte pour rouvrir l’éditeur de texte.

L’éditeur de texte prévisualise la taille exacte de la police si celle-ci est installée localement sur votre système.

### À propos de l’ajout de paramètres aux calques de texte

Nous suivons maintenant un processus similaire pour ajouter des paramètres de texte comme nous l’avons fait pour les paramètres de calque. Les calques de texte peuvent également utiliser des paramètres de calque pour la taille, la position, etc. cependant, ils peuvent prendre des paramètres supplémentaires qui vous permettent de contrôler n’importe quel aspect du RTF.

Contrairement aux paramètres de calque, vous sélectionnez uniquement la valeur à modifier et ajoutez un paramètre à cette valeur plutôt que d’ajouter un paramètre à la propriété entière.

![image](assets/basic-templates/basic-templates-10.jpg)

Exemple de RTF :

![image](assets/basic-templates/sample-rtf.png)

Lorsque vous examinez le RTF, vous devez déterminer où se trouve chaque paramètre que vous souhaitez modifier. Dans le RTF ci-dessus, certaines peuvent avoir un sens et vous pouvez voir d&#39;où vient le formatage.

Vous pouvez voir l&#39;expression Chocolate Mint Sandal — c&#39;est le texte lui-même.

- Il y a une référence à la police Pauvre Richard — c&#39;est là que les polices sont sélectionnées.
- Vous pouvez voir une valeur de RGB : \red56\green53\blue4 — c&#39;est la couleur du texte.
- Bien que la taille de police soit de 20, le nombre 20 n’apparaît pas. Cependant, une commande \fs40 s’affiche. Pour une raison étrange, RTF mesure les polices comme des demi-points. Par conséquent, \fs40 est la taille de police !

Vous disposez de suffisamment d’informations pour créer vos paramètres, mais il existe une référence complète de toutes les commandes RTF dans la documentation du serveur d’images. Visitez le [Documentation du serveur d’images](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html#concept-0d3136db7f6f49668274541cd4b6364c).

#### Ajout de paramètres aux calques de texte

Voici les étapes à suivre pour ajouter des paramètres aux calques de texte.

1. Cliquez sur le bouton **Paramètres** (un &quot;P&quot;) en regard du nom de la couche de texte pour laquelle vous souhaitez créer des paramètres. L’écran Paramètres s’affiche. Le **Courant** tab répertorie chaque propriété du calque et sa valeur. Vous pouvez y ajouter des paramètres de calque standard.
1. Cliquez sur le bouton **Texte** . Ici, vous pouvez voir le RTF en haut ; les paramètres que vous ajoutez se trouvent en dessous.
1. Pour ajouter un paramètre, mettez d’abord en surbrillance la valeur à modifier, puis cliquez sur le bouton **Ajouter un paramètre** bouton . Veillez à ne sélectionner que les valeurs des commandes et non la commande entière. Par exemple, pour définir un paramètre pour le nom de la police dans l’exemple de RTF ci-dessus, je vais uniquement mettre en surbrillance &quot;Pauvre Richard&quot; et ajouter un paramètre à cela, mais pas également &quot;\f0&quot;. Lorsque vous cliquez sur **Ajouter un paramètre** , il apparaît dans la liste ci-dessous et la valeur du paramètre apparaît en rouge dans le RTF tant qu’il est sélectionné. Si vous devez supprimer un paramètre, cochez la case en regard de **Activé** pour désactiver ce paramètre et il disparaît.
1. Cliquez sur pour renommer votre paramètre en un nom plus significatif.
1. Une fois que vous avez terminé, votre RTF est mis en surbrillance en vert, là où il existe des paramètres, et vos noms et valeurs de paramètre sont répertoriés ci-dessous.
1. Cliquez sur **Fermer** pour quitter l’écran Paramètres . Puis appuyez **Enregistrer** , pour enregistrer le modèle. Si vous avez terminé la modification, appuyez sur **Fermer** pour quitter la page Concepts de base des modèles .
1. Cliquez sur **Aperçu** pour tester votre modèle dans Dynamic Media Classic. Pour tester vos paramètres de texte, saisissez du nouveau texte ou de nouvelles valeurs dans la fenêtre de prévisualisation. Pour modifier la police, vous devez saisir le nom RTF exact de la police.

>[!TIP]
>
>Pour ajouter des paramètres à la couleur du texte, ajoutez séparément des paramètres pour le rouge, le vert et le bleu. Par exemple, si le RTF est `\red56\green53\blue46`, vous ajouteriez des paramètres rouges, verts et bleus distincts pour les valeurs 56, 53 et 46. Dans l’URL, vous modifiez la couleur en appelant les trois éléments suivants : `&$red=56&$green=53&$blue=46`.

Découvrez comment [Création de paramètres de texte dynamique](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html#creating-dynamic-text-parameters).

## Publication et création d’URL de modèle

### Création d’un paramètre d’image prédéfini

La création d’un paramètre prédéfini pour votre modèle n’est pas obligatoire. Il est recommandé d’appeler le modèle à une taille 1:1 et d’ajouter une accentuation aux calques d’image de grande taille qui sont redimensionnés pour s’adapter au modèle. Si vous appelez une image sans paramètre prédéfini, le serveur d’images peut redimensionner arbitrairement votre image à la taille par défaut (environ 400 pixels) et n’appliquera pas l’accentuation par défaut.

Un paramètre d’image prédéfini n’a rien de spécial pour un modèle. Si vous disposez déjà d’un paramètre prédéfini pour une image statique de la même taille, vous pouvez l’utiliser à la place.

### Publication

Vous devez exécuter une publication pour que vos modifications soient publiées sur le serveur d’images. Gardez à l’esprit les éléments qui doivent être publiés : les différents calques de ressource image, les polices du texte dynamique et le modèle lui-même. Comme pour d’autres ressources multimédias enrichies Dynamic Media Classic telles que les visionneuses d’images et les visionneuses à 360°, un modèle de base est une construction artificielle. Il s’agit d’un élément de ligne de la base de données qui référence les images et les polices à l’aide d’une série de commandes de diffusion d’images. Ainsi, lorsque vous publiez le modèle, vous ne faites que mettre à jour les données sur le serveur d’images.

En savoir plus sur [Publication de votre modèle](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/publishing-templates.html).

### Construction d’URL de modèle

Un modèle de base a la même syntaxe d’URL essentielle qu’un appel d’image normal, comme expliqué précédemment. Un modèle comporte généralement plus de modificateurs (commandes séparées par une esperluette (&amp;)), tels que des paramètres avec des valeurs. Cependant, la principale différence réside dans le fait que vous appelez au modèle comme image principale, au lieu d’appeler une image statique.

![image](assets/basic-templates/basic-templates-11.jpg)

Contrairement aux paramètres d’image prédéfinis, qui comportent un symbole dollar ($) de chaque côté du nom du paramètre prédéfini, les paramètres n’ont qu’un seul signe dollar au début. Le positionnement de ces panneaux de dollars est important.

**Correct :**

`$text=46-inch LCD HDTV`

**Incorrect :**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Comme nous l’avons déjà mentionné, des paramètres sont utilisés pour modifier le modèle. Si vous appelez le modèle sans paramètres, il revient à ses paramètres par défaut, tels qu’ils ont été conçus dans l’outil de création Concepts de base des modèles . Si une propriété n’a pas besoin d’être modifiée, il n’est pas nécessaire de définir un paramètre.

![image](assets/basic-templates/sandals-without-with-parameters.png)
_Exemples de modèles sans paramétrage (ci-dessus) et avec des paramètres (ci-dessous)._
