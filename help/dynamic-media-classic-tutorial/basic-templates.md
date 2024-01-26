---
title: Présentation des modèles de base
description: Découvrez les modèles de base dans Dynamic Media Classic. Ils sont basés sur des images, appelés à partir du serveur d’images et composés d’images et de texte rendu. Un modèle peut être modifié dynamiquement à partir de l’URL qui suit la publication du modèle. Vous apprendrez comment charger un PSD de Photoshop vers Dynamic Media Classic pour l’utiliser comme base d’un modèle. Créez un modèle de base de marchandisage simple constitué de calques d’image. Ajoutez des calques de texte et rendez-les variables à l’aide de paramètres. Créez une URL de modèle et manipulez l’image de manière dynamique par le biais du navigateur web.
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: d4e16b45-0095-44b4-8c16-89adc15e0cf9
duration: 1629
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '6219'
ht-degree: 100%

---

# Présentation des modèles de base {#basic-templates}

Dans Dynamic Media Classic, un modèle est un document qui peut être modifié dynamiquement via l’URL après la publication du modèle. Dynamic Media Classic propose des modèles de base. Ils sont basés sur des images, appelés à partir du serveur d’images et composés d’images et de texte rendu.

L’un des aspects les plus puissants des modèles est qu’ils comportent des points d’intégration directs qui vous permettent de les lier à votre base de données. Ainsi, non seulement vous pouvez diffuser une image et la redimensionner, mais vous pouvez également interroger votre base de données pour rechercher des articles nouveaux ou vendus et les faire apparaître comme une superposition sur l’image. Vous pouvez demander une description de l’élément et la faire apparaître sous forme de libellé dans une police choisie et mise en page. Les possibilités sont illimitées.

Les modèles de base peuvent être implémentés de différentes manières, de simple à complexe. Par exemple :

- Marchandisage de base. Utilise des libellés telles que « livraison gratuite » si ce produit en dispose d’une. Ces libellés sont configurés par l’équipe de marchandisage de Photoshop. Le web se base sur la logique pour savoir quand les appliquer à l’image.
- Marchandisage avancé. Chaque modèle comporte plusieurs variables et peut afficher plusieurs options en même temps. Utilise une base de données, un inventaire et des règles de fonctionnement pour déterminer quand afficher un produit sous la forme « Nouveau », « Promotion » ou « Épuisé ». Vous pouvez également utiliser la transparence derrière le produit pour l’afficher sur différents arrière-plans, dans différentes pièces par exemple. Les mêmes modèles et/ou ressources peuvent être réutilisés sur la page des détails du produit afin d’afficher une version agrandie ou zoomable du même produit avec différents arrière-plans.

Il est important de comprendre que Dynamic Media Classic ne fournit que la partie visuelle de ces applications basées sur des modèles. Les sociétés Dynamic Media Classic ou leurs partenaires d’intégration doivent fournir les règles commerciales, la base de données et les compétences de développement nécessaires pour créer les applications. Il n’existe pas d’application de modèle « intégrée » ; les concepteurs et conceptrices configurent le modèle dans Dynamic Media Classic et les développeurs et développeuses utilisent des appels d’URL pour modifier les variables du modèle.

D’ici la fin de cette section du tutoriel, vous saurez comment :

- Chargez un PSD de Photoshop vers Dynamic Media Classic pour l’utiliser comme base d’un modèle.
- Créez un modèle de base de marchandisage simple constitué de calques d’image.
- Ajoutez des calques de texte et rendez-les variables à l’aide de paramètres.
- Créez une URL de modèle et manipulez l’image de manière dynamique par le biais du navigateur web.

>[!NOTE]
>
>Toutes les URL de ce chapitre sont proposées à titre d’illustration uniquement ; il ne s’agit pas de liens directs.

## Vue d’ensemble des modèles de base

La définition d’un modèle de base (ou simplement « modèle », en abrégé) est une image avec un calque adressable par URL. Le résultat final est une image, mais qui peut être modifiée par l’URL. Elle peut se composer de photos, de texte ou de graphiques — de toute combinaison de ressources P-TIFF dans Dynamic Media Classic.

Les modèles sont très similaires aux fichiers PSD de Photoshop, car ils disposent d’un workflow et de fonctionnalités semblables.

- Les deux se composent de calques qui s’apparentent à des feuilles d’acétate empilées. Vous pouvez composer des images partiellement transparentes et les voir à travers les zones transparentes d’un calque jusqu’aux calques du dessous.
- Les calques peuvent être déplacés et pivotés pour repositionner le contenu. L’opacité et les modes de fusion peuvent être modifiés pour rendre le contenu partiellement transparent.
- Vous pouvez créer des calques de texte. La qualité peut être très bonne, car le serveur d’images utilise le même moteur de texte que Photoshop et Illustrator.
- Des styles de calque simples peuvent être appliqués à chaque calque pour créer des effets spéciaux tels que des ombres portées ou des lueurs.

Cependant, contrairement aux PSD de Photoshop, les calques peuvent être entièrement dynamiques et contrôlés via une URL sur le serveur d’images.

- Vous pouvez ajouter des variables à toutes les propriétés du modèle, ce qui facilite la modification à la volée de sa composition.
- Les variables appelées paramètres vous permettent d’afficher uniquement la partie du modèle que vous souhaitez modifier.

Il vous suffit d’ajouter un espace réservé pour chaque calque qui sera modifié au lieu de placer tous les calques dans un seul fichier comme vous le faites dans Photoshop, de les afficher et de les masquer (même si vous pouvez également le faire).

À l’aide d’un espace réservé, vous pouvez remplacer dynamiquement le contenu d’un calque par une autre ressource publiée. Le calque récupère automatiquement les mêmes propriétés (telles que la taille et la rotation) du calque qu’il a remplacé.

Comme les modèles de base sont généralement conçus dans Photoshop, mais déployés via une URL, un projet de modèle nécessite un mélange de compétences techniques et de conception. Nous partons généralement du principe que la personne qui crée le modèle créatif s’y connait en conception Photoshop et que la personne qui met en œuvre le modèle est versée dans le développement web. Les équipes de création et de développement doivent travailler en étroite collaboration pour que le modèle soit efficace.

Les projets de modèle peuvent être relativement simples ou extrêmement complexes en fonction des règles métier et des besoins de l’application. Les modèles de base sont appelés à partir du serveur d’images. Cependant, en raison de la flexibilité de l’environnement de Dynamic Media Classic, vous pouvez même imbriquer des modèles dans d’autres modèles, ce qui vous permet de créer des images assez complexes qui peuvent être liées par des variables communément nommées.

- En savoir plus sur les [Concepts de base des modèles](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/quick-start-template-basics.html?lang=fr).
- Découvrez comment créer un [Modèle de base](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html?lang=fr#creating_a_template).

## Créer un modèle de base

Lorsque vous utilisez un modèle de base, vous suivez généralement les étapes du workflow dans le diagramme ci-dessous. Les étapes marquées de lignes pointillées sont facultatives si vous utilisez des calques de texte dynamiques et sont indiquées dans les instructions ci-dessous comme « Workflow de texte ». Si vous n’utilisez pas de texte, suivez la méthode principale uniquement.

![image](assets/basic-templates/basic-templates-1.png)

_Workflow du modèle de base._

1. Concevez et créez vos ressources. La plupart des utilisateurs ou utilisatrices effectuent cette opération dans Adobe Photoshop. Concevez des ressources à la taille exacte dont vous avez besoin : s’il s’agit d’une image de 200 pixels pour une page de miniatures, concevez-la à 200 pixels. Si vous devez effectuer un zoom dessus, concevez-la à une taille d’environ 2 000 pixels. Utilisez Photoshop (et/ou la fonctionnalité d’enregistrement en tant qu’image bitmap d’Illustrator) pour créer les ressources et utilisez Dynamic Media Classic pour assembler les parties, gérer les calques et ajouter des variables.
2. Après avoir conçu vos ressources graphiques, chargez-les dans Dynamic Media Classic. Plutôt que de charger des ressources individuelles à partir du PSD, nous vous recommandons de charger l’intégralité de votre fichier PSD superposé et de faire en sorte que Dynamic Media Classic crée un fichier par couche, en utilisant la variable **Conserver les calques** lors du chargement (voir ci-dessous pour plus d’informations). _Workflow de texte : si vous créez du texte dynamique, chargez également vos polices. Le texte dynamique est variable et contrôlé via l’URL. Si votre texte est statique ou ne comporte que quelques phrases courtes qui ne changent pas (par exemple, des balises qui indiquent « Nouveau » ou « Soldes » plutôt que « X % de réduction », le X étant un nombre variable), nous vous recommandons de pré-rendre le texte dans Photoshop et de le transférer sous forme d’images et de calques pixellisés. Ce processus est plus simple et vous permet de mettre le texte en forme exactement comme vous le souhaitez._
3. Créez le modèle dans Dynamic Media Classic à l’aide de l’éditeur de modèles de base du menu Créer et ajoutez des calques d’image. Workflow de texte : créez des calques de texte dans le même éditeur. Cette étape est obligatoire lors de la création manuelle d’un modèle dans Dynamic Media Classic. Sélectionnez la taille de la zone de travail correspondant à votre conception, faites glisser et déposez les images sur la zone de travail, puis définissez les propriétés du calque (taille, rotation, opacité, etc.). Inutile de placer tous les calques possibles sur votre modèle, un seul espace réservé par calque d’image suffit. _Workflow de texte : vous créez des calques de texte à l’aide de l’outil Texte, comme vous le faites pour créer des calques de texte dans Photoshop. Vous pouvez choisir une police et la mettre en forme à l’aide des mêmes options que celles disponibles avec l’outil Texte de Photoshop._ Un autre workflow consiste à charger un PSD et à faire en sorte que Dynamic Media Classic génère un modèle « libre », voire à recréer des calques de texte. Ceci est abordé plus en détail ultérieurement.
4. Une fois les calques créés, ajoutez des paramètres (variables) à n’importe quelle propriété de calque que vous souhaitez contrôler via l’URL, y compris la source du calque (l’image elle-même). _Workflow de texte : vous pouvez également ajouter des paramètres aux calques de texte, à la fois pour contrôler le contenu du texte, la taille et la position du calque lui-même, ainsi que toutes les options de formatage telles que la couleur, la taille de police, le suivi horizontal, etc._
5. Créez un paramètre d’image prédéfini correspondant à la taille de votre modèle. Nous vous recommandons de faire en sorte que le modèle soit toujours appelé à une taille 1:1, mais aussi d’ajouter de l’accentuation à tous les calques d’image de grande taille qui sont redimensionnés pour s’adapter au modèle. Si vous créez un modèle sur lequel il faut effectuer un zoom, cette étape n’est pas nécessaire.
6. Publiez, copiez l’URL à partir de l’aperçu Dynamic Media Classic et testez-la dans un navigateur.

## Préparer et télécharger des ressources de modèle pour Dynamic Media Classic

Avant de télécharger vos ressources de modèle dans Dynamic Media Classic, vous devez effectuer quelques étapes préparatoires.

### Préparer le PSD pour le chargement

Avant de charger votre fichier Photoshop dans Dynamic Media Classic, simplifiez les calques Photoshop afin de faciliter l’utilisation et la compatibilité avec le serveur d’images. Votre fichier PSD se compose souvent d’éléments que Dynamic Media Classic ne reconnaît pas et vous pouvez également obtenir de nombreux petits éléments difficiles à gérer. Veillez à enregistrer une sauvegarde de votre PSD principal si vous devez modifier l’original ultérieurement. Vous téléchargerez la copie simplifiée, et non le fichier principal.

![image](assets/basic-templates/basic-templates-2.jpg)

1. Simplifiez la structure des calques en fusionnant/aplatissant les calques associés qui doivent être activés/désactivés ensemble en un seul calque. Par exemple, le libellé « NOUVEAU » et la bannière bleue sont fusionnés en un seul calque afin que vous puissiez les afficher ou les masquer en un seul clic.
   ![image](assets/basic-templates/basic-templates-3.jpg)
2. Certains types de calques et effets de calque ne sont pas pris en charge par Dynamic Media Classic ou le serveur d’images et doivent être pixellisés avant le chargement. Sinon, les effets risquent d’être ignorés ou les calques rejetés. La pixellisation d’un calque consiste à le convertir, celui-ci passant de modifiable à non modifiable. Pour pixelliser les effets de calque ou les calques de texte, créez un calque vide, sélectionnez les deux et fusionnez-les à l’aide de **Calques > Fusionner les calques** ou Ctrl+E/Cmd+E.

   - Dynamic Media Classic ne peut ni regrouper ni lier des calques. Tous les calques d’un groupe ou d’un ensemble lié sont convertis en calques distincts, qui ne sont plus regroupés/liés.
   - Les masques de calque sont convertis en transparence lors du chargement.
   - Les calques de réglage ne sont pas pris en charge et sont rejetés.
   - Les calques de remplissage, tels que les calques de couleur unie, sont pixellisés.
   - Les calques d’objet dynamique et les calques vectoriels sont pixellisés en images normales lors du chargement et les filtres intelligents sont appliqués et pixellisés.
   - Les calques de texte seront également pixellisés, sauf si vous utilisez l’option Extraire le texte. Pour plus d’informations, reportez-vous à la section ci-dessous.
   - La plupart des effets de calque sont ignorés et seuls quelques modes de fusion sont pris en charge. En cas de doute, ajoutez des effets simples dans Dynamic Media Classic (ombres intérieures ou extérieures, lueurs intérieures ou extérieures, par exemple) ou utilisez un calque vierge pour fusionner et pixelliser l’effet dans Photoshop.

### Utiliser les polices

Si vous devez générer du texte dynamique, vous pouvez également charger et publier vos polices. La seule police incluse dans Dynamic Media Classic est la police Arial.

Chaque entreprise se doit d’obtenir une licence pour utiliser une police sur le web. Le simple fait qu’une police soit installée sur votre ordinateur ne vous donne pas le droit de l’utiliser à des fins commerciales sur le web. Votre entreprise pourrait être confrontée à une action en justice par l’éditeur de la police si elle est utilisée sans autorisation. En outre, les conditions des licences varient : il se peut que vous ayez besoin de licences distinctes pour l’impression et l’affichage à l’écran, par exemple.

Dynamic Media Classic prend en charge les polices standard OpenType (OTF), TrueType (TTF) et PostScript Type 1. Les polices uniquement utilisables sur Mac, les fichiers de collection de type, les polices système Windows et les polices de machine propriétaires (comme les polices utilisées par les machines de gravure ou de broderie) ne sont pas toutes prises en charge. Vous devrez les convertir dans l’un des formats de police standard ou les remplacer par une police similaire pour les utiliser dans Dynamic Media Classic et sur le serveur d’images.

Une fois les polices chargées dans Dynamic Media Classic, comme toute autre ressource, elles doivent également être publiées sur le serveur d’images. Une erreur très courante avec les modèles consiste à oublier de publier vos polices, ce qui entraîne une erreur d’image : le serveur d’images ne remplace pas la police par une autre. En outre, si vous souhaitez utiliser l’option **Extraire le texte** lors du chargement, vous devez charger vos fichiers de police avant de charger le PSD qui les utilise. La fonction **Extraire le texte** tente de recréer votre texte en tant que calque de texte modifiable et de le placer dans un modèle Dynamic Media Classic. Ce sujet est abordé dans la rubrique suivante, Options de PSD.

Gardez à l’esprit que les polices possèdent plusieurs noms internes, souvent différents de leur nom de fichier externe. Vous trouverez tous leurs noms sur la page Détails de cette ressource dans Dynamic Media Classic. Voici les noms de la police Adobe Caslon Pro Semibold, répertoriés sous l’onglet Métadonnées dans Dynamic Media Classic :

![image](assets/basic-templates/basic-templates-4.jpg)

_Onglet Métadonnées de la page Détails d’une police dans Dynamic Media Classic._

Dynamic Media Classic utilise le nom de fichier de cette police (ACaslonPro-Semibold) comme identifiant de ressource, mais il ne s’agit pas du nom utilisé par le modèle. Le modèle utilise le nom Rich Text Format (RTF, format de texte enrichi), répertorié en bas. Le format RTF est la « langue » native du moteur de texte du serveur d’images.

Si vous devez modifier les polices via l’URL, appelez le nom RTF de la police (et non l’identifiant de ressource), faute de quoi vous obtiendrez une erreur. Dans ce cas, le nom correct de cette police serait « Adobe Caslon Pro ». Nous aborderons plus en détail le sujet des polices et du format RTF dans la rubrique Paramètres RTF et texte, ci-dessous.

Les formats des fichiers de police les plus courants, présents sur les systèmes Windows et Mac, sont OpenType et TrueType. OpenType possède une extension .OTF, tandis que TrueType a l’extension .TTF. Les deux formats fonctionnent aussi bien l’un que l’autre dans Dynamic Media Classic.

### Sélectionner des options lors du chargement d’un PSD

Il n’est pas nécessaire de charger un fichier Photoshop (PSD) pour créer un modèle. Vous pouvez créer un modèle à partir de n’importe quelle ressource d’image dans Dynamic Media Classic. Cependant, le chargement d’un PSD peut faciliter la création, car ces ressources se trouvent généralement déjà dans un PSD à plusieurs calques. En outre, Dynamic Media Classic génère automatiquement un modèle lorsque vous chargez un PSD à plusieurs calques.

- **Conserver les calques.** Il s’agit de l’option la plus importante. Elle indique à Dynamic Media Classic de créer une ressource image pour chaque calque Photoshop. Si cette option n’est pas cochée, toutes les autres sont désactivées et le PSD est aplati en une seule image.
- **Créer** **un modèle.** Cette option récupère les différents calques générés et crée automatiquement un modèle en les recombinant. L’inconvénient du modèle généré automatiquement est que Dynamic Media Classic place tous les calques dans un seul fichier, alors qu’un seul espace réservé par calque suffirait. Vous pouvez facilement supprimer les calques supplémentaires, mais s’ils sont nombreux, il est plus rapide de les recréer. N’oubliez pas de renommer le nouveau modèle, faute de quoi il sera écrasé lorsque vous chargerez de nouveau le même PSD.
- **Extraire du texte.** Les calques de texte du PSD sont ainsi recréés sous forme de calques de texte dans le modèle à l’aide de la police que vous avez chargée. Cette étape est obligatoire si votre texte se trouve sur un chemin d’accès dans Photoshop et que vous souhaitez conserver ce chemin d’accès dans le modèle. Cette fonctionnalité requiert la méthode **Créer un modèle**, car le texte extrait peut être créé uniquement par un modèle généré lors du chargement.
- **Étendez les calques à la taille de l’arrière-plan.** Ce paramètre donne à chaque calque la même taille que la zone de travail globale du PSD. Cela s’avère très utile pour les calques qui resteront toujours en position fixe : dans le cas contraire, lorsque vous permutez des images dans le même calque, vous devrez peut-être les repositionner.
- **Affectez un nom au calque.** Cela indique à Dynamic Media Classic comment nommer chaque ressource générée par un calque. Nous vous recommandons de les nommer **Photoshop** **et nom** du **calque** ou Photoshop et **numéro** du **calque**. Les deux options utilisent le nom du PSD comme première partie du nom et ajoutent le nom du calque ou le numéro à la fin. Par exemple, si vous disposez d’un PSD nommé « chemise.psd » et qu’il comporte des calques nommés « avant », « manches » et « col », si vous effectuez un chargement à l’aide de la variable **Photoshop et** nom du **calque**, Dynamic Media Classic génère les identifiants de ressource « chemise_avant », « chemise_manches » et « chemise_col ». Ces options permettent de s’assurer que le nom est unique dans Dynamic Media Classic.

## Créer un modèle avec des calques d’image

Même si Dynamic Media Classic peut créer automatiquement un modèle à partir d’un PSD avec un calque, vous devez savoir comment le créer manuellement. Comme expliqué ci-dessus, il peut arriver que vous ne souhaitiez pas utiliser le modèle créé par Dynamic Media Classic.

### Interface utilisateur des concepts de base des modèles

Commençons par nous familiariser avec l’interface d’édition.

Au milieu à gauche se trouve votre zone de travail qui affiche un aperçu de votre modèle final. Sur le côté droit se trouvent les panneaux Calques et Propriétés du calque. C’est là que vous allez travailler.

![image](assets/basic-templates/basic-templates-5.jpg)

_Page Concepts de base de la création des modèles._

- **Zone de travail/Aperçu.** Voici la fenêtre principale. Vous pouvez y déplacer, redimensionner et faire pivoter les calques à l’aide de la souris. Les contours des calques s’affichent en lignes pointillées.
- **Calques.** Cet espace est similaire au panneau Calques de Photoshop. Lorsque vous ajoutez des calques à votre modèle, ils s’affichent ici. Les calques sont empilés de haut en bas : le calque supérieur du panneau Calques est visible au-dessus des autres calques de la liste.
- **Propriétés du calque.** Vous pouvez y ajuster toutes les propriétés d’un calque à l’aide de commandes numériques. Sélectionnez d’abord un calque, puis ajustez ses propriétés.
- **URL** **composite.** La zone URL composite se trouve au bas de l’interface utilisateur. Cela ne sera pas abordé dans cette section du tutoriel. Cependant, ici, votre modèle sera déconstruit sous la forme d’une série de modificateurs d’URL de diffusion d’images. Cette zone est modifiable : si vous connaissez bien les commandes du serveur d’images, vous pouvez modifier manuellement le modèle ici. Cependant, vous pouvez également l’altérer. Comme Photoshop, la numérotation des calques commence à 0. La zone de travail est le calque 0, et le premier calque que vous ajoutez vous-même est le calque 1. Les modes de fusion déterminent la manière dont les pixels d’un calque se fondent. Vous pouvez créer divers effets spéciaux à l’aide des modes de fusion.

#### Utiliser l’éditeur de concepts de base des modèles

Voici les étapes du workflow pour créer votre modèle de base :

1. Dans Dynamic Media Classic, accédez à **Créer > Concepts de base des modèles**. Vous pouvez ne rien sélectionner ou commencer par sélectionner une image, qui devient le premier calque de votre modèle.
2. Choisissez une taille et appuyez sur **OK**. Cette taille doit correspondre à la taille que vous avez conçue dans Photoshop. L’éditeur de modèles se charge.
3. Si aucune image n’a été sélectionnée à l’étape 1, recherchez ou accédez à une image dans le panneau des ressources à gauche et faites-la glisser sur la zone de travail.

   - L’image est automatiquement redimensionnée pour s’adapter à la taille de la zone de travail. Si vous prévoyez d’intervertir vos images haute résolution, vous devez généralement importer l’une de vos grandes images P-TIFF (2 000 pixels) et l’utiliser comme espace réservé.
   - Il doit s’agir du calque le plus en bas de votre modèle. Vous pouvez toutefois réorganiser les calques ultérieurement.

4. Redimensionnez ou repositionnez le calque directement dans la zone de travail ou en réglant les paramètres du panneau Propriétés du calque.
5. Faites glisser d’autres calques d’image si nécessaire. Ajoutez des effets de calques si vous le souhaitez. Voir la rubrique _Ajouter des effets de calque_, ci-dessous.
6. Cliquez sur **Enregistrer**, choisissez un emplacement et attribuez un nom au modèle. Vous pouvez le prévisualiser, mais à ce stade, votre modèle ressemblera exactement à une image Photoshop aplatie : il n’est pas encore modifiable.

### Ajouter des effets de calque

Le serveur d’images prend en charge quelques effets de calque programmatiques : des effets spéciaux qui modifient l’aspect du contenu d’un calque. Ils fonctionnent de la même manière que les effets de calque dans Photoshop. Ils sont attachés à un calque, mais contrôlés indépendamment du calque. Vous pouvez les ajuster ou les supprimer sans apporter de modification permanente au calque lui-même.

- **Ombre portée**. Applique une ombre au-delà des limites du calque, positionnée par un décalage de pixel x et y.
- **Ombre interne**. Applique une ombre à l’intérieur des limites du calque, positionnée par un décalage de pixel x et y.
- **Lueur externe**. Applique un effet de lueur uniformément à l’extérieur des bords du calque.
- **Lueur interne**. Applique un effet de lueur uniformément à l’intérieur des bords du calque.

![image](assets/basic-templates/basic-templates-6.jpg)

_Calque avec et sans ombre portée_

Pour ajouter un effet, cliquez sur **Ajouter un effet**, puis choisissez un effet dans le menu. Comme les calques normaux, vous pouvez sélectionner un effet dans le panneau Calques et utiliser le panneau Propriétés du calque pour en ajuster les paramètres.

Les effets d’ombre sont décalés horizontalement ou verticalement par rapport au calque, tandis que les effets de lueur sont appliqués uniformément dans toutes les directions. Les effets internes agissent sur les parties opaques du calque, tandis que les effets externes n’affectent que les zones transparentes.

En savoir plus sur l’[ajout d’effets de calque](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template.html?lang=fr#using-shadow-and-glow-effects-on-layers).

### Ajouter des paramètres

Si vous combinez simplement des calques et que vous les enregistrez, le résultat net n’est pas différent d’une image Photoshop aplatie. Ce qui rend ces modèles spéciaux est la possibilité d’ajouter des paramètres aux propriétés de chaque calque afin qu’elles puissent être modifiées dynamiquement via l’URL.

En termes Dynamic Media Classic, un paramètre est une variable qui peut être liée à une propriété de modèle afin de pouvoir être manipulée via une URL. Lorsque vous ajoutez un paramètre à un calque, Dynamic Media Classic expose cette propriété dans l’URL en ajoutant en préfixe le nom de votre paramètre avec un signe dollar ($). Par exemple, si vous créez un paramètre appelé « taille » pour modifier la taille d’un calque, Dynamic Media Classic renommera votre paramètre « $taille ».

Si vous n’ajoutez pas de paramètre pour une propriété, celle-ci reste masquée dans la base de données Dynamic Media Classic et ne s’affiche pas dans l’URL.

![image](assets/basic-templates/parameters.png)

Sans paramètres, vos URL sont généralement beaucoup plus longues, en particulier si vous utilisez également du texte dynamique. Le texte ajoute plusieurs dizaines de caractères supplémentaires à chaque URL.

Enfin, votre ensemble de paramètres initiaux devient les valeurs par défaut des propriétés du modèle. Si vous créez votre modèle, ajoutez des paramètres, puis appelez l’URL sans ses paramètres, le serveur d’images crée l’image avec tous les paramètres par défaut que vous avez enregistrés dans le modèle. Les paramètres sont nécessaires uniquement si vous souhaitez modifier une propriété. Si une propriété n’a pas besoin d’être modifiée, il n’est pas nécessaire de définir un paramètre.

#### Créer des paramètres

Il s’agit du workflow de création de paramètres :

1. Cliquez sur le bouton **Paramètres** en regard du nom du calque pour lequel vous souhaitez créer des paramètres. L’écran Paramètres s’affiche. Il répertorie chaque propriété du calque et sa valeur.
1. Sélectionnez l’option **Activé** en regard du nom de chaque propriété que vous souhaitez transformer en paramètre. Un nom de paramètre par défaut s’affiche. Vous pouvez uniquement ajouter des paramètres aux propriétés dont l’état par défaut a été modifié.

   - Par exemple, si vous ajoutez un calque et que vous le conservez à sa position xy par défaut de 0,0, Dynamic Media Classic n’expose pas de propriété **Position**. Pour corriger cela, déplacez le calque d’au moins un pixel. Maintenant, Dynamic Media Classic va exposer **Position** en tant que propriété que vous pouvez paramétrer.
   - Pour ajouter un paramètre à la propriété d’affichage/masquage (qui active et désactive le calque), cliquez sur le bouton **Afficher** ou **Masquer le calque** pour désactiver le calque (si vous le souhaitez, vous pouvez l’activer à nouveau par la suite). Dynamic Media Classic expose désormais une propriété **Masquer** qui peut être paramétrée.

1. Renommez les paramètres par défaut avec un nom plus facile à identifier dans l’URL. Par exemple, si vous souhaitez ajouter un paramètre pour modifier le calque de bannière au-dessus d’une image, remplacez le nom par défaut « calque_2_src » par « bannière ».
1. Cliquez sur **Fermer** pour quitter l’écran Paramètres.
1. Répétez cette opération pour les autres calques en cliquant sur le bouton **Paramètres**, puis ajoutez et renommez des paramètres.
1. Une fois les modifications effectuées, enregistrez-les.

>[!TIP]
>
>Renommez vos paramètres pour qu’ils aient un sens et développez une convention de nommage pour normaliser ces noms. Assurez-vous que la convention de nommage a été préalablement convenue par les équipes de conception et de développement.
>
>Impossible d’ajouter un paramètre, car la propriété n’apparaît pas ? Il vous suffit de modifier la propriété du calque à partir de sa valeur par défaut (en le déplaçant, le redimensionnant, le masquant, etc.). Vous devriez maintenant voir cette propriété exposée.

En savoir plus sur les [paramètres de modèle](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html?lang=fr).

## Créer un modèle avec des calques de texte

Vous allez maintenant apprendre à créer un modèle de base qui comprend des calques de texte.

### Comprendre le texte dynamique

Vous savez maintenant comment créer un modèle de base à l’aide de calques d’image. Pour de nombreuses applications, c’est tout ce dont vous aurez besoin. Comme vous l’avez vu dans l’exercice précédent, les calques dont le texte est simple (comme « Soldes » et « Nouveau ») peuvent être pixellisés et traités comme des images, car leur texte n’a pas besoin d’être modifié.

Toutefois, que se passe-t-il si vous avez besoin d’effectuer les opérations suivantes :

- Ajouter un libellé pour indiquer « 25 % de réduction », la valeur de 25 % étant variable.
- Ajouter un libellé de texte avec le nom du produit au-dessus de l’image.
- Localiser vos calques dans différentes langues en fonction du pays dans lequel votre modèle est affiché.

Dans ce cas, vous souhaitez ajouter des calques de texte dynamiques avec des paramètres pour contrôler le texte et/ou le formatage.

Pour créer du texte, vous devez charger certaines polices. Sinon, Dynamic Media Classic prend la valeur Arial par défaut. Les polices doivent également être publiées sur le serveur d’images, sinon cela génère une erreur dès que ces polices doivent être rendues.

### Paramètres RTF et texte

Pour ajouter des variables au texte à l’aide de l’outil Concepts de base des modèles, vous devez comprendre comment le texte est rendu. Le serveur d’images génère du texte à l’aide du moteur de texte Adobe, le même moteur que celui utilisé par Photoshop et Illustrator, et le compose en tant que calque dans l’image finale. Pour communiquer avec le moteur, le serveur d’images utilise le format RTF (Rich Text Format).

RTF est une spécification de format de fichier développée par Microsoft pour spécifier le formatage des documents. Il s’agit d’un langage de balisage standard utilisé par la plupart des logiciels de traitement de texte et d’e-mails. Si vous avez écrit dans une URL « &amp;text=\b1 Bonjour », le serveur d’images génère une image avec le mot « Bonjour » en gras, car \b1 est la commande RTF permettant de mettre le texte en gras.

La bonne nouvelle est que Dynamic Media Classic génère le RTF pour vous. Chaque fois que vous saisissez du texte dans un modèle et que vous ajoutez une mise en forme, Dynamic Media Classic écrit automatiquement le code RTF dans le modèle. Ceci est important, car vous ajoutez des paramètres directement au RTF, il est donc nécessaire que vous ayez quelques connaissances à ce sujet.

#### Créer des calques de texte

Vous pouvez créer des calques de texte dans un modèle Dynamic Media Classic de deux manières :

1. Avec l’outil Texte dans Dynamic Media Classic. Nous parlerons de cette méthode ci-dessous. L’éditeur Concepts de base des modèles dispose d’un outil qui vous permet de créer une zone de texte, de saisir du texte et de le mettre en forme. Dynamic Media Classic génère le RTF selon les besoins et le place dans un calque distinct.
2. En extrayant du texte (lors d’un chargement). L’autre méthode consiste à créer le calque de texte dans Photoshop et à l’enregistrer dans le PSD en tant que calque de texte normal (au lieu de le pixelliser en tant que calque d’image). Vous téléchargez ensuite le fichier dans Dynamic Media Classic et utilisez l’option **Extraire le texte**. Dynamic Media Classic convertit chaque calque de texte Photoshop en calque de texte de diffusion d’images à l’aide de commandes RTF. Si vous utilisez cette méthode, assurez-vous d’abord de télécharger vos polices dans Dynamic Media Classic, sinon Dynamic Media Classic remplacera votre police par une police par défaut sans possibilité simple de la modifier ensuite.

### Éditeur de texte

Vous saisissez du texte à l’aide de l’éditeur de texte. L’éditeur de texte est une interface WYSIWYG qui vous permet de saisir et de mettre en forme votre texte à l’aide de commandes de formatage similaires à celles de Photoshop ou Illustrator.

![image](assets/basic-templates/basic-templates-9.jpg)

_Éditeur de texte des concepts de base des modèles._

Vous ferez la plus grande partie de votre travail dans l’onglet **Aperçu** qui vous permet de saisir du texte et de le voir affiché tel qu’il apparaîtra dans le modèle. Il y a également un onglet **Source**, qui est utilisé pour modifier manuellement le RTF, si nécessaire.

Le workflow général consiste à utiliser l’onglet **Aperçu** pour saisir du texte.

Sélectionnez ensuite le texte et définissez le formatage (couleur, taille de la police ou justification) à l’aide des commandes situées en haut. Une fois le style du texte défini, cliquez sur **Appliquer** pour l’afficher dans l’aperçu de la zone de travail. Fermez ensuite l’éditeur de texte pour revenir à la fenêtre principale Concepts de base des modèles.

#### Utiliser l’éditeur de thèmes

Voici les étapes de workflow pour ajouter du texte dans la page de création des concepts de base des modèles :

1. Cliquez sur le bouton de l’outil **Texte** dans la partie supérieure de la page de création.
2. Faites glisser une zone de texte dans laquelle le texte doit s’afficher. La fenêtre de l’éditeur de texte s’ouvre dans une fenêtre modale. En arrière-plan, le modèle s’affiche, mais il n’est pas modifiable tant que vous n’avez pas terminé de modifier le texte.
3. Saisissez l’exemple de texte à afficher lors du premier chargement du modèle. Par exemple, si vous créez une zone de texte pour une image d’e-mail personnalisée, votre texte peut indiquer « Bonjour, Prénom. Il est l’heure de faire des affaires ! ». Vous pouvez ensuite ajouter un paramètre de texte pour remplacer Prénom par une valeur que vous envoyez sur l’URL. Votre texte n’apparaîtra pas dans le modèle sous la fenêtre tant que vous n’aurez pas cliqué sur **Appliquer**.
4. Pour mettre en forme votre texte, sélectionnez-le en faisant glisser votre souris et choisissez un contrôle de formatage dans l’interface utilisateur.

   - Il existe de nombreuses options de formatage. Les plus courantes sont la police (face), la taille et la couleur de la police, ainsi que la justification gauche/centre/droite.
   - N’oubliez pas de sélectionner d’abord le texte. Sinon, vous ne pourrez appliquer aucun formatage.
   - Pour choisir une autre police, veillez à sélectionner le texte et à ouvrir le menu Police. L’éditeur affiche la liste de toutes les polices téléchargées dans Dynamic Media Classic. Si une police est également installée sur votre ordinateur, elle s’affiche en noir. Si elle n’est pas installée sur votre ordinateur, elle s’affiche en rouge. Toutefois, le rendu sera quand même effectué dans la fenêtre d’aperçu lorsque vous cliquerez sur **Appliquer**. Il vous suffit de télécharger des polices dans Dynamic Media Classic pour les rendre disponibles à tous les utilisateurs ou utilisatrices. Une fois le modèle publié, le serveur d’images utilise ces polices pour générer le texte. Vos utilisateurs et utilisatrices n’ont pas besoin d’installer des polices pour voir le texte que vous créez, car il fait partie d’une image.
   - Contrairement à Photoshop et Illustrator, le serveur d’images peut aligner votre texte verticalement dans la zone de texte. Le formatage par défaut est l’alignement Haut. Pour le modifier, sélectionnez votre texte et choisissez **Milieu** ou **Bas** dans le menu **Alignement vertical**.
   - Si vous agrandissez le texte de la zone (ou si la zone de texte est trop petite), l’intégralité ou une partie de ce texte sera tronquée et disparaîtra. Réduisez la taille de la police ou agrandissez la zone.

5. Cliquez sur **Appliquer** pour que vos modifications prennent effet dans la zone de travail. Cliquez sur **Appliquer** ou vous perdez vos modifications.
6. Lorsque vous avez terminé, cliquez sur **Fermer**. Si vous souhaitez revenir au mode de modification, double-cliquez sur le calque de texte pour rouvrir l’éditeur de texte.

L’éditeur de texte prévisualise la taille exacte de la police si celle-ci est installée localement sur votre système.

### À propos de l’ajout de paramètres aux calques de texte

Pour ajouter des paramètres de texte, nous suivons la même procédure que pour les paramètres de calque. Les calques de texte peuvent également utiliser des paramètres de calque pour la taille, la position, etc. Cependant, ils peuvent comporter des paramètres supplémentaires qui vous permettent de contrôler n’importe quel aspect du RTF.

Contrairement aux paramètres de calque, vous sélectionnez uniquement la valeur à modifier et ajoutez un paramètre à cette valeur, plutôt que d’ajouter un paramètre à la propriété entière.

![image](assets/basic-templates/basic-templates-10.jpg)

Exemple de RTF :

![image](assets/basic-templates/sample-rtf.png)

Lorsque vous examinez le RTF, vous devez déterminer où se trouve chaque paramètre que vous souhaitez modifier. Dans le RTF ci-dessus, cerains éléments peuvent avoir un sens et vous pouvez voir d’où vient le formatage.

Vous pouvez voir la phrase Chocolate Mint Sandal : il s’agit du texte.

- Il y a une référence à la police Poor Richard : c’est là que les polices sont sélectionnées.
- Vous pouvez voir une valeur RGB (\red56\green53\blue4) : il s’agit de la couleur du texte.
- Bien que la taille de police soit de 20, le nombre 20 n’apparaît pas. Cependant, une commande \fs40 s’affiche. Pour une raison étrange, RTF mesure les polices comme des demi-points. Par conséquent, \fs40 correspond à la taille de police.

Vous disposez de suffisamment d’informations pour créer vos paramètres, mais il existe une référence complète de toutes les commandes RTF dans la documentation du serveur d’images. Consultez la [documentation du serveur d’images](https://experienceleague.adobe.com/docs/dynamic-media-developer-resources/image-serving-api/image-serving-api/http-protocol-reference/text-formatting/c-text-formatting.html?lang=fr#concept-0d3136db7f6f49668274541cd4b6364c).

#### Ajouter des paramètres aux calques de texte

Voici les étapes à suivre pour ajouter des paramètres aux calques de texte.

1. Cliquez sur le bouton **Paramètres** (un « P ») à côté du nom du calque de texte pour lequel vous souhaitez créer des paramètres. L’écran Paramètres s’affiche. L’onglet **Courant** répertorie chaque propriété du calque et sa valeur. Vous pouvez y ajouter des paramètres de calque standard.
1. Cliquez sur l’onglet **Texte**. Ici, vous pouvez voir le RTF en haut ; les paramètres que vous ajoutez sont en dessous.
1. Pour ajouter un paramètre, mettez d’abord en surbrillance la valeur à modifier, puis cliquez sur le bouton **Ajouter un paramètre**. Veillez à ne sélectionner que les valeurs des commandes et non la commande entière. Par exemple, pour définir un paramètre pour le nom de la police dans l’exemple de RTF ci-dessus, je vais uniquement mettre en surbrillance « Poor Richard » et ajouter un paramètre à cela, mais pas à « \f0 ». Lorsque vous cliquez sur **Ajouter un paramètre**, il apparaît dans la liste ci-dessous et la valeur du paramètre apparaît en rouge dans le RTF tant qu’il est sélectionné. Si vous devez supprimer un paramètre, cochez la case en regard de **Activé** pour désactiver ce paramètre et il disparaît.
1. Cliquez pour renommer votre paramètre avec un nom plus significatif.
1. Une fois que vous avez terminé, votre RTF est mis en surbrillance en vert, là où il existe des paramètres, et vos noms et valeurs de paramètre sont répertoriés ci-dessous.
1. Cliquez sur **Fermer** pour quitter l’écran Paramètres. Puis appuyez sur **Enregistrer** pour enregistrer le modèle. Si vous avez terminé la modification, appuyez sur **Fermer** pour quitter la page Concepts de base des modèles.
1. Cliquez sur **Aperçu** pour tester votre modèle dans Dynamic Media Classic. Pour tester vos paramètres de texte, saisissez du nouveau texte ou de nouvelles valeurs dans la fenêtre d’aperçu. Pour modifier la police, vous devez saisir le nom RTF exact de la police.

>[!TIP]
>
>Pour ajouter des paramètres à la couleur du texte, ajoutez séparément des paramètres pour le rouge, le vert et le bleu. Par exemple, si le RTF est `\red56\green53\blue46`, vous ajoutez des paramètres rouges, verts et bleus distincts pour les valeurs 56, 53 et 46. Dans l’URL, vous modifiez la couleur en appelant les trois éléments suivants : `&$red=56&$green=53&$blue=46`.

Découvrez comment [créer des paramètres de texte dynamique](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/creating-template-parameters.html?lang=fr#creating-dynamic-text-parameters).

## Publier et créer des URL de modèle

### Créer un paramètre d’image prédéfini

La création d’un paramètre prédéfini pour votre modèle n’est pas obligatoire. Il est recommandé d’utiliser cette méthode pour que le modèle soit toujours appelé à une taille 1:1 et pour ajouter une accentuation à tous les calques d’image volumineux qui sont redimensionnés pour s’adapter au modèle. Si vous appelez une image sans paramètre prédéfini, le serveur d’images peut redimensionner arbitrairement votre image à la taille par défaut (environ 400 pixels) et n’appliquera pas l’accentuation par défaut.

Un paramètre d’image prédéfini pour un modèle n’a rien de particulier. Si vous disposez déjà d’un paramètre prédéfini pour une image statique de la même taille, vous pouvez l’utiliser à la place.

### Publication

Vous devez exécuter une publication pour que vos modifications soient publiées sur le serveur d’images. Gardez à l’esprit les éléments à publier : les différents calques de ressources d’image, les polices pour le texte dynamique et le modèle lui-même. Comme pour d’autres ressources de médias riches Dynamic Media Classic telles que les visionneuses d’images et les visionneuses à 360°, un modèle de base est une construction artificielle. Il s’agit d’un élément de ligne de la base de données qui référence les images et les polices à l’aide d’une série de commandes de diffusion d’images. Ainsi, lorsque vous publiez le modèle, vous ne faites que mettre à jour les données sur le serveur d’images.

En savoir plus sur la [publication de votre modèle](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/template-basics/publishing-templates.html?lang=fr).

### Construction d’URL de modèle

Un modèle de base a essentiellement la même syntaxe d’URL qu’un appel d’image normal, comme expliqué précédemment. Un modèle comporte généralement plus de modificateurs (commandes séparées par une esperluette (&amp;)), tels que des paramètres avec des valeurs. Cependant, la principale différence réside dans le fait que vous appelez le modèle comme image principale, au lieu d’appeler une image statique.

![image](assets/basic-templates/basic-templates-11.jpg)

Contrairement aux paramètres d’image prédéfinis, qui comportent un symbole de dollar ($) de chaque côté du nom du paramètre prédéfini, les paramètres n’ont qu’un seul symbole de dollar au début. Le placement de ces symboles de dollar est important.

**Correct :**

`$text=46-inch LCD HDTV`

**Incorrect :**

`$text$=46-inch LCD HDTV`

`$text=46-inch LCD HDTV$`

`text=46-inch LCD HDTV`

Comme nous l’avons déjà mentionné, les paramètres sont utilisés pour modifier le modèle. Si vous appelez le modèle sans paramètres, il revient à ses paramètres par défaut, tels qu’ils ont été conçus dans l’outil de création Concepts de base des modèles. Si une propriété n’a pas besoin d’être modifiée, il n’est pas nécessaire de définir un paramètre.

![image](assets/basic-templates/sandals-without-with-parameters.png)
_Exemples de modèles sans paramètre (ci-dessus) et avec des paramètres (ci-dessous)._
