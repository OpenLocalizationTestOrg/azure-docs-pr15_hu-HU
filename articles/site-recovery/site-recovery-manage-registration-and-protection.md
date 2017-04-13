<properties
    pageTitle="Távolítsa el a kiszolgálókat, és tiltsa le a védelem |} Microsoft Azure" 
    description="Ez a cikk ismerteti, hogyan unregister kiszolgálók a egy webhely helyreállítási tárolóból elemre, és tiltsa le a védelmet a virtuális gépeken futó és fizikai kiszolgálók." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="remove-servers-and-disable-protection"></a>Távolítsa el a kiszolgálókat, és tiltsa le a védelem

A webhely-helyreállítás Azure szolgáltatás hozzájárul az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) stratégia, replikációs, feladatátvétel és helyreállítási virtuális gépeken futó és fizikai kiszolgálók orchestrating. Gépek replikálhatók Azure, illetve egy másodlagos helyszíni adatközpont. Gyors áttekintéséhez olvassa el a [Mi az Azure webhely helyreállítási?](site-recovery-overview.md)

## <a name="overview"></a>– Áttekintés

Ez a cikk ismerteti, hogyan unregister kiszolgálók a a webhely helyreállítási tárolóból elemre, és a virtuális gépeken futó webhely helyreállítási védett védelmének letiltása. 

Ez a cikk, illetve a [Azure helyreállítási szolgáltatások fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)alsó megjegyzések vagy kérdések tartalmakat tehet közzé.

## <a name="unregister-a-vmm-server"></a>Unregister VMM kiszolgáló

A a tárolóból elemre VMM kiszolgáló képernyőjének **kiszolgálók** lapján az Azure webhely helyreállítási portálon a kiszolgáló törlésével unregister parancsot. Megjegyzés:

-  **Csatlakoztatott VMM kiszolgáló**: javasoljuk, hogy a VMM kiszolgáló unregister, amikor Azure csatlakozik. Ezzel biztosíthatja, hogy a helyszíni VMM, és a VMM kiszolgáló társítva (a felhő, amely a törölni kívánt kiszolgáló felhőket vannak megfeleltetve tartalmazó VMM kiszolgálók) beállítást megfelelően is törlődnek. Azt javasoljuk, hogy csak egy nem kiszolgáló eltávolítását egy állandó csatlakozási probléma esetén.
- **Nem VMM kiszolgáló**: Ha a VMM kiszolgáló nincs kapcsolatban, ha törölte kell parancsfájlokkal manuálisan a törlés végrehajtásához. A [Microsoft-gyűjtemény](http://aka.ms/asr-cleanup-script-vmm)a parancsfájl érhető el. Jegyezze fel a VMM kiszolgáló annak érdekében, hogy a kézi törlési folyamat befejezéséhez.
- **Fürt VMM kiszolgálója**: Ha módosítani szeretné, hogy telepítve van a fürt VMM kiszolgáló unregister tegye a következőket:

    - Ha a kiszolgáló által csatlakozik, törölje az összekapcsolt VMM kiszolgáló képernyőjének **kiszolgálók** lapján. Eltávolítja a szolgáltatót a kiszolgálón, jelentkezzen be a minden csomóponton és a Vezérlőpulton távolítsa el azt. Futtassa a Lemezkarbantartó beállításjegyzékbeli bejegyzés törlése a fürt minden passzív csomóponton az előző szakaszban hivatkozott.
    - Ha a kiszolgáló nincs kapcsolatban kell futtassa a Lemezkarbantartó parancsfájlt csomópontjait fürt.

### <a name="unregister-an-unconnected-vmm-server"></a>Egy nem VMM kiszolgáló unregister

A VMM kiszolgálón el szeretné távolítani:

1. A VMM kiszolgáló az Azure portálról unregister.
2. A VMM kiszolgálón töltse le a Lemezkarbantartó parancsfájl.
3. Nyissa meg a PowerShell a Futtatás rendszergazdaként beállítással a végrehajtása az alapértelmezett (LocalMachine) hatókör házirendjének módosítása.
4. Kövesse a parancsfájl a. 

A kiszolgáló eltávolítása felhőket vannak megfeleltetni felhő rendelkező VMM kiszolgálók:

1. Futtassa a Lemezkarbantartó, és kövesse a 2-4.
2. Adja meg a VMM kiszolgáló, amely nem regisztrált VMM Azonosítóját. 
3. A parancsfájl eltávolítja a regisztrációs adatok a VMM kiszolgáló és a felhő párosítás információkat.


## <a name="unregister-a-hyper-v-server-in-a-hyper-v-site"></a>A Hyper-V webhelyen a Hyper-V kiszolgáló unregister

Ha Azure webhely helyreállítási védelme virtuális gépeken futó-kiszolgálón található a Hyper-V szolgáltatást úgy a Hyper-V webhelyen (kiszolgálóval nincs VMM) telepítik az alábbi képlettel történik a a tárolóból elemre a Hyper-V kiszolgáló is unregister:

1. Tiltsa le a Hyper-V-kiszolgálón található virtuális gépeken futó védelmét.
2. Az Azure webhely helyreállítási portál **kiszolgálók** lapon jelölje be a kiszolgáló > törlése. A kiszolgáló nem kell csatlakoznia Azure ekkor.
3. Futtassa a következő jelenjenek meg a kiszolgáló beállításait, és regisztrációjuk azt a a tárolóból elemre. 

        pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }
    
            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host
        
            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }
        
            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }
        
            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
        
            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."  
                    Remove-Item -Recurse -Path $registrationPath
                }
    
                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }
    
                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }
    
            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {   
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0] 
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd


## <a name="stop-protecting-a-hyper-v-virtual-machine"></a>Állítsa le a Hyper-V virtuális gép védelme

Ha meg szeretné szüntetni a Hyper-V virtuális gép védelme kell távolítsa el a védelmét. Hogyan el a védelmet függően előfordulhat, hogy kell jelenjenek meg a Dokumentumvédelmi beállításokat manuálisan a gépen. 

### <a name="remove-protection"></a>Távolítsa el a védelmet

1. A **virtuális gépeken futó** felhőalapú tulajdonságainak fülre, jelölje ki a virtuális gép > **eltávolítása**.
2. A **virtuális gép eltávolítása megerősítése** lapon van, néhány beállítások:

    - **Tiltsa le a védelem**– Ha engedélyezi, és mentse a ezt a lehetőséget a virtuális gép már nem kell védi webhely helyreállítási. A virtuális gép védelem beállításainak fog lehet automatikusan törlődnek.
    - **Eltávolítás a tárolóból elemre a**– Ha bejelöli ezt a választógombot, a virtuális gép kikerül csak a webhely helyreállítási tárolóból elemre. A helyszíni Dokumentumvédelmi beállításokat a virtuális gép nem érinti. Távolítsa el a Dokumentumvédelmi beállításokat és a virtuális gép eltávolítása az Azure-előfizetést, és távolítsa el a Dokumentumvédelmi beállításokat eltávolítással manuálisan, az alábbi útmutatást követve kell manuálisan beállítások karbantartása kell.

Ha törli a virtuális gép és a merevlemezt, azok a célhely kikerül ki.

### <a name="clean-up-protection-settings-manually-between-vmm-sites"></a>Üríteni a Dokumentumvédelmi beállításokat kézzel (VMM helyek között)

Ha **a virtuális gép kezelésének leállítása**lehetőséget választotta, manuálisan karbantartása beállítások:

1. Az elsődleges kiszolgálón futtassa a parancsfájl érdekében konszolidálhatjuk a elsődleges virtuális gép beállításai VMM konzolról. A VMM konzolban kattintson a PowerShell gombra kattintva nyissa meg a VMM PowerShell konzolt. SQLVM1 cserélje le a virtuális számítógép nevét.

         $vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. A másodlagos VMM kiszolgálón futtassa a jelenjenek meg a másodlagos virtuális gép beállításai:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force

3. A másodlagos VMM kiszolgálón a parancsfájl futtatása után frissítse a virtuális gépeken futó a Hyper-V kiszolgálón, hogy a másodlagos virtuális gép kap újra észlelt a VMM konzolban.
4. A fenti lépéseket a replikációs beállításai csak VMM kiszolgáló törli. Ha el szeretné távolítani a virtuális gép virtuális gép replikáció. Hajtsa végre az alábbi bemutatjuk, mind az elsődleges és másodlagos kell virtuális gépeken futó. Futtassa a parancsprogram replikációs eltávolításához, valamint SQLVM1 cserélje ki a virtuális gép neve alatt.

        Remove-VMReplication –VMName “SQLVM1”


### <a name="clean-up-protection-settings-manually-between-on-premises-vmm-sites-and-azure"></a>Adatvédelmi beállítások manuális (között a helyszíni VMM helyek és Azure) karbantartása

1. A forrás VMM kiszolgálón futtassa a jelenjenek meg az elsődleges virtuális gép beállításai:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection

2. A fenti lépéseket a replikációs beállításai csak VMM kiszolgáló törli. Miután eltávolította a replikáció VMM kiszolgálóról győződjön meg arról való replikáció a kiszolgálón futó a Hyper-V e forgatókönyvvel virtuális gép eltávolítása. SQLVM1 cserélje le a Hyper-V kiszolgáló nevének a virtuális gép és host01.contoso.com nevét.

        $vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

### <a name="clean-up-protection-settings-manually-between-hyper-v-sites-and-azure"></a>Adatvédelmi beállítások manuális (között a Hyper-V helyek és Azure) karbantartása

1. A Hyper-V host forráskiszolgáló, a Ha el szeretné távolítani a virtuális gép replikációs a parancsfájl használatával. SQLVM1 cserélje le a virtuális számítógép nevét.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)

## <a name="stop-protecting-a-vmware-virtual-machine-or-a-physical-server"></a>VMware virtuális gépen vagy a fizikai kiszolgáló védelem megszüntetése

Ha meg szeretné szüntetni egy VMware virtuális számítógépre vagy a fizikai kiszolgáló védelme kell távolítsa el a védelmét. Hogyan el a védelmet függően előfordulhat, hogy kell jelenjenek meg a Dokumentumvédelmi beállításokat manuálisan a gépen. 

### <a name="remove-protection"></a>Távolítsa el a védelmet

1. A **virtuális gépeken futó** felhőalapú tulajdonságainak fülre, jelölje ki a virtuális gép > **eltávolítása**.
2. A **virtuális gép eltávolítása** válassza a lehetőségek közül:

    - **Tiltsa le a védelem (helyreállítási részletező és a hangerő átméretezése a használata)**– csak fogja látni, és engedélyezze ezt a beállítást, ha az alábbiakat végezte el:
        - **A virtuális gép mennyiségi resized**– kritikus állapotba kerül a virtuális gép kötet átméretezésekor. Ha ez akkor fordul elő, akkor válassza ezt a lehetőséget. Letiltja a védelem helyreállítási pontok Azure-ban megtartásával. Amikor újból engedélyezi-e a számítógép védelme az adatokat a átméretezett kötet átkerülnek Azure.
        - **Futtassa a feladatátvevő**– után a környezet futtatásával feladatátvevő helyszíni VMware virtuális gépeken futó vagy a fizikai kiszolgálók az Azure által vizsgált, jelölje be ezt a lehetőséget választva indítsa el a újra védelme a helyszíni virtuális gépeken futó. Ez a beállítás letiltja a virtuális gépeken, és ezután kell őket védelmének engedélyezheti. Megjegyzés:
            - Ezzel a beállítással a virtuális gép letiltása nem befolyásolja a replika virtuális gép Azure-ban.
            - Nem a mobilitás szolgáltatás eltávolítása a virtuális gépen.
    
    - **Tiltsa le a védelem**– Ha engedélyezi, és mentse a ezt a lehetőséget a gép már nem kell védi webhely helyreállítási. A gép védelem beállításainak fog lehet automatikusan törlődnek.
    - **Eltávolítás a tárolóból elemre a**– Ha bejelöli ezt a választógombot, a gép kikerül csak a webhely helyreállítási tárolóból elemre. A helyszíni Dokumentumvédelmi beállításokat a számítógép nem érinti. Ha el szeretné távolítani a számítógép beállításait, és a virtuális gép eltávolítása az Azure előfizetéssel, és kell karbantartása: a beállításokat a mobilitás szolgáltatás eltávolításával.
    
        ![Beállításainak eltávolítása](./media/site-recovery-manage-registration-and-protection/remove-vm.png)

















