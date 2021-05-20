---
title: Recadrage, images ajustées et cibles de zoom
description: L’image principale de Dynamic Media Classic prend en charge la création de versions recadrées distinctes pour chaque image afin d’afficher les détails ou les échantillons sans avoir à créer des versions recadrées distinctes pour chaque image. Découvrez comment recadrer des images dans Dynamic Media Classic et les enregistrer en tant que nouveau fichier maître ou image virtuelle, enregistrer des images modifiées virtuelles et les utiliser à la place des ressources originales, puis créer des cibles de zoom sur vos images pour afficher les détails mis en surbrillance.
sub-product: dynamic-media
feature: Dynamic Media Classic
doc-type: tutorial
topics: development, authoring, configuring
audience: all
activity: use
topic: Gestion de contenu
role: Business Practitioner
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '2670'
ht-degree: 0%

---


# Recadrage, images ajustées et cibles de zoom {#crop-adjusted-zoom-targets}

L’une des principales forces du concept d’image principale de Dynamic Media Classic est que vous pouvez réutiliser la ressource d’image à de nombreuses fins. Traditionnellement, vous devez créer des versions recadrées distinctes de chaque image pour afficher les détails ou les échantillons. Lors de l’utilisation de Dynamic Media Classic, vous pouvez effectuer les mêmes tâches sur votre seul maître et enregistrer ces versions recadrées en tant que nouveaux fichiers physiques ou en tant que dérivés virtuels qui ne prennent pas d’espace de stockage.

À la fin de cette section du tutoriel, vous saurez comment :

- Recadrez les images dans Dynamic Media Classic et enregistrez-les sous forme de nouveaux fichiers maîtres ou d’images virtuelles. [En savoir plus](https://docs.adobe.com/help/en/dynamic-media-classic/using/master-files/cropping-image.html).
- Enregistrez les images modifiées virtuelles et utilisez-les à la place des ressources originales. [En savoir plus](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/adjusting-image.html).
- Créez des cibles de zoom sur vos images pour afficher leurs mises en surbrillance. [En savoir plus](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html).

## Recadrage

Dynamic Media Classic propose quelques outils de retouche d’images, notamment l’outil Recadrer. Pour plusieurs raisons, vous pouvez recadrer votre image principale dans Dynamic Media Classic. Par exemple :

- Vous n’avez pas accès au fichier d’origine. Vous souhaitez afficher l’image avec un recadrage ou des proportions différents, mais vous ne disposez pas du fichier d’origine sur votre ordinateur ni ne travaillez depuis votre domicile. Dans ce cas, vous pouvez accéder à Dynamic Media Classic, rechercher l’image, la recadrer et l’enregistrer ou l’enregistrer sous une nouvelle version.
- Pour supprimer l’espace blanc excessif. L&#39;image a été photographiée avec trop d&#39;espace blanc, ce qui donne l&#39;impression que le produit est petit. Vous souhaitez que vos images miniatures remplissent la zone de travail autant que possible.
- Pour créer des images ajustées, des copies virtuelles des images qui ne prennent pas d’espace disque. Certaines entreprises ont des règles de fonctionnement qui les obligent à conserver des copies distinctes de la même image, mais avec un nom différent. Ou peut-être que vous voulez une version recadrée et non recadrée de la même image.
- Pour créer de nouvelles images à partir d’une image source. Par exemple, vous pouvez créer des échantillons de couleurs ou un détail de l’image principale. Vous pouvez effectuer cette opération dans Adobe Photoshop et charger séparément ou utiliser l’outil Recadrer dans Dynamic Media Classic.

>[!NOTE]
>
>Toutes les URL des discussions suivantes sur le recadrage sont proposées à titre d’illustration uniquement. ce ne sont pas des liens en direct.

### Utilisation de l’outil de recadrage

Vous pouvez accéder à l’outil Recadrage dans Dynamic Media Classic à partir de la page Détails d’une ressource ou en cliquant sur le bouton **Modifier** . Vous pouvez utiliser l’outil pour recadrer de deux manières :

- Mode de recadrage par défaut dans lequel vous faites glisser les poignées de la fenêtre de recadrage ou saisissez des valeurs dans la zone Taille. Découvrez comment effectuer un [recadrage manuel](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#select-an-area-to-crop).
- Rogner. Utilisez cette option pour supprimer des espaces blancs supplémentaires autour de votre image en calculant le nombre de pixels qui ne correspondent pas à votre image. Découvrez comment [Recadrer par rognage](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/master-files/cropping-image.html#crop-to-remove-white-space-around-an-image).

### _Recadrage manuel_

Lorsque vous enregistrez une version recadrée manuellement, il s’affiche que l’image est recadrée en permanence ; Dynamic Media Classic masque en fait les pixels en ajoutant un modificateur d’URL interne pour recadrer l’image. Lorsque vous publiez, il apparaît à tous que l’image est recadrée. Vous pouvez toutefois revenir à l’éditeur de recadrage et supprimer le recadrage ultérieurement.

Vous pouvez ensuite choisir d’enregistrer en tant que nouvelle image de Principal ou en tant que vue supplémentaire du gabarit. Un nouveau gabarit est un nouveau fichier physique (tel qu’un fichier TIFF ou JPEG) qui occupe de l’espace de stockage. Une vue supplémentaire est une image virtuelle qui ne prend pas d’espace serveur. Nous vous déconseillons de choisir Remplacer l’original, car cela remplacera votre gabarit et rendra le recadrage permanent. Si vous enregistrez en tant que nouveau gabarit ou vue supplémentaire, vous devez choisir un nouvel ID de ressource. Comme les autres identifiants de ressource, ce nom doit être unique dans Dynamic Media Classic.

### _Rognage_

Si vous téléchargez une image avec trop d’espace (zone de travail supplémentaire) autour de l’objet principal de l’image, elle sera beaucoup plus petite sur le Web lors du redimensionnement. C’est particulièrement vrai pour les miniatures de 150 pixels ou moins — le sujet de la photo peut se perdre dans tout l’espace qui l’entoure.

Comparez ces deux versions de la même image.

![image](assets/crop-adjusted-zoom-targets/trim-cropping.jpg)

L’image à droite devient beaucoup plus visible en supprimant l’espace supplémentaire autour du produit. Le rognage peut être effectué image par image, à l’aide de l’outil Recadrer ou exécuté en tant que traitement par lot lors du chargement. Il est recommandé de l’exécuter en tant que traitement par lot si vous souhaitez que toutes vos images soient recadrées de manière cohérente aux limites de l’objet principal. Rogner le recadrage sur le cadre de sélection : le rectangle entourant l’image.

>[!NOTE]
>
>Rogner ne crée pas de transparence autour de l’image. Pour ce faire, vous devez incorporer un chemin de détourage dans l’image et utiliser l’option de téléchargement **Créer un masque à partir du chemin du clip**.
>
>En outre, pour restaurer l’état d’origine d’une image après l’avoir recadrée lorsque vous avez utilisé l’option **Enregistrer**, affichez l’image dans l’écran Éditeur de recadrage et sélectionnez le bouton **Réinitialiser** .

### _Recadrage lors du téléchargement_

Comme mentionné précédemment, vous pouvez également choisir de recadrer les images au fur et à mesure de leur téléchargement. Pour utiliser le recadrage de rognage lors du chargement, cliquez sur le bouton **Options de tâche**, puis, sous Options de recadrage, sélectionnez **Rogner**.

Dynamic Media Classic se souviendra de cette option pour le prochain chargement. Même si vous souhaitez peut-être qu’il recadre les images pour ce transfert, il est préférable de ne pas les recadrer pour chaque téléchargement. Une autre option consiste à définir une tâche de téléchargement FTP planifiée spéciale et à y placer les options de recadrage. Ainsi, vous n’exécuteriez la tâche que lorsque vous avez besoin de recadrer vos images.

>[!IMPORTANT]
>
>Si vous définissez un recadrage pour votre chargement, Dynamic Media Classic place un cookie pour mémoriser ce paramètre la prochaine fois. Pour respecter les bonnes pratiques, cliquez sur le bouton **Réinitialiser les valeurs par défaut de la société** avant votre prochain chargement pour effacer toutes les options de recadrage qui restent à la fin du dernier chargement ; sinon, vous pourriez accidentellement recadrer le lot d’images suivant.

### Recadrage par URL

Bien que cela ne soit pas évident dans Dynamic Media Classic, vous pouvez également recadrer uniquement par l’URL (ou même ajouter un recadrage à un paramètre d’image prédéfini).

Chaque fois que vous utilisez l’outil Recadrage, les valeurs de l’URL s’affichent dans le champ en bas. Vous pouvez prendre ces valeurs et les appliquer directement à une image en tant que modificateurs d’URL.

![](assets/crop-adjusted-zoom-targets/cropping-by-url.png)
_modificateurs de commande imageCrop au bas de l’éditeur de recadrage_

![image](assets/crop-adjusted-zoom-targets/uncropped-cropped.png)

La taille devant être calculée par image lorsque vous utilisez le recadrage par rognage, elle ne peut pas être automatisée via l’URL. Le recadrage de rognage ne peut être exécuté que lors du téléchargement ou en l’appliquant une seule image à la fois.

### _Recadrage du paramètre d’image prédéfini_

Les paramètres d’image prédéfinis comportent un champ dans lequel vous pouvez ajouter des commandes de diffusion d’images supplémentaires. Pour ajouter le même recadrage que ci-dessus à votre paramètre d’image prédéfini, modifiez-le, collez ou saisissez les valeurs dans le champ Modificateurs d’URL, puis enregistrez et publiez.

![](assets/crop-adjusted-zoom-targets/cropping-in-image-preset.jpg)
_imageAjoutez des commandes de recadrage (ou toute commande) aux modificateurs d’URL du paramètre d’image prédéfini._

Le recadrage fait désormais partie de ce paramètre d’image prédéfini et est appliqué automatiquement chaque fois qu’il est utilisé. Bien sûr, cette méthode dépend de toutes les images nécessitant la même quantité de recadrage. Si toutes vos images ne sont pas tournées de la même manière, cette méthode ne fonctionnera pas pour vous.

## Images ajustées

Lorsque vous utilisez l’outil Recadrage, vous avez la possibilité de **Enregistrer comme vue supplémentaire du Principal**. Une fois enregistré, cela crée un nouveau type de ressource Dynamic Media Classic : une image ajustée. Une image ajustée, aussi appelée dérivée, est une image virtuelle. Ce n&#39;est pas du tout une image ; il s’agit d’une référence de base de données (comme un alias ou un raccourci) vers l’image maître physique.

### L&#39;image réelle se tiendra-t-elle `?`

Pouvez-vous dire lequel est le gabarit, et lequel est l’image ajustée ?

![image](assets/crop-adjusted-zoom-targets/real-image-stand-up.png)

Vous ne devriez pas être en mesure de le savoir sans avoir regardé dans Dynamic Media Classic et vu le type de ressource &quot;Image ajustée&quot; pour SBR_MAIN2.

Une image ajustée n’utilise pas d’espace disque, car elle n’existe que sous forme d’élément de ligne dans la base de données. Il est également lié de manière permanente à la ressource d’origine ; si l’original est supprimé, l’image ajustée est également supprimée. Il peut se composer d’une image entière non recadrée ou d’une partie seulement d’une image (recadrage).

![image](assets/crop-adjusted-zoom-targets/adjusted-image.png)

Vous créez généralement des images ajustées à l’aide de l’outil Recadrage ; toutefois, ils peuvent également être créés avec d’autres éditeurs d’image : les outils Ajuster et Accentuer .

Les images ajustées nécessitent un identifiant de ressource unique. Une fois publiées (vous devez publier comme toute autre ressource), elles agissent comme toute autre image et sont appelées sur une URL par leur identifiant de ressource. Sur la page Détails, vous pouvez afficher les images ajustées associées à une image originale sous l’onglet **Créé et dérivés**.

![](assets/crop-adjusted-zoom-targets/derivatives.jpg)
_imagesAdjusted Views pour l’image principale ASIAN_BR_MAIN_

## Cibles de zoom

Les cibles de zoom figurent également dans le menu **Modifier** et la page **Détails** d’une image. Ils vous permettent de définir des &quot;zones réactives&quot; pour mettre en évidence des fonctionnalités de marchandisage spécifiques d’une image de zoom. Au lieu de créer des images distinctes en recadrant un grand gabarit, la visionneuse de zoom peut afficher les détails au-dessus de l’image, ainsi qu’un libellé court que vous créez.

![image](assets/crop-adjusted-zoom-targets/arm-with-watch.jpg)

Les cibles de zoom étant essentiellement une fonction de marchandisage qui nécessite une connaissance des points de vente d’un produit, elles sont généralement créées par une personne de l’équipe Marchandisage ou Produit d’une entreprise.

Le processus est très simple : cliquez sur la fonction, donnez-lui un nom explicite, puis enregistrez. Les cibles peuvent être copiées d’une image à une autre si elles sont similaires, mais le processus est manuel. Dynamic Media Classic ne permet pas d’automatiser la création de cibles de zoom, car chaque image est différente et possède des fonctionnalités différentes.

Le choix de la visionneuse est un autre facteur permettant de décider si vous souhaitez utiliser les cibles de zoom. Tous les types de visionneuses ne peuvent pas afficher de cibles de zoom (par exemple, la visionneuse de visionneuse de fenêtre déroulante ne les prend pas en charge).

Découvrez comment [Créer des cibles de zoom](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/zoom/creating-zoom-targets-guided-zoom.html#creating-and-editing-zoom-targets).

![image](assets/crop-adjusted-zoom-targets/zoom-targets.jpg)

### Utilisation de l’outil Cible de zoom

Voici le workflow de création de cibles dans Dynamic Media Classic.

1. Recherchez votre image, cliquez sur le bouton **Modifier**, puis sélectionnez **Cibles de zoom**.
2. L’éditeur de cible de zoom se charge. Vous verrez votre image au milieu, certains boutons en haut et un panneau cible vide à droite. En bas à gauche, un paramètre prédéfini de visionneuse est sélectionné. La valeur par défaut est &quot;Zoom1-Guided&quot;.
3. Déplacez la zone rouge avec la souris et cliquez pour créer une nouvelle cible.

   - La zone rouge est la zone cible. Lorsqu’un utilisateur clique sur cette cible, il effectue un zoom avant sur la zone à l’intérieur de la zone.
   - La taille cible est déterminée par la taille d’affichage dans le paramètre prédéfini de la visionneuse. Cela détermine la taille de l’image de zoom principale. Voir _Définition de la taille d’affichage_, ci-dessous.

4. Vous verrez la cible que vous venez de créer devenir bleue, et sur la droite vous verrez une version miniature de cette cible, ainsi que le nom par défaut &quot;target-0&quot;.
5. Pour renommer votre cible, cliquez sur sa miniature, saisissez un nouveau **Nom**, puis cliquez sur **Saisissez** ou **Onglet** — si vous cliquez tout simplement sur Supprimer, votre nom ne sera pas enregistré.
6. Lorsque la cible est sélectionnée, la zone est entourée de lignes de tirets verts que vous pouvez redimensionner et déplacer. Faites glisser les coins à redimensionner ou faites glisser la zone cible pour la déplacer.

   - L’image sera alors chargée dans la visionneuse de zoom personnalisée par défaut. Assurez-vous que le paramètre prédéfini de la visionneuse prend en charge les cibles de zoom. En règle générale, tous les paramètres prédéfinis standard qui comportent le mot &quot;-guidés&quot; ont été conçus pour être utilisés avec les cibles de zoom. Pour utiliser les cibles, passez la souris sur la miniature de la cible (ou icône de zone réactive) pour afficher le libellé, puis cliquez dessus pour afficher le zoom de la visionneuse sur cette fonction.
   - Comme toutes les autres tâches que vous effectuez dans Dynamic Media Classic, vous devez publier pour que vos cibles de zoom soient actives sur le Web. Si vous utilisez déjà une visionneuse qui prend en charge les cibles, elles s’affichent immédiatement (une fois le cache effacé). Cependant, si vous n’utilisez pas de visionneuse avec la fonction Cible de zoom, elles restent masquées.

      ![image](assets/crop-adjusted-zoom-targets/zoom-target-green-box.jpg)

7. En outre, si vous devez supprimer une cible, sélectionnez-la en cliquant sur sa miniature, puis appuyez sur le bouton **Supprimer la cible** ou appuyez sur la touche DELETE de votre clavier.
8. Continuez à cliquer sur pour ajouter de nouvelles cibles, renommer et/ou redimensionner après l’ajout.
9. Une fois que vous avez terminé, cliquez sur le bouton **Enregistrer**, puis sur **Aperçu**.

### Définition de la taille d’affichage dans le paramètre prédéfini de la visionneuse de zoom

Parlons un instant d&#39;où vient la taille des cibles de zoom. Dans le paramètre prédéfini de visionneuse de votre visionneuse de zoom, il existe un paramètre appelé taille d’affichage. La taille d’affichage correspond à la taille de votre image de zoom dans la visionneuse. Elle diffère de la taille de l’étape, qui correspond à la taille totale de la visionneuse, y compris les composants de l’interface utilisateur et l’illustration.

Lorsque vous créez une cible, elle dérive sa taille et ses proportions de la taille de l’affichage. Si, par exemple, la taille de l’affichage est de 200 x 200, vous ne pourrez créer que des cibles carrées, avec une zone de zoom maximale de 200 pixels. Vos cibles peuvent dépasser 200 pixels, mais toujours carrées. Mais cela signifie aussi que l’image à l’intérieur de votre visionneuse de zoom ne fait que 200 pixels — la taille de la cible de zoom a une relation directe avec la taille de votre visionneuse. Vous devez donc d’abord décider de la conception de votre visionneuse avant de définir des cibles.

Cependant, par défaut, la taille de la vue est vide (définie sur 0 x 0), car la taille de l’image de la vue principale est dynamique et est automatiquement dérivée en fonction de la taille de la scène. Le problème est que si vous ne définissez pas explicitement une taille d’affichage dans votre paramètre prédéfini, l’outil Cible de zoom ne saura pas quelle taille créer les cibles.

Lorsque vous chargez l’outil Cible de zoom, la taille d’affichage s’affiche en regard du nom du paramètre prédéfini. Comparez la taille d’affichage entre le paramètre prédéfini assisté Zoom1 intégré et le paramètre prédéfini ZT_AUTHORING personnalisé.

![image](assets/crop-adjusted-zoom-targets/view-size.jpg)

Vous pouvez voir que le paramètre prédéfini intégré a une taille de 900 x 550, ce qui signifie que la cible ne peut jamais devenir plus petite que cette taille plutôt grande. C’est probablement trop grand — si vous avez une image de 2 000 pixels, vous ne pouvez afficher qu’une fonction d’au moins 900 pixels. L’utilisateur peut effectuer un zoom supplémentaire manuellement, mais vous ne pouvez pas le rapprocher. La définition d’une taille d’affichage de 350 x 350 permet aux cibles d’effectuer un zoom assez rapproché ou d’être redimensionnées plus grand. Cependant, si vous souhaitez une image de zoom plus grande dans la visionneuse, vous devez créer un nouveau paramètre prédéfini, car le vôtre est verrouillé à 350 pixels.

### Création ou modification d’un paramètre prédéfini de visionneuse prenant en charge les cibles de zoom

Pour définir la taille de l’affichage, créez ou modifiez un paramètre prédéfini de visionneuse qui prend en charge les cibles de zoom.

1. Dans le paramètre prédéfini de la visionneuse, sélectionnez l’option **Paramètres de zoom** .
2. Définissez une Largeur et une Hauteur.
3. Enregistrez le paramètre prédéfini, puis fermez-le. Si vous souhaitez utiliser ce paramètre prédéfini sur votre site actif, vous devrez également le publier ultérieurement.
4. Accédez à l’outil Cible de zoom et sélectionnez le paramètre prédéfini que vous avez modifié en bas à gauche. La nouvelle taille d’affichage apparaît immédiatement dans vos cibles.
