<properties
    pageTitle="A PowerShell Azure virtuális mindig rendelkezésre állási csoport beállítása"
    description="Ebben az oktatóanyagban a klasszikus telepítési modell készült az erőforrásokat használó, és a PowerShell használatával mindig a elérhetősége csoport létrehozása az Azure."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/22/2016"
    ms.author="mikeray" />

# <a name="configure-always-on-availability-group-in-azure-vm-with-powershell"></a>A PowerShell Azure virtuális mindig rendelkezésre állási csoport beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő: sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Erőforrás-kezelő: kézi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasszikus: felhasználói felület](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasszikus: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

> [AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Azure virtuális gépeken futó (VMs) segíthetnek adatbázis-rendszergazdák alsó elérhetőséget SQL Server rendszeren költségét végrehajtásához. Ebből az oktatóanyagból megtudhatja, hogy hogyan végrehajtása egy rendelkezésre állási csoport használata SQL Server mindig a-végpont belül Azure környezetben. Az oktatóanyag végén az SQL Server mindig a megoldást Azure-ban a következő elemekből áll:

- Több alhálózat, beleértve a előtér és a háttér-alhálózat tartalmazó virtuális hálózaton

- A tartományvezérlőnek Active Directory (AD) tartománnyal

- Két SQL Server VMs a háttéradatbázist az alhálózathoz rendszerbe, és az Active Directory tartományhoz

- A 3-csomópont WSFC fürtre csomópont többsége kvórum modell

- Az elérhetőség adatbázis két szinkron jóváhagyás kópiákkal egy rendelkezésre állási csoport

Ebben az esetben az az egyszerűség kedvéért nem a költség hatékonyságának vagy egyéb tényezők a Azure választja. Két-replika elérhetősége csoport VMs számát méretűre például annak érdekében, hogy a tartományvezérlőnek a kvórum tanúsító fájlmegosztás a 2-csomópont WSFC fürt használatával számítási óra Azure-ban a mentés. Ez a módszer virtuális számának csökkenti a fenti konfiguráció egyet.

Ebben az oktatóanyagban mutatja, hogy a lépéseket a fent ismertetett megoldást beállítása nélkül a minden egyes lépés részleteinek kidolgozása íródott. Ezért helyett grafikus beállítási lépések láthatók, akkor átvezetik gyorsan minden egyes lépés a parancsfájlok PowerShell. Azt feltételezi, hogy a következőket:

- A virtuális gép előfizetés Azure fiókkal már rendelkezik.

- Telepítve van az [Azure PowerShell-parancsmagok](../powershell-install-configure.md).

- Már rendelkezik a helyszíni megoldások mindig a elérhetőségét a csoportok tömör megértése. További tudnivalókért lásd: [Mindig a elérhetősége csoportok (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-to-your-azure-subscription-and-create-the-virtual-network"></a>Azure-előfizetéséhez hozhat létre, és a virtuális hálózat

1. A helyi számítógépen PowerShell ablakban az Azure modul importálása, közzétételi beállítások fájl letöltése a számítógépre és Azure-előfizetése csatlakozik a PowerShell-munkamenetet a letöltött közzétételi beállítások importálása.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    A **Get-AzurePublishgSettingsFile** parancs automatikusan létrehoz egy felügyeleti Azure tanúsítvány letölti a számítógépen. A böngésző automatikusan megnyílik, és rákérdez, hogy adja meg az Azure előfizetés a Microsoft-fiók hitelesítő adatait. A letöltött **.publishsettings** fájlt az Azure előfizetés kezeléséhez szükséges összes információkat tartalmazza. Miután mentette a fájlt egy helyi könyvtár, importálja az **Importálás-AzurePublishSettingsFile** paranccsal.

    >[AZURE.NOTE] A publishsettings fájlt tartalmazza a hitelesítő adatok (nem titkosított), amellyel az Azure előfizetések és szolgáltatások felügyelete. Biztonsági célszerű ezt a fájlt, hogy kívüli (például a mappában Libraries\Documents) a forrás könyvtárak ideiglenes tárolni, törölje az importálás befejeződése után. Rosszindulatú átvevő hozzáférést a publishsettings fájl szerkesztése, létrehozása és törlése a Azure szolgáltatások.

1. Adja meg a felhőben informatikai infrastruktúrát létrehozásához használt változók sorozata.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Figyelje meg, annak érdekében, hogy a parancsok sikeres lesz később a következő:

    - Változók **$storageAccountName** és **$dcServiceName** egyedinek kell lennie mivel azonosíthatja a felhőalapú tárolási fiók és a felhő kiszolgálót, a kurzor az interneten.

    - Változók **$affinityGroupName** és **$virtualNetworkName** megadott nevek megtörténik a dokumentumban virtuális hálózati konfigurációja, hogy később fogja használni.

    - **$sqlImageName** a virtuális képet tartalmazó SQL Server 2012 Service Pack 1 Enterprise Edition frissített nevét adja meg.

    - Az egyszerűség kedvéért **Contoso! 000** ugyanazt a jelszót a teljes oktatóanyag mindenütt használatban.

1. Hozzon létre egy affinitás csoportot.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

1. Hozzon létre egy virtuális hálózati konfigurációs fájl importálásával.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    A konfigurációs fájl az alábbi XML-dokumentum tartalmazza. Röviden egy virtuális hálózati **ContosoNET** nevű **ContosoAG**nevű affinitás csoportjában adja meg, és a cím terület **10.10.0.0/16** , illetve két alhálózat, **10.10.1.0/24** és **10.10.2.0/24**, amelyek az első alhálózat és alhálózat, illetve biztonsági rendelkezik. Az első alhálózat, ahol ügyfélalkalmazásokban, például a Microsoft SharePoint is elhelyezhet a hátsó alhálózat az SQL Server VMs elhelyezni. Ha a **$affinityGroupName** és **$virtualNetworkName** változók korábbi módosításához az alábbi a megfelelő neveket is módosítania kell.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

1. A affinitás csoport létrehozott, és állítsa be azt az aktuális tárterület-fiókként az előfizetése társított tárterület-fiók létrehozása.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

1. Az Adatközpont kiszolgálót hozhat létre új felhőalapú szolgáltatás és az elérhetőség beállítása.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Sorozat védőeszközön parancsok hajtsa végre a következő műveleteket:

    - **Új-AzureVMConfig** a virtuális konfiguráció hoz létre.

    - **Hozzáadás-AzureProvisioningConfig** egy különálló a Windows Server konfigurációs paraméterek adja vissza.

    - **Hozzáadás-AzureDataDisk** összeadja az adatok lemezre adattárolásra az Active Directory, a beállítás értéke Nincs gyorsítótárazás használt.

    - **Új-AzureVM** hoz létre egy új, felhőalapú szolgáltatásba, és az új Azure virtuális létrehozza az új felhőszolgáltatásában.

1. Várja meg a teljes mértékben kiépítése, és töltse le a távoli asztali fájlt a munkakönyvtár új virtuális. Az új Azure virtuális kiépítése hosszú ideig tart, mivel a közben ismétlése továbbra is az új virtuális lekérdezik, amíg a használatra kész.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

Az Adatközpont kiszolgáló sikeresen most már kiépítve. Ezután az Active Directory tartományi fog konfigurálni a Adatközpont kiszolgálón. Hagyja a PowerShell ablakban nyissa meg a helyi számítógépre. Újra hozhat létre a két SQL Server VMs fogja használni.

## <a name="configure-the-domain-controller"></a>A tartományvezérlőnek konfigurálása

1. A távoli asztali fájl megnyitása az Adatközpont kiszolgáló csatlakozik. A rendszergazdai jogosultságokkal AzureAdmin felhasználónév és jelszó használata **Contoso! 000**, az új virtuális létrehozásakor megadott.

1. Rendszergazdai módban PowerShell-ablak megnyitása

1. Futtassa a következő **DCPROMO. EXE** parancs az adatok könyvtárak M meghajtón, **vallalat.kontraktor.hu** tartomány beállítása.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Ha befejeződött a parancs, a virtuális automatikusan újraindul.

1. Csatlakozzon az Adatközpont kiszolgáló ismét a távoli asztali fájl megnyitása. Ebben az esetben **CORP\Administrator**jelentkezzen.

1. Egy PowerShell-ablak megnyitása rendszergazdai módban, és importálja a Powershellhez Active Directory modult használja a következő parancsot:

        Import-Module ActiveDirectory

1. A következő parancsokat három felhasználók hozzáadása a tartomány.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    Állítsa be az SQL Server szolgáltatást példányait, a WSFC fürt és az elérhetőség csoport kapcsolódó minden **CORP\Install** szolgál. **CORP\SQLSvc1** és **CORP\SQLSvc2** segítségével az SQL Server szolgáltatást partnerként a két SQL Server VMs.

1. Ezután a következő parancsokat a engedélyezhetik **CORP\Install** a számítógép-objektumok létrehozása a tartományban.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    A fent ismertetett globálisan egyedi azonosítója a számítógépen objektumtípus GUID azonosítója. A **CORP\Install** fiók létrehozásához az az aktív közvetlen objektumok a WSFC fürt az **Összes tulajdonság olvasása** és a **Számítógép-objektumok létrehozása** engedély szükséges. Az **Összes tulajdonság olvasása** engedélyt már kapja CORP\Install alapértelmezés szerint így nem kell kifejezetten megadását. A WSFC fürt létrehozásához szükséges engedélyekkel kapcsolatos további tudnivalókért lásd: [feladatátvevő fürt lépésenkénti útmutató: fiókok konfigurálása az Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Most, hogy végzett konfigurálása az Active Directory és a felhasználó objektumok, hozzon létre két SQL Server VMs, és vegye fel őket a tartományba.

## <a name="create-the-sql-server-vms"></a>Az SQL Server VMs létrehozása

1. Továbbra is használhatja a PowerShell ablak, amely meg van nyitva, a helyi számítógépre. A következő további változók meghatározása:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Az IP-cím **10.10.0.4** általában van hozzárendelve a első virtuális hoz létre a **10.10.0.0/16** alhálózat a Azure virtuális hálózat. Ellenőriznie kell **IPCONFIG**futtatásával: az Adatközpont kiszolgáló címét.

1. A következő parancsokat védőeszközön a WSFC fürt, **ContosoQuorum**nevű a első virtuális létrehozásához:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Megjegyzés: a fenti parancs kapcsolatban a következő:

    - **Új-AzureVMConfig** a virtuális konfiguráció kívánt elérhetőségének beállítása nevű hoz létre. A későbbi VMs létrejön a elérhetőségének beállítása névvel, így elérhetőségét mindazon tartományhoz.

    - **Hozzáadás-AzureProvisioningConfig** a virtuális az Active Directory-tartományhoz létrehozott fűz össze.

    - A telefon vissza alhálózat a virtuális **Set-AzureSubnet** helyezi.

    - **Új-AzureVM** hoz létre egy új, felhőalapú szolgáltatásba, és az új Azure virtuális létrehozza az új felhőszolgáltatásában. A **DnsSettings** paraméterrel, hogy a DNS-kiszolgáló az új felhőszolgáltatásában kiszolgálóinak van-e a IP cím **10.10.0.4**, amely kiszolgáló Adatközpont IP-címét. Ez a paraméter ahhoz, hogy a felhőbeli szolgáltatástól új VMs csatlakozzon, az Active Directory tartományi sikeresen van szükség. A paraméter nélkül kell IPv4 beállításainak manuális beállítása a virtuális való használatára az Adatközpont az elsődleges DNS-kiszolgáló után a virtuális már kiépítve, és kattintson a virtuális csatlakozzon, az Active Directory tartományi.

1. A következő parancsokat védőeszközön az SQL Server VMs **ContosoSQL1** és **ContosoSQL2**nevű létrehozásához.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Megjegyzés: a fenti parancsok kapcsolatban a következő:

    - **Új-AzureVMConfig** az Adatközpont kiszolgáló elérhetőségének beállítása azonos nevét használja, és használja az SQL Server 2012 Service Pack 1 Enterprise Edition képét a virtuális gépek galéria. Azt is állítja be az operációs rendszer lemezen olvasható gyorsítótárazás csak (nincs írási gyorsítótárazás). Ajánlott az adatbázis-fájlok áttelepítése külön adatok lemezen, hogy a virtuális csatolása és a nincs további állította be, vagy írási gyorsítótárazás. A személyes találkozás azonban nem írási gyorsítótárazás operációs rendszer eltávolítása, mivel az nem lehet eltávolítani, olvassa el az operációs rendszer lemezen gyorsítótárazás.

    - **Hozzáadás-AzureProvisioningConfig** a virtuális az Active Directory-tartományhoz létrehozott fűz össze.

    - A telefon vissza alhálózat a virtuális **Set-AzureSubnet** helyezi.

    - **Hozzáadás-AzureEndpoint** hozzáadása access végpontok, így ügyfélalkalmazásokban is elérheti ezeket az SQL Server services-példányok az interneten. Különböző portokat ContosoSQL1 és ContosoSQL2 kapják.

    - **Új-AzureVM** az új SQL Server virtuális ugyanazt a felhőalapú szolgáltatást, mint ContosoQuorum hoz létre. Ha azt szeretné, hogy szeretné az elérhetőség ugyanazok, be kell jelölnie a VMs ugyanazt a felhőalapú szolgáltatást.

1. Várja meg az egyes virtuális teljesen kiépítése, és töltse le a távoli asztali fájlt a munkakönyvtár. A Váltás a három új VMs végighaladhat, és végrehajtja a legfelső szintű kapcsos zárójelben minden egyes őket.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until the VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    Az SQL Server VMs most kiépített környezetet és fut, de vannak telepítve SQL Server-alapértelmezett beállításokkal.

## <a name="initialize-the-wsfc-cluster-vms"></a>A WSFC fürt VMs inicializálni

Ebben a részben, módosítania kell a három kiszolgálók, használhatja a WSFC fürt és az SQL Server telepítéséhez. Kifejezetten:

- (Az összes kiszolgálón) A **Feladatátvételét** funkció telepítéséhez szükséges.

- (Az összes kiszolgálón) Hogy fel kell vennie **CORP\Install** a gép **rendszergazda**.

- (ContosoSQL1 és ContosoSQL2 csak) Hogy fel kell vennie **CORP\Install** **rendszergazdák** szerepkör az alapértelmezett adatbázisban.

- (ContosoSQL1 és ContosoSQL2 csak) Meg kell **NT AUTHORITY\System** egy jelentkezzen be az alábbi engedélyek megadása:

    - Minden rendelkezésre állási csoport módosítása

    - Csatlakozás SQL

    - Nézet kiszolgáló állapota

- (ContosoSQL1 és ContosoSQL2 csak) A **TCP** -jegyzőkönyv már engedélyezve van az SQL Server virtuális. Azonban van szüksége a tűzfal távoli eléréséhez, az SQL Server megnyitásához.

Ettől kezdve készen áll indításához. **ContosoQuorum**kezdve, kövesse az alábbi lépéseket:

1. **ContosoQuorum** csatlakozzon a távoli asztali fájlok indítása. A rendszergazdai jogosultságokkal **AzureAdmin** felhasználónév és jelszó használata **Contoso! 000**, a VMs létrehozásakor megadott.

1. Győződjön meg arról, hogy a számítógép lett sikeresen csatlakoztak az **vallalat.kontraktor.hu**.

1. Megvárja, amíg fut az automatizált inicializálni feladatok a folytatás előtt befejezéséhez az SQL Server telepítéséhez.

1. Rendszergazdai módban PowerShell-ablak megnyitása

1. Telepítse a Windows feladatátvételét funkciót.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Adja hozzá a **CORP\Install** helyi rendszergazdaként.

        net localgroup administrators "CORP\Install" /Add

1. Jelentkezzen ki a ContosoQuorum. Végzett ezzel a kiszolgálóval most.

        logoff.exe

Ezután inicializálni **ContosoSQL1** és **ContosoSQL2**. Kövesse az alábbi lépéseket, amelyek megegyeznek a az SQL Server VMs egyaránt.

1. A két SQL Server VMs csatlakozzon a távoli asztali fájlok indítása. A rendszergazdai jogosultságokkal **AzureAdmin** felhasználónév és jelszó használata **Contoso! 000**, a VMs létrehozásakor megadott.

1. Győződjön meg arról, hogy a számítógép lett sikeresen csatlakoztak az **vallalat.kontraktor.hu**.

1. Megvárja, amíg fut az automatizált inicializálni feladatok a folytatás előtt befejezéséhez az SQL Server telepítéséhez.

1. Rendszergazdai módban PowerShell-ablak megnyitása

1. Telepítse a Windows feladatátvételét funkciót.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering

1. Vegye fel a **CORP\Install** helyi közé

        net localgroup administrators "CORP\Install" /Add

1. Az SQL Server PowerShell szolgáltató importálni.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking

1. Vegye fel a **CORP\Install** az alapértelmezett SQL Server-példányt a Rendszergazdák szerepkörnek.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."

1. Adja hozzá az a három bejelentkezési **NT AUTHORITY\System** fentebb ismertetett engedélyeket.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE TO [NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."

1. Nyissa meg a távoli eléréséhez, az SQL Server tűzfalat.

        netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP

1. Jelentkezzen ki mindkét VMs.

        logoff.exe

Végezetül készen áll az elérhetőség csoport konfigurálása. Végezze el az összes munkáját a **ContosoSQL1**PowerShell szolgáltató az SQL Server fogja használni.

## <a name="configure-the-availability-group"></a>Az elérhetőség csoport konfigurálása

1. Csatlakozzon **ContosoSQL1** újra indítása a távoli asztali fájlokat. Helyett jelentkezik be a számítógép-fiókkal, jelentkezzen be **CORP\Install**.

1. Rendszergazdai módban PowerShell-ablak megnyitása

1. A következő változók meghatározása:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"

1. A PowerShell-szolgáltató SQL Server importálni.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking

1. Az SQL Server-szolgáltatási fiók módosítsa az ContosoSQL1 CORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Az SQL Server-szolgáltatási fiók módosítsa az ContosoSQL2 CORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. A helyi munkakönyvtár [Létrehozása WSFC fürt mindig a elérhetősége csoportok Azure virtuális](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) **CreateAzureFailoverCluster.ps1** letöltése. A parancsfájl funkcionális WSFC fürt létrehozásához használandó. Fontos információt meg, hogy WSFC hogyan kommunikáljon a Azure hálózati című témakörben talál [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md).

1. Váltson a munkakönyvtárat, és hozza létre WSFC a letöltött forgatókönyvvel.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"

1. Az alapértelmezett SQL Server-példányok **ContosoSQL1** és **ContosoSQL2**mindig a elérhetősége csoportok engedélyezése.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)

1. Biztonsági másolat könyvtár létrehozása, és adja meg az SQL Server szolgáltatást fiókok engedélyeket. Az elérhetőség adatbázis, a másodlagos replikakészlettagon előkészítése ezt a címtárat fogja használni.

        $backup = "C:\backup"
        New-Item $backup -ItemType directory
        net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
        icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")

1. Adatbázis létrehozása a **ContosoSQL1** **MyDB1**nevű, teljes biztonsági mentés és a napló biztonsági másolat készítése és **A NORECOVERY** lehetőséggel a **ContosoSQL2** visszaállíthassa őket.

        Invoke-SqlCmd -Query "CREATE database $db"
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
        Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
        Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery

1. Hozzon létre az elérhetőség csoport végpontok az SQL Server VMs és a megfelelő engedélyeket állíthat be a végpontok.

        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server1\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"
        $endpoint =
            New-SqlHadrEndpoint MyMirroringEndpoint `
            -Port 5022 `
            -Path "SQLSERVER:\SQL\$server2\Default"
        Set-SqlHadrEndpoint `
            -InputObject $endpoint `
            -State "Started"

        Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct2]" -ServerInstance $server1
        Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
        Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] TO [$acct1]" -ServerInstance $server2

1. Az elérhetőség kópiák létrehozása.

        $primaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server1 `
            -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate
        $secondaryReplica =
            New-SqlAvailabilityReplica `
            -Name $server2 `
            -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
            -AvailabilityMode "SynchronousCommit" `
            -FailoverMode "Automatic" `
            -Version 11 `
            -AsTemplate

1. Végül az elérhetőség csoport létrehozása, és a másodlagos replika csatlakozzon az elérhetőség csoportban.

        New-SqlAvailabilityGroup `
            -Name $ag `
            -Path "SQLSERVER:\SQL\$server1\Default" `
            -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
            -Database $db
        Join-SqlAvailabilityGroup `
            -Path "SQLSERVER:\SQL\$server2\Default" `
            -Name $ag
        Add-SqlAvailabilityDatabase `
            -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
            -Database $db

## <a name="next-steps"></a>Következő lépések
Akkor most sikeresen végrehajtotta az SQL Server mindig a: hozzon létre egy rendelkezésre állási csoport Azure-ban. Az elérhetőség csoport figyelő konfigurálásához a [konfigurálása egy ILB figyelő mindig a elérhetősége csoportok Azure-ban](virtual-machines-windows-classic-ps-sql-int-listener.md)című témakört.

Más használatáról az SQL Server Azure-ban olvassa el a [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.
