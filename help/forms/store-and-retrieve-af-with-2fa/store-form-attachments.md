---
title: Stocker les pièces jointes d’un formulaire
description: Extrayez les pièces jointes du formulaire et stockez-les dans un nouvel emplacement dans le référentiel CRX.
feature: Adaptive Forms
type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-6537
thumbnail: 6537.jpg
topic: Development
role: Developer
level: Experienced
exl-id: ec50b9b1-e28c-4d84-ae90-6a21c9700688
duration: 65
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: ht
source-wordcount: '192'
ht-degree: 100%

---

# Stocker les pièces jointes d’un formulaire

Lorsque vous ajoutez des pièces jointes à un formulaire adaptatif, elles sont stockées dans un emplacement temporaire dans le référentiel CRX. Pour que notre cas d’utilisation fonctionne, nous devons stocker les pièces jointes de formulaire dans un nouvel emplacement dans le référentiel CRX.

Le service OSGi est créé pour stocker les pièces jointes de formulaire dans un nouvel emplacement dans le référentiel CRX. Un nouveau mappage de fichiers est créé avec le nouvel emplacement des pièces jointes dans le CRX et renvoyé à l’application appelante.
Voici le FileMap envoyé au servlet. La clé est le champ de formulaire adaptatif et la valeur est l’emplacement temporaire de la pièce jointe. Dans notre servlet, nous allons extraire la pièce jointe et la stocker dans un nouvel emplacement dans le référentiel AEM et mettre à jour le FileMap avec le nouvel emplacement.

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "idcard/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "tableItem11/BankStatement-Sept-2020.pdf"
}
```

Voici le code qui extrait les pièces jointes de la requête et les stocke sous le dossier **/content/afattachments**.

```java
public String storeAFAttachments(JSONObject fileMap, SlingHttpServletRequest request) {
    JSONObject newFileMap = new JSONObject();
    try {
        Iterator keys = fileMap.keys();
        log.debug("The file map is " + fileMap.toString());
        while (keys.hasNext()) {
            String key = (String) keys.next();
            log.debug("#### The key is " + key);
            String attacmenPath = (String) fileMap.get((key));
            log.debug("The attachment path is " + attacmenPath);
            if (!attacmenPath.contains("/content/afattachments")) {
                String fileName = attacmenPath.split("/")[1];
                log.debug("#### The attachment name  is " + fileName);
                InputStream is = request.getPart(attacmenPath).getInputStream();
                Document aemFDDocument = new Document(is);
                String crxPath = saveDocumentInCrx("/content/afattachments", fileName, aemFDDocument);
                log.debug(" ##### written to crx repository  " + attacmenPath.split("/")[1]);
                newFileMap.put(key, crxPath);
            } else {
                log.debug("$$$$ The attachment was already added " + key);
                log.debug("$$$$ The attachment path is " + attacmenPath);
                int position = attacmenPath.indexOf("//");
                log.debug("$$$$ After substring " + attacmenPath.substring(position + 1));
                log.debug("$$$$ After splitting " + attacmenPath.split("/")[1]);
                newFileMap.put(key, attacmenPath.substring(position + 1));
            }
        }
    } catch (Exception ex) {
        log.debug(ex.getMessage());
    }
    return newFileMap.toString();

}


}
```

Il s’agit du nouveau FileMap avec l’emplacement mis à jour des pièces jointes du formulaire.

```java
{
"guide[0].guide1[0].guideRootPanel[0].idDocumentPanel[0].idcard[0]": "/content/afattachments/7dc0cbde-404d-49a9-9f7b-9ab5ee7482be/CA-DriversLicense.pdf",
"guide[0].guide1[0].guideRootPanel[0].documentation[0].yourBankStatements[0].table1603552612235[0].Row1[0].tableItem11[0]": "/content/afattachments/81653de9-4967-4736-9ca3-807a11542243/BankStatement-Sept-2020.pdf"
}
```

## Étapes suivantes

[Enregistrer les données du formulaire](./store-form-data.md)