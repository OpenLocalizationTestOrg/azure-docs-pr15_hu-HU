<properties 
   pageTitle="Nyilvános IP (ILPIP) szint példány |} Microsoft Azure"
   description="ILPIP (PIP), és hogy miként kezelheti őket ismertetése"
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

# <a name="instance-level-public-ip-overview"></a>Példány szintű nyilvános IP – áttekintés
Egy példány a egy nyilvános IP-címet, amely közvetlenül a virtuális vagy szerepkör-példányt, helyett a felhőalapú szolgáltatás, amely a virtuális vagy szerepkör példány tárolnak rendelhet szintű nyilvános IP (ILPIP). Ez a felhőalapú szolgáltatás rendelt virtuális (ezek olyan virtuális IP) helye nem igénybe vehet. Inkább érdemes további IP-címet, csatlakoztassa közvetlenül a virtuális vagy szerepkör példány is használhatja.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Megtudhatja, miként [végezheti el ezeket a lépéseket az erőforrás-kezelő modellt használja](virtual-network-ip-addresses-overview-arm.md). 

Győződjön meg arról, hogy megismeri [IP-címek](virtual-network-ip-addresses-overview-classic.md) működése Azure-ban.

>[AZURE.NOTE] Az elmúlt egy ILPIP is megtekinthetők és MEZŐPONT, amely a nyilvános IP. 

![ILPIP és virtuális közötti különbség](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Ahogy azt az 1, a felhőbeli szolgáltatástól érhető el a virtuális, használatával, miközben az egyes VMs általában érhetők el használatáról virtuális:&lt;port száma&gt;. Egy ILPIP rendel egy adott virtuális, hogy virtuális azok webböngészőn közvetlenül az IP-címet.

Azure-ban létrehozott egy felhőalapú szolgáltatásba, megfelelő DNS A-rekordokat automatikusan létrehozza a szolgáltatás révén a teljes tartománynevét (FQDN) való hozzáférés engedélyezése a tényleges virtuális használata helyett. Ugyanezt az eljárást ILPIP, a virtuális vagy szerepkör példányban teljesen minősített tartománynév helyett a ILPIP hozzáférést történik. Például ha hoz létre egy felhőalapú szolgáltatásba, *contosoadservice*nevű, és beállíthatja, hogy egy webes szerepkör nevű *contosoweb* a két példánya, Azure regisztrálja a következő példányok A-rekordokat:

- contosoweb\_IN_0.contosoadservice.cloudapp.net
- contosoweb\_IN_1.contosoadservice.cloudapp.net 

>[AZURE.NOTE] Az egyes virtuális vagy szerepkör példány csak egy ILPIP oszthatnak meg. Legfeljebb 5 ILPIP's előfizetésenként is használhatja. Ekkor ILPIP többszörös-hálózati VMs nem támogatott.

## <a name="why-should-i-request-an-ilpip"></a>Miért érdemes kérése egy ILPIP?
Ha szeretne csatlakozni, hogy a virtuális vagy szerepkör példány közvetlenül rendelt IP-címet is, nem oszlophivatkozás használatával a felhőben szolgáltatás virtuális:&lt;port száma&gt;, egy ILPIP kérése a virtuális vagy a szerepkör-példány.
- **Passive FTP** - úgy, hogy egy ILPIP a virtuális, a kaphat forgalom szinte minden olyan portot, akkor nem lesz kattintva nyissa meg szeretné kapni az adatforgalom zárólap. Lehetővé teszi például passzív FTP, ahol a portokat kiválasztva dinamikusan esetek.
- **Kimenő IP** - kimenő forgalmat a virtuális származó kerüljön a ILPIP forrásaként és egyedileg kell azonosítania a virtuális külső személynek.

## <a name="how-to-request-an-ilpip-during-vm-creation"></a>Hogy hogyan kérhet egy ILPIP virtuális létrehozása során.
Az alábbi PowerShell parancsprogramot hoz létre egy új felhőszolgáltatásba nevű *FTPService*, majd olvassa be a képet az Azure, és létrehoz egy virtuális *FTPInstance* a beolvasott kép felhasználásával nevű, állítja be a virtuális használni egy ILPIP és felveszi az új szolgáltatás a virtuális:

    New-AzureService -ServiceName FTPService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
  	| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
  	| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

## <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Hogyan lehet egy virtuális ILPIP adatainak beolvasása
Ha szeretné megtekinteni a fenti forgatókönyvvel létre virtuális ILPIP adatait, futtassa az alábbi PowerShell-parancsot, és tekintse meg az értékeket *PublicIPAddress* és *PublicIPName*:

    Get-AzureVM -Name FTPInstance -ServiceName FTPService

    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

## <a name="how-to-remove-an-ilpip-from-a-vm"></a>Ha el szeretne távolítani egy ILPIP egy virtuális hogyan
Távolítsa el a fenti parancsfájl a virtuális hozzáadni a ILPIP, futtassa az alábbi PowerShell-parancsot:
    
    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Remove-AzurePublicIP `
  	| Update-AzureVM

## <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Egy meglévő virtuális egy ILPIP hozzáadása
A virtuális a fenti parancsfájl használatával létrehozott egy ILPIP hozzáadásához a következő parancsot:

    Get-AzureVM -ServiceName FTPService -Name FTPInstance `
  	| Set-AzurePublicIP -PublicIPName ftpip2 `
  	| Update-AzureVM

## <a name="how-to-associate-an-ilpip-to-a-vm-by-using-a-service-configuration-file"></a>Hogy miként társítható egy ILPIP egy virtuális gép szolgáltatás konfigurációs fájl használatával
Egy ILPIP egy virtuális gép szolgáltatás beállítása (CSCFG)-fájl használatával is társíthat. Az alábbi minta XML-fájl egy felhőalapú szolgáltatásba, egy névvel ellátott *MyPublicIP* az szerepkör példány ILPIP használatára konfigurálni mutatja be: 
    
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <VirtualNetworkSite name="VNet"/>
        <AddressAssignments>
          <InstanceAddress roleName="VMRolePersisted">
            <Subnets>
              <Subnet name="Subnet1"/>
              <Subnet name="Subnet2"/>
            </Subnets>
            <PublicIPs>
              <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Következő lépések

- [IP-címek](virtual-network-ip-addresses-overview-classic.md) működésének megértése a klasszikus telepítési modell.

- Tudjon meg többet [fenntartott IP-címei](virtual-networks-reserved-public-ip.md).
 