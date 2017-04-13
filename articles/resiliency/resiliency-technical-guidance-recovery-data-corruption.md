<properties
   pageTitle="Az adatvesztést vagy a véletlen törlését helyreállítása technikai útmutató tűrőképessége |} Microsoft Azure"
   description="A adatsérülésekkel adatok vagy a véletlen adattörlés és rugalmas, könnyen hozzáférhető, hibafa alternatív alkalmazások tervezése, valamint vészhelyreállítás megtervezése helyreállításáról ismertető cikket"
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#<a name="azure-resiliency-technical-guidance-recovery-from-data-corruption-or-accidental-deletion"></a>Azure tűrőképessége technikai útmutató: adatsérülés vagy véletlen törlését helyreállítása

Ha az adatok kap sérült vagy véletlenül törölt robusztus üzleti folytonosságot terv részét problémákat terv. Az alábbiakban helyreállítási információt után adatok sérült, vagy véletlenül töröl, alkalmazáshibák vagy operátorral hiba miatt.

##<a name="virtual-machines"></a>Virtuális gépeken futó

Alkalmazáshibák vagy a véletlen törlését elleni védelem az Azure virtuális gépeken futó (más néven infrastruktúra-mint-a-szolgáltatás VMs), használja az [Azure biztonsági másolatot](https://azure.microsoft.com/services/backup/). Azure biztonsági másolat lehetővé teszi a biztonsági másolatok, amely több virtuális lemezre konzisztensek létrehozását. Ezenkívül a biztonsági másolat tárolóra replikálhatók különböző régiók régió elvesztését a helyreállítási megadását.

##<a name="storage"></a>Tárhely

Figyelje meg, hogy Azure tároló adatok tűrőképessége automatikus kópiák keresztül biztosít, amíg ez nem akadályozza meg az alkalmazás kódjának (vagy fejlesztők és felhasználók) származó adatok rongál keresztül véletlen vagy várt törlését, frissítés, és így tovább. Speciális technikák, például az adatok másolása egy másodlagos tárolási helye a naplók karbantartása adatok funkcióvesztést növelése alkalmazás vagy felhasználó hiba van szükség. A fejlesztők kihasználhatja a blob [Pillanatfelvétel lehetőséget](https://msdn.microsoft.com/library/azure/ee691971.aspx), csak olvasható pont és az idő pillanatképek blob-tartalom is létrehozhat, amelyek. A használható adatok hűségű megoldást alapjaként tároló Azure BLOB.

###<a name="blob-and-table-storage-backup"></a>BLOB és a táblázat tároló biztonsági mentése

Habár BLOB és táblázatok csak nagyon tartós, mindig jelölnek az adatokat az aktuális állapotát. A nem kívánt módosítása vagy törlése az adatok helyreállítás megkövetelheti adatok visszaállítása az előző állapotát. Ez az Azure tárolására és megőrzése pont és az idő másolatok által biztosított funkciók kihasználva érhető el.

Az Azure BLOB hajtsa végre az [blob-pillanatfelvétel szolgáltatás](https://msdn.microsoft.com/library/ee691971.aspx)pont és az idő biztonsági másolatok. Minden egyes pillanatkép csak az előfizetést terhelő a tárterület a különbségeket belül a blob tárolásához, mivel az utolsó pillanatkép állam szükséges. A pillanatképek függenek a alapulnak, az eredeti blob megléte, ezért tanácsos egy fájlmásolás más blob vagy akár egy másik tárterület-fiókot. Ezzel biztosíthatja, hogy adatok biztonsági másolatának megfelelően védeni véletlen törlését. Azure táblázatok esetében pont és az idő másolatok egy másik tábla vagy Azure BLOB teheti meg. Részletes útmutatást és a biztonsági mentés táblák alkalmazásszintű példák és BLOB itt találja:

  * [A táblázatok alkalmazáshibák elleni védelem](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/05/03/protecting-your-tables-against-application-errors/)
  * [A BLOB alkalmazáshibák elleni védelem](https://blogs.msdn.microsoft.com/windowsazurestorage/2010/04/29/protecting-your-blobs-against-application-errors/)

##<a name="database"></a>Adatbázis

Nincsenek Azure SQL-adatbázis több [üzemkészségének](../sql-database/sql-database-business-continuity.md) (biztonsági másolat, visszaállítás) beállítás. Adatbázisok másolhatók, az [Adatbázis másolása](../sql-database/sql-database-copy.md) funkció használata, vagy [exportálása](../sql-database/sql-database-export.md) és [importálása](https://msdn.microsoft.com/library/hh710052.aspx) egy SQL Server bacpac fájlt. Adatbázis-példány adja meg a tranzakción keresztül egységes eredményeit, míg a bacpac (szolgáltatással az importálás/exportálás) nem. Az alábbi lehetőségek is szolgáltatásként várólista alapú belül az adatközpont, és egy idő – Befejezés SLA jelenleg nem biztosítanak.

>[AZURE.NOTE]Az adatbázis vágólapra másolhatja, és az importálás/exportálás beállításai elhelyezése a forrásadatbázis terhelés jelentős mértékben. Indíthatnak erőforrás kérelem vagy események szabályozásának.

###<a name="sql-database-backup"></a>SQL-adatbázis biztonsági mentése

Microsoft Azure SQL-adatbázis pont és az idő másolatok [az Azure SQL-adatbázis](../sql-database/sql-database-copy.md)másolásával érhetők el. Ez a parancs használatával adatbázis tranzakción keresztül egységes másolatának létrehozása azonos logikai adatbázis-kiszolgálón vagy egy másik kiszolgálóra. Bármelyik lehetőséget választja az adatbázis-példány teljes funkcionalitású és a forrásadatbázis teljesen független. Minden egyes példányt hoz létre egy pont és az idő helyreállítási lehetőséget jelöli. Az adatbázis állam teljesen helyreállítása átnevezése a forrás adatbázis nevét az új adatbázist. Másik lehetőségként, adatok meghatározott részhalmazát az új adatbázisból úgy állíthatja Transact-SQL lekérdezésekkel. SQL-adatbázissal kapcsolatos további részletekért lásd: [az Azure SQL-adatbázis üzemkészségének áttekintése](../sql-database/sql-database-business-continuity.md).

###<a name="sql-server-on-virtual-machines-backup"></a>A biztonsági virtuális gépeken futó SQL Server

Az SQL Server Azure infrastruktúra helyőrzőként a szolgáltatás virtuális gépeken futó (más néven IaaS vagy IaaS VMs), két lehetőség: hagyományos biztonsági másolatok készítése és a szállítási naplót. Hagyományos biztonsági másolatok használata lehetővé teszi, ha vissza szeretne állítani egy adott pontjához időben, de lassú a helyreállítási folyamat. Hagyományos biztonsági mentés visszaállítása és elő kell készítenie az első teljes biztonsági kezdve, majd alkalmazása az összes biztonsági másolatot készített ezt követően. A második, hogy állítsa be a naplózási szállítás munkamenetek késleltetés a napló biztonsági mentés visszaállítása (például úgy, hogy két óra). Ezzel a megoldással egy ablakban lévő hibák végzett az elsődleges visszaállításához.

##<a name="other-azure-platform-services"></a>Más Azure platform szolgáltatások

Egyes Azure platform-szolgáltatások felhasználó által felügyelt tárterület-fiók vagy Azure SQL-adatbázis adatok tárolására. Ha a fiók vagy tároló erőforrás törölt, vagy a sérült, ez a szolgáltatás komoly hibák okozhat. Ebben az esetben fontos megőrzéséhez biztonsági másolatokat, amelyekben szeretné, hogy ezek az erőforrások hozza létre, mintha törölt vagy megsérült.

Azure-webhelyek és Azure Mobile szolgáltatások biztonsági mentése és a társított adatbázisok kezelése. Az Azure Media szolgáltatás és a virtuális gépeken futó kell a társított Azure tárterület-fiók és az összes erőforrásában található, a fiók kezelése. Például virtuális gépeken futó, meg kell biztonsági mentése és kezelése az Azure blob-tárolóhoz virtuális lemezt.

##<a name="checklists-for-data-corruption-or-accidental-deletion"></a>Ellenőrzőlisták a adatsérülés vagy véletlen törlését

##<a name="virtual-machines-checklist"></a>Virtuális gépeken futó ellenőrzőlista

  1. Tekintse át a jelen dokumentum virtuális gépeken futó szakaszát.
  2. Készítsen biztonsági másolatot, és Azure biztonsági (vagy saját biztonsági rendszer Azure blob-tárolóhoz és virtuális pillanatképek) virtuális lemezt karbantartása.

##<a name="storage-checklist"></a>Tároló feladatlista

  1. Tekintse át a dokumentum tárolási szakaszát.
  2. Rendszeresen biztonsági másolatot készíteni a kritikus tároló erőforrásokat.
  3. Fontolja meg BLOB-pillanatfelvétel funkcióval.

##<a name="database-checklist"></a>Adatbázis ellenőrzőlista

  1. Tekintse át a jelen dokumentum adatbázis szakaszát.
  2. Pont és az idő biztonsági másolatok létrehozása az adatbázis Másolás paranccsal.

##<a name="sql-server-on-virtual-machines-backup-checklist"></a>SQL Server virtuális gépeken futó biztonsági másolat ellenőrzőlista

  1. Tekintse át az SQL Server, a jelen dokumentum virtuális gépeken futó biztonsági másolat szakaszát.
  2. Hagyományos biztonsági mentése és visszaállító technikákat.
  3. Munkamenet szállítási késésben lévő naplót létrehozni.

##<a name="web-apps-checklist"></a>Web Apps alkalmazások ellenőrzőlista

  1. Készítsen biztonsági másolatot, és a társított adatbázis kezelése, ha van ilyen.

##<a name="media-services-checklist"></a>Media Services ellenőrzőlista

  1. Készítsen biztonsági másolatot, és a kapcsolódó tároló erőforrások karbantartása.

##<a name="more-information"></a>További információ

Azure biztonsági mentési és visszaállítási szolgáltatásaival kapcsolatos további tudnivalókért olvassa el a [tárhely, biztonsági mentése és helyreállítása esetek](https://azure.microsoft.com/documentation/scenarios/storage-backup-recovery/)című témakört.

##<a name="next-steps"></a>Következő lépések

Ez a cikk csak a [technikai útmutató Azure tűrőképessége](./resiliency-technical-guidance.md)sorozat része. Ha további tűrőképessége vészhelyreállítás és magas elérhetősége erőforrások keresett Azure tűrőképessége technikai útmutató [további forrásokat](./resiliency-technical-guidance.md#additional-resources)talál.