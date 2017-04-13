<properties
   pageTitle="SQL-adatraktár biztonsági másolatok |} Microsoft Azure"
   description="Tudjon meg többet az SQL-adatraktár beépített adatbázis biztonsági mentése, amely lehetővé teszi az Azure SQL-adatraktár visszaállítása visszaállítási pont vagy egy másik földrajzi területhez tartozik."
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lakshmi1812"
   manager="barbkess"
   editor="monicar"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/06/2016"
   ms.author="lakshmir;barbkess"/>

# <a name="sql-data-warehouse-backups"></a>SQL-adatraktár biztonsági másolatok

SQL-adatraktár kínál a helyi és a földrajzi biztonsági másolatok raktári biztonsági funkciói adatok részeként. Ezek közé tartozik az Azure Blob-tároló pillanatképek és geo felesleges tárolási. Adatok raktári biztonsági másolatok segítségével a adatraktár visszaállítása a elsődleges régióban visszaállítási pontra vagy visszaállítani egy másik földrajzi területhez tartozik. Ez a cikk ismerteti az SQL adatraktár biztonsági másolatok részletei határozzák meg.

## <a name="what-is-a-data-warehouse-backup"></a>Mi az adatok raktári biztonsági?

Raktári másolatának az adatokat, ha vissza szeretne állítani egy adatraktár egy adott időpontban használható.  SQL adatraktár rendszer elosztott, mivel sok fájl Azure BLOB-a tárolt adatok raktári biztonsági áll. 

Adatbázis biztonsági mentése minden üzleti folytonosságot és katasztrófa helyreállítási stratégia alapvető részét képezik, mert azok az adatok védelme biztonsági véletlen sérülése vagy törlését. További információ az [üzleti folytonosságot áttekintése](../sql-database/sql-database-business-continuity.md)című témakörben találhat.

## <a name="data-redundancy"></a>Adatok redundancia

SQL-adatraktár az adatok úgy, hogy az adatok tárolása helyileg felesleges (LRS) Azure prémium tároló védi. Azure tároló funkció az adatok szinkronizált példányok a helyi adatok központ átlátszó adatvédelem zökkenőmentes, ha honosított hibák tárol. Adatok redundancia biztosítja, hogy az adatok pusztítani infrastruktúra problémák, például a lemez hibáit. Adatok redundancia biztosítja az alternatív hiba üzemkészségének és könnyen hozzáférhető infrastruktúra.

További tudnivalók:

- Azure prémium tárolására, [Azure prémium tároló](../storage/storage-premium-storage.md)áttekintése című témakörben.
- Helyi meghajtóra felesleges tárolására, lásd: [Azure tároló replikáció](../storage/storage-redundancy.md#locally-redundant-storage).


## <a name="azure-storage-blob-snapshots"></a>Azure Blob tároló pillanatképek

Azure prémium tároló használata egy előnyeit, mint adatraktár SQL Azure Blob-tároló pillanatképek használja biztonsági mentése helyi meghajtóra adatraktár. Visszaállíthatja a adatraktár pillanatkép visszaállítási pontra. Egy adott időpontban érvényes legalább nyolc óránként indítása és hét napig.  

További tudnivalók:

- Azure blob-pillanatfelvételek, lásd [létrehozása blob-pillanatfelvétel](../storage/storage-blob-snapshots.md).


## <a name="geo-redundant-backups"></a>GEO felesleges biztonsági másolatok

24 óránként SQL adatraktár tárolja a teljes adatraktár szabványos tároló. A teljes adatraktár időtartamát az utolsó pillanatkép megfelelően jön létre. A szokásos tároló tartozik egy geo felesleges tárterület-fiókkal (TS-GRS) olvasási hozzáférést. Az Azure tároló TS-GRS funkció a biztonságimásolat-fájlok replikálja [párosított adatközpont](../best-practices-availability-paired-regions.md). A geo replikáció köszönhetően abban az esetben, ha nem érhetők el a pillanatképek elsődleges régiójában vissza tudja állítani adatraktár. 

Ez a szolgáltatás alapértelmezés szerint be van kapcsolva. Ha nem szeretné geo felesleges biztonsági másolatokat, választhatja. 

>[AZURE.NOTE] Azure-tárolóban lévő a szerződési időszak *replikációs* fájlok másolása egyik helyről a másikra hivatkozik. SQL- *adatbázis-replikáció* szinkronizálja az elsődleges adatbázis több másodlagos adatbázisokhoz való megőrzési hivatkozik. 

További tudnivalók:
- GEO felesleges tárhelyek – lásd: [Azure tároló replikáció](../storage/storage-redundancy.md).
- TS-GRS tárhelyek – lásd: [olvasásra geo felesleges tároló](../storage/storage-redundancy.md#read-access-geo-redundant-storage).

## <a name="data-warehouse-backup-schedule-and-retention-period"></a>Adatok raktári biztonsági mentés ütemezése és az adatmegőrzési időszak

SQL-adatraktár pillanatképek hoz létre az online adatraktárakhoz négy és nyolc óránként, és továbbra is az egyes pillanatkép hét napig. Vissza tudja állítani az online adatbázisra egyik visszaállítása pont az utóbbi hét napban a. 

Az utolsó pillanatfelvétel indításakor megtekintéséhez a lekérdezés futtatása a SQL online adatraktár. 

```sql
select top 1 *
from sys.pdw_loader_backup_runs 
order by run_id desc;
```

Ha nincs hét napja pillanatképét megőrzése kell, a visszaállítási pont egy új adatraktár vissza tudja állítani. A visszaállítás befejezése után SQL adatraktár pillanatképek hozza létre az új adatraktár indítja el. Ha az új adatraktár nem módosítja, a pillanatképek kapcsolatának üres és ezért a pillanatkép költség minimális. Tudta is mutasson az, hogy a SQL adatraktár pillanatképek létrehozása az adatbázisban.


### <a name="what-happens-to-my-backup-retention-while-my-data-warehouse-is-paused"></a>Mi történik a biztonsági másolat adatmegőrzési, miközben a adatraktár fel van függesztve?

SQL-adatraktár nem hoz létre egy adott időpontban érvényes, és nem jár le pillanatképek közben egy adatraktár fel van függesztve. Pillanatfelvétel kora nem változtatja meg, amíg a adatraktár fel van függesztve. Pillanatkép adatmegőrzési alapuló a adatraktár online szolgáltatás, nem a naptári napok napok számát.

Például ha pillanatfelvétel október 1 4 du kezdődő és a adatraktár fel van függesztve október 3, 4 du, a pillanatfelvétel régi két napig tart. Amikor a adatraktár ismét online állapotba kerül a pillanatkép a régi két nap. Ha a adatraktár online állapotba kerül október 5 4 du., a pillanatkép két napnál régebbi és öt több napig maradjon.

A adatraktár ismét online állapotba kerül, amikor SQL adatraktár új pillanatképek megjelenni, és pillanatképek lejár, ha több mint hét napja az adatok rendelkeznek.

### <a name="how-long-is-the-retention-period-for-a-dropped-data-warehouse"></a>Mennyi ideig az adatmegőrzési időszak egy kihagyott adatraktár?
Amikor egy adatraktár megszakad, a adatraktár a pillanatképek hét napig mentett és majd eltávolítja. Vissza tudja állítani a adatraktár mentett visszaállítási pontok közül.

> [AZURE.IMPORTANT] Ha töröl egy logikai SQL server-példányt, adatbázisokra, amelyek a példány tartoznak is törlődik, és nem állíthatók. A Törölt kiszolgáló nem tudja visszaállítani.

## <a name="data-warehouse-backup-costs"></a>Adatok raktári biztonsági költségek

A legközelebbi TB teljes költségét a elsődleges adatraktár és Azure Blob-pillanatképek hét napig lesz kerekítve. Például a adatraktár 1,5 TB és a pillanatképek 100 GB-ot használja, ha meg van számlát kapni az Azure prémium tároló kamatlába adatok 2 TB. 

>[AZURE.NOTE] Minden egyes pillanatkép üres kezdetben, és végezze el a módosításokat a elsődleges adatraktár nő. Az összes pillanatfelvételek a adatok raktári változzon méretű növelése Emiatt a tárolási költségeket pillanatképek nagyobb aszerint, hogy mennyi díjat kell módosítása.

Ha geo felesleges tárterületet használ, külön tároló díjat jelenik meg. Geo felesleges tárolására van az olvasásra – földrajzilag felesleges tároló (TS-GRS) alapdíj számlát kapni.

SQL adatraktár árak kapcsolatos további tudnivalókért olvassa el az [SQL-adatok raktári árak](https://azure.microsoft.com/pricing/details/sql-data-warehouse/)című témakört.

## <a name="using-database-backups"></a>Használatával az adatbázis biztonsági mentése

Az elsődleges SQL adatok raktári biztonsági másolatok használata Ha vissza szeretne állítani a adatraktár egyik visszaállítása pont az adatmegőrzési időszak belül.  

- Egy SQL adatraktár, című témakörben tájékozódhat [a SQL adatraktár visszaállítása](sql-data-warehouse-restore-database-overview.md).


## <a name="related-topics"></a>Kapcsolódó témakörök

### <a name="scenarios"></a>Felhasználási területei

- Egy üzleti folytonosságot áttekintését a [üzleti folytonosságot áttekintése](../sql-database/sql-database-business-continuity.md) című témakörben találhat.


<!-- ### Tasks -->

- Egy adatraktár, című témakörben tájékozódhat [a SQL adatraktár visszaállítása](sql-data-warehouse-restore-database-overview.md)

<!-- ### Tutorials -->

