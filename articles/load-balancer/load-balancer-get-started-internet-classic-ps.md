<properties
   pageTitle="Első lépések megtételében szemben lévő terheléselosztó a PowerShell használatá Klasszikus módú internetes |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó Klasszikus módú PowerShell használatával"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/05/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-powershell"></a>Első lépések megtételében egy internetes PowerShell terheléselosztó (klasszikus)

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó Azure erőforrás-kezelő használatával](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]



## <a name="set-up-load-balancer-using-powershell"></a>Állítsa be a PowerShell használatá terheléselosztó

A powershell használatá terheléselosztó beállításához kövesse az alábbi lépéseket:

1. Ha még sosem használt Azure PowerShell, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell](../../articles/powershell-install-configure.md) , és a képernyőn megjelenő utasításokat követve teljes mértékben a végén jelentkezzen be az Azure, és jelölje ki azt az előfizetést.


2. Miután létrehozott egy virtuális gép, PowerShell-parancsmagok egy terheléselosztó hozzáadása egy virtuális gép belül ugyanazt a felhőalapú szolgáltatást használhatja.

A következő példa ad hozzá egy terheléselosztó úgynevezett "webfarm" szolgáltatás "mytestcloud" (vagy myctestcloud.cloudapp.net), a végpontokat a terheléselosztó hozzáadása a "weben 1" és a "weben 2" nevű virtuális gépeken futó cloud beállítása. A terheléselosztó hálózati forgalmat a 80-as port kap, és a betöltés egyenlege között a virtuális gépeken futó határozza meg a helyi végpont (a Ez esetben 80-as port) TCP használatával.


### <a name="step-1"></a>Lépés: 1
Az első virtuális "weben 1" terheléselosztása végpont létrehozása

    Get-AzureVM -ServiceName "mytestcloud" -Name "web1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

### <a name="step-2"></a>Lépés: 2

Hozzon létre egy másik végpontot betöltés terheléselosztó beállítása azonos nevű használata második virtuális "weben 2"

    Get-AzureVM -ServiceName "mytestcloud" -Name "web2" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 80 -LBSetName "WebFarm" -ProbePort 80 -ProbeProtocol "http" -ProbePath '/' | Update-AzureVM

## <a name="remove-a-virtual-machine-from-a-load-balancer"></a>A virtuális gép eltávolítása egy terheléselosztó

Eltávolítás-AzureEndpoint használhatja a terheléselosztó egy virtuális gép végpont eltávolítása

    Get-azureVM -ServiceName mytestcloud  -Name web1 |Remove-AzureEndpoint -Name httpin| Update-AzureVM

## <a name="next-steps"></a>Következő lépések

Is [első lépések megtételében egy belső terheléselosztó](load-balancer-get-started-ilb-classic-ps.md) és [terjesztési mód](load-balancer-distribution-mode.md) egy especific betöltés terheléselosztó hálózati forgalmának engedélyezésére elemeire vonatkozó típusának beállítása.

Ha az alkalmazás hagyja kapcsolat életben mögött egy terheléselosztó kiszolgálók, akkor is többet szeretne tudni [a terheléselosztó üresjárati TCP időtúllépés beállításainak](load-balancer-tcp-idle-timeout.md). Tudnivalók az Azure terheléselosztó használatakor üresjárati kapcsolati viselkedés segítik.

