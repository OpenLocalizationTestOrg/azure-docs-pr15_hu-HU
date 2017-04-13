<properties
    pageTitle="Trend-mély Micro Security telepítése egy virtuális |} Microsoft Azure"
    description="Ez a cikk ismerteti, hogyan telepítheti, állíthatja a Trend Micro biztonsági a egy virtuális a klasszikus telepítési modell Azure-ban készült."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="iainfou"/>


# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Telepítse és állítsa be a Trend Micro-mély biztonsági kattintson egy Windows virtuális szolgáltatásként hogyan

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Ez a cikk bemutatja, hogyan telepítheti, állíthatja a Trend Micro-mély biztonsági egy új vagy meglévő virtuális gépen (virtuális) a Windows Server operációs rendszert futtató szolgáltatásként. Szolgáltatásként mély biztonsági kártevővédelem, a tűzfal, egy behatolási megelőzése rendszer és tartalmazza integritását figyelése.

Az ügyfél telepítve van a virtuális Agent keresztül biztonsági bővítményként. Új virtuális gépen telepítse a virtuális Agent a mély biztonsági ügynök együtt. Egy meglévő virtuális gépen, amely a virtuális Agent nincs akkor kell letöltheti és telepítheti azt. Ez a cikk bemutatja a két helyzetben.

Ha a helyszíni megoldás Trend Micro meglévő szóló előfizetés vele az Azure virtuális gépeken futó védelme érdekében. Ha nem egy ügyfél még, iratkozzon fel a próba-előfizetés. Többet szeretne tudni a megoldás olvassa el a [Microsoft Azure virtuális ügynök bővítmény-mély biztonsági](http://go.microsoft.com/fwlink/p/?LinkId=403945)Trend Micro blogbejegyzésből című témakört.

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Egy új virtuális a mély biztonsági ügynök telepítése

A virtuális Agent és a Trend Micro biztonsági bővítmény telepítésekor, akkor az **A gyűjtemény** hozza létre a virtuális gép az [Azure klasszikus portal](http://manage.windowsazure.com) segítségével. Ha egy virtuális egyetlen számítógépre készít, ugyanígy védelem a Trend Micro a portálon.

**A gyűjtemény** beállítás megnyílik egy varázslót, amely segít beállítani a virtuális gépen. A varázsló utolsó lapja használatával a virtuális ügynök és a Trend Micro biztonsági bővítmény telepítése. Általános útmutató lehetőséget olvassa el a [virtuális gép létrehozása az Azure klasszikus portálon Windows rendszerű](virtual-machines-windows-classic-tutorial.md)című témakört. Ha az utolsó a varázsló lapján, tegye a következőket:

1.  A **Virtuális ügynök**ellenőrizze a **Virtuális ügynök telepítése**.

2.  **Biztonsági bővítmények**csoportban jelölje be a **Trend Micro mély biztonsági ügynök**.

    ![A virtuális Agent és a mély biztonsági ügynök telepítése](./media/virtual-machines-windows-classic-install-trend/InstallVMAgentandTrend.png)

3.  Kattintson a jelölőnégyzet be van jelölve a virtuális gép létrehozása.

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Egy meglévő virtuális a mély biztonsági ügynök telepítése

Egy meglévő virtuális a agent telepítéséhez az alábbiakra van szükség:

- Az Azure PowerShell-modult, a verzió 0.8.2 vagy újabb, a helyi számítógépre telepítve. Ellenőrizheti, hogy az Azure PowerShell használatával telepített verziója a **Get-modul azure |} táblázat formázása verzió** parancsot. Útmutatást és a legújabb verzióra mutató hivatkozást megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md). Jelentkezzen be az Azure-előfizetést használ `Add-AzureAccount`.

- A virtuális Agent a cél virtuális gépen.

Először ellenőrizze, hogy a virtuális Agent már telepítve van. Írja be a felhőalapú szolgáltatás neve és a virtuális számítógép nevét, és futtassa a következő parancsok végrehajtása egy rendszergazdai szintű Azure PowerShell parancssorba. Mindent, ami az idézőjelekkel együtt cseréje a < és > karaktereket.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Ha nem tudja, hogy a felhőbeli szolgáltatástól és a virtuális számítógépnév, futtassa a **Get-AzureVM** ezeket az információkat a virtuális gépeken futó jelennek meg a jelenlegi előfizetését.

Az **írás-host** parancs **Igaz**értéket ad vissza, ha a virtuális ügynök telepítve van. Azt **hamis**értéket ad vissza, ha a képernyőn megjelenő utasításokat és az Azure blogban letöltéséhez [virtuális ügynök](http://go.microsoft.com/fwlink/p/?LinkId=403947)és bővítmények - kijelző 2 hivatkozás témakörben talál.

Ha a virtuális ügynök telepítve van, ezek a parancsok futtathatók.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Következő lépések

A agent indítása fut, ha telepítve van néhány percet vesz igénybe. Ezután azt előbb aktiválnia kell mély biztonsági a virtuális gépen, azt is kezelhetők a mély biztonsági kezelője. A következő további útmutatásért lásd:

- A trend cikk arról, hogy a megoldás [Instant-On Cloud Security a Microsoft Azure](http://go.microsoft.com/fwlink/?LinkId=404101)
- [A Windows PowerShell mintaparancsfájl](http://go.microsoft.com/fwlink/?LinkId=404100) beállítása a virtuális gépen
- [Utasítások](http://go.microsoft.com/fwlink/?LinkId=404099) a minta

## <a name="additional-resources"></a>További források

[Hogyan lehet bejelentkezni a Windows Server operációs rendszert futtató virtuális gép]

[Azure virtuális Extensions és szolgáltatások]


<!--Link references-->
[Hogyan lehet bejelentkezni a Windows Server operációs rendszert futtató virtuális gép]: virtual-machines-windows-classic-connect-logon.md
[Azure virtuális Extensions és szolgáltatások]: http://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
