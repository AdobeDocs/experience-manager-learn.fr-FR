---
title: Chapitre 7 - Utiliser AEM Content Services à partir d’une application mobile - Content Services
description: Le chapitre 7 du tutoriel exécute l’application mobile Android pour utiliser du contenu créé à partir d’AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 512
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 100%

---

# Chapitre 7 : consulter AEM Content Services à partir d’une application mobile

Le chapitre 7 du tutoriel emploie une application mobile Android native pour utiliser du contenu d’AEM Content Services.

## Application mobile Android

Ce tutoriel utilise une **application mobile Android native simple** pour utiliser et afficher le contenu d’événement exposé par AEM Content Services.

L’utilisation d’[Android](https://developer.android.com/) n’est pas importante, et l’application mobile consommatrice peut être écrite dans n’importe quel framework pour n’importe quelle plateforme mobile, par exemple iOS.

Android est utilisé pour les tutoriels en raison de la possibilité d’exécuter un émulateur Android sous Windows, macOs et Linux, de sa popularité, et du fait qu’il peut être écrit en Java, un langage bien compris par les développeurs et développeuses AEM.

*L’application mobile Android du tutoriel n’est **pas**destinée à expliquer comment créer des applications mobiles Android ou transmettre les bonnes pratiques de développement Android, mais plutôt à illustrer la manière dont AEM Content Services peut être utilisé à partir d’une application mobile.*

### Comment AEM Content Services oriente l’expérience de l’application mobile

![Mappage de l’application mobile à Content Services](assets/chapter-7/content-services-mapping.png)

1. Le **logo** comme défini par le **Composant d’image** de la page [!DNL Events API].
1. Le **ligne de balise** comme définie sur le **Composant de texte** de la page [!DNL Events API].
1. Cette **liste des événements** est dérivée de la sérialisation des fragments de contenu d’événement, exposés via le **composant Liste de fragments de contenu** configuré.

## Démonstration de l’application mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configuration de l’application mobile pour une utilisation autre que localhost

Si l’instance de publication AEM n’est pas exécutée sur **http://localhost:4503**, l’hôte et le port peuvent être mis à jour dans les [!DNL Settings] de l’application mobile pour pointer vers la propriété hôte/port de l’instance de publication AEM.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Exécution locale de l’application mobile

1. Téléchargez et installez [Android Studio](https://developer.android.com/studio/install) pour installer l’émulateur Android.
1. **Téléchargez** le fichier [!DNL APK] Android [GitHub > Ressources > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest).
1. Ouvrez **Android Studio**.
   * Lors du lancement initial d’Android Studio, une invite vous demandant d’installer le [!DNL Android SDK] s’affiche. Acceptez les valeurs par défaut et terminez l’installation.
1. Ouvrez Android Studio et sélectionnez **Profile or Debug APK** (APK de profil ou de débogage).
1. Sélectionnez le fichier APK (**wknd-mobile.x.x.x.apk**) téléchargé à l’étape 2, puis cliquez sur **OK**
   * Si vous êtes invité à **Create a New Folder** (Créer un nouveau dossier) ou à **Use Existing** (Utiliser un dossier existant), sélectionnez **Use Existing** (Utiliser un dossier existant).
1. Lors du lancement initial d’Android Studio, cliquez avec le bouton droit de la souris sur **wknd-mobile.x.x.x** dans la liste Projects (Projets), puis sélectionnez **Open Module Settings** (Ouvrir les paramètres du module).
   * Sous **Modules > wknd-mobile.x.x.x > Dependencies tab** (Modules > wknd-mobile.x.x.x > onglet Dépendances), sélectionnez **Android API 29 Platform** (Plateforme d’API Android 29). Appuyez sur OK pour fermer et enregistrer les modifications.
   * Si vous ne le faites pas, une erreur « Please select Android SDK » (« Veuillez sélectionner le SDK Android ») s’affichera lors du lancement de l’émulateur.
1. Ouvrez **AVD Manager** (Gestionnaire AVD) en sélectionnant **Tools > AVD Manager** (Outils > Gestionnaire AVD) ou en appuyant sur l’icône **AVD Manager** (Gestionnaire AVD) dans la barre supérieure.
1. Dans la fenêtre **AVD Manager** (Gestionnaire AVD), cliquez sur **+ Create Virtual Device...** (+ Créer un appareil virtuel...) si l’appareil n’est pas encore enregistré.
   1. Dans la partie gauche, sélectionnez la catégorie **Phone** (Téléphone).
   1. Sélectionnez un **Pixel 2**.
   1. Cliquez sur le bouton **Next** (Suivant).
   1. Sélectionnez **Q** avec le **API Level 29** (Niveau d’API 29).
      * Lors du lancement initial du gestionnaire AVD, vous êtes invité à télécharger l’API avec version. Cliquez sur le lien Download (Télécharger) à côté de la version « Q », puis effectuez le téléchargement et l’installation.
   1. Cliquez sur le bouton **Next** (Suivant).
   1. Cliquez sur le bouton **Finish** (Terminer).
1. Fermez la fenêtre **AVD Manager** (Gestionnaire AVD).
1. Dans la barre de menu supérieure, sélectionnez **wknd-mobile.x.x.x** dans le menu déroulant **Run/Edit Configurations** (Exécuter/modifier des configurations).
1. Appuyez sur le bouton **Run** (Exécuter) à côté de l’option sélectionnée **Run/Edit Configurations** (Exécuter/modifier des configurations).
1. Dans la pop-up, sélectionnez l’appareil virtuel **[!DNL Pixel 2 API 29]** que vous venez de créer et appuyez sur **OK**.
1. Si l’application [!DNL WKND Mobile] ne charge pas immédiatement, cherchez et cliquez sur l’icône **[!DNL WKND]** de l’écran d’accueil Android dans l’émulateur.
   * Si l’émulateur se lance mais que l’écran de l’émulateur reste noir, appuyez sur le bouton **power** dans la fenêtre des outils de l’émulateur à côté de la fenêtre de l’émulateur.
   * Pour faire défiler dans l’appareil virtuel, cliquez et maintenez, puis faites glisser.
   * Pour actualiser le contenu à partir d’AEM, descendez depuis le haut jusqu’à ce que l’icône Actualiser (Refresh) s’affiche, puis relâchez.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## Code de l’application mobile

Cette section décrit le code de l’application mobile Android qui interagit le plus et qui dépend d’AEM Content Services et de sa sortie JSON.

Lors du chargement, l’application mobile applique `HTTP GET` à `/content/wknd-mobile/en/api/events.model.json` qui est le point d’entrée AEM Content Services configuré pour fournir le contenu permettant d’orienter l’application mobile.

Comme le modèle modifiable de l’API d’événements (`/content/wknd-mobile/en/api/events.model.json`) est verrouillé, l’application mobile peut être codée pour rechercher des informations spécifiques à des emplacements spécifiques dans la réponse JSON.

### Flux de code de haut niveau

1. Ouvrir l’application [!DNL WKND Mobile] génère un appel de requête `HTTP GET` à l’instance de publication AEM sur `/content/wknd-mobile/en/api/events.model.json` pour collecter le contenu afin de renseigner l’interface utilisateur de l’application mobile.
2. Lors de la réception du contenu d’AEM, les trois éléments d’affichage de l’application mobile, **le logo, la ligne de balise et la liste d’événements**, sont initialisés avec le contenu d’AEM.
   * Pour lier le contenu d’AEM à l’élément d’affichage de l’application mobile, le fichier JSON représentant chaque composant AEM est un objet mappé à un POJO Java, qui est à son tour lié à l’élément d’affichage Android.
      * JSON de composant d’image → POJO de logo → ImageView de logo
      * JSON de composant Texte → POJO de ligne de balise → ImageView de texte
      * JSON de liste de fragments de contenu → POJO d’événements → RecyclerView d’événements
   * *Le code de l’application mobile peut mapper le JSON aux POJO en raison des emplacements connus dans la réponse JSON supérieure. N’oubliez pas que les clés JSON « image », « text » et « contentfragmentlist » sont dictées par les noms de nœuds des composants AEM de support. Si ces noms de nœuds changent, l’application mobile est interrompue, car elle ne sait pas comment sourcer le contenu requis à partir des données JSON.*

#### Appeler le point d’entrée d’AEM Content Services

Vous trouverez ci-dessous un condensé du code dans la `MainActivity` de l’application mobile chargée d’appeler AEM Content Services pour collecter le contenu qui oriente l’expérience de l’application mobile.

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` est le crochet d’initialisation de l’application mobile et enregistre les 3 `ViewBinders` personnalisés chargés de l’analyse du fichier JSON et de la liaison des valeurs aux éléments `View`.

`initApp(...)` est ensuite appelée, ce qui envoie la requête HTTP GET au point d’entrée d’AEM Content Services sur l’instance de publication AEM pour collecter le contenu. Lors de la réception d’une réponse JSON valide, la réponse JSON est transmise à chaque `ViewBinder` chargé d’analyser le fichier JSON et de le lier aus éléments `View` mobiles.

#### Analyser la réponse JSON

Ensuite, nous allons voir le `LogoViewBinder`, qui est simple, mais met en évidence plusieurs points importants.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

La première ligne de `bind(...)` navigue vers le bas de la réponse JSON à l’aide des clés **:items → root → :items** qui représentent le conteneur de disposition d’AEM auquel les composants ont été ajoutés.

À partir de là, la clé nommée **image** fait l’objet d’une vérification. Cette clé représente le composant Image (il est important que ce nom de nœud → clé JSON soit stable). Si cet objet existe, il est lu et mappé au [POJO d’image personnalisée](#image-pojo) via la bibliothèque `ObjectMapper` Jackson. Le POJO d’image est exploré ci-dessous.

Enfin, le `src` du logo est chargé dans l’ImageView d’Android à l’aide de la bibliothèque d’assistance [!DNL Glide].

Notez que nous devons fournir le schéma, l’hôte et le port d’AEM (via `aemHost`) à l’instance de publication AEM, car AEM Content Services ne fournira que le chemin d’accès JCR (c’est-à-dire `/content/dam/wknd-mobile/images/wknd-logo.png`) au contenu référencé.

#### POJO d’image{#image-pojo}

Bien que cela soit facultatif, l’utilisation de l’[ObjectMapper Jackson](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou des fonctionnalités similaires fournies par d’autres bibliothèques comme Gson, permettent de mapper des structures JSON complexes à des POJO Java sans avoir à traiter directement les objets JSON natifs eux-mêmes. Dans ce cas simple, nous mappons la clé `src` de l’objet JSON `image` à l’attribut `src` dans le POJO d’image, directement via l’annotation `@JSONProperty`.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

Le POJO d’événement, qui nécessite de sélectionner beaucoup plus de points de données à partir de l’objet JSON, bénéficie de cette technique plus que le POJO d’image, dont nous voulons simplement le `src`.

## Explorer l’expérience de l’application mobile

Maintenant que vous comprenez comment AEM Content Services peut générer une expérience mobile native, utilisez ce que vous avez appris pour effectuer les étapes suivantes et voir vos modifications répercutées dans l’application mobile.

Après chaque étape, actualisez l’application mobile en balayant du haut de l’écran vers le bas et vérifiez la mise à jour de l’expérience mobile.

1. Créer et publier **un nouveau fragment de contenu d’[!DNL Event]**
1. Annuler la publication d’un **fragment de contenu d’[!DNL Event] existant**
1. Publier une mise à jour sur la **ligne de balise**

## Félicitations.

**Vous avez terminé le tutoriel AEM Headless.**

Pour en savoir plus sur AEM Content Services et AEM en tant que CMS découplé, consultez d’autres documentations et supports de formation d’Adobe :

* [Utiliser les fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guide d’utilisation des composants principaux du gestionnaire de contenu web d’AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [Bibliothèque de composants principaux du gestionnaire de contenu web d’AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Projet GitHub des composants principaux du gestionnaire de contenu web d’AEM](https://github.com/adobe/aem-core-wcm-components)
* [Exemple de code pour l’exporteur de composants](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
