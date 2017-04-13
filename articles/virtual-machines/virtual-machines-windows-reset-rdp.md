<properties
    pageTitle="Állítsa alaphelyzetbe a jelszó vagy a Windows virtuális távoli asztali konfigurációjának |} Microsoft Azure"
    description="Megtudhatja, hogy miként alaphelyzetbe karaktertípusnak vagy egy Windows virtuális az Azure portálja vagy az Azure PowerShell használata a távoli asztali szolgáltatásokat."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/01/2016"
    ms.author="iainfou"/>

# <a name="how-to-reset-the-remote-desktop-service-or-its-login-password-in-a-windows-vm"></a>A távoli asztali szolgáltatás vagy egy Windows virtuális a saját bejelentkezési jelszó alaphelyzetbe állítása

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

A Windows virtuális gép (virtuális) nem tud csatlakozni, ha a helyi rendszergazdai jelszó alaphelyzetbe állítása, vagy állítsa alaphelyzetbe a távoli asztali szolgáltatás beállításai. Használhatja az Azure portálon vagy a virtuális bővítményében a Azure PowerShellben a jelszó alaphelyzetbe állítása. Ha a PowerShell használata esetén ellenőrizze a legújabb PowerShell-modult a munkahelyi számítógépen telepítve van, és Azure-előfizetése van bejelentkezve. A lépések részletes leírását olvassa el a [hogyan telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).

> [AZURE.TIP] Ellenőrizheti, hogy a PowerShell használatával telepített verziója`Import-Module Azure, AzureRM; Get-Module Azure, AzureRM | Format-Table Name, Version`

## <a name="windows-vms-in-resource-manager-deployment-model"></a>Az erőforrás-kezelő telepítési modell Windows VMs

### <a name="azure-portal"></a>Azure portálon
Jelölje ki a virtuális a **Tallózás**gombra kattintva > **virtuális gépeken futó** > *a Windows virtuális gép* > **minden elérhető beállítás** > **jelszó alaphelyzetbe állítása**. A jelszó alaphelyzetbe állítása lap jelenik meg:

![Jelszó alaphelyzetbe állítása lap](./media/virtual-machines-windows-reset-rdp/Portal-RM-PW-Reset-Windows.png)

Írja be a felhasználónevét és a új jelszót, majd kattintson a **Mentés**gombra. Próbáljon újra a virtuális gép.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess bővítmény és a PowerShell

Ellenőrizze, hogy Azure PowerShell 1.0 van vagy újabb verziója van telepítve, és be van jelentkezve a fiók használatával a `Login-AzureRmAccount` parancsmag.

#### <a name="reset-the-local-administrator-account-password"></a>**A helyi rendszergazdai fiók jelszavának alaphelyzetbe állítása**

A [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShell parancs használatával alaphelyzetbe állíthatja a rendszergazdai jelszavát vagy a felhasználó nevét.

Hozhat létre a helyi rendszergazdai fiók hitelesítő adatait a következő parancsot:

    $cred=Get-Credential

Ha beír egy másik nevet, mint az aktuális számla, a következő VMAccess bővítmény parancsot átnevezi a helyi rendszergazdafiók jelszavát rendel a fiókhoz társított és a távoli asztali kijelentkezés esemény problémák. Ha a helyi rendszergazdafiók le van tiltva, a VMAccess bővítmény engedélyezi azt.

A virtuális access bővítmény segítségével az új hitelesítő adatainak beállítása az alábbi képlettel történik:

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" -Name "myVMAccess" `
        -Location WestUS -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"


Csere `myRG`, `myVM`, `myVMAccess`, és a beállítások vonatkozó értékeket tartalmazó helyét.


#### <a name="reset-the-remote-desktop-service-configuration"></a>**Állítsa alaphelyzetbe a távoli asztali szolgáltatás beállításai**

Távoli hozzáférés [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) vagy a [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx), az alábbi képlettel történik használatával visszaállíthatja a virtuális. (Cserélje le a `myRG`, `myVM`, `myVMAccess` és helyét a saját értékekkel.)

    Set-AzureRmVMExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -ExtensionType "VMAccessAgent" -Location WestUS `
        -Publisher "Microsoft.Compute" -typeHandlerVersion "2.0"

Vagy:<br>

    Set-AzureRmVMAccessExtension -ResourceGroupName "myRG" -VMName "myVM" `
        -Name "myVMAccess" -Location WestUS -typeHandlerVersion "2.0


> [AZURE.TIP] Mindkét parancsok hozzáadása a virtuális gép új névvel ellátott virtuális access ügynök. Bármely pontján egy virtuális csak egy virtuális access képviselő van. A virtuális access ügynök tulajdonságainak sikerült beállítani, távolítsa el az access ügynök korábban vagy használatával állíthatják be `Remove-AzureRmVMAccessExtension` vagy `Remove-AzureRmVMExtension`. Azure PowerShell verzió 1.2.2 kezdve, kerülheti el ezt a lépést használatakor `Set-AzureRmVMExtension` az egy `-ForceRerun` lehetőséget. Használatakor `-ForceRerun`, ügyeljen arra, hogy ugyanazt a nevet a virtuális access ügynökök beállítása az előző parancs használni.

Ha továbbra is nem tud csatlakozni távolról a virtuális géphez, próbálja meg a [távoli asztali kapcsolatos hibák elhárítása kapcsolatok Azure virtuális Windows-alapú géphez](virtual-machines-windows-troubleshoot-rdp-connection.md)további lépések.


## <a name="windows-vms-in-the-classic-deployment-model"></a>A klasszikus telepítési modell Windows VMs

### <a name="azure-portal"></a>Azure portál

A klasszikus telepítési modell alapján létre virtuális gépekhez az [Azure portált](https://portal.azure.com) a távoli asztali szolgáltatás hozzon létre egy új is használhatja. : Kattintson a **Tallózás** > **virtuális gépeken futó (klasszikus)** > *a Windows virtuális gép* > **Alaphelyzetbe távoli...**. A megjelenő lap jelenik meg.

![RDP konfigurációs oldal visszaállítása](./media/virtual-machines-windows-reset-rdp/Portal-RDP-Reset-Windows.png)

A név és a helyi rendszergazdafiók jelszó alaphelyzetbe állítása is próbálkozhat. : Kattintson a **Tallózás** > **virtuális gépeken futó (klasszikus)** > *a Windows virtuális gép* > **minden elérhető beállítás** > **jelszó alaphelyzetbe állítása**. A megjelenő lap jelenik meg.

![Jelszó alaphelyzetbe állítása lap](./media/virtual-machines-windows-reset-rdp/Portal-PW-Reset-Windows.png)

Miután megadta az új felhasználó nevét és jelszavát, kattintson a **Mentés**gombra.

### <a name="vmaccess-extension-and-powershell"></a>VMAccess bővítmény és a PowerShell

Győződjön meg arról, hogy a virtuális ügynök telepítve van a virtuális gépen. A VMAccess kiterjesztést nem kell telepíteni kell, mielőtt használni tudja, mindaddig, amíg a virtuális Agent érhető el. Győződjön meg arról, hogy a virtuális Agent már telepítve van a következő parancs használatával. (Felülírásához "myCloudService" és "myVM" a felhőalapú szolgáltatásba, és a virtuális, azoknak a következő lapjához. A nevek talál futtatásával `Get-AzureVM` paraméter nélkül.)

    $vm = Get-AzureVM -ServiceName "myCloudService" -Name "myVM"
    write-host $vm.VM.ProvisionGuestAgent

Az **írási-host** parancs megjelenik a **Igaz**, ha a virtuális ügynök telepítve van. Ha **FALSE értéket**jeleníti meg, olvassa el a található utasításokat, és a letöltés a [virtuális ügynök és bővítmények - kijelző 2](http://go.microsoft.com/fwlink/p/?linkid=403947&clcid=0x409) Azure blogbejegyzés mutató hivatkozást.

A virtuális gép a portál használatával létrehozott, ellenőrizze, hogy `$vm.GetInstance().ProvisionGuestAgent` **Igaz**értéket ad vissza. Ha nem, akkor beállíthatja, ez a parancs használatával:

    $vm.GetInstance().ProvisionGuestAgent = $true

Ez a parancs megakadályozza, hogy a következő hiba esetén a következő lépésben a **Set-AzureVMExtension** parancsot futtatja: "Rendelkezés Vendég ügynök engedélyeznie kell a virtuális objektum IaaS virtuális Access bővítmény telepítése előtt."

#### <a name="reset-the-local-administrator-account-password"></a>**A helyi rendszergazdai fiók jelszavának alaphelyzetbe állítása**

A bejelentkezési hitelesítő adatok létrehozása az aktuális helyi rendszergazdai fiók nevét, és új jelszót, és futtassa a `Set-AzureVMAccessExtension` az alábbi képlettel történik.

    $cred=Get-Credential
    Set-AzureVMAccessExtension –vm $vm -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password  | Update-AzureVM

Ha beír egy másik nevet, mint az aktuális számla, a VMAccess bővítmény átnevezi a helyi rendszergazdafiók jelszavát rendel a fiókhoz társított és kijelentkezési távoli asztali problémák. Ha a helyi rendszergazdafiók le van tiltva, a VMAccess bővítmény engedélyezi azt.

Ezek a parancsok is visszaállítása a távoli asztali szolgáltatás beállításai.

#### <a name="reset-the-remote-desktop-service-configuration"></a>**Állítsa alaphelyzetbe a távoli asztali szolgáltatás beállításai**

Hozzon létre egy új a távoli asztali szolgáltatás beállításai, a következő parancsot:

    Set-AzureVMAccessExtension –vm $vm | Update-AzureVM

A VMAccess bővítmény két parancsot futtatja, a virtuális gépen:

- `netsh advfirewall firewall set rule group="Remote Desktop" new enable=Yes`

Ez a parancs lehetővé teszi, hogy a beépített a Windows tűzfal csoport, amely lehetővé teszi, hogy a bejövő távoli asztali a forgalmat, aminek 3389 portot használja.

- `Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0`

Ez a parancs állítja be a fDenyTSConnections beállításazonosítót engedélyezése a távoli asztali kapcsolat, a 0.


## <a name="next-steps"></a>Következő lépések

Ha nem tudja a jelszó alaphelyzetbe állítása az Azure virtuális access bővítmény nem válaszol, azt is megteheti, [offline a helyi Windows jelszó alaphelyzetbe állítása](virtual-machines-windows-reset-local-password-without-agent.md). Ez a módszer összetettebb folyamat, és van szükség az, hogy a merevlemez a hibás virtuális csatlakozzon másik virtuális. Hajtsa végre az első ebben a cikkben leírt lépéseket, és csak megpróbálja a kapcsolat nélküli jelszó alaphelyzetbe állítása módszer utolsó lehetőségként.

[Azure virtuális extensions és szolgáltatások](virtual-machines-windows-extensions-features.md)

[Csatlakozás egy Azure virtuális géphez RDP vagy SSH](http://msdn.microsoft.com/library/azure/dn535788.aspx)

[A Windows-alapú Azure virtuális géphez távoli asztali kapcsolat hibaelhárítása](virtual-machines-windows-troubleshoot-rdp-connection.md)
