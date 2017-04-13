<properties
    pageTitle="Nem lehet RDP-Azure virtuális gép |} Microsoft Azure"
    description="Ha nem tud kapcsolódni a Windows Azure távoli asztali változatában virtuális gép hibáinak elhárításához"
    keywords="Távoli asztali hiba, a távoli asztali kapcsolat hiba, nem tud csatlakozni a virtuális, távoli asztal – hibaelhárítás"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Az Azure virtuális géphez távoli asztali kapcsolat hibaelhárítása

A távoli asztali Protocol (RDP-) kapcsolat a Windows-alapú Azure virtuális géphez (virtuális) hiba történhet a különféle okok, így nem fér hozzá a virtuális. A probléma a virtuális, a hálózati kapcsolat vagy a távoli asztali ügyfél host a számítógépen a távoli asztali szolgáltatással is lehet. Ez a cikk végigvezeti Önt a leggyakoribb módszerek RDP csatlakozási problémák megoldására egy része. 

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet [az MSDN Azure és fórumok Papírhalom túlcsordulás](https://azure.microsoft.com/support/forums/)Azure szakértői. Másik lehetőségként a be Azure támogatási kérelmeiket is fájl. Nyissa meg az [Azure támogatja a webhely](https://azure.microsoft.com/support/options/) , és jelölje ki az **Első támogatja**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Rövid hibaelhárítási lépések
Minden hibaelhárítási lépés után próbálja meg, a virtuális újracsatlakozás:

1. Állítsa alaphelyzetbe a távoli asztali beállítása.
2. Jelölje be a hálózati biztonsági csoport szabályok / Cloud Services a végpontok.
3. Tekintse át a virtuális konzol naplók.
4. A virtuális erőforrás állapotának ellenőrzése.
5. A virtuális jelszó.
6. Indítsa újra a virtuális.
7. A virtuális újratelepítése.

Ha további részletes lépéseket és magyarázatokat folytatni szeretné az olvasást.

> [AZURE.TIP] Ha a virtuális a **Csatlakozás** gomb szürkén jelenik meg a portál és [Express útvonal](../expressroute/expressroute-introduction.md) vagy a [webhely VPN-](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) kapcsolaton keresztül nem kapcsolódik Azure, kell létrehozása és hozzárendelése a nyilvános IP-címet a virtuális RDP használata előtt. Erről további tudnivalók a [nyilvános IP-címek Azure-ban](../virtual-network/virtual-network-ip-addresses-overview-arm.md).


## <a name="ways-to-troubleshoot-rdp-issues"></a>Módjai RDP kapcsolatos problémák megoldása
Elháríthatja a VMs létrehozott, az erőforrás-kezelő telepítési modellt használja az alábbi módszerek valamelyikével:

- [Azure portál](#using-the-azure-portal) - remek, ha módosítani szeretné, hogy gyorsan alaphelyzetbe RDP konfigurációs vagy a felhasználó hitelesítő adatait, és nincs telepítve az Azure eszközök.
- [Azure PowerShell](#using-azure-powershell) - Ha Ön egy PowerShell-parancssorában gondot gyorsan a RDP konfigurációs vagy a felhasználó hitelesítő adatok alaphelyzetbe állítása az Azure PowerShell-parancsmagok használata.

A [Klasszikus telepítési modell](#troubleshoot-vms-created-using-the-classic-deployment-model)használatával létrehozott VMs hibaelhárítási lépéseket is megtalálhatók.


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Az Azure portál használata – problémamegoldás
Minden hibaelhárítási lépés után próbálja a virtuális újra. Ha még mindig nem tud csatlakozni, próbálkozzon a következő lépéssel.

1. **A RDP kapcsolat alaphelyzetbe állítása**. Ezt a hibaelhárítási lépést a RDP konfiguráció alaphelyzetbe állítása, ha távkapcsolat le vannak tiltva vagy a Windows tűzfal szabályok például RDP, amelyek gátolják.

    Jelölje ki a virtuális az Azure-portálon. Görgessen lefelé a beállítások panel a **támogatási + hibaelhárítás** csoportban a lista aljára közelében. Kattintson a **jelszó alaphelyzetbe állítása** gombra. **Mód** állítsa alaphelyzetbe állítása **csak beállítás** , és kattintson a **frissítés** gombra:

    ![Az Azure-portálon RDP konfigurációjának visszaállítása](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Ellenőrizze a hálózati biztonsági csoport szabályokat**. Hibaelhárítási ebben a lépésben ellenőrzi, hogy egy szabály RDP-forgalmat a hálózati biztonsági csoport. RDP alapértelmezett portja 3389 portot. Egy szabályt, amely a forgalmat a RDP előfordulhat, hogy nem automatikusan létrejön a virtuális létrehozásakor.

    Jelölje ki a virtuális az Azure-portálon. Kattintson a beállítások ablak a **hálózati kapcsolatok** .

    ![Hálózati kapcsolatok megtekintése a portálon Azure virtuális](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Jelölje ki a hálózati kapcsolat a (nincs általában csak az egyiket) listában:

    ![Jelölje ki a hálózati kapcsolat az Azure-portálon](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Jelölje ki a **hálózati biztonsági csoport** a hálózati kapcsolat társított hálózati biztonsági csoport megtekintéséhez:

    ![Hálózati biztonsági csoport válassza az Azure-portálon](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Győződjön meg arról, hogy egy bejövő szabályt létezik, amely lehetővé teszi, hogy a TCP-port 3389 RDP-forgalmat. A következő példa bemutatja egy érvényes szabályt, amely lehetővé teszi a RDP-forgalmat. Megtekintheti `Service` és `Action` megfelelően vannak beállítva:

    ![Ellenőrizze az Azure-portálon RDP NSG szabály](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Ha nem rendelkezik egy szabályt, amely lehetővé teszi RDP-forgalmat [a hálózati biztonsági csoport szabály létrehozása](virtual-machines-windows-nsg-quickstart-portal.md). Engedélyezze a TCP-port 3389.

3. **Véleményezés virtuális indítási diagnosztika**. Ebben a lépésben hibaelhárítási áttekinti a virtuális konzol naplók határozza meg, ha a virtuális hibát jelez. Nem minden VMs van engedélyezve, indítási diagnosztika, így a lehet, hogy ez a hibaelhárítási lépés nem kötelező.
    
    Konkrét hibaelhárítási lépéseket a jelen cikk túlra, de nem elég széles, amely gyengíti RDP kapcsolódási hibát jelző. A konzol naplókról és a virtuális képernyőképe megtekintésével kapcsolatos további tudnivalókért olvassa el a [VMs az indítási diagnosztika](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)című témakört.

4. **A virtuális erőforrás állapotának ellenőrzése**. Hibaelhárítási ebben a lépésben ellenőrzi, hogy nincsenek ismert problémák és a Azure platform, amely hatással lehetnek a virtuális kapcsolatot.

    Jelölje ki a virtuális az Azure-portálon. Görgessen lefelé a beállítások panel a **támogatási + hibaelhárítás** csoportban a lista aljára közelében. Kattintson az **Erőforrás állapot** gombra. Egy megfelelő virtuális jelenti, hogy **akkor érhető el**:

    ![Az Azure-portálon a virtuális erőforrás állapotának ellenőrzése](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. A **felhasználó hitelesítő adatok alaphelyzetbe állítása**. Ezt a hibaelhárítási lépést egy helyi rendszergazdai fiók jelszava alaphelyzetbe állítása, ha bizonytalan vagy elfelejtette a hitelesítő adatokat.

    Jelölje ki a virtuális az Azure-portálon. Görgessen lefelé a beállítások panel a **támogatási + hibaelhárítás** csoportban a lista aljára közelében. Kattintson a **jelszó alaphelyzetbe állítása** gombra. Ellenőrizze, hogy a **mód** **jelszó** van beállítva, és írja be a felhasználónevét és új jelszót. Végül kattintson a **frissítés** gombra:

    ![Az Azure-portálon a felhasználó hitelesítő adatok alaphelyzetbe állítása](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Indítsa újra a virtuális**. Ebben a lépésben hibaelhárítási javíthatja az esetleges mögöttes problémák merülnek fel a virtuális magát.

    Jelölje ki a virtuális az Azure-portálra, majd kattintson az **Áttekintés** lapon. Kattintson az **Újraindítás** gombra:

    ![Indítsa újra a virtuális az Azure-portálon](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **A virtuális telepítsen újra**. Ez a hibaelhárítási lépés a másik állomáshoz belül bármely mögöttes platform vagy hálózati problémák megoldására Azure virtuális redeploys.

    Jelölje ki a virtuális az Azure-portálon. Görgessen lefelé a beállítások panel a **támogatási + hibaelhárítás** csoportban a lista aljára közelében. Kattintson a **telepítsen újra** gombra, és kattintson a **telepítsen újra**:

    ![A virtuális az Azure-portálon újratelepítése](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Ez a művelet befejezése után rövid életű lemez adatok elvész, és dinamikus IP-címek a virtuális társított frissülnek.

Továbbra is tapasztalja RDP problémák, akkor [Nyissa meg a támogatási kérelmet](https://azure.microsoft.com/support/options/) el vagy olvassa el a [részletesebb RDP hibaelhárítási fogalmak és lépéseket](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Azure PowerShell használata – problémamegoldás
Ha még nem tette meg, [Telepítse és állítsa be a legújabb Azure Powershellt](../powershell-install-configure.md).

Az alábbi példák használata változó például `myResourceGroup`, `myVM`, és `myVMAccessExtension`. Cserélje a változó nevét és helyét a saját.

> [AZURE.NOTE] A [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell parancsmaggal visszaállítása a felhasználó hitelesítő adatait, illetve a RDP beállításait. Az alábbi példákban `myVMAccessExtension` a folyamat részeként megadott neve. Ha a VMAccessAgent a korábban dolgozott, a meglévő bővítmény nevét elérheti használatával `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` a virtuális tulajdonságainak ellenőrzéséhez. Ha szeretné megtekinteni a nevét, keresse meg a a kimenet a "Bővítmények" című szakaszát.

Minden hibaelhárítási lépés után próbálja a virtuális újra. Ha még mindig nem tud csatlakozni, próbálkozzon a következő lépéssel.

1. **A RDP kapcsolat alaphelyzetbe állítása**. Ezt a hibaelhárítási lépést a RDP konfiguráció alaphelyzetbe állítása, ha távkapcsolat le vannak tiltva vagy a Windows tűzfal szabályok például RDP, amelyek gátolják.

    A következő példában a egy virtuális nevű RDP kapcsolat alaphelyzetbe állítása `myVM` a a `WestUS` helyét, és az erőforráscsoport nevű `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Ellenőrizze a hálózati biztonsági csoport szabályokat**. Hibaelhárítási ebben a lépésben ellenőrzi, hogy egy szabály RDP-forgalmat a hálózati biztonsági csoport. RDP alapértelmezett portja 3389 portot. Egy szabályt, amely a forgalmat a RDP előfordulhat, hogy nem automatikusan létrejön a virtuális létrehozásakor.

    Első lépésként hozzárendelése a konfigurációs adatok a hálózati biztonsági csoport a `$rules` változó. A következő példa beolvassa a hálózati biztonsági csoport nevű információt `myNetworkSecurityGroup` az erőforráscsoport nevű `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Most tekintse meg az szabályokat, a hálózati biztonsági csoport konfigurált. Ellenőrizze, hogy egy szabály létezik-e engedélyezése a bejövő kapcsolatok portot 3389 az alábbi képlettel történik:

    ```powershell
    $rules.SecurityRules
    ```

    A következő példa bemutatja egy érvényes szabályt, amely lehetővé teszi a RDP-forgalmat. Megtekintheti `Protocol`, `DestinationPortRange`, `Access`, és `Direction` megfelelően vannak beállítva:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Ha nem rendelkezik egy szabályt, amely lehetővé teszi RDP-forgalmat [a hálózati biztonsági csoport szabály létrehozása](virtual-machines-windows-nsg-quickstart-powershell.md). Engedélyezze a TCP-port 3389.

3. A **felhasználó hitelesítő adatok alaphelyzetbe állítása**. Ezt a hibaelhárítási lépést a a helyi rendszergazdafiók, adja meg, ha biztos abban, vagy a hitelesítő adatok elfelejtett jelszó alaphelyzetbe állítása.

    Első lépésként adja meg a felhasználónév és jelszó új hozzárendelése a hitelesítő adatait, és a `$cred` változó az alábbiak szerint:

    ```powershell
    $cred=Get-Credential
    ```

    Most frissítse a hitelesítő adatait a virtuális. A következő példa frissíti egy virtuális nevű a hitelesítő adatait `myVM` a a `WestUS` helyét, és az erőforráscsoport nevű `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Indítsa újra a virtuális**. Ebben a lépésben hibaelhárítási javíthatja az esetleges mögöttes problémák merülnek fel a virtuális magát.

    Az alábbi példa a virtuális nevű újraindul `myVM` az erőforráscsoport nevű `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **A virtuális telepítsen újra**. Ez a hibaelhárítási lépés a másik állomáshoz belül bármely mögöttes platform vagy hálózati problémák megoldására Azure virtuális redeploys.

    Az alábbi példa a virtuális nevű redeploys `myVM` a a `WestUS` helyét, és az erőforráscsoport nevű `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Továbbra is tapasztalja RDP problémák, akkor [Nyissa meg a támogatási kérelmet](https://azure.microsoft.com/support/options/) el vagy olvassa el a [részletesebb RDP hibaelhárítási fogalmak és lépéseket](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>A klasszikus telepítési modell használatával létrehozott VMs – problémamegoldás

Minden hibaelhárítási lépés után próbálja meg, a virtuális való csatlakozáshoz.

1. **A RDP kapcsolat alaphelyzetbe állítása**. Ezt a hibaelhárítási lépést a RDP konfiguráció alaphelyzetbe állítása, ha távkapcsolat le vannak tiltva vagy a Windows tűzfal szabályok például RDP, amelyek gátolják.

    Jelölje ki a virtuális az Azure-portálon. Kattintson a **... További** gombra, majd kattintson az **Alaphelyzet távelérési**:

    ![Az Azure-portálon RDP konfigurációjának visszaállítása](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Ellenőrizze a Cloud Services végpontok**. Hibaelhárítási ebben a lépésben ellenőrzi, hogy végpontjait a Cloud Services lehetővé teszi a RDP-forgalmat a. RDP alapértelmezett portja 3389 portot. Egy szabályt, amely a forgalmat a RDP előfordulhat, hogy nem automatikusan létrejön a virtuális létrehozásakor.

    Jelölje ki a virtuális az Azure-portálon. A **Végpontok** gombra kattintva megtekintheti a végpontok már konfigurálva vannak a virtuális. Győződjön meg arról, hogy a végpontok létezik, amelyek lehetővé teszik a TCP-port 3389 RDP-forgalmat.
    
    A következő példa bemutatja, hogy a forgalmat a RDP érvényes végpontok:

    ![Ellenőrizze az Azure-portálon Cloud Services végpontok](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Ha nem rendelkezik zárólap, amely lehetővé teszi a RDP-forgalmat [Cloud Services végpont létrehozásához](virtual-machines-windows-classic-setup-endpoints.md). A magánjellegű port 3389 TCP engedélyezni.

3. **Véleményezés virtuális indítási diagnosztika**. Ebben a lépésben hibaelhárítási áttekinti a virtuális konzol naplók határozza meg, ha a virtuális hibát jelez. Nem minden VMs van engedélyezve, indítási diagnosztika, így a lehet, hogy ez a hibaelhárítási lépés nem kötelező.
    
    Konkrét hibaelhárítási lépéseket a jelen cikk túlra, de nem elég széles, amely gyengíti RDP kapcsolódási hibát jelző. A konzol naplókról és a virtuális képernyőképe megtekintésével kapcsolatos további tudnivalókért olvassa el a [VMs az indítási diagnosztika](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/)című témakört.

4. **A virtuális erőforrás állapotának ellenőrzése**. Hibaelhárítási ebben a lépésben ellenőrzi, hogy nincsenek ismert problémák és a Azure platform, amely hatással lehetnek a virtuális kapcsolatot.

    Jelölje ki a virtuális az Azure-portálon. Görgessen lefelé a beállítások panel a **támogatási + hibaelhárítás** csoportban a lista aljára közelében. Kattintson az **Erőforrás állapot** gombra. Egy megfelelő virtuális jelenti, hogy **akkor érhető el**:

    ![Az Azure-portálon a virtuális erőforrás állapotának ellenőrzése](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. A **felhasználó hitelesítő adatok alaphelyzetbe állítása**. Ebben a lépésben hibaelhárítási visszaállítja a rendszer biztosan, hogy, vagy a hitelesítő adatok elfelejtette adja meg a helyi rendszergazdafiók jelszavát.

    Jelölje ki a virtuális az Azure-portálon. Görgessen lefelé a beállítások panel a **támogatási + hibaelhárítás** csoportban a lista aljára közelében. Kattintson a **jelszó alaphelyzetbe állítása** gombra. Adja meg a felhasználónevét és új jelszót. Végül kattintson a **Mentés** gombra:

    ![Az Azure-portálon a felhasználó hitelesítő adatok alaphelyzetbe állítása](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Indítsa újra a virtuális**. Ebben a lépésben hibaelhárítási javíthatja az esetleges mögöttes problémák merülnek fel a virtuális magát.

    Jelölje ki a virtuális az Azure-portálra, majd kattintson az **Áttekintés** lapon. Kattintson az **Újraindítás** gombra:

    ![Indítsa újra a virtuális az Azure-portálon](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Továbbra is tapasztalja RDP problémák, akkor [Nyissa meg a támogatási kérelmet](https://azure.microsoft.com/support/options/) el vagy olvassa el a [részletesebb RDP hibaelhárítási fogalmak és lépéseket](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>Adott RDP-hibák elhárítása
Egy adott hibaüzenet jelenik meg a virtuális RDP keresztül csatlakozhat előfordulhatnak. A leggyakoribb hibaüzeneteket a következők:

- [A távoli munkamenetet meg lett szakítva, mert nincsenek távoli asztali licenc kiszolgálók licenc számára érhető el](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Távoli asztal nem találja a számítógép "név"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- Hitelesítés [Hiba történt. A helyi biztonsági szervezet nem lehet Önnel kapcsolatba lépni](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Windows biztonsági hiba: A hitelesítő adatok nem működött](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Ezen a számítógépen a távoli számítógép nem tud csatlakozni](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>További források
Ha még mindig nem tud csatlakozni a virtuális távoli asztali keresztül történt, a hibák egyike sem, olvassa el a részletes [Útmutató a távoli asztali hibaelhárítás](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure IaaS (Windows) diagnosztika csomag](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- A hibaelhárítási lépéseket egy virtuális futó alkalmazások elérése, [az Azure virtuális futó alkalmazás hibaelhárítása access](virtual-machines-linux-troubleshoot-app-connection.md)témakörben talál.
- Ha nehézségei vannak a Linux virtuális Azure-ban való kapcsolódáshoz biztonságos rendszerhéj (SSH) használ, lásd: [kapcsolatos hibák elhárítása SSH kapcsolatokat az Azure-ban egy Linux virtuális](virtual-machines-linux-troubleshoot-ssh-connection.md).
