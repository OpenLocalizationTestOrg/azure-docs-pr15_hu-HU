<properties
    pageTitle="Azure-kiszolgálók védelme az Azure erőforrás-kezelő Azure PowerShell használatával |} Microsoft Azure"
    description="Az Azure-webhely helyreállításhoz Azure-kiszolgálók védelme automatizálása PowerShell- és erőforrás-kezelő Azure használatával."
    services="site-recovery"
    documentationCenter=""
    authors="bsiva"
    manager="abhiag"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="09/27/2016"
    ms.author="bsiva"/>

# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>A helyszíni a Hyper-V virtuális gépeken futó és Azure között PowerShell-és erőforrás-kezelő Azure bizonyos

> [AZURE.SELECTOR]
- [Azure portál](site-recovery-hyper-v-site-to-azure.md)
- [A PowerShell - erőforrás-kezelő](site-recovery-deploy-with-powershell-resource-manager.md)
- [Klasszikus portál](site-recovery-hyper-v-site-to-azure-classic.md)



## <a name="overview"></a>– Áttekintés

Azure webhely helyreállítási hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási stratégia, replikációs, feladatátadási és helyreállítási virtuális gépeken futó telepítési forgatókönyvek össze a orchestrating. Telepítési forgatókönyvek teljes listáját a az [Azure webhely helyreállítás – áttekintés](site-recovery-overview.md)című témakörben találhat.

Azure PowerShell modul, amely tartalmaz kezelése Azure – a Windows PowerShell-parancsmagok. Kétféle modulok dolgozhat: a profil Azure modul vagy az erőforrás-kezelő Azure modul.

Webhely helyreállítási PowerShell-parancsmagok, Azure PowerShell az Azure erőforrás parancsra a rendelkezésre álló segítséget nyújt a védelme és helyreállítása az Azure-kiszolgálókon.

Ez a cikk ismerteti a webhely helyreállítás beállítása és a védelem server Azure téve együtt Azure erőforrás-kezelő, a Windows PowerShell használatával. A jelen cikkben használt példa bemutatja, hogyan védelme, átveszi és virtuális gépeken futó Azure, a Hyper-V állomáson helyreállítása az Azure erőforrás-kezelő Azure PowerShell használatával.

> [AZURE.NOTE] A webhely helyreállítási PowerShell-parancsmagok jelenleg lehetővé teszi a következő beállítások megadása: egy virtuális gép kezelő webhely között, Azure virtuális gép Manager hely és Azure a Hyper-V hely.

Nem kell lennie a PowerShell használatá ezekkel a cikkekkel szakértői, de a alapfogalmak, például modulokat, parancsmagok és a munkamenetek megértéséhez szükséges. További információt a Windows PowerShell olvassa el a [Windows PowerShell használatába](http://technet.microsoft.com/library/hh857337.aspx)című témakört.

Is további információ [Az Azure erőforrás-kezelő Azure a PowerShell használatával](../powershell-azure-resource-manager.md)kapcsolatban.

> [AZURE.NOTE] A Microsoft-partnerek által a felhőben megoldás szolgáltató (CSP) program keretében beállíthatja és kezelheti a védelem ügyfeleik kiszolgálók ügyfeleik megfelelő CSP előfizetések (bérlői előfizetések).

## <a name="before-you-start"></a>Előzetes teendők

Győződjön meg arról, hogy az alábbi előfeltételek már a helyükön:

- A [Microsoft Azure](https://azure.microsoft.com/) -fiókjába. Az [ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/)kiindulhat. Ezeken kívül erről [Azure helyreállítási webhelykezelő árak](https://azure.microsoft.com/pricing/details/site-recovery/)kapcsolatban.
- Azure PowerShell 1.0. Ebben a kiadásban és útmutatást adunk a telepítés kapcsolatos további tudnivalókért lásd [Azure PowerShell 1.0.](https://azure.microsoft.com/)
- A [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) és [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) modulokat. A [PowerShell gyűjtemény](https://www.powershellgallery.com/) elérheti ezeket a modulokat legújabb verziója

Ez a cikk bemutatja, hogyan Powershell-lel Azure Azure erőforrás-kezelővel beállítása és kezelése a kiszolgálón védelme. A jelen cikkben használt példa bemutatja, hogyan fut a Hyper-V állomáson az Azure virtuális gép védelmét. Ebben a példában az alábbi előfeltételek vonatkoznak. A webhely helyreállítási különböző forgatókönyvekben követelményei átfogóbb halmazának tekintse át a az eset kapcsolatos dokumentációt.

- A Windows Server 2012 R2 vagy Microsoft a Hyper-V Server 2012 R2 tartalmazó egy vagy több virtuális gépeken futó operációs rendszert futtató a Hyper-V host.
- A Hyper-V kiszolgálókhoz csatlakozik az internethez, vagy közvetlenül egy proxyn keresztül.
- A virtuális gépeken futó védelemmel ellátni kívánt meg kell felelnie a [virtuális gép Előfeltételek](site-recovery-best-practices.md#virtual-machines).


## <a name="step-1-sign-in-to-your-azure-account"></a>Lépés: 1: Jelentkezzen be az Azure-fiók


1. Nyisson meg egy PowerShell konzolt, és futtassa az ezzel a paranccsal jelentkezzen be az Azure-fiókjába. A parancsmaggal kérni fogja a fiók hitelesítő adatait az weblapon megjeleníti.

        Login-AzureRmAccount

    Másik megoldás, meg is lehetnek a fiók hitelesítő adatait, a paramétert a `Login-AzureRmAccount` parancsmag használatával a `-Credential` paraméter.

    Ha CSP partner nevében bérlő dolgozik, adja meg az ügyfél bérlői webhelyre, azok tenantID vagy bérlői elsődleges tartománynévnek használatával.

        Login-AzureRmAccount -Tenant "fabrikam.com"

2. Fiók van több előfizetések, így az előfizetés a fiókkal használni kívánt társítania kell.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName

3.  Győződjön meg arról, hogy az előfizetés regisztrálva van-e használni az Azure szolgáltatók helyreállítási szolgáltatások és a webhely helyreállítási, az alábbi parancsok használatával:

    - `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
    -  `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

    Ezek a parancsok az eredményt ad ha van beállítva a **RegistrationState** **regisztrált**, is kattint, lépés 2. Ha nem, a hiányzó szolgáltató az előfizetésben regisztrálnia kell.

    Az Azure-szolgáltató regisztrálhatja a webhely helyreállítási és helyreállítási szolgáltatások, futtassa az alábbi parancsokat:

        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
        Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

    Ellenőrzéséhez, hogy a szolgáltatók sikeresen regisztrálva használatával az alábbi parancsokat: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` és `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.



## <a name="step-2-set-up-the-recovery-services-vault"></a>Lépés: 2: Állítsa be a helyreállítási szolgáltatások tárolóból elemre.

1. Hozzon létre egy erőforrás-kezelő Azure erőforráscsoport, amelybe fogja létrehozása a tárolóból elemre, vagy az erőforrás meglévő csoport használata. Új erőforráscsoport hozhat létre, a következő parancs használatával:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    Ha a $ResourceGroupName változó tartalmazza a csoport nevét, az erőforrás szeretne létrehozni, és a $Geo változó tartalmazza az Azure régió található, amelyben létre szeretné hozni az erőforráscsoport (például "Dél Brazília").

    Szerezhet be az erőforrás csoportok listája az előfizetésben használatával a `Get-AzureRmResourceGroup` parancsmag.

2. Hozzon létre egy új Azure helyreállítási szolgáltatások tárolóból elemre a következőképpen:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Meglévő tárolókban listájának használatával meghallgathatja a `Get-AzureRmRecoveryServicesVault` parancsmag.

> [AZURE.NOTE] Ha szeretne a klasszikus portál vagy az Azure szolgáltatás kezelése a PowerShell modul használatával létrehozott webhely helyreállítási tárolókban műveletek hajthatók végre, ilyen tárolókban listájának meghallgathatja használatával a `Get-AzureRmSiteRecoveryVault` parancsmag. Hozzon létre egy új helyreállítási szolgáltatások új műveletekhez tárolóból elemre. A korábban létrehozott webhely helyreállítási tárolókban használata támogatott, de nincs telepítve a legújabb funkciókhoz.

## <a name="step-3-set-the-recovery-services-vault-context"></a>3 lépés: A helyreállítási szolgáltatások tárolóra környezet beállítása

1.  A tárolóból elemre környezet beállítása a következő parancs futtatásával:

        Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-the-site"></a>Lépés: 4: A Hyper-V webhely létrehozása, és a webhely új tárolóra regisztrációs kulcs generálása.

1. Új a Hyper-V webhely létrehozása az alábbi képlettel történik:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Ezzel a parancsmaggal elkezd egy webhely helyreállítási feladatot hozhat létre a webhelyet, és a webhely helyreállítási feladat objektum adja eredményül. Várja meg a feladat befejezéséhez, és ellenőrizze, hogy a feladat sikeresen befejeződött.

    A feladat objektum beolvasásához, és egyúttal ellenőrizze a Get-AzureRmSiteRecoveryJob parancsmaggal a feladatot, aktuális állapotát.
2. Készítése, és töltse le a egy regisztrációs kulcsot a webhelyen, az alábbi képlettel történik:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    A letöltött billentyűt a Hyper-V host másolja. A kulcs regisztrálhatja a Hyper-V host arra a webhelyre van szüksége.

## <a name="step-5-install-the-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>5 lépés: Az Azure webhely helyreállítási szolgáltató és az Azure helyreállítási szolgáltatások Agent telepítése a Hyper-V-szolgáltató

1. Töltse le a [Microsoft](https://aka.ms/downloaddra)a telepítő meg a szolgáltató legújabb verzióját.

2. A telepítő futtatása a Hyper-V állomáson, és a telepítés végén folytassa a regisztráció.

3. Amikor a rendszer kéri, adja meg a letöltött webhely regisztrációs billentyűt, és a teljes regisztrációs a Hyper-V fogadó azt a webhelyet.

4. Győződjön meg arról, hogy a Hyper-V host regisztrált azt a webhelyet, a következő parancs használatával:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-the-protection-container"></a>Lépés a 6: Hozzon létre egy replikációs házirendet, és a védelem tároló társítása

1. Hozzon létre egy replikációs házirendet a következőképpen:

        $ReplicationFrequencyInSeconds = "300";     #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                 #specify the number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Jelölje be annak érdekében, hogy tud-e a replikáció házirend létrehozása a visszaadott feladatot.

    >[AZURE.IMPORTANT] A megadott tárterület-fiók a helyreállítási szolgáltatások tárolóra azonos Azure régióban kell lennie, és geo replikációs engedélyezve kell lennie.
    >
    > - Ha a megadott helyreállítási tároló típusú Azure tároló (klasszikus), a védett gépek feladatátvevő helyreállítása a gép Azure IaaS (klasszikus).
    > - Ha a megadott helyreállítási tároló típusú Azure tároló (Azure erőforrás-kezelő), a védett gépek feladatátvevő helyreállítása a gép Azure IaaS (Azure erőforrás-kezelő).

2. A védelem tároló megfelelő azt a webhelyet, a következőképpen jelenik meg:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3.  Kiindulás a társítás a védelem tároló a replikáció házirendet, az alábbi képlettel történik:

        $Policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $PolicyName
        $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy $Policy -PrimaryProtectionContainer $protectionContainer

    A társítási feladat elvégzéséhez várja meg, és győződjön meg arról, hogy sikeresen befejeződött-e.

##<a name="step-7-enable-protection-for-virtual-machines"></a>7 lépés: A virtuális gépeken futó védelem bekapcsolása

1. A védelem entitás a következőképpen védelemmel ellátni kívánt virtuális megfelelő kap:

        $VMFriendlyName = "Fabrikam-app"                    #Name of the VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

2. Indítsa el a virtuális gép védelem, az alábbi képlettel történik:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

    >[AZURE.IMPORTANT] A megadott tárterület-fiók a helyreállítási szolgáltatások tárolóra azonos Azure régióban kell lennie, és geo replikációs engedélyezve kell lennie.
    >
    > - Ha a megadott helyreállítási tároló típusú Azure tároló (klasszikus), a védett gépek feladatátvevő helyreállítása a gép Azure IaaS (klasszikus).
    > - Ha a megadott helyreállítási tároló típusú Azure tároló (Azure erőforrás-kezelő), a védett gépek feladatátvevő helyreállítása a gép Azure IaaS (Azure erőforrás-kezelő).

    > Ha a virtuális védelmére van csatolva egynél több szabad, adja meg az operációs rendszer lemezen a *OSDiskName* paraméter használatával.

3. Várja meg a virtuális gépeken futó védett állapot elérje a kezdeti replikáció után. Ez eltarthat egy ideig, például kell replikált adatok mennyiségét és a rendelkezésre álló felsőbb szintű sávszélességgel Azure tényezők függően. A feladat állapota és StateDescription frissülnek az alábbiak szerint a virtuális alkalmazáson keresztül egy védett állapot alapján.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed

4. Frissítse a helyreállítási tulajdonságait, például a virtuális szerepkör méretét és az Azure hálózati csatolhat a virtuális gép hálózati kártya való feladatátvevő alapján.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob


        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update the virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Lépés 8: Próba feladatátvevő futtatása

1. Feladat futtatása olyan próba feladatátvevő, az alábbi képlettel történik:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id

2. Győződjön meg arról, hogy a vizsgálat virtuális létrejön Azure-ban. (A próba feladatátvevő feladat fel van függesztve, a vizsgálat virtuális létrehozása az Azure után. A feladat befejezése folytatása a feldolgozás után a létrehozott vakpróbát megtisztítása, ahogy a következő lépés.)

3. A próba feladatátvételt, végezze el az alábbi képlettel történik:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob


##<a name="next-steps"></a>Következő lépések

[További információ:](https://msdn.microsoft.com/library/azure/mt637930.aspx) Azure webhely helyreállítása az Azure erőforrás-kezelő PowerShell-parancsmagok.
