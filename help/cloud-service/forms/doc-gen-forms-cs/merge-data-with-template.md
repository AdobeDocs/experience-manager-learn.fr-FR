---
title: Fusionner les données avec le modèle XDP
description: Effectuer une requête POST au point d’entrée avec les paramètres requis
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Output Service
topic: Development
jira: KT-8185
thumbnail: 332439.jpg
exl-id: d144b3f6-7c7a-46a7-bc5f-1767895749d0
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 100%

---

# Effectuer l’appel POST


L’étape suivante consiste à effectuer un appel POST HTTP au point d’entrée avec les paramètres nécessaires. Le modèle et les fichiers de données sont fournis sous forme de fichiers de ressources. Les propriétés du PDF généré sont spécifiées via le paramètre de l’option dans la requête. La propriété embedFonts permet d’incorporer des polices personnalisées dans le PDF généré.[Consultez cette documentation pour déployer des polices personnalisées sur votre instance cloud de Forms.](https://experienceleague.adobe.com/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/intellij-set-up.html?lang=fr) Les propriétés sont spécifiées dans le fichier de ressource options.json. Puisque le point d’entrée dispose d’une authentification basée sur les jetons, nous transmettons le jeton d’accès dans l’en-tête de la requête.

Le code suivant a été utilisé pour générer le PDF en fusionnant les données avec le modèle.

```java
public class DocumentGeneration
{
        public String SAVE_LOCATION = "c:\\aspire1";
        public void mergeDataWithXdpTemplate(String postURL)
        {
                HttpPost httpPost = new HttpPost(postURL);
                CredentialUtilites cu = new CredentialUtilites();
                String accessToken = cu.getAccessToken();
                httpPost.addHeader("Authorization", "Bearer " + accessToken);
                ClassLoader classLoader = DocumentGeneration.class.getClassLoader();
                URL templateFile = classLoader.getResource("templates/custom_fonts.xdp");
                File xdpTemplate = new File(templateFile.getPath());
                URL url = classLoader.getResource("datafiles");
                System.out.println(url.getPath());
                File files[] = new File(url.getPath()).listFiles();
                for (int i = 0; i < files.length; i++) {
                        MultipartEntityBuilder builder = MultipartEntityBuilder.create();
                        ContentType strContent = ContentType.create("text/plain", Charset.forName("UTF-8"));
                        builder.setMode(HttpMultipartMode.BROWSER_COMPATIBLE);
                        builder.addBinaryBody("data", files[i]);
                        builder.addBinaryBody("template", xdpTemplate);
                        builder.addBinaryBody("options",GetOptions.getPDFOptions().getBytes(),ContentType.APPLICATION_JSON,"options"
                        try {
                                HttpEntity entity = builder.build();
                                httpPost.setEntity(entity);
                                CloseableHttpClient httpclient = HttpClients.createDefault();
                                CloseableHttpResponse response = httpclient.execute(httpPost);
                                InputStream generatedPDF = response.getEntity().getContent();
                                byte[] bytes = IOUtils.toByteArray(generatedPDF);
                                File saveLocation = new File(SAVE_LOCATION);
                                if (!saveLocation.exists()) {
                                        saveLocation.mkdirs();
                                }
                                File outputFile = new File(SAVE_LOCATION+File.separator+files[i].getName().replace("xml", "pdf"));
                                try (FileOutputStream outputStream = new FileOutputStream(outputFile)) {
                                        outputStream.write(bytes);
                                }
                        } catch (Exception e) {
                                System.out.println("The error is " + e.getMessage());
                        }

                }
                System.out.println("Done generating " + files.length + " files");

        }

}
```
