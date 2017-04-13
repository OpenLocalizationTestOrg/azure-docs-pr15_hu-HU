<properties
    pageTitle="A SharePoint server-farmok létrehozása |} Microsoft Azure"
    description="Gyorsan létrehozhat egy új SharePoint 2013-ban vagy a SharePoint 2016 farm Azure-ban."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>A SharePoint server-farmok létrehozása

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klasszikus modell.

## <a name="sharepoint-2013-farms"></a>A SharePoint 2013-farmok

A portál Microsoft Azure Piactérhez gyorsan létrehozhat előre beállított SharePoint Server 2013-farmok. Ez mentheti, sok időt egy egyszerű vagy magas rendelkezésre állás SharePoint-farm fejlesztők/próba környezet szükség esetén, vagy ha értékelése a SharePoint Server 2013 együttműködési megoldásként a szervezet számára.

> [AZURE.NOTE] Megszűnt a **SharePoint Server-farmba** cikk az Azure-portálra a Microsoft Azure piactéren. Helyette a **SharePoint 2013-ban nem – HA Farm** és a **SharePoint 2013-ban, HA Farm** elemekkel.

Az egyszerű SharePoint-farm három virtuális gépeken futó ebben a konfigurációban áll.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

A kiszolgálófarm beállítása egy egyszerűsített beállítása a SharePoint-alkalmazások fejlesztéséhez vagy a SharePoint 2013-ban első értékelése is használhatja.

Az egyszerű (három-kiszolgáló) SharePoint-farm létrehozása:

1. Kattintson [ide](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Kattintson a **üzembe**.
3. A **SharePoint 2013-ban nem – HA Farm** ablakban kattintson a **Létrehozás**gombra.
4. Adja meg a beállításokat a a lépéseket a **Létrehozása a SharePoint 2013-ban nem – HA Farm** ablaktábla, és kattintson a **Létrehozás**gombra.

A nagy-elérhetőség SharePoint-farm kilenc virtuális gépeken futó ebben a konfigurációban áll.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

A kiszolgálófarm beállítása magasabb ügyfél terhelések, a külső SharePoint-webhely magas rendelkezésre állásának és az SQL Server AlwaysOn rendelkezésre állási csoportok teszteléshez egy SharePoint-farm is használhatja. Ebben a konfigurációban a SharePoint-alkalmazások fejlesztéséhez magas rendelkezésre állás környezetben is használhatja.

A magas rendelkezésre állás (kilenc-kiszolgáló) SharePoint-farm létrehozása:

1. Kattintson [ide](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Kattintson a **üzembe**.
3. A **SharePoint 2013-ban, HA Farm** ablakban kattintson a **Létrehozás**gombra.
4. Adja meg a beállításokat a hét lépéseit a **Létrehozása a SharePoint 2013-ban HA Farm** ablakban, és kattintson a **Létrehozás**gombra.

> [AZURE.NOTE] A **SharePoint 2013-ban nem – HA Farm** vagy a **SharePoint 2013-ban, HA Farm** nem hozható létre az Azure ingyenes próbaverziójára való.

Az Azure-portálra hoz létre, a következő farmok felhőben virtuális hálózatban az egy internetes webes jelenlét. Nincs vissza szeretne a szervezet hálózati hely közötti VPN vagy készült ExpressRoute kapcsolat.

> [AZURE.NOTE] Ha hoz létre a basic vagy magas rendelkezésre állás SharePoint-farmokon az Azure portálon, nem adhat meg egy meglévő erőforrás-csoportot. Ezt a korlátozást megkerüléséhez Azure PowerShell ezek farmok létrehozása. További tudnivalókért olvassa el a [fejlesztők/próba farmok létrehozása a SharePoint 2013-ban a Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell)című témakört.

## <a name="sharepoint-2016-farms"></a>A SharePoint 2016 farmok

[Ez a cikk](https://technet.microsoft.com/library/mt723354.aspx) az utasításokat követve kiszolgálófarm a következő egyetlen – a SharePoint Server 2016 tartalmaz.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>A SharePoint-farmokon kezelése

Felügyelheti a kiszolgálókat az alábbi farmok távoli asztali kapcsolaton keresztül. További tudnivalókért lásd: [a virtuális gépen jelentkezzen be](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

A központi felügyelet SharePoint-webhelyről beállíthatja a webhelyek, a SharePoint-alkalmazások és az egyéb funkciók. További tudnivalókért olvassa el a [Konfigurálása a SharePoint](http://technet.microsoft.com/library/ee836142.aspx)című témakört.

## <a name="next-steps"></a>Következő lépések

- Fedezze fel az Azure infrastruktúrájának szolgáltatásai [SharePoint konfiguráció](https://technet.microsoft.com/library/dn635309.aspx) szükséges.
