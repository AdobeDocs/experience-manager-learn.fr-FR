---
title: Comprendre les différents types de PDF forms et de documents
description: PDF est en fait une famille de formats de fichier, et cet article décrit les types de PDF importants et pertinents pour les développeurs de formulaires.
solution: Experience Manager Forms
product: aem
type: Documentation
role: Developer
level: Beginner,Intermediate
version: 6.3,6.4,6.5
feature: Document Services
topic: development
kt: 7071
translation-type: tm+mt
source-git-commit: 1e945afddda3d7735005029952a9d7ec46828bc6
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 0%

---


# Formats PDF

Portable Document Format (PDF) est en fait une famille de formats de fichier, et cet article détaille ceux qui sont les plus pertinents pour les développeurs de formulaires. La plupart des détails techniques et des normes des différents types de PDF évoluent et évoluent. Certains de ces formats et spécifications sont des normes de l&#39;Organisation internationale de normalisation (ISO), et d&#39;autres sont la propriété intellectuelle spécifique de l&#39;Adobe.

Cet article vous explique comment créer différents types de PDF. Il vous aidera à comprendre comment et pourquoi utiliser chacun d&#39;eux. Tous ces types fonctionnent mieux avec l’outil client de premier plan pour l’affichage et l’utilisation de fichiers PDF—Adobe Acrobat DC.

Il s’agit d’un exemple de fichier PDF/A dans Acrobat DC.
![pdfa](assets/pdfa-file-in-acrobat.png)

Les exemples de fichiers peuvent être [téléchargés ici](assets/pdf-file-types.zip)

## PDF XFA

Adobe utilise le terme formulaire PDF pour faire référence aux formulaires interactifs et dynamiques que vous créez avec AEM Forms Designer. Il est important de noter qu’il existe un autre type de formulaire PDF, appelé Acroform, différent des PDF forms que vous créez dans AEM Forms Designer. Les formulaires et les fichiers que vous créez avec Designer sont basés sur l’architecture XML Forms Architecture (XFA) de l’Adobe. De bien des façons, le format de fichier PDF XFA est plus proche d’un fichier HTML que d’un fichier PDF traditionnel. Par exemple, le code suivant vous montre à quoi ressemble un simple objet de texte dans un fichier PDF XFA.

![champ de texte](assets/text-field.JPG)

Comme vous pouvez le constater, les formulaires XFA sont basés sur XML. Ce format flexible et bien structuré permet à un AEM Forms Server de transformer vos fichiers Designer en différents formats, notamment PDF, PDF/A et HTML traditionnels. Vous pouvez voir la structure XML complète de vos formulaires dans Designer en sélectionnant l’onglet Source XML de l’éditeur de mise en forme. Vous pouvez créer des formulaires XFA statiques et dynamiques dans AEM Forms Designer.

## PDF statique

Les PDF forms XFA statiques ne modifieront pas leur disposition au moment de l’exécution, mais ils peuvent être interactifs pour l’utilisateur. Voici quelques avantages des PDF forms XFA statiques :

* Les PDF forms XFA statiques ne modifieront pas leur disposition au moment de l’exécution, mais ils peuvent être interactifs pour l’utilisateur.
* Les formulaires statiques prennent en charge les outils de commentaires et de balisage d’Acrobat.
* Les formulaires statiques vous permettent d’importer et d’exporter des commentaires Acrobat.
* Les formulaires statiques prennent en charge le sous-paramétrage des polices, technique qui peut être appliquée sur un serveur AEM Forms.
* Les formulaires statiques peuvent être rendus à l’aide du lecteur PDF intégré fourni avec les navigateurs modernes.

>[!NOTE]
> Vous pouvez créer des fichiers PDF statiques à l’aide d’AEM Forms Designer en enregistrant le fichier XDP en tant que Adobe de formulaire PDF statique.

## Forms dynamique

Les fichiers PDF XFA dynamiques peuvent modifier leur disposition au moment de l’exécution. Les fonctions de commentaire et d’annotation ne sont donc pas prises en charge. Toutefois, les fichiers PDF XFA dynamiques offre les avantages suivants :

* Les formulaires dynamiques prennent en charge les scripts client qui modifient la disposition et la pagination du formulaire. Par exemple, le fichier Purchase Order.xdp s’agrandit et pagine pour contenir une quantité infinie de données si vous l’enregistrez sous forme de formulaire dynamique.
* Les formulaires dynamiques prennent en charge toutes les propriétés de votre formulaire au moment de l’exécution, tandis que les formulaires statiques ne prennent en charge qu’un sous-ensemble.


>[!NOTE]
> Vous pouvez créer des fichiers PDF dynamiques à l’aide d’AEM Forms Designer en enregistrant le fichier XDP en tant que Adobe de formulaire XML dynamique.

>[!NOTE]
> Les formulaires dynamiques ne peuvent pas être rendus à l’aide des visionneuses PDF intégrées des navigateurs modernes.


## Fichier PDF (PDF traditionnel)

Le format PDF le plus populaire et le plus répandu est le fichier PDF traditionnel. Il existe de nombreuses façons de créer un fichier PDF traditionnel, notamment en utilisant Acrobat et de nombreux outils tiers. Acrobat propose toutes les méthodes suivantes pour créer des fichiers PDF traditionnels. Si Acrobat n’est pas installé sur votre ordinateur, il se peut que ces options ne s’affichent pas sur celui-ci.

* En capturant le flux d’impression d’une application de bureau : Choisissez la commande Imprimer d’une application de création et sélectionnez l’icône d’imprimante Adobe PDF. Au lieu d’une copie imprimée de votre document, vous aurez créé un fichier PDF de votre document.
* En utilisant le module externe Acrobat PDFMaker avec les applications Microsoft Office : Lorsque vous installez Acrobat, il ajoute un menu Adobe PDF aux applications Microsoft Office et une icône au ruban Office. Vous pouvez utiliser ces fonctions ajoutées pour créer des fichiers PDF directement dans Microsoft Office.
* En utilisant Acrobat Distiller pour convertir des fichiers Postscript et Postscript encapsulé (EPS) en fichiers PDF : Distiller est généralement utilisé dans la publication imprimée et les autres workflows qui nécessitent une conversion du format Postscript au format PDF.
* Sous le capot, un PDF traditionnel est très différent d’un PDF XFA. Il n’a pas la même structure XML et puisqu’il est créé en capturant le flux d’impression d’un fichier, un fichier PDF traditionnel est un fichier statique et en lecture seule.

Un Document certifié fournit aux destinataires de document PDF et de formulaires des garanties supplémentaires d’authenticité et d’intégrité.

## Acroforms

Les Acroforms sont l’ancienne technologie de formulaire interactif de l’Adobe ; ils remontent à la version 3 d&#39;Acrobat. Adobe fournit le [Référence de l&#39;API Forms ](assets/FormsAPIReference.pdf) de Acrobat, daté de mai 2003, pour fournir les détails techniques de cette technologie. Les Acroformes sont une combinaison de
éléments suivants :

* PDF traditionnel qui définit la disposition statique et les graphiques du formulaire.
* Champs de formulaire interactifs verrouillés au-dessus avec les outils de formulaire du programme Adobe Acrobat. Notez que ces outils de formulaire constituent un petit sous-ensemble de ce qui est disponible dans AEM Forms Designer.

## PDF/A (PDF pour archivage)

PDF/A (PDF for Archives) s&#39;appuie sur les avantages des PDF traditionnels pour l&#39;enregistrement des documents, avec de nombreux détails spécifiques qui améliorent l&#39;archivage à long terme. Le format de fichier PDF traditionnel offre de nombreux avantages pour les enregistrements de document à long terme. La nature compacte du PDF facilite le transfert et permet de conserver de l’espace, et sa nature bien structurée permet d’indexer et de rechercher de manière puissante. Le format PDF traditionnel prend en charge les métadonnées de manière intensive et le format PDF prend en charge depuis longtemps différents environnements informatiques.

Tout comme PDF, PDF/A est une norme ISO. Il a été mis au point par une force de tâche qui comprenait l&#39;AIIM (Association for Information and Image Management), la NPES (National Printing Equipment Association) et le bureau administratif des tribunaux américains. La spécification PDF/A ayant pour objectif de fournir un format d’archive à long terme, de nombreuses fonctions PDF sont omises afin que les fichiers puissent être autonomes. Voici quelques points clés de la spécification qui améliorent la reproductibilité à long terme du fichier PDF/A :

* Tout le contenu doit être contenu dans le fichier et il ne peut pas y avoir de dépendance sur des sources externes telles que les hyperliens, les polices ou les programmes logiciels.
* Toutes les polices doivent être incorporées et doivent être des polices disposant d’une licence d’utilisation illimitée pour les documents électroniques.
* JavaScript non autorisé
* La transparence n’est pas autorisée
* Le chiffrement n&#39;est pas autorisé
* Le contenu audio et vidéo n’est pas autorisé
* Les espaces colorimétriques doivent être définis indépendamment du périphérique
* Toutes les métadonnées doivent respecter certaines normes

## Affichage d’un fichier PDF/A

Deux fichiers des exemples de fichiers ont été créés à partir du même fichier Microsoft Word. L’un a été créé en tant que PDF traditionnel, l’autre en tant que fichier PDF/A. Ouvrez ces deux fichiers dans Acrobat Professional :

simpleWordFile.pdf
simpleWordFilePDFA.pdf

Bien que les documents aient la même apparence, le fichier PDF/A s’ouvre avec une barre bleue en haut, ce qui indique que vous visualisez ce document en mode PDF/A. Cette barre bleue est la barre de message document d’Acrobat, que vous verrez lorsque vous ouvrirez certains types de fichiers PDF.

![pdf-img](assets/pdfa-message.png)

La barre de message du document contient des instructions et peut-être des boutons pour vous aider à terminer une tâche. Il est codé par couleur et la couleur bleue s’affiche lorsque vous ouvrez des types spéciaux de fichiers PDF (tels que ce fichier PDF/A) ainsi que des fichiers PDF certifiés et signés numériquement. La barre devient violette pour le PDF forms et jaune lorsque vous participez à une révision PDF.

>[!NOTE]
> Si vous cliquez sur Activer la modification, vous supprimerez ce document de la conformité à la norme PDF/A.




