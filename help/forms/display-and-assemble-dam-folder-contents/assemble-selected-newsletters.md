---
title: Combinaison de newsletters sélectionnées dans un fichier
description: Combiner des newsletters sélectionnées à l’aide du service Assembler
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 3a64315f-f699-4538-b999-626e7a998c05
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 1%

---

# Combiner les newsletters sélectionnées en un pdf

Les sélections de l’utilisateur sont stockées dans un champ masqué. La valeur de ce champ masqué est transmise au servlet qui combine les sélections dans un pdf à l’aide de [Service Forms Assembler](https://developer.adobe.com/experience-manager/reference-materials/6-5/forms/javadocs/com/adobe/fd/assembler/service/AssemblerService.html).


## Servlet d’assemblage de fichiers pdf

Le code suivant effectue l’assemblage des newsletters sélectionnées. Le code crée une carte des documents à partir des sélections de l’utilisateur. À partir de cette carte, un DDX est créé et ce DDX ainsi que le mappage des documents sont transmis à la méthode invoke du service Assembler pour obtenir le document combiné. Le pdf assemblé est stocké dans le référentiel et son chemin d’accès est renvoyé à l’application appelante.

```java
protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
   {
   
       String []newsletters = request.getParameter("selectedNewsLetters").split(",");
       Map<String, Object> mapOfDocuments = new HashMap<String, Object>();
       for(int i= 0;i<newsletters.length;i++)
       {
           Resource resource = request.getResourceResolver().getResource(newsletters[i]);
           
           log.debug("The resource name is "+resource.getName());
           Document newsletter = new Document(resource.getPath());
           mapOfDocuments.put(resource.getName(), newsletter);
       }
       log.debug("The newsletters selected: "+newsletters);
       Document ddxDocument = createDDXFromMapOfDocuments(mapOfDocuments);
       AssemblerOptionSpec aoSpec = new AssemblerOptionSpec();
       aoSpec.setFailOnError(true);
       AssemblerResult ar = null;
       try {
               ar = assemblerService.invoke(ddxDocument, mapOfDocuments, aoSpec);
               Document assembledPDF = ar.getDocuments().get("GeneratedPDF.pdf");
               // This is my custom code to get fd-service user
               ResourceResolver formsServiceResolver = getResolver.getFormsServiceResolver();
               
               Resource nodeResource = formsServiceResolver.getResource("/content/newsletters");
           
               UUID uuid = UUID.randomUUID();
               String uuidString = uuid.toString();
               javax.jcr.Node assembledNewsletters = nodeResource.adaptTo(Node.class);
               javax.jcr.Node assembledNewsletter =  assembledNewsletters.addNode(uuidString + ".pdf", "nt:file");
               javax.jcr.Node resNode = assembledNewsletter.addNode("jcr:content", "nt:resource");
               ValueFactory valueFactory = formsServiceResolver.adaptTo(Session.class).getValueFactory();
               Binary contentValue = valueFactory.createBinary(assembledPDF.getInputStream());
               resNode.setProperty("jcr:data", contentValue);
               formsServiceResolver.commit();
               PrintWriter out = response.getWriter();
               response.setContentType("application/json");
               response.setCharacterEncoding("UTF-8");
               JsonObject asset = new JsonObject();
          
               asset.addProperty("assetPath", assembledNewsletter.getPath());
               out.print(new Gson().toJson(asset));
               out.flush();  
               
           } 
           catch (IOException | OperationException | RepositoryException e)
           {
           
               log.error("Error is "+e.getMessage());
           }
   }
```

## Fonctions Utiles

Les fonctions utilitaires suivantes ont été utilisées pour assembler les newsletters. Ces fonctions d’utilitaire créent DDX à partir du mappage de documents et convertissent l’objet de document org.w3c.dom.Document en AEMFD.


```java
public Document createDDXFromMapOfDocuments(Map<String, Object> mapOfDocuments)
     {
         
        final DocumentBuilderFactory docFactory = DocumentBuilderFactory.newInstance();
        org.w3c.dom.Document ddx = null;
        try
               {
                   DocumentBuilder docBuilder = docFactory.newDocumentBuilder();
                   ddx = docBuilder.newDocument();
                   Element rootElement = ddx.createElementNS("http://ns.adobe.com/DDX/1.0/", "DDX");
                ddx.appendChild(rootElement);
                Element pdfResult = ddx.createElement("PDF");
                pdfResult.setAttribute("result","GeneratedPDF.pdf");
                rootElement.appendChild(pdfResult);
                for (String key : mapOfDocuments.keySet())
                    {
                        log.debug(key + " " + mapOfDocuments.get(key));
                        Element pdfSourceElement = ddx.createElement("PDF");
                        pdfSourceElement.setAttribute("source", key);
                        pdfSourceElement.setAttribute("bookmarkTitle",key);
                        pdfResult.appendChild(pdfSourceElement);
                    }
                return orgw3cDocumentToAEMFDDocument(ddx);
            }
            catch (ParserConfigurationException e)
                {
                    log.debug("Error:"+e.getMessage());
                }
            return null;
     }
```

```java
public Document orgw3cDocumentToAEMFDDocument( org.w3c.dom.Document xmlDocument)
     {
         final ByteArrayOutputStream outputStream = new ByteArrayOutputStream();
         DOMSource source = new DOMSource(xmlDocument);
         log.debug("$$$$In orgW3CDocumentToAEMFDDocument method");
         StreamResult outputTarget = new StreamResult(outputStream);
         try
             {
               TransformerFactory.newInstance().newTransformer().transform(source, outputTarget);
               InputStream is1 = new ByteArrayInputStream(outputStream.toByteArray());
               Document xmlAEMFDDocument = new Document(is1);
               if (log.isDebugEnabled())
                   {
                    xmlAEMFDDocument.copyToFile(new File("dataxmldocument.xml"));
                }
             return xmlAEMFDDocument;
            }
            catch (Exception e)
                 {
                    log.error("Error in generating ddx " + e.getMessage());
                    return null;
                 }
        }
```

## Étapes suivantes

[Déployer les exemples de ressources sur votre système](./deploy-on-your-system.md)