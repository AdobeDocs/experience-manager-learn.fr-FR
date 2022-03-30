---
title: Chapitre 7 - Consommation AEM Content Services à partir d’une application Mobile - Content Services
description: Le chapitre 7 du tutoriel exécute l’application Android Mobile pour utiliser du contenu créé à partir d’AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '1392'
ht-degree: 1%

---

# Chapitre 7 - Consommation AEM Content Services à partir d’une application mobile

Le chapitre 7 du tutoriel utilise une application Android Mobile native pour consommer du contenu d’AEM Content Services.

## Application Mobile Android

Ce tutoriel utilise une **application Android Mobile native simple** pour utiliser et afficher le contenu d’événement exposé par AEM Content Services.

L’utilisation de [Android](https://developer.android.com/) n’est pas importante, et l’application mobile consommatrice peut être écrite dans n’importe quelle structure pour n’importe quelle plateforme mobile, par exemple iOS.

Android est utilisé pour les tutoriels en raison de la possibilité d’exécuter un émulateur Android sous Windows, macOs et Linux, sa popularité, et du fait qu’il peut être écrit en tant que Java, une langue bien comprise par les développeurs AEM.

*L’application Mobile Android du tutoriel est **not**destiné à expliquer comment créer des applications Mobile Android ou transmettre les bonnes pratiques de développement Android, mais plutôt à illustrer comment AEM Content Services peut être utilisé à partir d’une application Mobile.*

### Comment AEM Content Services génère l’expérience de l’application Mobile

![Mappage de l’application Mobile vers Content Services](assets/chapter-7/content-services-mapping.png)

1. Le **logo** comme défini par la variable [!DNL Events API] de la page **Composant d’image**.
1. Le **ligne de balise** comme défini sur la variable [!DNL Events API] de la page **Composant textuel**.
1. Ceci **Liste des événements** est dérivé de la sérialisation des fragments de contenu d’événement, exposés via le **Composant Liste de fragments de contenu**.

## Démonstration de l’application Mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuration de l’application Mobile pour une utilisation autre que localhost

Si AEM Publish n’est pas exécuté sur **http://localhost:4503** l’hôte et le port peuvent être mis à jour dans l’application Mobile [!DNL Settings] pour pointer vers la propriété hôte/port de publication AEM.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Exécution locale de l’application Mobile

1. Téléchargez et installez le [Android Studio](https://developer.android.com/studio/install) pour installer l’émulateur Android.
1. **Télécharger** Android [!DNL APK] fichier [GitHub > Ressources > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Ouvrir **Android Studio**
   * Au lancement initial d’Android Studio, une invite vous invite à installer le [!DNL Android SDK] sera présent. Acceptez les valeurs par défaut et terminez l’installation.
1. Ouvrez Android Studio et sélectionnez **APK Profile ou Debug**
1. Sélectionnez le fichier APK (**wknd-mobile.x.x.apk**) téléchargé à l’étape 2, puis cliquez sur **OK**
   * Si vous êtes invité à **Création d’un dossier** ou **Utiliser Existant**, sélectionnez **Utiliser Existant**.
1. Lors du lancement initial d’Android Studio, cliquez avec le bouton droit de la souris sur le **wknd-mobile.x.x.x** dans la liste Projets, puis sélectionnez **Ouvrir les paramètres du module**.
   * Sous **Modules > wknd-mobile.x.x.x > Onglet Dépendances**, sélectionnez **Plateforme d’API Android 29**. Appuyez sur OK pour fermer et enregistrer les modifications.
   * Si vous ne le faites pas, une erreur &quot;Veuillez sélectionner le SDK Android&quot; s’affichera lors du lancement de l’émulateur.
1. Ouvrez le **Gestionnaire AVD** en sélectionnant **Outils > Gestionnaire AVD** ou en appuyant sur le bouton **Gestionnaire AVD** dans la barre supérieure.
1. Dans le **Gestionnaire AVD** fenêtre, cliquez sur **+ Créer un périphérique virtuel...** si le périphérique n’est pas déjà enregistré.
   1. Dans la partie gauche, sélectionnez le **Téléphone** catégorie.
   1. Sélectionnez une **Pixel 2**.
   1. Cliquez sur le bouton **Suivant** bouton .
   1. Sélectionner **Q** avec **Niveau d’API 29**.
      * Lors du lancement initial d’AVD Manager, vous serez invité à télécharger l’API versionnée. Cliquez sur le lien Télécharger en regard de la version &quot;Q&quot;, puis effectuez le téléchargement et l’installation.
   1. Cliquez sur le bouton **Suivant** bouton .
   1. Cliquez sur le bouton **Terminer** bouton .
1. Fermez la **Gestionnaire AVD** fenêtre.
1. Dans la barre de menu supérieure, sélectionnez **wknd-mobile.x.x.x** de la **Exécuter/modifier des configurations** déroulant.
1. Appuyez sur le bouton **Exécuter** en regard du bouton sélectionné **Exécution/modification de la configuration**
1. Dans la fenêtre contextuelle, sélectionnez la **[!DNL Pixel 2 API 29]** périphérique virtuel et appuyez sur **OK**
1. Si la variable [!DNL WKND Mobile] L’application ne se charge pas immédiatement, ne trouve pas et n’appuie pas sur la **[!DNL WKND]** de l’écran d’accueil Android dans l’émulateur.
   * Si l’émulateur se lance mais que l’écran de l’émulateur reste noir, appuyez sur la touche **power** dans la fenêtre des outils de l’émulateur en regard de la fenêtre de l’émulateur.
   * Pour faire défiler l’appareil virtuel, maintenez la touche enfoncée et faites glisser.
   * Pour actualiser le contenu à partir d’AEM, effectuez une descente depuis le haut jusqu’à ce que l’icône Actualiser s’affiche, puis lancez .

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Code de l’application Mobile

Cette section décrit le code de l’application Mobile Android qui interagit le plus et qui dépend d’AEM Content Services et de sa sortie JSON.

Au chargement, l’application Mobile effectue les `HTTP GET` to `/content/wknd-mobile/en/api/events.model.json` qui est le point d’entrée AEM Content Services configuré pour fournir le contenu permettant de piloter l’application Mobile.

Parce que le modèle modifiable de l’API Events (`/content/wknd-mobile/en/api/events.model.json`) est verrouillée, l’application Mobile peut être codée pour rechercher des informations spécifiques à des emplacements spécifiques dans la réponse JSON.

### Flux de code de haut niveau

1. Ouverture de la [!DNL WKND Mobile] L’application appelle une `HTTP GET` demande à AEM Publish à l’adresse `/content/wknd-mobile/en/api/events.model.json` pour collecter le contenu afin de renseigner l’interface utilisateur de l’application Mobile.
2. Lors de la réception du contenu d’AEM, chacun des trois éléments d’affichage de l’application Mobile, la variable **logo, ligne de balise et liste d’événements**, sont initialisés avec le contenu d’AEM.
   * Pour lier le contenu AEM à l’élément d’affichage de l’application Mobile, le JSON représentant chaque composant AEM est un objet mappé à un POJO Java, qui est à son tour lié à l’élément d’affichage Android.
      * Composant d’image JSON → Logo POJO → Logo ImageView
      * Composant Texte JSON → TagLine POJO → Text ImageView
      * Liste de fragments de contenu JSON → Événements POJO →Events RecyclerView
   * *Le code de l’application Mobile peut mapper le JSON aux POJO en raison des emplacements bien connus dans la réponse JSON supérieure. N’oubliez pas que les clés JSON de &quot;image&quot;, &quot;text&quot; et &quot;contentfragmentlist&quot; sont dictées par les noms de noeud des composants de l’AEM de support. Si ces noms de noeud changent, l’application Mobile se brise, car elle ne sait pas comment sources le contenu requis à partir des données JSON.*

#### Appel du point d’entrée Content Services AEM

Vous trouverez ci-dessous un condensé du code dans l’application Mobile `MainActivity` chargé d’appeler AEM Content Services pour collecter le contenu qui entraîne l’expérience de l’application Mobile.

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

`onCreate(..)` est le crochet d’initialisation de l’application Mobile et enregistre les 3 `ViewBinders` responsable de l’analyse du fichier JSON et de la liaison des valeurs à la variable `View` éléments .

`initApp(...)` est ensuite appelée, ce qui envoie la requête de GET HTTP au point de terminaison AEM Content Services sur AEM Publish pour collecter le contenu. Lors de la réception d’une réponse JSON valide, la réponse JSON est transmise à chaque `ViewBinder` qui est chargé d’analyser le fichier JSON et de le lier au fichier mobile ; `View` éléments .

#### Analyse de la réponse JSON

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

La première ligne de `bind(...)` navigue vers le bas de la réponse JSON à l’aide des clés ; **:items → root →:items** qui représente le conteneur de mise en page AEM auquel les composants ont été ajoutés.

A partir de là, une vérification pour une clé nommée **image**, qui représente le composant Image (il est important que ce nom de noeud → clé JSON soit stable). Si cet objet existe, il est lu et mappé à l’objet [POJO d’image personnalisée](#image-pojo) via le Jackson `ObjectMapper` bibliothèque . Le POJO de l&#39;image est exploré ci-dessous.

Enfin, le logo `src` est chargé dans Android ImageView à l’aide de la variable [!DNL Glide] bibliothèque d’assistance.

Notez que nous devons fournir le schéma AEM, l’hôte et le port (via `aemHost`) à l’instance de publication AEM en tant qu’AEM Content Services ne fournira que le chemin d’accès JCR (c.-à-d. `/content/dam/wknd-mobile/images/wknd-logo.png`) au contenu référencé.

#### POJO{#image-pojo}

Bien que cela soit facultatif, l’utilisation de la variable [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou des fonctionnalités similaires fournies par d’autres bibliothèques comme Gson, permettent de mapper des structures JSON complexes à des POJO Java sans avoir à traiter directement les objets JSON natifs eux-mêmes. Dans ce cas simple, nous mettons en correspondance la variable `src` de la fonction `image` Objet JSON, sur la propriété `src` dans le POJO de l’image, directement via la propriété `@JSONProperty` annotation.

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

Le POJO de l’événement, qui nécessite de sélectionner beaucoup plus de points de données à partir de l’objet JSON, bénéficie de cette technique plus que la simple image, que nous voulons tout simplement : `src`.

## Exploration de l’expérience de l’application Mobile

Maintenant que vous savez comment AEM Content Services peut générer une expérience Mobile native, utilisez ce que vous avez appris pour effectuer les étapes suivantes et voir vos modifications répercutées dans l’application Mobile.

Après chaque étape, effectuez une extraction pour actualiser l’application Mobile et vérifiez la mise à jour de l’expérience mobile.

1. Créer et publier **new [!DNL Event] Fragment de contenu**
1. Annulation de la publication d’une **existant [!DNL Event] Fragment de contenu**
1. Publier une mise à jour sur le **Ligne de balise**

## Félicitations

**Vous avez terminé avec le tutoriel AEM sans tête !**

Pour en savoir plus sur AEM Content Services et AEM as a Headless CMS, consultez la documentation et les documents d’activation de l’Adobe :

* [Utilisation de fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guide de l’utilisateur des composants principaux WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=fr)
* [Bibliothèque de composants principaux de la gestion du contenu web AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM projet GitHub des composants principaux WCM](https://github.com/adobe/aem-core-wcm-components)
* [Exemple de code pour l’exportateur de composants](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
