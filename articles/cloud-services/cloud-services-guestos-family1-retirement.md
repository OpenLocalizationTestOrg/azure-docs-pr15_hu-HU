<properties
   pageTitle="A vendégként való bekapcsolódáshoz OS család 1 elavulása figyelje |} Microsoft Azure"
   description="Amikor az Azure Vendég OS család 1 elavulása történt és miként állapítható meg, ha érintett információt tartalmaz"
   services="cloud-services"
   documentationCenter="na"
   authors="raiye"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="10/24/2016"
   ms.author="raiye"/>



# <a name="guest-os-family-1-retirement-notice"></a>A vendégként való bekapcsolódáshoz OS család 1 elavulása értesítés

Az operációs rendszer család 1 elavulása először bejelentve a következő 2013 június 1.

**Sept 2, 2014-es** Az Azure vendég operációs rendszeren Vendég család 1.x, amely a Windows Server 2008 operációs rendszer alapul, ugyan hivatalos már inaktív. Új szolgáltatásainak üzembe, vagy használja a család 1 meglévő szolgáltatások frissítése az összes kísérletek sikertelen lesz a megjelenő hibaüzenet, amely közli, hogy a vendégként való bekapcsolódáshoz OS család 1 van többé nem érhető el.

**November 3, 2014-es** A vendégként való bekapcsolódáshoz OS család 1 kiterjesztett technikai támogatással befejeződött, és teljesen már inaktív. Továbbra is a család 1 szolgáltatások lesz hatással lehet. Azokat a szolgáltatásokat bármikor leállhat azt. A szolgáltatások tovább futnak, kivéve, ha saját kezűleg frissítenie őket saját maga garanciát nem.

Ha további kérdései vannak, látogasson el a [Felhőbeli szolgáltatások fórumok](http://social.msdn.microsoft.com/Forums/home?forum=windowsazuredevelopment&filter=alltypes&sort=lastpostdesc) vagy [Azure ügyfélszolgálatát](https://azure.microsoft.com/support/options/).




## <a name="are-you-affected"></a>Érintettek?

A felhőalapú szolgáltatások érinti, ha teljesül a következők valamelyikét:

1. Egy érték, akkor "osFamily ="1"explicit módon megadva az ServiceConfiguration.cscfg fájlban a felhőalapú szolgáltatáshoz.
2. Nincs explicit módon megadva az ServiceConfiguration.cscfg fájlban a felhőben szolgáltatásbeli osFamily értéket. Jelenleg, a rendszer használja az alapértelmezett érték az "1" Ebben az esetben.
3. Az Azure klasszikus portált a vendég operációs rendszer családi érték jelez "Windows Server 2008".

Megkeresése, amely a felhőbeli szolgáltatások futtatja a mely operációs rendszer család, futtathatja az alábbi parancsfájl Azure PowerShell, az, hogy be kell [állítania Azure PowerShell](../powershell-install-configure.md) először. A parancsfájl további részleteket lásd: [Azure Vendég OS család 1 vége a leírási_idő: június 2014-es](http://blogs.msdn.com/b/ryberry/archive/2014/04/02/azure-guest-os-family-1-end-of-life-june-2014.aspx). 

```Powershell
foreach($subscription in Get-AzureSubscription) {
    Select-AzureSubscription -SubscriptionName $subscription.SubscriptionName

    $deployments=get-azureService | get-azureDeployment -ErrorAction Ignore | where {$_.SdkVersion -NE ""}

    $deployments | ft @{Name="SubscriptionName";Expression={$subscription.SubscriptionName}}, ServiceName, SdkVersion, Slot, @{Name="osFamily";Expression={(select-xml -content $_.configuration -xpath "/ns:ServiceConfiguration/@osFamily" -namespace $namespace).node.value }}, osVersion, Status, URL
}
```

A felhőalapú szolgáltatások OS család 1 elavulása lesz hatással, ha a művelet eredményébe osFamily oszlopában üres, vagy a "1".

## <a name="recommendations-if-you-are-affected"></a>Ha érinti javaslatok

Azt javasoljuk, hogy a Felhőbeli szolgáltatástól szerepkörök áttelepítése a vendégként való bekapcsolódáshoz támogatott operációs rendszer családok egyikét:

**Vendég OS családi 4.x** – a Windows Server 2012 R2 *(ajánlott)*

1. Győződjön meg arról, hogy az alkalmazás a .NET-keretrendszer 4.0, 4.5 vagy 4.5.1 használ SDK 2.1-es vagy újabb verziója.
2. Telepítsen újra a felhőalapú szolgáltatásba, és állítsa az "4" osFamily attribútum a ServiceConfiguration.cscfg fájlban.


**Vendég OS családi 3.x** – Windows Server 2012-ben

1. Győződjön meg arról, hogy az az alkalmazás a .NET-keretrendszer 4.5 4.0 vagy a SDK 1,8 vagy újabb verzióját használja-e.
2. Telepítsen újra a felhőalapú szolgáltatásba, és állítsa a osFamily attribútum "3" a ServiceConfiguration.cscfg fájlban.


**Vendég OS családi 2.x** – a Windows Server 2008 R2 rendszerben

1. Győződjön meg arról, hogy az alkalmazást használ-e SDK 1.3-as és újabb a .NET-keretrendszer 3.5-ös vagy 4.0.
2. Telepítsen újra a felhőalapú szolgáltatásba, és állítsa az "2" osFamily attribútum a ServiceConfiguration.cscfg fájlban.


## <a name="extended-support-for-guest-os-family-1-ended-nov-3-2014"></a>A vendégként való bekapcsolódáshoz OS család 1 kiterjesztett technikai támogatással befejeződött november 3, 2014-es
A vendégként való bekapcsolódáshoz OS család 1 felhőszolgáltatások már nem támogatottak. Ki a család 1 amint lehetséges, hogy a szolgáltatási zavarok elkerülése áttelepítése.  

## <a name="next-steps"></a>Következő lépések
Tekintse át a [vendégként való bekapcsolódáshoz operációs rendszer által kiadott](cloud-services-guestos-update-matrix.md)legújabb.
