---
title: Explication de la gestion des couleurs avec AEM Dynamic Media
description: Dans cette vidéo, nous étudions la gestion des couleurs de Dynamic Media et comment elle peut être utilisée pour fournir des fonctionnalités de prévisualisation de correction des couleurs dans pour AEM Assets.
sub-product: dynamic-media
feature: Profils d’images, Profils vidéo
version: 6.3, 6.4, 6.5
topic: Gestion de contenu
role: Développeur
level: Intermédiaire
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 24%

---


# Explication de la gestion des couleurs avec AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Dans cette vidéo, nous étudions la gestion des couleurs de Dynamic Media et comment elle peut être utilisée pour fournir des fonctionnalités de prévisualisation de correction des couleurs dans pour AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Activez l’AEM ](https://docs.adobe.com/docs/en/aem/6-0/administer/integration/dynamic-media/enabling-dynamic-media.html) Média dynamique pour utiliser cette fonction.

Cette fonctionnalité est disponible pour AEM versions 6.1 et 6.2 en tant que Feature Pack.

## Modèle XML pour le noeud de configuration Gestion des couleurs {#xml-template-for-the-color-management-configuration-node}

Voici le modèle XML pour le noeud de configuration Gestion des couleurs. Ce modèle XML peut être copié dans le projet de développement AEM et configuré avec les configurations appropriées au projet.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--
    XML Node definition for: /etc/dam/imageserver/configuration/jcr:content/settings

 Adobe Docs

 * Image Server Configuration: https://docs.adobe.com/docs/en/aem/6-2/administer/content/dynamic-media/config-dynamic.html#Configuring%20Dynamic%20Media%20Image%20Settings

* Default Color Profile Configuration: https://docs.adobe.com/docs/en/aem/6-1/administer/content/dynamic-media/config-dynamic.html#Configuring%20the%20default%20color%20profiles

    iccprofileXXX values:
        Node name of color profile found at: /etc/dam/imageserver/profiles

    iccblackpointcompensation values:
        true | false

    iccdither values:
        true | false

    iccrenderintent values:
        0 for perceptual
        1 for relative colorimetric
        2 for saturation
        3 for absolute colorimetric

-->

<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0"
    xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
    jcr:primaryType="nt:unstructured"

        bkgcolor="FFFFFF"
        defaultpix="300,300"
        defaultthumbpix="100,100"
        expiration="{Long}36000000"
        jpegquality="80"
        maxpix="2000,2000"
        resmode="SHARP2"
        resolution="72"
        thumbnailtime="[1%,11%,21%,31%,41%,51%,61%,71%,81%,91%]"
        iccprofilergb=""
        iccprofilecmyk=""
        iccprofilegray=""
        iccprofilesrcrgb=""
        iccprofilesrccmyk=""
        iccprofilesrcgray=""
        iccblackpointcompensation="{Boolean}true"
        iccdither="{Boolean}false"
        iccrenderintent="{Long}0"
/>
```

### La liste des profils de couleur d&#39;Adobe par défaut est répertoriée ci-dessous {#list-of-default-adobe-color-profiles-are-listed-below}

| Nom | Espace colorimétrique | Description |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RVB | Adobe RGB (1998) |
| AppleRGB | RVB | Apple RGB |
| CIERGB | RVB | CIE RGB |
| CoatedFogra27 | CMJN | FOGRA27 recouvert (ISO 12647-2:2004) |
| CoatedFogra39 | CMJN | FOGRA39 recouvert (ISO 12647-2:2004) |
| CoatedGraCol | CMJN | GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RVB | ColorMatch RGB |
| EuropeISOCoated | CMJN | Europe ISO Coated FOGRA27 |
| EuroscaleCoated | CMJN | Euroscale Coated v2 |
| EuroscaleNon couché | CMJN | Euroscale UnCouché v2 |
| JapanColorCoated | CMJN | Japan Color 2001 Coated |
| JapanColorNewspaper | CMJN | Journal de Japan Color 2002 |
| JapanColorUnenduit | CMJN | Japan Color 2001 Unbedded |
| JapanColorWebCoated | CMJN | Couleur du Japon 2003 - Web Coated |
| JapanWebCoated | CMJN | Japon Web Coated (publicité) |
| NewsprintSNAP2007 | CMJN | Newsprint (SNAP 2007) |
| NTSC | RVB | NTSC (1953) |
| PAL | RVB | PAL/SECAM |
| ProPhoto | RVB | ProPhoto RGB |
| PS4Default | CMJN | CMJN par défaut Photoshop 4 |
| PS5Default | CMJN | CMJN par défaut Photoshop 5 |
| Feuillettré | CMJN | U.S. Sheetfed Coated v2 |
| FeuillesNon couché | CMJN | U.S. Sheetfed Non couché v2 |
| SMPTE | RVB | SMPTE-C |
| sRVB | RVB sRGB | IEC61966-2.1 |
| Fogra29 non couché | CMJN | FOGRA29 non couché (ISO 12647-2:2004) |
| WebCoated | CMJN | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMJN | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMJN | Papier de 3e année SWOP 2006 à revêtement Web |
| WebCoatedGrade5 | CMJN | Papier enrobé Web SWOP 2006 de classe 5 |
| WebUnCouché | CMJN | U.S. Web non couché v2 |
| WideGamutRGB | RVB | Gamme large RVB |

## Ressources supplémentaires{#additional-resources}

* [Configuration de la gestion des couleurs Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
