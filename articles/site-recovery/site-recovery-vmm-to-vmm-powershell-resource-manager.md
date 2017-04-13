<properties
    pageTitle="A Hyper-V virtuális gépeken futó VMM felhőket a replikáció egy másodlagos VMM webhelyre PowerShell (erőforrás-kezelő) |} Microsoft Azure"
    description="Megtudhatja, hogy miként Azure webhely helyreállítási való replikáció, feladatátvétel és egy másodlagos VMM webhelyre PowerShell (erőforrás-kezelő) a Hyper-V VMs VMM felhőket a helyreállítási téve terjesztése"
    services="site-recovery"
    documentationCenter=""
    authors="sujaytalasila"
    manager="rochakm"
    editor="raynew"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="sutalasi"/>

# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a>A Hyper-V virtuális gépeken futó VMM felhőket a replikáció másodlagos VMM webhelyre (erőforrás-kezelő) PowerShell használatával

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-vmm-to-vmm.md)
- [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-vmm-to-vmm-powershell-resource-manager.md)

Üdvözli a Azure webhely helyreállítási! Ez a cikk, ha azt szeretné, hogy való replikáció helyszíni a Hyper-V virtuális gépeken futó kezelése a másodlagos webhelyre System Center virtuális gép Manager (VMM) felhőket használatát. 

Ez a cikk bemutatja, hogyan PowerShell használatával automatizálhatja a gyakori feladatok végrehajtásához beállításakor Azure webhely helyreállítási System Center VMM felhőket másodlagos webhelyen való replikáció a System Center VMM felhőket a Hyper-V virtuális gépeken futó.

A cikk az alkalmazási példát előfeltételei tartalmazza, és megmutatja, hogy 

- Hogy miként állíthatja be a helyreállítási szolgáltatások tárolóból elemre.
- A forrás VMM és a cél VMM kiszolgáló az Azure webhely helyreállítási szolgáltató telepítése
- A VMM kiszolgálói rögzítheti a tárolóból elemre
- Replikációs házirend beállítása a VMM felhőben. Az összes védett virtuális gépeken futó fog vonatkozni a házirend az replikációs beállításai 
- Engedélyezze a virtuális gépeken futó védelmét. 
- VMs egyenként vagy termékcsomag részeként, hogy minden megfelelően működik a helyreállítási terv tesztelni.
- Hajtsa végre a tervezett vagy feladatátvételhez VMs egyenként, illetve a helyreállítási terv, hogy minden megfelelően működik részeként.

Ha problémákat tapasztal, ebben az esetben beállítani, tegye fel a felmerülő kérdésekre az [Azure helyreállítási szolgáltatások fórumát](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [AZURE.NOTE] Azure van két különböző [telepítési modellek](../resource-manager-deployment-model.md) az erőforrások létrehozásáról és használatáról: Azure erőforrás-kezelő és klasszikus. Azure szintén két portálokról – az Azure klasszikus portálon, amely támogatja a klasszikus telepítési modell, és az Azure portálon, kijelölt mindkét telepítési az adatmodellek támogatása. Ez a cikk bemutatja az erőforrás-kezelő telepítési modell.



## <a name="on-premises-prerequisites"></a>A helyszíni vonatkozó követelmények

Az alábbiakban amire szüksége lesz az elsődleges és másodlagos helyszíni webhelyek üzembe ebben az esetben:

**Előfeltételek** | **Részletek** 
--- | ---
**VMM** | Azt javasoljuk, hogy egy VMM az elsődleges webhelyen és egy VMM kiszolgáló kiegészítő helyén rendszerbe.<br/><br/> Is [bizonyos egy VMM kiszolgálón felhő között](site-recovery-single-vmm.md). Ehhez a VMM kiszolgálón beállított legalább két felhőket lesz szüksége.<br/><br/> VMM kiszolgálók legalább futnia kell System Center 2012 SP1 a legújabb frissítésekkel együtt.<br/><br/> Minden VMM kiszolgáló egyik van, vagy további felhőket beállítva, és minden felhő rendelkeznie kell a Hyper-V kapacitás profil beállítása. <br/><br/>Felhőket ábrázoló kell tartalmaznia egy vagy több VMM host csoportot.<br/><br/>További tudnivalók [a VMM felhő háló konfigurálása](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), VMM felhőket beállítása és [Útmutató: hoz létre saját felhőket System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM kiszolgálók internet-hozzáféréssel kell rendelkeznie. 
**A Hyper-V** | A Hyper-V kiszolgálók legalább futnia kell a Windows Server 2012 a Hyper-V szerepkörrel és telepítve van a legújabb frissítéseket.<br/><br/> A Hyper-V server tartalmaznia kell egy vagy több VMs.<br/><br/>  Az elsődleges és másodlagos VMM felhőket ábrázoló host csoportjai a Hyper-V kiszolgálók kell lennie.<br/><br/> Ha a Hyper-V fürt t futtat a Windows Server 2012 R2 telepítenie kell [2961977 frissítése](https://support.microsoft.com/kb/2961977)<br/><br/> Ha a Hyper-V fürt t futtat a Windows Server 2012 megjegyzés adott fürt ügynök nem automatikusan létrejön egy statikus IP cím alapú fürt esetén. Kézi konfigurálás fürt közvetítő kell. [További információ:](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).
**Szolgáltató** | Webhely helyreállítási a telepítés során az Azure webhely helyreállítási szolgáltató VMM kiszolgálókon telepítése. A szolgáltató kommunikál webhely helyreállítási HTTPS 443-as való replikáció téve fölé. Adatok replikáció a helyi hálózati vagy a virtuális Magánhálózati kapcsolat elsődleges és másodlagos a Hyper-V kiszolgálók között.<br/><br/> A szolgáltató a VMM kiszolgálón futó az alábbi URL-hozzáférésre van szüksége: *. hypervrecoverymanager.windowsazure.com; *. AccessControl.Windows.NET; *. backup.windowsazure.com; *. BLOB.Core.Windows.NET; *. store.core.windows.net.<br/><br/> Ezenkívül tűzfal kommunikációt a VMM-kiszolgálókról az [Azure adatközpont IP-tartományokat](https://www.microsoft.com/download/confirmation.aspx?id=41653) , és lehetővé teszi a (443-as) HTTPS protokollt.

### <a name="network-mapping-prerequisites"></a>Hálózati megfeleltetés vonatkozó követelmények
Hálózati megfeleltetés térképek kiszolgálókon az elsődleges és másodlagos VMM VMM virtuális hálózatok között:

- Optimálisan helyezze replika VMs a másodlagos a Hyper-V hosts feladatátvétel után.
- Csatlakozás replika VMs megfelelő virtuális hálózatok.
- Ha nem adja meg hálózati replika leképezése VMs nem csatlakozik olyan hálózat feladatátvétel után.
- A hálózat beállítása a webhely helyreállítás során leképezése az alábbi telepítési szükség amire szüksége lesz:

    - Győződjön meg arról, hogy a Hyper-V host forráskiszolgáló VMs VMM virtuális hálózathoz csatlakozik. A hálózat kell kötni logikai a felhőben társított hálózathoz.
    - Ellenőrizze, hogy a másodlagos felhő, amely a helyreállítás kell megadnia rendelkezik-e a megfelelő virtuális hálózatnak. Virtuális hálózat kell kötni logikai hálózat, amely a másodlagos felhőben van társítva.


További tudnivalók a VMM hálózatok konfigurálása az alábbi cikkek

- [Logikai hálózatok konfigurálása a VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
- [Virtuális hálózatokat, és az átjárók VMM konfigurálása](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Tudjon meg többet](site-recovery-network-mapping.md) a hálózati hozzárendelést működéséről.

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

1. Ha nincs telepítve egyik már, hozzon létre egy erőforrás-kezelő Azure erőforráscsoport

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location

2. Hozzon létre egy új helyreállítási szolgáltatások tárolóból elemre, és mentse a létrehozott automatikus rendszer-Helyreállítás tárolóra objektum (használandó később) változó. Az automatikus rendszer-Helyreállítás tárolóra objektum bejegyzés létrehozása a Get-AzureRMRecoveryServicesVault parancsmag használatával meghallgathatja is:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location 

## <a name="step-3-set-the-recovery-services-vault-context"></a>3 lépés: A helyreállítási szolgáltatások tárolóra környezet beállítása

1.  Ha már létrehozott egy tárolóra, futtassa az alábbi parancsot a tárolóból elemre.

        $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname

2.  A tárolóból elemre környezet beállítása futtatásával az alábbi parancsot.

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


## <a name="step-5-create-and-associate-a-replication-policy"></a>Lépés 5: Hozzon létre, és egy replikációs házirend hozzárendelése

1.  A Hyper-V 2012 R2 replikációs házirend létrehozása a következő parancs futtatásával:

    
        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod 

    > [AZURE.NOTE] VMM felhőbeli a Hyper-V hosts (mint az említett a Hyper-V Előfeltételek) a Windows Server másik verzióját futtató is tartalmazhat, de a replikáció házirend adott operációs rendszer verziója. Ha a különféle operációs rendszereken futtató különböző állomások, hozzon létre külön replikációs házirendek operációs rendszer hibatípusonként. A pl.: Ha futó Windows kiszolgálók 2012-ben és a Windows Server 2012 R2 három öt hosts, létre kell hoznia két replikációs házirendeket – egy minden egyes operációs rendszerek esetében.

2.  Az elsődleges védelem tároló (elsődleges VMM felhő) és helyreállítási védelem tároló (helyreállítási VMM felhő) kap a következő parancs futtatásával:
    
        $PrimaryCloud = "testprimarycloud"
        $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

        $RecoveryCloud = "testrecoverycloud"
        $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
  
3.  A házirend könnyen megjegyezhető nevet a házirend 1 lépésben létrehozott beolvasása

        $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname

4.  A társítási a védelem tároló (VMM felhő) kezdődik replikációs szabályokat:

        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer

5.  Várja meg a házirend-társítási feladat elvégzéséhez. Ha a feladat befejezése használja az alábbi PowerShell kódtöredékének ellenőrizni.
   
        $job = Get-AzureRmSiteRecoveryJob -Job $associationJob
        if($job -eq $null -or $job.StateDescription -ne "Completed")
         {
            $isJobLeftForProcessing = $true;
        }

    A feladat feldolgozás után a következő parancsot:

        if($isJobLeftForProcessing)
        {
        Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)


A művelet befejezése ellenőrzéséhez kövesse a [Tevékenység figyelése](#monitor).

## <a name="step-5-configure-network-mapping"></a>Lépés 5: A hálózati hozzárendelést beállítása

1. Az első parancs kiszolgálók lekérése az aktuális Azure webhely helyreállítási tárolóból elemre. A parancs a Microsoft Azure webhely helyreállítási kiszolgálók a $Servers tömb változó tárol.

        $Servers = Get-AzureRmSiteRecoveryServer

2. A parancsok alatti beszerzése a webhely helyreállítási hálózati a forrás VMM és a cél VMM kiszolgáló.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    
    > [AZURE.NOTE] A forráskiszolgálóval VMM lehet első szakasz vagy a kiszolgálók tömb a második lehetőséget. Jelölje be a nevét a VMM kiszolgálók, és a hálózatok megfelelően beszerzése


4. A végleges parancsmag között az elsődleges és a helyreállítási hálózat megfeleltetés hoz létre. A parancsmaggal megadja az elsődleges hálózat $PrimaryNetworks és a helyreállítási hálózathoz $RecoveryNetworks, az első elemet az első elemet.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-6-configure-storage-mapping"></a>Lépés 6: A tárterület-megfeleltetés beállítása

1. A gomb alatti kapja a tárhely besorolása elemet be $storageclassifications változó listáját.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification


2. A parancsai alatt a forrás osztályozás $SourceClassificaion változó- és célwebhelyek besorolás $TargetClassification változó be az első. 

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    
    > [AZURE.NOTE] A forrás- és célwebhelyek sorosztály minden olyan elem, a tömb lehet. Hivatkozik, a kimeneti az alábbi ábra a forrás- és célwebhelyek minősítés $storageclassifications tömbben az index parancs. 
    
    > Get-AzureRmSiteRecoveryStorageClassification |} Jelölje ki-objektum - tulajdonság rövid név, azonosító |} Táblázat formázása


3. Az alábbi parancsmag hoz létre az adatforrás osztályozás és a cél besorolás közötti megfeleltetés. 

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-7-enable-protection-for-virtual-machines"></a>7 lépés: A virtuális gépeken futó védelem bekapcsolása

Után a kiszolgálók, a felhő és a hálózatok helyesen van beállítva, engedélyezheti a felhőben virtuális gépeken futó védelmét. 


  1. Ahhoz, hogy a védelmét, futtassa a következő parancsot a védelem tároló első:
    
            $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
    
  2. A védelem entitás (virtuális) kap a következő parancs futtatásával:
    
            $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
        
  3. A virtuális replikációs engedélyezése a következő parancs futtatásával:

            $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy


## <a name="test-your-deployment"></a>A telepítés tesztelése

A telepítés, futtathatja a próba feladatátvevő egyetlen virtuális géphez, vagy több virtuális gépeken futó álló helyreállítási terv létrehozása és futtatása a terv próba feladatátvevő ellenőrzéséhez. Teszt feladatátvevő keltő szűrő megőrzi a feladatátvétel és helyreállítási mechanizmusa elszigetelt hálózat. 

> [AZURE.NOTE] Létrehozhat egy helyreállítási terv az alkalmazás az Azure-portálon.

A művelet befejezése ellenőrzéséhez kövesse a [Tevékenység figyelése](#monitor).


### <a name="run-a-test-failover"></a>A próba feladatátvétel futtatása

1.  Futtassa az alatt a virtuális hálózat, amelyhez tesztelni szeretné a VMs get-parancsmagok.

        $Servers = Get-AzureRmSiteRecoveryServer
        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

2.  Végezze el a virtuális gép próba feladatátvevő az alábbiak szerint:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1] 

2.  Végezze el a próba feladatátvevő helyreállítási terv az alábbiak szerint:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1] 

### <a name="run-a-planned-failover"></a>Tervezett feladatátvevő futtatása

1. Végezze el a virtuális gép tervezett feladatátvevő az alábbiak szerint:
    
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. Végezze el a helyreállítási terv tervezett feladatátvevő az alábbiak szerint:
    
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Feladatátvételhez futtatása

1. Végezze el a virtuális feladatátvételhez az alábbiak szerint:
        
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 

2. végezze el a helyreállítási terv feladatátvételhez az alábbiak szerint:
        
        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity 
    
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

