<properties
    pageTitle="Részletes távoli asztali elhárítása |} Microsoft Azure"
    description="Tekintse át a hibaelhárítási lépések részletes távoli asztali hibákat, ha nem szeretne egy Windows virtuális gépeken futó Azure-ban"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"
    keywords="nem tud csatlakozni távoli asztal hárítsa el a távoli asztali, távoli asztali nem tud csatlakozni, távoli asztali hibák, a távoli asztali hibaelhárítási, a távoli asztali problémák
"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="09/27/2016"
    ms.author="iainfou"/>

# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-to-windows-vms-in-azure"></a>Hibaelhárítási lépések részletes távoli asztali kapcsolat problémák a Windows VMs Azure-ban

Ebben a cikkben részletes diagnosztizálása és javítása a Windows-alapú a Azure virtuális gépeken futó összetett távoli asztali hibák elhárításának lépéseit.

> [AZURE.IMPORTANT] Kattintva kiküszöbölheti a gyakrabban távoli asztali hibákat, győződjön meg róla, hogy a [távoli asztali egyszerű hibaelhárítási témakör](virtual-machines-windows-troubleshoot-rdp-connection.md) a folytatás előtt.

A távoli asztali hibaüzenet [a távoli asztali egyszerű hibaelhárítási útmutatója](virtual-machines-windows-troubleshoot-rdp-connection.md)foglalt az adott hibaüzenetek nem hasonlító előfordulhatnak. Kövesse ezeket a lépéseket, annak megállapításához, hogy miért az asztal (RDP) ügyfél nem tudja elérni a Azure virtuális a RDP szolgáltatást.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet az Azure szakértői [az MSDN Azure és a túlcsordulás Papírhalom fórumokon](https://azure.microsoft.com/support/forums/). Másik lehetőségként az Azure támogatási kérelmeiket is küldhet. Nyissa meg a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , és kattintson a **Támogatás első**. Azure támogatási használatával kapcsolatos információkért olvassa el a [Microsoft Azure támogatási – gyakori kérdések](https://azure.microsoft.com/support/faq/).


## <a name="components-of-a-remote-desktop-connection"></a>Távoli asztali kapcsolaton összetevői

Az alábbi összetevőket egy RDP-kapcsolat játszik szerepet:

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_0.png)

A folytatás előtt érdemes szellemi áttekintéséhez, hogy mi változott az utolsó sikeres távoli asztali kapcsolat a virtuális óta. Példa:

- A nyilvános IP-címét a virtuális vagy a felhőbeli szolgáltatástól, ahol az a virtuális (más néven a virtuális [virtuális](https://en.wikipedia.org/wiki/Virtual_IP_address)IP-cím) megváltozott. A RDP hiba akkor fordulhat elő a DNS-ügyfél gyorsítótára továbbra is tartalmaz, a *régi IP-cím* regisztrált a DNS-nevét. A DNS-ügyfél gyorsítótárának kiürítése, és próbáljon ismét a virtuális. Vagy próbáljon meg közvetlenül a az új virtuális.
- Harmadik fél alkalmazással kezelheti a távoli asztali kapcsolat helyett az Azure portal által létrehozott kapcsolat. Győződjön meg arról, hogy az alkalmazás-konfiguráció tartalmazza-e a megfelelő portot a távoli asztali forgalmat. A virtuális beállításai gombra kattintva ellenőrizheti a klasszikus virtuális gép az [Azure portált](https://portal.azure.com)a port > Végpontok pontra.


## <a name="preliminary-steps"></a>Első lépések

A részletes hibaelhárítási a folytatás előtt

- A virtuális gép az Azure klasszikus portálra vagy az esetleges egyértelmű problémák az Azure portal állapotának ellenőrzése.
- Kövesse a [fix első lépései az egyszerű hibaelhárítási útmutatója RDP gyakori hibáinak](virtual-machines-windows-troubleshoot-rdp-connection.md#quick-troubleshooting-steps).


Próbálja meg a virtuális keresztül távoli asztali újracsatlakozás után ezeket a lépéseket.


## <a name="detailed-troubleshooting-steps"></a>Részletes hibaelhárítási lépések

Előfordulhat, hogy a távoli asztali ügyfélprogram nem tudja elérni a távoli asztali szolgáltatás az alábbi források a problémák miatt a Azure virtuális:

- [Távoli asztali ügyfélszámítógépen](#source-1-remote-desktop-client-computer)
- [Szervezeti intranetes él eszköz](#source-2-organization-intranet-edge-device)
- [A szolgáltatás végpontjának felhő és hozzáférés vezérlőelem-lista (vezérlés)](#source-3-cloud-service-endpoint-and-acl)
- [Hálózati biztonsági csoportok](#source-4-network-security-groups)
- [Windows-alapú Azure virtuális](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a>1 forrása: Távoli asztali ügyfélszámítógép

Győződjön meg arról, hogy a számítógép fel, hogy egy másik a helyszíni, a Windows-alapú számítógépen távoli asztali kapcsolat.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_1.png)

Ha nem tud, győződjön meg a számítógépén a következő beállításokat:

- A beállítás, amely helyi tűzfal blokkolja a távoli asztali forgalmat.
- Helyileg telepített proxy ügyfélszoftver, hogy megakadályozza, hogy a távoli asztali kapcsolat.
- Hálózati ellenőrzése, hogy megakadályozza, hogy a távoli asztali kapcsolat szoftver helyileg telepített.
- Más típusú biztonsági szoftver figyelheti a forgalmat, és engedélyezheti/tilthatja, hogy bizonyos típusú forgalmakról megakadályozza, hogy a távoli asztali kapcsolat.

Minden ezekben az esetekben ideiglenesen tiltsa le a szoftvert, és való csatlakozáskor a helyszíni keresztül távoli asztali számítógépre. Láthatja, hogy a tényleges OK módja, ha a hálózati rendszergazda, a szoftver beállítások engedélyezése a távoli asztali kapcsolat javításához dolgozhat.

## <a name="source-2-organization-intranet-edge-device"></a>Forrás 2: Szervezet intranetes él eszköz

Győződjön meg arról, hogy a számítógép közvetlenül csatlakozik az internethez fel, hogy az Azure virtuális géphez távoli asztali kapcsolat.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_2.png)

Ha nem rendelkezik olyan számítógép, amely közvetlenül csatlakozik az internethez, létrehozása, és tesztelje az erőforrás-csoport vagy felhőalapú szolgáltatást az új Azure virtuális gépet. További tudnivalókért olvassa el a [Létrehozás virtuális gépen futó Windows Azure-ban](virtual-machines-windows-hero-tutorial.md)című témakört. A vizsgálat után törölheti a virtuális gép és az erőforráscsoport vagy a felhőalapú szolgáltatást.

Ha közvetlenül csatlakozik az internetre a számítógép egy távoli asztali kapcsolat hozhat létre, jelölje be a a szervezet intranetes él eszköz:

- Egy belső tűzfal blokkolja a HTTPS-kapcsolatokat az internethez.
- A proxykiszolgáló távoli asztali kapcsolat megakadályozása.
- Behatolási észlelési vagy hálózati megakadályozza, hogy a távoli asztali kapcsolat él hálózata eszközökön futó szoftver figyelése.

A hálózati rendszergazda módosítsa a beállításokat a szervezet intranetes él eszköz engedélyezése a távoli asztali HTTPS-alapú kapcsolatot az internetkapcsolat működik.

## <a name="source-3-cloud-service-endpoint-and-acl"></a>Forrás 3: Felhőalapú szolgáltatás végpontjának és vezérlés

A klasszikus telepítési modell használatával létrehozott VMs ellenőrizze, hogy egy másik, amely a egy felhőalapú szolgáltatásba vagy egy virtuális hálózati Azure virtuális végezhet távoli asztali kapcsolat az Azure virtuális.

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_3.png)

> [AZURE.NOTE] A virtuális gépeken futó az erőforrás-kezelő létrehozni, ugorja át [forrás 4: hálózati biztonsági csoportok](#source-4-network-security-groups).

Ha nem rendelkezik a egy felhőalapú szolgáltatásba vagy egy virtuális hálózati másik virtuális gép, hozzon létre egyet. Kövesse a [Létrehozás virtuális gépen futó Windows Azure-ban](virtual-machines-windows-hero-tutorial.md). A próba virtuális gép törlése, a vizsgálat befejeződése után.

Ha egy felhőalapú szolgáltatásba vagy virtuális hálózati virtuális gép keresztül távoli asztali lehet csatlakozni, győződjön meg ezeket a beállításokat:

- A távoli asztali forgalmat a cél virtuális végpont-konfiguráció: A személyes portot végpontjának meg kell egyeznie a olyan portot, amelyet a virtuális távoli asztali szolgáltatás figyel (az alapértelmezett érték 3389).
- A hozzáférés-Vezérlési a távoli asztali forgalom végpont a virtuális célhelyen: hozzáférés-vezérlési listák meg lehet adni engedélyezni vagy tiltani a bejövő forgalom az internetről forrás IP-címének alapján. Helytelenül vannak beállítva hozzáférés-vezérlési listák megakadályozhatja, hogy a végpontra bejövő távoli asztali forgalmat. Jelölje be a hozzáférés-vezérlési listák annak érdekében, hogy érkező forgalmat a nyilvános IP-címét a proxykiszolgáló vagy más biztonsági kiszolgálójának engedélyezve van. További tudnivalókért lásd: [egy hálózati hozzáférés vezérlő lista (vezérlés) tartalma?](../virtual-network/virtual-networks-acl.md)

Annak ellenőrzéséhez, ha a végpont okozza a problémát, távolítsa el az aktuális végpontot, és hozzon létre egy újat véletlen port választása a külső portszámot 49152 – 65535 tartományát. További információért megtudhatja, [hogy miként állíthat be egy virtuális gép végpontok](virtual-machines-windows-classic-setup-endpoints.md).

## <a name="source-4-network-security-groups"></a>Forrás 4: Hálózati biztonsági csoportok

Hálózati biztonsági csoportok engedélyezése pontosabban szabályozhatja a megengedett bejövő és kimenő forgalmának engedélyezésére. Alhálózat tartó szabályok létrehozása és a felhőszolgáltatásokba az Azure virtuális hálózat. Jelölje be annak érdekében, hogy engedélyezett-e az internetről távoli asztali forgalmat a hálózati biztonsági csoport szabályok:

- Az Azure-portálon válassza ki a virtuális
- Kattintson a **beállítások minden** | **hálózati kapcsolatok** , és válassza a hálózati kapcsolat.
- Kattintson a **beállítások minden** | **hálózati biztonsági csoportot** , és válassza a hálózat biztonsági csoport.
- Kattintson a **beállítások minden** | **a bejövő szabályok** , valamint lehetővé teszi a TCP-port 3389 RDP szabály van.
    - Ha nem rendelkezik egy szabályt, kattintson a **Hozzáadás** szabályt szeretne létrehozni. A Protocol (protokoll), majd a **3389** az a cél porttartományt **TCP** adja meg.
    - Ellenőrizze, hogy a művelet **engedélyezve** van állítva, és kattintson az OK gombra kattintva mentheti a új bejövő szabályt.


További tudnivalókért lásd: [egy hálózati biztonsági csoport (NSG) tartalma?](../virtual-network/virtual-networks-nsg.md)

## <a name="source-5-windows-based-azure-vm"></a>5 forrás: Windows-alapú Azure virtuális

![](./media/virtual-machines-windows-detailed-troubleshoot-rdp/tshootrdp_5.png)

Az [Azure IaaS (Windows) diagnosztika csomag](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) használatával megtekintheti, ha a hiba oka az, hogy az Azure virtuális gép magát. Nem lehet a **kapcsolat RDP-Azure virtuális (indítani)** hiba megoldása a diagnosztika csomag esetén kövesse az [Ebben](virtual-machines-windows-reset-rdp.md)a cikkben utasításokat. Ez a cikk visszaállítja a távoli asztali szolgáltatás a virtuális gépen:

- Engedélyezze a "Távoli asztali" a Windows tűzfal alapértelmezett szabály (TCP-port 3389).
- Engedélyezze a távoli asztali kapcsolat 0 a HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections beállításazonosítót megadásával.

Próbáljon meg újra a kapcsolatot a számítógépről. Ha továbbra sem sikerül kapcsolódni a távoli asztali keresztül, a következő lehetséges problémák ellenőrzése:

- A távoli asztali szolgáltatás nem fut a virtuális célhelyen.
- A távoli asztali szolgáltatás nem figyel 3389 portot.
- A Windows tűzfal vagy más helyi tűzfal tartalmaz egy kimenő szabály megakadályozza, hogy a távoli asztali forgalmat.
- Behatolási észlelési vagy hálózati figyelése a Azure virtuális gépen futó szoftver megakadályozza, hogy a távoli asztali kapcsolat.

A klasszikus telepítési modell használatával létrehozott VMs távoli Azure PowerShell-munkamenetet a Azure virtuális géppel is használhatja. Először akkor telepítenie kell a virtuális gép felhő szolgáltatójánál tanúsítványt. Nyissa meg a [Biztonságos távoli PowerShell hozzáférés beállításainak Azure virtuális gépeken futó](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) , és töltse le a **InstallWinRMCertAzureVM.ps1** parancsfájl a helyi számítógépre.

Ezután telepítse az Azure PowerShell, ha még nem tette meg. [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md)tájékozódhat.

Ezután nyissa meg az Azure PowerShell parancssort, és az aktuális mappa módosítsa a **InstallWinRMCertAzureVM.ps1** script fájl helyét. Futtassa az Azure PowerShell-parancsprogramot, meg kell a megfelelő végrehajtási házirend. Futtassa a **Get-végrehajtási házirend** -alapján határozza meg az aktuális házirend parancsot. A megfelelő szintű beállításáról információért lásd: [Set-végrehajtási házirend](https://technet.microsoft.com/library/hh849812.aspx).

Ezután írja be az Azure előfizetése nevére, a felhőalapú szolgáltatás neve és a virtuális gép neve (eltávolítása a < és > karakterek), majd futtassa az ezek a parancsok.

    $subscr="<Name of your Azure subscription>"
    $serviceName="<Name of the cloud service that contains the target virtual machine>"
    $vmName="<Name of the target virtual machine>"
    .\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName

A **Get-AzureSubscription** parancs a Megjelenítés _SubscriptionName_ tulajdonságból elérheti a megfelelő előfizetés neve. Felhasználói felületén a **Get-AzureVM** parancs a _ServiceName_ oszlopból elérheti a felhőalapú szolgáltatás neve a virtuális gépen.

Jelölje be az új tanúsítvány esetén. Nyissa meg a tanúsítványok beépülő az aktuális felhasználó számára, és keresse meg a **Megbízható legfelső szintű hitelesítésszolgáltató Authorities\Certificates** mappába. Meg kell jelennie a felhőalapú szolgáltatás, a kiállító oszlop a DNS-neve tanúsítvány (Példa: cloudservice4testing.cloudapp.net).

Ezután indítsa távoli Azure PowerShell-munkamenetet a parancsok használatával.

    $uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
    $creds = Get-Credential
    Enter-PSSession -ConnectionUri $uri -Credential $creds

Miután beírta az érvényes rendszergazdai hitelesítő adatait, meg kell jelennie a következő Azure PowerShell-parancssorában hasonló:

    [cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>

Ez a kérdés első része a felhőalapú szolgáltatás neve, amely tartalmazza a cél virtuális, amelyek nem feltétlenül egyeznek meg a "cloudservice4testing.cloudapp.net". Azure PowerShell-parancsok a felhőalapú szolgáltatás, hogy kivizsgáljuk a probléma említett, és javítsa ki a konfigurációs most állíthatnak.

### <a name="to-manually-correct-the-remote-desktop-services-listening-tcp-port"></a>A távoli asztali szolgáltatásokat portot figyeli manuálisan megoldására

Ha nem tudja a **kapcsolat RDP-Azure virtuális (indítani)** problémát az [Azure IaaS (Windows) diagnosztika csomag](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864) futtatásához a távoli Azure PowerShell-munkamenet kérő, a művelet végrehajtása

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

A PortNumber tulajdonság a jelenlegi port száma látható. Ha szükséges, módosítsa a távoli asztali port száma a vissza az alapértelmezett érték (3389) Ez a parancs használatával.

    Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389

Győződjön meg arról, hogy a port módosítását követően 3389 Ez a parancs használatával.

    Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"

Kilépés a távoli Azure PowerShell-munkamenetet a parancs használatával.

    Exit-PSSession

Győződjön meg arról, hogy a távoli asztali végpont az Azure virtuális is használ portot 3398 meg a belső porttal. Indítsa újra a Azure virtuális, és próbálkozzon újra a távoli asztali kapcsolat.


## <a name="additional-resources"></a>További források

[Azure IaaS (Windows) diagnosztika csomag](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)

[A jelszó vagy a távoli asztali szolgáltatás Windows virtuális gépeken futó alaphelyzetbe állítása](virtual-machines-windows-reset-rdp.md)

[Telepítse és állítsa be a Azure PowerShell hogyan](../powershell-install-configure.md)

[Azure virtuális Linux-alapú géphez biztonságos rendszerhéj (SSH) kapcsolatok hibaelhárítása](virtual-machines-linux-troubleshoot-ssh-connection.md)

[Az alkalmazás-Azure virtuális gépen futó access – problémamegoldás](virtual-machines-linux-troubleshoot-app-connection.md)
