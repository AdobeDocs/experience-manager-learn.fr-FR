---
title: Stocker des données de formulaire adaptatif
description: Stocker des données de formulaire adaptatif dans la base de données dans le cadre de votre workflow AEM
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 3dd552da-fc7c-4fc7-97ec-f20b6cc33df0
last-substantial-update: 2020-03-20T00:00:00Z
duration: 150
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: ht
source-wordcount: '382'
ht-degree: 100%

---

# Stocker des envois de formulaire adaptatif dans la base de données

Il existe plusieurs façons de stocker les données de formulaire envoyées dans la base de données de votre choix. Une source de données JDBC peut être utilisée pour stocker directement les données dans la base de données. Un lot OSGI personnalisé peut être écrit pour stocker les données dans la base de données. Cet article utilise une étape de processus personnalisée dans un workflow AEM pour stocker les données.
Le cas d’utilisation consiste à déclencher un workflow AEM lors de l’envoi d’un formulaire adaptatif et une étape du workflow stocke les données envoyées dans la base de données.



## Pool de connexions JDBC

* Accédez à [ConfigMgr](http://localhost:4502/system/console/configMgr).

   * Recherchez « JDBC Connection Pool ». Créez un pool de connexions JDBC Day Commons. Spécifiez les paramètres spécifiques à votre base de données.

   * ![Configuration OSGi du pool de connexions JDBC.](assets/aemformstutorial-jdbc.png)

## Spécifier les détails de la base de données

* Recherchez « **Spécifier les détails de la base de données** ».
* Spécifiez les propriétés spécifiques à votre base de données.
   * DataSourceName : nom de la source de données que vous avez configurée précédemment.
   * TableName : nom du tableau dans laquelle vous souhaitez stocker les données du formulaire adaptatif.
   * FormName : nom de la colonne contenant le nom du formulaire.
   * ColumnName : nom de la colonne destinée à contenir les données du formulaire adaptatif.

  ![Spécification de la configuration OSGi des détails de la base de données.](assets/specify-database-details.png)



## Code pour la configuration OSGi

```java
package com.aemforms.dbsamples.core.insertFormData;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name = "Specify Database details", description = "Specify Database details")

public @interface InsertFormDataConfiguration {
  @AttributeDefinition(name = "DataSourceName", description = "Data Source Name configured")
  String dataSourceName() default "";
  @AttributeDefinition(name = "TableName", description = "Name of the table")
  String tableName() default "";
  @AttributeDefinition(name = "FormName", description = "Column Name for form name")
  String formName() default "";
  @AttributeDefinition(name = "columnName", description = "Column Name for form data")
  String columnName() default "";

}
```

## Lisez les valeurs de configuration.

```java
package com.aemforms.dbsamples.core.insertFormData;
import org.osgi.service.component.annotations.Activate;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.metatype.annotations.Designate;

@Component(service={InsertFormDataConfigurationService.class})
@Designate(ocd=InsertFormDataConfiguration.class)

public class InsertFormDataConfigurationService {
    public String TABLE_NAME;
    public String DATA_SOURCE_NAME;
    public String COLUMN_NAME;
    public String FORM_NAME;
    @Activate      
      protected final void activate(InsertFormDataConfiguration insertFormDataConfiguration)
      {
        TABLE_NAME = insertFormDataConfiguration.tableName();
        DATA_SOURCE_NAME = insertFormDataConfiguration.dataSourceName();
        COLUMN_NAME = insertFormDataConfiguration.columnName();
        FORM_NAME = insertFormDataConfiguration.formName();
      }
    public String getTABLE_NAME()
    {
        return TABLE_NAME;
    }
    public String getDATA_SOURCE_NAME()
    {
        return DATA_SOURCE_NAME;
    }
    public String getCOLUMN_NAME()
    {
        return COLUMN_NAME;
    }
    public String getFORM_NAME()
    {
        return FORM_NAME;
    }
}
```

## Code pour implémenter l’étape de processus

```java
package com.aemforms.dbsamples.core.insertFormData;
import java.io.InputStream;
import java.io.StringWriter;
import java.nio.charset.StandardCharsets;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.jcr.Node;
import javax.jcr.Session;
import javax.sql.DataSource;

import org.apache.commons.io.IOUtils;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.adobe.granite.workflow.WorkflowException;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
import com.adobe.granite.workflow.exec.WorkflowProcess;
import com.adobe.granite.workflow.metadata.MetaDataMap;
import com.day.commons.datasource.poolservice.DataSourcePool;

@Component(property = {
  Constants.SERVICE_DESCRIPTION + "=Insert Form Data in Database",
  Constants.SERVICE_VENDOR + "=Adobe Systems",
  "process.label" + "=Insert Form Data in Database"
})

public class InsertAfData implements WorkflowProcess {
  @Reference
  InsertFormDataConfigurationService insertFormDataConfig;
  @Reference
  DataSourcePool dataSourcePool;
  private final Logger log = LoggerFactory.getLogger(getClass());
  @Override
  public void execute(WorkItem workItem, WorkflowSession session, MetaDataMap metaDataMap) throws WorkflowException {

    String proccesArgsVals = (String) metaDataMap.get("PROCESS_ARGS", (Object)
      "string");
    String[] values = proccesArgsVals.split(",");
    String AdaptiveFormName = values[0];
    String formDataFile = values[1];
    String payloadPath = workItem.getWorkflowData().getPayload().toString();
    Session jcrSession = (Session) session.adaptTo((Class) Session.class);
    String dataFilePath = payloadPath + "/" + formDataFile + "/jcr:content";
    log.debug("The data file path is " + dataFilePath);
    PreparedStatement ps = null;
    Connection con = null;
    DataSource dbSource = null;

    try {
      dbSource = (DataSource) dataSourcePool.getDataSource(insertFormDataConfig.getDATA_SOURCE_NAME());
      log.debug("Got db source");
      con = dbSource.getConnection();

      Node xmlDataNode = jcrSession.getNode(dataFilePath);
      InputStream xmlDataStream = xmlDataNode.getProperty("jcr:data").getBinary().getStream();
      StringWriter writer = new StringWriter();
      String encoding = StandardCharsets.UTF_8.name();
      IOUtils.copy(xmlDataStream, writer, encoding);
      String queryStmt = "insert into " + insertFormDataConfig.TABLE_NAME + "(" + insertFormDataConfig.COLUMN_NAME + "," + insertFormDataConfig.FORM_NAME + ") values(?,?)";
      log.debug("The query Stmt is " + queryStmt);
      ps = con.prepareStatement(queryStmt);
      ps.setString(1, writer.toString());
      ps.setString(2, AdaptiveFormName);
      ps.executeUpdate();

    } catch (Exception e) {
      log.debug("The error message is " + e.getMessage());
    } finally {
      if (ps != null) {
        try {
          ps.close();
        } catch (SQLException sqlException) {
          log.debug(sqlException.getMessage());
        }
      }
      if (con != null) {
        try {
          con.close();
        } catch (SQLException sqlException) {
          log.error("Unable to close connection to database", sqlException);
        }
      }
    }
  }

}
```

## Déployer des exemples de ressources

* Vérifiez que vous avez configuré votre pool de connexions JDBC.
* Spécifiez les détails de la base de données à l’aide de configMgr.
* [Téléchargez le fichier zip et extrayez son contenu sur votre disque dur.](assets/article-assets.zip)

   * Déployez le fichier jar à l’aide de la [console web AEM](http://localhost:4502/system/console/bundles). Ce fichier jar contient le code permettant de stocker les données de formulaire dans la base de données.

   * Importez les deux fichiers zip dans [AEM à l’aide du gestionnaire de packages](http://localhost:4502/crx/packmgr/index.jsp). Vous obtiendrez l’[exemple de workflow](http://localhost:4502/editor.html/conf/global/settings/workflow/models/storeformdata.html) et l’[exemple de formulaire adaptatif](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) qui déclenchera le workflow lors de l’envoi du formulaire. Notez les arguments de processus dans l’étape du workflow. Ces arguments indiquent le nom du formulaire et le nom du fichier de données qui contiendra les données du formulaire adaptatif. Le fichier de données est stocké sous le dossier de payload dans le référentiel crx. Remarquez comment le [formulaire adaptatif](http://localhost:4502/editor.html/content/forms/af/addformdataindb.html) est configuré pour déclencher le workflow AEM lors de l’envoi et de la configuration du fichier de données (data.xml).

   * Prévisualisez et remplissez le formulaire et envoyez-le. Vous devriez voir une nouvelle ligne créée dans votre base de données.

