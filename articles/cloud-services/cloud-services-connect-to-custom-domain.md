<properties
  pageTitle="Csatlakozás egy felhőalapú szolgáltatásba egy egyéni tartományvezérlőnek |} Microsoft Azure"
  description="Megtudhatja, hogy miként való csatlakozáshoz a webes/dolgozói szerepkörök AD egyéni tartományt PowerShell és az Active Directory tartományi bővítmény használatával"
  services="cloud-services"
  documentationCenter=""
  authors="Thraka"
  manager="timlt"
  editor=""/>

  <tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/21/2016"
    ms.author="adegeo"/>

# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure Cloud Services szerepkörök csatlakozik egy egyéni üzemeltetett az Azure Active Directory tartományi vezérlő

Azt fogja először egy virtuális hálózat beállítása (VNet) Azure-ban. Ezután hozzáadja azt a VNet az Active Directory tartományvezérlőnek (szolgáltatott az Azure virtuális gépen). Fog ezután azt az előre létrehozott VNet hozzáadása meglévő felhőalapú szolgáltatás szerepkörök, és ezt követően a tartományvezérlőnek csatlakoztatása őket.

Mielőtt hozzákezdene azt, néhány dolgot szem előtt kell tartania:

1.  Ebben az oktatóanyagban PowerShell használ, győződjön meg arról, hogy az Azure PowerShell telepítése, és készen áll a küldésre, adja. Segítség kérése az Azure PowerShell beállítása, hogy megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md).

2.  Az Active Directory tartományi vezérlő és a webes/dolgozó szerepkör-példányok kell a VNet.

Kövesse a cikk részletesen ismerteti, és ha problémák merülnek fel, visszajelzés küldése az alábbi. Valaki fog vissza szeretne lépni, (Igen, akkor olvassa el megjegyzések).

3. A hálózat, amelyre hivatkozik a felhőben <mark>kell lennie</mark> a **Klasszikus virtuális hálózati**szolgáltatás.

## <a name="create-a-virtual-network"></a>Hozzon létre egy virtuális hálózaton

Létrehozhat egy virtuális hálózati az Azure klasszikus portálon vagy a PowerShell használatával Azure-ban. Az ebben az oktatóanyagban fogjuk használni PowerShell. Létrehozása az Azure klasszikus portálon virtuális hálózatot, olvassa el a [Virtuális hálózati létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)című témakört.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

Miután elvégezte a virtuális hálózat beállításával, szüksége lesz az Active Directory tartományi vezérlő létrehozása. Ebben az oktatóanyagban azt fogja állítani az Active Directory tartományi vezérlő az Azure virtuális gépen.

Ehhez az alábbi parancsokkal Powershellen keresztül virtuális gép létrehozása:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>A tartományvezérlőnek a virtuális gépen előléptetése
A virtuális gépen konfigurálása az Active Directory tartományvezérlőnek, meg kell jelentkezzen be a virtuális, és adja meg.

Jelentkezzen be a virtuális, a RDP-fájl első Powershellen keresztül, az alábbi parancsokat használhatja.

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Miután bejelentkezett a virtuális, a telepítő a virtuális gép az Active Directory tartományvezérlőnek a lépésenkénti útmutató következő [az Active Directory tartományi vezérlő ügyfél beállítása](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx)a.

## <a name="add-your-cloud-service-to-the-virtual-network"></a>A felhőalapú szolgáltatás hozzáadása a virtuális hálózathoz

Következő lépésként meg kell a felhőalapú szolgáltatás üzembe hozzáadása az imént létrehozott VNet. Ehhez a felhőalapú szolgáltatás cscfg módosítása a Visual Studio vagy a szerkesztővel cscfg a vonatkozó szakaszokat hozzáadásával.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Ezután a cloud services project összeállítása és Azure kell üzembe. A felhőalapú szolgáltatások csomag bevezetéshez Azure segítséget című témakörben kaphat [létrehozása és a Deploy egy felhőalapú szolgáltatásba](cloud-services-how-to-create-deploy.md#deploy)

## <a name="connect-your-webworker-roles-to-the-domain"></a>A webes/dolgozói szerepkör(ök) csatlakoztatása a tartomány

Miután a felhőbe szolgáltatási projekt Azure telepítve van, a szerepkör-példányok csatlakozni az egyéni Active Directory-tartományt az Active Directory tartományi bővítmény használatával. Az Active Directory tartományi bővítmény hozzáadása a meglévő cloud services telepítése, és az egyéni tartomány kapcsolódhat, hajtsa végre a következő parancsok végrehajtása a PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

És ez azt.

Felhőalapú, szolgáltatások most már az egyéni tartomány vezérlő összekapcsolhatók. Ha azt szeretné, ha többet szeretne megtudni a konfigurálása az Active Directory tartományi kiterjesztése a különböző beállításokat, használja a PowerShell súgója alább látható módon.

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
