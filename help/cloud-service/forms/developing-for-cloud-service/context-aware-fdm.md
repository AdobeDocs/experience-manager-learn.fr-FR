---
title: Prise en charge du remplacement de la configuration basée sur le contexte pour le modèle de données de formulaire
description: Configurez des modèles de données de formulaire et communiquez avec différents points d’entrée en fonction des environnements.
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-10423
exl-id: 2ce0c07b-1316-4170-a84d-23430437a9cc
duration: 99
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 100%

---

# Configurations cloud basées sur le contexte

Lorsque vous créez une configuration cloud dans votre environnement local et que les tests sont probants, vous souhaitez utiliser la même configuration cloud dans vos environnements en amont, mais sans avoir à modifier le point d’entrée, la clé secrète ou le mot de passe et le nom d’utilisateur ou d’utilisatrice. Pour réaliser ce cas d’utilisation, AEM Forms as a Cloud Service permet désormais de définir des configurations cloud basées sur le contexte.
Par exemple, la configuration cloud du compte de stockage Azure peut être réutilisée dans les environnements de développement, d’évaluation et de production à l’aide de différentes chaînes et clés de connexion.

Pour créer une configuration cloud basée sur le contexte, procédez comme suit.

## Créer des variables d’environnement

Les variables d’environnement standard peuvent être configurées et gérées via Cloud Manager. Elles sont fournies à l’environnement d’exécution et peuvent être utilisés dans des configurations OSGi. [Les variables d’environnement peuvent être des valeurs spécifiques à un environnement ou des secrets d’environnement, en fonction de ce qui est modifié.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/environment-variables.html?lang=fr)



La copie d’écran suivante montre les variables d’environnement azure_key et azure_connection_string définies.
![environment_variables](assets/environment-variables.png)

Ces variables d’environnement peuvent ensuite être spécifiées dans les fichiers de configuration à utiliser dans l’environnement approprié.
Par exemple, si vous souhaitez que toutes vos instances de création utilisent ces variables d’environnement, vous devez définir le fichier de configuration dans le dossier config.author comme indiqué ci-dessous.

## Créer un fichier de configuration

Ouvrez votre projet dans IntelliJ. Accédez à config.author et créez un fichier appelé :

```java
org.apache.sling.caconfig.impl.override.OsgiConfigurationOverrideProvider-integrationTest.cfg.json
```

![config.author](assets/config-author.png).

Copiez le texte suivant dans le fichier que vous avez créé à l’étape précédente. Le code de ce fichier remplace la valeur des propriétés accountName et accountKey par les variables d’environnement **azure_connection_string** et **azure_key**.

```json
{
  "enabled":true,
  "description":"dermisITOverrideConfig",
  "overrides":[
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountName=\"$[env:azure_connection_string]\"",
   "cloudconfigs/azurestorage/FormsCSAndAzureBlob/accountKey=\"$[secret:azure_key]\""

  ]
}
```

>[!NOTE]
>
>Cette configuration s’applique à tous les environnements de création de votre instance de Cloud Service. Pour appliquer la configuration aux environnements de publication, vous devez placer le même fichier de configuration dans le dossier config.publish de votre projet intelliJ.
>[!NOTE]
> Assurez-vous que la propriété en cours de remplacement est une propriété valide de la configuration cloud. Accédez à la configuration cloud pour trouver la propriété à remplacer, comme illustré ci-dessous.

![cloud-config-property](assets/cloud-config-properties.png)

Pour les configurations cloud REST avec authentification de base, vous devez généralement créer des variables d’environnement pour les propriétés serviceEndPoint, userName et password.

## Étapes suivantes

[Envoyer votre projet AEM vers Cloud Manager](./push-project-to-cloud-manager-git.md)
