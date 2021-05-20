---
title: Chapitre 7 - Consommation AEM Content Services à partir d’une application mobile - Content Services
description: Le chapitre 7 du tutoriel exécute l’application mobile Android pour utiliser du contenu créé à partir d’AEM Content Services.
feature: Fragments de contenu, API
topic: Sans affichage, gestion de contenu
role: Developer
level: Beginner
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1414'
ht-degree: 1%

---


# Chapitre 7 - Consommation AEM Content Services à partir d’une application mobile

Le chapitre 7 du tutoriel utilise une application mobile Android native pour consommer du contenu d’AEM Content Services.

## Application mobile Android

Ce tutoriel utilise une **application mobile Android native simple** pour consommer et afficher le contenu d’événement exposé par AEM Content Services.

L’utilisation d’[Android](https://developer.android.com/) n’a guère d’importance, et l’application mobile consommatrice peut être écrite dans n’importe quelle structure pour n’importe quelle plateforme mobile, par exemple iOS.

Android est utilisé pour les tutoriels en raison de la possibilité d’exécuter un émulateur Android sous Windows, macOs et Linux, sa popularité, et du fait qu’il peut être écrit en tant que Java, une langue bien comprise par les développeurs AEM.

*L’application mobile Android du tutoriel ****ne vise pas à expliquer comment créer des applications mobiles Android ou transmettre les bonnes pratiques de développement Android, mais plutôt à illustrer comment AEM Content Services peut être utilisé à partir d’une application mobile.*

### Comment AEM Content Services génère l’expérience de l’application mobile

![Mappage de l’application mobile vers Content Services](assets/chapter-7/content-services-mapping.png)

1. **logo** tel que défini par le [!DNL Events API] composant **Image de la page**.
1. **ligne de balise** telle que définie sur le [!DNL Events API] composant **Texte de la page**.
1. Cette **liste d’événements** est dérivée de la sérialisation des fragments de contenu d’événement, exposés via le **composant Liste de fragments de contenu** configuré.

## Démonstration de l’application mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuration de l’application mobile pour une utilisation autre que localhost

Si la publication AEM n’est pas exécutée sur **http://localhost:4503**, l’hôte et le port peuvent être mis à jour dans la balise [!DNL Settings] de l’application mobile pour pointer vers la propriété hôte/port de publication AEM.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Exécution locale de l’application mobile

1. Téléchargez et installez [Android Studio](https://developer.android.com/studio/install) pour installer l’émulateur Android.
1. **** Téléchargez le  [!DNL APK] fichier Android  [GitHub > Ressources > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Ouvrez **Android Studio**
   * Au lancement initial d’Android Studio, une invite s’affiche pour installer [!DNL Android SDK]. Acceptez les valeurs par défaut et terminez l’installation.
1. Ouvrez Android Studio et sélectionnez **Profil ou Débogage APK**
1. Sélectionnez le fichier APK (**wknd-mobile.x.x.x.apk**) téléchargé à l’étape 2 et cliquez sur **OK**
   * Si vous êtes invité à **Créer un dossier** ou **Utiliser la balise** existante, sélectionnez **Utiliser la balise** existante.
1. Au lancement initial d’Android Studio, cliquez avec le bouton droit de la souris sur **wknd-mobile.x.x.x** dans la liste Projets, puis sélectionnez **Ouvrir les paramètres du module**.
   * Sous **Modules > wknd-mobile.x.x.x > Onglet Dépendances**, sélectionnez **Plateforme d’API Android 29**. Appuyez sur OK pour fermer et enregistrer les modifications.
   * Si vous ne le faites pas, une erreur &quot;Veuillez sélectionner le SDK Android&quot; s’affichera lors du lancement de l’émulateur.
1. Ouvrez le **Gestionnaire AVD** en sélectionnant **Outils > Gestionnaire AVD** ou en appuyant sur l’icône **Gestionnaire AVD** dans la barre supérieure.
1. Dans la fenêtre **Gestionnaire AVD**, cliquez sur **+ Créer un périphérique virtuel...** si le périphérique n’est pas déjà enregistré.
   1. Dans la partie gauche, sélectionnez la catégorie **Téléphone**.
   1. Sélectionnez un **pixel 2**.
   1. Cliquez sur le bouton **Suivant** .
   1. Sélectionnez **Q** avec **API Niveau 29**.
      * Lors du lancement initial d’AVD Manager, vous serez invité à télécharger l’API versionnée. Cliquez sur le lien Télécharger en regard de la version &quot;Q&quot;, puis effectuez le téléchargement et l’installation.
   1. Cliquez sur le bouton **Suivant** .
   1. Cliquez sur le bouton **Terminer** .
1. Fermez la fenêtre **Gestionnaire AVD** .
1. Dans la barre de menu supérieure, sélectionnez **wknd-mobile.x.x.x** dans la liste déroulante **Exécuter/modifier les configurations**.
1. Appuyez sur le bouton **Exécuter** en regard de l’option **Exécuter/modifier la configuration** sélectionnée.
1. Dans la fenêtre contextuelle, sélectionnez le nouvel appareil virtuel **[!DNL Pixel 2 API 29]** et appuyez sur **OK**
1. Si l’application [!DNL WKND Mobile] ne se charge pas immédiatement, recherchez l’icône **[!DNL WKND]** sur l’écran d’accueil d’Android dans l’émulateur et appuyez dessus.
   * Si l’émulateur se lance mais que l’écran de l’émulateur reste noir, appuyez sur le bouton **power** dans la fenêtre des outils de l’émulateur en regard de la fenêtre de l’émulateur.
   * Pour faire défiler l’appareil virtuel, maintenez la touche enfoncée et faites glisser.
   * Pour actualiser le contenu à partir d’AEM, effectuez une descente depuis le haut jusqu’à l’icône Actualiser .
s’affiche et est publié.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Code de l’application mobile

Cette section décrit le code de l’application mobile Android qui interagit le plus et qui dépend d’AEM Content Services et de sa sortie JSON.

Au chargement, l’application mobile envoie `HTTP GET` vers `/content/wknd-mobile/en/api/events.model.json`, qui est le point d’entrée AEM Content Services configuré pour fournir le contenu permettant de piloter l’application mobile.

Étant donné que le modèle modifiable de l’API Events (`/content/wknd-mobile/en/api/events.model.json`) est verrouillé, l’application mobile peut être codée pour rechercher des informations spécifiques à des emplacements spécifiques dans la réponse JSON.

### Flux de code de haut niveau

1. L’ouverture de l’application [!DNL WKND Mobile] appelle une requête `HTTP GET` à la publication AEM à l’adresse `/content/wknd-mobile/en/api/events.model.json` pour collecter le contenu afin de renseigner l’interface utilisateur de l’application mobile.
2. Lors de la réception du contenu d’AEM, chacun des trois éléments d’affichage de l’application mobile, le **logo, la ligne de balise et la liste d’événements**, sont initialisés avec le contenu d’AEM.
   * Pour lier le contenu AEM à l’élément d’affichage de l’application mobile, le JSON représentant chaque composant AEM est un objet mappé à un POJO Java, qui est à son tour lié à l’élément d’affichage Android.
      * Composant d’image JSON → Logo POJO → Logo ImageView
      * Composant Texte JSON → TagLine POJO → Text ImageView
      * Liste de fragments de contenu JSON → Événements POJO →Events RecyclerView
   * *Le code de l’application mobile peut mapper le JSON aux POJO en raison des emplacements bien connus dans la réponse JSON supérieure. N’oubliez pas que les clés JSON de &quot;image&quot;, &quot;text&quot; et &quot;contentfragmentlist&quot; sont dictées par les noms de noeud des composants de l’AEM de support. Si ces noms de noeud changent, l’application mobile se brise, car elle ne sait pas comment sources le contenu requis à partir des données JSON.*

#### Appel du point d’entrée Content Services AEM

Vous trouverez ci-dessous un condensé du code dans la section `MainActivity` de l’application mobile chargée d’appeler AEM Content Services pour collecter le contenu qui anime l’expérience de l’application mobile.

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

`onCreate(..)` est le point d’extension d’initialisation de l’application mobile et enregistre les 3 éléments personnalisés  `ViewBinders` chargés d’analyser le fichier JSON et de lier les valeurs aux  `View` éléments .

`initApp(...)` est ensuite appelée, ce qui envoie la requête de GET HTTP au point de terminaison AEM Content Services sur AEM Publish pour collecter le contenu. Lors de la réception d’une réponse JSON valide, la réponse JSON est transmise à chaque `ViewBinder` responsable de l’analyse du fichier JSON et de sa liaison aux éléments mobiles `View`.

#### Analyse de la réponse JSON

Nous allons maintenant examiner le `LogoViewBinder`, qui est simple, mais qui met en évidence plusieurs points importants.

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

La première ligne de `bind(...)` descend la réponse JSON via les clés **:items → root →:items** qui représente le conteneur de mise en page AEM auquel les composants ont été ajoutés.

A partir de là, une vérification est effectuée pour une clé nommée **image**, qui représente le composant Image (là encore, il est important que ce nom de noeud → clé JSON soit stable). Si cet objet existe, il est lu et mappé sur le [POJO d’image personnalisé](#image-pojo) via la bibliothèque `ObjectMapper` Jackson. Le POJO de l&#39;image est exploré ci-dessous.

Enfin, la balise `src` du logo est chargée dans l’affichage d’image Android à l’aide de la bibliothèque [!DNL Glide] d’assistance.

Notez que nous devons fournir le schéma AEM, l’hôte et le port (via `aemHost`) à l’instance de publication AEM, car AEM Content Services ne fournira que le chemin JCR (c’est-à-dire. `/content/dam/wknd-mobile/images/wknd-logo.png`) au contenu référencé.

#### Image POJO{#image-pojo}

Bien que cela soit facultatif, l’utilisation de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou de fonctionnalités similaires fournies par d’autres bibliothèques comme Gson permet de mapper des structures JSON complexes à des POJO Java sans avoir à traiter directement les objets JSON natifs eux-mêmes. Dans ce cas simple, nous associons la clé `src` de l’objet `image` JSON à l’attribut `src` du POJO de l’image directement via l’annotation `@JSONProperty`.

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

Le POJO d’événement, qui nécessite de sélectionner beaucoup plus de points de données à partir de l’objet JSON, bénéficie de cette technique plus que de l’image simple, que nous voulons tout simplement : `src`.

## Exploration de l’expérience de l’application mobile

Maintenant que vous savez comment AEM Content Services peut générer une expérience mobile native, utilisez ce que vous avez appris pour effectuer les étapes suivantes et voir vos modifications répercutées dans l’application mobile.

Après chaque étape, effectuez une extraction pour actualiser l’application mobile et vérifiez la mise à jour de l’expérience mobile.

1. Créer et publier **nouveau [!DNL Event] fragment de contenu**
1. Annuler la publication d’un **fragment de contenu [!DNL Event] existant**
1. Publier une mise à jour sur la **ligne de balise**

## Félicitations

**Vous avez terminé avec le tutoriel AEM sans tête !**

Pour en savoir plus sur AEM Content Services et AEM as a Headless CMS, consultez la documentation et les documents d’activation de l’Adobe :

* [Utilisation de fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guide de l’utilisateur des composants principaux WCM AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [Bibliothèque de composants principaux de la gestion du contenu web AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM projet GitHub des composants principaux WCM](https://github.com/adobe/aem-core-wcm-components)
* [AEM Composants principaux de la gestion de contenu web - Demandez à l’expert](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Exemple de code pour l’exportateur de composants](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
