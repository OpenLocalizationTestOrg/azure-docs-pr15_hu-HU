<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket Azure webhely helyreállítási és a PowerShell használata a replikáció |} Microsoft Azure"
    description="Megtudhatja, hogy miként automatizálhatja a Hyper-V virtuális gépeken futó a webhely helyreállítási és a PowerShell használatával VMM felhőket a replikáció."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell---classic"></a>Azure Powershell - klasszikus szolgáltatással való replikáció a VMM felhőket a Hyper-V virtuális gépeken futó

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-azure.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasszikus portál](site-recovery-vmm-to-azure-classic.md)
- [A PowerShell - klasszikus](site-recovery-deploy-with-powershell.md)


## <a name="overview"></a>– Áttekintés

Azure webhely helyreállítási hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, orchestrating replikációs, feladatátvétel és helyreállítása virtuális gépeken futó telepítési forgatókönyvek össze. Telepítési forgatókönyvek teljes listáját a az [Azure webhely helyreállítás – áttekintés](site-recovery-overview.md)című témakörben találhat.

Ez a cikk bemutatja, hogyan PowerShell használatával automatizálhatja a gyakori feladatok végrehajtásához beállításakor Azure webhely helyreállítási való replikáció a Hyper-V virtuális gépeken futó System Center VMM felhőket az Azure-tárolóhoz.

A cikk az alkalmazási példát előfeltételei tartalmazza, és megtudhatja, hogy miként állíthat be egy webhely helyreállítási tárolóból elemre, telepítse az Azure webhely helyreállítási szolgáltató VMM forráskiszolgáló, a kiszolgáló regisztrálása a tárolóból elemre, Azure tárterület-fiók felvétele, telepíteni az Azure helyreállítási szolgáltatások ügynököt a Hyper-V kiszolgálók, VMM felhő, amely az összes védett virtuális gépeken futó fog vonatkozni védelem beállításainak konfigurálása , majd engedélyezze az adott virtuális gépeken futó védelmét. Győződjön meg arról, hogy minden működik-e meg a várt módon való áttérés tesztelésével befejezéshez.

Ha problémákat tapasztal, ebben az esetben beállítani, tegye fel a felmerülő kérdésekre az [Azure helyreállítási szolgáltatások fórumát](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


> [AZURE.NOTE] Azure van két különböző telepítési modellekkel létrehozásáról és használatáról az erőforrások: [az erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md). Ez a cikk bemutatja, hogy a klasszikus telepítési modell használata.



## <a name="before-you-start"></a>Előzetes teendők

Győződjön meg arról, hogy az alábbi előfeltételek már a helyükön:

### <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

- Akkor, ha a [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat.
- Azure tároló fiók replikált adatok tárolására lesz szüksége. A fiók geo replikációs engedélyezve van szüksége. Kell tekinthetők meg az ugyanazon régió az Azure webhely helyreállítási tárolóból elemre, és társítjuk az azonos előfizetés. [További tudnivalók a Azure tárhelyet](../storage/storage-introduction.md).
- Győződjön meg arról, hogy a védelemmel ellátni kívánt virtuális gépeken futó megfelelnek az [Azure virtuális gép Előfeltételek](site-recovery-best-practices.md#virtual-machines)kell.

### <a name="vmm-prerequisites"></a>VMM vonatkozó követelmények
- System Center 2012 R2 rendszeren futó VMM kiszolgáló kell.
- Meg kell legalább egy felhőalapú a védelemmel ellátni kívánt VMM kiszolgálón. A felhőben kell tartalmaznia:
    - Egy vagy több VMM host csoportot.
    - Egy vagy több a Hyper-V kiszolgálók vagy fürt minden host csoportban.
    - Egy vagy több virtuális gépeken futó a Hyper-V forrás kiszolgálón.

### <a name="hyper-v-prerequisites"></a>A Hyper-V vonatkozó követelmények

- A Hyper-V kiszolgálók legalább futnia kell **a Windows Server 2012** a Hyper-V szerepkör vagy a **Microsoft a Hyper-V Server 2012** és telepítve van a legújabb frissítéseket.
- Ha futtatja a Hyper-V fürt feljegyzésben, hogy fürt ügynök nem automatikusan létrejön egy statikus IP cím alapú fürt esetén. Kézi konfigurálás fürt közvetítő kell. Ehhez a Kiszolgálókezelő > Feladatátvevőfürt-kezelő csatlakozzon a fürthöz, jelölje ki a **Szerepkör konfigurálása** és jelölje be a **Hyper-V replika ügynök** a **Szerepkör válassza a** képernyőn a magas elérhetősége varázsló.
- Bármely a Hyper-V kiszolgáló vagy, amelynek a védelmét kezelni kívánt csoport VMM felhő szerepelnie kell.

### <a name="network-mapping-prerequisites"></a>Hálózati megfeleltetés vonatkozó követelmények
Ha védelme virtuális gépeken futó Azure hálózati megfeleltetés mértékben a forrás VMM kiszolgálón virtuális hálózatok között, és Azure hálózatok ahhoz, hogy a következő a cél:

- Egymást, mely helyreállítási terv függetlenül azok csatlakozhat az összes gép, amely a hálózaton keresztül nem működnek.
- A hálózati átjáró állítva, a célhelyen Azure hálózati, ha más helyszíni virtuális gépeken futó csatlakoztathatja virtuális gépeken futó.
- Ha nem adja meg hálózati megfeleltetés csak virtuális gépeken futó, amely fölé a helyreállítási ugyanarra a csomagra sikertelen lesz kapcsolhatók Azure való áttérés után.

Ha azt szeretné, a hálózati hozzárendelést üzembe szüksége lesz az alábbiakat:

- A virtuális gépeken futó VMM a forráskiszolgálóval a védelemmel ellátni kívánt kell csatlakoztatni egy virtuális hálózathoz. A hálózat kell kötni logikai a felhőben társított hálózathoz.
- Az Azure hálózat, amelyhez a replikált virtuális gépeken futó feladatátvétel után csatlakozhat. A hálózat feladatátvevő idején választania. A hálózat azonos régió az Azure webhely helyreállítási előfizetés kell lennie.
- [Tudjon meg többet](site-recovery-network-mapping.md) is megtudhat a hálózati hozzárendelést:

###<a name="powershell-prerequisites"></a>A PowerShell vonatkozó követelmények
Győződjön meg arról, hogy készen áll a küldésre Azure PowerShell. Ha már használja a PowerShell, kell frissítési verzióra 0.8.10 vagy újabb verzióját. PowerShell beállításával kapcsolatos további tudnivalókért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Után állítsa be, és a PowerShell beállítva, tekintheti meg a szolgáltatás elérhető parancsmagjai a [alábbi](https://msdn.microsoft.com/library/dn850420.aspx).

Megismerheti, amely segít a parancsmagok hogyan paraméterértékeket, a ráfordítások és a kimeneti értékeket általában történnek az Azure PowerShell, például használata [Azure parancsmagokat első lépések](https://msdn.microsoft.com/library/azure/jj554332.aspx)című.

## <a name="step-1-set-the-subscription"></a>Lépés: 1: Az előfizetés beállítása

Az alábbi parancsmag PowerShell:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Cserélje ki az elemeket, a "< >" belül az adott.

## <a name="step-2-create-a-site-recovery-vault"></a>Lépés: 2: Hozzon létre egy webhely helyreállítási tárolóból elemre.

A PowerShell cserélje ki az elemeket, a "< >" belül a konkrét, és a parancsok:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>3 lépés: A tárolóból elemre regisztrációs kulcs generálása

Regisztráció kulcsot létrehozni a tárolóból elemre. Miután töltse le az Azure webhely helyreállítási szolgáltató, és telepítse a VMM kiszolgáló, a kulcsot kell használni a VMM kiszolgáló regisztrálása a tárolóból elemre.

1.  Beszerzése a beállítási fájl tárolóból elemre, és adja meg a környezet:

    ```

    $VaultName = "<testvault123>"
    $VaultGeo  = "<Southeast Asia>"
    $OutputPathForSettingsFile = "<c:\>"

    $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

    ```

2.  A tárolóból elemre környezet beállítása a következő parancs futtatásával:

    ```

    $VaultSettingFilePath = $vaultSetingsFile.FilePath
    $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

    ```

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Lépés: 4: Telepítse az Azure webhely helyreállítási szolgáltató

1.  A VMM gépen könyvtár létrehozása a következő parancs futtatásával:

    ```

    pushd C:\ASR\

    ```

2.  Bontsa ki a fájlokat a következő parancs futtatásával a letöltött-szolgáltató használata

    ```

    AzureSiteRecoveryProvider.exe /x:. /q

    ```

3.  Telepítse a szolgáltató, használja az alábbi parancsokat:

    ```

    .\SetupDr.exe /i

    ```

    ```

    $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
    do
    {
        $isNotInstalled = $true;
        if(Test-Path $installationRegPath)
        {
            $isNotInstalled = $false;
        }
    }While($isNotInstalled)

    ```

    Várja meg a telepítés befejezéséhez.

4.  A kiszolgáló regisztrálása a tárolóból elemre a következő parancsot:

    ```

    $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
    pushd $BinPath
    $encryptionFilePath = "C:\temp\"
    .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

    ```

## <a name="step-5-create-an-azure-storage-account"></a>Lépés az 5: Azure tároló fiók létrehozása

Ha nincs Azure tárterület-fiókkal, geo replikációs engedélyezve fiók létrehozása a következő parancs futtatásával:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Figyelje meg, hogy a tárterület-fiókot kell ugyanabban a régióban a Azure webhely helyreállítás szolgáltatással, és társítjuk az azonos előfizetés.


## <a name="step-6-install-the-azure-recovery-services-agent"></a>Lépés a 6: Telepítse az Azure helyreállítási szolgáltatások ügynök

Az Azure portálról telepítése az Azure helyreállítási szolgáltatások ügynök kiszolgálón egyes a Hyper-V a védelemmel ellátni kívánt VMM felhőket található.

A következő parancsot az összes VMM állomásokon:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>7 lépés: A felhő konfigurálása adatvédelmi beállítások

1.  Azure felhő védelem profil létrehozása a következő parancs futtatásával:

    ```

    $ReplicationFrequencyInSeconds = "300";
    $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds  $ReplicationFrequencyInSeconds;

    ```

2.  Tároló alkalmazása védelem a következő parancs futtatásával:

    ```

    $PrimaryCloud = "testcloud"
    $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

    ```

3.  A védelem tároló a társítás kezdődik a felhőbe:

    ```

    $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

    ```

4.  A feldolgozás után a következő parancsot:


        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


5.  A feladat feldolgozás után a következő parancsot:


        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



A művelet befejezése ellenőrzéséhez kövesse a [Tevékenység figyelése](#monitor).

## <a name="step-8-configure-network-mapping"></a>Lépés 8: A hálózati hozzárendelést beállítása
Első lépések a hálózati hozzárendelést ellenőrizze, hogy virtuális gépeken futó a forrás VMM kiszolgálón virtuális hálózathoz csatlakozik. Ezeken kívül hozzon létre egy vagy több Azure virtuális hálózatok. Figyelje meg, hogy egyetlen hálózat Azure virtuális hálózatok összevonása csatolható.

Megjegyzendő, hogy a célhely hálózatnak több alhálózat és az egy adott alhálózat nevével megegyező alhálózat, amelyen a forrás virtuális gép található, majd a replika virtuális gép fog csatlakozni cél alhálózat feladatátvétel után. Ha nem áll egyező nevű cél alhálózat, a virtuális gép csatlakozik az első alhálózat a hálózaton.

Az első parancs kiszolgálók lekérése az aktuális Azure webhely helyreállítási tárolóból elemre. A parancs a Microsoft Azure webhely helyreállítási kiszolgálók a $Servers tömb változó tárol.

    $Servers = Get-AzureSiteRecoveryServer


A második parancs a webhely helyreállítási hálózati megkapja a $Servers tömb az első kiszolgáló. A parancs a hálózatok a $Networks változó tárol.


    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

A harmadik parancs a Get-AzureSubscription parancsmaggal a Azure előfizetések kap, és kattintson a $Subscriptions változóban tárolja a ezt az értéket.

    $Subscriptions = Get-AzureSubscription



A negyedik parancs használatával a Get-AzureVNetSite parancsmag, és ezt az értéket a $AzureVmNetworks változó Azure virtuális hálózatok kapja meg.


    $AzureVmNetworks = Get-AzureVNetSite



A végleges parancsmag között az elsődleges és az Azure virtuális gép hálózat megfeleltetés hoz létre. A parancsmaggal megadja az elsődleges hálózat $Networks első eleme. A parancsmaggal adja meg egy virtuális gép hálózati $AzureVmNetworks az első elemként azonosítójú használatával A parancs tartalmazza az Azure előfizetés azonosítójával.


    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Lépés a 9: A virtuális gépeken futó védelem bekapcsolása

Miután kiszolgálók, a felhő és a hálózatok helyesen van beállítva, engedélyezheti védelmének virtuális gépeken futó a felhőben. Vegye figyelembe az alábbiakat:

Virtuális gépeken futó meg kell felelnie [Azure virtuális gép Előfeltételek](site-recovery-best-practices.md#virtual-machines).

Védelem bekapcsolása az operációs rendszer és az operációs rendszer lemez tulajdonságainak megadása kell a virtuális gépen. Virtuális gép sablon használatával VMM virtuális gép létrehozásakor beállíthatja, hogy a tulajdonságot. Is beállíthatja, hogy a meglévő virtuális gépeken futó tulajdonságokból virtuális gép tulajdonságainak **Általános** és **Hardverkonfiguráció** lapokon. Ha VMM nem állítja be a tulajdonságok is beállíthatja azt az Azure webhely helyreállítási portálon.



1.  Ahhoz, hogy a védelmét, futtassa a következő parancsot a védelem tároló első:

        $ProtectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $CloudName



2. A védelem entitás (virtuális) kap a következő parancs futtatásával:


        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



3. A virtuális a DR engedélyezése a következő parancs futtatásával:



        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity  -Protection Enable -Force



## <a name="test-your-deployment"></a>A telepítés tesztelése

A telepítés, futtathatja a próba feladatátvevő egyetlen virtuális géphez, vagy több virtuális gépeken futó álló helyreállítási terv létrehozása és futtatása a terv próba feladatátvevő ellenőrzéséhez. Teszt feladatátvevő keltő szűrő megőrzi a feladatátvétel és helyreállítási mechanizmusa elszigetelt hálózat. Megjegyzés:

- A távoli asztali változatában a feladatátvétel után Azure virtuális gép csatlakozni, engedélyezze a távoli asztali kapcsolat a virtuális gépen a próba feladatátvételi futtatása előtt.
- Feladatátvétel, után kell megadnia a nyilvános IP-címet a távoli asztali változatában Azure virtuális gép csatlakozni. Ha meg szeretne ehhez, győződjön meg róla, nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.

A művelet befejezése ellenőrzéséhez kövesse a [Tevékenység figyelése](#monitor).

### <a name="create-a-recovery-plan"></a>Helyreállítási terv létrehozása

1. Egy .xml fájl létrehozása sablon használatával az alábbi adatok helyreállítás csomagja, és mentse "C:\RPTemplatePath.xml".
2. Azonosító RecoveryPlan csomópontot, név, PrimaryServerId és SecondaryServerId módosítása.
3. Módosítsa a ProtectionEntity PrimaryProtectionEntityId (a VMM vmid) csomópontot.
4. További VMs további ProtectionEntity csomópontok hozzáadásával is hozzáadhat.



        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"  PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"  SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03- cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"  Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"  After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



4. Töltse ki az adatokat a sablonban:


        $TemplatePath = "C:\RPTemplatePath.xml";



5. A RecoveryPlan létrehozása:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;



### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

1.  A RecoveryPlan objektum kaphat a következő parancs futtatásával:

        $RPObject = Get-AzureSiteRecoveryRecoveryPlan -Name $RPName;


2.  Indítsa el a tesztet feladatátvételi a következő parancs futtatásával:


        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Tevékenység figyelése

Az alábbi parancsokkal a figyelése. Látható, hogy a feladat befejezéséhez feldolgozásra elhelyezésével várni.


    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Következő lépések

[Szeretné, többet is](https://msdn.microsoft.com/library/dn850420.aspx) megtudhat a Azure webhely helyreállítási PowerShell-parancsmagok. </a>.
