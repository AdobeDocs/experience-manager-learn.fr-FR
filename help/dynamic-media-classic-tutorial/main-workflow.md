---
title: Processus principal de Dynamic Media Classic et aperçu des ressources
description: 'Découvrez le processus principal dans Dynamic Media Classic, qui comprend les trois étapes : Créer (et Télécharger), Auteur (et Publier) et Diffuser. Découvrez ensuite comment prévisualiser des ressources dans Dynamic Media Classic.'
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring, architecture, publishing
audience: all
activity: use
topic: Content Management
role: User
level: Beginner
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2703'
ht-degree: 7%

---

# Processus principal de Dynamic Media Classic et aperçu des ressources {#main-workflow}

Dynamic Media prend en charge un processus de création (et de téléchargement), d’auteur (et de publication) et de diffusion. Vous commencez par charger des ressources, puis faites quelque chose avec ces ressources, comme créer une visionneuse d’images et enfin les publier pour les rendre actives. L’étape de création est facultative pour certains workflows. Par exemple, si votre objectif est de n’effectuer que le dimensionnement et le zoom dynamiques sur les images ou de convertir et publier la vidéo pour la diffusion en continu, aucune étape de création n’est nécessaire.

![image](assets/main-workflow/create-author-deliver.jpg)

Le workflow dans les solutions Dynamic Media Classic se compose de trois étapes principales :

1. Création (et chargement) de SourceContent
2. Création (et publication) de ressources
3. Diffusion de ressources

## Étape 1 : Créer (et télécharger)

Il s’agit du début du workflow. Au cours de cette étape, vous rassemblez ou créez le contenu source qui s’adapte au workflow que vous utilisez et vous le chargez dans Dynamic Media Classic. Le système prend en charge plusieurs types de fichiers pour les images, les vidéos et les polices, mais aussi pour PDF, Adobe Illustrator et Adobe InDesign.

Consultez la liste complète des [Types de fichiers pris en charge](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#supported-asset-file-formats).

Vous pouvez charger du contenu source de plusieurs manières différentes :

- Directement à partir de votre bureau ou du réseau local. [Découvrez comment](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-sps-desktop-application).
- à partir d’un serveur FTP Dynamic Media Classic. [Découvrez comment](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-files-using-via-ftp).

Le mode par défaut est À partir du bureau, où vous recherchez des fichiers sur votre réseau local et lancez le téléchargement.

![image](assets/main-workflow/upload.jpg)

>[!TIP]
>
>N’ajoutez pas manuellement vos dossiers. Exécutez plutôt un transfert depuis FTP et utilisez la variable **Inclure les sous-dossiers** pour recréer la structure de dossiers dans Dynamic Media Classic.

Les deux options de chargement les plus importantes sont activées par défaut : **Marquer pour publication**, dont nous avons parlé précédemment, et **Remplacer**. L’option Remplacer signifie que si le fichier en cours de chargement porte le même nom qu’un fichier existant déjà dans le système, le nouveau fichier remplacera la version existante. Si vous décochez cette option, le fichier risque de ne pas être chargé.

### Options de remplacement lors du téléchargement d’images

Il existe quatre variantes de l’option Écraser l’image qui peuvent être définies pour l’ensemble de l’entreprise et qui sont souvent mal comprises. En résumé, vous définissez les règles de sorte que les ressources portant le même nom soient remplacées plus fréquemment ou vous souhaitez que les remplacements se produisent moins fréquemment (auquel cas la nouvelle image sera renommée avec une extension &quot;-1&quot; ou &quot;-2&quot;).

- **Remplacer dans le dossier actuel, même nom/même extension de fichier de base**. Cette option est la règle la plus stricte pour le remplacement. Elle implique que vous chargiez l’image de remplacement dans le même dossier que l’original, et qu’elle ait la même extension que le fichier d’origine. Si ces conditions ne sont pas remplies, un doublon est créé.

- **Écraser dans le dossier actuel, même nom de fichier de base, extension indépendante**.
Nécessite que vous chargiez l’image de remplacement dans le même dossier que l’original, mais l’extension du nom de fichier peut être différente de celle de l’original. Par exemple, chaise.tif remplace chaise.jpg.

- **Remplacer dans un dossier, même nom/extension de ressource de base**.
Nécessite que l’image de remplacement ait la même extension que l’image originale (par exemple, chaise.jpg doit remplacer chaise.jpg, et non chaise.tif ). Vous pouvez néanmoins télécharger l’image de remplacement dans un dossier différent de celui de l’image d’origine. L’image mise à jour se trouve dans le nouveau dossier ; le fichier d’origine n’est plus disponible à l’emplacement d’origine..

- **Remplacer dans un dossier, même nom de ressource de base, quelle que soit l’extension**.
Cette option est la règle de remplacement la plus concrète. Elle vous permet de charger une image de remplacement dans un dossier autre que celui de l’image d’origine, de charger un fichier avec une extension de nom de fichier différente et de remplacer le fichier original. Si le fichier d’origine se trouve dans un dossier différent, l’image de remplacement est enregistrée dans le nouveau dossier où elle a été chargée.

En savoir plus sur les [Option Remplacer les images](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html#using-the-overwrite-images-option).

Bien que cela ne soit pas obligatoire, lors du chargement à l’aide de l’une des deux méthodes ci-dessus, vous pouvez spécifier les options de tâche pour ce chargement spécifique, par exemple pour planifier un chargement récurrent, définir des options de recadrage lors du chargement, et bien d’autres. Il peut s’avérer utile pour certains workflows. Il est donc intéressant de déterminer s’ils peuvent l’être pour le vôtre.

En savoir plus sur [Options de tâche](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#upload-options).

Le téléchargement est la première étape nécessaire dans un workflow, car Dynamic Media Classic ne peut pas fonctionner avec un contenu qui n’est pas déjà dans son système. En arrière-plan lors du chargement, le système enregistre chaque ressource chargée dans la base de données Dynamic Media Classic centralisée, affecte un identifiant et le copie dans le stockage. En outre, le système convertit les fichiers image dans un format qui permet le redimensionnement et le zoom dynamiques, et convertit les fichiers vidéo dans un format compatible avec le web MP4.

### Concept : Voici ce qui se passe pour les images lorsque vous les téléchargez vers Dynamic Media Classic

Lorsque vous téléchargez une image de n’importe quel type vers Dynamic Media Classic, elle est convertie en un format d’image maître appelé TIFF Pyramid ou TIFF P. Un TIFF P est similaire au format d’une image bitmap de TIFF en couches, sauf qu’au lieu de calques différents, le fichier contient plusieurs tailles (résolutions) de la même image.

![image](assets/main-workflow/pyramid-p-tiff.png)

Lorsque l’image est convertie, Dynamic Media Classic prend un &quot;instantané&quot; de la taille totale de l’image, l’met à l’échelle de moitié et l’enregistre, l’met à l’échelle de moitié à nouveau et l’enregistre, et ainsi de suite jusqu’à ce qu’elle soit remplie avec des multiples paires de la taille d’origine. Par exemple, un TIFF P de 2 000 pixels possède des tailles de 1 000, 500, 250 et 125 pixels (et plus petites) dans le même fichier. Le fichier de TIFF P est le format de ce qu’on appelle une &quot;image originale&quot; dans Dynamic Media Classic.

Lorsque vous demandez une image de taille spécifique, la création du TIFF P permet au serveur d’images pour Dynamic Media Classic de trouver rapidement la taille supérieure suivante et de la réduire. Par exemple, si vous téléchargez une image de 2 000 pixels et demandez une image de 100 pixels, Dynamic Media Classic recherche la version de 125 pixels et la réduit à 100 pixels au lieu de la mettre à l’échelle de 2 000 à 100 pixels. Cela rend l&#39;opération très rapide. En outre, lorsque vous effectuez un zoom sur une image, la visionneuse de zoom ne demande qu’une mosaïque de l’image en cours de zoom, plutôt que l’image en pleine résolution. C’est ainsi que le format d’image principale, le fichier TIFF P, prend en charge le dimensionnement dynamique et le zoom.

De même, vous pouvez charger la vidéo source originale dans Dynamic Media Classic. Lors du transfert, Dynamic Media Classic peut automatiquement la redimensionner et la convertir au format Web MP4.

### Règles de base pour déterminer la taille optimale des images téléchargées

**Chargez des images de la plus grande taille dont vous avez besoin.**

- Si vous devez effectuer un zoom, téléchargez une image haute résolution d’une plage de 1 500 à 2 500 pixels dans la dimension la plus longue. Tenez compte de la quantité de détails que vous souhaitez fournir, de la qualité de vos images sources et de la taille du produit affiché. Par exemple, téléchargez une image de 1 000 pixels pour un tout petit anneau, mais une image de 3 000 pixels pour une scène de pièce entière.
- Si vous n’avez pas besoin de zoomer, téléchargez-le à la taille exacte de son affichage. Si, par exemple, vos pages contiennent des logos ou des images de bannière ou de démarrage, téléchargez-les exactement à la taille 1:1 et appelez-les exactement à cette taille.

**Ne jamais mettre à niveau ou faire exploser vos images avant de les télécharger vers Dynamic Media Classic.** Par exemple, ne pas mettre à niveau une image plus petite pour en faire une image de 2 000 pixels. Ça n&#39;aura pas l&#39;air bon. Avant de télécharger vos images, rapprochez-les autant que possible de la perfection.

**Il n’existe pas de taille minimale pour le zoom, mais par défaut, les visionneuses n’effectuent pas un zoom supérieur à 100 %.** Si votre image est trop petite, elle ne zoomera pas du tout ou ne zoome qu&#39;une petite quantité pour l&#39;empêcher de mal apparaître.

**Bien qu’il n’y ait pas de taille d’image minimale, nous vous déconseillons de télécharger des images géantes.** Une image géante peut être considérée comme de plus de 4 000 pixels. Le téléchargement d’images de cette taille peut présenter des défauts potentiels comme des grains de poussière ou des poils dans l’image. Ces images occupent plus d’espace sur le serveur Dynamic Media Classic, ce qui peut vous faire dépasser les limites de stockage prévues par votre contrat.

En savoir plus sur [Chargement de fichiers](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html#uploading-your-files).

## Étape 2 : Auteur (et publication)

Après avoir créé et téléchargé votre contenu, vous allez créer de nouvelles ressources multimédias enrichies à partir des ressources chargées en exécutant un ou plusieurs sous-workflows. Cela inclut tous les types de collections d’ensembles (visionneuses d’images, d’échantillons, à 360° et de supports variés), ainsi que les modèles. Elle comprend également la vidéo. Nous approfondirons ultérieurement les détails de chaque type de visionneuse de collections d’images et de contenu multimédia vidéo. Cependant, dans la plupart des cas, vous commencez par sélectionner une ou plusieurs ressources (ou ne sélectionner aucune ressource) et par choisir le type de ressource à créer. Par exemple, vous pouvez sélectionner une image principale et quelques vues de cette image et choisir de créer une visionneuse d’images, une collection d’affichages alternatifs du même produit.

>[!IMPORTANT]
>
>Assurez-vous que toutes vos ressources sont marquées pour publication. Bien que toutes les ressources soient automatiquement marquées pour publication au chargement, toutes les ressources nouvellement créées issues du contenu chargé doivent également être marquées pour publication.

Après avoir créé votre nouvelle ressource, vous exécuterez une tâche de publication. Vous pouvez le faire manuellement ou planifier une tâche de publication qui s’exécute automatiquement. La publication copie tout le contenu de la sphère privée Dynamic Media Classic vers la sphère publique du serveur de publication de l’équation. Le produit d’une tâche de publication Dynamic Media est une URL unique pour chaque ressource publiée.

Le serveur sur lequel vous publiez dépend du type de contenu et de workflow. Par exemple, toutes les images vont au serveur d’images et la vidéo en continu au serveur FMS. À titre de référence, nous parlerons d’un événement &quot;publication&quot; sur un seul serveur.

La publication publie tout le contenu marqué pour publication, et pas seulement votre contenu. En règle générale, un administrateur unique publie au nom de tous plutôt que des utilisateurs individuels exécutant une publication. L’administrateur peut publier ou configurer une tâche récurrente quotidienne, hebdomadaire ou même toutes les 10 minutes qui sera automatiquement publiée. Publiez selon un calendrier adapté à votre entreprise.

>[!TIP]
>
>Automatisez vos tâches de publication et planifiez l’exécution d’une publication complète tous les jours à 00h00 ou à n’importe quelle heure du soir.

### Concept : Présentation de l’URL Dynamic Media Classic

Le produit final d’un workflow Dynamic Media Classic est une URL qui pointe vers la ressource (visionneuse d’images ou de vidéos adaptatives). Ces URL sont très prévisibles et suivent le même modèle. Dans le cas des images, chaque image est générée à partir de l’image principale en TIFF P.

Voici la syntaxe de l’URL d’une image avec quelques exemples :

![image](assets/main-workflow/dmc-url.jpg)

Dans l’URL, tout ce qui se trouve à gauche du point d’interrogation est le chemin virtuel vers une image spécifique. Tout ce qui se trouve à droite du point d’interrogation est un modificateur Image Server, une instruction permettant de traiter l’image. Lorsque vous avez plusieurs modificateurs, ils sont séparés par des esperluettes.

Dans le premier exemple, le chemin d’accès virtuel à l’image &quot;Backpack_A&quot; est : `http://sample.scene7.com/is/image/s7train/BackpackA`. Les modificateurs du serveur d’images redimensionnent l’image sur une largeur de 250 pixels (à partir de wid=250) et rééchantillonnent l’image à l’aide de l’algorithme d’interpolation Lanczos, qui l’accentue au fur et à mesure de son redimensionnement (à partir de resMode=sharp2).

Le deuxième exemple applique un &quot;paramètre d’image prédéfini&quot; à la même image Backpack_A, comme indiqué par $!_template300$. Les symboles $ de chaque côté de l’expression indiquent qu’un paramètre d’image prédéfini, un ensemble de modificateurs d’image empaqueté, est appliqué à l’image.

Une fois que vous avez compris comment les URL Dynamic Media Classic sont assemblées, vous comprenez comment les modifier par programmation et comment les intégrer plus profondément à votre site et à vos systèmes principaux.

### Concept : Présentation du délai de mise en cache

Les ressources récemment chargées et publiées sont immédiatement affichées, tandis que les ressources mises à jour peuvent être soumises au délai de mise en cache de 10 heures. Par défaut, toutes les ressources publiées ont un minimum de 10 heures avant leur expiration. Nous disons minimum, parce que chaque fois que l&#39;image est vue, elle démarre une horloge qui n&#39;expire pas avant que 10 heures se soient écoulées dans lesquelles personne n&#39;a vu cette image. Cette période de 10 heures est la &quot;durée de vie&quot; d’une ressource. Une fois le cache expiré pour cette ressource, la version mise à jour peut être diffusée.

En règle générale, il ne s’agit pas d’un problème sauf si une erreur s’est produite et que l’image/la ressource porte le même nom que la version précédemment publiée, mais qu’il existe un problème avec l’image. Par exemple, vous avez accidentellement téléchargé une version basse résolution ou votre directeur artistique n’a pas approuvé l’image. Dans ce cas, vous souhaitez rappeler l’image d’origine et la remplacer par une nouvelle version utilisant le même ID de ressource.

Découvrez comment [Effacer manuellement le cache des URL qui doivent être mises à jour](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=fr).

>[!TIP]
>
>Pour éviter des problèmes liés au délai de mise en cache, veillez à toujours anticiper les problèmes : une soirée, un jour, deux semaines, etc. Créez à temps pour que les parties internes valident votre travail avant de le publier au public. Même travailler une soirée avant vous permet d&#39;apporter des modifications et de republier ce soir-là. Le matin, les 10 heures se sont écoulées et le cache se met à jour avec la bonne image.

- En savoir plus sur [Création d’une tâche de publication](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html#creating-a-publish-job).
- En savoir plus sur [Publication](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html).

## Étape 3 : Diffuser

N’oubliez pas que le produit final d’un workflow Dynamic Media Classic est une URL qui pointe vers la ressource. L’URL peut pointer vers une image individuelle, une visionneuse d’images, une visionneuse à 360° ou une autre collection ou vidéo de visionneuse d’images. Vous devez utiliser cette URL et y faire quelque chose, par exemple modifier votre HTML pour que la variable `<IMG>` les balises pointent vers l’image Dynamic Media Classic au lieu de pointer vers une image provenant de votre site actuel.

À l’étape Diffuser , vous devez intégrer ces URL dans votre site web, application mobile, campagne par e-mail ou tout autre point de contact numérique sur lequel vous souhaitez afficher la ressource.

Exemple d&#39;intégration de l&#39;URL Dynamic Media Classic d&#39;une image dans un site web :

![image](assets/main-workflow/example-url-1.png)

L’URL en rouge est le seul élément spécifique à Dynamic Media Classic.

Votre équipe informatique ou partenaire d’intégration peut prendre l’initiative d’écrire et de modifier du code pour intégrer des URL Dynamic Media Classic à votre site. Adobe dispose d’une équipe de conseillers qui peut vous aider dans cette tâche, soit en vous fournissant des conseils techniques, créatifs ou généraux.

Pour les solutions plus complexes telles que les visionneuses de zoom ou les visionneuses qui combinent le zoom à des vues alternatives, l’URL pointe généralement vers une visionneuse hébergée par Dynamic Media Classic, et cette URL est également une référence à un ID de ressource.

Exemple de lien (en rouge) qui ouvre une visionneuse d’images dans une visionneuse dans une nouvelle fenêtre contextuelle :

![image](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Vous devez intégrer les URL Dynamic Media Classic dans votre site web, application mobile, courrier électronique et autres points de contact numériques — Dynamic Media Classic ne peut pas le faire pour vous !

## Aperçu des ressources

Vous voudrez probablement prévisualiser les ressources que vous avez chargées ou que vous créez ou modifiez pour vous assurer qu’elles apparaissent comme vous le souhaitez lorsque vos clients les visualisent. Vous pouvez accéder à la fenêtre Aperçu en cliquant sur l’une des **Aperçu** , soit sur la miniature de la ressource, en haut de la **Panneau Parcourir/Créer** ou en accédant à **Fichier > Aperçu**. Dans une fenêtre de navigateur, il prévisualise la ressource qui se trouve actuellement dans le panneau, qu’il s’agisse d’une image, d’une vidéo ou d’une ressource générée telle qu’une visionneuse d’images.

### Aperçu de la taille dynamique (paramètres d’image prédéfinis)

Vous pouvez prévisualiser vos images en plusieurs tailles à l’aide de la variable **Tailles** aperçu. Cette opération charge une liste de vos paramètres d’image prédéfinis disponibles. Nous discuterons des paramètres d’image prédéfinis ultérieurement, mais pensez à ces paramètres comme à des &quot;recettes&quot; pour charger votre image à une taille nommée, avec des quantités spécifiques d’accentuation et de qualité d’image.

### Aperçu de zoom

Vous pouvez également utiliser la variable **Zoom** pour prévisualiser votre image dans l’un des nombreux paramètres prédéfinis de zoom prédéfinis, reposant sur différentes visionneuses de zoom incluses.

En savoir plus sur [Aperçu des ressources](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html).
