<properties
   pageTitle="SQL-adatbázis vészhelyreállítás |} Microsoft Azure"
   description="Megtudhatja, hogy miként adatbázis helyreállítása regionális adatközponthoz üzemszünetek vagy sikertelen az Azure SQL adatbázis aktív Geo replikációs és Geo-helyreállítási lehetőségeket."
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/13/2016"
   ms.author="carlrab"/>

# <a name="restore-an-azure-sql-database-or-failover-to-a-secondary"></a>Az Azure SQL-adatbázis vagy feladatátvevő visszaállítása a másodlagos

Azure SQL-adatbázis-a-üzemszünetek helyreállítása az alábbi funkciókat kínál:

- [Aktív Geo replikációs](sql-database-geo-replication-overview.md)
- [GEO-visszaállítása](sql-database-recovery-using-backups.md#point-in-time-restore)

Üzleti folytonosságot forgatókönyvek és a funkciók támogatása forgatókönyvekben kapcsolatos további tudnivalókért olvassa el a [üzemkészségének](sql-database-business-continuity.md)című témakört.

### <a name="prepare-for-the-event-of-an-outage"></a>Arra az esetre, egy üzemszünetek előkészítése

A helyreállítási szeretne egy másik adatterület az aktív Geo replikációs vagy a geo felesleges biztonsági másolatok success, frissítenie kell a kiszolgáló előkészítése másik adatközpont válik az új elsődleges kiszolgáló üzemszünetek kell szükséges a merülnek fel, valamint a jól definiált dokumentálni és annak biztosítására, a zökkenőmentes helyreállítás vizsgálni lépéseket. Előkészítés lépések a következők:

- Azonosítsa a logikai server egy másik területen az új elsődleges kiszolgáló lesz. Az aktív Geo replikációs, ez lesz talán minden másodlagos-kiszolgálót és legalább egy. Geo-visszaállításhoz Ez általában lesz az adatbázis a régió [párosított régió](../best-practices-availability-paired-regions.md) kiszolgáló.
- Azonosítása, és tetszés szerint megadása a felhasználóknak az új elsődleges adatbázis eléréséhez szükséges kiszolgálói szintű tűzfalszabályokat.
- Határozza meg, hogyan fogja irányítsa át a felhasználókat az új elsődleges kiszolgálóhoz, például a csatlakozási_karakterlánc módosításával vagy a DNS-bejegyzések módosításával.
- Azonosítása, és tetszés szerint létrehozni, a bejelentkezések kell lennie a fő adatbázist az új elsődleges kiszolgálón, és győződjön meg róla, ezek bejelentkezések rendelkezik a megfelelő engedélyekkel a fő adatbázist, ha vannak ilyenek. További tudnivalókért lásd: az [SQL-adatbázis biztonsági vészhelyreállítás után](sql-database-geo-replication-security-config.md)
- Azonosítsa a figyelmeztetési szabályokat, amelyek kell frissíteni kell, hogy az új elsődleges adatbázis megfeleltetése.
- A naplózási konfigurációt, az aktuális elsődleges adatbázis a dokumentumban
- Hajtsa végre a [vészhelyreállítás kevésbé részletes adatok megjelenítése](sql-database-disaster-recovery-drills.md). Hasonlóan egy üzemszünetek Geo-visszaállításhoz, törlése, és nevezze át a forrásadatbázis okoz a csatlakozási alkalmazáshiba. Hasonlóan egy üzemszünetek aktív Geo-replikáció, letilthatja a webalkalmazás vagy csatlakozik az adatbázis vagy az adatbázis feladatátvevő virtuális gép connectity alkalmazáshibák okozhat.

## <a name="when-to-initiate-recovery"></a>Mikor érdemes helyreállítási kezdeményezése

A helyreállítás hatással van az alkalmazás. Szükséges SQL-kapcsolati karakterlánc vagy a DNS-sel átirányítás módosítása és a állandó adatvesztés eredményezhet. Ezért el kell végezni csak akkor, amikor a üzemszünetek valószínű, hogy az alkalmazás helyreállítási idő cél már utolsó. Amikor az alkalmazás telepítve van a termelési végezze el az alkalmazás állapotát jelző rendszeres ellenőrzésére, és használja az alábbi adatpontok, hogy a helyreállítás indokolt fogadhatja:

1.  Állandó kapcsolódási hiba az alkalmazás réteg a az adatbázist.
2.  Az Azure portál kapcsolatos esemény értesítés széles hatása a régió jeleníti meg.
3.  Az Azure SQL-adatbázis kiszolgálója teljesítménye csökkent van-e jelölve.

Attól függően, hogy az alkalmazás hibatűrést a lehető legrövidebb leállás és a vállalati felelősséget vegye figyelembe az alábbi helyreállítási lehetőségeket.

Az [Első helyreállítható adatbázis](https://msdn.microsoft.com/library/dn800985.aspx) (*LastAvailableBackupDate*) segítségével a legújabb Geo replikált visszaállítási pontra.

## <a name="wait-for-service-recovery"></a>Várja meg a szolgáltatás helyreállítás

Azure csapatok munka szorgalmasan visszaállítása szolgáltatáselérhetőség, gyorsan a lehető, de attól függően, hogy a legfelső szintű okozza, azt is igénybe vehet, óra vagy nap.  Ha az alkalmazás elviseli jelentős legrövidebb leállás is egyszerűen várja meg a helyreállítás befejezéséhez. Ebben az esetben nincs további teendő hajtana szükség. Az aktuális szolgáltatásállapot az [Azure állapotjelző irányítópultja](https://azure.microsoft.com/status/)megjelenik. Az alkalmazás elérhetőségét a program visszaállítja a helyreállítás a régió.

## <a name="failover-to-geo-replicated-secondary-database"></a>Másodlagos adatbázis geo replikált áttérni

Ha az alkalmazás legrövidebb leállás eredményezhet üzleti felelősséggel kell használni geo replikált adatbázis(ok) az alkalmazásban. Engedélyezi az alkalmazás elérhetősége egy másik régióbeli abban az esetben, ha egy üzemszünetek gyorsan visszaállítására. További tudnivalók [A replikáció Geo](sql-database-geo-replication-portal.md)beállításáról.

A támogatott módszerekkel geo replikált másodlagos való áttérés kezdeményezzen kell adatbázis(ok) elérhetősége visszaállításához.

Kövesse az alábbi útmutatók egy másodlagos geo replikált adatbázist áttérni egyikét:

- [Azure portálon geo replikált másodlagos áttérni](sql-database-geo-replication-portal.md)
- [Egy geo replikált másodlagos áttérni PowerShell használatával](sql-database-geo-replication-powershell.md)
- [Az SQL-T használ geo replikált másodlagos áttérni](sql-database-geo-replication-transact-sql.md)

## <a name="recover-using-geo-restore"></a>Geo-visszaállítási használatával helyreállítása

Ha az alkalmazás legrövidebb leállás nem vezet üzleti felelősséggel szolgál Geo-visszaállítási is használhatja a helyreállítása az alkalmazás adatbázis(ok). Létrehoz egy másolatot az adatbázisról legújabb geo felesleges másolatból.

Válasszon az alábbi útmutatókat geo visszaállítandó adatbázis be egy új terület:

- [Azure portálon új területére adatbázis GEO-visszaállítása](sql-database-geo-restore-portal.md)
- [GEO-visszaállítása az adatbázis egy új terület PowerShell használatával](sql-database-geo-restore-powershell.md)

## <a name="configure-your-database-after-recovery"></a>Az adatbázis konfigurálása helyreállítás

Ha egy üzemszünetek helyreállítása geo replikációs feladatátvevő vagy a geo-visszaállítási szolgáltatást használ, győződjön meg arról, hogy a kapcsolatot az új adatbázisok helyesen van beállítva, hogy a normál alkalmazás függvény folytatható. Ez az első készen áll a visszaállított adatbázist gyártási egy tevékenység-ellenőrzőlistát.

### <a name="update-connection-strings"></a>Kapcsolati karakterlánc frissítése

A helyreállított adatbázis egy másik Server elhelyezkednek, mert frissítenie kell az alkalmazás kapcsolati karakterláncot, ennek a kiszolgálónak mutassanak.

A csatlakozási_karakterlánc módosításával kapcsolatos további tudnivalókért lásd: a [kapcsolattár](sql-database-libraries.md)megfelelő fejlesztői nyelvét.

### <a name="configure-firewall-rules"></a>Tűzfalszabályokat konfigurálása

Győződjön meg arról, hogy a kiszolgálón és az adatbázis konfigurált tűzfalszabályokat megegyeznek az elsődleges kiszolgálón és elsődleges adatbázis konfigurált szüksége. További tudnivalókért lásd: [Útmutató: tűzfal beállításainak konfigurálása (Azure SQL-adatbázis)](sql-database-configure-firewall-settings.md).


### <a name="configure-logins-and-database-users"></a>Állítsa be a bejelentkezési és az adatbázis-felhasználók

Győződjön meg arról, hogy az alkalmazás által használt bejelentkezési nevek szerepel-e a kiszolgálón, amelynek van a visszaállított adatbázist tartalmazó szüksége. További tudnivalókért lásd: [A replikáció Geo biztonsági beállításai](sql-database-geo-replication-security-config.md).

>[AZURE.NOTE] Állítsa be, és ellenőrizze a a kiszolgáló tűzfalszabályokat és bejelentkezések (és engedélyeiket) egy katasztrófa helyreállítási részletező során. Ezek kiszolgálói szintű az objektumok és azok konfigurációja nem feltétlenül vehető igénybe az üzemszünetek során.

### <a name="setup-telemetry-alerts"></a>Telemetriai értesítések beállítása

Ellenőrizze, hogy a meglévő szabályt beállításainak frissülnek a visszaállított adatbázist, és a különböző kiszolgáló megfeleltetése szüksége.

Adatbázis riasztási szabályok kapcsolatos további tudnivalókért olvassa el a [Értesítés értesítést kap](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) , és a [Szolgáltatásállapot nyomon követése](../monitoring-and-diagnostics/insights-service-health.md)című témakört.

### <a name="enable-auditing"></a>A naplózás engedélyezése

Ha a naplózás az adatbázis eléréséhez szükséges, szüksége naplózás az adatbázis-helyreállítás engedélyezése. Jól jelzi, hogy a naplózás szükség, ügyfélalkalmazásokban használata biztonságos kapcsolatot karakterláncok mintát a *. database.secure.windows.net. További tudnivalókért olvassa el a [– első lépések az SQL-adatbázis Képletvizsgálat](sql-database-auditing-get-started.md)című témakört.


## <a name="next-steps"></a>Következő lépések

- Azure SQL-adatbázis automatikus biztonsági mentés kapcsolatos további tudnivalókért lásd: [az automatikus biztonsági mentés SQL-adatbázis](sql-database-automated-backups.md)
- Üzleti folytonosságot tervezés és helyreállítási forgatókönyvek kapcsolatos további tudnivalókért lásd: a [folyamatosság felhasználási területei](sql-database-business-continuity.md)
- Az automatikus biztonsági mentés helyreállítási használata című témakörben talál [a szolgáltatás által kezdeményezett biztonsági mentés adatbázis visszaállítása](sql-database-recovery-using-backups.md)
