<properties 
   pageTitle="Hogyan kell beállítani egy statikus belső magánjellegű IP-cím"
   description="Statikus belső IP-címei (immerzióban), és hogy miként kezelheti őket ismertetése"
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
   ms.date="03/22/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Hogyan kell beállítani egy statikus belső magánjellegű IP-cím (klasszikus) PowerShell használatával
A legtöbb esetben nem szükséges adja meg a virtuális gép statikus belső IP-címet. Virtuális hálózatban VMs automatikusan kap egy belső IP-címének egy cellatartományból, megadott feltételeknek. De van ilyesmire lehetőség a statikus IP-cím megadása egy adott virtuális bizonyos esetekben. Ha például a virtuális, látogasson el a DNS futtatása, vagy egy tartományvezérlőnek lesz. A belső statikus IP-címet a virtuális még egy leállítás/deprovision állam keresztül fog mutatni. 

> [AZURE.IMPORTANT] Azure van két különböző telepítési modellekkel létrehozásáról és használatáról az erőforrások: [az erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md). Ez a cikk bemutatja, hogy a klasszikus telepítési modellt használja. Azt javasoljuk, hogy a legtöbb új telepítések használja az [erőforrás-kezelő telepítési modell](virtual-networks-static-private-ip-arm-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Ha érhető el egy adott IP-cím ellenőrzése
Annak ellenőrzéséhez, ha az IP-cím *10.0.0.7* érhető el a *TestVnet*nevű vnet, futtassa az alábbi PowerShell-parancsot, és ellenőrizze a az érték az *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

>[AZURE.NOTE] Ha szeretne egy biztonságos környezet követés útmutatását követve hozzon létre egy vnet [létrehozása egy virtuális hálózati](virtual-networks-create-vnet-classic-portal.md) *TestVnet* nevű, és győződjön meg arról, akkor használja a *10.0.0.0/8* címterületet tesztelheti a fenti parancsot.

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Statikus belső IP-cím megadása, egy virtuális létrehozása
Az alábbi PowerShell parancsprogramot hoz létre egy új felhőszolgáltatásba *TestService*nevű, majd olvassa be a képet az Azure, majd létrehoz egy virtuális *TestVM* nevű fájlt a beolvasott kép új felhőalapú szolgáltatás, állítja be a virtuális el szeretné helyezni egy alhálózat nevű *alhálózat-1*és a virtuális statikus belső IP-cím *10.0.0.7* állítja be:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzureSubnet –SubnetNames Subnet-1 `
  	| Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
  	| New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Hogyan lehet egy virtuális statikus belső IP adatainak beolvasása
A fenti forgatókönyvvel létre virtuális belső statikus IP-információ megtekintése, az alábbi PowerShell-parancsot, és tekintse meg az értékeket az *IP-címe*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Statikus belső IP-cím eltávolítása egy virtuális
A statikus belső IP-cím eltávolítása a virtuális a fenti parancsfájl hozzáadni, futtassa az alábbi PowerShell-parancsot:
    
    Get-AzureVM -ServiceName TestService -Name TestVM `
  	| Remove-AzureStaticVNetIP `
  	| Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Statikus belső IP-cím felvétele egy meglévő virtuális
Belső statikus hozzáadása a virtuális IP-létre a fenti runt parancsfájl használatával, a következő parancsot:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
  	| Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
  	| Update-AzureVM

## <a name="next-steps"></a>Következő lépések

[Fenntartott IP](virtual-networks-reserved-public-ip.md)

[IP-példány szintű nyilvános (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Fenntartott IP REST API-hoz](https://msdn.microsoft.com/library/azure/dn722420.aspx)
 
