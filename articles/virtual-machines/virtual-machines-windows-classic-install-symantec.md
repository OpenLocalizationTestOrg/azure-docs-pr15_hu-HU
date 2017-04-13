<properties
    pageTitle="A virtuális Symantec Endpoint Protection telepítése |} Microsoft Azure"
    description="Megtudhatja, hogy miként telepítheti, állíthatja a Symantec Endpoint Protection biztonsági bővítmény a és a klasszikus telepítési modellt létre egy új vagy meglévő Azure virtuális."
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

# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Telepítése és beállítása a Windows virtuális Symantec Endpoint Protection

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Ez a cikk bemutatja, hogyan telepítheti, állíthatja a Symantec Endpoint Protection ügyfél egy meglévő virtuális gépen (virtuális) a Windows Server operációs rendszert futtató. A teljes ügyfél, amelyek tartalmazzák még a szolgáltatásokból, például a vírussal és a kémprogramok védelmét, a tűzfal és a behatolási megelőzése az. Az ügyfél biztonsági bővítményként az virtuális ügynök használatával telepítve van.

Ha egy meglévő előfizetés Symantec helyszíni megoldás, használhatja az Azure virtuális gépeken futó védelme. Ha nem egy ügyfél még, iratkozzon fel a próba-előfizetés. A megoldás kapcsolatos további tudnivalókért lásd a [A Microsoft Azure platform Symantec Endpoint Protection][Symantec]. Ezen az oldalon a licenceléssel kapcsolatos információkat és az ügyfél telepítésével, ha már Symantec ügyfél kapcsolatos útmutatót mutató hivatkozásokat tartalmaz.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Egy meglévő virtuális Symantec Endpoint Protection telepítése

Mielőtt elkezdené, az alábbiakra van szükség:

- Az Azure PowerShell-modult, a verzió 0.8.2 vagy újabb verzió, a munkahelyi számítógépen. Ellenőrizheti, hogy telepítve van az Azure PowerShell verzióját a **Get-modul azure |} táblázat formázása verzió** parancsot. Az útmutatók és a legújabb verzióra mutató hivatkozást, megtudhatja, [hogy miként telepítése és beállítása Azure PowerShell][PS]. Jelentkezzen be az Azure-előfizetést használ `Add-AzureAccount`.

- A virtuális Agent a Azure virtuális gépen futó.

Először ellenőrizze, hogy a virtuális Agent már telepítve van a virtuális gépen. Írja be a felhőalapú szolgáltatás neve és a virtuális számítógép nevét, és futtassa a következő parancsok végrehajtása egy rendszergazdai szintű Azure PowerShell parancssorba. Mindent, ami az idézőjelekkel együtt cseréje a < és > karaktereket.

> [AZURE.TIP] Ha nem tudja, hogy a felhőbeli szolgáltatástól és a virtuális gép neve, futtassa a **Get-AzureVM** összes virtuális gépeken futó a jelenlegi előfizetéséről a nevek listáját.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Az **írási-host** parancs megjelenik a **Igaz**, ha a virtuális ügynök telepítve van. Ha **FALSE értéket**jeleníti meg, olvassa el a képernyőn megjelenő utasításokat és a [virtuális ügynök]és bővítmények - kijelző 2 az Azure blogban letöltéséhez hivatkozás[Agent].

A virtuális ügynök telepítve van, futtassa a Symantec Endpoint Protection ügynököt a parancsok.

    $Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

    Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection -VM $vm | Update-AzureVM

Ellenőrizze, hogy a Symantec biztonsági bővítmény telepítve van, és naprakész:

1.  Jelentkezzen be a virtuális gépen. Című cikkben olvashat [annak jelentkezzen be a virtuális gépen futó Windows Server][Logon].
2.  Kattintson a Windows Server 2008 R2 **Start > Symantec Endpoint Protection**. A Windows Server 2012-ben vagy Windows Server 2012 R2, a kezdőképernyőről írja be a **Symantec**, és válassza a **Symantec Endpoint Protection**.
3.  Az **állapot** fülre, az **Állapot-Symantec Endpoint Protection** ablak-frissítések telepítése vagy, ha szükséges, indítsa újra.

## <a name="additional-resources"></a>További források

[Jelentkezzen be a Windows Server operációs rendszert futtató virtuális gép hogyan][Logon]

[Azure virtuális Extensions és szolgáltatások][Ext]


<!--Link references-->
[Symantec]: http://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Portal]: http://manage.windowsazure.com

[Create]: virtual-machines-windows-classic-tutorial.md

[PS]: ../powershell-install-configure.md

[Agent]: http://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]: virtual-machines-windows-classic-connect-logon.md

[Ext]: http://go.microsoft.com/fwlink/p/?linkid=390493