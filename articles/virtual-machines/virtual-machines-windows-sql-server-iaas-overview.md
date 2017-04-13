<properties
    pageTitle="Azure virtuális gépeken futó SQL Server áttekintése |} Microsoft Azure"
    description="További tudnivalók a Azure virtuális gépeken futó SQL Server-kiadásai teljes olvashat. Az SQL Server virtuális képek és a kapcsolódó tartalom közvetlen hivatkozásainak beolvasása"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/19/2016"
    ms.author="jroth"/>

# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Azure virtuális gépeken futó SQL Server – áttekintés

Ez a témakör az SQL Server Azure virtuális gépeken futó VMs szolgáltatást futtató beállításainak [portál képek mutató hivatkozások](#option-1-create-a-sql-vm-with-per-minute-licensing) és a [Gyakori feladatok](#manage-your-sql-vm)áttekintése.

>[AZURE.NOTE] Ha már régóta SQL Server, és egyszerűen szeretné, hogy egy SQL Server virtuális telepítéséről, olvassa el a [rendelkezni az SQL Server virtuális gép az Azure-portálon](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="overview"></a>– Áttekintés
Ha egy adatbázis-adminisztrátorhoz vagy egy fejlesztő, Azure VMs a helyszíni SQL Server-munkaterhelésekből, és a felhőbe alkalmazások ad lehetőséget. Az alábbi videóban áttekintést kaphat technikai SQL Server Azure VMs.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

A videó bemutatja az alábbi kérdésekre:

|Idő|Terület|
|---|---|
| 00:21 | Mik azok a Azure VMs? |
| 01:45 | Biztonsági |
| 02:50 | Kapcsolat |
| 03:30 | Tárterület megbízhatóságának és teljesítményének |
| 05:20 | Virtuális méretek |
| 05:54 | Magas rendelkezésre állásának és SLA |
| 07:30 | Konfigurációs támogatás |
| 08:00 | Figyelése |
| 08:32 | Bemutató: Hozzon létre egy SQL Server 2016 virtuális |

>[AZURE.NOTE] A videó az SQL Server 2016 koncentrál, de az Azure virtuális képek nyújt az SQL Server 2008, 2012, 2014-es és 2016 korábbi verziói. 

## <a name="scenarios"></a>Felhasználási területei
Számos oka választhatja az Azure-ban adatok tárolni. Ha az alkalmazás áthelyezi az Azure, javítja a teljesítmény is helyezze át az adatokat. De más előnyökkel. Automatikusan több adatközpontokban globális jelenléti adatok és katasztrófa helyreállítás hozzáférése van. Az adatok egyben erősen védett és tartós.

Azure VMs futó SQL Server Azure-ban a relációs adatok tárolására egy beállítás. Még több esetek kinek ajánljuk. Érdemes lehet például egy helyszíni SQL Server gépi a lehető hasonlóképpen konfigurálása a Azure virtuális. Vagy lehet, hogy ugyanazt az adatbázis-kiszolgálón további alkalmazások és szolgáltatások futtatásához. Van két fő források, amelyek gondolja át még több esetek és előtt megfontolandó kérdések segíthetnek:

 - [Azure virtuális gépeken futó SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/) áttekintést nyújt a SQL Server Azure VMs a legjobb esetet tárgyal. 
 - [Válassza az SQL Server lehetőség felhő: (PaaS) Azure SQL-adatbázis vagy SQL Server Azure VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md) tartalmaz egy SQL-adatbázis és az SQL Server egy virtuális futó részletes összehasonlítását.

## <a name="create-a-new-sql-vm"></a>Hozzon létre egy új SQL virtuális
Az alábbi szakaszok, amely közvetlenül az Azure-portálra az SQL Server virtuális gép gyűjtemény képekhez. Attól függően, hogy a kép lehetőséget választja perc alapon költségek licencelési SQL Server vagy fizetési lehetséges, vagy áttelepítheti a saját licenc (BYOL).

A folyamat részletes útmutató található az oktatóanyagot, [rendelkezni az SQL Server virtuális gép az Azure-portálon](virtual-machines-windows-portal-sql-server-provision.md). Is tekintse át a [Teljesítmény gyakorlati tanácsok az SQL Server VMs](virtual-machines-windows-sql-performance.md), amely bemutatja, hogyan jelölje ki a megfelelő gépi méretét és egyéb kiépítési alatt elérhető szolgáltatások.

## <a name="option-1-create-a-sql-vm-with-per-minute-licensing"></a>1 beállítást: Hozzon létre egy SQL virtuális perc licencelés
Az alábbi táblázat ismerteti a virtuális gépek galéria elérhető SQL Server képek mátrixok. Kattintson a hivatkozásokra kattintva hoz létre egy új SQL virtuális a megadott verziót, a edition és az operációs rendszer megkezdéséhez.

|Verzió|Operációs rendszer|Edition|
|---|---|---|
|**SQL 2016-BAN**|A Windows Server 2012 R2|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [normál](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [fejlesztők](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL-2014-ES SP1**|A Windows Server 2012 R2|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [szabványos](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2) [webes](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL-2014-ES**|A Windows Server 2012 R2|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [szabványos](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2) [webes](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|A Windows Server 2012 R2|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [szabványos](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2) [webes](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|A Windows Server 2012 R2|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [szabványos](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2) [webes](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|A Windows Server 2012|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [szabványos](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012) [webes](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012) [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**AZ SQL 2008 R2 SP3**|A Windows Server 2008 R2 rendszerben|[Vállalati](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [szabványos](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2) [webes](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**AZ SQL 2008 R2 SP3**|A Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## <a name="option-2-create-a-sql-vm-with-an-existing-license"></a>2 lehetőség: Hozzon létre egy SQL virtuális meglévő licenccel rendelkező
Is áttelepítheti a saját licenc (BYOL). Ebben az esetben csak fizet a virtuális bármilyen további díjak az SQL Server licencelési nélkül. A saját licenc, használja a mátrix az SQL Server verziói, kiadásai és az alábbi operációs rendszerek. A kép nevek a portálon, a **{BYOL}**elé.

|Verzió|Operációs rendszer|Edition|
|---|---|---|
|**Az SQL Server 2016-ban**|A Windows Server 2012 R2|[Vállalati BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [szabványos BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014-es SP1**|A Windows Server 2012 R2|[Vállalati BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [szabványos BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**Az SQL Server 2012 SP2**|A Windows Server 2012 R2|[Vállalati BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [szabványos BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

> [AZURE.IMPORTANT] BYOL virtuális képek használatához rendelkeznie kell és az Enterprise Agreement [licenc](https://azure.microsoft.com/pricing/license-mobility/)mozgásukban Azure a frissítési garancia keresztül. A használni kívánt SQL Server verzió/edition érvényes licenc is szükséges. [Adja meg a szükséges BYOL adatokat a Microsoftnak](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf) be kell a virtuális kiépítési **10** napon belül.

## <a name="manage-your-sql-vm"></a>Az SQL-virtuális kezelése
Az SQL Server virtuális-kiépítése után vannak több választható engedélykezelési feladatai. Több szempontból akkor konfigurálása és kezelése az SQL Server pontosan, például a kezelné a egy helyszíni SQL Server-példányt. Néhány műveletet azonban olyan Azure-specifikus. Az alábbi szakaszok kiemelése néhány további információra mutató hivatkozásokat tartalmazó ezekre a területekre.

### <a name="connect-to-the-vm"></a>A virtuális csatlakoztatása
A legtöbb kezelési alaplépései egyik az SQL Server virtuális eszközök, például SQL Server Management Studio (SSMS) keresztül csatlakozhat. Kapcsolatos tudnivalókat az új SQL Server virtuális csatlakozni olvassa el a [Csatlakozás az SQL Server virtuális gép a Azure](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Az adatok áttelepítése
Ha egy meglévő adatbázishoz, bizonyára tudni szeretné áthelyezni, amely a újonnan kiépített SQL virtuális. Az áttelepítési beállítások és útmutatás, című témakör [az SQL Server-Azure virtuális adatbázis áttelepítése](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Magas elérhetőségének konfigurálása
Ha magas elérhetőség van szüksége, fontolja meg, hogy az SQL Server rendelkezésre állási csoportok. Ez a virtuális hálózatban több Azure VMs jár. Az Azure portál tartalmaz egy sablont, amely meg a fenti konfiguráció beállítása. További tudnivalókért lásd: a [Configure egy AlwaysOn rendelkezésre állási csoport erőforrás-kezelő Azure virtuális gépeken futó](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Ha szeretné kézzel adja meg a rendelkezésre állási csoport és a kapcsolódó figyelő, [AlwaysOn elérhetősége csoport konfigurálása az Azure virtuális](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)megjelenítéséhez.

Más magas elérhetősége szempontok olvassa el a [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md)című témakört.

### <a name="back-up-your-data"></a>Biztonsági másolatok készítése
Azure VMs kihasználhatja [Az automatikus biztonsági mentés](virtual-machines-windows-sql-automated-backup.md), amely rendszeresen hoz létre blob-tárolóhoz az adatbázis biztonsági másolatait. Ez a módszer manuálisan is használhatja. További információ a [Használata Azure tárhely az SQL Server biztonsági mentése és visszaállítása](virtual-machines-windows-use-storage-sql-server-backup-restore.md)című témakör. Minden biztonsági mentési és visszaállítási beállítások áttekintése olvassa el a [biztonsági mentése és visszaállítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-backup-recovery.md)című témakört.

### <a name="automate-updates"></a>Frissítések automatizálása
Azure VMs is ütemezéséhez használja az [Automatikus javítási](virtual-machines-windows-sql-automated-patching.md) karbantartási az ablak fontos windows telepítése, és az SQL Server automatikusan frissül.

### <a name="customer-experience-improvement-program-ceip"></a>Felhasználói élmény fokozása programjában
A felhasználói élmény Program fokozása alapértelmezés szerint engedélyezve van. Ez rendszeres küld a jelentések Microsoft SQL Server javítása érdekében. Nem kötelező a felhasználói élmény fokozása program, kivéve, ha le szeretné tiltani azt követően, hogy kiépítési management feladat nem. Testre szabhatja, vagy a felhasználói élmény fokozása program letiltása a csatlakozás a virtuális a távoli asztal. Futtassa a **Hiba az SQL Server és a használati jelentéskészítés** segédprogramot. Kövesse az utasításokat követve tiltsa le a reporting. 

További tudnivalókért olvassa el a a a [Licencfeltételek elfogadása](https://msdn.microsoft.com/library/ms143343.aspx) témakör felhasználói élmény fokozása program szakaszát. 

## <a name="next-steps"></a>Következő lépések
[Tallózás a tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) az SQL Server Azure virtuális gépeken.

További kérdés? Első lépésként lásd: az [SQL Server Azure virtuális gépeken futó – gyakori kérdések](virtual-machines-windows-sql-server-iaas-faq.md). De is hozzáadhat a kérdéseiket vagy megjegyzéseiket kommunikálni a Microsoft és a Közösség bármelyik SQL virtuális témakörök aljára.
