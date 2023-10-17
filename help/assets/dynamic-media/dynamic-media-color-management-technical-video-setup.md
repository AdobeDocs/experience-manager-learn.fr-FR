---
title: Explication de la gestion des couleurs avec AEM Dynamic Media
description: Cette vidéo est consacrée à la gestion des couleurs de Dynamic Media et à ses fonctionnalités de prévisualisation de la correction des couleurs pour AEM Assets.
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: ht
source-wordcount: '319'
ht-degree: 100%

---

# Explication de la gestion des couleurs avec AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Cette vidéo est consacrée à la gestion des couleurs de Dynamic Media et à ses fonctionnalités de prévisualisation de la correction des couleurs pour AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792?quality=12&learn=on)

>[!NOTE]
>
>[Activez Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr) dans AEM pour utiliser cette fonctionnalité.

Cette fonctionnalité est disponible dans AEM 6.1 et 6.2 en tant que pack de fonctionnalités.

## Modèle XML pour le nœud de configuration de la gestion des couleurs {#xml-template-for-the-color-management-configuration-node}

Voici le modèle XML du nœud de configuration de la gestion des couleurs. Ce modèle XML peut être copié dans le projet de développement AEM et doté des configurations de projet appropriées.

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

### La liste des profils Adobe Color par défaut est répertoriée ci-dessous. {#list-of-default-adobe-color-profiles-are-listed-below}

| Nom | Espace colorimétrique | Description |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RVB | Adobe RGB (1998) |
| AppleRGB | RVB | Apple RGB |
| CIERGB | RVB | CIE RGB |
| CoatedFogra27 | CMJN | Coated FOGRA27 (ISO 12647-2:2004) |
| CoatedFogra39 | CMJN | Coated FOGRA39 (ISO 12647-2:2004) |
| CoatedGraCol | CMJN | Coated GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RVB | RGB ColorMatch |
| EuropeISOCoated | CMJN | Europe ISO Coated FOGRA27 |
| EuroscaleCoated | CMJN | Euroscale Coated v2 |
| EuroscaleUncoated | CMJN | Euroscale Uncoated v2 |
| JapanColorCoated | CMJN | Japan Color 2001 Coated |
| JapanColorNewspaper | CMJN | Japan Color 2002 Newspaper |
| JapanColorUncoated | CMJN | Japan Color 2001 Uncoated |
| JapanColorWebCoated | CMJN | Japan Color 2003 Web Coated |
| JapanWebCoated | CMJN | Japan Web Coated (Ad) |
| NewsprintSNAP2007 | CMJN | US Newsprint (SNAP 2007) |
| NTSC | RVB | NTSC (1953) |
| PAL | RVB | PAL/SECAM |
| ProPhoto | RVB | ProPhoto RGB |
| PS4Default | CMJN | Photoshop 4 Default CMYK |
| PS5Default | CMJN | Photoshop 5 Default CMYK |
| SheetfedCoated | CMJN | U.S. Sheetfed Coated v2 |
| SheetfedUncoated | CMJN | U.S. Sheetfed Uncoated v2 |
| SMPTE | RVB | SMPTE-C |
| sRVB | RVB sRGB | IEC61966-2.1 |
| UncoatedFogra29 | CMJN | Uncoated FOGRA29 (ISO 12647-2:2004) |
| WebCoated | CMJN | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMJN | Web Coated FOGRA28 (ISO 12647-2:2004) |
| WebCoatedGrade3 | CMJN | Web Coated SWOP 2006 Grade 3 Paper |
| WebCoatedGrade5 | CMJN | Web Coated SWOP 2006 Grade 5 Paper |
| WebUncoated | CMJN | U.S. Web Uncoated v2 |
| WideGamutRGB | RVB | Wide Gamut RGB |

## Ressources supplémentaires{#additional-resources}

* [Configuration de la gestion des couleurs Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-65/assets/dynamic/config-dynamic.html?lang=fr)
