<properties
    pageTitle="Azure alkalmazás szolgáltatása szolgáltatója magas sűrűség |} Microsoft Azure"
    description="Magas sűrűség szolgáltatója Azure alkalmazás szolgáltatása"
    authors="btardif"
    manager="wpickett"
    editor=""
    services="app-service\web"
    documentationCenter=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="byvinyal"/>

# <a name="high-density-hosting-on-azure-app-service"></a>Magas sűrűség szolgáltatója Azure alkalmazás szolgáltatása

Alkalmazás szolgáltatás használata esetén a program a kapacitás két fogalmak elosztja a leválasztott az alkalmazást:

- **Az alkalmazás:** Az alkalmazás és runtime konfigurációját jelöli. Például a .NET, amelyek a futtatókörnyezet be kell, az alkalmazás beállításainak stb verziója tartalmazza.

- **Alkalmazás szolgáltatáscsomagja:** Határozza meg a beosztását, érhető el a szolgáltatás beállítása és, az alkalmazást településen jellemzői. Jellemzők lehet például nagy (négy magmintákat) gépi, négy példányok, kelet USA-beli prémium szolgáltatásaihoz.

Alkalmazás mindig csatolva van az alkalmazás szolgáltatáscsomagja, de egy alkalmazás szolgáltatáscsomagja kapacitása ahhoz, hogy egy vagy több alkalmazások tartalmaznak.

Emiatt a platform elkülöníteni egy egyetlen alkalmazást vagy az erőforrások megosztása a megosztott alkalmazás szolgáltatás csomagjával több alkalmazások rugalmasságot.

Azonban-alkalmazás szolgáltatáscsomagja megosztásakor a több alkalmazások, hogy az alkalmazás egy példánya fut összes előfordulását, hogy az alkalmazás szolgáltatáscsomagja.

## <a name="per-app-scaling"></a>Egy alkalmazás méretezése
*Egy alkalmazás méretezés* lehetővé teszi az alkalmazás szolgáltatás terv szintjén engedélyezve van, akkor egy alkalmazást használja.

Egy alkalmazás méretezés független ablakkal alkalmazás szolgáltatáscsomagja alkalmazás méretezze át. Ezzel a módszerrel alkalmazás szolgáltatás csomagjával beállítható úgy, hogy adja meg a 10 példányok, de az alkalmazás beállítható, hogy ha csak az 5.

Az alábbi erőforrás-kezelő Azure-sablon hoz létre egy alkalmazás szolgáltatáscsomagja méretezett meg a 10 példányok és az alkalmazás, amely egy alkalmazás méretezése és méretezni, csak 5 példányaiban van beállítva.

Az alkalmazás szolgáltatáscsomagja beállítja a **webhely méretezés** tulajdonság igaz ( `"perSiteScaling": true`). Az alkalmazás beállítja a **dolgozók számának** 5 (`"properties": { "numberOfWorkers": "5" }`).

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters":{
            "appServicePlanName": { "type": "string" },
            "appName": { "type": "string" }
         },
        "resources": [
        {
            "comments": "App Service Plan with per site perSiteScaling = true",
            "type": "Microsoft.Web/serverFarms",
            "sku": {
                "name": "S1",
                "tier": "Standard",
                "size": "S1",
                "family": "S",
                "capacity": 10
                },
            "name": "[parameters('appServicePlanName')]",
            "apiVersion": "2015-08-01",
            "location": "West US",
            "properties": {
                "name": "[parameters('appServicePlanName')]",
                "perSiteScaling": true
            }
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[parameters('appName')]",
            "apiVersion": "2015-08-01-preview",
            "location": "West US",
            "dependsOn": [ "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" ],
            "properties": { "serverFarmId": "[resourceId('Microsoft.Web/serverFarms', parameters('appServicePlanName'))]" },
            "resources": [ {
                    "comments": "",
                    "type": "config",
                    "name": "web",
                    "apiVersion": "2015-08-01",
                    "location": "West US",
                    "dependsOn": [ "[resourceId('Microsoft.Web/Sites', parameters('appName'))]" ],
                    "properties": { "numberOfWorkers": "5" }
             } ]
         }]
    }


## <a name="recommended-configuration-for-high-density-hosting"></a>Ajánlott konfiguráció a magas sűrűség szolgáltatónál

Egy alkalmazás méretezés lehetővé teszi, hogy engedélyezve van-e nyilvános Azure régiók és a alkalmazás szolgáltatási környezetben is. Az ajánlott stratégia azonban nem alkalmazás környezetek használata a speciális szolgáltatásait és a nagyobb teljesítményű készletek.  

Kövesse ezeket a lépéseket követve állítsa be az alkalmazás szolgáltatója magas sűrűség:

1. Állítsa be az alkalmazás-szolgáltatási környezetben, és válassza a dolgozó erőforráskészlethez tartozik, amelyek a magas sűrűség üzemeltetési forgatókönyv van hozzárendelve.

1. Egyetlen alkalmazás szolgáltatás terv létrehozása, és méretezze úgy, hogy a rendelkezésre álló kapacitás használata a dolgozó készletre.

1. A webhely méretezési jelző IGAZ meg az App szolgáltatáscsomagja.

1. Új webhelyek létrehozása és a **numberOfWorkers** tulajdonság értéke **1**, hogy az alkalmazás szolgáltatáscsomagja rendelve. Ez a beállítás használatával együttesen dolgozó készlet a lehető legmagasabb sűrűség.

1. Munkatársak számát önállóan konfigurálható-webhelyenként, igény szerint további erőforrások megadását. Nagy használható webhely például **3** , hogy további feldolgozásra beosztását, hogy az alkalmazás, miközben alacsony használható webhelyek **numberOfWorkers** volna értéke **1**lehet, hogy állítsa **numberOfWorkers** .
