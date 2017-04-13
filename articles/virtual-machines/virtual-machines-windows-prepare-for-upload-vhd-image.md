<properties
    pageTitle="Felkészülés az Azure feltöltése a Windows virtuális |} Microsoft Azure"
    description="Ajánlott eljárások a Windows virtuális előkészítése Azure feltöltése előtt"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Azure feltöltése a Windows virtuális előkészítése
Egy Windows virtuális helyszíni Azure való feltöltéséhez megfelelően elő kell készítenie a virtuális merevlemez (virtuális). Nincsenek több javasolt lépéseket, mielőtt egy virtuális feltöltése Azure végrehajtásához. Ebből a cikkből megtudhatja, hogy miként tölthet fel a Microsoft Azure virtuális Windows Merevlemezt, és azt is megtudhatja, [mikor és hogyan Sysprep használni](#step23).

## <a name="prepare-the-virtual-disk"></a>A virtuális lemez előkészítése

>[AZURE.NOTE] 
> Azure csak támogatja [generációs 1 virtuális gépeken futó](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) , amelyek a virtuális formátumban. Újabb VHDX formátumúvá Azure-ban nem támogatott. 
>
> A virtuális kell lennie a rögzített méretű – nem dinamikus. Ha szükséges, az alábbi útmutatást részletesen a VHDX vagy dinamikus lemezre konvertálása. A virtuális számára engedélyezett maximális mérete 1,023 GB.


Győződjön meg arról, hogy a Windows virtuális helyi kiszolgálói megfelelően működik-e. A virtuális magát mielőtt megpróbálja konvertálni, és töltse fel a Azure belül hibák megoldásához.

Ha a virtuális lemez átalakítása szükséges formátumú az Azure, használja az jegyezni, az alábbi szakaszok a lehetőségek közül választhat. Pillanatkép konvertálás vagy a Sysprep futtatása előtt készítsen biztonsági másolatot a virtuális.

### <a name="convert-using-hyper-v-manager"></a>Konvertálás a Hyper-V-kezelővel
- Nyissa meg a Hyper-V kezelője, és válassza a helyi számítógépen, a bal oldalon. A felette menüben kattintson a **művelet** > **Lemez szerkesztése**.
    - A **virtuális merevlemez keresse meg** a képernyőn tallózással keresse meg és jelölje ki a virtuális lemezre.
    - Jelölje be a **Konvertálás** a következő képernyőn
        - Ha VHDX konvertálása van szüksége, jelölje ki a **virtuális** , és kattintson a **Tovább** gombra
        - Ha kell dinamikus lemezről konvertálni, jelölje be a **rögzített méretű** , és kattintson a **Tovább** gombra

    - Keresse meg és jelölje be **az új virtuális fájl elérési útját**.
    - Kattintson a **Befejezés gombra** kattintva zárja be.

### <a name="convert-using-powershell"></a>Konvertálás a PowerShell használatával
A [Konvertálás-virtuális PowerShell-parancsmag](http://technet.microsoft.com/library/hh848454.aspx)használatával virtuális lemez alakíthatja. A következő példa azt egy VHDX konvertálása virtuális, és dinamikus konvertálása rögzített típusát:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK lemezre formátum konvertálása
Az [VMDK fájlformátum](https://en.wikipedia.org/wiki/VMDK)Ha egy Windows virtuális képet, átalakíthatja a virtuális a [Microsoft virtuális gép konverter](https://www.microsoft.com/download/details.aspx?id=42497)használatával. Olvassa el a további információt a blog [egy VMware VMDK a Hyper-V virtuális való alakításával](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) .

## <a name="prepare-windows-configuration-for-upload"></a>Felkészülés a feltöltés a Windows konfigurációja

> [AZURE.NOTE] Futtassa a következő parancsok végrehajtása [rendszergazdai jogosultságokkal](https://technet.microsoft.com/library/cc947813.aspx)rendelkező.

1. Távolítsa el a útválasztási táblázatban bármely statikus állandó útvonalon:

    - Az útvonal táblázat megtekintéséhez futtasson `route print`.
    - Jelölje be az **Adatmegőrzési útvonalak** szakaszok. Ha állandó útvonal, [útvonal törlése](https://technet.microsoft.com/library/cc739598.apx) eltávolításához használja azt.

2. Távolítsa el a WinHTTP proxy:

    ```
    netsh winhttp reset proxy
    ```

3. A lemez SAN házirendet, amely [Onlineall](https://technet.microsoft.com/library/gg252636.aspx)beállításához:

    ```
    diskpart san policy=onlineall
    ```

4. Egyezményes világidő (UTC) szerint idő használata a Windows, és adja meg az indítási típus, a Windows idő (w32time) szolgáltatás **automatikus**:

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Windows-szolgáltatások beállítása:
5. Győződjön meg arról, hogy minden, az alábbi Windows-szolgáltatások **Windows alapértelmezett értékek**értéke. Az indítási beállításokkal jegyezni, az alábbi listában szereplő konfigurálhatók. A parancsok az indítási beállításainak a visszaállítása:

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Állítsa be a távoli asztali beállítása
6. Ha a távoli asztali Protocol (RDP) figyelő területhez tartozik minden önaláírt tanúsítvány, távolítsa el őket:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Tanúsítványok RDP értesülnie beállításával kapcsolatos további tudnivalókért lásd: [Figyelő tanúsítvány konfigurációk a Windows Server rendszeren](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. A [életben tartási](https://technet.microsoft.com/library/cc957549.aspx) értékek RDP szolgáltatás beállítása:

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. A hitelesítési módot a RDP szolgáltatás beállítása:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. RDP szolgáltatás engedélyezése a beállításjegyzék az alábbi alkulcsok hozzáadásával:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>A Windows tűzfal szabályok konfigurálása
10. A három tűzfal profilok (tartomány, személyes és nyilvános) keresztül lehetővé teszik a WinRM és távoli PowerShell-szolgáltatás:

    ```
    Enable-PSRemoting -force
    ```

11. Ellenőrizze, hogy a következő vendég operációs rendszer tűzfalszabályokat helyen:

    - Bejövő

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Bejövő és kimenő

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Kimenő

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>További Windows konfigurálási lépéseket
12. Futtatása `winmgmt /verifyrepository` meggyőződni arról, hogy a Windows Management műszerezettségi WMI-tárházából egységes. Ha a tárházba megsérült, akkor a [blogbejegyzésben](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Ügyeljen arra, hogy az indítási konfigurációs adatok (BCD) beállítások megfelelően a következőket:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Bármilyen további átviteli illesztőprogram felületének szűrőkkel, például a szoftvert, elemzi a TCP-csomagok eltávolítása
15. Győződjön meg arról, hogy a lemez megfelelő és egységes, futtassa a `CHKDSK /f` parancsot.
16. Távolítsa el a külső szoftver és kapcsolódó fizikai összetevők vagy bármely más virtualizációs technológia illesztőprogram.
17. Győződjön meg róla, hogy egy harmadik fél alkalmazás nem használ Port 3389. A port a RDP szolgáltatás Azure-ban használatos. Használhatja a `netstat -anob` jelölje be a portokat, az alkalmazások által használt parancsot.
18. Ha a Windows virtuális, amely a feltölteni kívánt a tartományvezérlőnek, hajtsa végre a [További lépések](https://support.microsoft.com/kb/2904015) a Felkészülés a lemez.
19. Indítsa újra a rendszert a virtuális győződjön meg arról, hogy a Windows megfelelő továbbra is a RDP-kapcsolaton keresztül érhető el.
20. Az aktuális helyi rendszergazdai jelszó alaphelyzetbe állítása, és győződjön meg arról, hogy használni ehhez a fiókhoz jelentkezzen be Windows RDP-kapcsolaton keresztül.  Csoportházirend-objektum "Bejelentkezés engedélyezése a távoli asztali szolgáltatásokat keresztül" a hozzáférési engedélyt szabályozza. Az objektum helyezkedik el a "Számítógép konfigurációja beállításai\Biztonsági ablakban házirend\Felhasználói jogok kiosztása."


## <a name="install-windows-updates"></a>Windows-frissítések telepítése
22. A legújabb frissítések telepítése a Windows. Ha ez nem lehetséges, győződjön meg arról, hogy telepítve vannak-e az alábbi lehetőségeket:

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs egy hálózati üzemszünetek nem helyreállítása és adatok sérült problémák

    - [KB3115224](https://support.microsoft.com/kb/3115224) Megbízhatósága javítása a Windows Server 2012 R2 vagy Windows Server 2012 állomáson futó VMs

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Biztonsági frissítés a Microsoft Windows cím kiterjesztését: 2016 március 8-ban

    - [KB3063075](https://support.microsoft.com/kb/3063075) Sok azonosító 129 eseménynaplózás Microsoft Azure virtuális Windows Server 2012 R2 gép futtatásakor

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure VMs egy hálózati üzemszünetek nem helyreállítása és adatok sérült problémák

    - [KB3114025](https://support.microsoft.com/kb/3114025) Azure elérésekor gyenge teljesítményt fájlok a Windows 8.1 vagy Server 2012 R2 tárhely

    - [KB3033930](https://support.microsoft.com/kb/3033930) Gyorsjavítás növeli a folyamat a Windows Azure szolgáltatás egy RIO puffer 64 ezer vonatkozó

    - [KB3004545](https://support.microsoft.com/kb/3004545) Nem tud hozzáférni a virtuális gépeken futó keresztül a virtuális Magánhálózati kapcsolat, a Windows Azure üzemeltetési szolgáltatásokhoz tárolt

    - [KB3082343](https://support.microsoft.com/kb/3082343) Azure-webhely VPN alagutak használatával a Windows Server 2012 R2 RRAS határokon helyszíni virtuális Magánhálózati kapcsolat elvész

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Biztonsági frissítés a Microsoft Windows cím kiterjesztését: 2016 március 8-ban

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: A biztonsági frissítés CSRSS leírása: 2016 áprilisi 12
    - [KB2904100](https://support.microsoft.com/kb/2904100) Rendszer Windows lemezművelet közben lefagy<a id="step23"></a>
23. Ha szeretne létrehozni a belőle több számítógépen telepíteni a képre, akkor a kép generalize futtatásával `sysprep` a virtuális Azure feltöltése előtt. Nem kell futtatni `sysprep` egy speciális virtuális használatához. Egy általános képet létrehozásával kapcsolatban további információt az alábbi cikkekben talál:

    - [A virtuális kép létrehozása egy meglévő Azure virtuális, az erőforrás-kezelő telepítési modell használata](virtual-machines-windows-create-vm-generalized.md)
    - [A virtuális kép létrehozása egy meglévő Azure virtuális a klasszikus telepítési modem használatával](virtual-machines-windows-classic-capture-image.md)
    - [A kiszolgálói szerepkörök Sysprep támogatása](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Javasolt további beállítások

Az alábbi beállítások nem érintik virtuális feltöltése. Azonban azt ajánljuk, hogy van konfigurálva őket.

- Telepítse az [Azure virtuális gépeken futó ügynök](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Miután telepítette a agent, engedélyezheti a virtuális bővítmények. A virtuális bővítmények végrehajtja a kritikus funkcióit, például jelszavak alaphelyzetbe állítása az VMs RDP konfigurálása a használni kívánt a legtöbb és sok más.

- A kiírása napló akkor lehet hasznos, a Windows összeomlik hibaelhárítás. A napló kiírása webhelycsoport engedélyezése:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Azure-ban a virtuális létrehozása után állítsa be a megadott mérete rendszerlapozófájlt D: meghajtón

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Következő lépések

- [Erőforrás-kezelő telepítésekhez Azure egy Windows virtuális kép feltöltése](virtual-machines-windows-upload-image.md)
