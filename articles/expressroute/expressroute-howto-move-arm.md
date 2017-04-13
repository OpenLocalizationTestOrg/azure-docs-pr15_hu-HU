<properties
   pageTitle="Készült ExpressRoute áramkörök Váltás a klasszikus erőforrás-kezelő |} Microsoft Azure"
   description="A lap azt ismerteti, hogy a klasszikus áramkört áthelyezése az erőforrás-kezelő telepítési modell."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="ganesr"/>


# <a name="move-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>A klasszikus készült ExpressRoute áramkörök áthelyezése az erőforrás-kezelő telepítési modell

## <a name="configuration-prerequisites"></a>Konfigurációs vonatkozó követelmények

- Az Azure PowerShell-modul legújabb verziójának szükséges (legalább 1.0 verziójú).
- Győződjön meg arról, hogy ellenőrzését a [Előfeltételek](expressroute-prerequisites.md), a [követelmények routing](expressroute-routing.md)és a [munkafolyamatok](expressroute-workflows.md) konfigurációs megkezdése előtt.
- Előtt további megelőző, tekintse át, amely [egy készült ExpressRoute áramkör, az erőforrás-kezelő klasszikus áthelyezése](expressroute-move.md)alapján. Győződjön meg arról, hogy meg van teljesen értelmezni, korlátozások, és mi mindenre korlátai.
- Ha vissza szeretne váltani az Azure készült ExpressRoute áramkör a klasszikus telepítési modell az Azure erőforrás-kezelő telepítési modell, a teljes mértékben konfigurálva vannak és műveleti a klasszikus telepítési modell áramkör kell rendelkeznie.
- Győződjön meg arról, hogy az erőforrás-kezelő telepítési modell létrehozott erőforráscsoport.

## <a name="move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>Ugrás a készült ExpressRoute áramkör az erőforrás-kezelő telepítési modell

Az készült ExpressRoute áramkör kell helyeznie az erőforrás-kezelő telepítési modell, hogy használni tudja a hagyományos és az erőforrás-kezelő telepítési modelljei is keresztül. Ez az alábbi PowerShell-parancsait futtatásával teheti meg.

### <a name="step-1-gather-circuit-details-from-the-classic-deployment-model"></a>Lépés: 1: Áramkör részletek összegyűjtése a klasszikus telepítési modell

Először gyűjtse össze a a készült ExpressRoute áramkör információt szeretne.

Jelentkezzen be az Azure klasszikus környezet, és a szolgáltatás kulcs gyűjtése. Információk összegyűjtése az alábbi PowerShell kódtöredékének használhatók:

    # Sign in to your Azure account
    Add-AzureAccount

    # Select the appropriate Azure subscription
    Select-AzureSubscription "<Enter Subscription Name here>"

    # Import the PowerShell modules for Azure and ExpressRoute
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\Azure.psd1'
    Import-Module 'C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement\Azure\ExpressRoute\ExpressRoute.psd1'

    # Get the service keys of all your ExpressRoute circuits
    Get-AzureDedicatedCircuit

Másolja a vágólapra a áramkör, vigye fölé az erőforrás-kezelő telepítési modellhez kívánt **szolgáltatás billentyűt** .

### <a name="step-2-sign-in-to-the-resource-manager-environment-and-create-a-new-resource-group"></a>Lépés: 2: Jelentkezzen be az erőforrás-kezelő környezet, és hozzon létre egy új erőforráscsoport

A következő kódtöredékének használatával hozhat létre új erőforráscsoport:

    # Sign in to your Azure Resource Manager environment
    Login-AzureRmAccount

    # Select the appropriate Azure subscription
    Get-AzureRmSubscription -SubscriptionName "<Enter Subscription Name here>" | Select-AzureRmSubscription

    #Create a new resource group if you don't already have one
    New-AzureRmResourceGroup -Name "DemoRG" -Location "West US"

Meglévő erőforráscsoport is használhatja, ha már van egy.

### <a name="step-3-move-the-expressroute-circuit-to-the-resource-manager-deployment-model"></a>3 lépés: Az erőforrás-kezelő telepítési modell a készült ExpressRoute áramkör áthelyezése

Most már készen áll a klasszikus az erőforrás-kezelő telepítési modell áthelyezése a készült ExpressRoute áramkör fölé. Tekintse át a folytatás előtt [áthelyezése egy készült ExpressRoute áramkör, az erőforrás-kezelő telepítési adatmodellhez klasszikus](expressroute-move.md) csoportban megadott adatokkal.

Ehhez a következő kódtöredékének fut:

    Move-AzureRmExpressRouteCircuit -Name "MyCircuit" -ResourceGroupName "DemoRG" -Location "West US" -ServiceKey "<Service-key>"

>[AZURE.NOTE] Az áthelyezés után az új nevet, amely szerepel az előző parancsmag használandó cím az erőforrás. A kapcsolat lényegében lehet átnevezni.

## <a name="enable-an-expressroute-circuit-for-both-deployment-models"></a>A két környezetben modellek az készült ExpressRoute áramkör engedélyezése

A telepítési modell való hozzáférés szabályozása előtt, helyezze át a készült ExpressRoute áramkör az erőforrás-kezelő telepítési modellhez.

Futtassa a következő parancsmagot a két környezetben levő hozzáférés engedélyezése:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to TRUE
    $ckt.AllowClassicOperations = $true

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Ez a művelet sikeresen befejeződött, miután lesz a áramkör megtekintése a klasszikus telepítési modell.

Futtassa a következő, a készült ExpressRoute áramkör részleteket:

    get-azurededicatedcircuit

A szolgáltatás kulcs felsorolt látja kell lennie. Most már kezelheti a készült ExpressRoute áramkör, a szabványos klasszikus telepítési modell a klasszikus VNets és a szokásos ARM parancsok használata ARM VNETs mutató hivatkozásokat. Az alábbi cikkekben végigvezeti a készült ExpressRoute áramkör mutató hivatkozások kezelése:

- [A virtuális hálózat csatolása a készült ExpressRoute áramkör, az erőforrás-kezelő telepítési modell](expressroute-howto-linkvnet-arm.md)
- [A virtuális hálózat csatolása a készült ExpressRoute áramkör a klasszikus telepítési modell](expressroute-howto-linkvnet-classic.md)


## <a name="disable-the-expressroute-circuit-to-the-classic-deployment-model"></a>A klasszikus telepítési modell a készült ExpressRoute áramkör letiltása

Futtassa a következő parancsmagot a klasszikus telepítési modell elérésének letiltása:

    # Get details of the ExpressRoute circuit
    $ckt = Get-AzureRmExpressRouteCircuit -Name "DemoCkt" -ResourceGroupName "DemoRG"

    #Set "Allow Classic Operations" to FALSE
    $ckt.AllowClassicOperations = $false

    # Update circuit
    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

Ez a művelet sikeresen befejeződött, miután nem fogja tudni megtekinteni a áramkör a klasszikus telepítési modell.

## <a name="next-steps"></a>Következő lépések

Miután létrehozta a áramkör, ügyeljen, tegye a következőket:

- [Létrehozása és módosítása a készült ExpressRoute áramkör továbbítása](expressroute-howto-routing-arm.md)
- [A virtuális hálózat csatolása a készült ExpressRoute áramkör](expressroute-howto-linkvnet-arm.md)
