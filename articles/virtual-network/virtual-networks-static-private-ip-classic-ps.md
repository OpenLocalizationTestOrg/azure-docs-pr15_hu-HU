<properties 
   pageTitle="Statikus magánjellegű IP-cím beállíthatja a PowerShell használatá Klasszikus módú |} Microsoft Azure"
   description="Statikus magánjellegű IP-címei (immerzióban), és hogyan kezelheti a Klasszikus módú és PowerShell ismertetése"
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn"
   tags="azure-service-management"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/02/2016"
   ms.author="jdial" />

# <a name="how-to-set-a-static-private-ip-address-classic-in-powershell"></a>Hogyan kell beállítani egy privát statikus IP-cím (klasszikus) PowerShell

[AZURE.INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[AZURE.INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Kezelheti [az erőforrás-kezelő telepítési modell statikus magánjellegű IP-címet](virtual-networks-static-private-ip-arm-ps.md).

[AZURE.INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

A minta PowerShell-parancsok az alábbi fejlemények már létrehozott egy egyszerű környezetben. Ha szeretne a dokumentumban megjelenített parancsokat, először összeállítása a [Create a VNet](virtual-networks-create-vnet-classic-netcfg-ps.md)ismertetett tesztkörnyezetben.

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Ha érhető el egy adott IP-cím ellenőrzése
Annak ellenőrzéséhez, ha az IP-cím *192.168.1.101* érhető el a *TestVNet*nevű VNet, futtassa az alábbi PowerShell-parancsot, és ellenőrizze a az érték az *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Várt kimenet:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>A magánjellegű statikus IP-cím megadása, egy virtuális létrehozásakor
Az alábbi PowerShell parancsprogramot hoz létre egy új felhőszolgáltatásba nevű *TestService*, majd olvassa be a képet az Azure, létrehoz egy virtuális *DNS01* nevű fájlt a beolvasott kép új felhőalapú szolgáltatás, beállítása *FrontEnd*nevű alhálózat kell a virtuális és *192.168.1.7* egy privát statikus IP-cím beállítása a virtuális:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Várt kimenet:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Hogyan statikus magánjellegű IP-cím adatait egy virtuális beolvasása
Statikus magánjellegű IP-cím információit a fenti forgatókönyvvel létre virtuális megtekintéséhez futtassa az alábbi PowerShell-parancsot, és tekintse meg az értékeket az *IP-címe*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Várt kimenet:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>A magánjellegű statikus IP-cím eltávolítása egy virtuális
Ha el szeretné távolítani a magánjellegű statikus IP-cím hozzáadni a virtuális a fenti parancsfájl futtassa az alábbi PowerShell-parancsot:
    
    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Várt kimenet:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>A magánjellegű statikus IP-cím felvétele egy meglévő virtuális
Statikus hozzáadni személyes IP-címet a fenti runt az parancsfájl használatával létrehozott virtuális a következő parancsot:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Várt kimenet:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="next-steps"></a>Következő lépések

- [Fenntartott nyilvános IP](virtual-networks-reserved-public-ip.md) -címekről bővebb ismertetőt.
- [Példány szintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) -címekről bővebb ismertetőt.
- Nézze meg a [fenntartott IP REST API-khoz](https://msdn.microsoft.com/library/azure/dn722420.aspx).
