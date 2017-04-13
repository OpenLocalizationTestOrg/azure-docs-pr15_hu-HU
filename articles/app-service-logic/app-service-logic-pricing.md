<properties 
    pageTitle="Árak modell összefüggés-alkalmazások |} Microsoft Azure" 
    description="Részletek árak működésével kapcsolatos összefüggés-alkalmazásokban" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="logic-apps-pricing-model"></a>Logika alkalmazások árak modell

Azure logika Apps lehetővé teszi méretezni, majd hajtsa végre a munkafolyamatok integráció a felhőben.  Az alábbiakban a számlázás és árak csomagok logika alkalmazásai tájékoztatást.

## <a name="consumption-pricing"></a>Felhasználás árak

Az újonnan létrehozott logika alkalmazások felhasználási terv használja. Logika alkalmazások felhasználási árak modell csak fizet, használja a.  Logika alkalmazások vannak nem szabályozott, amikor felhasználási-csomagot használ.
Az összes, a Futtatás logika alkalmazás példány végrehajtható műveletek forgalmi vannak.

### <a name="what-are-action-executions"></a>Mik azok a művelet végrehajtások?

Logika alkalmazás definícióját minden lépésként művelet.  Eseményindítók, ellenőrző folyamat lépéseinek, feltételek és keresési tartományok, minden egyes hurkok, amíg hurkok, összekötőkhöz hívások és a hívások átirányítása natív műveletek működnek, mint tartalmazó beállítás.
Eseményindítók, amelyek elindítását egy logikai alkalmazás új példányát, amikor a adott esemény csak speciális műveletek.  Számos különböző viselkedése eseményindítók, amelyek hogyan a logika alkalmazás forgalmi van hatással lehetnek.

-   Zárólap **eseményindító lekérdezési** – Ez az eseményindító folyamatosan lekérdezi, amíg meg nem kapja, egy üzenet, amely eleget tesz a hozhat létre egy logikai alkalmazás új példányát.  A lekérdezési időköz a logika alkalmazások-szerkesztőben az eseményindító beállíthatók.  Minden lekérdezési kérést, akkor is, ha nem jön létre a logika alkalmazás új példányát számít, egy művelet végrehajtása.

-   **Eseményindító Webhook** – Ez az eseményindító megvárja, amíg az ügyfél-összehívásokkal azt egy adott végpont.  Minden küldött a webhook végpont kérés számít egy művelet végrehajtása. A kérelem és a HTTP Webhook eseményindító mindkét webhook eseményindítók.

-   **Ismétlődés eseményindító** – Ez az eseményindító hoz létre egy új példányát az logika alkalmazást, az ismétlődési gyakoriságát az eseményindító konfigurált alapján.  Például az ismétlődés az eseményindító beállítható úgy, hogy minden 3 nap vagy akár percenként futtatásához.

Eseményindító végrehajtások a logika alkalmazások erőforrás lap az eseményindító verziók részben láthatók.

Az összes műveletek végrehajtása történt, a művelet végrehajtása, hogy sikeres vagy sikertelen volt vannak forgalmi.  Művelet végrehajtások nem számítanak a műveleteket, amelyeket egy feltétel nem teljesül miatt hagyva vagy műveleteket, amelyeket nem hajtható végre, mert a befejezés előtt végződő a logika alkalmazást.

Belül hurkok végrehajtható műveletek / Ismétlés a leállításig a számítanak.  Ha például az egyetlen művelet a az egyes hurok léptetés – 10 elem listájának lesznek megszámlálva műveletek (1) le számának szorzata (10) listában az elemek számának, plusz egyet a kezdeményezési a hurok, amely az ebben a példában lenne (10 * 1) + 1 = 11 művelet végrehajtások.

Logika le vannak tiltva alkalmazásai példányként létrehozott új példányok nem lehet, és ezért le vannak tiltva ideje alatt nem kérjen díjkötelesek.  Kell szem előtt tartva, hogy egy összefüggés-alkalmazás letiltása után eltarthat egy kis időt, mielőtt teljesen le van tiltva a előfordulását szeretné leválasztása.

## <a name="app-service-plans"></a>App milyen szolgáltatáscsomagok

Alkalmazás szolgáltatás tervek már nem szükséges összefüggés-alkalmazás létrehozása céljából.  Az alkalmazás szolgáltatás terv egy meglévő logika alkalmazással is hivatkozhat.  Korábban létrehozott egy alkalmazás szolgáltatás csomaggal logika alkalmazások továbbra is működnek előtti, attól függően, hogy a terv választott fog első szabályozott napi végrehajtások számos túllépik és fog nem számlát kapni a művelet végrehajtása állapotjelzője használata után.

Alkalmazás szolgáltatás tervek és a napi engedélyezett művelet végrehajtások:

| |Szabad/megosztott/Basic|Normál|Támogatás|
|---|---|---|---|
|Napi művelet végrehajtások| 200|10 000|50 000|

### <a name="convert-from-consumption-to-app-service-plan-pricing"></a>Az felhasználási konvertálása árak alkalmazás szolgáltatás megtervezése

Hivatkozás egy alkalmazás szolgáltatás megtervezése logika alkalmazás felhasználás; teheti egyszerűen [futtassa az alábbi PowerShell parancsprogramot](https://github.com/logicappsio/ConsumptionToAppServicePlan).  Ellenőrizze, hogy először az [Azure PowerShell eszközök](https://github.com/Azure/azure-powershell) telepítve van.

``` powershell
Param(
    [string] $AppService_RG = '<app-service-resource-group>',
    [string] $AppService_Name = '<app-service-name>',
    [string] $LogicApp_RG = '<logic-app-resource-group>',
    [string] $LogicApp_Name = '<logic-app-name>',
    [string] $subscriptionId = '<azure-subscription-id>'
)

Login-AzureRmAccount 
$subscription = Get-AzureRmSubscription -SubscriptionId $subscriptionId
$appserviceplan = Get-AzureRmResource -ResourceType "Microsoft.Web/serverFarms" -ResourceGroupName $AppService_RG -ResourceName $AppService_Name
$logicapp = Get-AzureRmResource -ResourceType "Microsoft.Logic/workflows" -ResourceGroupName $LogicApp_RG -ResourceName $LogicApp_Name

$sku = @{
    "name" = $appservicePlan.Sku.tier;
    "plan" = @{
      "id" = $appserviceplan.ResourceId;
      "type" = "Microsoft.Web/ServerFarms";
      "name" = $appserviceplan.Name  
    }
}

$updatedProperties = $logicapp.Properties | Add-Member @{sku = $sku;} -PassThru

$updatedLA = Set-AzureRmResource -ResourceId $logicapp.ResourceId -Properties $updatedProperties -ApiVersion 2015-08-01-preview
```

### <a name="convert-from-app-service-plan-pricing-to-consumption"></a>Alkalmazás szolgáltatás megtervezése árak felhasználásának konvertálása

Egy logikai alkalmazás, amely tartalmaz egy alkalmazás szolgáltatás megtervezése felhasználási modellhez társítva az alkalmazás szolgáltatás tervezése a hivatkozás eltávolítása a logika alkalmazás meghatározása módosítása.  Ez lehet tenni a hívást kezdeményez egy PowerShell-parancsmag:

`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`

## <a name="pricing"></a>Árak

Árak részletes útmutatásért olvassa [Logika alkalmazások árak](https://azure.microsoft.com/pricing/details/logic-apps/).

## <a name="next-steps"></a>Következő lépések

- [Logika alkalmazások áttekintése][whatis]
- [Az első összefüggés-alkalmazás létrehozása][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: app-service-logic-what-are-logic-apps.md
[create]: app-service-logic-create-a-logic-app.md

