<properties
    pageTitle="Az SQL Server virtuális gép kiépítése |} Microsoft Azure"
    description="Hozzon létre, és Csatlakozás SQL Server virtuális géphez a portálon Azure-ban. Ebben az oktatóprogramban az erőforrás-kezelő módban használja."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    editor=""
    manager="jhubbard"
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/21/2016"
    ms.author="jroth" />

# <a name="provision-a-sql-server-virtual-machine-in-the-azure-portal"></a>Az SQL Server virtuális gép az Azure-portálon kiépítése

> [AZURE.SELECTOR]
- [Portál](virtual-machines-windows-portal-sql-server-provision.md)
- [A PowerShell](virtual-machines-windows-ps-sql-create.md)

Ebben az oktatóanyagban végpontok közötti bemutatja, hogyan használni az Azure-portált SQL Servert futtató virtuális gép hozhatók létre.

Az Azure virtuális gép (virtuális) gyűjteménye, amely tartalmazza a Microsoft SQL Server több kép tartalmazza. Néhány kattintással válassza ki a gyűjteményből az SQL-virtuális képek közül, és kiépítése azt az Azure környezetben.

Ebben az oktatóanyagban lesz:

- [Válassza ki a gyűjteményből egy SQL virtuális képe](#select-a-sql-vm-image-from-the-gallery)
- [Állítsa be, és a virtuális létrehozása](#configure-the-vm)
- [Nyissa meg a virtuális távoli asztali](#open-the-vm-with-remote-desktop)
- [Csatlakozás SQL Server távolról](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-the-gallery"></a>Válassza ki a gyűjteményből egy SQL virtuális képe

1. Az [Azure portált](https://portal.azure.com) a fiók használatával jelentkezzen be.

    >[AZURE.NOTE] Azure fiókja nem rendelkezik, látogasson el a [Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

1. Az Azure portálon kattintson az **Új**gombra. A portál az **Új** lap nyílik meg. Az SQL Server virtuális források állnak, a piactér **virtuális gépeken futó** csoportjában található.

1. Kattintson az **Új** lap **virtuális gépeken futó**.

1. Az összes elérhető képek megtekintéséhez kattintson a **virtuális gépeken futó** lap a **látható a teljes** gombra.

    ![Azure virtuális gépeken futó lap](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade.png)

1. **Adatbázis-kiszolgálók**csoportban kattintson az **SQL Server**parancsra. Előfordulhat, hogy csak görgetéssel keresse meg az **adatbázis-kiszolgálók**. Tekintse át a rendelkezésre álló SQL Server-sablonok.

    ![SQL-képek virtuális gépi gyűjtemény](./media/virtual-machines-windows-portal-sql-server-provision/virtual-machine-gallery-sql-server.png)

1. Minden sablon egy SQL Server-verzióra, és az operációs rendszer azonosítja. Válasszon egy ezeket a képeket a listából. Tekintse át a Részletek lap, amely a virtuális gép kép leírását tartalmazza.

    >[AZURE.NOTE] SQL-virtuális képek az SQL Server licencelési költségek Belefoglalás a hoz létre virtuális perc árak. Van más lehetőség hozása-a-saját-licenc (BYOL) és a fizetési csak a virtuális. Kép neveket a {BYOL} elé. Ezt a beállítást a további tudnivalókért olvassa el [a Azure virtuális gépeken futó SQL Server – első lépések](virtual-machines-windows-sql-server-iaas-overview.md)című témakört.

1. Csoportban **Válasszon ki egy telepítési modellt**ellenőrizze, hogy be van jelölve **Az erőforrás-kezelő** . Erőforrás-kezelő az új virtuális gépeken futó a javasolt telepítési modellt. Kattintson a **létrehozása**gombra.

    ![SQL-virtuális az erőforrás-kezelő létrehozása](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-the-vm"></a>A virtuális konfigurálása
Vannak olyan konfigurálása az SQL Server virtuális gép öt fűrészlap.

| Lépés               | Leírás                          |
|---------------------|-------------------------------|
| **Alapjai**              | [Egyszerű beállításainak konfigurálása](#1-configure-basic-settings)      |
| **Mérete**                | [Válassza ki a virtuális gép mérete](#2-choose-virtual-machine-size)   |
| **Beállítások**            | [Választható szolgáltatások konfigurálása](#3-configure-optional-features)   |
| **Az SQL Server-beállítások** | [SQL server-beállításainak konfigurálása](#4-configure-sql-server-settings) |
| **Összefoglalás**             | [Tekintse át az összefoglaló](#5-review-the-summary)            |

## <a name="1-configure-basic-settings"></a>1. a egyszerű beállításainak konfigurálása
Az **alapvető tudnivalók** lap adja meg a következő adatokat:

* Írja be egy egyedi virtuális számítógép **nevét**.
* Adja meg a helyi rendszergazdafiók **felhasználó nevét** a virtuális a. Ehhez a fiókhoz is hozzáadódik az SQL Server- **rendszergazdák** rögzített kiszolgálói szerepkör.
* Erős **jelszó**megadását.
* Ha több előfizetéssel rendelkezik, ellenőrizze, hogy helyesek-e az előfizetés az új virtuális.
* **Erőforráscsoport** mezőbe írja be az új erőforráscsoport nevét. Azt is megteheti használja a meglévő erőforrás csoport kattintva **Jelölje ki a meglévő**. Erőforráscsoport (virtuális gépeken futó, tároló fiókok, virtuális hálózatok stb.) Azure-ban a kapcsolódó források gyűjteménye.

    >[AZURE.NOTE] Új erőforráscsoport használata hasznos, ha csak tesztelés vagy információk az SQL Server-telepítések Azure-ban. Miután végzett a próbához, törölje az erőforráscsoport automatikusan törli a virtuális és erőforrás csoporthoz rendelt összes erőforrás. Többet szeretne tudni az erőforrás csoportok [Azure erőforrás-kezelő áttekintése](../azure-resource-manager/resource-group-overview.md)című témakörben találhat.

* Válasszon egy **helyet** a telepítéshez.
* Kattintson az **OK gombra** a beállítások mentéséhez.

    ![SQL alapvető tudnivalók lap](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. Válassza ki a virtuális gép mérete
A **méret** lapon válassza a **Válassza ki a méretet** a lap egy virtuális gép méretet. A lap először a kijelölt sablonon alapuló ajánlott gépi méretét jeleníti meg. A becslést ad a virtuális futtatásához a havi költséget is.

![SQL-virtuális méret beállításai](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

Gyártási munkaterhelésekből, a Másolás beállítást javasoljuk egy virtuális gép mérete, amely támogatja a [Prémium tároló](../storage/storage-premium-storage.md). Ha nem kér a teljesítmény szintnek, használja az **összes megjelenítése** gombra, amely mutatja az összes gépi méret beállításai. Gépi kisebb méretű használható például fejlesztési vagy tesztkörnyezetben.

>[AZURE.NOTE] További információt a virtuális gép méretű lásd: [virtuális gépeken futó méretét](virtual-machines-windows-sizes.md). Az SQL Server virtuális méretű kapcsolatos szempontok olvassa el a [Teljesítmény gyakorlati tanácsok az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-performance.md)című témakört.

Válassza ki a gép méretét, és kattintson a **Jelölje ki**.

## <a name="3-configure-optional-features"></a>3. a választható szolgáltatások konfigurálása
Kattintson a **Beállítások** lap állítsa be az Azure tárhelyek – a hálózat, valamint a virtuális gép figyelése.

- **Tárhely**adja meg azt a **lemez írja be** a normál vagy a prémium (SSD). Prémium tároló gyártási munkaterhelésekből ajánlott.

>[AZURE.NOTE] A gép méret, amely nem támogatja a prémium tároló prémium (SSD) választja, a gép mérete automatikusan módosul.  

- **Tárterület-fiók**csoportjában fogadja el a automatikusan kiépített tároló fiók nevére. A **tárterület-fiók** válasszon ki egy meglévő fiókot, és állítsa be a tárhely fióktípus is kattinthat. Alapértelmezés szerint a Azure helyileg felesleges adathordozós létrehoz egy új tárterület-fiókot. A fájltárolási lehetőségekkel kapcsolatos további tudnivalókért lásd: [Azure tároló replikáció](../storage/storage-redundancy.md).

- A **hálózat**fogadja el az automatikusan feltöltött értékeket. A kézi beállításához a **virtuális hálózati**, **alhálózat**, **nyilvános IP-cím**vagy **Hálózati biztonsági csoport**szolgáltatások is kattinthat. Ebben az oktatóanyagban alkalmazásában tartsa szem előtt az alapértelmezett értékeket.

- Azure lehetővé teszi, hogy **Figyelés** alapértelmezés szerint a virtuális kijelölt ugyanazzal tárhely a fiókkal. Az alábbi beállítások módosíthatók.

- **Elérhetőség beállítása**adja meg azt az elérhetőség beállítása. Ebben az oktatóanyagban alkalmazásában, válassza a **Nincs beállítást**. Ha SQL AlwaysOn elérhetősége csoportokat állíthat be, a virtuális gép kiindulópontként elkerülése érdekében elérhetőségének beállítása.  További információ a [virtuális gépeken futó elérhetőségének kezelése](virtual-machines-windows-manage-availability.md)című cikkben talál.

Amikor befejezte a beállítások konfigurálásával, kattintson **az OK gombra**.

## <a name="4-configure-sql-server-settings"></a>4. SQL server-beállításainak konfigurálása
Az **SQL Server-beállítások** lap a konfigurálása a speciális beállításokkal és az SQL Server optimalizálásokat. A beállításokat, amelyekkel az SQL Server beállíthatja a következők lehetnek.

| A beállítás               |
|---------------------|
| [Kapcsolat](#connectivity)              |
| [Hitelesítés](#authentication)                |
| [Tárterület konfigurálása](#storage-configuration)            |
| [Az automatikus javítás](#automated-patching) |
| [Az automatikus biztonsági mentése](#automated-backup)             |
| [Azure kulcs tárolóra-integráció](#azure-key-vault-integration)             |
| [R-szolgáltatások](#r-services) |

### <a name="connectivity"></a>Kapcsolat
Az **SQL-kapcsolat**szakaszban adja meg az SQL Server-példányt a virtuális a kívánt hozzáférés típusát. Ebben az oktatóanyagban alkalmazásában jelölje be a **nyilvános (internet)** kezdeményezett kapcsolatok SQL Server gépek vagy internetes szolgáltatások. Ezt a lehetőséget választja, az Azure automatikusan állítja be a tűzfalat, és a hálózati biztonsági csoport port 1433 forgalmának engedélyezésére.  

![SQL-csatlakozási beállítások](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

Az SQL Server az interneten keresztül csatlakozhat is engedélyeznie kell az SQL Server-hitelesítés, a következő szakaszban ismertetett.

>[AZURE.NOTE] A hálózati kommunikáció további korlátozások hozzáadása az SQL Server virtuális lehetőség. Ehhez a hálózati biztonsági csoport szerkesztése, a virtuális létrehozását követően. További tudnivalókért lásd: [egy hálózati biztonsági csoport (NSG) tartalma?](../virtual-network/virtual-networks-nsg.md)

Ha inkább nem engedélyezte a kapcsolatokat az interneten keresztül a adatbázismotor, az alábbi lehetőségek közül választhat:

- **Helyi (belül csak virtuális)** lehetővé teszi a virtuális belül csak az SQL Server-kapcsolatot.
- A **magánjellegű (ezek olyan virtuális hálózatból)** kapcsolatok gépek vagy a virtuális ugyanabba a hálózatba szolgáltatásai lehetővé teszi az SQL Server.

>[AZURE.NOTE] Az SQL Server Express edition virtuális gép elem képével automatikusan nem engedélyezi a TCP/IP protokollt. Az IGAZ, ha a nyilvános és magán csatlakozási beállítások az. Express Edition kell használnia az SQL Server Configuration Manager ahhoz, hogy [manuálisan a TCP/IP protokollt](#configure-sql-server-to-listen-on-the-tcp-protocol) a virtuális létrehozása után.

Az általános növelheti biztonsági kiválasztása a legszigorúbb kapcsolatot, amely lehetővé teszi, hogy Ön esetében. De összes beállítást biztonságos hálózati biztonsági csoport szabályok és SQL vagy Windows-hitelesítés keresztül.

**Port** 1433 alapértelmezés szerint. Megadhatja, hogy egy másik portszámot.
További tudnivalókért lásd: [csatlakoztatása az SQL Server virtuális gép (erőforrás-kezelő) |} Microsoft Azure](virtual-machines-windows-sql-connect.md).

### <a name="authentication"></a>Hitelesítés
Ha az SQL Server-hitelesítés van szüksége, kattintson a **SQL**-hitelesítés **engedélyezése** parancsára.

![SQL Server-hitelesítés](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

>[AZURE.NOTE] Ha szeretne elérni az SQL Server (azaz a nyilvános kapcsolódási lehetőség) az interneten, engedélyeznie kell az alábbi SQL-hitelesítést. Nyilvános hozzáférés az SQL Server SQL-hitelesítést igényel.

Ha bekapcsolja az SQL Server-hitelesítés, adja meg a **bejelentkezési nevét** és **jelszavát**. A felhasználó nevét, mely SQL Server-hitelesítés bejelentkezési és a **rendszergazdák** kiszolgálói szerepkör rögzített tagja van beállítva. További információt a hitelesítési mód [kiválasztása egy hitelesítési mód](http://msdn.microsoft.com/library/ms144284.aspx) című szakaszában.

Ha nem engedélyezi az SQL Server-hitelesítés, majd felhasználhatja a helyi rendszergazdafiók a virtuális csatlakozhat az SQL Server-példányt.

### <a name="storage-configuration"></a>Tárterület konfigurálása
Kattintson a **tárolás konfigurációs** adja meg a tárhelyre parancsra.

![SQL-tároló beállítások](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

>[AZURE.NOTE] Ha bejelöli a szabványos tárhely, ez a beállítás nem érhető el. Automatikus tároló optimalizálási csak prémium tároló érhető el.

Megadhatja a követelmények bemeneti és kimeneti műveletként másodpercenként (IOPs), a MB/s és a teljes tárterület méretének kapacitása. Állítsa be ezeket az értékeket a csúszó értékekhez használatával. A portál automatikusan ezeknek a követelményeknek alapján lemez számát számítja ki.

Alapértelmezés szerint Azure optimalizálja a tárhely 5000 IOPs, 200 MB és 1 TB-nyi rendelkezésre álló tárterület méretének. A terhelést alapján tároló beállítások módosíthatók. A **tároló optimalizálva**jelölje be az alábbi lehetőségek közül:

- **Általános** az alapértelmezett beállítás, és a legtöbb munkaterhelésekből támogatja.
- **Tranzakció alapú** feldolgozás optimalizálja a hagyományos adatbázis OLTP munkaterhelésekből tárhelyet.
- Elemzési és jelentéskészítési munkaterhelésekből tárolt **adatok raktározással** optimalizálja.

>[AZURE.NOTE] A csúszkák a felső határa a kijelölt virtuális gép méret függően változnak.

### <a name="automated-patching"></a>Az automatikus javítás
**Az automatikus javítási** alapértelmezés szerint engedélyezve van. Az automatikus javítási lehetővé teszi, hogy az automatikus javítás az SQL Server- és az operációs rendszer Azure. Adjon meg egy nap, hét, időt és a karbantartás ablak időtartam. Azure hajt végre, az ablak karbantartási javítása. A karbantartási ablak ütemezése idő a virtuális területi beállításait használja. Ha nem szeretné, hogy az automatikus javítás az SQL Server- és az operációs rendszer Azure, kattintson a **letiltása**lehetőséget.  

![SQL automatikus javítása](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

További tudnivalókért olvassa el a [Automatikus javítása az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-automated-patching.md)című témakört.

### <a name="automated-backup"></a>Az automatikus biztonsági mentése
Adatbázis automatikus biztonsági mentés csoportban **az automatikus biztonsági mentés**adatbázisokra engedélyezése. Az automatikus biztonsági mentés alapértelmezés szerint nincs engedélyezve.

Ha engedélyezi az SQL automatikus biztonsági másolat, adhatja meg a következőket:

- Adatmegőrzés időtartama (nap) biztonsági másolatok
- Biztonsági másolatok tároló fiókot
- Titkosítási beállítás és a biztonsági másolatok jelszó

Ha titkosítani szeretné a biztonsági mentés, kattintson az **Engedélyezés**parancsra. Ezután adja meg a **jelszót**. Azure létrehozza a biztonsági másolatok titkosítása tanúsítvány, és a megadott jelszót használja a tanúsítvány védelmét.

![Az automatikus biztonsági mentés SQL](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup.png)

 További tudnivalókért lásd: az [Automatikus biztonsági mentés az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-automated-backup.md).

### <a name="azure-key-vault-integration"></a>Azure kulcs tárolóra-integráció
Biztonsági titkos kulcsok Azure-ban a titkosítási tárolhat, kattintson a **integrációs Azure kulcs tárolóból elemre** , és kattintson az **Engedélyezés**gombra.

![Az SQL Azure kulcs tárolóra integrációja](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

Az alábbi táblázat a paraméterek Azure kulcs tárolóra integráció szükséges.

|PARAMÉTER|LEÍRÁS|PÉLDA|
|----------|----------|-------|
|**Fő tárolóra URL-címe** |A fő tárolóra helyét.|https://contosokeyvault.vault.Azure.NET/ |
|**Egyszerű felhasználónév** |Azure Active Directory egyszerű szolgáltatásnévvel. Ez a név is nevezik az ügyfél-azonosító  |5 – 4e11-af04eb07b669ccf2 fde2b411 - 33d|
| **Egyszerű titkos kulcs**|Azure Active Directory szolgáltatás fő titkos. A titkos is nevezik az ügyfél titkos. | 9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM =|
|**Hitelesítő adatok neve**|**Hitelesítő adatok neve**: AKV Integration létrehoz egy hitelesítő belül az SQL Server lehetővé teszi a virtuális hozzáférjen a fő tárolóból elemre. Válasszon egy nevet a hitelesítő adatok.| mycred1|

További tudnivalókért lásd: [Konfigurálása Azure kulcs tárolóból elemre integráció az SQL Server Azure VMs](virtual-machines-windows-ps-sql-keyvault.md).

Ha befejezte az SQL Server-beállításokkal kapcsolatosan, kattintson az **OK gombra**.

### <a name="r-services"></a>R-szolgáltatások
SQL Server 2016 Enterprise Edition Ha [Az SQL Server-R Services](https://msdn.microsoft.com/library/mt604845.aspx)engedélyezése lehetőséget. Ez lehetővé teszi az SQL Server 2016 fejlett analitikai használja. Kattintson a **engedélyezése** a az **SQL Server-beállítások** lap.

![Az SQL Server R Services engedélyezése](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

>[AZURE.NOTE] Az SQL Server képek, amelyek nem 2016 Enterprise edition R szolgáltatás lehetővé teszi le van tiltva.

## <a name="5-review-the-summary"></a>5. az összefoglaló áttekintése
A **összesítő** lap tekintse át az összefoglaló, és kattintson az **OK gombra** az SQL Server, erőforráscsoport és a virtuális megadott erőforrások létrehozása.

A telepítési az azure portálról figyelheti meg. Az **értesítések** gombra a képernyő tetején a telepítés egyszerű állapotot mutatja.

>[AZURE.NOTE] Hogy biztosítson arról a telepítési időpontok, e telepítését egy SQL virtuális kelet-amerikai régió alapértelmezett beállításokkal. A próba-telepítés 26 percig összesen került. De esetleg tapasztalható egy gyorsabban vagy lassabban telepítési idő a régió alapján, és a kiválasztott beállítások.

## <a name="open-the-vm-with-remote-desktop"></a>Nyissa meg a virtuális távoli asztali

Csatlakozás távoli asztali virtuális készülék kövesse az alábbi lépéseket:

1. Miután elkészült a Azure virtuális, a virtuális ikon megjelenik az Azure irányítópult. Is megtalálja a meglévő virtuális gépeken futó között. Kattintson az új SQL virtuális gépen. A **virtuális gép** lap a virtuális gép részleteinek megjelenítése.
1. A **virtuális gép** a lap tetején kattintson a **Csatlakozás**gombra.
1. A böngészőben a virtuális tölthető le egy RDP-fájlt. Nyissa meg a RDP-fájlt.
    ![Távoli asztal SQL virtuális](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-remote-desktop.png)
1. A távoli asztali kapcsolat figyelmeztet, hogy a távoli kapcsolat a publisher nem azonosítania kell magát. Kattintson a **Csatlakozás** továbbra is.
1. A **Windows biztonsági** párbeszédpanelen jelölje be a **másik fiók használata**választógombot.
1. **Felhasználónév** típus ** \<felhasználónév >**, ahol <user name> a felhasználónév, amikor úgy állította be, a virtuális megadott. Akkor adja hozzá az eredeti fordított perjel a neve előtt.
1. Írja be a virtuális korábban beállított **jelszót** , és kattintson az **OK gombra** .
1. Ha egy másik **Távoli asztali kapcsolat** párbeszédpanel kéri, hogy csatlakozzon-e, kattintson az **Igen**gombra.

Miután csatlakozott az SQL Server virtuális géphez, indítsa el az SQL Server Management Studio, és kapcsolatba lépni a Windows-hitelesítés, a helyi rendszergazdai hitelesítő adataival. Ha engedélyezte az SQL Server-hitelesítés, is csatlakozhat, SQL-hitelesítéssel az SQL bejelentkezés, és a jelszavát, során kiépítési konfigurált.

A számítógéphez hozzáférést lehetővé teszi, hogy közvetlenül a számítógép és a saját igényeknek megfelelően alakítható az SQL Server-beállítások módosítása. Például sikerült a tűzfal beállításainak vagy SQL Server-beállítások módosítása.

## <a name="connect-to-sql-server-remotely"></a>Csatlakozás SQL Server távolról

Ebben az oktatóanyagban azt kijelölve a virtuális gép és az **SQL Server-hitelesítés** **nyilvános** elérése. Ezek a beállítások automatikusan konfigurálja a virtuális gép SQL Server-kapcsolatok engedélyezésére bármely ügyféltől az interneten (feltételezve, hogy a helyes SQL bejelentkezési rendelkeznek).

>[AZURE.NOTE] Ha nem jelölte be nyilvános kiépítési alatt, majd további lépéseket eléréséhez szükséges az SQL Server-példányt az interneten keresztül. További tudnivalókért olvassa el a [Csatlakozás az SQL Server virtuális géphez](virtual-machines-windows-sql-connect.md).

Az alábbi szakaszok bemutatják, hogyan csatlakozhat az SQL Server-példányt, kattintson a virtuális egy másik számítógépről az interneten keresztül.

> [AZURE.INCLUDE [Connect to SQL Server in a VM Resource Manager](../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Következő lépések
További információt az SQL Server Azure-ban való használatáról című témakörben talál [A Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-server-iaas-overview.md) és a [Gyakori kérdésekre](virtual-machines-windows-sql-server-iaas-faq.md).

A Azure virtuális gépeken futó SQL Server videó áttekintéséhez nézze meg [Azure virtuális az SQL Server 2016 a legjobb platform](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016).

[Tallózás a tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) az SQL Server Azure virtuális gépeken.
