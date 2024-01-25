---
title: Comprendre les différents types de formulaires et documents PDF
description: PDF est en fait une famille de formats de fichiers. Cet article décrit les types de PDF importants et pertinents pour les équipes de développement de formulaires.
type: Documentation
role: Developer
level: Beginner, Intermediate
version: 6.4, 6.5
feature: PDF Generator
jira: KT-7071
topic: Development
last-substantial-update: 2020-07-07T00:00:00Z
duration: 297
exl-id: ffa9d243-37e5-420c-91dc-86c73a824083
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 98%

---

# PDF

Le format Portable Document Format (PDF) est en fait une famille de formats de fichier. Cet article recense ceux qui sont les plus pertinents pour les équipes de développement de formulaires. Les détails techniques et les normes de différents types de PDF évoluent constamment. Certains de ces formats et spécifications sont des normes de l’Organisation internationale de normalisation (ISO). D’autres sont la propriété intellectuelle spécifique d’Adobe.

Cet article explique comment créer différents types de PDF. Il explique comment et pourquoi utiliser chacun d’eux. Le fonctionnement de tous ces types est optimisé dans l’outil client principal pour l’affichage et l’utilisation de PDF : Adobe Acrobat DC.

Voici un exemple de fichier PDF/A dans Acrobat DC.

![Pdfa](assets/pdfa-file-in-acrobat.png)

Vous pouvez télécharger des exemples de fichiers [ici](assets/pdf-file-types.zip).

## PDF d’architecture de formulaires XML (PDF XFA)

Adobe utilise le terme de formulaire PDF XFA pour faire référence aux formulaires interactifs et dynamiques que vous créez avec AEM Forms Designer. Les formulaires et les fichiers que vous créez avec Designer sont basés sur l’architecture de formulaires XML (XFA) d’Adobe. À de nombreux égards, le format de fichier PDF XFA est plus proche de celui d’un fichier HTML que d’un fichier PDF traditionnel. Par exemple, le code suivant montre à quoi ressemble un objet de texte simple dans un fichier PDF XFA.

![Champ de texte.](assets/text-field.JPG)

Les formulaires XFA sont basés sur le langage XML. Ce format bien structuré et flexible permet à un serveur AEM Forms de transformer vos fichiers Designer en différents formats, y compris les formats PDF, PDF/A et HTML traditionnels. Vous pouvez voir la structure XML complète de vos formulaires dans Designer en sélectionnant l’onglet Source XML de l’éditeur de disposition. Vous pouvez créer des formulaires XFA statiques et dynamiques dans AEM Forms Designer.

## PDF statique

La disposition des formulaires PDF XFA statiques ne change jamais au moment de l’exécution, mais elle peut être interactive pour l’utilisateur ou l’utilisatrice. Voici quelques avantages des formulaires PDF XFA statiques :

* La disposition des formulaires PDF XFA statiques ne change jamais au moment de l’exécution, mais elle peut être interactive pour l’utilisateur ou l’utilisatrice.
* Les formulaires statiques prennent en charge les outils de commentaires et de balisage Acrobat.
* Les formulaires statiques vous permettent d’importer et d’exporter des commentaires Acrobat.
* Les formulaires statiques prennent en charge le subsetting des polices. Il s’agit d’une technique pouvant être réalisée sur un serveur AEM Forms.
* Les formulaires statiques peuvent être rendus à l’aide de la visionneuse de PDF intégrée, disponible dans les navigateurs modernes.

>[!NOTE]
>
> Vous pouvez créer des PDF statiques à l’aide d’AEM Forms Designer en enregistrant le fichier XDP en tant que formulaire PDF statique d’Adobe.



### Formulaires dynamiques

La disposition des PDF XFA dynamiques peut se modifier au moment de l’exécution, si bien que les fonctions de commentaire et de balisage ne sont pas prises en charge. Toutefois, les PDF XFA dynamiques offrent les avantages suivants :

* Les formulaires dynamiques prennent en charge les scripts côté client qui modifient la disposition et la pagination du formulaire. Par exemple, le fichier Purchase Order.xdp s’agrandit et modifie sa pagination pour s’adapter à une quantité infinie de données si vous l’enregistrez en tant que formulaire dynamique.
* Les formulaires dynamiques prennent en charge toutes les propriétés de votre formulaire au moment de l’exécution, tandis que les formulaires statiques ne prennent en charge qu’un sous-ensemble.

* [Consultez ce document pour comprendre les différences entre les formulaires pdf statiques et dynamiques.](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/document-services/pdf-forms-and-documents.html#:~:text=Dynamic%20forms%20support%20all%20the,forms%20support%20only%20a%20subset)

>[!NOTE]
>
> Vous pouvez créer des PDF dynamiques à l’aide d’AEM Forms Designer en enregistrant le fichier XDP en tant que formulaire XML dynamique Adobe.

>[!NOTE]
>
> Les formulaires dynamiques ne peuvent pas être rendus à l’aide des visionneuses PDF intégrées des navigateurs modernes.

### Fichier PDF (PDF traditionnel)

Un document certifié fournit aux personnes destinataires d’un document et de formulaires PDF des garanties supplémentaires sur leur authenticité et leur intégrité.

Le format PDF le plus populaire et le plus répandu est le fichier PDF traditionnel. Il existe de nombreuses façons de créer un fichier PDF traditionnel, notamment en utilisant Acrobat et de nombreux outils tiers. Acrobat propose toutes les méthodes suivantes pour créer des fichiers PDF traditionnels. Si Acrobat n’est pas installé, il se peut que ces options ne s’affichent pas sur votre ordinateur.

* En capturant le flux d’impression d’une application de bureau : choisissez la commande Imprimer d’une application de création et sélectionnez l’icône d’imprimante Adobe PDF. Au lieu d’obtenir un exemplaire papier du document, vous aurez créé un fichier PDF de ce document.
* En utilisant le module Acrobat PDFMaker avec les applications Microsoft Office : lorsque vous installez Acrobat, un menu Adobe PDF est ajouté aux applications Microsoft Office et une icône est intégrée au ruban Office. Vous pouvez utiliser ces fonctions supplémentaires pour créer des fichiers PDF directement dans Microsoft Office.
* En utilisant Acrobat Distiller pour convertir des fichiers Postscript et Encapsulated Postscript (EPS) en PDF : Distiller est généralement utilisé dans la publication papier et dans d’autres workflows qui nécessitent une conversion du format Postscript au format PDF.
* En termes de structure, un PDF traditionnel est très différent d’un PDF XFA. Il n’a pas la même structure XML, et comme il est créé en capturant le flux d’impression d’un fichier, un PDF traditionnel est un fichier statique et en lecture seule.

Un document certifié fournit aux personnes destinataires d’un document et de formulaires PDF des garanties supplémentaires sur leur authenticité et leur intégrité.

### AcroForms

Les formulaires AcroForms sont l’ancienne technologie de formulaire interactif d’Adobe. Ils remontent à la version 3 d’Acrobat. Adobe fournit la [Référence de l’API des formulaires Acrobat](assets/FormsAPIReference.pdf), datée de mai 2003, afin de fournir les détails techniques de cette technologie. Les formulaires AcroForms sont une combinaison des éléments suivants :

* PDF traditionnel qui définit la disposition statique et les graphiques du formulaire.
* Champs de formulaire interactifs ajoutés avec des outils de formulaire du programme Adobe Acrobat. Ces outils de formulaire constituent un petit sous-ensemble de ce qui est disponible dans AEM Forms Designer.

### PDF/A (PDF pour les archives)

PDF/A (PDF pour les archives) tire parti des avantages du stockage de documents PDF traditionnels avec de nombreux détails spécifiques qui améliorent l’archivage à long terme. Le format de fichier PDF traditionnel offre de nombreux avantages pour le stockage de documents à long terme. La nature compacte du PDF facilite le transfert et conserve l’espace, et sa nature bien structurée permet une indexation et une recherche puissantes. Le format PDF traditionnel prend largement en charge les métadonnées, et il prend depuis longtemps en charge différents environnements informatiques.

Comme PDF, PDF/A est une spécification standard ISO. Il a été développé par un groupe de travail comprenant l’AIIM (Association for Information and Image Management), la NPES (National Printing Equipment Association) et le bureau d’administration des tribunaux des États-Unis. Puisque l’objectif de la spécification PDF/A est de fournir un format d’archive à long terme, de nombreuses fonctionnalités de PDF sont omises afin que les fichiers puissent être autonomes. Voici quelques points clés concernant la spécification qui améliore la reproductibilité à long terme du fichier PDF/A :

* Tout le contenu doit être contenu dans le fichier et il ne peut pas y avoir de dépendances à des sources externes telles que des hyperliens, des polices ou des logiciels.
* Toutes les polices doivent être incorporées et il doit s’agir de polices disposant d’une licence d’utilisation illimitée pour les documents électroniques.
* JavaScript n’est pas autorisé.
* La transparence n’est pas autorisée.
* Le chiffrement n’est pas autorisé.
* Les contenus audio et vidéo ne sont pas autorisés.
* Les espaces colorimétriques doivent être définis indépendamment de l’appareil.
* Toutes les métadonnées doivent respecter certaines normes.

### Afficher un fichier PDF/A

Deux fichiers dans les exemples de fichiers ont été créés à partir du même fichier Microsoft Word. L’un a été créé en tant que PDF traditionnel, l’autre en tant que fichier PDF/A. Ouvrez ces deux fichiers dans Acrobat Professional :

* simpleWordFile.pdf
* simpleWordFilePDFA.pdf

Bien que les documents semblent identiques, le fichier PDF/A s’ouvre avec une barre bleue en haut, indiquant que vous affichez ce document en mode PDF/A. Cette barre bleue est la barre de message du document Acrobat, qui s’affiche lorsque vous ouvrez certains types de fichiers PDF.

![Pdf-img](assets/pdfa-message.png)

La barre de message du document contient des instructions et éventuellement des boutons pour vous aider à effectuer une tâche. Elle est codée en couleur, et vous verrez la couleur bleue lorsque vous ouvrez des types spéciaux de PDF (comme ce fichier PDF/A) ainsi que des PDF certifiés et signés numériquement. La barre devient violette pour les formulaires PDF forms et jaune lorsque vous participez à une révision d’un PDF.

>[!NOTE]
>
> Si vous cliquez sur Activer la modification, ce document n’est plus conforme à la norme PDF/A.
