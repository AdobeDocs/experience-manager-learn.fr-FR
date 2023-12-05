---
title: Workflow principal de Dynamic Media Classic et aperçu des ressources
description: 'Découvrez le workflow principal de Dynamic Media Classic et ses trois étapes : Création (et chargement), Service de création (et publication) et Diffusion. Découvrez comment prévisualiser des ressources dans Dynamic Media Classic.'
feature: Dynamic Media Classic
topic: Content Management
role: User
level: Beginner
doc-type: Tutorial
exl-id: 04aacd81-bbb2-4742-9306-f0eabc665a41
duration: 700
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '2658'
ht-degree: 99%

---

# Workflow principal de Dynamic Media Classic et aperçu des ressources {#main-workflow}

Dynamic Media prend en charge un workflow de création (et de chargement), de service de création (et de publication) et de diffusion. Commencez par charger des ressources, puis travaillez dessus, vous pouvez par exemple créer une visionneuse d’images et les publier en ligne. L’étape de construction est facultative pour certains workflows. Par exemple, si votre objectif est de n’effectuer que le dimensionnement et le zoom dynamiques sur des images ou de convertir et publier une vidéo en flux continu, aucune étape de construction n’est nécessaire.

![image](assets/main-workflow/create-author-deliver.jpg)

Le workflow des solutions Dynamic Media Classic se compose de trois étapes principales :

1. Création (et chargement) du contenu source
2. Service de création (et publication) des ressources
3. Diffusion des ressources

## Étape 1 : création (et chargement)

Il s’agit du début du workflow. Au cours de cette étape, vous rassemblez ou créez le contenu source qui s’adapte au workflow que vous utilisez et vous le chargez dans Dynamic Media Classic. Le système prend en charge plusieurs types de fichiers pour les images, les vidéos et les polices, mais aussi pour PDF, Adobe Illustrator et Adobe InDesign.

Consultez la liste complète des [Types de fichiers pris en charge](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=fr#supported-asset-file-formats).

Vous pouvez charger du contenu source de plusieurs manières différentes :

- Directement depuis votre poste de travail ou votre réseau local. [Découvrez comment](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=fr#upload-files-using-sps-desktop-application).
- À partir d’un serveur FTP Dynamic Media Classic. [Découvrez comment](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=fr#upload-files-using-via-ftp).

Le mode par défaut est À partir du poste de travail. Vous recherchez des fichiers sur votre réseau local et lancez le chargement.

![image](assets/main-workflow/upload.jpg)

>[!TIP]
>
>N’ajoutez pas manuellement vos dossiers. Exécutez plutôt un transfert depuis FTP et utilisez l’option **Inclure les sous-dossiers** pour recréer votre structure de dossiers dans Dynamic Media Classic.

Les deux options de chargement les plus importantes sont activées par défaut : **Marquer pour publication**, dont nous avons parlé précédemment, et **Remplacer**. L’option Remplacer signifie que si le fichier en cours de chargement porte le même nom qu’un fichier existant déjà dans le système, le nouveau fichier remplacera la version existante. Si vous décochez cette option, le fichier risque de ne pas être chargé.

### Options Remplacer lors du chargement d’images

Il existe quatre variations de l’option Remplacer l’image. Elles peuvent être définies pour l’ensemble de l’entreprise, mais sont souvent mal comprises. En résumé, vous définissez les règles de sorte que les ressources portant le même nom soient remplacées plus fréquemment ou que les remplacements se produisent moins fréquemment (auquel cas la nouvelle image sera renommée avec une extension « -1 » ou « -2 »).

- **Remplacement dans le dossier actuel, même nom/extension de l’image de base**.
Cette option est la règle la plus stricte pour le remplacement. Elle implique que vous chargiez l’image de remplacement dans le même dossier que l’original, et qu’elle ait la même extension que le fichier d’origine. Si ces conditions ne sont pas remplies, un doublon est créé.

- **Remplacement dans le dossier actuel, même nom de ressource de base, quelle que soit l’extension**.
Nécessite que vous chargiez l’image de remplacement dans le même dossier que l’original, mais l’extension du nom de fichier peut être différente de celle de l’original. Par exemple, chaise.tif remplace chaise.jpg.

- **Remplacer dans un dossier, même nom/même extension de fichier de base**.
Nécessite que l’image de remplacement ait la même extension que l’image originale (par exemple, chaise.jpg doit remplacer chaise.jpg, et non chaise.tif ). Vous pouvez toutefois charger l’image de remplacement dans un dossier différent de celui de l’image d’origine. L’image mise à jour se trouve dans le nouveau dossier. Le fichier n’est plus accessible à son emplacement d’origine.

- **Remplacement dans un dossier, même nom de ressource de base, quelle que soit l’extension**.
Cette option est la règle de remplacement la plus concrète. Elle vous permet de charger une image de remplacement dans un dossier autre que celui de l’image d’origine, de charger un fichier avec une extension de nom de fichier différente et de remplacer le fichier original. Si le fichier d’origine se trouve dans un dossier différent, l’image de remplacement est enregistrée dans le nouveau dossier où elle a été chargée.

En savoir plus sur l’ [Option Remplacer les images](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/setup/application-setup.html?lang=fr#using-the-overwrite-images-option).

Bien que cela ne soit pas obligatoire, lors du chargement à l’aide de l’une des deux méthodes ci-dessus, vous pouvez spécifier les options de tâche pour ce chargement spécifique. Par exemple pour planifier un chargement récurrent, définir des options de recadrage lors du chargement, etc. Cela peut s’avérer utile pour certains workflows. Il est donc intéressant de déterminer si c’est le cas pour le vôtre.

En savoir plus sur les [Options de tâche](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=fr#upload-options).

Le téléchargement est la première étape nécessaire dans un workflow, car Dynamic Media Classic ne peut pas fonctionner avec un contenu qui n’est pas déjà dans son système. En arrière-plan lors du chargement, le système enregistre chaque ressource chargée dans la base de données centralisée Dynamic Media Classic et leur affecte un ID qui est copié dans le stockage. En outre, le système convertit les fichiers image dans un format qui permet le redimensionnement et le zoom et convertit les fichiers vidéo dans un format compatible web MP4.

### Concept : voici ce qui se passe pour les images lorsque vous les chargez dans Dynamic Media Classic.

Lorsque vous téléchargez une image de n’importe quel type vers Dynamic Media Classic, elle est convertie en un format d’image principale appelée Pyramid TIFF ou P-TIFF. Un P-TIFF est similaire au format d’une image bitmap de TIFF avec des calques, sauf qu’au lieu de calques différents, le fichier contient plusieurs résolutions de la même image.

![image](assets/main-workflow/pyramid-p-tiff.png)

Lorsque l’image est convertie, Dynamic Media Classic prend un « instantané » de la taille totale de l’image, le réduit à l’échelle de moitié et l’enregistre, le réduit à l’échelle de moitié à nouveau et l’enregistre, et ainsi de suite jusqu’à ce qu’elle soit remplie avec des multiples pairs de la taille d’origine. Par exemple, un P-TIFF de 2 000 pixels existe en résolutions de 1 000, 500, 250 et 125 pixels (et moins) dans le même fichier. Le fichier P-TIFF est le format de ce qu’on appelle une « image principale » dans Dynamic Media Classic.

Lorsque vous demandez une image de taille spécifique, la création du P-TIFF permet au serveur d’images pour Dynamic Media Classic de trouver rapidement la résolution supérieure suivante et de la réduire. Par exemple, si vous téléchargez une image de 2 000 pixels et demandez une image de 100 pixels, Dynamic Media Classic recherche la version de 125 pixels et la réduit à 100 pixels au lieu de la mettre à l’échelle de 2 000 à 100 pixels. Cela rend l’opération très rapide. En outre, lorsque vous effectuez un zoom sur une image, la visionneuse de zoom ne demande qu’une mosaïque de l’image en cours de zoom, plutôt que l’image en pleine résolution. C’est ainsi que le format d’image principale, le fichier P-TIFF, prend en charge le dimensionnement dynamique et le zoom.

De même, vous pouvez charger la vidéo source principale dans Dynamic Media Classic. Lors du transfert, Dynamic Media Classic peut automatiquement la redimensionner et la convertir au format compatible avec le web MP4.

### Règles de base pour déterminer la taille optimale des images chargées

**Chargez des images avec la plus haute résolution dont vous avez besoin.**

- Si vous devez effectuer un zoom, téléchargez une image haute résolution entre 1 500 et 2 500 pixels dans la dimension la plus élevée. Tenez compte de la quantité de détails que vous souhaitez fournir, de la qualité de vos images sources et de la taille du produit affiché. Par exemple, chargez une image de 1 000 pixels pour un tout petit encadré, mais une image de 3 000 pixels pour une scène entière.
- Si vous n’avez pas besoin de zoomer, chargez-la à la taille exacte de son affichage. Si, par exemple, vos pages contiennent des logos ou des images de bannière ou de démarrage, chargez-les exactement à leur taille 1:1 et appelez-les exactement à cette taille.

**N’échantillonez ni ne divisez jamais vos images avant de les charger vers Dynamic Media Classic.** Par exemple, n’échantillonez pas une image plus petite pour en faire une image de 2 000 pixels. Le résultat ne sera pas bon. Avant de charger vos images, rapprochez-les autant que possible de la perfection.

**Il n’existe pas de taille minimale pour le zoom, mais par défaut, les visionneuses n’effectuent pas un zoom supérieur à 100 %.** Si votre image est trop petite, elle ne zoomera pas du tout ou ne zoomera que très légèrement, pour empêcher un rendu de mauvaise qualité.

**Bien qu’il n’y ait pas de taille d’image minimale, nous vous déconseillons de charger des images géantes.** Une image géante correspond à plus de 4 000 pixels. Le téléchargement d’images de cette taille peut présenter des défauts potentiels comme de la poussière ou du bruit dans l’image. Ces images occupent plus d’espace sur le serveur Dynamic Media Classic, ce qui peut vous faire dépasser les limites de stockage prévues par votre contrat.

En savoir plus sur le [Chargement de fichiers](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/uploading-files.html?lang=fr#uploading-your-files).

## Étape 2 : création (et publication)

Après avoir créé et chargé votre contenu, vous allez créer de nouvelles ressources de médias riches à partir des ressources chargées en exécutant un ou plusieurs sous-workflows. Cela inclut tous les types de collections d’ensembles : image, échantillon, visionneuses à 360° et de supports variés, ainsi que les modèles. Cela inclut également la vidéo. Nous approfondirons ultérieurement les détails de chaque type d’ensemble de collections d’images et de médias riches vidéo. Cependant, dans la plupart des cas, vous commencez par sélectionner une ou plusieurs ressources (ou aucune ressource) et par choisir le type de ressource à créer. Par exemple, vous pouvez sélectionner une image principale et quelques vues de cette image et choisir de créer une visionneuse d’images, une collection de vues alternatives du même produit.

>[!IMPORTANT]
>
>Assurez-vous que toutes vos ressources sont marquées pour publication. Bien que toutes les ressources soient automatiquement marquées pour publication lors du chargement, toutes les ressources nouvellement créées issues du contenu chargé doivent également être marquées pour publication.

Après avoir créé votre nouvelle ressource, vous exécuterez une tâche de publication. Vous pouvez le faire manuellement ou planifier une tâche de publication qui s’exécute automatiquement. La publication copie tout le contenu de la sphère privée, Dynamic Media Classic, vers la sphère publique, le serveur de publication, de l’équation. Le produit d’une tâche de publication Dynamic Media est une URL unique pour chaque ressource publiée.

Le serveur sur lequel vous publiez dépend du type de contenu et du workflow. Par exemple, toutes les images vont au serveur d’images et la vidéo en streaming au serveur FMS. À titre de référence, nous parlerons d’un événement de « publication » sur un seul serveur.

La publication publie tout le contenu marqué pour publication, et pas seulement votre contenu. En règle générale, une seule personne en charge de l’administration publie au nom de tout le monde, plutôt que des personnes individuelles exécutant une publication. La personne en charge de l’administration peut publier ou configurer une tâche récurrente quotidienne, hebdomadaire ou même toutes les 10 minutes qui sera automatiquement publiée. Publiez selon un planning adapté à votre entreprise.

>[!TIP]
>
>Automatisez vos tâches de publication et planifiez l’exécution d’une publication complète tous les jours à minuit ou à une heure tardive.

### Concept : présentation de l’URL Dynamic Media Classic

Le produit final d’un workflow Dynamic Media Classic est une URL qui pointe vers la ressource (visionneuse d’images ou de vidéos adaptatives). Ces URL sont très prévisibles et suivent le même modèle. Dans le cas des images, chaque image est générée à partir de l’image principale en P-TIFF.

Voici la syntaxe de l’URL d’une image avec quelques exemples :

![image](assets/main-workflow/dmc-url.jpg)

Dans l’URL, tout ce qui se trouve à gauche du point d’interrogation est le chemin virtuel vers une image spécifique. Tout ce qui se trouve à droite du point d’interrogation est un modificateur du serveur d’images, une instruction sur la manière de traiter l’image. Lorsque vous avez plusieurs modificateurs, ils sont séparés par des esperluettes.

Dans le premier exemple, le chemin d’accès virtuel à l’image « Backpack_A » est `http://sample.scene7.com/is/image/s7train/BackpackA`. Les modificateurs du serveur d’images redimensionnent l’image sur une largeur de 250 pixels (à partir de wid=250) et rééchantillonnent l’image à l’aide de l’algorithme d’interpolation Lanczos, qui accentue la netteté au fur et à mesure de son redimensionnement (à partir de resMode=sharp2).

Le deuxième exemple applique un « paramètre prédéfini d’image » à la même image Backpack_A, comme indiqué par $!_template300$. Les symboles $ de chaque côté de l’expression indiquent qu’un paramètre prédéfini d’image, un ensemble de modificateurs d’image, est appliqué à l’image.

Une fois que vous avez compris comment les URL Dynamic Media Classic sont assemblées, vous comprenez comment les modifier par programmation et comment les intégrer plus profondément à votre site et à vos systèmes back-end.

### Concept : présentation du délai de mise en cache

Les ressources récemment chargées et publiées sont immédiatement affichées, tandis que les ressources mises à jour peuvent être soumises au délai de mise en cache de 10 heures. Par défaut, toutes les ressources publiées ont un minimum de 10 heures avant d’expirer. Nous disons minimum, parce que chaque fois que l’image est vue, une horloge est déclenchée qui n’expire que lorsque 10 heures se sont écoulées sans que personne n’ait vu cette image. Cette période de 10 heures est la « durée de vie » (TTL) d’une ressource. Une fois que le cache expire pour cette ressource, la version mise à jour peut être diffusée.

En règle générale, il ne s’agit pas d’un problème, sauf si une erreur s’est produite et que l’image/la ressource porte le même nom que la version précédemment publiée, mais qu’il existe un problème avec l’image. Par exemple, vous avez accidentellement chargé une version de faible résolution ou votre directeur ou directrice artistique n’a pas approuvé l’image. Dans ce cas, vous souhaitez rappeler l’image d’origine et la remplacer par une nouvelle version utilisant le même ID de ressource.

Découvrez comment [effacer manuellement le cache des URL à mettre à jour](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/assets/dynamicmedia/invalidate-cdn-cache-dynamic-media.html?lang=fr).

>[!TIP]
>
>Pour éviter des problèmes liés au délai de mise en cache, travaillez toujours à l’avance : un soir, un jour, deux semaines, etc. Prévoyez une période d’assurance qualité/acceptation pour que les parties internes puissent vérifier votre travail avant de le rendre public. Même en travaillant la veille au soir, vous pouvez apporter des modifications et republier ce soir-là. Le matin, les 10 heures se sont écoulées et le cache se met à jour avec la bonne image.

- En savoir plus sur la [création d’une tâche de publication](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html?lang=fr#creating-a-publish-job).
- En savoir plus sur la.[publication](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/upload-publish/publishing-files.html?lang=fr).

## Étape 3 : diffuser

N’oubliez pas que le produit final d’un workflow Dynamic Media Classic est une URL qui pointe vers la ressource. L’URL peut pointer vers une image individuelle, une visionneuse d’images, une visionneuse à 360° ou une autre collection de visionneuses d’images ou de vidéos. Vous devez utiliser cette URL et y faire quelque chose, par exemple modifier votre HTML pour que les balises `<IMG>` pointent vers l’image Dynamic Media Classic au lieu de pointer vers une image provenant de votre site actuel.

À l’étape Diffuser, vous devez intégrer ces URL dans votre site web, application mobile, campagne par e-mail ou tout autre point de contact numérique sur lequel vous souhaitez afficher la ressource.

Exemple d’intégration de l’URL Dynamic Media Classic d’une image dans un site web :

![image](assets/main-workflow/example-url-1.png)

L’URL en rouge est le seul élément spécifique à Dynamic Media Classic.

Votre équipe informatique ou partenaire d’intégration peut prendre l’initiative d’écrire et de modifier du code pour intégrer des URL Dynamic Media Classic à votre site. Adobe dispose d’une équipe de conseil dont les personnes membres peuvent vous aider dans cette tâche en vous fournissant des conseils techniques, créatifs ou généraux.

Pour les solutions plus complexes telles que les visionneuses de zoom ou les visionneuses qui combinent le zoom à des vues alternatives, l’URL pointe généralement vers une visionneuse hébergée par Dynamic Media Classic, et cette URL est également une référence à un ID de ressource.

Exemple de lien (en rouge) qui ouvre une visionneuse d’images dans une visionneuse dans une nouvelle fenêtre contextuelle :

![image](assets/main-workflow/example-url-2.png)

>[!IMPORTANT]
>
>Vous devez intégrer les URL Dynamic Media Classic dans votre site web, application mobile, e-mail et autres points de contact numériques ; Dynamic Media Classic ne peut pas le faire pour vous.

## Prévisualiser les ressources

Vous voudrez probablement prévisualiser les ressources que vous avez chargées ou que vous créez ou modifiez pour vous assurer qu’elles apparaissent comme vous le souhaitez lorsque vos clientes ou clients les visualisent. Vous pouvez accéder à la fenêtre Aperçu en cliquant sur l’un des boutons **Aperçu**, soit sur la miniature de la ressource, en haut du **panneau Parcourir/Créer** soit en accédant à **Fichier > Aperçu**. Dans une fenêtre du navigateur, vous pouvez obtenir un aperçu de la ressource qui se trouve actuellement dans le panneau, qu’il s’agisse d’une image, d’une vidéo ou d’une ressource générée telle qu’une visionneuse d’images.

### Aperçu de la taille dynamique (paramètres d’image prédéfinis)

Vous pouvez prévisualiser vos images en plusieurs tailles à l’aide de l’aperçu **Tailles**. Cette opération charge une liste de vos paramètres d’image prédéfinis disponibles. Nous discuterons des paramètres d’image prédéfinis ultérieurement, mais veuillez les considérer déjà comme des « recettes » pour charger votre image à une taille nommée, avec des quantités spécifiques d’accentuation et de qualité d’image.

### Aperçu zoom

Vous pouvez également utiliser l’option **zoom** pour prévisualiser votre image dans l’un des nombreux paramètres préconfigurés de zoom prédéfinis, qui sont basés sur différentes visionneuses de zoom incluses.

En savoir plus sur l’[Aperçu des ressources](https://experienceleague.adobe.com/docs/dynamic-media-classic/using/managing-assets/previewing-asset.html?lang=fr).
