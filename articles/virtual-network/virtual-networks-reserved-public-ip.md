<properties
   pageTitle="Fenntartott IP |} Microsoft Azure"
   description="Fenntartott IP-címei és hogy miként kezelheti őket ismertetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/10/2016"
   ms.author="jdial" />

# <a name="reserved-ip-overview"></a>Fenntartott IP – áttekintés
IP-címek Azure-ban két kategóriába sorolhatók: dinamikus és fenntartott. Azure kezeli nyilvános IP-címek dinamikus alapértelmezés szerint. Ez azt jelenti, hogy az IP-cím használható egy adott felhőszolgáltatásba (virtuális) vagy egy virtuális vagy szerepkör-példány közvetlenül (ILPIP) eléréséhez időről-időre, módosíthatja, ha a források állnak a Leállítás vagy felszabadítása.

IP-címek módosítása megakadályozásához foglalhat IP-címet. Fenntartott IP-címei csak egy virtuális biztosítva, hogy a felhőbeli szolgáltatástól IP-címe lesz azonos még akkor is, mint az erőforrások leállítása vagy felszabadítása is használható. Ezenkívül meglévő dinamikus IP-címei egy virtuális IP-címre fenntartott használt alakíthatók.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, hogy miként lefoglalása az [erőforrás-kezelő telepítési modell](virtual-network-ip-addresses-overview-arm.md)statikus nyilvános IP-címet.

Győződjön meg arról, hogy megismeri [IP-címek](virtual-network-ip-addresses-overview-classic.md) működése Azure-ban.

## <a name="when-do-i-need-a-reserved-ip"></a>Mikor van szükség egy fenntartott IP?
- **Szeretne róla, hogy az IP-előfizetése lefoglalt**. Ha meg szeretné lefoglalása az előfizetéséből semmilyen körülmények között nem lesz feloldva IP-címet, egy fenntartott nyilvános IP kell használnia.  
- **Azt szeretné, hogy a felhőalapú szolgáltatás, akár leállítva vagy felszabadítása állapota (VMs) keresztül a kapcsolatának IP-címe**. Ha azt szeretné a szolgáltatás nem változik, IP-cím használatával érhető el, még akkor is, ha VMs a felhőszolgáltatásában leállítása vagy felszabadítása.
- **Szeretne róla, hogy a kimenő forgalom az Azure egy kiszámíthatóan IP-címet használja**. Előfordulhat, hogy a tűzfal konfigurált csak az adott IP-címek érkező forgalmat. Szerint lefoglalására IP-címre, tudni fogja, hogy a forrás IP-címét, és nem kell frissíteni a tűzfalszabályokat miatt egy IP-módosul.

## <a name="faq"></a>GYAKORI KÉRDÉSEK
1. Használhatom-e egy fenntartott IP az összes Azure szolgáltatás?  
  - Fenntartott IP-címei csak akkor használható, VMs és felhőalapú szolgáltatást példány szerepkörök elérhetővé tett egy virtuális keresztül.
1. Hány fenntartott IP-címei lehet?  
  - Ekkor az összes Azure előfizetésben 20 fenntartott IP-címei használata engedélyezett. Azonban kérhet további fenntartott IP-címei. Lásd az [előfizetést és a szolgáltatás korlátai](../azure-subscription-service-limits.md) lap további információt.
1. Van lehetőség a költség fenntartott IP-címei az?
  - [Fenntartott IP árak címadatok](http://go.microsoft.com/fwlink/?LinkID=398482) árinformációkat talál.
1. Hogyan lehet lefoglalása IP-címének?
  - PowerShell vagy az [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx) segítségével lefoglalása egy adott régióban IP-címet. Fenntartott IP-cím társítva az előfizetést. IP-címet az adatkezelési portál használatával nem lehet foglalni.
1. Van-e lehetőség a affinitás csoporttal VNets alapú?
  - Fenntartott IP-címei csak az által támogatott területi VNets. Nem támogatott VNets társított affinitás csoportok számára. Egy VNet társítása egy régió vagy affinitás csoport kapcsolatos további tudnivalókért lásd: a [területi VNets és affinitás csoportok](virtual-networks-migrate-to-regional-vnet.md).

## <a name="how-to-manage-reserved-vips"></a>Fenntartott VIP kezelése

Fenntartott IP-címei használata előtt hozzá kell adnia azt az előfizetést. Hozzon létre egy fenntartott IP nyilvános IP-címek elérhető *Központi USA -beli* hely a készletből, futtassa az alábbi PowerShell-parancsot:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

Figyelje meg, azonban, hogy nem adhat meg mi IP folyamatban van fenntartva. Ha szeretné megtekinteni, mely IP-címek vannak fenntartva az előfizetését, futtassa az alábbi PowerShell-parancsot, és figyelje meg, az értékek *ReservedIPName* és a *cím*:

    Get-AzureReservedIP

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

IP-címre van fenntartva, miután marad-előfizetéséhez társított mindaddig, amíg ki törölheti azt. A fent látható fenntartott IP törléséhez futtassa az alábbi PowerShell-parancsot:

    Remove-AzureReservedIP -ReservedIPName "MyReservedIP"

## <a name="how-to-reserve-the-ip-address-of-an-existing-cloud-service"></a>Hogyan kell az IP-címe egy meglévő felhőalapú szolgáltatást lefoglalása

A *- ServiceName* paraméter hozzáadásával foglalhat egy meglévő felhőalapú szolgáltatás IP-címét. Az IP-címe egy felhőalapú szolgáltatásba *TestService* *Központi USA -beli* hely lefoglalása, futtassa az alábbi PowerShell-parancsot:

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService


## <a name="how-to-associate-a-reserved-ip-to-a-new-cloud-service"></a>Hogy miként társítható egy új felhőszolgáltatásokba fenntartott IP
Az alábbi parancsfájl létrehoz egy új fenntartott IP, majd *TestService*nevű új felhőszolgáltatásokba társítja azt.

    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"

>[AZURE.NOTE] Amikor létrehoz egy fenntartott IP használata egy felhőalapú szolgáltatásba, továbbra is kell a virtuális hivatkozni használatával *virtuális:&lt;port száma >* a bejövő kommunikációhoz. IP-címre lefoglalására nem jelent csatlakozhat a virtuális közvetlenül. A foglalt IP a felhőbeli szolgáltatástól, amely a virtuális való lett telepítve van rendelve. Ha közvetlenül egy virtuális IP által csatlakozni szeretne, akkor konfigurálása példány szintű nyilvános IP-címre. Példány szintű nyilvános IP-címre olyan nyilvános IP-(más néven egy ILPIP), amely közvetlenül a virtuális van hozzárendelve. Nem foglalható. További információt talál [Példány szintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) .

## <a name="how-to-remove-a-reserved-ip-from-a-running-deployment"></a>A futó telepítési egy fenntartott IP eltávolításáról
Az új szolgáltatás a fenti parancsfájl létrehozott hozzáadott fenntartott IP-cím eltávolítása, futtassa az alábbi PowerShell-parancsot:

    Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService

>[AZURE.NOTE] Egy fenntartott IP futó telepítés eltávolítás nem távolítja a Foglalás az előfizetéséből. Akkor egyszerűen szabadítja az IP-előfizetése más erőforrás által használandó.

## <a name="how-to-associate-a-reserved-ip-to-a-running-deployment"></a>Hogy miként társítható egy fenntartott IP-futó telepítéshez
Az alábbi parancsfájl hoz létre egy új felhőszolgáltatásba *TestService2* az egy *TestVM2*nevű új virtuális nevű, és majd társít a meglévő fenntartott IP *MyReservedIP* nevű a felhőalapú szolgáltatást.

    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| New-AzureVM -ServiceName TestService2 -Location "Central US"
    Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2

## <a name="how-to-associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Hogy miként társítható egy fenntartott IP, ha egy felhőalapú szolgáltatásba szolgáltatás konfigurációs fájl használatával
Ha egy felhőalapú szolgáltatásba egy fenntartott IP szolgáltatás beállítása (CSCFG)-fájl használatával is társíthat. Az alábbi minta XML-fájl egy felhőalapú szolgáltatásba, egy névvel ellátott *MyReservedIP*fenntartott virtuális használandó konfigurálása mutatja be:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Következő lépések

- [IP-címek](virtual-network-ip-addresses-overview-classic.md) működésének megértése a klasszikus telepítési modell.

- Tudjon meg többet [fenntartott magánjellegű IP-címek](virtual-networks-reserved-private-ip.md).

- [Példány szint nyilvános IP (ILPIP)-címekről](virtual-networks-instance-level-public-ip.md)bővebb ismertetőt.
