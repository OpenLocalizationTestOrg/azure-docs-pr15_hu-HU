<properties
    pageTitle="Az SQL Server üzleti intelligenciával kapcsolatos funkciók |} Microsoft Azure"
    description="Ez a témakör a klasszikus telepítési modell készült erőforrásokat használ, és ismerteti a vállalati Üzletiintelligencia-funkciókat a Azure virtuális gépeken futó (VMs) futó SQL Server."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="guyinacube"
    manager="erikre"
    editor="monicar"
    tags="azure-service-management"/>
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/04/2016"
    ms.author="asaxton" />

# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>Az SQL Server üzleti intelligenciával kapcsolatos funkciók az Azure virtuális gépeken futó

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

A Microsoft Azure virtuális gépek galéria képeket tartalmazó SQL Server-telepítések tartalmazza. Az SQL Server kiadásában támogatott a gyűjtemény képeket a helyszíni számítógépek és a virtuális gépeken futó telepítheti azonos telepítőfájlokat. Ez a témakör az SQL Server üzleti Üzletiintelligencia-funkciók telepítve van a képek és konfiguráció szükséges lépésekről után egy virtuális gép már kiépítve összegzi. Ez a témakör is ismerteti az Üzletiintelligencia-funkciókat és a gyakorlati tanácsok a támogatott telepítés topológiák.

## <a name="license-considerations"></a>Licenccel kapcsolatos kérdések

A Microsoft Azure virtuális gépeken futó SQL Server licencre két módja van:

1. Frissítési garancia részeként mobilitás juttatások licencet. További tudnivalókért lásd: [Licenc mobilitás Azure a frissítési garancia keresztül](https://azure.microsoft.com/pricing/license-mobility/).

1. A fizetés per óradíj az Azure virtuális gépeken futó SQL Server-telepítve. A [Virtuális gépeken futó árak](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql)"SQL Server" című.

Licencelési és az aktuális díjak kapcsolatos további tudnivalókért olvassa el a [Virtuális gépeken futó licencelési – gyakori kérdések](https://azure.microsoft.com/pricing/licensing-faq/%20/)című témakört.

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>Az SQL Server ké Azure virtuális gépi gyűjtemény

A Microsoft Azure virtuális gépek galéria, amelyeknél a Microsoft SQL Server több kép tartalmazza. A virtuális gép képeket a szoftvert az operációs rendszer verziója és az SQL Server függ. A listában az Azure virtuális gépek galéria elérhető képek gyakran változik.

![Kép SQL azure virtuális gyűjtemény](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)

![A PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Az alábbi PowerShell parancsprogramot: Azure képek "SQL-kiszolgálójába" a ImageName tartalmazó listáját adja vissza:

    # assumes you have already uploaded a management certificate to your Microsoft Azure Subscription. View the thumbprint value from the "settings" menu in Azure classic portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is the one specified with the -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Többet kiadásainak és az SQL Server által támogatott funkciók olvassa el a következőket:

- [Az SQL Server kiadása](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)

- [Az SQL Server 2016 kiadásai által támogatott funkciók](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-the-sql-server-virtual-machine-gallery-images"></a>Telepítve van a virtuális gépi gyűjtemény képek az SQL Server üzleti INTELLIGENCIÁVAL kapcsolatos funkciók

Az alábbi táblázat összefoglalja az üzleti intelligenciával kapcsolatos funkciók telepítve a Microsoft Azure virtuális gép közös gyűjtemény képek az SQL Server"

- Az SQL Server 2016 RC3

- Az SQL Server 2014-es SP1 Enterprise

- Az SQL Server 2014-es SP1 Standard

- Az SQL Server 2012 SP2 vállalati

- Az SQL Server 2012 SP2 Standard

|SQL Server Üzletiintelligencia-funkció|Telepítve van a a gyűjteménye|Jegyzetek|
|---|---|---|
|**A Reporting Services natív mód**|igen|Telepítve van, de igényel beállítást, beleértve a jelentés manager URL-címe. A [Reporting Services konfigurálása](#configure-reporting-services)szakaszban olvashat.|
|**SharePoint-üzemmódban a Reporting Services**|nem|A Microsoft Azure virtuális gép gyűjteménye nem része a SharePoint vagy a SharePoint telepítőfájlokat. <sup>1</sup>|
|**Többdimenziós Analysis Services és az adatok adatbányászati (OLAP)**|igen|Telepítette és beállította az Analysis Services-példányt alapértelmezettként|
|**Az Analysis Services táblázatos**|nem|Támogatott SQL Server 2012, 2014- és 2016 képek, de ez az alapértelmezés szerint nincs telepítve. Telepítse az Analysis Services egy másik példányát. Lásd az ebben a témakörben telepítse az egyéb SQL Server-szolgáltatások és funkciók.|
|**SharePoint-Analysis Services Power Pivot programban**|nem|A Microsoft Azure virtuális gép gyűjteménye nem része a SharePoint vagy a SharePoint telepítőfájlokat. <sup>1</sup>|

<sup>1</sup> SharePoint és az Azure virtuális gépeken futó további tudnivalókért olvassa el [A Microsoft Azure architektúrákban a SharePoint 2013-hoz](https://technet.microsoft.com/library/dn635309.aspx) és a [SharePoint üzembe a Microsoft Azure virtuális gépeken futó](https://www.microsoft.com/download/details.aspx?id=34598).

![A PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) A következő parancsot a PowerShell egy listát a telepített szolgáltatások, amelyek tartalmazzák a "SQL" a szolgáltatás neve.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Általános javaslatok és gyakorlati tanácsok

- A minimális ajánlott virtuális géphez mérete **A3** SQL Server Enterprise Edition használata esetén. Az SQL Server BI Analysis Services és a Reporting Services telepítéseknél ajánlott **A4** virtuális gép méretét.

    Az aktuális virtuális méretű további tudnivalókért lásd [Az Azure virtuális gép méretét](virtual-machines-linux-sizes.md).

- Egy célszerű lemez management adatok, a napló és a biztonságimásolat- **C**eltérő meghajtókon lévő fájlok tárolására,: és a **D**:. Létrehozhat például adatok lemez **E**: és az **F**:.

    - A gyorsítótár-házirendet, az alapértelmezett meghajtó **C**meghajtó: nem optimális az adatok használatához.

    - A **D**: meghajtó az ideiglenes meghajtó, amellyel elsősorban a lap fájlt. A **D**: meghajtó nem megőrződnek, és a program nem menti a blob-tárolóhoz. Felügyeleti feladatok, például a virtuális gép méretének módosítása a **D**visszaállítása: meghajtóra. Ajánlott **nem** használja a **D**: adatbázis-fájlok, többek között a tempdb meghajtóra.

    További információt a létrehozásáról és feltüntetésével lemezen megtudhatja, [hogy miként csatolni virtuális géphez adatok lemezen](virtual-machines-windows-classic-attach-disk.md).

- Le, vagy távolítsa el a nem kívánt szolgáltatásokat használja. A példában ha a virtuális gép csak a Reporting Services használatos leállítása vagy távolítsa el az Analysis Services és az SQL Server Integration Services. Az alábbi képen az alapértelmezés szerint elindított szolgáltatások példája.

    ![Az SQL Server-szolgáltatások](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)

    >[AZURE.NOTE] Az SQL Server-adatbázis motor a támogatott Üzletiintelligencia-forgatókönyvekben van szükség. Az egyetlen kiszolgáló virtuális topológiát a adatbázismotor futnia kell a azonos virtuális szükséges.

    További információ a következő témakörben találhat: [Reporting Services távolítsa el](https://msdn.microsoft.com/library/hh479745.aspx) és [eltávolítása egy példány Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).

- Jelölje be a **Windows Update** az új "fontos frissítéseket". A Microsoft Azure virtuális gép képek gyakran frissüljenek; azonban fontos frissítések válhat letölthető a **Windows Update** után a virtuális kép lett utoljára frissítve.

## <a name="example-deployment-topologies"></a>Példa telepítési topológiák

Az alábbiakban a Microsoft Azure virtuális gépeken futó használó példa telepítések. Az alábbi diagramokban topológiák nem csak a lehetséges topológiák és a Microsoft Azure virtuális gépeken futó SQL Server Üzletiintelligencia-funkcióit használhatja.

### <a name="single-virtual-machine"></a>Egyetlen virtuális gépen

Analysis Services, Reporting Services, az SQL Server adatbázismotort, adatok és adatforrások egyetlen virtuális gépen.

![üzleti intelligencia számvitel eset: 1 virtuális gép a](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Két virtuális gépeken futó

- Az Analysis Services Reporting Services és az SQL Server adatbázismotort egyetlen virtuális gépen. A telepítési a jelentés kiszolgálói adatbázisok tartalmazza.

- A második virtuális adatforrások. A második virtuális SQL Server adatbázismotort adatforrásként is tartalmaz.

![üzleti intelligencia iaas eset: a 2 virtuális gépeken futó](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Vegyes Azure – adatok Azure SQL-adatbázishoz

- Az Analysis Services Reporting Services és az SQL Server adatbázismotort egyetlen virtuális gépen. A telepítési a jelentés kiszolgálói adatbázisok tartalmazza.

- Adatforrás az Azure SQL-adatbázishoz.

![üzleti intelligencia iaas esetek virtuális és AzureSQL adatforrásként](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hibrid – helyszíni adatok

- A példa telepítési Analysis Services Reporting Services és az SQL Server adatbázismotor futtassa egyetlen virtuális gépen. A virtuális gép tárolja a jelentés kiszolgálói-adatbázisait. A virtuális számítógép csatlakozik egy helyszíni tartomány Azure virtuális hálózaton keresztül, vagy néhány egyéb VPN tunneling megoldás.

- Legyen a helyszíni adatforrás.

![üzleti intelligencia iaas esetek virtuális, majd a helyi adatforrásokat](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Reporting Services natív üzemmód

A virtuális gép gyűjteménye az SQL Server Reporting Services natív mód van telepítve, tartalmaz, azonban a jelentés kiszolgálója nincs beállítva. Ez a szakasz lépései állítsa be a Reporting Services jelentéskészítő kiszolgálót. További információ a Reporting Services natív mód konfigurálása témakörben [Telepítése Reporting Services natív mód jelentés Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

>[AZURE.NOTE] A Windows PowerShell-parancsfájlokat konfigurálása a jelentéskészítő kiszolgálót használó hasonló tartalom olvassa el a [PowerShell használatá létrehozása az Azure virtuális együtt a natív mód jelentéskészítő kiszolgálót](virtual-machines-windows-classic-ps-sql-report.md)című témakört.

### <a name="connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager"></a>A virtuális gép csatlakozzon, és indítsa el a Reporting Services Configuration Manager

Van egy Azure virtuális gépen való csatlakozáshoz két gyakori munkafolyamatok:

- A virtuális gép nevére, kattintson a csatlakozás, és kattintson a **Csatlakozás**. A távoli asztali kapcsolat megnyílik, és automatikusan kitölti a rendszer a számítógép nevét.

    ![azure virtuális gép csatlakoztatása](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)

- Csatlakozás Windows távoli asztali kapcsolat virtuális készülék. A felhasználói felületen a távoli asztali:

    1. Írja be a **felhőalapú szolgáltatás neve** a számítógép neve.

    1. Írja be a kettőspont (:) és a nyilvános portszámot, amely a TCP távoli asztali végpont van beállítva.

        Myservice.cloudapp.NET:63133

        További tudnivalókért lásd: [Mi az, hogy egy felhőalapú szolgáltatásba?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).

**Indítsa el a Reporting Services Configuration Manager.**

1. A **Windows Server 2012**:

1. A **kezdőképernyőn** írja be a **Reporting Services** , az alkalmazások listájának megtekintéséhez.

1. Kattintson a jobb gombbal a **Reporting Services Configuration Manager** , és kattintson a **Futtatás rendszergazdaként**parancsra.

1. A **Windows Server 2008 R2 rendszerben**:

1. Kattintson a **Start**gombra, és kattintson a **Minden program**.

1. Kattintson a **Microsoft SQL Server 2016-ban**.

1. Kattintson a **Configuration Tools**.

1. Kattintson a jobb gombbal a **Reporting Services Configuration Manager** , és kattintson a **Futtatás rendszergazdaként**parancsra.

Vagy

1. Kattintson a **Start**gombra.

1. A **Keresés programokban és fájlokban** párbeszédpanelen írja be a **reporting Services szolgáltatáshoz**. Ha a virtuális fut a Windows Server 2012, írja be a **reporting services** a Windows Server 2012 kezdőképernyőjén.

1. Kattintson a jobb gombbal a **Reporting Services Configuration Manager** , és kattintson a **Futtatás rendszergazdaként**parancsra.

    ![ssrs Konfigurációkezelő keresése](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services konfigurálása

**Szolgáltatásfiók, valamint a webes szolgáltatás URL-címe:**

1. Ellenőrizze a **Kiszolgálónév** a helyi kiszolgáló nevét, és kattintson a **Csatlakozás**gombra.

1. Megjegyzés: az üres **Jelentés Server-adatbázis neve**. Az adatbázis létrejött a konfiguráció befejezése után.

1. **Jelentés kiszolgáló állapotának** ellenőrzéséhez **lépések**. Ha szeretné ellenőrizni a Windows Server kezelő szolgáltatás, akkor a szolgáltatás nem a Windows-alapú **SQL Server Reporting Services** szolgáltatást.

1. Kattintson a **Fiók** , és szükség szerint módosítsa a fiók. A virtuális gép tartományon kívüli illesztett környezetben használja, ha a beépített **jelentéskészítő** fiókot is elegendő. A szolgáltatásfiók kapcsolatos további tudnivalókért olvassa el a [Szolgáltatásfiók](https://msdn.microsoft.com/library/ms189964.aspx)című témakört.

1. A bal oldali ablaktáblában kattintson a **Webhely szolgáltatás URL-CÍMÉT** .

1. Kattintson az **alkalmazás** beállítása az alapértelmezett értékeket.

1. Megjegyzés: a **jelentés kiszolgálói webes szolgáltatás URL-címeket**. Ne feledje, alapértelmezett portja a TCP 80 és az URL-cím része. A leendő hozzon létre egy Microsoft Azure virtuális gép végpontot portszámként.

1. Az **eredmények** ablaktáblájában ellenőrizze a művelet sikeresen befejeződött.

**Adatbázis:**

1. A bal oldali ablaktáblában kattintson az **adatbázisból** .

1. Kattintson a **Módosítás-adatbázisból**.

1. **Új jelentés server-adatbázis létrehozása** választógombot, és kattintson a Tovább gombra.

1. Ellenőrizze a **Kiszolgáló nevét** , majd **A kapcsolat tesztelése**gombra.

1. Ha az eredmény **a kapcsolat tesztelése sikeresen befejeződött**, kattintson az **OK gombra** , és kattintson a **Tovább gombra**.

1. Megjegyzés: az adatbázis neve **jelentéskészítő** és a **jelentéskészítő kiszolgálót mód** **natív** , majd a **Tovább**gombra.

1. A **hitelesítő adatok** lapján kattintson a **Tovább gombra** .

1. Az **Összegzés** lapon kattintson a **Tovább gombra** .

1. A **folyamatban és Befejezés** lapon kattintson a **Tovább gombra** .

**A 2012 és 2014-es a Web portál URL-címe vagy Jelentéskezelő URL-címe:**

1. 2014-es és 2012, a bal oldali ablaktáblában kattintson a **Webes portál URL-cím**vagy **Jelentéskezelő URL-CÍMÉT** .

1. Kattintson a **alkalmazása**gombra.

1. Az **eredmények** ablaktáblájában ellenőrizze a művelet sikeresen befejeződött.

1. Kattintson a **Kilépés**gombra.

Jelentés kiszolgálói engedélyekkel kapcsolatos további tudnivalókért lásd: az [Engedélyek megadása natív mód jelentéskészítő kiszolgálón](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-to-the-local-report-manager"></a>Keresse meg a helyi Jelentéskezelő

Ellenőrizze a konfigurációt, nyissa meg azt a virtuális a jelentéskezelőből.

1. A virtuális, a rendszergazda jogosultságokkal rendelkező az Internet Explorer indítása.

1. Tallózással keresse meg a virtuális http://localhost/reports.

### <a name="to-connect-to-remote-web-portal-or-report-manager-for-2014-and-2012"></a>A csatlakozás távoli webes portál vagy 2012 és 2014-es Jelentéskezelő

Ha a használni kívánt webes portál vagy Jelentéskezelő 2014-es és 2012 távoli számítógépről virtuális gépen új virtuális gép TCP-végpont létrehozása. Alapértelmezés szerint a jelentéskészítő kiszolgálót figyeli **80-as port**a HTTP-kérelmeket. Ha a jelentés kiszolgáló URL-ek használata egy másik port adja meg, az alábbi lépéseket kell megadnia a portszámot.

1. A virtuális gép a TCP 80-as Port zárólap hozhat létre. További információ, a [Végpontok virtuális gép és tűzfalportokat](#virtual-machine-endpoints-and-firewall-ports) szakasz a dokumentumban.

1. Nyissa meg a virtuális gép tűzfal 80-as port.

1. Nyissa meg a webes portál vagy a jelentéskezelőből, a kiszolgáló nevét az URL-cím Azure virtuális gép **Tartománynév** használatával. Példa:

    **Jelentéskészítő kiszolgálót**: http://uebi.cloudapp.net/reportserver  **webes portál**: http://uebi.cloudapp.net/reports

    [A jelentés kiszolgálói hozzáférés tűzfal beállítása](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="to-create-and-publish-reports-to-the-azure-virtual-machine"></a>Hozhat létre, és tegye közzé a jelentéseket a Azure virtuális géphez

Az alábbi táblázat összefoglalja, hogy a meglévő kimutatások egy helyszíni számítógépről, a jelentéskészítő kiszolgálón tárolt a a Microsoft Azure virtuális gép közzététele elérhető beállítások:

- **A jelentéskészítő**: A virtuális gép is tartalmaz, kattintson a-egyszer verzióját a Microsoft SQL Server jelentéskészítő használatával létrehozott SQL 2014-es és 2012. Jelentés builder az első alkalommal indítása az SQL-2016 virtuális gépen:

    1. Indítsa el a böngésző rendszergazdai jogosultságokkal rendelkező.

    1. Keresse meg a webes portált a virtuális gépen, és válassza ki a **Letöltés** ikonra a jobb felső sarokban.
    
    1. Jelölje ki a **Jelentéskészítő használatával létrehozott**.

    További tudnivalókért olvassa el a [Indítsa el a jelentéskészítő használatával létrehozott](https://msdn.microsoft.com/library/ms159221.aspx)című témakört.

- **Az SQL Server Data Tools**: virtuális: SQL Server Data Tools a virtuális gépen telepítve van, és a **Jelentés kiszolgálói projektek** és jelentések létrehozása a virtuális gépen használható. Az SQL Server Data Tools közzéteheti a jelentések, a jelentéskészítő kiszolgálón a virtuális gépen.

- **SQL Server Data Tools: távoli**: a helyi számítógépen Reporting Services projekt létrehozása az SQL Server Data Tools Reporting Services-jelentéseket tartalmazó. Állítsa be a projekt való csatlakozáshoz a webes szolgáltatás URL-címe.

    ![projekt tulajdonságainak SSDT SSRS projekt](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)

- Hozzon létre egy. Virtuális merevlemez-meghajtón, jelentést tartalmaz, és töltse és a meghajtó csatolni.

    1. Hozzon létre egy. Virtuális merevlemez-meghajtó a helyi számítógépen, amely tartalmazza a jelentéseket.

    1. Hozzon létre, és telepítse az adatkezelési tanúsítvány.

    1. A parancsmaggal hozzáadása-AzureVHD [létrehozása és feltöltése a Windows Server virtuális az Azure](virtual-machines-windows-classic-createupload-vhd.md)Azure töltse fel a virtuális fájlt.

    1. A lemez csatolni virtuális gépen.

## <a name="install-other-sql-server-services-and-features"></a>Más SQL Server-szolgáltatások és funkciók telepítése

További SQL Server-szolgáltatásokat, például az Analysis Services táblázatos módban telepítéséhez futtassa az SQL server beállítási varázsló. A telepítő fájlok helyi lemezre a virtuális gépen.

1. Kattintson a **Start** gombra, és válassza a **Minden program**.

1. Kattintson a **Microsoft SQL Server 2016-ban**, a **Microsoft SQL Server 2014-es** vagy a **Microsoft SQL Server 2012** , és kattintson a **Configuration Tools**.

1. Kattintson a **SQL Server telepítése elemre**.

Vagy C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe vagy C:\SQLServer_11.0_full\setup.exe futtatása

>[AZURE.NOTE] Az első alkalommal futtatja az SQL Server telepítése további telepítőfájlokat letölthetők, és csak a újra kell indítani a virtuális gép, újra kell indítani az SQL Server telepítése.
>
>Ha többször testre a képet kijelölve a a Microsoft Azure virtuális gép van szükség, fontolja meg, a saját SQL Server kép létrehozását. Analysis Services SysPrep funkcióinak engedélyezték az SQL Server 2012 SP1 CU2. További tudnivalókért lásd: [telepíti az SQL Server használatával SysPrep szempontjai](https://msdn.microsoft.com/library/ee210754.aspx) és a [Kiszolgálói szerepkörök Sysprep támogatása](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="to-install-analysis-services-tabular-mode"></a>Analysis Services táblázatos mód telepítése

Ennek lépéseit szakasz **Összegzés** az Analysis Services táblázatos mód telepítését. További tudnivalókért lásd: a következőket:

- [Telepítse az Analysis Services táblázatos módban](https://msdn.microsoft.com/library/hh231722.aspx)

- [Táblázatos modellezése (Adventure Works oktatóprogram)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**Analysis Services táblázatos mód telepítése:**

1. Az SQL Server telepítése varázslóban kattintson a **telepítés** a bal oldali ablaktáblában, és válassza a **Új SQL server önálló telepítés vagy szolgáltatások hozzáadása meglévő telepítés**.

    - Ha megjelenik a **Tallózás**, tallózással keresse meg a c:\SQLServer_13.0_full, c:\SQLServer_12.0_full vagy c:\SQLServer_11.0_full, és kattintson az **OK gombra**.

1. A termék frissítések lapon kattintson a **Tovább gombra** .

1. A **Telepítési típus** lapon jelölje ki a **végrehajtandó egy új, az SQL Server telepítése** , és kattintson a **Tovább**gombra.

1. A **Telepítő szerepkör** lapon kattintson az **SQL Server szolgáltatások telepítésével**.

1. A **Szolgáltatás kiválasztása** lapon kattintson az **Analysis Services szolgáltatásból**.

1. A **Példány beállítása** lapon adja meg nevét, például **táblázatos** **Nevű példány** és **Példány azonosítója** mezőbe.

1. **Az Analysis Services beállítása** lapon válassza ki a **Táblázatos módot**. Az aktuális felhasználó hozzáadása a rendszergazdai engedélyekkel listában.

1. Töltse ki, és zárja be az SQL Server telepítés varázslót.

## <a name="analysis-services-configuration"></a>Az Analysis Services beállítása

### <a name="remote-access-to-analysis-services-server"></a>Analysis Services-kiszolgáló távoli eléréséhez

Analysis Services-kiszolgáló csak támogatja a windows-hitelesítés használatával. Az Analysis Services távoli eléréséhez ügyfél alkalmazásokból, például SQL Server Management Studio vagy SQL Server Data Tools, a virtuális gépen kell kell tartományhoz a helyi, Azure virtuális hálózat segítségével. További információ, [Azure virtuális hálózat](../virtual-network/virtual-networks-overview.md).

Az Analysis Services egy **alapértelmezett példány** TCP port **2383**figyeli. Nyissa meg a port a virtuális gépeken futó tűzfalon keresztül. Az Analysis Services egy csoportosított elnevezett példánya is figyeli port **2383**.

**Megnevezett példány** az Analysis Services, az a SQL Server-tallózó port hozzáférés kezelése a szükséges. Az SQL Server böngésző alapértelmezett konfigurációja port **2382**.

A virtuális gépeken futó tűzfal nyissa meg a port **2382** , és hozzon létre egy példány port nevű statikus Analysis Services.

1. Már használatban van a virtuális, és milyen folyamat használja a portokat portok ellenőrzéséhez rendszergazdai jogosultságokkal rendelkező futtassa a következő parancsot:

        netstat /ao

1. Használata SQL Server Management Studio hozhat létre egy statikus Analysis Services-nevű példány port frissítésével Port"érték táblázatos, mint például az általános tulajdonságok. További információ látható a "a rögzített portot használja az alapértelmezett vagy megnevezett példány" az [Analysis Services hozzáférés engedélyezése a Windows tűzfal konfigurálása](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).

1. Indítsa újra az Analysis Services szolgáltatás táblázatos példányát.

További információ, a **Végpontok virtuális gép és tűzfalportokat** szakasz a dokumentumban.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Virtuális gép végpontok és a tűzfalportokat

Ebben a szakaszban a Microsoft Azure virtuális gép végpont létrehozásához, és nyissa meg a virtuális gép tűzfalak-portok foglalja össze. Az alábbi táblázat összefoglalja a **TCP** végpontokat létrehozásához és a porttal a virtuális gépeken futó tűzfal megnyitásához.

- Egy egyetlen virtuális használja, és a következő két elemek mindegyike igaz, ha a szükségtelen hozhat létre virtuális végpontok, és nem kell a tűzfalat a virtuális nyissa meg a portokat.

    - Ön nem távolról kapcsolódni a virtuális az SQL Server funkciókkal. A virtuális távoli asztali kapcsolat létrehozásáról és a helyileg a virtuális az SQL Server-funkciók elérése nem tekinthető az SQL Server-funkcióinak távoli kapcsolatot.

    - A virtuális nem csatlakozik egy helyszíni tartományhoz Azure virtuális hálózati vagy egy másik VPN közötti megoldás keresztül.

- Ha a virtuális gép nem csatlakozik egy tartományt, de szeretne távolról csatlakozhat az SQL Server-funkciók a virtuális:

    - Nyissa meg a portokat a tűzfalat a virtuális.

    - Hozzon létre virtuális gép végpontok feljegyzett portjához (*).

- Ha a virtuális gépet használ egy VPN-csatorna, például az Azure virtuális hálózati tartomány csatlakozik, majd a végpontokat nem szükségesek. A portokat azonban nyissa meg a tűzfalat a virtuális.

  	|Port|Típus|Leírás|
|---|---|---|
|**80**|TCP|Jelentéskészítő kiszolgálót távelérési (*).|
|**1433**|TCP|SQL Server Management Studio (*).|
|**1434**|UDP|Az SQL Server böngészőben. Ez szükséges, amikor a virtuális a tartományhoz.|
|**2382**|TCP|Az SQL Server böngészőben.|
|**2383**|TCP|SQL Server Analysis Services alapértelmezett példány és csoportosított elnevezett példányok.|
|**A felhasználó által definiált**|TCP|Hozzon létre egy példány port neve a portszámot, kiválaszthatja, hogy statikus Analysis Services, és ezután tiltásának feloldása a portszámot, a tűzfal.|

Végpontok létrehozásával kapcsolatos további tudnivalókért lásd: a következőket:

- Hozza létre a végpontok:[hogyan állíthat be egy virtuális gép végpontok](virtual-machines-windows-classic-setup-endpoints.md).

- SQL Server: Olvassa el a "Teljes konfigurációs lépéseit a virtuális gép használata SQL Server Management Studio csatlakoztatása" szakaszát [virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md).

Az alábbi ábra szemlélteti a a virtuális tűzfal szolgáltatások és -összetevők távoli elérésének engedélyezése a virtuális a megnyitni kívánt portokat.

![Nyissa meg az Azure VMs üzleti intelligencia alkalmazások-portok](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Erőforrások

- Tekintse át a Microsoft-kiszolgáló szoftverek az Azure virtuális gép környezetben használja szabályzata. A következő témakör foglalja össze, például BitLocker, feladatátvételét és a terheléselosztás-szolgáltatások támogatása. A [Microsoft-kiszolgáló szoftver támogatja az Azure virtuális gépeken futó](http://support.microsoft.com/kb/2721672).

- [SQL Server Azure virtuális gépeken futó – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md)

- [Virtuális gépeken futó](https://azure.microsoft.com/documentation/services/virtual-machines/)

- [Virtuális gép SQL Server Azure a kiépítési](virtual-machines-windows-portal-sql-server-provision.md)

- [Hogyan csatolhat adatok lemezen virtuális gépen](virtual-machines-windows-classic-attach-disk.md)

- [Adatbázis áttelepítése az SQL Server-Azure virtuális](virtual-machines-windows-migrate-sql.md)

- [Egy Analysis Services-példány a kiszolgálón mód meghatározása](https://msdn.microsoft.com/library/gg471594.aspx)

- [Többdimenziós modellezése (Adventure Works oktatóprogram)](https://technet.microsoft.com/library/ms170208.aspx)

- [Azure dokumentáció központ](https://azure.microsoft.com/documentation/)

- [Power BI használata hibrid környezetben](https://msdn.microsoft.com/library/dn798994.aspx)

>[AZURE.NOTE] [Visszajelzés és a Microsoft SQL Server kapcsolódni keresztül partneradatok elküldése](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Közösségi tartalom

- [Az SQL Azure adatbázis-kezelő szolgáltatást a PowerShell használatával](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)
