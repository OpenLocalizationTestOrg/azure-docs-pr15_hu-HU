<properties 
    pageTitle="ÜZLETI alkalmazás tesztkörnyezetben |} Microsoft Azure" 
    description="Megtudhatja, hogy miként üzleti alkalmazás webalapú sor létrehozása a felhőben hibrid környezetben informatikai pro vagy a tesztelés fejlesztési." 
    services="virtual-machines-windows" 
    documentationCenter="" 
    authors="JoeDavies-MSFT" 
    manager="timlt" 
    editor=""
    tags="azure-resource-manager"/>

<tags 
    ms.service="virtual-machines-windows" 
    ms.workload="infrastructure-services" 
    ms.tgt_pltfrm="vm-windows" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2016" 
    ms.author="josephd"/>

# <a name="set-up-a-web-based-lob-application-in-a-hybrid-cloud-for-testing"></a>A hibrid felhőben teszteléshez egy webes üzleti alkalmazás beállítása

Ez a témakör a Microsoft Azure-ban tárolt üzleti (üzleti) alkalmazás webalapú vonal teszteléshez szimulált hibrid felhő környezet létrehozásának folyamatát lépéseket. Az alábbiakban az eredményül kapott konfigurációt.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)

Ebben a konfigurációban áll:

- Az Azure (TestLab VNet) tárolt szimulált a helyszíni hálózaton.
- Az Azure (TestVNET) tárolt határokon helyszíni virtuális hálózat.
- A VNet-VNet virtuális Magánhálózati kapcsolat.
- A webes üzleti server, a SQL server és a másodlagos tartományvezérlőnek a TestVNET virtuális hálózaton.

Ebben a konfigurációban alap és közös kiindulási pontként, akkor is biztosít:

- Fejlesztése, és tesztelje a üzleti alkalmazások egy SQL Server 2014-es adatbázis kódmentes Azure-ban található Internet Information Services (IIS).
- Teszteléssel győződjön meg a szimulált hibrid felhőalapú informatikai terhelést.

Vannak három fő fázisait a hibrid felhő tesztkörnyezetben beállítása:

1.  Állítsa be a felhőben szimulált hibrid környezetben.
2.  Állítsa be az SQL server (SQL1).
3.  Konfigurálja az üzleti kiszolgálót (LOB1).

Ez a terhelést egy Azure-előfizetést igényel. Ha az MSDN webhelyen vagy a Visual Studio előfizetéssel rendelkezik, olvassa el a [havi Azure hitelképesség Visual Studio előfizetők számára](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)című témakört.

Példa egy gyártási Azure-ban tárolt üzleti alkalmazás olvassa el a a [Microsoft-szoftverek architektúradiagramok](http://msdn.microsoft.com/dn630664)és tervrajzokat **üzleti** architektúra tervrajz című témakört.

## <a name="phase-1-set-up-the-simulated-hybrid-cloud-environment"></a>1 fázis: A felhő szimulált hibrid környezet beállítása

A [hibrid felhő tesztkörnyezetben szimulált](virtual-machines-windows-ps-hybrid-cloud-test-env-sim.md)létrehozása Mivel ez tesztkörnyezetben nem kell-e a Corpnet alhálózat APP1 kiszolgálón jelenlétét, akkor is le most.

Ez a beállítás az aktuális.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph1.png)
 
## <a name="phase-2-configure-the-sql-server-computer-sql1"></a>2 fázis: Állítsa be az SQL server számítógépen (SQL1)

Az Azure portálról indítsa el a DC2 számítógépet, ha szükséges.

Ezután hozzon létre egy virtuális gép SQL1 ezek a parancsok az Azure PowerShell parancssorba a helyi számítógépre. Ezek a parancsok fut, előtt töltse ki a változók értékein, és távolítsa el a < és > karaktereket.

    $rgName="<your resource group name>"
    $locName="<the Azure location of your resource group>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name SQL1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName SQL1 -VMSize Standard_A4
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $vhdURI=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-SQLDataDisk.vhd"
    Add-AzureRMVMDataDisk -VM $vm -Name "Data" -DiskSizeInGB 100 -VhdUri $vhdURI  -CreateOption empty
    
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for the SQL Server computer." 
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName SQL1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftSQLServer -Offer SQL2014-WS2012R2 -Skus Standard -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/SQL1-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name "OSDisk" -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Az Azure portal segítségével SQL1 csatlakoztatása SQL1 helyi rendszergazdai fiókkal.

Ezután beállítása a Windows tűzfal szabályok ahhoz, hogy egyszerű kapcsolat tesztelése és az SQL Server-forgalmat. Egy rendszergazdai szintű a Windows PowerShell parancssorból kapott SQL1 ezek a parancsok futtathatók.

    New-NetFirewallRule -DisplayName "SQL Server" -Direction Inbound -Protocol TCP -LocalPort 1433,1434,5022 -Action allow 
    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

A ping parancsot az IP-cím 192.168.0.4 négy sikeres választ kell eredményez.

Ezután adja hozzá a felesleges adatok lemez a SQL1 olyan új kötet, amelynek f betűjele.

1.  A bal oldali ablaktáblában a Kiszolgálókezelő kattintson a **fájl- és tároló szolgáltatást**, és válassza a **lemez**.
2.  A Tartalom ablaktáblán, a **lemezt** csoportban kattintson **lemez 2** (a **partíciót** **Ismeretlen**meg).
3.  Kattintson a **Feladatok nézetre**, és kattintson az **Új mennyiségi**.
4.  Kattintson az előtte kezdje el az új mennyiségi varázsló lapján található, kattintson a **Tovább**gombra.
5.  A válassza ki a kiszolgáló és a lemez lapra kattintson a **lemez 2**elemre, és kattintson a **Tovább gombra**. Amikor a rendszer kéri, kattintson az **OK gombra**.
6.  Adja meg a mennyiségi lap méretét kattintson a **Tovább**gombra.
7.  A kijelölés betűt vagy mappa lapra kattintson a **Tovább**gombra.
8.  A fájl kiválasztása rendszer beállításai lapon kattintson a **Tovább**gombra.
9.  A Confirm beállítások lapon kattintson a **Létrehozás**gombra.
10. Amikor elkészült, kattintson a **Bezárás**gombra.

Ezek a parancsok a Windows PowerShell-parancssorában futtassa SQL1:

    md f:\Data
    md f:\Log
    md f:\Backup

Ezután SQL1 csatlakozzon a CORP Windows Server Active Directory tartományi ezek a parancsok, a Windows PowerShell-parancssorában SQL1 a.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

A CORP\User1 fiókot, amikor a rendszer kéri segítségével adja meg a **Számítógép hozzáadása** parancs a tartományi fiók hitelesítő adatait.

Újraindítás után csatlakozhat az Azure portál SQL1 *SQL1 a helyi rendszergazdai fiókkal*.

Ezután állítsa be az SQL Server 2014-es az f-meghajtó használatára, az új adatbázisok és a felhasználói fiók engedélyeket.

1.  A kezdőképernyőn írja be az **SQL Server Management**, és válassza az **SQL Server 2014-es Management Studio eszközben**.
2.  A **Csatlakozás a kiszolgálóhoz**kattintson a **Csatlakozás**gombra.
3.  Az objektum-tallózó fa panelen kattintson a jobb gombbal a **SQL1**, és válassza a **Tulajdonságok parancsot**.
4.  A **Kiszolgáló Tulajdonságok** párbeszédpanelen kattintson az **Adatbázis**.
5.  Keresse meg az **adatbázis-alapértelmezett helyeken** , és adja meg a következő értékeket: 
    - Az **adatok**írja be az elérési út **f:\Data**.
    - **Log**írja be az elérési út **f:\Log**.
    - **Biztonsági másolat**írja be az elérési út **f:\Backup**.
    - Megjegyzés: Csak az új adatbázisok használja a következő helyekhez.
6.  Az ablak bezárásához az **OK** gombra.
7.  Az **Objektum Explorer** fa ablakban nyissa meg a **biztonsági**.
8.  Kattintson a jobb gombbal a **bejelentkezések** , és kattintson az **Új bejelentkezési**.
9.  A **felhasználónév**mezőbe írja be a **CORP\User1**.
10. A **Kiszolgálói szerepkörök** lapon kattintson a **rendszergazdák**, és kattintson **az OK**gombra.
11. Zárja be a Microsoft SQL Server Management Studio eszközben.

Ez a beállítás az aktuális.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph2.png)
 
## <a name="phase-3-configure-the-lob-server-lob1"></a>3 fázis: Konfigurálja az üzleti-kiszolgálót (LOB1)

Először létre virtuális gép LOB1 az ezek a parancsok a helyi számítógépen Azure PowerShell parancssorba.

    $rgName="<your resource group name>"
    $locName="<your Azure location, such as West US>"
    $saName="<your storage account name>"
    
    $vnet=Get-AzureRMVirtualNetwork -Name "TestVNET" -ResourceGroupName $rgName
    $subnet=Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name "TestSubnet"
    $pip=New-AzureRMPublicIpAddress -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
    $nic=New-AzureRMNetworkInterface -Name LOB1-NIC -ResourceGroupName $rgName -Location $locName -Subnet $subnet -PublicIpAddress $pip
    $vm=New-AzureRMVMConfig -VMName LOB1 -VMSize Standard_A2
    $storageAcc=Get-AzureRMStorageAccount -ResourceGroupName $rgName -Name $saName
    $cred=Get-Credential -Message "Type the name and password of the local administrator account for LOB1."
    $vm=Set-AzureRMVMOperatingSystem -VM $vm -Windows -ComputerName LOB1 -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm=Set-AzureRMVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm=Add-AzureRMVMNetworkInterface -VM $vm -Id $nic.Id
    $osDiskUri=$storageAcc.PrimaryEndpoints.Blob.ToString() + "vhds/LOB1-TestLab-OSDisk.vhd"
    $vm=Set-AzureRMVMOSDisk -VM $vm -Name LOB1-TestVNET-OSDisk -VhdUri $osDiskUri -CreateOption fromImage
    New-AzureRMVM -ResourceGroupName $rgName -Location $locName -VM $vm

Ezután az Azure portal segítségével LOB1 csatlakoztatása LOB1 helyi rendszergazdai fiókjának hitelesítő adataival.

Ezután beállítása a Windows tűzfal szabály egyszerű kapcsolat tesztelése forgalmának engedélyezésére. Egy rendszergazdai szintű a Windows PowerShell parancssorból kapott LOB1 ezek a parancsok futtathatók.

    Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
    ping dc2.corp.contoso.com

A ping parancsot az IP-cím 192.168.0.4 négy sikeres választ kell eredményez.

Ezután LOB1 csatlakozzon a CORP Active Directory tartományi, ezek a parancsok, a Windows PowerShell-parancssorában.

    Add-Computer -DomainName corp.contoso.com
    Restart-Computer

A CORP\User1 fiókot, amikor a rendszer kéri segítségével adja meg a **Számítógép hozzáadása** parancs a tartományi fiók hitelesítő adatait.

Újraindítása után csatlakozhat az Azure portál LOB1 CORP\User1 fiók és jelszóval.

Ezután LOB1 konfigurálása az IIS, és tesztelje a ÜGYFÉL1 hozzáférést.

1.  A Kiszolgálókezelő kattintson a **Hozzáadás szerepkörök és szolgáltatások**elemre.
2.  **Első lépések** lapon kattintson a **Tovább**gombra.
3.  A **telepítés típusának kiválasztása** lapon kattintson a **Tovább**gombra.
4.  **Jelölje be a cél kiszolgálón** lapon kattintson a **Tovább**gombra.
5.  A **kiszolgálói szerepkörök** lapon kattintson a **Webkiszolgáló (IIS)** **szerepkörének**felsorolása.
6.  Amikor a rendszer kéri, kattintson a **Szolgáltatás hozzáadása**elemre, és kattintson a **Tovább gombra**.
7.  **Válassza a lehetőségek** lapon kattintson a **Tovább**gombra.
8.  **Webkiszolgáló (IIS)** lapon kattintson a **Tovább**gombra.
9.  **Jelölje ki a szerepkör-szolgáltatások** lapon jelölje be vagy törölje a jelet a jelölőnégyzetből a szolgáltatásokra van szüksége az üzleti alkalmazás tesztelése, és kattintson a **Tovább gombra**.
10. A **Confirm telepítési beállítások** lapon kattintson a **telepítés**gombra.
11. Várja meg, amíg a összetevők telepítése befejeződött, és kattintson a **Bezárás**gombra.
12. Az Azure portálról csatlakoztatása az ÜGYFÉL1 számítógépet a CORP\User1 fiók hitelesítő adataival, és indítsa el az Internet Explorer.
13. A címsorban írja be a **http://lob1/** , és nyomja le az ENTER BILLENTYŰT. Meg kell jelennie a alapértelmezett IIS 8 weblapot.

Ez a beállítás az aktuális.

![](./media/virtual-machines-windows-ps-hybrid-cloud-test-env-lob/virtual-machines-windows-ps-hybrid-cloud-test-env-lob-ph3.png)
 
Ez a környezet ettől kezdve készen áll a LOB1 a webes alkalmazás telepítéséhez, és tesztelje a Corpnet alhálózat ÜGYFÉL1 szolgáltatásai.

## <a name="next-step"></a>Következő lépés

- Adja hozzá az [Azure portál](virtual-machines-windows-hero-tutorial.md)új virtuális gépet.
