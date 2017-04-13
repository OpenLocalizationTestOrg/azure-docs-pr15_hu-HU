<properties
    pageTitle="Mindig a elérhetősége csoport konfigurálása az Azure virtuális automatikusan - erőforrás-kezelő"
    description="Mindig a elérhetősége csoport létrehozása az Azure virtuális gépeken futó Azure erőforrás-kezelő módban. Ebben az oktatóanyagban elsősorban a felhasználói felület alapján automatikusan létrehozza a teljes megoldás."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="10/20/2016"
    ms.author="MikeRayMSFT" />

# <a name="configure-always-on-availability-group-in-azure-vm-automatically---resource-manager"></a>Mindig a elérhetősége csoport konfigurálása az Azure virtuális automatikusan - erőforrás-kezelő

> [AZURE.SELECTOR]
- [Erőforrás-kezelő: sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md)
- [Erőforrás-kezelő: kézi](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)
- [Klasszikus: felhasználói felület](virtual-machines-windows-classic-portal-sql-alwayson-availability-groups.md)
- [Klasszikus: PowerShell](virtual-machines-windows-classic-ps-sql-alwayson-availability-groups.md)

<br/>

Ebben az oktatóanyagban végpontok közötti bemutatja, hogyan az erőforrás-kezelő Azure virtuális gépeken futó SQL Server elérhetőség csoport létrehozásához. Az oktatóprogram Azure pengéit állítson be egy sablont. Használni az alapértelmezett beállítások áttekintése, írja be a kívánt beállításokat, és frissítése a portálon rögzítéséhez, ahogy azt ismerteti, ebben az oktatóanyagban.

Az oktatóanyag végén az SQL Server elérhetősége csoport megoldás Azure-ban a következő elemekből áll:

- Több alhálózat, beleértve a előtér és a háttér-alhálózat tartalmazó virtuális hálózaton

- Két tartományvezérlőnek Active Directory (AD) tartománnyal

- Két SQL Server VMs a háttéradatbázist az alhálózathoz rendszerbe, és az Active Directory tartományhoz

- A 3-csomópont WSFC fürtre csomópont többsége kvórum modell

- Az elérhetőség adatbázis két szinkron jóváhagyás kópiákkal egy rendelkezésre állási csoport

Az alábbi ábrán a megoldást grafikus ábrázolása.

![Teszt labor architektúra AG Azure-ban](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/0-EndstateSample.png)

Egységes erőforrás csoport összes a megoldás az erőforrás tartozik.

Ebben az oktatóanyagban feltételezi, hogy a következőket:

- Már rendelkezik egy Azure-fiók. Ha nem rendelkezik egy, a [próbaverzió fiókot regisztrálni](http://azure.microsoft.com/pricing/free-trial/).

- Már tudja kiépítését egy SQL Server virtuális a felhasználói felülettel a virtuális gép gyűjteményből. További tudnivalókért lásd: a [kiépítési egy SQL Server Azure virtuális gép](virtual-machines-windows-portal-sql-server-provision.md)

- Már rendelkezik egy rendelkezésre állási csoportok tömör megértése. További tudnivalókért lásd: a [mindig a elérhetősége-csoportok (SQL Server)](http://msdn.microsoft.com/library/hh510230.aspx).

>[AZURE.NOTE] Ha érdeklik a elérhetőség csoportok használatával a SharePoint, is megjelenik a [állítsa be az SQL Server 2012 mindig a elérhetőség csoportok a SharePoint 2013-hoz](http://technet.microsoft.com/library/jj715261.aspx).

Ebben az oktatóprogramban az Azure portal használandó:

- Jelölje ki a a mindig a sablon a portálon

- A sablon beállítások áttekintése és környezetben néhány konfigurációs beállítások frissítése

- Ahogy a monitor Azure hoz létre, a teljes környezetben

- A tartomány vezérlőket, majd az SQL-kiszolgálók egyik csatlakoztatása

[AZURE.INCLUDE [availability-group-template](../../includes/virtual-machines-windows-portal-sql-alwayson-ag-template.md)]


## <a name="provision-the-cluster-from-the-gallery"></a>A gyűjteményből az fürt kiépítése

Azure biztosít a teljes megoldás olyan gyűjteménye. Annak érdekében, hogy keresse meg a sablon:

1.  Az Azure-portálra a fiók használatával jelentkezzen be.
1.  Az Azure portál kattintásra **+ Új.** A portál az új lap nyílik meg.
1.  Kattintson az új lap keresése **AlwaysOn**.
![Keresse meg azt a sablont AlwaysOn](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/16-findalwayson.png)
1.  Keresse meg **Az SQL Server AlwaysOn fürt**a keresési eredmények között.
![AlwaysOn sablon](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/17-alwaysontemplate.png)
1.  **Jelölje ki a telepítési modell** válassza **Az erőforrás-kezelő**.

### <a name="basics"></a>Alapjai

Kattintson a **alapjai** , és adja meg a következő:

- **Rendszergazda** felhasználóneve mindkét esetben az SQL Server egy felhasználói fiókot a tartomány rendszergazdai jogosultságok és az SQL Server-rendszergazdák rögzített kiszolgálói szerepkör tagja. Ebben az oktatóanyagban **DomainAdminPassword**használja.

- **Jelszó** megadása a tartomány rendszergazdai fiók jelszava. Használhat összetett jelszót. Erősítse meg a jelszót.

- **Előfizetés** az előfizetést, az Azure fog bill az elérhetőség csoport forrásokkal futtatásához a. Másik előfizetést megadhatja, ha a fiók több előfizetéssel rendelkezik.

- **Erőforráscsoport** a nevét az a csoportot, hogy az összes Azure erőforrás ebben az oktatóanyagban által létrehozott fog tartozni. Ebben az oktatóanyagban használata **SQL-HA-RG**. További tudnivalókért lásd: (erőforrás-kezelő Azure áttekintése) [erőforrás-csoport-overview.md / # erőforrás-csoportok].

- **Hely** az Azure régió, ahol ebben az oktatóprogramban az erőforrások létrejön egy. Jelöljön ki egy Azure területet az infrastruktúra tárolni.

Az alábbi képen a az **alapvető tudnivalók** lap hasonlóan fog kinézni:

![Alapjai](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/1-basics.png)

- Kattintson az **OK gombra**.

### <a name="domain-and-network-settings"></a>Tartomány és a hálózati beállítások

Ezzel a sablonnal Azure gyűjtemény új tartományt hoz létre új tartomány vezérlőkkel. Ezenkívül létrehoz egy új hálózati és két alhálózat. A sablon nem engedélyezi a kiszolgálók létrehozása egy meglévő tartomány vagy a virtuális hálózat. A következő lépésként a tartomány és a hálózat beállításainak konfigurálása.

A **tartomány és a hálózati beállítások** lap tekintse át az előre beállított értékeket a tartomány és a hálózati beállításokat:

- **Erdő legfelső szintű tartomány nevét** a tartomány nevét, az Active Directory tartomány esetén ezen fog tárolni a fürt használt. Az oktatóprogram használja a **contoso.com címen**.

- **Virtuális hálózat** neve a hálózat nevét az Azure virtuális hálózathoz. Ebben az oktatóanyagban **autohaVNET**használja.

- **Tartományvezérlőnek alhálózat** neve egy részét, amelyen a tartományvezérlőnek a virtuális hálózat nevét. Ebben az oktatóanyagban használata **alhálózat-1**. Az alhálózathoz cím előtag **10.0.0.0/24**fogja használni.

- **Az SQL Server alhálózat** neve egy részét, hogy tárolja az SQL Server és a fájl megosztása tanú a virtuális hálózat nevét. Ebben az oktatóanyagban használja a **2-alhálózat**. Az alhálózathoz cím előtag **10.0.1.0/26**fogja használni.

További tudnivalók az [Azure virtuális hálózati áttekintésben](../virtual-network/virtual-networks-overview.md)virtuális hálózatok.  

A **tartomány és a hálózati beállítások** így néz ki:

![Tartomány és a hálózati beállítások](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/2-domain.png)

Szükség esetén módosíthatja ezeket az értékeket. Ebben az oktatóanyagban fogjuk kiszámolni az előre beállított értékeket.

- Tekintse át a beállításokat, és kattintson az **OK gombra**.

###<a name="availability-group-settings"></a>elérhetőség csoport beállításai

**Elérhetőség csoport beállításaival** kapcsolatos tekintse át az elérhetőség csoport és a figyelő előre definiált értékei.

- **Availablity csoport** neve az elérhetőség csoport a csoportosított erőforrás nevét. Ebben az oktatóanyagban **Contoso-ag**használja.

- **Elérhetőség figyelő csoportnévre** a fürt és a belső terheléselosztó használják. Csatlakozás SQL Server ügyfélprogramok az ilyen nevű segítségével a megfelelő replika az adatbázis csatlakoztatása. Ebben az oktatóanyagban **Contoso-figyelő**használja.

-  **Elérhetőség csoport figyelő port** adja meg a portot az SQL Server figyelő fogja használni. Ebben az oktatóanyagban használja az alapértelmezett portja a **1433**.

Szükség esetén módosíthatja ezeket az értékeket. Ebben az oktatóanyagban használja az előre beállított értékeket.  

![elérhetőség csoport beállításai](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/3-availabilitygroup.png)

- Kattintson az **OK gombra**.

###<a name="vm-size-storage-settings"></a>Virtuális méretét, a tárhely beállításai

A **virtuális méretét, a tárhely beállításai** válassza az SQL Server virtuális gép mérete, és tekintse át az egyéb beállításokat.

- **Az SQL Server virtuális gép** mérete mindkét SQL Server Azure virtuális gép méretét. Válassza ki a virtuális gép a terhelést a megfelelő méretet. Ha ez a környezet **DS2**oktatóanyag használatra hoz létre. Gyártási munkaterhelésekből közül, amelyek támogatják a terhelést a virtuális gép méretet. Sok gyártási munkaterhelésekből esetén kell **DS4** vagy annál nagyobb. A sablon két virtuális gépeken futó ilyen méretű össze, és a SQL Server telepítése mindegyik. További tudnivalókért lásd: a [virtuális gépeken futó méretét](virtual-machines-linux-sizes.md).

>[AZURE.NOTE]Azure telepíti az SQL Server Enterprise Edition. A költség attól függ, a edition és virtuális gép méretét. Aktuális költségek kapcsolatos részletes tudnivalókért olvassa el a [virtuális gépeken futó árak](http://azure.microsoft.com/pricing/details/virtual-machines/#Sql)című témakört.

- **Tartomány vezérlő virtuális gép** mérete a tartomány vezérlők virtuális gép méretét. Ebben az oktatóanyagban **D2**használja.

- **Fájl megosztása tanú virtuális gép** mérete tanúsító a fájlmegosztás virtuális gép méretét. Ebben az oktatóanyagban **A1**használja.

- **SQL-tárolót fiók** pedig az SQL Server-adatok és az operációs rendszer lemezre tartsa tárterület-fiók neve. Ebben az oktatóanyagban **alwaysonsql01**használja.

- A tárhely fiók nevét a tartomány-vezérlők szolgáltatás **Adatközpont tárterület-fiókra** . Ebben az oktatóanyagban **alwaysondc01**használja.

- **Az SQL Server adatainak lemezre méretét** a TB mérete a TB az SQL Server-adatok lemez. Adja meg az 1-4-es szám. Ez a mérete az adatok lemez, amely az egyes SQL Server csatolni. Ebben az oktatóanyagban használja az **1**.

- **Tárterület optimalizálási** állítja be az SQL Server virtuális gépeken futó terhelést függően bizonyos tárolási beállításait. Ebben az esetben az összes SQL-kiszolgálók prémium tároló használata Azure host lemezgyorsítótár írásvédettek. Ezeken kívül az SQL Server beállításait a terhelést a tudja optimalizálni ezek a beállítások közül:

    - **Általános terhelést** állítja be a nincs speciális beállításokkal

    - Nyomkövetés-jelölő 1117 és 1118 **tranzakció alapú feldolgozása** állítja be.

    - **Adatok raktározással** állítja be a nyomkövetési jelző 1117 és 610

Ebben az oktatóanyagban **Általános terhelést**használja.

![Virtuális méret tárhely beállításai](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/4-vm.png)

- Tekintse át a beállításokat, és kattintson az **OK gombra**.

####<a name="a-note-about-storage"></a>Tárterület megjegyzést

További optimalizálást attól függenek, hogy az SQL Server-adatok lemez méretét. Minden adat lemez adjon Azure hozzáadja a-1 TB prémium tárhely (SSD). Ha egy kiszolgáló 2 TB vagy több igényel, a sablon egy tárolókészlethez minden SQL Server hoz létre. A tárhely kvótáját megjeleníthető a tárhely virtualizációs, ahol több lemezre vannak beállítva, hogy újabb beosztását, tűrőképessége és teljesítménye.  A sablon a tárhely kvótáját hoz létre a rendelkezésre álló tárterület méretének, majd az operációs rendszer egyetlen adatok jeleníti meg. A sablon jelöli meg ezt a lemezt a adatok lemez az SQL Server. A sablon a tárhely kvótáját beállítja az SQL Server az alábbi beállításokkal:

- Csíkok mérete a virtuális lemez összefűzött memóriában beállítását. A tranzakció alapú munkaterhelésekből értéke 64 KB. Adattárolási munkaterhelésekből a értéke 256 KB.

- Tűrőképessége egyszerű (tűrőképessége nem).

>[AZURE.NOTE] Azure prémium tároló helyileg redundáns, és továbbra is az adatok három példányát egy egyetlen régión belüli, így a tárhely kvótáját a további tűrőképessége nem szükséges.

- Az oszlopok száma megegyezik az lemezt a tárhely kvótáját számát.

További információ a tárterület és tárhely terület készletek talál:

- [Tárterület szóközöket áttekintése](http://technet.microsoft.com/library/hh831739.aspx).

- [Felhasználói felülete és a tárterület-készletek](http://technet.microsoft.com/library/dn390929.aspx)

Az SQL Server konfigurációs gyakorlati tanácsokat olvashat lásd: a [Teljesítmény gyakorlati tanácsok az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-performance.md)


###<a name="sql-server-settings"></a>Az SQL Server-beállítások

**Az SQL Server-beállítások** áttekintése és módosítása az SQL Server virtuális neve előtag, SQL Server-verzióra, SQL Server-szolgáltatási fiók és a jelszó és az SQL automatikus javítási karbantartási ütemezése.

- **Az SQL Server neve előtag** egy nevet az egyes SQL Server létrehozására szolgál. Ebben az oktatóanyagban **Contoso-ag**használja. Az SQL Server nevek *Contoso-ag-0* és a *Contoso-ag-1*lesz.

- **Az SQL Server-verzióra** az SQL Server verziója. Ebben az oktatóanyagban használata **SQL Server 2014-es**. **SQL Server 2012** - vagy **SQL Server 2016**is választhat.

- **Az SQL Server-szolgáltatási fiók felhasználónevét** az SQL Server szolgáltatást a tartománynév fiók. Ebben az oktatóanyagban **sqlservice**használja.

- Kell a jelszót az SQL Server-szolgáltatás fiókjához tartozó **jelszót** .  Használhat összetett jelszót. Erősítse meg a jelszót.

- **SQL-automatikus javítási karbantartási ütemezése** azonosítja a hét napjának Azure automatikus javítás a az SQL-kiszolgálók. Ebben az oktatóanyagban típusú **vasárnap**.

- **SQL automatikus javítási karbantartási indítása óra** esetén az Azure terület időpontja az automatikus javítási elindul.

>[AZURE.NOTE]Az egyes virtuális javítási ablak húzódik el egy órával. Csak egy virtuális gép egyszerre van javítással, hogy zavarok szolgáltatások.

![Az SQL Server-beállítások](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/5-sql.png)

Tekintse át a beállításokat, és kattintson az **OK gombra**.

###<a name="summary"></a>Összefoglalás

Az Összegzés lapon Azure ellenőrzi a beállításokat. Vagy töltse le a sablont. Tekintse át az összefoglalásra. Kattintson az **OK gombra**.

###<a name="buy"></a>Megvásárlása

Ez a végleges lap **használati feltételek**és **adatvédelmi**tartalmaz. Olvassa el ezt az információt. Ha készen áll a virtuális gépeken futó, és az összes egyéb szükséges erőforrások elérhetősége csoport létrehozásához Azure, kattintson a **Létrehozás**gombra.

Az Azure-portál és az összes az erőforrások erőforráscsoport hoz létre.

##<a name="monitor-deployment"></a>Telepítési monitor

A telepítés előrehaladását az Azure portálról figyelheti. A telepítési jelölő ikon automatikusan ki van emelve az Azure portál irányítópult.

![Azure irányítópult](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/11-deploydashboard.png)

##<a name="connect-to-sql-server"></a>Csatlakozás SQL Server

Az SQL Server új példányai, amelyek nem rendelkezik az internet-kapcsolatot virtuális gépeken futnak. A tartomány vezérlők azonban van egy internetes kapcsolat. Hogy a távoli asztal, az egyik tartományt vezérlők első RDP SQL-kiszolgálók csatlakoztatni. Nyissa meg az SQL Server egy másik RDP a tartományvezérlőnek.

Az elsődleges tartományvezérlőnek RDP, hogy kövesse az alábbi lépéseket:

1.  Az Azure portál irányítópult nagyon, hogy a telepítés sikerült.

1.  Kattintson a **források**gombra.

1.  Kattintson az **erőforrások** lap az **Active Directory-elsődleges-adatközpont** , ami a számítógép neve a virtuális gép az elsődleges tartomány vezérlőhöz.

1.  A lap, az **Active Directory-elsődleges-adatközpont** kattintson a **Csatlakozás**gombra. A böngésző kéri, hogy megnyitni vagy menteni kívánja a távkapcsolat objektum kívánja. Kattintson a **Megnyitás**gombra.
![Csatlakozás Adatközpont](./media/virtual-machines-windows-portal-sql-alwayson-availability-groups/13-ad-primary-dc-connect.png)
1.  **Távoli asztali kapcsolat** is figyelmezteti, hogy a távoli kapcsolat a publisher nem azonosítania kell magát. Kattintson a **Csatlakozás**gombra.

1.  A Windows biztonsági figyelmeztetéseinek meg kell adnia a hitelesítő adatok csatlakoztatása a elsődleges tartományvezérlőnek IP-címét. Kattintson a **másik fiók használata**. Írja be **felhasználónevét** **contoso\DomainAdmin**. Ez a rendszergazdai felhasználónév választotta a fiók. Ha úgy állította be a sablon kiválasztott összetett jelszót használja.

1.  **Távoli asztali** is figyelmezteti, hogy a távoli számítógép nem sikerült hitelesíteni a biztonsági tanúsítványával probléma miatt. Meg kell követnie a biztonsági tanúsítvány nevét. Ha követte az oktatóprogram neve lesz az **Active Directory-elsődleges-dc.contoso.com**. Kattintson az **Igen**gombra.

Most már az elsődleges tartományvezérlőnek csatlakozik. Az SQL Server RDP, hogy kövesse az alábbi lépéseket:

1.  Nyissa meg a tartományvezérlőnek a **Távoli asztali kapcsolat**.

1.  **Számítógép**írja be az SQL Server egyik nevét. Az ebben az oktatóanyagban írja be az **SQL Server-0**.

1.  Használja a ugyanazzal a felhasználói fiókkal és a jelszót, amelyet a tartományvezérlőnek RDP használt.

Most már csatlakozott az SQL Server RDP. Nyissa meg az SQL Server management Studio eszközben, az alapértelmezett példány az SQL Server kapcsolódni, és ellenőrizze, hogy a availabilty csoportban be van állítva.


