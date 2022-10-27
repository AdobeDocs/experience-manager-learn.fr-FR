---
title: Présentation des différents types de PDF forms et de documents
description: PDF est en fait une famille de formats de fichiers, et cet article décrit les types de PDF importants et pertinents pour les développeurs de formulaires.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
kt: 7071
topic: Development
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
last-substantial-update: 2020-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 0%

---

# PDF

Portable Document Format (PDF) est en fait une famille de formats de fichier, et cet article détaille ceux qui sont les plus pertinents pour les développeurs de formulaires. De nombreux détails techniques et normes de différents types de PDF évoluent et changent. Certains de ces formats et spécifications sont des normes de l’Organisation internationale de normalisation (ISO), et d’autres sont une propriété intellectuelle spécifique appartenant à l’Adobe.

Cet article vous explique comment créer différents types de PDF. Cela vous aide à comprendre comment et pourquoi utiliser chacun d’eux. Tous ces types fonctionnent mieux dans l’outil client principal pour l’affichage et l’utilisation de PDF : Adobe Acrobat DC.

Voici un exemple de fichier PDF/A dans Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Des exemples de fichiers peuvent être [téléchargé ici](assets/pdf-file-types.zip)

## PDF d’architecture de Forms XML (PDF XFA)

Adobe utilise le terme PDF XFA pour faire référence au Forms interactif et dynamique que vous créez avec AEM Forms Designer. Les Forms et les fichiers que vous créez avec Designer sont basés sur l’architecture Forms XML (XFA) d’Adobe. Le format de fichier du PDF XFA est plus proche d’un fichier de HTML que d’un fichier de PDF traditionnel. Par exemple, le code suivant montre à quoi ressemble un objet de texte simple dans un fichier de PDF XFA.

![Champ de texte](assets/text-field.JPG)

XFA Forms est basé sur XML. Ce format bien structuré et flexible permet à un serveur AEM Forms de transformer vos fichiers Designer en différents formats, y compris les formats traditionnels PDF, PDF/A et HTML. Vous pouvez voir la structure XML complète de votre Forms dans Designer en sélectionnant le panneau Source XML de l’éditeur de mise en forme. Vous pouvez créer des Forms XFA statiques et dynamiques dans AEM Forms Designer.

## PDF statique

La mise en page statique des PDF forms XFA ne change jamais au moment de l’exécution, mais elle peut être interactive pour l’utilisateur. Voici quelques avantages des PDF forms XFA statiques :

* La mise en page statique des PDF forms XFA ne change jamais au moment de l’exécution, mais elle peut être interactive pour l’utilisateur.
* Les Forms statiques prennent en charge les outils de commentaires et de balisage Acrobat.
* Forms statique vous permet d’importer et d’exporter des commentaires Acrobat.
* Les Forms statiques prennent en charge le sous-paramétrage des polices. Il s’agit d’une technique pouvant être réalisée sur un serveur AEM Forms.
* La Forms statique peut être rendue à l’aide de la visionneuse de PDF intégrée fournie avec les navigateurs modernes.

>[!NOTE]
>
> Vous pouvez créer des PDF statiques à l’aide d’AEM Forms Designer en enregistrant XDP en tant que Adobe de PDF statique.



### Forms dynamique

Les PDF XFA dynamiques peuvent modifier leur mise en page au moment de l’exécution, de sorte que les fonctions de commentaire et de balisage ne sont pas prises en charge. Toutefois, les PDF XFA dynamiques offrent les avantages suivants :

* Les formulaires dynamiques prennent en charge les scripts côté client qui modifient la mise en page et la pagination du formulaire. Par exemple, le fichier Purchase Order.xdp s’agrandit et s’agrandit pour s’adapter à une quantité infinie de données si vous l’enregistrez en tant que formulaire dynamique.
* Les formulaires dynamiques prennent en charge toutes les propriétés de votre formulaire au moment de l’exécution, tandis que les formulaires statiques ne prennent en charge qu’un sous-ensemble.

>[!NOTE]
>
> Vous pouvez créer des formulaires PDF dynamiques à l’aide d’AEM Forms Designer en enregistrant le fichier XDP en tant que Adobe de formulaire XML dynamique.

>[!NOTE]
>
> Les formulaires dynamiques ne peuvent pas être rendus à l’aide des visionneuses PDF intégrées des navigateurs modernes.

### Fichier PDF (PDF traditionnel)

Un document certifié fournit au document PDF et aux destinataires Forms des assurances supplémentaires de son authenticité et de son intégrité.

Le format de PDF le plus populaire et le plus répandu est le fichier PDF traditionnel. Il existe de nombreuses façons de créer un fichier de PDF traditionnel, notamment en utilisant Acrobat et de nombreux outils tiers. Acrobat propose toutes les méthodes suivantes pour créer des fichiers de PDF traditionnels. Si Acrobat n’est pas installé, il se peut que ces options ne s’affichent pas sur votre ordinateur.

* En capturant le flux d’impression d’une application de bureau : Choisissez la commande Imprimer d’une application de création et sélectionnez l’icône d’imprimante Adobe PDF. Au lieu d’une copie imprimée du document, vous aurez créé un fichier PDF du document.
* En utilisant le module externe Acrobat PDFMaker avec les applications Microsoft Office : Lorsque vous installez Acrobat, un menu Adobe PDF est ajouté aux applications Microsoft Office, ainsi qu’une icône au ruban Office. Vous pouvez utiliser ces fonctions ajoutées pour créer des fichiers PDF directement dans Microsoft Office.
* En utilisant Acrobat Distiller pour convertir des fichiers Postscript et Encapsulated Postscript (EPS) en PDF : Distiller est généralement utilisé dans la publication imprimée et dans d’autres workflows qui nécessitent une conversion du format Postscript au format PDF.
* Sous le capot, un PDF traditionnel est très différent d’un PDF XFA. Il n’a pas la même structure XML, et comme il est créé en capturant le flux d’impression d’un fichier, un PDF traditionnel est un fichier statique et en lecture seule.

Un document certifié fournit au PDF un document et aux destinataires des formulaires des garanties supplémentaires d’authenticité et d’intégrité.

### Acro

Les Acroforms sont une ancienne technologie de formulaire interactif de l’Adobe. ils remontent à Acrobat version 3. Adobe fournit la variable [Référence de l’API Forms Acrobat](assets/FormsAPIReference.pdf), daté de mai 2003, afin de fournir les détails techniques de cette technologie. Les Acroforms sont une combinaison des éléments suivants :

* PDF traditionnel qui définit la disposition statique et les graphiques du formulaire.
* Champs de formulaire interactifs verrouillés au-dessus des outils de formulaire du programme Adobe Acrobat. Ces outils de formulaire constituent un petit sous-ensemble de ce qui est disponible dans AEM Forms Designer.

### PDF/A (PDF pour l’archive)

PDF/A (PDF pour les archives) tire parti des avantages du stockage de documents des PDF traditionnels avec de nombreux détails spécifiques qui améliorent l’archivage à long terme. Le format de fichier PDF traditionnel offre de nombreux avantages pour le stockage de documents à long terme. La nature compacte du PDF facilite le transfert et conserve l’espace, et sa nature bien structurée permet une indexation et une recherche puissantes. Le PDF traditionnel prend largement en charge les métadonnées et PDF prend depuis longtemps en charge différents environnements informatiques.

Comme PDF, PDF/A est une spécification standard ISO. Il a été développé par un groupe de travail comprenant AIIM (Association for Information and Image Management), NPES (National Printing Equipment Association) et le bureau administratif des tribunaux américains. Puisque l’objectif de la spécification PDF/A est de fournir un format d’archive à long terme, de nombreuses fonctionnalités de PDF sont omises afin que les fichiers puissent être autonomes. Voici quelques points clés concernant la spécification qui améliore la reproductibilité à long terme du fichier PDF/A :

* Tout le contenu doit être contenu dans le fichier et il ne peut pas y avoir de dépendances à des sources externes telles que des hyperliens, des polices ou des logiciels.
* Toutes les polices doivent être incorporées et il doit s’agir de polices disposant d’une licence d’utilisation illimitée pour les documents électroniques.
* JavaScript non autorisé
* La transparence n&#39;est pas autorisée
* Le chiffrement n’est pas autorisé
* Le contenu audio et vidéo n’est pas autorisé
* Les espaces colorimétriques doivent être définis indépendamment de l’appareil.
* Toutes les métadonnées doivent respecter certaines normes.

### Affichage d’un fichier PDF/A

Deux fichiers des exemples de fichiers ont été créés à partir du même fichier Microsoft Word. L’un a été créé en tant que PDF traditionnel, l’autre en tant que fichier PDF/A. Ouvrez ces deux fichiers dans Acrobat Professional :

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Bien que les documents semblent identiques, le fichier PDF/A s’ouvre avec une barre bleue en haut, indiquant que vous affichez ce document en mode PDF/A. Cette barre bleue est la barre de message du document Acrobat, qui s’affiche lorsque vous ouvrez certains types de fichiers de PDF.

![Pdf-img](assets/pdfa-message.png)

La barre de message du document contient des instructions et éventuellement des boutons pour vous aider à effectuer une tâche. Il est codé en couleur, et vous verrez la couleur bleue lorsque vous ouvrez des types spéciaux de PDF (comme ce fichier PDF/A) ainsi que des PDF certifiés et signés numériquement. La barre devient violette pour les PDF forms et jaune lorsque vous participez à une révision de PDF.

>[!NOTE]
>
> Si vous cliquez sur Activer la modification, vous sortez ce document de la conformité PDF/A.
