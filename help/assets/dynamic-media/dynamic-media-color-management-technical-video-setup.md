---
title: Explication de la gestion des couleurs avec AEM Dynamic Media
description: Dans cette vidéo, nous examinons la gestion des couleurs de Dynamic Media et comment elle peut être utilisée pour fournir des fonctionnalités d’aperçu de la correction des couleurs dans pour AEM Assets.
sub-product: dynamic-media
feature: Image Profiles, Video Profiles
version: 6.4, 6.5
topic: Content Management
role: Developer
level: Intermediate
exl-id: a733532b-db64-43f6-bc43-f7d422d5071a
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 26%

---

# Explication de la gestion des couleurs avec AEM Dynamic Media{#understanding-color-management-with-aem-dynamic-media}

Dans cette vidéo, nous examinons la gestion des couleurs de Dynamic Media et comment elle peut être utilisée pour fournir des fonctionnalités d’aperçu de la correction des couleurs dans pour AEM Assets.

>[!VIDEO](https://video.tv.adobe.com/v/16792/?quality=9&learn=on)

>[!NOTE]
>
>[Activer Dynamic Media](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=fr) dans AEM pour utiliser cette fonctionnalité.

Cette fonctionnalité est disponible pour AEM versions 6.1 et 6.2 en tant que Feature Pack.

## Modèle XML pour le noeud de configuration de la gestion des couleurs {#xml-template-for-the-color-management-configuration-node}

Voici le modèle XML du noeud de configuration de la gestion des couleurs. Ce modèle XML peut être copié dans le projet de développement AEM et configuré avec les configurations appropriées au projet.

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

### La liste des profils colorimétriques d’Adobe par défaut est répertoriée ci-dessous. {#list-of-default-adobe-color-profiles-are-listed-below}

| Nom | Espace colorimétrique | Description |
| ------------------- | ---------- | ------------------------------------- |
| AdobeRGB | RVB | Adobe RGB (1998) |
| AppleRGB | RVB | Apple RGB |
| CIERGB | RVB | RGB CIE |
| CoatedFogra27 | CMJN | FOGRA27 (ISO 12647-2:2004) enrobé |
| CoatedFogra39 | CMJN | FOGRA39 (ISO 12647-2:2004) enrobé |
| CoatedGraCol | CMJN | Coated GRACoL 2006 (ISO 12647-2:2004) |
| ColorMatchRGB | RVB | RGB ColorMatch |
| EuropeISOCoated | CMJN | Europe ISO Coated FOGRA27 |
| EuroscaleCoated | CMJN | Euroscale Coated v2 |
| EuroscaleUncoul | CMJN | Euroscale Uncoute v2 |
| JapanColorCoated | CMJN | Japan Color 2001 Coated |
| JapanColorNewspaper | CMJN | Journal Japan Color 2002 |
| JapanColorUnfill | CMJN | Japan Color 2001 Unfill |
| JapanColorWebCoated | CMJN | Japan Color 2003 Web Coated |
| JapanWebCoated | CMJN | Japan Web Coated (Ad) |
| NewsprintSNA2007 | CMJN | Journal des États-Unis (SNA 2007) |
| NTSC | RVB | NTSC (1953) |
| PAL | RVB | PAL/SECAM |
| ProPhoto | RVB | ProPhoto RGB |
| PS4Default | CMJN | Photoshop 4 CMJN par défaut |
| PS5Default | CMJN | Photoshop 5 CMJN par défaut |
| SheetfedCoated | CMJN | U.S. Sheetfed Coated v2 |
| SheetfedUnPAW | CMJN | U.S. Sheetfed Non Couché v2 |
| SMPTE | RVB | SMPTE-C |
| sRVB | sRVB RGB | IEC61966-2.1 |
| UncondamnésFogra29 | CMJN | FOGRA29 non couché (ISO 12647-2:2004) |
| WebCoated | CMJN | U.S. Web Coated (SWOP) v2 |
| WebCoatedFogra28 | CMJN | FOGRA Web Coated 28 (ISO 12647-2:2004) |
| WebCoatedClass3 | CMJN | Document SWOP 2006 de qualité 3 sur support Web |
| WebCoatedClass5 | CMJN | Papier de qualité 5 SWOP 2006 à couverture Web |
| WebUnCouché | CMJN | U.S. Web Non couché v2 |
| WideGamutRGB | RVB | RGB Gamme large |

## Ressources supplémentaires{#additional-resources}

* [Configuration de la gestion des couleurs Dynamic Media](https://helpx.adobe.com/experience-manager/6-5/assets/using/config-dynamic.html#ConfiguringDynamicMediaColorManagement)
