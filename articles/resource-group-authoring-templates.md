<properties
   pageTitle="Azure erőforrás-kezelő sablonok létrehozása |} Microsoft Azure"
   description="Erőforrás-kezelő Azure sablonok JSON szintaxissal deklaráció a Azure alkalmazások terjesztése létrehozása."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="tomfitz"/>

# <a name="authoring-azure-resource-manager-templates"></a>Erőforrás-kezelő Azure-sablonok létrehozása

Ez a témakör ismerteti a szerkezet egy erőforrás-kezelő Azure-sablon. Jelenítse meg a sablon és a Tulajdonságok szakaszokat rendelkezésre álló szakaszonkénti. A sablont, ahol a JSON és kifejezések, amelyeket a telepítéshez értékek összeállításához használhat. 

Erőforrások már telepítette a sablon megtekintéséhez olvassa el a [egy erőforrás-kezelő Azure-sablon a meglévő erőforrásoktól exportálása](resource-manager-export-template.md)című témakört. A sablon létrehozása, olvassa el az [Erőforrás-kezelő sablon forgatókönyv](resource-manager-template-walkthrough.md). Sablonok létrehozásával kapcsolatos javaslatok olvassa el a [Gyakorlati tanácsok az erőforrás-kezelő Azure-sablonok létrehozása](resource-manager-template-best-practices.md)című témakört.

Egy jó JSON-szerkesztő egyszerűsítheti sablonok létrehozását. Visual Studio a sablonok használatáról további tudnivalókért lásd [létrehozása és üzembe helyezése a Visual Studio segítségével Azure erőforrás-csoportokat](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). VIEWBEN kód használatával kapcsolatos információkért olvassa el az [Azure erőforrás-kezelő sablonok a Visual Studio kód használata](resource-manager-vs-code.md)című témakört.

Korlátozza a méretét, 1 MB a sablont, és mindegyik paraméter fájl 64 KB. Az 1 MB-os korlát vonatkozik a végleges állapot sablon után közelítéses erőforrás-definíciókat, és a változók és a paraméterek értékekkel lett bontva. 

## <a name="template-format"></a>Sablon formázása

A legegyszerűbb felépítésétől sablon az alábbi elemeket tartalmazza:

    {
       "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
       "contentVersion": "",
       "parameters": {  },
       "variables": {  },
       "resources": [  ],
       "outputs": {  }
    }

| Elem neve   | Szükséges | Leírás
| :------------: | :------: | :----------
| $schema        |   igen    | A sablon nyelvi verziójában leíró JSON séma fájl helyét. Az előző példában, az URL-címet használja.
| contentVersion |   igen    | A sablon (például 1.0.0.0) verziója. Bármilyen érték adhat az elemhez. A sablon használatával erőforrások telepítésekor ezt az értéket győződjön meg arról, hogy a megfelelő sablon használatos használható.
| Paraméterek     |   nem     | Telepítési erőforrás telepítési testreszabása végrehajtásakor kapott érték.
| változók      |   nem     | A sablonban JSON töredékek sablon nyelvi kifejezések egyszerűsítése érdekében használt értékeket.
| erőforrások      |   igen    | Erőforrás típusa telepített vagy frissített egy erőforrás csoportban.
| kimeneti értékeket        |   nem     | Telepítés után a visszaadott értékek.

Azt vizsgálja meg a sablon a témakör későbbi részletesebben a szakaszok. Most azt fogja megvizsgálni, az alábbi, a sablon alkotó részét.

## <a name="expressions-and-functions"></a>Kifejezésekről és függvényekről

A sablon egyszerű szintaxisa JSON. A kifejezések és a függvények bővítése a JSON elérhető a sablonban. Kifejezések használatával hozhat létre, amelyek nem szigorú konstans érték értékeket. A kifejezések vannak szögletes zárójelekkel [és], és kiértékelése, ha telepíti a sablont. A kifejezések is tetszőleges JSON szöveges értékként jelennek meg, és mindig eredmény egy másik JSON. Ha szeretne egy konstans karakterlánccal kezdődik egy szögletes zárójel [, kell használnia, két zárójelek [[.

Általában segítségével kifejezések függvényekkel a telepítés beállításával kapcsolatos műveletek hajthatók végre. Hasonlóan a JavaScript, a függvény hívások formázott **functionName(arg1,arg2,arg3)**. A pont- és [index] operátorokkal referencia a Tulajdonságok parancsot.

A következő példa bemutatja a több függvény használata az értékek kiszámításakor:
 
    "variables": {
       "location": "[resourceGroup().location]",
       "usernameAndPassword": "[concat('parameters('username'), ':', parameters('password'))]",
       "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
    }

A teljes listáját a sablon függvények lásd: [Azure erőforrás-kezelő sablon függvények](resource-group-template-functions.md). 


## <a name="parameters"></a>Paraméterek

A sablon a paraméterek csoportban adja meg az értékek is a szövegbeviteli az erőforrások telepítésekor. Következő paraméterértékeket lehetővé teszi a telepítési testreszabása, mert az értékek, amelyek igényeihez vannak szabva egy adott környezetben (például a fejlesztők, tesztelése és a gyártási). Nem rendelkezik a sablon paraméterek megadására, de a paraméter nélkül a sablon mindig szeretné üzembe azonos a nevek, helyek és tulajdonságok ugyanazokat az erőforrásokat.

Következő paraméterértékeket egész a sablon segítségével adja meg a telepített erőforrások értékeit. Csak a paraméterek szakasz deklarált paraméterek más szakaszokba sablon használható.

Az alábbi szerkezet paramétereket definiálása:

    "parameters": {
       "<parameter-name>" : {
         "type" : "<type-of-parameter-value>",
         "defaultValue": "<default-value-of-parameter>",
         "allowedValues": [ "<array-of-allowed-values>" ],
         "minValue": <minimum-value-for-int>,
         "maxValue": <maximum-value-for-int>,
         "minLength": <minimum-length-for-string-or-array>,
         "maxLength": <maximum-length-for-string-or-array-parameters>,
         "metadata": {
             "description": "<description-of-the parameter>" 
         }
       }
    }

| Elem neve   | Szükséges | Leírás
| :------------: | :------: | :----------
| parameterName  |   igen    | A paraméter neve. Egy érvényes JavaScript-azonosító kell lennie.
| típus           |   igen    | A paraméter értéke típusát. Lásd: engedélyezett típusú az alábbi listából.
| defaultValue   |   nem     | A paraméter, ha a paraméter nincs érték megadva alapértelmezett értékét.
| allowedValues  |   nem     | Győződjön meg arról, hogy a megfelelő érték szerepel-e a paraméter megengedett értékek tömbjét.
| a minValue       |   nem     | A minimális int típusú paramétereket, ez az érték értéke végpontokat is beleértve.
| maxValue       |   nem     | A maximális int típusú paramétereket, ez az érték értéke végpontokat is beleértve.
| minLength      |   nem     | A karakterlánc, secureString és tömb típusú paramétereket minimális hosszát, ez az érték végpontokat is beleértve.
| maxLength      |   nem     | A maximális karakterlánc, secureString és tömb típusú paramétereket, ez az érték hossza végpontokat is beleértve.
| Leírás    |   nem     | A paramétert, amely akkor jelenik meg a sablon a portál egyéni sablon felületen felhasználók leírását.

Az engedélyezett típusok és értékek a következők:

- **karakterlánc**
- **secureString**
- **int**
- **logikai**
- **objektum** 
- **secureObject**
- **tömb**

Paraméter megadása nem kötelező, adja meg a defaultValue (üres karakterlánc is lehet). 

Ha a paraméter egy bevezetését tervezi a sablon a parancsot a paraméterek megegyező nevet ad meg, a program kéri a adjon meg egy értéket a utólagos javítás **FromTemplate**paraméter. Például ha **ResourceGroupName** nevű a sablon paraméter is, hogy ez ugyanaz, mint a **ResourceGroupName** paramétert a [New-AzureRmResourceGroupDeployment] [ deployment2cmdlet] parancsmag, adjon értéket **ResourceGroupNameFromTemplate**kéri. Általánosságban elmondható ne a fejetlenséget által nem elnevezési paraméterek paraméterként használt üzembe helyezési műveleteket azonos nevű.

>[AZURE.NOTE] Minden jelszóval, billentyűparancsok és más titkos kulcsok **secureString** típusa kell használni. Sablon paraméterek secureString típusú erőforrás-telepítés után nem olvashatók. 

A következő példa bemutatja a paraméter megadása:

    "parameters": {
      "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
      },
      "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
      },
      "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
      },
      "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
      }
    }

Beviteli paraméterértékeket a telepítés során, akkor olvassa el a [Deploy egy alkalmazást, az erőforrás-kezelő Azure-sablon](resource-group-template-deploy.md#parameter-file)leírását. 

## <a name="variables"></a>Változók

Változók szakaszban használható értékek egész a sablon létrehozása. Változók általában paraméterekből megadott értékek alapulnak. Nem kell változók, de azok összetett kifejezésekben csökkentésével gyakran leegyszerűsíti a sablont.

Az alábbi szerkezet tartalmazó változók definiálása:

    "variables": {
       "<variable-name>": "<variable-value>",
       "<variable-name>": { 
           <variable-complex-type-value> 
       }
    }

A következő példa bemutatja egy változó, amely a két paraméterértékeket összeállítás megadása:

     "variables": {
       "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
    }

A következő példa bemutatja a, amely egy összetett JSON típusú változóban tárolhatja, és a változók épülnek fel a további változót:

    "parameters": {
       "environmentName": {
         "type": "string",
         "allowedValues": [
           "test",
           "prod"
         ]
       }
    },
    "variables": {
       "environmentSettings": {
         "test": {
           "instancesSize": "Small",
           "instancesCount": 1
         },
         "prod": {
           "instancesSize": "Large",
           "instancesCount": 4
         }
       },
       "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
       "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
       "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
    }

## <a name="resources"></a>Erőforrások

Az erőforrások szakaszban az erőforrások telepített vagy frissített definiálhatók. Ez a szakasz bonyolult elérheti, mert látnia kell a típusok telepíti az adja meg a megfelelő értékeket. 

Az alábbi szerkezet rendelkező erőforrások definiálása:

    "resources": [
       {
         "apiVersion": "<api-version-of-resource>",
         "type": "<resource-provider-namespace/resource-type-name>",
         "name": "<name-of-the-resource>",
         "location": "<location-of-resource>",
         "tags": "<name-value-pairs-for-resource-tagging>",
         "comments": "<your-reference-notes>",
         "dependsOn": [
           "<array-of-related-resource-names>"
         ],
         "properties": "<settings-for-the-resource>",
         "copy": {
           "name": "<name-of-copy-loop>",
           "count": "<number-of-iterations>"
         }
         "resources": [
           "<array-of-child-resources>"
         ]
       }
    ]

| Elem neve             | Szükséges | Leírás
| :----------------------: | :------: | :----------
| apiVersion               |   igen    | A REST API-t az erőforrás elkészítéséhez használható verziója.
| típus                     |   igen    | Az erőforrás típusa. Ez az érték a névtér az erőforrás-szolgáltató és az erőforrás típusa (például **Microsoft.Storage/storageAccounts**) kombinációi.
| név                     |   igen    | Az erőforrás neve. A név URI-összetevő korlátozások RFC3986 meghatározott kell követnie. Ezeken kívül Azure szolgáltatások jelenítik meg az erőforrás neve kívüli feleket ellenőrzéséhez győződjön meg arról, hogy a név nem kísérlet egy másik identitás jele lehet. Lásd: [Jelölje be az erőforrás neve](https://msdn.microsoft.com/library/azure/mt219035.aspx).
| hely                 |   Változó  | Támogatott az adott erőforrás geo helyét. Az elérhető helyek közül választhat, de általában célszerű leginkább hasonlító a felhasználók egy másik. Általában is célszerű helyezze információforrások, amelyek egymással ugyanabban a régióban interaktív módon. A legtöbb erőforrástípus szüksége egy helyet, de bizonyos típusú (például a szerepkör-hozzárendelés) nincs szüksége egy helyen.
| címkék                     |   nem     | Az erőforráshoz társított címkék.
| Megjegyzések                 |   nem     | A jegyzetek dokumentálás, a sablon az erőforrások
| dependsOn                |   nem     | Az erőforrások függ, hogy az erőforrás definiálását. Erőforrások közötti függőségek kiértékelése és erőforrások van telepítve, a függő sorrendben. Ha erőforrások nem egymástól függenek, azok párhuzamosan van telepítve. Az érték egy erőforrás vesszővel tagolt listáját lehet neveket vagy az erőforrás-egyedi azonosítókat.
| Tulajdonságok               |   nem     | Erőforrás-specifikus konfigurációs beállítások. Az a tulajdonságokat értékei ugyanaz, mint a REST API művelet (helyezése módszer) hozhat létre az erőforrás összehívás törzsében szükséges értékeket. Hivatkozások erőforrás séma dokumentáció vagy REST API-t olvassa el az [erőforrás-kezelő szolgáltatók régiók, API-verziók sémák és](resource-manager-supported-services.md)című témakört.
| másolása                     |   nem     | Ha egynél több példány szükségessége, a hozzárendelt erőforrások hozhat létre. További tudnivalókért olvassa el a [létrehozása az Azure erőforrás-kezelő erőforrások több példányával](resource-group-create-multiple.md)című témakört. |
| erőforrások                |   nem     | Az erőforrás definiálását függő gyermek erőforrásokat. Megadhatja, hogy csak a séma a szülő erőforrás által megengedett erőforrás típusú. A gyermek erőforrástípus teljesen minősített neve szülőwebhely erőforrástípus, például **Microsoft.Web/sites/extensions**tartalmaz. A szülő erőforrás függés nem implicit; meg kell határoznia, hogy függőség explicit módon. 

Arra, hogy milyen értékeket **apiVersion**megadása, **típusát**, és a **hely** nem egyértelmű azonnal. Ezek az értékek Azure PowerShell vagy Azure CLI keresztül Szerencsére megállapítható.

Az erőforrás-szolgáltatók **PowerShell**, használja:

    Get-AzureRmResourceProvider -ListAvailable

A visszaadott listában keresse meg az erőforrás-szolgáltatók érdeklik. Az erőforrás típusa (például tárhely) erőforrás szolgáltató, használja:

    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes

Az API-verziók esetében egy erőforrás típusa (például tárterület-fiókok esetén), használja:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).ApiVersions

Támogatott helyei egy erőforrás típusa, használja:

    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Storage).ResourceTypes | Where-Object ResourceTypeName -eq storageAccounts).Locations

Az erőforrás-szolgáltatók az **Azure CLI**, használja:

    azure provider list

A visszaadott listában keresse meg az erőforrás-szolgáltatók érdeklik. Az erőforrás típusa (például tárhely) erőforrás szolgáltató, használja:

    azure provider show Microsoft.Storage

Támogatott helyek és az API-verzió, használja:

    azure provider show Microsoft.Storage --details --json

Többet szeretne tudni az erőforrás-szolgáltatók, lásd: az [erőforrás-kezelő szolgáltatók régiók, API-verziók sémák és](resource-manager-supported-services.md).

Az erőforrások szakasz az erőforrások üzembe tömb tartalmazza. Belül az egyes erőforrásokhoz a gyermek erőforrások tömbje is megadhatja. Emiatt a források részben sikerült van egy struktúra, például:

    "resources": [
       {
           "name": "resourceA",
       },
       {
           "name": "resourceB",
           "resources": [
               {
                   "name": "firstChildResourceB",
               },
               {   
                   "name": "secondChildResourceB",
               }
           ]
       },
       {
           "name": "resourceC",
       }
    ]


A következő példa bemutatja egy **Microsoft.Web/serverfarms** és a gyermek **tartozzon** erőforráshoz **Microsoft.Web/sites** típusú erőforrást. Figyelje meg, hogy a webhely van megjelölve ezektől függnek a kiszolgálófarmban óta a kiszolgálófarm léteznie kell, mielőtt a telepíthető-e a webhelyen. Figyelje meg is, hogy a **tartozzon** erőforráshoz gyermekének a webhelyet.

    "resources": [
      {
        "apiVersion": "2015-08-01",
        "name": "[parameters('hostingPlanName')]",
        "type": "Microsoft.Web/serverfarms",
        "location": "[resourceGroup().location]",
        "tags": {
          "displayName": "HostingPlan"
        },
        "sku": {
          "name": "[parameters('skuName')]",
          "capacity": "[parameters('skuCapacity')]"
        },
        "properties": {
          "name": "[parameters('hostingPlanName')]",
          "numberOfWorkers": 1
        }
      },
      {
        "apiVersion": "2015-08-01",
        "type": "Microsoft.Web/sites",
        "name": "[parameters('siteName')]",
        "location": "[resourceGroup().location]",
        "tags": {
          "environment": "test",
          "team": "Web"
        },
        "dependsOn": [
          "[concat('Microsoft.Web/serverFarms/', parameters('hostingPlanName'))]"
        ],
        "properties": {
          "name": "[parameters('siteName')]",
          "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
        },
        "resources": [
          {
            "apiVersion": "2015-08-01",
            "type": "extensions",
            "name": "MSDeploy",
            "dependsOn": [
              "[concat('Microsoft.Web/sites/', parameters('siteName'))]"
            ],
            "properties": {
              "packageUri": "https://auxmktplceprod.blob.core.windows.net/packages/StarterSite-modified.zip",
              "dbType": "None",
              "connectionString": "",
              "setParameters": {
                "Application Path": "[parameters('siteName')]"
              }
            }
          }
        ]
      }
    ]


## <a name="outputs"></a>Kimeneti értékeket

A Kimenet csoportban adja meg az értékek, amelyek telepítési ad vissza. A telepített erőforrás eléréséhez URI például sikerült adja vissza.

A következő példa bemutatja egy kimenet definition felépítésének:

    "outputs": {
       "<outputName>" : {
         "type" : "<type-of-output-value>",
         "value": "<output-value-expression>"
       }
    }

| Elem neve   | Szükséges | Leírás
| :------------: | :------: | :----------
| outputName     |   igen    | A kimeneti értéket neve. Egy érvényes JavaScript-azonosító kell lennie.
| típus           |   igen    | A kimeneti értéket típusát. Kimeneti értékeket támogatnak mint sablon bemeneti paramétereket.
| érték          |   igen    | Sablon nyelven a kifejezés, amely kiértékelt és a kimeneti értéket adja vissza.


A következő példa bemutatja egy érték, amely a kimeneti értékeket szakasz ad vissza.

    "outputs": {
       "siteUri" : {
         "type" : "string",
         "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
       }
    }

Kimeneti használatával kapcsolatos további tudnivalókért lásd: [Azure erőforrás-kezelő sablonok megosztás állapotát](best-practices-resource-manager-state.md).

## <a name="next-steps"></a>Következő lépések
- Számos különböző típusú megoldások kész sablonok megtekintéséhez, lásd: az [Azure quickstart útmutató sablonokat](https://azure.microsoft.com/documentation/templates/).
- A sablon belül használhatja funkciók kapcsolatos részletekért lásd: [Azure erőforrás-kezelő sablon függvények](resource-group-template-functions.md).
- Több sablonok egyesítéséhez a telepítés során, olvassa el a [Azure erőforrás-kezelővel csatolt sablonok használata](resource-group-linked-templates.md)című témakört.
- Előfordulhat, hogy használja, amely tartalmaz egy másik erőforráscsoport erőforrásait. Ebben az esetben nem általános tárolási fiókok vagy több erőforrás csoportok közötti megosztott virtuális hálózatok. További tudnivalókért olvassa el a a [resourceId függvény](resource-group-template-functions.md#resourceid)című témakört.


[deployment2cmdlet]: https://msdn.microsoft.com/library/mt740620(v=azure.200).aspx
