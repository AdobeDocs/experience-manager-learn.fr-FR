---
title: Tutoriel pour ajouter des balises de métadonnées spécifiées par l’utilisateur ou l’utilisatrice
description: Découvrez comment stocker et récupérer les données de formulaire adaptatif du compte de stockage Azure.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14501
duration: 34
exl-id: 94454327-86d9-468e-9f08-50b8a9c530f3
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '106'
ht-degree: 100%

---

# Étendre les composants de groupe de choix

Les composants principaux checkboxgroup, dropdown et radiobutton ont été étendus afin d’inclure un onglet Propriétés supplémentaires. L’onglet Propriétés supplémentaires comporte une case à cocher pour indiquer si le champ doit être utilisé comme onglet d’index d’objets blob.
![additonal-properties](assets/drop-down-additonal-properties.png). Lorsque la case est cochée, une propriété appelée Indexable est créée et sa valeur est définie sur true dans le référentiel jcr, comme illustré dans la copie d’écran suivante.
![searchable](assets/searchable-true.png).

Le fichier .content.xml suivant a été créé sous le dossier _cq_dialog.

![drop-down-project-view](assets/drop-down-project-view.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:sling="http://sling.apache.org/jcr/sling/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:nt="http://www.jcp.org/jcr/nt/1.0"
          jcr:primaryType="nt:unstructured"
          jcr:title="Check box group"
          sling:resourceType="cq/gui/components/authoring/dialog">
    <content
            jcr:primaryType="nt:unstructured"
            sling:resourceType="granite/ui/components/coral/foundation/container">
        <items jcr:primaryType="nt:unstructured">
            <tabs
                    jcr:primaryType="nt:unstructured"
                    sling:resourceType="granite/ui/components/coral/foundation/tabs"
                    maximized="{Boolean}false">
                <items jcr:primaryType="nt:unstructured">

                    <properties
                            jcr:primaryType="nt:unstructured"
                            jcr:title="Additional Properties"
                            sling:resourceType="granite/ui/components/coral/foundation/container"
                            margin="{Boolean}true">
                        <items jcr:primaryType="nt:unstructured">
                            <columns
                                    jcr:primaryType="nt:unstructured"
                                    sling:resourceType="granite/ui/components/coral/foundation/fixedcolumns"
                                    margin="{Boolean}true">
                                <items jcr:primaryType="nt:unstructured">
                                    <column
                                            jcr:primaryType="nt:unstructured"
                                            sling:resourceType="granite/ui/components/coral/foundation/container">
                                        <items jcr:primaryType="nt:unstructured">
                                            <Searchable
                                                    jcr:primaryType="nt:unstructured"
                                                    sling:resourceType="granite/ui/components/coral/foundation/form/checkbox"
                                                    emptyText="Want to include in search?"
                                                    fieldDescription="Indicate if you want to use in search"
                                                    text="Want to use this field in query"
                                                    value="{Boolean}true"
                                                    uncheckedValue="{Boolean}false"

                                                    name="./Searchable"
                                                    checked="{Boolean}false"
                                                    required="{Boolean}false"/>


                                        </items>
                                    </column>
                                </items>
                            </columns>
                        </items>
                    </properties>
                </items>
            </tabs>
        </items>
    </content>
</jcr:root>
```

## Étapes suivantes

[Créer une configuration de portail Azure](./create-osgi-configuration.md)
