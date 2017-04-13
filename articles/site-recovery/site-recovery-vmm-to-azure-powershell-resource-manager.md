<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket Azure webhely helyreállítási és PowerShell (erőforrás-kezelő) használata a replikáció |} Microsoft Azure"
    description="Bizonyos a Hyper-V virtuális gépeken futó a VMM felhőket Azure webhely helyreállítási és a PowerShell használatával"
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="rajanaki"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-azure-using-powershell-and-azure-resource-manager"></a>Bizonyos VMM felhőket a Hyper-V virtuális gépeken futó az Azure PowerShell és Azure erőforrás-kezelő használatával

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-azure.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-vmm-to-azure-powershell-resource-manager.md)
- [Klasszikus portál](site-recovery-vmm-to-azure-classic.md)
- [A PowerShell - klasszikus](site-recovery-deploy-with-powershell.md)



## <a name="overview"></a>– Áttekintés

Azure webhely helyreállítási hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, orchestrating replikációs, feladatátvétel és helyreállítása virtuális gépeken futó telepítési forgatókönyvek össze. Telepítési forgatókönyvek teljes listáját a az [Azure webhely helyreállítás – áttekintés](site-recovery-overview.md)című témakörben találhat.

Ez a cikk bemutatja, hogyan PowerShell használatával automatizálhatja a gyakori feladatok végrehajtásához beállításakor Azure webhely helyreállítási való replikáció a Hyper-V virtuális gépeken futó System Center VMM felhőket az Azure-tárolóhoz.

A cikk az alkalmazási példát előfeltételei tartalmazza, és megmutatja, hogy

- Hogy miként állíthatja be a helyreállítási szolgáltatások tárolóból elemre.
- Telepítse az Azure webhely helyreállítási szolgáltató VMM forráskiszolgáló
- A tárolóból elemre a kiszolgáló regisztrálása, tárterület Azure-fiók felvétele
- Az Azure helyreállítási szolgáltatások ügynököt a Hyper-V host kiszolgálókon
- VMM felhőket ábrázoló, amely az összes védett virtuális gépeken futó fog vonatkozni védelem beállításainak konfigurálása
- Az adott virtuális gépeken futó védelmet.
- Tesztelje a veszi át győződjön meg arról, hogy minden megfelelően működik.

Ha problémákat tapasztal, ebben az esetben beállítani, tegye fel a felmerülő kérdésekre az [Azure helyreállítási szolgáltatások fórumát](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure van két különböző telepítési modellekkel létrehozásáról és használatáról az erőforrások: [az erőforrás-kezelő és klasszikus](../resource-manager-deployment-model.md). Ez a cikk bemutatja, hogy az erőforrás-kezelő telepítési modellt használja.

## <a name="before-you-start"></a>Előzetes teendők

Győződjön meg arról, hogy az alábbi előfeltételek már a helyükön:

### <a name="azure-prerequisites"></a>Azure vonatkozó követelmények

- Akkor, ha a [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Ha nincs telepítve egyik, indítsa el az [ingyenes fiókot](https://azure.microsoft.com/free). Ezeken kívül erről az [Azure helyreállítási webhelykezelő árak](https://azure.microsoft.com/pricing/details/site-recovery/)kapcsolatban.
- Ha a replikáció CSP előfizetés eset ki szüksége lesz az CSP előfizetés. További tudnivalók a CSP program [tájékoztatásért keressék CSP programban](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
- Azure replikált adatok tárolására Azure v2 (erőforrás-kezelő) tároló fiók lesz szüksége. A fiók geo replikációs engedélyezve van szüksége. Meg kell ugyanabban a régióban a Azure webhely helyreállítás szolgáltatással, és társítjuk az azonos előfizetés vagy a CSP előfizetést. Azure tároló beállításával kapcsolatos további információért olvassa el a hivatkozás a [Microsoft Azure tároló – bevezetés](../storage/storage-introduction.md) című témakört.
- Győződjön meg arról, hogy a védelemmel ellátni kívánt virtuális gépeken futó megfelelnek az [Azure virtuális gép Előfeltételek](site-recovery-best-practices.md#azure-virtual-machine-requirements)kell.

> [AZURE.NOTE] Jelenleg csak a virtuális szintű műveletek lehetségesek Powershellen keresztül. Helyreállítási terv szintű műveletek támogatása a leggyorsabban történik.  Most Ön korlátozott fail-overs csak egy "védett virtuális" Granularitás és helyreállítási terv szinten lévő nem hajt végre.

### <a name="vmm-prerequisites"></a>VMM vonatkozó követelmények
- System Center 2012 R2 rendszeren futó VMM kiszolgáló kell.
- Bármely VMM kiszolgáló virtuális gépeken futó védelemmel ellátni kívánt tartalmazó az Azure webhely helyreállítási szolgáltató rendszernek kell futnia. Ez az Azure webhely helyreállítási a telepítés során telepítve van.
- Meg kell legalább egy felhőalapú a védelemmel ellátni kívánt VMM kiszolgálón. A felhőben kell tartalmaznia:
    - Egy vagy több VMM host csoportot.
    - Egy vagy több a Hyper-V kiszolgálók vagy fürt minden host csoportban.
    - Egy vagy több virtuális gépeken futó a Hyper-V forrás kiszolgálón.
- További információ a VMM felhőket beállítása:
    - További információk a saját VMM felhőket újdonságai [magánjellegű felhőben való System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) és [VMM 2012](http://go.microsoft.com/fwlink/?LinkId=324956)és a felhő.
    - [A VMM felhő háló beállításának](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric) ismertetése
    - Miután a felhőbe háló elemek vannak még a [VMM a magánjellegű felhő](http://go.microsoft.com/fwlink/p/?LinkId=324953) létrehozásával és a személyes felhőket létrehozásával kapcsolatos tudnivalók [Útmutató: személyes felhőket System Center 2012 SP1 VMM létrehozása](http://go.microsoft.com/fwlink/p/?LinkId=324954).


### <a name="hyper-v-prerequisites"></a>A Hyper-V vonatkozó követelmények

- A Hyper-V kiszolgálók legalább futnia kell **a Windows Server 2012** a Hyper-V szerepkör vagy a **Microsoft a Hyper-V Server 2012** és telepítve van a legújabb frissítéseket.
- Ha futtatja a Hyper-V fürt feljegyzésben, hogy fürt ügynök nem automatikusan létrejön egy statikus IP cím alapú fürt esetén. Kézi konfigurálás fürt közvetítő kell. Az
- [Állítsa be a Hyper-V replika ügynök hogyan](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx)témakörben olvashat.
- Bármely a Hyper-V kiszolgáló vagy, amelynek a védelmét kezelni kívánt csoport VMM felhő szerepelnie kell.

### <a name="network-mapping-prerequisites"></a>Hálózati megfeleltetés vonatkozó követelmények
Amikor virtuális gépeken futó az Azure, a virtuális gép hálózatok a forrás VMM kiszolgáló hálózati megfeleltetés térképek védelme és Azure hálózatok ahhoz, hogy a következő a cél:

- Egymást, mely helyreállítási terv függetlenül azok csatlakozhat az összes gép, amely a hálózaton keresztül nem működnek.
- A hálózati átjáró állítva, a célhelyen Azure hálózati, ha más helyszíni virtuális gépeken futó csatlakoztathatja virtuális gépeken futó.
- Hálózati hozzárendelést nem állítja be, ha csak a virtuális gépeken futó, amelyek a helyreállítási ugyanarra a csomagra keresztül nem fogja tudni csatlakozás után Azure veszi át.

Ha azt szeretné, a hálózati hozzárendelést üzembe szüksége lesz az alábbiakat:

- A virtuális gépeken futó VMM a forráskiszolgálóval a védelemmel ellátni kívánt kell csatlakoztatni egy virtuális hálózathoz. A hálózat kell kötni logikai a felhőben társított hálózathoz.
- Egy Azure hálózat, amelyhez replikált virtuális gépeken futó csatlakoztatása után fellépő hibák fölé. A hálózat veszi át idején választania. A hálózat azonos régió az Azure webhely helyreállítási előfizetés kell lennie.

További tudnivalók a hálózati hozzárendelést

- [Logikai hálózatok konfigurálása a VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Virtuális hálózatokat, és az átjárók VMM konfigurálása](http://go.microsoft.com/fwlink/p/?LinkId=386308)
- [Beállítása és Azure virtuális hálózatok figyelése](https://azure.microsoft.com/documentation/services/virtual-network/)


###<a name="powershell-prerequisites"></a>A PowerShell vonatkozó követelmények
Győződjön meg arról, hogy készen áll a küldésre Azure PowerShell. Ha már használja a PowerShell, kell frissítési verzióra 0.8.10 vagy újabb verzióját. PowerShell beállításával kapcsolatos további tudnivalókért lásd: az [Útmutató a telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md). Után állítsa be, és a PowerShell beállítva, tekintheti meg a szolgáltatás elérhető parancsmagjai a [alábbi](https://msdn.microsoft.com/library/dn850420.aspx).

Ha többet szeretne tudni, amely segít a parancsmagok hogyan paraméterértékeket, a ráfordítások és a kimeneti értékeket általában történnek az Azure PowerShell, például használata [Útmutató az első lépések az Azure-parancsmagok](https://msdn.microsoft.com/library/azure/jj554332.aspx)talál.

## <a name="step-1-set-the-subscription"></a>Lépés: 1: Az előfizetés beállítása

1. Az Azure powershell, jelentkezzen be az Azure-fiók: segítségével a következő parancsmagok

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred


2. Ismerkedés a előfizetések listáját. Az egyes az előfizetések ezt is felsorolja a subscriptionIDs. Megjegyzés: az előfizetés, amelyben ki szeretne létrehozni a helyreállítási szolgáltatások tárolóból elemre a subscriptionID lefelé

        Get-AzureRmSubscription

3. Az előfizetés, amelyben a helyreállítási szolgáltatások tárolóra hozza létre az előfizetés azonosítója megemlítése van beállítva

        Set-AzureRmContext –SubscriptionID <subscriptionId>


## <a name="step-2-create-a-recovery-services-vault"></a>Lépés: 2: Hozzon létre egy helyreállítási szolgáltatások tárolóból elemre.

1. Erőforráscsoport létrehozása az Azure erőforrás-kezelő Ha nincs telepítve egyik már

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Hozzon létre egy új helyreállítási szolgáltatások tárolóból elemre, és mentse a létrehozott automatikus rendszer-Helyreállítás tárolóra objektum (használandó később) változó. Az automatikus rendszer-Helyreállítás tárolóra objektum bejegyzés létrehozása a Get-AzureRMRecoveryServicesVault parancsmag használatával meghallgathatja is:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a>3 lépés: A helyreállítási szolgáltatások tárolóra környezet beállítása

1.  A tárolóból elemre környezet beállítása futtatásával az alábbi parancsot.

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a>Lépés: 4: Telepítse az Azure webhely helyreállítási szolgáltató

1.  A VMM gépen könyvtár létrehozása a következő parancs futtatásával:

        New-Item c:\ASR -type directory

2.  Bontsa ki a fájlokat a következő parancs futtatásával a letöltött-szolgáltató használata

        pushd C:\ASR\
        .\AzureSiteRecoveryProvider.exe /x:. /q


3.  Telepítse a szolgáltató, használja az alábbi parancsokat:

        .\SetupDr.exe /i
        $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
        do
        {
                        $isNotInstalled = $true;
                        if(Test-Path $installationRegPath)
                        {
                                        $isNotInstalled = $false;
                        }
        }While($isNotInstalled)

    Várja meg a telepítés befejezéséhez.

4.  A kiszolgáló regisztrálása a tárolóból elemre a következő parancsot:

        $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
        pushd $BinPath
        $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice


## <a name="step-5-create-an-azure-storage-account"></a>Lépés az 5: Azure tároló fiók létrehozása

1. Ha nincs Azure tárterület-fiókkal, geo replikációs engedélyezve fiók létrehozása az az azonos geo, mint a tárolóból elemre a következő parancs futtatásával:

        $StorageAccountName = "teststorageacc1" #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"  
        $ResourceGroupName =  “myRG”            #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Figyelje meg, hogy a tárterület-fiókot kell ugyanabban a régióban a Azure webhely helyreállítás szolgáltatással, és társítjuk az azonos előfizetés.

## <a name="step-6-install-the-azure-recovery-services-agent"></a>Lépés a 6: Telepítse az Azure helyreállítási szolgáltatások ügynök

1. Az Azure helyreállítási szolgáltatások ügynök [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) a letöltheti és telepítheti azt kiszolgálón egyes a Hyper-V a védelemmel ellátni kívánt VMM felhőket található.

2. A következő parancsot az összes VMM állomásokon:

    marsagentinstaller.exe/q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>7 lépés: A felhő konfigurálása adatvédelmi beállítások

1.  Azure való replikáció házirend létrehozása a következő parancs futtatásával:


        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

2.  Tároló alkalmazása védelem a következő parancs futtatásával:

        $PrimaryCloud = "testcloud"
        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

3.  A házirend részleteket a feladat létrehozott használ, és a rövid házirend neve megemlítése egy változó:

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  Indítsa el a védelmet tároló a társítás replikációs házirend:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  

5.  A feldolgozás után a következő parancsot:

        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
        $isJobLeftForProcessing = $true;
        }

6.  A feladat feldolgozás után a következő parancsot:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)

A művelet befejezése ellenőrzéséhez kövesse a [Tevékenység figyelése](#monitor).

## <a name="step-8-configure-network-mapping"></a>Lépés 8: A hálózati hozzárendelést beállítása

Első lépések a hálózati hozzárendelést ellenőrizze, hogy virtuális gépeken futó a forrás VMM kiszolgálón virtuális hálózathoz csatlakozik. Ezeken kívül hozzon létre egy vagy több Azure virtuális hálózatok.

További információ arról, hogy miként hozhat létre virtuális hálózat Azure erőforrás-kezelő és, a PowerShell használata [Hozzon létre egy virtuális hálózati hely közötti virtuális Magánhálózati kapcsolatot Azure erőforrás-kezelő és a PowerShell használatával](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Figyelje meg, hogy egyetlen hálózat Azure virtuális gép hálózatok összevonása csatolható. A cél hálózatnak több alhálózat és az egy adott alhálózat alhálózat, amelyen a forrás virtuális gép található nevét, majd a replika virtuális gép fog csatlakoznia cél alhálózat után fellépő hibák fölé. Ha nincs cél alhálózat egyező nevű, a virtuális gép csatlakozik az első alhálózat a hálózaton.

1. Az első parancs kiszolgálók lekérése az aktuális Azure webhely helyreállítási tárolóból elemre. A parancs a Microsoft Azure webhely helyreállítási kiszolgálók a $Servers tömb változó tárol.

        $Servers = Get-AzureRmSiteRecoveryServer

2. A második parancs a webhely helyreállítási hálózati megkapja a $Servers tömb az első kiszolgáló. A parancs a hálózatok a $Networks változó tárol.


        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

3. A harmadik parancs megkapja a $AzureVmNetworks változó Azure virtuális hálózatok, és ezt az értéket.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork

4. A végleges parancsmag között az elsődleges és az Azure virtuális gép hálózat megfeleltetés hoz létre. A parancsmaggal megadja az elsődleges hálózat $Networks első eleme. A parancsmaggal egy virtuális gép hálózatot $AzureVmNetworks az első elemként ad meg.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]


## <a name="step-9-enable-protection-for-virtual-machines"></a>Lépés a 9: A virtuális gépeken futó védelem bekapcsolása

Után a kiszolgálók, a felhő és a hálózatok helyesen van beállítva, engedélyezheti a felhőben virtuális gépeken futó védelmét.

 Vegye figyelembe az alábbiakat:

 - Virtuális gépeken futó meg kell felelnie Azure követelményeknek. Ellenőrizze a következőket [Előfeltételek](site-recovery-best-practices.md) és támogatás a tervezési útmutatóban olvashat.

 - Ahhoz, hogy védelem operációs rendszer és az operációs rendszer lemez tulajdonságai kell beállítani a virtuális gépen. Virtuális gép sablon használatával VMM virtuális gép létrehozásakor beállíthatja, hogy a tulajdonságot. Is beállíthatja, hogy a meglévő virtuális gépeken futó tulajdonságokból virtuális gép tulajdonságainak **Általános** és **Hardverkonfiguráció** lapokon. Ha VMM nem állítja be a tulajdonságok is beállíthatja azt az Azure webhely helyreállítási portálon.


  1. Ahhoz, hogy a védelmét, futtassa a következő parancsot a védelem tároló első:

            $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName

  2. A védelem entitás (virtuális) kap a következő parancs futtatásával:

            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer

  3. A virtuális a DR engedélyezése a következő parancs futtatásával:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1



## <a name="test-your-deployment"></a>A telepítés tesztelése

A telepítés, futtathatja a teszt sikertelen fölé egyetlen virtuális géphez, vagy több virtuális gépeken futó álló helyreállítási terv létrehozása és futtatása a teszt sikertelen fölé a terv ellenőrzéséhez. Teszt sikertelen fölé keltő szűrő megőrzi a veszi át és helyreállítási mechanizmusa elszigetelt hálózat. Megjegyzés:

- A távoli asztal használata után a fail fölé Azure virtuális gép csatlakozni, engedélyezze a távoli asztali kapcsolat a virtuális gépen a próba feladatátvételi futtatása előtt.
- Után fellépő hibák fölé kell megadnia a nyilvános IP-címet a távoli asztali változatában Azure virtuális gép csatlakozni. Ha meg szeretne ehhez, győződjön meg róla, nincs talált tartomány házirendeket, hogy megakadályozni a csatlakozást nyilvános címmel virtuális géphez.

A művelet befejezése ellenőrzéséhez kövesse a [Tevékenység figyelése](#monitor).


### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

1.  Indítsa el a tesztet feladatátvételi a következő parancs futtatásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Tervezett feladatátvevő futtatása

1. Indítsa el a tervezett feladatátvételt a következő parancs futtatásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Feladatátvételhez futtatása

1. Indítsa el a tervezett feladatátvételt a következő parancs futtatásával:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  


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

[További információ:](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure webhely helyreállítása az Azure erőforrás-kezelő PowerShell-parancsmagok.
