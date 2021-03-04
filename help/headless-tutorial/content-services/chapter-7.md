---
title: Chapitre 7 - Consommation de AEM Content Services à partir d’une application mobile - Content Services
description: Le chapitre 7 du didacticiel exécute l'application mobile Android pour utiliser le contenu créé à partir d'AEM Content Services.
feature: Fragments de contenu, API
topic: Sans tête, Gestion de contenu
role: Développeur
level: Début
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 1%

---


# Chapitre 7 - Consommation de AEM Content Services à partir d&#39;une application mobile

Le chapitre 7 du didacticiel utilise une application Android Mobile native pour consommer du contenu provenant d’AEM Content Services.

## Application mobile Android

Ce didacticiel utilise une **application mobile Android native simple** pour consommer et afficher du contenu de Événement exposé par AEM Content Services.

L&#39;utilisation de [Android](https://developer.android.com/) n&#39;a guère d&#39;importance et l&#39;application mobile consommatrice peut être écrite dans n&#39;importe quelle structure pour n&#39;importe quelle plate-forme mobile, par exemple iOS.

Android est utilisé pour les didacticiels en raison de la possibilité d&#39;exécuter un émulateur Android sur Windows, macOs et Linux, sa popularité, et qu&#39;il peut être écrit en tant que Java, un langage bien compris par les développeurs AEM.

*L&#39;application mobile Android du didacticiel ****ne vise pas à expliquer comment créer des applications mobiles Android ou transmettre les meilleures pratiques de développement Android, mais plutôt à illustrer comment AEM Content Services peut être utilisé à partir d&#39;une application mobile.*

### AEM Content Services génère l&#39;expérience des applications mobiles

![Mappage de l’application mobile vers Content Services](assets/chapter-7/content-services-mapping.png)

1. **logo** tel que défini par le [!DNL Events API] composant **Image** de la page.
1. La ligne de balise **** telle que définie sur le [!DNL Events API] composant de texte **de la page**.
1. Cette **liste de Événement** est dérivée de la sérialisation des fragments de contenu de Événement, exposés via le composant **Liste de fragment de contenu** configuré.

## Démonstration des applications mobiles

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuration de l’application mobile pour une utilisation non localhost

Si AEM Publish n’est pas exécuté sur **http://localhost:4503**, l’hôte et le port peuvent être mis à jour dans le [!DNL Settings] de l’application mobile afin de pointer vers la propriété hôte/port AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Exécution locale de l’application mobile

1. Téléchargez et installez l’[Android Studio](https://developer.android.com/studio/install) pour installer l’émulateur Android.
1. **** Téléchargez le  [!DNL APK] fichier Android  [GitHub > Ressources > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Ouvrez **Android Studio**
   * Lors du lancement initial d’Android Studio, une invite d’installation de [!DNL Android SDK] s’affiche. Acceptez les valeurs par défaut et terminez l’installation.
1. Ouvrez Android Studio et sélectionnez **Profil ou Debug APK**
1. Sélectionnez le fichier APK (**wknd-mobile.x.x.x.apk**) téléchargé à l’étape 2 et cliquez sur **OK**.
   * Si vous êtes invité à **Créer un nouveau dossier** ou **Utiliser un dossier existant**, sélectionnez **Utiliser un dossier existant**.
1. Lors du lancement initial d’Android Studio, cliquez avec le bouton droit sur le **wknd-mobile.x.x.x** dans la liste Projets, puis sélectionnez **Ouvrir les paramètres du module**.
   * Sous **Modules > wknd-mobile.x.x.x > onglet Dépendances**, sélectionnez **Plate-forme API 29 Android**. Appuyez sur OK pour fermer et enregistrer les modifications.
   * Si vous ne le faites pas, une erreur &quot;Veuillez sélectionner le SDK Android&quot; s’affichera lors du lancement de l’émulateur.
1. Ouvrez le **Gestionnaire AVD** en sélectionnant **Outils > Gestionnaire AVD** ou en appuyant sur l&#39;icône **Gestionnaire AVD** dans la barre supérieure.
1. Dans la fenêtre **AVD Manager**, cliquez sur **+ Créer un périphérique virtuel...** si le périphérique n&#39;est pas déjà enregistré.
   1. Dans la partie gauche, sélectionnez la catégorie **Téléphone**.
   1. Sélectionnez un **pixel 2**.
   1. Cliquez sur le bouton **Suivant**.
   1. Sélectionnez **Q** avec **API Niveau 29**.
      * Lors du lancement initial d’AVD Manager, vous serez invité à télécharger l’API avec version. Cliquez sur le lien Télécharger en regard de la version &quot;Q&quot;, puis terminez le téléchargement et l’installation.
   1. Cliquez sur le bouton **Suivant**.
   1. Cliquez sur le bouton **Terminer**.
1. Fermez la fenêtre **Gestionnaire AVD**.
1. Dans la barre de menus supérieure, sélectionnez **wknd-mobile.x.x.x** dans la liste déroulante **Exécuter/modifier les configurations**.
1. Appuyez sur le bouton **Exécuter** en regard du **Exécuter/modifier la configuration** sélectionné.
1. Dans la fenêtre contextuelle, sélectionnez le périphérique virtuel **[!DNL Pixel 2 API 29]** nouvellement créé et appuyez sur **OK**.
1. Si l&#39;application [!DNL WKND Mobile] ne se charge pas immédiatement, recherchez et appuyez sur l&#39;icône **[!DNL WKND]** de l&#39;écran d&#39;accueil Android dans l&#39;émulateur.
   * Si l&#39;émulateur démarre mais que l&#39;écran de l&#39;émulateur reste noir, appuyez sur le bouton **power** dans la fenêtre d&#39;outils de l&#39;émulateur en regard de la fenêtre de l&#39;émulateur.
   * Pour faire défiler le périphérique virtuel, cliquez et maintenez-le enfoncé et faites glisser.
   * Pour actualiser le contenu à partir de l’AEM, descendez du haut jusqu’à l’icône Actualiser.
s’affiche et est publié.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## Code d’application mobile

Cette section présente le code de l’application mobile Android qui interagit le plus et dépend de AEM Content Services et de sa sortie JSON.

Lors du chargement, l’application mobile effectue `HTTP GET` sur `/content/wknd-mobile/en/api/events.model.json`, qui est le point de terminaison AEM Content Services configuré pour fournir le contenu permettant de piloter l’application mobile.

Le modèle modifiable de l’API Événements (`/content/wknd-mobile/en/api/events.model.json`) étant verrouillé, l’application mobile peut être codée pour rechercher des informations spécifiques à des emplacements spécifiques dans la réponse JSON.

### Flux de code de haut niveau

1. L’ouverture de l’application [!DNL WKND Mobile] appelle une requête `HTTP GET` à l’éditeur AEM à `/content/wknd-mobile/en/api/events.model.json` pour collecter le contenu à utiliser pour remplir l’interface utilisateur de l’application mobile.
2. A la réception du contenu de l&#39;AEM, chacun des trois éléments de vue de l&#39;application mobile, le **logo, la ligne de balise et la liste de événement**, sont initialisés avec le contenu de l&#39;AEM.
   * Pour lier le contenu AEM à l’élément de vue de l’application mobile, le JSON représentant chaque composant AEM est un objet mappé à un POJO Java, qui est à son tour lié à l’élément de Vue Android.
      * Composant d&#39;image JSON → Logo POJO → Logo ImageView
      * Composant de texte JSON → TagLine POJO → Text ImageView
      * Liste de fragment de contenu JSON → Événements POJO → Événements RecyclerView
   * *Le code de l’application mobile permet de mapper le fichier JSON aux objets POJO en raison des emplacements bien connus dans la réponse JSON la plus élevée. N’oubliez pas que les clés JSON de &quot;image&quot;, &quot;texte&quot; et &quot;contentfragmentlist&quot; sont dictées par le nom de noeud des composants AEM de support. Si ces noms de noeud changent, l&#39;application mobile se rompt car elle ne sait pas comment sources le contenu requis à partir des données JSON.*

#### Appel du point de terminaison AEM Content Services

Vous trouverez ci-dessous une distillation du code dans le `MainActivity` de l’application mobile, responsable de l’appel d’AEM Content Services pour collecter le contenu qui motive l’expérience de l’application mobile.

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

`onCreate(..)` est le hook d’initialisation de l’application mobile et enregistre les 3 éléments personnalisés  `ViewBinders` chargés d’analyser le fichier JSON et de lier les valeurs aux  `View` éléments.

`initApp(...)` est ensuite appelée, ce qui envoie la demande de GET HTTP au point de terminaison AEM Content Services sur AEM Publish pour collecter le contenu. A la réception d’une réponse JSON valide, la réponse JSON est transmise à chaque `ViewBinder` responsable de l’analyse du fichier JSON et de sa liaison aux éléments mobiles `View`.

#### Analyse de la réponse JSON

Nous allons maintenant examiner le `LogoViewBinder`, qui est simple, mais qui met en évidence plusieurs considérations importantes.

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

La première ligne de `bind(...)` navigue vers la réponse JSON via les clés **:items → racine → :items** qui représente le Conteneur de mise en page AEM auquel les composants ont été ajoutés.

A partir de là, une vérification est faite pour une clé nommée **image**, qui représente le composant Image (encore une fois, il est important que ce nom de noeud → clé JSON est stable). Si cet objet existe, il est lu et mappé à l&#39;[Image personnalisée POJO](#image-pojo) par l&#39;intermédiaire de la bibliothèque Jackson `ObjectMapper`. L&#39;Image POJO est explorée ci-dessous.

Enfin, le logo `src` est chargé dans l&#39;Android ImageView à l&#39;aide de la bibliothèque d&#39;aide [!DNL Glide].

Notez que nous devons fournir l’schéma, l’hôte et le port AEM (via `aemHost`) à l’instance de publication AEM car AEM Content Services ne fournira que le chemin d’accès au JCR (c.-à-d. `/content/dam/wknd-mobile/images/wknd-logo.png`) au contenu référencé.

#### Image POJO{#image-pojo}

Bien qu’elle soit facultative, l’utilisation de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou de fonctionnalités similaires fournies par d’autres bibliothèques comme Gson permet de mapper des structures JSON complexes à des POJO Java sans avoir à traiter directement avec les objets JSON natifs eux-mêmes. Dans ce cas simple, nous associons la clé `src` de l’objet JSON `image` à l’attribut `src` dans le POJO de l’image directement via l’annotation `@JSONProperty`.

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

Le Événement POJO, qui nécessite de sélectionner beaucoup plus de points de données à partir de l&#39;objet JSON, bénéficie de cette technique plus que la simple Image, que nous ne voulons que le `src`.

## Explorez l&#39;expérience des applications mobiles

Maintenant que vous savez comment AEM Content Services peut générer une expérience Mobile native, utilisez ce que vous avez appris pour effectuer les étapes suivantes et voir vos modifications répercutées dans l’application mobile.

Après chaque étape, effectuez l’actualisation de l’application mobile et vérifiez la mise à jour de l’expérience mobile.

1. Créer et publier **un nouveau fragment de contenu [!DNL Event]**
1. Annulation de la publication d&#39;un **fragment de contenu [!DNL Event] existant**
1. Publier une mise à jour de la ligne de balise ****

## Félicitations

**Vous avez terminé avec le tutoriel AEM sans tête !**

Pour en savoir plus sur AEM Content Services et AEM en tant que CMS sans en-tête, consultez la documentation et le matériel d’activation du Adobe :

* [Utilisation de fragments de contenu](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guide de l’utilisateur des composants principaux de la gestion de contenu Web AEM](https://docs.adobe.com/content/help/fr-FR/experience-manager-core-components/using/introduction.html)
* [Bibliothèque de composants principaux de la gestion de l&#39;environnement de développement](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Projet GitHub des composants principaux de la gestion de contenu Web AEM](https://github.com/adobe/aem-core-wcm-components)
* [AEM Composants de base de la gestion de la gestion des ressources humaines - Demandez à l&#39;expert](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Exemple de code de l’exportateur de composants](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
