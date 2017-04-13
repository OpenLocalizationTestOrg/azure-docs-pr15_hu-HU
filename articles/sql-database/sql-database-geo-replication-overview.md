<properties
    pageTitle="Azure SQL-adatbázis aktív Geo replikációs"
    description="Aktív Geo replikációs lehetővé teszi, hogy az adatbázis bármelyik az Azure adatközpontokkal 4 kópiák beállítása."
    services="sql-database"
    documentationCenter="na"
    authors="stevestein"
    manager="jhubbard"
    editor="monicar" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="NA"
    ms.date="09/26/2016"
    ms.author="sstein" />

# <a name="overview-sql-database-active-geo-replication"></a>Áttekintés: Az SQL adatbázis aktív Geo replikációs

Aktív Geo replikációs lehetővé teszi, hogy négy olvasható másodlagos adatbázisok beállítása az ugyanazon vagy másik adatok központ helye (régió). Másodlagos adatbázisok lekérdezése és feladatátvételi egy adatok központ üzemszünetek vagy az elsődleges adatbázishoz való csatlakozáshoz nem érhetők el.

>[AZURE.NOTE] Aktív Geo replikációs (olvasható formátumú másodlagos zónák) az összes szolgáltatási rétegek adatbázisokra most érhető el. Április 2017 visszavonásának nem olvasható másodlagos típusát, és nem olvasható létező adatbázisokat olvasható formátumú másodlagos zónák automatikusan frissülnek.

 Beállíthatja, hogy az aktív Geo replikációs használatával az [Azure portálon](sql-database-geo-replication-portal.md), a [PowerShell](sql-database-geo-replication-powershell.md), a [Transact-SQL nyelvben](sql-database-geo-replication-transact-sql.md), vagy a [REST API - létrehozása vagy az adatbázis frissítése](https://msdn.microsoft.com/library/azure/mt163685.aspx).

> [AZURE.SELECTOR]
- [Beállítása: Azure portál](sql-database-geo-replication-portal.md)
- [Állítsa be: PowerShell](sql-database-geo-replication-powershell.md)
- [Beállítása: Kétmintás T-SQL](sql-database-geo-replication-transact-sql.md)

Ha az bármilyen okból az elsődleges adatbázis sikertelen, vagy egyszerűen offline állapotba kell, *feladatátvevő* valamelyik, az másodlagos adatbázisok is. Egy, a másodlagos adatbázisok feladatátvevő aktiválásakor az összes többi formátumú másodlagos zónák automatikusan az új elsődleges kapcsolódnak.

Azt is megteheti az [Azure portálon](sql-database-geo-replication-failover-portal.md), a [PowerShell](sql-database-geo-replication-failover-powershell.md), [Transact-SQL nyelvben](sql-database-geo-replication-failover-transact-sql.md), a [REST API - tervezett feladatátvevő](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)vagy a [REST API - nem tervezett feladatátvételre](https://msdn.microsoft.com/library/azure/mt582027.aspx)másodlagos áttérni.


> [AZURE.SELECTOR]
- [Feladatátvevő: Azure portál](sql-database-geo-replication-failover-portal.md)
- [Feladatátvevő: PowerShell](sql-database-geo-replication-failover-powershell.md)
- [Feladatátvevő: Kétmintás T-SQL nyelvben](sql-database-geo-replication-failover-transact-sql.md)

Után feladatátvételt ellenőrizze a hitelesítési követelményeket, a kiszolgáló és az adatbázis meg az új elsődleges. A részletekért olvassa [SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md).


Az aktív Geo replikációs funkció egy adatbázist redundancia ugyanabban a Microsoft Azure régióban belül, illetve különböző régióban (geo-redundancia) megadására mechanizmusa hajtja végre. Aktív Geo replikációs aszinkron replikálja végrehajtott tranzakciók adatbázisból négy másolatán az adatbázis használata különböző kiszolgálókon lekötött pillanatkép elkülönítési (RCSI) leválasztáshoz olvasható. Ha aktív Geo replikációs van konfigurálva, a másodlagos adatbázis a megadott kiszolgálón jön létre. Az eredeti adatbázis lesz az elsődleges adatbázist. Az elsődleges adatbázis aszinkron replikálja végrehajtott tranzakciók összes másodlagos adatbázist. Csak a teljes tranzakciók replikált. A bármely pontján a másodlagos adatbázis előfordulhat, hogy kissé mögött az elsődleges adatbázis, a másodlagos adatkapcsolat garantált részleges tranzakciók soha nem kell. A Készletben adatait a [Üzemkészségének áttekintése](sql-database-business-continuity.md)találhatók.

A legfontosabb előnye aktív Geo-replikáció egyik adatbázis szintű katasztrófa helyreállítási megoldást teszi az alacsony helyreállítási idővel. Amikor a másodlagos adatbázist egy kiszolgálón egy másik régióbeli, az alkalmazás hozzáadása maximális címtárfrissítések. Idegen-régió redundancia lehetővé teszi az alkalmazások lévő adatvesztést egy teljes adatközponthoz vagy egy természeti katasztrófák, katasztrofális emberi hibák vagy kártékony jogszabályok által okozott adatközponthoz részei visszaállításához. Az alábbi ábrán látható példa aktív Geo-replikáció konfigurált egy elsődleges régióban Észak központi US és a másodlagos a déli központi US régióban.

![A replikáció GEO kapcsolat](./media/sql-database-active-geo-replication/geo-replication-relationship.png)

Egy másik legfontosabb előnye, hogy a másodlagos adatbázisok olvasható, és csak olvasható munkaterhelésekből, például a feladatok jelentéskészítés kiürítése használható. Ha csak a másodlagos adatbázis használt terheléselosztási, az az elsődleges ugyanabban a régióban hozhat létre. Másodlagos létrehozása ugyanabban a régióban, nem növeli az alkalmazás címtárfrissítések katasztrofális hibákra.  

Más esetben, ha aktív Geo replikációs is használható a következők:

- **Adatbázis áttelepítése**: áttelepítése az adatbázis egyik kiszolgálóról egy másik online, minimális legrövidebb leállás aktív Geo replikációs is használhatja.
- **Alkalmazás frissítése**: létrehozhat egy további másodlagos fail vissza másolatként alkalmazás frissítéskor.

Valós üzemkészségének eléréséhez adatbázist redundancia adatközpontokkal közötti Hozzáadás része csak a megoldást. Az alkalmazás (szolgáltatás)-végpont helyreállítása a után egy Katasztrofális hiba helyreállítási minden összetevő alkotják a szolgáltatást, és a függő szolgáltatásokra van szükség. Ezek az összetevők többek között a ügyfélszoftver (például a böngészőben az egyéni JavaScript-kód), a webes első végű, a tárhely és a DNS. Rendkívül fontos, hogy minden összetevő rugalmassá a az ugyanazt hibákat, a helyreállítási idő cél (RTO) az alkalmazás belül elérhetővé válnak. Ezért szeretne függő szolgáltatások azonosítása és garanciáit és funkciók nyújtanak megértését. Ezután ahhoz, hogy a szolgáltatás funkciók a szolgáltatások, amelyektől függ feladatátvételkor megfelelő lépéseket kell végrehajtania. További információt a tervezése megoldások vészhelyreállítás megoldásainak [tervezése felhő katasztrófa helyreállítási használatával aktív Geo-replikáció](sql-database-designing-cloud-solutions-for-disaster-recovery.md).

## <a name="active-geo-replication-capabilities"></a>Aktív Geo replikációs funkciók
Az aktív Geo replikációs funkció a következő alapvető lehetőségeket nyújtja:

- **Aszinkron automatikus replikáció**: csak akkor készíthet egy másodlagos adatbázis ad hozzá egy meglévő adatbázishoz. A másodlagos csak egy másik Azure SQL-adatbázis kiszolgálója a hozhatók létre. Amikor létrejött, a másodlagos adatbázis kitölti az elsődleges adatbázisból másolt adatok. Ez a folyamat rendezi nevezik. Miután másodlagos adatbázis létrehozott, és a rendezés, az elsődleges adatbázis frissítése aszinkron replikált a másodlagos adatbázis automatikusan. Aszinkron replikáció, az azt jelenti, hogy tranzakciók vannak az elsődleges adatbázison előtt elkövetett a másodlagos adatbázis replikált. 

- **Több másodlagos adatbázisok**: két vagy több másodlagos adatbázist redundancia és az elsődleges adatbázis és az alkalmazás védelmi szintjének növelése. Ha több másodlagos adatbázisok létezik, az alkalmazás marad védett, még akkor is, ha nem sikerül egy a másodlagos adatbázisok valamelyikével. Ha csak egy másodlagos adatbázis található, és sikertelen, az alkalmazás van kitéve magasabb kockázat mindaddig, amíg az új másodlagos adatbázis jön létre.

- **Másodlagos adatbázisok olvasható**: az alkalmazások elérhetik a másodlagos adatbázis használata a fő-adatbázis eléréséhez használt ugyanazon vagy másik rendszerbiztonsági csak olvasható műveletekhez. A másodlagos adatbázisok annak érdekében, az elsődleges (log ismétléses) frissítések nem replikáció végrehajtása a másodlagos lekérdezések pillanatkép elkülönítési üzemmódban működnek.

>[AZURE.NOTE] A napló ismétléses séma, amely azt az elsődleges fogad igénylő frissítések a másodlagos adatbázis sémája lakat esetén a másodlagos adatbázishoz késik.

- **Rugalmas készlet adatbázisok aktív geo-replikáció**: aktív Geo replikációs bármilyen adatbázis bármelyik rugalmas adatbázis készletben állítható be. A másodlagos adatbázis lehet egy másik adatbázis rugalmas készletébe. Normál adatbázisokhoz a másodlagos lehet egy rugalmas adatbázis készlet és fordítva fordítva mindaddig, amíg a szolgáltatási rétegek megegyeznek. 

- **A másodlagos adatbázis konfigurálható teljesítményszint**: másodlagos adatbázis kisebb, mint az elsődleges teljesítményszint hozhatja létre. Elsődleges és másodlagos adatbázisok ugyanabban a szolgáltatás rétegben van szüksége. Ez a beállítás nem ajánlott alkalmazások magas adatbázis írási tevékenységeket használó, mert a nagyobb replikációs eltérés növeli a kockázat jelentős adatvesztés feladatátvevő után. Ezeken kívül feladatátvétel után az alkalmazás teljesítményének hatással van mindaddig, amíg az új elsődleges teljesítmény magasabb szintre frissítését. A napló IO százalékos diagram Azure portálon becsüljük meg a másodlagos a replikáció betöltésének fenntartása szükséges minimális teljesítményével nagyszerűen biztosít. Ha például az elsődleges adatbázisa P6 (1000 DTU), és a napló IO készültségi 50 %-os a másodlagos kell legalább P4 (500 DTU). A IO naplóadatokat [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) vagy [sys.dm_db_resource_stats]( https://msdn.microsoft.com/library/dn800981.aspx) adatbázis-nézetek használata is meghallgathatja.  Az SQL-adatbázis teljesítményszint a további tudnivalókért lásd: az [SQL-adatbázis beállítások és a teljesítmény](sql-database-service-tiers.md). 

- **Felhasználó által felügyelt feladatátvétel és visszaállás**: másodlagos adatbázis explicit módon lehet másikra váltani az elsődleges szerepkör bármikor az alkalmazás vagy a felhasználó. A valós üzemszünetek során a "nem tervezett" beállítás használandó, amely azonnal előléptethet egy másodlagos lesz az elsődleges. Sikertelen elsődleges helyreállítása és érhető el ismét, a rendszer automatikusan megjelöli a helyreállított elsődleges, másodlagos, és jelenítse meg az új elsődleges naprakészen. Miatt replikációs aszinkron jellegét kis mennyiségű adattal elveszhetnek során nem tervezett feladatátadás Ha nem sikerül egy elsődleges, mielőtt egymás között másolják a legutóbbi módosításokat szeretne a másodlagos. Ha több formátumú másodlagos zónák az elsődleges nem sikerül fölé, a rendszer automatikusan átállítja a replikáció kapcsolatok, és a hátralévő formátumú másodlagos zónák csatolja az újonnan előléptetett elsődleges felhasználói beavatkozás nélkül. Miután a üzemszünetek a feladatátvételi okozó szünteti meg, célszerű ahhoz, hogy az alkalmazás az elsődleges területre. Ehhez a feladatátvevő parancs a "tervezett" lehetőséggel lehet hivatkozni. 

- **Hitelesítő adatait, és szinkronizálja tűzfalszabályokat megőrzési**: azt javasoljuk, [adatbázis tűzfalszabályokat](sql-database-firewall-configure.md) használatának geo replikált adatbázisok, ezért ezeket a szabályokat replikálhatók másodlagos adatbázisokra biztosan a azonos tűzfalszabályokat az az elsődleges adatbázissal. Ezt a megközelítést szükségtelenné manuális beállítása és kezelése a mind az elsődleges és másodlagos adatbázisok üzemeltető kiszolgálókon tűzfalszabályokat ügyfeleknek. Hasonlóképpen így használatának [felhasználó az adatbázisban tárolt](sql-database-manage-logins.md) adatokhoz való hozzáférés gondoskodhat arról elsődleges és másodlagos adatbázisok mindig az adott felhasználó hitelesítő adatait a meghibásodáskor, és nincs megszakadása miatt eltérések való bejelentkezést és a jelszavakat. [Azure Active Directory](../active-directory/active-directory-whatis.md)hozzáadásával, ügyfelek is felhasználói hozzáférés kezelése a elsődleges és másodlagos adatbázisok és így nem kell az adatbázisok teljes a hitelesítő adatok kezelése.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Frissítése vagy Visszalépés elsődleges adatbázis-ról
Frissítse, vagy egy másik teljesítményszint (belül ugyanabban a szolgáltatás rétegben) az elsődleges adatbázis vissza léptetheti a másodlagos adatbázis leválasztása nélkül. Frissítéskor, azt javasoljuk, hogy először frissítse a másodlagos adatbázist, és frissítse az elsődleges. Visszalépés-ról, ha a sorrend megfordításához: vissza léptetheti először az elsődleges, és ezután vissza léptetheti a másodlagos. 

A másodlagos adatbázis ugyanabban a szolgáltatás rétegben az az elsődleges, így az elsődleges adatbázis áttelepítése az egy másik szolgáltatáscsaládba réteg igényel, hogy megszünteti a geo replikációs hivatkozásra, és nevezze át a másodlagos adatbázist, vagy egyszerűen engedje el kell lennie. Az elsődleges áttelepítése az új szolgáltatási réteg, majd a replikáció geo átkonfigurálása. Az új másodlagos létrejön automatikusan az elsődleges teljesítmény azonos szintre, alapértelmezés szerint.

## <a name="preventing-the-loss-of-critical-data"></a>A kritikus adatvesztés megelőzése
Miatt nagytávolságú hálózattal nagy késést a folyamatos másolása egy aszinkron replikációs eljárás használja. Aszinkron replikációs teszi adatvesztést megkerülhetetlen, ha hiba történik. Egyes alkalmazások azonban nem az adatok elvesztését szükség lehet. A fontos frissítések védelme, az alkalmazás fejlesztője közvetlenül azután, hogy a tranzakció véglegesítése felhívhatja a [sp_wait_for_database_copy_sync](https://msdn.microsoft.com/library/dn467644.aspx) rendszer eljárást. Hívó **sp_wait_for_database_copy_sync** blokkolja a hívó szál mindaddig, amíg az utolsó lekötött tranzakció volt eljutott a másodlagos adatbázis. Az eljárás a rendszer megvárja, amíg az összes várólistás tranzakció van elismert a másodlagos adatbázis. egy adott folyamatos másolása hivatkozásra **sp_wait_for_database_copy_sync** megfelelően változik. A kapcsolat engedélyekkel az elsődleges adatbázis bármelyik felhasználó felhívhatja ezt az eljárást.

>[AZURE.NOTE] Lehet, hogy egy **sp_wait_for_database_copy_sync** eljáráshívás okozta késleltetés jelentős. A késleltetés függ, hogy a tranzakció napló hossza méretének pillanatában, és a hívás függvény nem ad vissza, amíg az egész naplót replikált van. Kerülje a hívó ezt az eljárást hacsak nem feltétlenül szükséges.

## <a name="programmatically-managing-active-geo-replication"></a>Programozás útján az aktív Geo-replikáció kezelése

A korábban tárgyalt, aktív Geo replikációs is kezelhetők programozás útján a Azure PowerShell és a REST API-t használja. Az alábbi táblázat ismerteti az elérhető parancsok megadása.

- **Azure erőforrás-kezelő API -val és a szerepköralapú biztonsági funkciókat**: aktív Geo replikációs kezelés, beleértve az [erőforrás-kezelő Azure-alapú PowerShell-parancsmagok](sql-database-geo-replication-powershell.md)az [Azure erőforrás-kezelő API -khoz]( https://msdn.microsoft.com/library/azure/mt163571.aspx) készletét tartalmazza. Ezek az API-khoz kell az erőforrás csoportokat használni, és szerepköralapú biztonsági funkciókat (RBAC) támogatja. További információt az access megvalósításáról szerepkörök megtekintése [Azure Role-Based hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)

>[AZURE.NOTE] Sok új funkcióval aktív Geo-replikáció csak támogatott használatával [Azure erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md) [Azure SQL REST API -val](https://msdn.microsoft.com/library/azure/mt163571.aspx) és [Azure SQL-adatbázis PowerShell-parancsmagok](https://msdn.microsoft.com/library/azure/mt574084.aspx)alapján. A (klasszikus) REST API] (https://msdn.microsoft.com/library/azure/dn505719.aspx) és [Azure SQL-adatbázis (klasszikus) parancsmagok](https://msdn.microsoft.com/library/azure/dn546723.aspx) a visszamenőleges kompatibilitás támogatottak, hogy az erőforrás-kezelő Azure-alapú API-k használata ajánlott. 


### <a name="transact-sql"></a>A Transact-SQL nyelvben

|Parancs|Leírás|
|-------|-----------|
|[ALTER adatbázis (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt574871.aspx)|Hozzon létre egy meglévő adatbázis és indítást adatok replikációs másodlagos adatbázist a másodlagos a kiszolgáló hozzáadása argumentum használata|
|[ALTER adatbázis (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt574871.aspx)|FELADATÁTVÉTEL vagy FORCE_FAILOVER_ALLOW_DATA_LOSS használatával válthat a másodlagos adatbázis legyen elsődleges feladatátvevő kezdeményezése
|[ALTER adatbázis (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt574871.aspx)|Távolítsa el a másodlagos a kiszolgáló használata SQL-adatbázishoz, és a megadott másodlagos adatbázis között egy adatok replikációs befejezéséhez.|
|[sys.geo_replication_links (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt575501.aspx)|A logikai Azure SQL-adatbázis-kiszolgáló-adatbázisonként minden meglévő replikációs hivatkozások információt adja vissza.|
|[sys.dm_geo_replication_link_status (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/mt575504.aspx)|Egy adott SQL-adatbázis replikációs legutóbb utolsó replikációs eltérés és a replikáció hivatkozás kapcsolatos egyéb adatok beolvasása|
|[sys.dm_operation_status (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/dn270022.aspx)|Az összes adatbázis-műveletek, többek között a replikáció hivatkozások állapotának állapotjelzője.|
|[sp_wait_for_database_copy_sync (Azure SQL-adatbázis)](https://msdn.microsoft.com/library/dn467644.aspx)|hatására az alkalmazás várnia, amíg az összes végrehajtott tranzakciók replikált, és az aktív másodlagos adatbázis nyugtázza.|
||||

### <a name="powershell"></a>A PowerShell

|Parancsmag|Leírás|
|------|-----------|
|[Get-AzureRmSqlDatabase](https://msdn.microsoft.com/en-us/library/azure/mt603648.aspx)|Egy vagy több adatbázis kap.|
|[Új AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx)|Egy meglévő adatbázis egy másodlagos adatbázist hoz létre, és elindítja az adatok replikáció.|
|[Set-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt619393.aspx)|Átvált egy másodlagos adatbázis elsődleges feladatátvevő elindítására.|
|[Eltávolítás-AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/en-us/library/mt603457.aspx)|Megszakítja adatok replikációs SQL-adatbázishoz, és a megadott másodlagos adatbázis között.|
|[Get-AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx)|Azure SQL-adatbázis és erőforráscsoport vagy SQL Server közötti geo-replikációs hivatkozások kap.|
||||

### <a name="rest-api"></a>REST API-VAL

|API|Leírás|
|---|-----------|
|[Hozzon létre vagy az adatbázis frissítése (createMode = visszaállítás)](https://msdn.microsoft.com/library/azure/mt163685.aspx)|Hoz létre, frissítések, vagy egy elsődleges és másodlagos adatbázis visszaállítása.|
|[Get létrehozása, vagy az adatbázis állapotának módosítása](https://msdn.microsoft.com/library/azure/mt643934.aspx)|Az állapot eredménye létrehozása művelet során.|
|[Az elsődleges (tervezett feladatátvétel) másodlagos adatbázis beállítása](https://msdn.microsoft.com/ibrary/azure/mt575007.aspx)|Az új elsődleges adatbázis válhat Geo replikációs partnerség a másodlagos adatbázis előléptetése|
|[Az elsődleges (tervezett feladatátvétel) másodlagos adatbázis beállítása](https://msdn.microsoft.com/library/azure/mt582027.aspx)|Kényszerített feladatátvevő a másodlagos adatbázishoz, és a másodlagos beállítása az az elsődleges.|
|[A replikáció hivatkozásainak beolvasása](https://msdn.microsoft.com/library/azure/mt600929.aspx)|Az összes replikációs hivatkozások kap geo replikációs partnerség az adott SQL-adatbázishoz. Azt beolvassa az adatokat a sys.geo_replication_links katalógus nézetben látható.|
|[Replikációs hivatkozás létrehozása](https://msdn.microsoft.com/library/azure/mt600778.aspx)|Az adott replikációs hivatkozás megkapja a replikáció geo partnerség az adott SQL-adatbázishoz. Azt beolvassa az adatokat a sys.geo_replication_links katalógus nézetben látható.|
||||



## <a name="next-steps"></a>Következő lépések

- Egy üzleti folytonosságot – áttekintés és -esetek [áttekintése](sql-database-business-continuity.md) című témakörben találhat üzleti folytonosságot
- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért olvassa el a [SQL-adatbázis automatikus biztonsági mentés](sql-database-automated-backups.md)című témakört.
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md).
- Az automatikus biztonsági mentés archiválásra használatáról című témakörben talál [adatbázis másolatot](sql-database-copy.md).
- Hitelesítési követelményeket egy új elsődleges kiszolgáló és az adatbázis kapcsolatos további tudnivalókért lásd: az [SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md).
