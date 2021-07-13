---
title: Présentation vidéo
description: Dynamic Media Classic est fourni avec la conversion automatique de la vidéo lors du chargement, la diffusion en continu de la vidéo sur les ordinateurs de bureau et les appareils mobiles, ainsi que des visionneuses de vidéos adaptatives optimisées pour la lecture en fonction de l’appareil et de la bande passante. En savoir plus sur la vidéo dans Dynamic Media Classic et obtenir une introduction sur les concepts et la terminologie vidéo. Apprenez ensuite comment charger et coder des vidéos, choisir des paramètres vidéo prédéfinis pour les charger, ajouter ou modifier un paramètre prédéfini vidéo, prévisualiser des vidéos dans une visionneuse, déployer des vidéos sur des sites web et mobiles, ajouter des sous-titres et des marqueurs de chapitre à la vidéo et publier des visionneuses de vidéos sur les utilisateurs de bureau et de mobiles.
sub-product: dynamic-media
feature: Dynamic Media Classic, Profils vidéo, Paramètres prédéfinis de la visionneuse
doc-type: tutorial
topics: development, authoring, configuring, videos, video-profiles
audience: all
activity: use
topic: Gestion de contenu
role: User
level: Beginner
source-git-commit: b0bca57676813bd353213b4808f99c463272de85
workflow-type: tm+mt
source-wordcount: '6231'
ht-degree: 1%

---


# Présentation vidéo {#video-overview}

Dynamic Media Classic est fourni avec la conversion automatique de la vidéo lors du chargement, la diffusion en continu de la vidéo sur les ordinateurs de bureau et les appareils mobiles, ainsi que des visionneuses de vidéos adaptatives optimisées pour la lecture en fonction de l’appareil et de la bande passante. Une des choses les plus importantes dans la vidéo est que le workflow est simple — il est conçu pour que n&#39;importe qui puisse l&#39;utiliser, même s&#39;il ne connaît pas très bien la technologie vidéo.

À la fin de cette section du tutoriel, vous saurez comment :

- Chargement et codage (transcodage) de vidéo dans différents formats et tailles
- Sélection parmi les paramètres vidéo prédéfinis disponibles pour le téléchargement
- Ajout ou modification d’un paramètre prédéfini de codage vidéo
- Prévisualisation de vidéos dans une visionneuse de vidéos
- Déploiement d’une vidéo sur des sites web et mobiles
- Ajout de sous-titres et de marqueurs de chapitre à la vidéo
- Personnalisation et publication de visionneuses de vidéos pour les utilisateurs de bureau et de mobile

>[!NOTE]
>
>Toutes les URL de ce chapitre sont proposées à titre d’illustration uniquement ; ce ne sont pas des liens en direct.

## Présentation de la vidéo Dynamic Media Classic

Commençons par mieux comprendre les possibilités de vidéo avec Dynamic Media Classic.

### Fonctionnalités et fonctionnalités

La plateforme vidéo Dynamic Media Classic offre toutes les parties de la solution vidéo : chargement, conversion et gestion des vidéos ; la possibilité d’ajouter des sous-titres et des marqueurs de chapitre à une vidéo ; et la possibilité d’utiliser des paramètres prédéfinis pour une lecture facile.

Il facilite la publication de vidéos adaptatives de haute qualité pour la diffusion en continu sur plusieurs écrans, y compris les appareils mobiles de bureau, iOS, Android, Blackberry et Windows. Une visionneuse de vidéos adaptative regroupe les versions d’une même vidéo codées dans des débits et des formats différents, par exemple 400 kbit/s, 800 kbit/s et 1 000 kbit/s. Le poste de travail ou l’appareil mobile détecte la bande passante disponible.

En outre, la qualité vidéo est automatiquement activée dynamiquement si les conditions réseau changent sur le bureau ou sur l’appareil mobile. En outre, si un client passe en mode plein écran sur un bureau, la visionneuse de vidéos adaptative répond en utilisant une meilleure résolution, améliorant ainsi l’expérience de visionnage du client. L’utilisation de visionneuses de vidéos adaptatives vous permet de lire le mieux possible la vidéo Dynamic Media Classic sur plusieurs écrans et appareils.

### Gestion des vidéos

L’utilisation de vidéos peut s’avérer plus complexe que l’utilisation d’images numériques. Avec la vidéo, vous traitez de nombreux formats et normes et vous savez si votre audience sera en mesure de lire vos clips. Dynamic Media Classic facilite l’utilisation de vidéos, fournissant de nombreux outils puissants &quot;en arrière-plan&quot;, mais en éliminant la complexité liée à l’utilisation des vidéos.

Dynamic Media Classic reconnaît et peut fonctionner avec de nombreux formats source disponibles. Cependant, la lecture de la vidéo n&#39;est qu&#39;une partie de l&#39;effort — vous devez également convertir la vidéo en un format adapté au web. Dynamic Media Classic s’en charge en vous permettant de convertir une vidéo en vidéo H.264.

La conversion de la vidéo par vous-même peut s’avérer très compliquée à l’aide des nombreux outils professionnels et passionnés disponibles. Dynamic Media Classic permet de rester simple en offrant des paramètres prédéfinis simples, optimisés pour différents paramètres de qualité. Cependant, si vous souhaitez quelque chose de plus personnalisé, vous pouvez également créer vos propres paramètres prédéfinis.

Si vous disposez de nombreuses vidéos, vous apprécierez la possibilité de gérer toutes vos ressources avec vos images et autres médias dans Dynamic Media Classic. Vous pouvez organiser, cataloguer et rechercher vos ressources, y compris les ressources vidéo, avec une prise en charge XMP métadonnées performante.

### Lecture vidéo

Tout comme le problème lié à la conversion d’une vidéo pour la rendre conviviale et accessible sur le Web, il s’agit du problème lié à l’implémentation et au déploiement d’une vidéo sur votre site. Si vous choisissez d’acheter un lecteur ou de créer le vôtre, ce qui le rend compatible avec divers appareils et écrans, puis si vous conservez vos lecteurs, il peut s’agir d’une occupation à temps plein.

Encore une fois, l’approche de Dynamic Media Classic vous permet de choisir le paramètre prédéfini et la visionneuse qui correspondent à vos besoins. Vous disposez de nombreux choix de visionneuses et d’une bibliothèque de nombreux paramètres prédéfinis disponibles.

Vous pouvez diffuser facilement des vidéos sur le Web et sur les appareils mobiles, car Dynamic Media Classic prend en charge la vidéo HTML5, ce qui signifie que vous pouvez cibler les utilisateurs exécutant différents navigateurs, ainsi que les utilisateurs de la plateforme Android et iOS. La vidéo en flux continu permet une lecture fluide d’un contenu plus long ou haute définition, tandis que la vidéo HTML5 progressive comporte des paramètres prédéfinis optimisés pour le petit écran.

Les paramètres prédéfinis de visionneuse pour la vidéo sont partiellement configurables en fonction du type de visionneuse.

Comme toutes les visionneuses, l’intégration s’effectue via une seule URL Dynamic Media Classic par visionneuse ou vidéo.

>[!NOTE]
>
>En règle générale, utilisez les visionneuses de vidéos HTML5 Dynamic Media Classic. Les paramètres prédéfinis utilisés dans les visionneuses de vidéos HTML5 sont des lecteurs vidéo fiables. En combinant dans un seul lecteur la possibilité de concevoir les composants de lecture à l’aide de code HTML5 et CSS, d’incorporer la lecture et d’utiliser la diffusion en continu adaptative et progressive selon les fonctionnalités du navigateur, vous étendez la portée de votre contenu multimédia enrichi à l’ordinateur, à la tablette et aux utilisateurs mobiles, et garantissez une expérience vidéo rationalisée.

Dernière remarque au sujet de la vidéo Dynamic Media Classic qui peut s’appliquer à certains clients : la conversion automatique, la diffusion en continu ou les paramètres vidéo prédéfinis ne sont pas activés pour toutes les entreprises pour leur compte. Si, pour une raison quelconque, vous ne pouvez pas accéder aux URL pour la diffusion vidéo en continu, c’est peut-être la raison. Vous pourrez toujours charger et publier des vidéos téléchargées progressivement et accéder à toutes les visionneuses de vidéos. Cependant, pour profiter de toutes les fonctionnalités vidéo de Dynamic Media Classic, vous souhaiterez contacter votre gestionnaire de compte ou votre responsable commercial pour que ces fonctionnalités soient activées.

En savoir plus sur [Vidéo dans Dynamic Media Classic](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/quick-start-video.html).

## Vidéo 101

### Concepts vidéo de base et terminologie

Avant de commencer, discutons de quelques termes que vous devriez connaître pour pouvoir utiliser la vidéo. Ces concepts ne sont pas spécifiques à Dynamic Media Classic, et si vous gérez la vidéo pour un site web professionnel, vous feriez bien d’obtenir une formation supplémentaire sur le sujet. Nous vous recommandons quelques ressources à la fin de cette section.

- **Encodage/transcodage.** Le codage est le processus d’application de la compression vidéo pour convertir des données vidéo brutes non compressées dans un format qui facilite leur utilisation. Le transcodage, bien que similaire, fait référence à la conversion d’une méthode d’encodage à une autre.

   - Les fichiers vidéo Principal créés avec un logiciel d’édition vidéo sont souvent trop volumineux et ne sont pas au format approprié pour une diffusion vers des destinations en ligne. Elles sont généralement codées pour une lecture rapide sur l’ordinateur de bureau et pour modification, mais pas pour une diffusion sur le web.
   - Pour convertir une vidéo numérique au format et aux spécifications appropriés pour la lecture sur différents écrans, les fichiers vidéo sont transcodés dans une taille de fichier plus petite et efficace, optimale pour la diffusion sur le web et sur les appareils mobiles.

- **Compression vidéo.** La réduction de la quantité de données utilisées pour représenter les images vidéo numériques est une combinaison de compression de l’image spatiale et de compensation du mouvement temporel.

   - La plupart des techniques de compression sont perdues, ce qui signifie qu’elles rejettent les données afin d’obtenir une taille plus petite.
   - Par exemple, la vidéo DV est compressée relativement peu et permet de modifier facilement le contenu source, mais elle est beaucoup trop volumineuse pour être utilisée sur le web ou même placée sur un DVD.

- **Formats de fichier.** Le format est un conteneur, similaire à un fichier ZIP, qui détermine la manière dont les fichiers sont organisés dans le fichier vidéo, mais généralement pas la manière dont ils sont codés.

   - Les formats de fichiers courants pour la vidéo source incluent Windows Media (WMV), QuickTime (MOV), Microsoft AVI et MPEG, entre autres. Les formats publiés par Dynamic Media Classic sont MP4.
   - Un fichier vidéo contient généralement plusieurs pistes — une piste vidéo (sans audio) et une ou plusieurs pistes audio (sans vidéo) — interconnectées et synchronisées.
   - Le format de fichier vidéo détermine la manière dont ces différentes pistes de données et métadonnées sont organisées.

- **Codec.** Un codec vidéo décrit l’algorithme par lequel une vidéo est codée à l’aide de la compression. L’audio est également codé au moyen d’un codec audio.

   - Les codecs minimisent la quantité d’informations requises pour lire une vidéo. Au lieu d’informations sur chaque image, seules les informations sur les différences entre une image et la suivante sont stockées.
   - Comme la plupart des vidéos changent peu d’une image à l’autre, les codecs permettent des taux de compression élevés, ce qui se traduit par des tailles de fichiers plus petites.
   - Un lecteur vidéo décode la vidéo en fonction de son codec, puis affiche une série d’images, ou images, à l’écran.
   - Les codecs vidéo courants comprennent H.264, On2 VP6 et H.263.

![image](assets/video-overview/bird-video.png)

- **Résolution.** Hauteur et largeur de la vidéo, en pixels.

   - La taille de la vidéo source est déterminée par l’appareil photo et la sortie de votre logiciel de modification. Une caméra HD crée généralement une vidéo haute résolution 1920 x 1080. Toutefois, pour une lecture fluide sur le web, vous la sous-échantillonnez (redimensionnez) à une résolution plus petite, comme 1280 x 720, 640 x 480, ou plus petite.
   - La résolution a un impact direct sur la taille du fichier ainsi que sur la bande passante requise pour lire cette vidéo.

- **Afficher les proportions.** Rapport largeur/hauteur d’une vidéo. Lorsque les proportions de la vidéo ne correspondent pas aux proportions du lecteur, des &quot;barres noires&quot; peuvent s’afficher ou un espace vide s’affiche. Deux proportions courantes sont utilisées pour afficher la vidéo :

   - 4:3 (1.33:1). Utilisé pour presque tous les contenus télévisés à définition standard.
   - 16:9 (1.78:1). Utilisé pour presque tous les contenus télévisés haute définition (HDTV) et les films grand écran.

- **Débit/débit/débit de données.** La quantité de données codées pour constituer une seconde de lecture vidéo (en kilobits par seconde).

   - En règle générale, plus le débit est faible, plus il est souhaitable pour le web, car il peut être téléchargé plus rapidement. Cependant, cela peut également signifier que la qualité est faible en raison de la perte de compression.
   - Un bon codec doit équilibrer un débit faible avec une bonne qualité.

- **Taux d’images (images par seconde ou images).** Nombre d’images, ou images fixes, pour chaque seconde de vidéo. En règle générale, la télévision nord-américaine (NTSC) est diffusée en 29,97 i/s; La télévision européenne et asiatique (PAL) est diffusée en 25 i/s ; et le film (analogique et numérique) sont généralement en 24 (23.976) i/s.

   - Pour rendre les choses plus déroutantes, il existe également des cadres progressifs et entrelacés. Chaque image progressive contient un cadre d’image entier, tandis que les images entrelacées contiennent une rangée de pixels sur deux dans un cadre d’image. Les images sont ensuite lues très rapidement et semblent se mélanger. Le film utilise une méthode de numérisation progressive, tandis que la vidéo numérique est généralement entrelacée.
   - En règle générale, peu importe que le métrage source soit entrelacé ou non : Dynamic Media Classic conserve la méthode d’analyse dans la vidéo convertie.
   - Diffusion en flux continu/progressive. La diffusion vidéo en continu est l’envoi de médias dans un flux continu qui peut être lu à mesure qu’il arrive, tandis que la vidéo téléchargée progressivement est téléchargée comme tout autre fichier à partir d’un serveur et mise en cache localement dans votre navigateur.

Avec un peu de chance, cette introduction vous aidera à comprendre les différentes options impliquées dans l’utilisation de la vidéo Dynamic Media Classic.

## Workflow vidéo

Lorsque vous utilisez une vidéo dans Dynamic Media Classic, vous suivez un processus de base similaire à celui de l’utilisation d’images.

![image](assets/video-overview/video-overview-2.png)

1. Commencez par charger des fichiers vidéo dans Dynamic Media Classic. Pour ce faire, ouvrez le **menu Outils** au bas du panneau de l’extension Dynamic Media Classic, puis sélectionnez **Télécharger vers Dynamic Media Classic > Fichiers dans le nom de dossier** ou **Télécharger vers Dynamic Media Classic > Dossiers dans le nom de dossier**. &quot;Nom du dossier&quot; correspond au dossier que vous parcourez actuellement avec l’extension. Les fichiers vidéo peuvent être volumineux. Nous vous recommandons donc d’utiliser le protocole FTP pour télécharger des fichiers volumineux. Dans le cadre du téléchargement, sélectionnez un ou plusieurs paramètres vidéo prédéfinis pour le codage de vos vidéos. La vidéo peut être transcodée au format MP4 lors du téléchargement. Pour plus d’informations sur l’utilisation et la création de paramètres prédéfinis de codage, voir la rubrique Paramètres vidéo prédéfinis ci-dessous. Découvrez [Téléchargement et codage des vidéos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html).
2. Sélectionnez ou modifiez un paramètre prédéfini de visionneuse de vidéos et prévisualisez votre vidéo. Vous pouvez choisir un paramètre prédéfini de visionneuse prédéfini ou personnaliser le vôtre. Si vous ciblez des utilisateurs mobiles, vous n’avez rien à faire ici, car les plateformes mobiles ne nécessitent pas de visionneuse ni de paramètre prédéfini. En savoir plus sur [la prévisualisation de vidéos dans une visionneuse de vidéos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html) et [l’ajout ou la modification d’un paramètre prédéfini de visionneuse de vidéos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/previewing-videos-video-viewer.html#adding-or-editing-a-video-viewer-preset).
3. Exécutez une publication de vidéo, obtenez l’URL et intégrez. La principale différence entre cette étape du workflow vidéo et celle de l’image réside dans le fait que vous exécuterez une publication vidéo spéciale au lieu de la publication de diffusion d’images standard (ou peut-être aussi). L’intégration de la visionneuse de vidéos sur le bureau fonctionne exactement comme l’intégration de la visionneuse d’images. Toutefois, pour les appareils mobiles, c’est encore plus simple — tout ce dont vous avez besoin est l’URL de la vidéo elle-même.

### A propos du transcodage

Le transcodage a été défini précédemment comme le processus de conversion d’une méthode d’encodage à une autre. Dans le cas de Dynamic Media Classic, il s’agit du processus de conversion de votre vidéo source au format MP4 à partir de son format actuel. Cette opération est nécessaire avant que votre vidéo n’apparaisse dans le navigateur de bureau ou sur un appareil mobile.

Dynamic Media Classic peut gérer tout le transcodage pour vous, ce qui est très bénéfique. Vous pouvez transcoder la vidéo vous-même et télécharger les fichiers déjà convertis au format MP4, mais il peut s’agir d’un processus complexe qui nécessite des logiciels sophistiqués. A moins de savoir ce que vous faites, vous n&#39;obtiendrez généralement pas de bons résultats lors de votre première tentative.

Dynamic Media Classic ne convertit pas seulement les fichiers pour vous, mais il facilite également la tâche en fournissant des paramètres prédéfinis faciles à utiliser. Vous n&#39;avez vraiment pas besoin d&#39;en savoir plus sur le côté technique de ce processus — tout ce que vous devez savoir c&#39;est à peu près la ou les taille(s) finale(s) que vous souhaitez obtenir du système et une idée de la bande passante de vos utilisateurs finaux.

Bien que les paramètres prédéfinis prédéfinis soient pratiques et répondent à la plupart des besoins, il arrive parfois que vous souhaitiez quelque chose de plus personnalisé. Dans ce cas, vous pouvez créer votre propre paramètre prédéfini de codage. Dans Dynamic Media Classic, un paramètre prédéfini de codage est appelé paramètre prédéfini vidéo. Cela sera expliqué plus loin dans ce chapitre.

### A propos de la diffusion en continu

Une autre fonctionnalité majeure à noter est la diffusion en continu de vidéos, une fonctionnalité standard de la plateforme vidéo Dynamic Media Classic. Les médias en flux continu sont constamment reçus et présentés à un utilisateur final lors de leur diffusion. C&#39;est important et souhaitable pour plusieurs raisons.

La diffusion en flux continu nécessite généralement moins de bande passante que le téléchargement progressif, car seule la partie de la vidéo visionnée est réellement diffusée. Le serveur de diffusion vidéo en continu Dynamic Media Classic et les visionneuses utilisent la détection automatique de bande passante pour diffuser le meilleur flux possible pour la connexion Internet d’un utilisateur.

Avec la diffusion en continu, la lecture de la vidéo commence plus tôt qu’avec d’autres méthodes. Cela optimise également l’utilisation des ressources réseau, car seules les parties de la vidéo visionnées sont envoyées au client.

L’autre méthode de diffusion est le téléchargement progressif. Par rapport à la diffusion vidéo en continu, le téléchargement progressif n’offre qu’un seul avantage : vous n’avez pas besoin d’un serveur de diffusion pour diffuser la vidéo. Et c&#39;est bien sûr là que Dynamic Media Classic entre en jeu — Dynamic Media Classic a un serveur de diffusion intégré à la plate-forme, donc vous n&#39;avez pas besoin des tracas ou des coûts supplémentaires liés à la maintenance de ce matériel dédié.

La vidéo de téléchargement progressif peut être diffusée à partir de n’importe quel serveur web normal. Bien que cela puisse être pratique et potentiellement rentable, gardez à l’esprit que les téléchargements progressifs ont des capacités de recherche et de navigation limitées et que les utilisateurs peuvent accéder à votre contenu et le réutiliser. Dans certains cas, comme la lecture derrière des pare-feu réseau très stricts, la diffusion en continu peut être bloquée ; dans ce cas, la restauration de la diffusion progressive peut être souhaitable.

Le téléchargement progressif est un bon choix pour les amateurs ou les sites web à faible trafic ; si le contenu est mis en cache sur l’ordinateur de l’utilisateur, cela ne les dérange pas ; s’ils ne doivent diffuser que des vidéos de plus courte durée (moins de 10 minutes) ; ou si leurs visiteurs ne peuvent pas recevoir de vidéo en continu pour une raison quelconque.

Vous devrez diffuser votre vidéo en continu si vous avez besoin de fonctionnalités avancées et de contrôle sur la diffusion vidéo, et/ou si vous devez afficher la vidéo à un public plus large (plusieurs centaines de visionneuses simultanées, par exemple), suivre et générer des rapports sur l’utilisation ou les statistiques d’affichage, ou si vous souhaitez offrir à vos visionneuses la meilleure expérience de lecture interactive.

Enfin, si vous vous souciez de la protection de vos médias pour des questions de propriété intellectuelle ou de gestion des droits, la diffusion en continu permet une diffusion plus sécurisée des vidéos, car les médias ne sont pas enregistrés dans le cache du client lors de la diffusion en continu.

## Paramètres vidéo prédéfinis

Lorsque vous téléchargez une vidéo, vous pouvez choisir parmi un ou plusieurs paramètres prédéfinis qui contiennent les paramètres de conversion de la vidéo originale en un format adapté au web par le biais du codage. Les paramètres vidéo prédéfinis sont disponibles en deux versions : les paramètres vidéo prédéfinis adaptatifs et les paramètres prédéfinis de codage uniques.

Voir [Paramètres vidéo prédéfinis disponibles](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/application-setup.html#video-presets-for-encoding-video-files).

Les paramètres prédéfinis de vidéo adaptative sont activés par défaut, ce qui signifie qu’ils sont disponibles pour le codage. Si vous souhaitez utiliser un paramètre prédéfini de codage unique, votre administrateur doit l’activer pour qu’il apparaisse dans la liste des paramètres vidéo prédéfinis.

Découvrez comment [activer ou désactiver les paramètres vidéo prédéfinis](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#activating-or-deactivating-video-encoding-presets).

Vous pouvez choisir l’un des nombreux paramètres prédéfinis prédéfinis fournis avec Dynamic Media Classic ou créer les vôtres. toutefois, aucun paramètre prédéfini n’est sélectionné par défaut pour le chargement. En d’autres termes, **si vous ne sélectionnez pas de paramètre vidéo prédéfini au moment du téléchargement, votre vidéo ne sera pas convertie et peut être impossible à publier**. Cependant, vous pouvez convertir la vidéo vous-même hors ligne et la télécharger et la publier simplement correctement. Les paramètres vidéo prédéfinis ne sont requis que si vous souhaitez que Dynamic Media Classic effectue la conversion à votre place.

Au moment du téléchargement, vous sélectionnez un paramètre vidéo prédéfini en sélectionnant **Options vidéo** dans le panneau Options de tâche. Vous pouvez ensuite choisir de coder pour Ordinateur, Mobile ou Tablette.

- L’ordinateur est destiné à l’utilisation de bureau. Vous y trouverez généralement des paramètres prédéfinis plus volumineux (tels que HD) qui consomment plus de bande passante.
- Mobile et tablette créent une vidéo MP4 pour les appareils tels que les iPhone et les smartphones Android. La seule différence entre Mobile et Tablette est que les paramètres prédéfinis de la tablette ont généralement une bande passante plus élevée, car ils sont basés sur l’utilisation du Wi-Fi. Les paramètres prédéfinis mobiles sont optimisés pour une utilisation plus lente en 3G.

### Questions à vous poser avant de choisir un paramètre prédéfini

Lorsque vous choisissez un paramètre prédéfini, vous devez connaître votre audience ainsi que vos images sources. Que savez-vous de votre client ? Comment regardent-ils la vidéo — avec un écran d&#39;ordinateur ou un appareil mobile ?

Quelle est la résolution de votre vidéo ? Si vous choisissez un paramètre prédéfini plus grand que l’original, vous pouvez obtenir une vidéo floue/pixelisée. Cela vous convient si votre vidéo est plus grande que le paramètre prédéfini, mais ne choisissez pas un paramètre prédéfini plus grand que votre vidéo source.

Quel est son rapport d’aspect ? Si des barres noires s’affichent autour de la vidéo convertie, les proportions sont incorrectes. Dynamic Media Classic ne peut pas détecter automatiquement ces paramètres, car il doit d’abord examiner le fichier avant de le télécharger.

### Ventilation des options vidéo

Les paramètres vidéo prédéfinis déterminent le codage de votre vidéo en spécifiant ces paramètres. Si vous ne connaissez pas ces termes, consultez la rubrique Concepts de base de la vidéo et terminologie, ci-dessus.

![image](assets/video-overview/video-overview-3.jpg)

- **Format.** Généralement 4:3 standard ou 16:9 grand écran.
- **Taille.** Il s’agit de la même résolution que celle de l’affichage, exprimée en pixels. Ceci est lié aux proportions. Avec un rapport de 16:9, une vidéo fera 432 x 240 pixels, tandis qu’avec un rapport de 4:3, elle fera 320 x 240 pixels.
- **FPS.** Les débits d’images standard sont de 30, 25 ou 24 images par seconde (i/s), selon la norme vidéo : NTSC, PAL ou Film. Ce paramètre n’a pas d’importance, car Dynamic Media Classic utilise toujours la même fréquence d’image que la vidéo source.
- **Format.** Il s’agit de MP4.
- **Bande passante.** Il s’agit de la vitesse de connexion souhaitée de votre utilisateur ciblé. Ont-ils une connexion internet rapide ou lente ? Utilisent-ils généralement des ordinateurs de bureau ou des appareils mobiles ? Cela est également lié à la résolution (taille), car plus la vidéo est grande, plus la bande passante nécessaire est importante.

### Détermination du débit de données ou du débit binaire pour votre vidéo

Le calcul du débit pour votre vidéo est l’un des facteurs les moins bien compris pour diffuser de la vidéo sur le web, mais potentiellement le plus important, car il impacte directement l’expérience de l’utilisateur. Si vous définissez un débit trop élevé, la qualité de la vidéo sera élevée, mais les performances seront médiocres. Les utilisateurs ayant des connexions Internet plus lentes seront obligés d’attendre pendant que la vidéo s’interrompt constamment au fur et à mesure de sa lecture. Cependant, si vous la définissez sur une valeur trop basse, la qualité en pâtira. Dans le paramètre vidéo prédéfini, Dynamic Media Classic propose une gamme de données en fonction de la bande passante de votre cible. C&#39;est un bon point de départ.

Cependant, si vous voulez le comprendre vous-même, vous aurez besoin d&#39;une calculatrice de débit. Il s’agit d’un outil couramment utilisé par les professionnels de la vidéo et les passionnés pour estimer la quantité de données qui s’adaptera à un flux ou à un élément multimédia donné (un DVD, par exemple).

## Création d’un paramètre vidéo prédéfini personnalisé

Il peut arriver que vous ayez besoin d’un paramètre vidéo prédéfini spécial qui ne correspond pas aux paramètres des paramètres prédéfinis de codage vidéo intégrés. Cela peut se produire si vous disposez d’une vidéo personnalisée d’une taille spécifique, telle qu’une vidéo créée à partir d’un logiciel d’animation 3D ou qui a été recadrée à partir de sa taille d’origine. Vous souhaitez peut-être tester différents paramètres de bande passante pour diffuser des vidéos de qualité supérieure ou inférieure. Quel que soit le cas, vous devrez créer un paramètre prédéfini vidéo de codage unique personnalisé.

### Workflow vidéo prédéfini

1. Les paramètres vidéo prédéfinis sont situés sous **Configuration > Configuration de l’application > Paramètres vidéo prédéfinis**. Vous trouverez ici la liste de tous les paramètres prédéfinis de codage disponibles pour votre entreprise.

   - Chaque compte vidéo en continu comporte des dizaines de paramètres prédéfinis prêts à l’emploi. Si vous créez vos propres paramètres prédéfinis personnalisés, vous les verrez également ici.
   - Vous pouvez filtrer par type à l’aide du menu déroulant. Les paramètres prédéfinis sont divisés entre Ordinateur, Mobile et Tablette.
      ![image](assets/video-overview/video-overview-4.jpg)

2. La colonne Principal vous permet de choisir d’afficher tous les paramètres prédéfinis lors du téléchargement ou uniquement ceux de votre choix. Si vous vous trouvez aux États-Unis, vous pouvez décocher les paramètres prédéfinis PAL européens et, si vous vous trouvez au Royaume-Uni/dans la région EMEA, décocher les paramètres prédéfinis NTSC.
3. Cliquez sur le bouton **Ajouter** pour créer un paramètre prédéfini personnalisé. Le panneau Ajouter un paramètre vidéo prédéfini s’affiche alors. Le processus ici est similaire à la création d’un paramètre d’image prédéfini.
4. Tout d’abord, attribuez-lui un **nom de paramètre prédéfini** qui apparaîtra dans la liste des paramètres prédéfinis. Dans l’exemple ci-dessus, ce paramètre prédéfini est destiné aux vidéos de tutoriels de capture d’écran.
5. **Description** est facultatif, mais il fournit à vos utilisateurs une info-bulle qui décrit l’objectif de ce paramètre prédéfini.
6. Le **suffixe de fichier de codage** sera ajouté à la fin du nom de la vidéo que vous créez ici. N’oubliez pas que vous disposez d’une vidéo par Principal ainsi que de cette vidéo codée, qui est une dérivée du gabarit, et qu’aucune des deux ressources de Dynamic Media Classic ne peut avoir le même ID de ressource.
7. **Périphériques** de lecture où vous choisissez le format de fichier vidéo souhaité (ordinateur, mobile ou tablette). N’oubliez pas que Mobile et Tablette produisent le même format MP4. Dynamic Media Classic doit simplement savoir dans quelle catégorie placer le paramètre prédéfini ; cependant, la différence théorique est que les paramètres prédéfinis de tablette sont généralement pour une connexion Internet plus rapide, car tous prennent en charge le Wi-Fi.
8. **Le** taux de données cible est une chose que vous devrez comprendre vous-même, mais vous pouvez voir une plage suggérée sur l’image ci-dessous. Vous pouvez également faire glisser le curseur vers la bande passante cible approximative. Pour obtenir un chiffre plus précis, utilisez un calculateur de débit. Il y a un peu d&#39;essais et d&#39;erreurs.

   ![image](assets/video-overview/video-overview-5.jpg)

9. Définissez les **proportions** de votre fichier source. Ce paramètre est directement lié à la taille, située en dessous. Si vous choisissez _Personnalisé_, vous devrez saisir manuellement la largeur et la hauteur.
10. Si vous choisissez un format, définissez une valeur pour **Taille de résolution** , et Dynamic Media Classic remplira automatiquement l’autre valeur. Toutefois, pour un rapport d’aspect personnalisé, renseignez les deux valeurs. Votre taille doit correspondre à votre débit de données. Si vous définissez un débit de données très faible et une grande taille, vous vous attendriez à une mauvaise qualité.
11. Cliquez sur **Enregistrer** pour enregistrer votre paramètre prédéfini. Contrairement à tous les autres paramètres prédéfinis, vous n’avez pas besoin d’effectuer une publication à ce stade, car les paramètres prédéfinis sont réservés uniquement au chargement de fichiers. Vous devrez ultérieurement publier les vidéos codées, mais les paramètres prédéfinis sont réservés à l’utilisation interne de Dynamic Media Classic.
12. Pour vérifier que votre paramètre vidéo prédéfini figure dans la liste de chargement, accédez à **Télécharger**.Sélectionnez **Options de tâche** et développez **Options vidéo**. Votre paramètre prédéfini sera répertorié dans la catégorie du périphérique de lecture que vous avez choisi (Ordinateur, Mobile ou Tablette).

En savoir plus sur [l’ajout ou la modification d’un paramètre vidéo prédéfini](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/uploading-encoding-videos.html#adding-or-editing-a-video-encoding-preset).

## Ajout de sous-titres à une vidéo

Dans certains cas, il peut s’avérer utile d’ajouter des sous-titres à votre vidéo, par exemple, lorsque vous devez fournir la vidéo aux visionneuses dans plusieurs langues, mais que vous ne souhaitez pas duper l’audio dans une autre langue ou enregistrer à nouveau la vidéo dans des langues distinctes. En outre, l’ajout de sous-titres permet une meilleure accessibilité pour les malentendants et l’utilisation de sous-titres codés. Dynamic Media Classic facilite l’ajout de sous-titres à vos vidéos.

Découvrez comment [Ajouter des sous-titres à la vidéo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-captions-video.html).

## Ajout de marqueurs de chapitre à votre vidéo

Pour les vidéos longues, vos visionneuses apprécieront probablement la facilité de navigation dans votre vidéo à l’aide de marqueurs de chapitre. Dynamic Media Classic permet d’ajouter facilement des marqueurs de chapitre à votre vidéo.

Découvrez comment [ajouter des marqueurs de chapitre à la vidéo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/adding-chapter-markers-video.html).

## Rubriques de mise en oeuvre vidéo

### Publier et copier l’URL

La dernière étape du processus Dynamic Media Classic consiste à publier votre contenu vidéo. Toutefois, la vidéo possède sa propre tâche de publication, appelée publication de serveur vidéo, qui se trouve sous Avancé.

![image](assets/video-overview/video-overview-6.jpg)

Découvrez comment [publier votre vidéo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#publishing-video).

Une fois que vous avez exécuté une publication de vidéo, vous pouvez obtenir une URL pour accéder à vos vidéos et à tous les paramètres prédéfinis de visionneuse Dynamic Media Classic standard dans un navigateur web. Cependant, si vous personnalisez ou créez votre propre paramètre prédéfini de visionneuse vidéo, vous devrez tout de même exécuter une publication de serveur d’images distincte.

- Découvrez comment [lier une URL à un site mobile ou à un site web](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#linking-a-video-url-to-a-mobile-site-or-a-website).
- Découvrez comment [incorporer la visionneuse de vidéos dans une page web](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#embedding-the-video-viewer-on-a-web-page).

Vous pouvez également déployer votre vidéo à l’aide d’un lecteur vidéo tiers ou personnalisé.

Découvrez comment [Déployer une vidéo à l’aide d’un lecteur vidéo tiers](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#deploying-video-using-a-third-party-video-player).

De plus, si vous souhaitez également utiliser les miniatures vidéo — l’image extraite de la vidéo — vous devrez également exécuter une publication Image Server. Cela est dû au fait que la miniature de la vidéo réside sur le serveur d’images, alors que la vidéo elle-même se trouve sur le serveur vidéo. Les miniatures vidéo peuvent être utilisées dans les résultats de recherche vidéo, dans les listes de lecture vidéo et comme &quot;cadre d’affiche&quot; initial qui apparaît dans la visionneuse avant la lecture de la vidéo.

En savoir plus sur [l’utilisation des miniatures vidéo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/video/deploying-video-websites-mobile-sites.html#working-with-video-thumbnails).

### Sélection et personnalisation d’un paramètre prédéfini de visionneuse

Le processus de sélection et de personnalisation d’un paramètre prédéfini de visionneuse est identique à celui des images. Vous pouvez soit créer un nouveau paramètre prédéfini, soit modifier un paramètre prédéfini existant et l’enregistrer sous un nouveau nom, apporter des modifications et exécuter une publication de diffusion d’images. Tous les paramètres prédéfinis de la visionneuse sont publiés sur le serveur d’images, pas seulement les paramètres prédéfinis des images. Vous devez donc exécuter une publication d’image pour afficher vos paramètres prédéfinis nouveaux ou modifiés.

>[!TIP]
>
>Exécutez une publication de diffusion d’images après la publication sur votre serveur vidéo pour publier toutes les images miniatures associées à vos vidéos.

## Optimisation du moteur de recherche vidéo

L’optimisation du moteur de recherche (SEO) est le processus d’amélioration de la visibilité d’un site web ou d’une page web dans les moteurs de recherche. Bien que les moteurs de recherche réussissent à rassembler des informations sur du contenu textuel, ils ne peuvent pas acquérir d’informations appropriées sur la vidéo sans leur fournir ces informations. L’optimisation du référencement des vidéos Dynamic Media Classic vous permet d’utiliser des métadonnées pour fournir aux moteurs de recherche des descriptions de vos vidéos. La fonction d’optimisation pour les moteurs de recherche vidéo vous permet de créer des plans de site vidéo et des flux Media RSS (mRSS).

- **Plan du site vidéo**. Informe Google exactement où et quel contenu vidéo se trouve sur un site. Par conséquent, les vidéos peuvent faire l’objet de recherches complètes sur Google. Par exemple, un plan de site vidéo peut spécifier la durée d’exécution et les catégories des vidéos.
- **flux mRSS**. Utilisé par les éditeurs de contenu pour alimenter les fichiers multimédia dans Yahoo! Recherche vidéo. Google prend en charge le protocole de flux Plan de site vidéo et Media RSS (mRSS) pour envoyer des informations aux moteurs de recherche.

Lorsque vous créez des plans de site vidéo et des flux mRSS, vous décidez des champs de métadonnées des fichiers vidéo à inclure. Ainsi, vous décrivez vos vidéos aux moteurs de recherche afin qu’ils puissent rediriger plus précisément le trafic vers les vidéos de votre site web.

Une fois le plan de site ou le flux créé, Dynamic Media Classic peut le publier automatiquement, le publier manuellement ou simplement générer un fichier que vous pourrez modifier ultérieurement. En outre, Dynamic Media Classic peut générer et publier automatiquement ce fichier chaque jour.

À la fin du processus, vous enverrez le fichier ou l’URL à votre moteur de recherche. Cette tâche est effectuée en dehors de Dynamic Media Classic ; cependant, nous en discuterons brièvement ci-dessous.

### Conditions requises pour les fichiers Sitemap/mRSS

Pour que Google et d&#39;autres moteurs de recherche ne rejettent pas vos fichiers, ils doivent être au format approprié et inclure certaines informations. Dynamic Media Classic génère un fichier correctement formaté. cependant, si les informations ne sont pas disponibles pour certaines de vos vidéos, elles ne seront pas incluses dans le fichier .

Les champs requis sont Page d’entrée (URL de la page qui diffuse la vidéo, et non URL de la vidéo elle-même), Titre et Description. Chaque vidéo doit comporter une entrée pour ces éléments, sinon elle ne sera pas incluse dans le fichier généré. Les champs facultatifs sont Balises et Catégorie.

Il existe deux autres champs obligatoires : URL du contenu, URL de la ressource vidéo elle-même et Miniature, une URL d’une miniature de la vidéo. Toutefois, Dynamic Media Classic remplira automatiquement ces valeurs pour vous.

Il est recommandé d’incorporer ces données dans vos vidéos avant de les transférer à l’aide XMP métadonnées ; Dynamic Media Classic les extraira lors du téléchargement. Vous pouvez utiliser une application telle qu’Adobe Bridge, qui est incluse avec toutes les applications Adobe Creative Cloud, pour renseigner les données dans des champs de métadonnées standard.

En suivant cette méthode, vous n’aurez pas à saisir manuellement ces données à l’aide de Dynamic Media Classic. Cependant, vous pouvez également utiliser des paramètres prédéfinis de métadonnées dans Dynamic Media Classic, afin de saisir rapidement les mêmes données à chaque fois.

Pour plus d’informations sur cette rubrique, voir [Affichage, ajout et exportation de métadonnées](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html).

![image](assets/video-overview/video-overview-7.jpg)

Une fois les métadonnées renseignées, vous pourrez les voir dans l’affichage des détails de cette ressource vidéo. Les mots-clés sont également présents, mais ils se trouvent sous l’onglet Mots-clés .

- En savoir plus sur [l’ajout de mots-clés](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/managing-assets/viewing-adding-exporting-metadata.html#add-or-edit-keywords).
- En savoir plus sur [SEO vidéo](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html).
- Découvrez les [paramètres pour l’optimisation du référencement des vidéos](https://docs.adobe.com/content/help/en/dynamic-media-classic/using/setup/video-seo-search-engine-optimization.html#choosing-video-seo-settings).

#### Configuration de l’optimisation du référencement vidéo

La configuration de l’optimisation du référencement des vidéos commence par le choix du type de format souhaité, de la méthode de génération et des champs de métadonnées à inclure dans le fichier.

1. Accédez à **Configuration > Configuration de l’application > Opérateur de référencement vidéo > Paramètres**.
2. Dans le menu **Mode de génération**, choisissez un format de fichier. La valeur par défaut est Désactivé. Pour l’activer, sélectionnez Plan de site vidéo, mRSS ou Les deux.
3. Choisissez de générer automatiquement ou manuellement. Pour plus de simplicité, nous vous recommandons de la définir sur **Mode automatique**. Si vous choisissez Automatique, définissez également l’option **Marquer pour publication**, sinon le ou les fichiers ne seront pas activés. Les fichiers Sitemap et RSS sont des types de document XML et doivent être publiés comme toute autre ressource. Utilisez l’un des modes manuels si vous n’avez pas toutes les informations à portée de main, ou si vous ne souhaitez générer qu’une seule fois.
4. Renseignez les balises de métadonnées qui seront utilisées dans les fichiers. Cette étape n’est pas facultative. Vous devez au minimum inclure les trois champs marqués d’un astérisque (\*) : **Page d’entrée** , **Titre** et **Description**. Pour utiliser vos métadonnées pour ces tâches, faites glisser les champs du panneau Métadonnées à droite dans un champ correspondant du formulaire. Dynamic Media Classic renseigne automatiquement le champ d’espace réservé avec les données réelles de chaque vidéo. Vous n’avez pas à utiliser de champs de métadonnées. Vous pouvez saisir du texte statique ici, mais ce même texte apparaîtra pour chaque vidéo.
5. Une fois que vous avez renseigné les informations dans les trois champs requis, Dynamic Media Classic activera les boutons **Enregistrer** et **Enregistrer et générer**. Cliquez sur l’un d’eux pour enregistrer vos paramètres. Utilisez **Enregistrer** si vous êtes en mode automatique et souhaitez que Dynamic Media Classic génère les fichiers ultérieurement. Utilisez **Enregistrer et générer** pour créer le fichier immédiatement.

### Test et publication du plan de site vidéo, du flux mRSS ou des deux fichiers

Les fichiers générés s’affichent dans le répertoire racine (base) de votre compte.

![image](assets/video-overview/video-overview-8.jpg)

Ces fichiers doivent être publiés, car l’outil d’optimisation pour les moteurs de recherche vidéo ne peut pas exécuter une publication seule. Tant qu’ils sont marqués pour publication, ils sont envoyés aux serveurs de publication lors de la prochaine exécution d’une publication.

Après la publication, vos fichiers seront disponibles à l’aide de ce format d’URL.

![image](assets/video-overview/video-url-format.png)

Exemple :

![image](assets/video-overview/video-format-example.png)

### Envoi à des moteurs de recherche

La dernière étape du processus consiste à envoyer vos fichiers et/ou URL aux moteurs de recherche. Dynamic Media Classic ne peut pas effectuer cette étape pour vous ; toutefois, en supposant que vous envoyiez l’URL et non le fichier XML proprement dit, votre flux doit être mis à jour la prochaine fois que votre fichier est généré et qu’une publication a lieu.

La méthode d’envoi à votre moteur de recherche varie, mais pour Google, vous utilisez les outils de webmaster Google. Une fois sur place, accédez à **Configuration du site > Plans de site** , puis cliquez sur le bouton **Envoyer un plan de site** . Ici, vous pouvez placer l’URL Dynamic Media Classic dans votre ou vos fichiers d’optimisation pour les moteurs de recherche.

### Rapport d’optimisation du référencement vidéo

Dynamic Media Classic fournit un rapport qui indique le nombre de vidéos qui ont été incluses avec succès dans les fichiers et, plus important encore, qui n’ont pas été incluses en raison d’erreurs. Pour accéder au rapport, accédez à **Configuration > Configuration de l’application > Optimisation du référencement vidéo > Rapport**.

![image](assets/video-overview/video-overview-9.jpg)

## Mise en oeuvre mobile pour la vidéo MP4

Dynamic Media Classic n’inclut pas les paramètres prédéfinis de visionneuse pour les appareils mobiles, car les visionneuses ne sont pas nécessaires pour lire la vidéo sur les appareils mobiles pris en charge. Tant que vous encodez au format H.264 MP4 (soit en effectuant une conversion lors du téléchargement, soit en pré-encodant sur votre bureau), les tablettes et smartphones pris en charge pourront lire vos vidéos sans avoir besoin de visionneuse. Cette fonctionnalité est prise en charge sur les appareils Android et iOS (iPhone et iPad).

La raison pour laquelle aucune visionneuse n’est requise est que les deux plateformes prennent en charge la version H.264 native. Vous pouvez incorporer la vidéo dans une page web HTML5 ou l’incorporer dans l’application elle-même ; les systèmes d’exploitation Android et iOS fourniront un contrôleur pour lire la vidéo.

Pour cette raison, Dynamic Media Classic ne vous donne pas d’URL d’accès à une visionneuse pour les appareils mobiles, mais vous donne plutôt une URL directement vers la vidéo. Dans la fenêtre Aperçu d’une vidéo MP4, il y a des liens pour Bureau et Mobile. L’URL mobile pointe vers la vidéo publiée.

Il est important de noter que l’URL répertorie le chemin d’accès complet à la vidéo, et pas seulement l’ID de ressource. Lorsque vous traitez des images, vous appelez l’image à l’aide de son identifiant de ressource, quelle que soit la structure de dossiers. Toutefois, pour la vidéo, vous devez également spécifier la structure de dossiers. Dans les URL ci-dessus, la vidéo est stockée dans le chemin d’accès :

![image](assets/video-overview/mobile-implement-1.png)

Cela peut également être exprimé sous la forme du nom de l’entreprise, du chemin du dossier ou du nom de la vidéo.

### Méthode #1 : Lecture du navigateur — Code HTML5

Pour incorporer votre vidéo MP4 dans une page web, utilisez la balise vidéo HTML5.

![image](assets/video-overview/browser-playback.png)

Cette méthode fonctionnera également pour le web de bureau, mais vous risquez de rencontrer des problèmes avec la prise en charge du navigateur ; tous les navigateurs web de bureau ne prennent pas en charge la vidéo H.264 en mode natif, y compris Firefox.

### Méthode #2 : Lecture de l’application sur iOS - Structure du lecteur multimédia

Vous pouvez également incorporer la vidéo MP4 Dynamic Media Classic dans le code de votre application mobile. Voici un exemple générique pour iOS utilisant la structure du lecteur multimédia fournie à titre d’illustration uniquement :

![image](assets/video-overview/app-playback.png)

## Ressources supplémentaires

Regardez le [créateur de compétences Dynamic Media : Vidéo dans Dynamic Media Classic](https://seminars.adobeconnect.com/p2ueiaswkuze) webinaire à la demande pour découvrir comment utiliser les fonctionnalités vidéo dans Dynamic Media Classic.
