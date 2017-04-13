<properties
    pageTitle="Az SQL Server ajánlott eljárások a teljesítmény |} Microsoft Azure"
    description="Ajánlott eljárások nyújt a Microsoft Azure VMs az SQL Server teljesítmény optimalizálása."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="rothja"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="09/07/2016"
    ms.author="jroth" />

# <a name="performance-best-practices-for-sql-server-in-azure-virtual-machines"></a>Az Azure virtuális gépeken futó SQL Server ajánlott eljárások a teljesítmény

## <a name="overview"></a>– Áttekintés

Ez a témakör a Microsoft Azure virtuális gép SQL Server teljesítmény optimalizálása vonatkozó gyakorlati tanácsokat. Az Azure virtuális gépeken futó SQL Server rendszerben közben azt javasoljuk, hogy továbbra is az azonos adatbázis teljesítmény javítása, amelyek a helyszíni környezetben az SQL Server vonatkozó beállítások. A teljesítmény a relációs adatbázisok egy nyilvános felhőben azonban számos tényező, például egy virtuális gép méretének és a konfigurációs adatok lemezt függ.

Az SQL Server-képek, [fontolja meg a kiépítési a VMs az Azure-portálon](virtual-machines-windows-portal-sql-server-provision.md)létrehozásakor. SQL Server VMs kiépítve az erőforrás-kezelő portál alábbi gyakorlati tanácsokat, például a tárhely konfiguráció hajtja végre.

Ez a cikk első a *legjobb* teljesítmény elérése érdekében az SQL Server Azure VMs a összpontosít. Ha a terhelést kevésbé bonyolult, nem előfordulhat minden optimalizálás, az alább felsorolt. Fontolja meg a teljesítmény igényeinek, és a terhelést mintázatok, akkor kiértékelésének eredménye a fenti ajánlást.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="quick-check-list"></a>Rövid jelölőnégyzetekkel ellátott listára

Az alábbiakban gyors jelölőnégyzetet az optimális teljesítmény eléréséhez a Azure virtuális gépeken futó SQL Server:

|Terület|Optimalizálásokat|
|---|---|
|[Virtuális mérete](#vm-size-guidance)|[DS3](virtual-machines-windows-sizes.md#standard-tier-ds-series) vagy nagyobb a SQL Enterprise edition.<br/><br/>[DS2](virtual-machines-windows-sizes.md#standard-tier-ds-series) vagy újabb SQL Standard és a webes verziója esetén.|
|[Tárhely](#storage-guidance)|[Prémium tároló](../storage/storage-premium-storage.md)használja. Szabványos tároló csak ajánlott fejlesztők/vizsgálathoz.<br/><br/>Ne a [tárterület-fiók](../storage/storage-create-storage-account.md) és az SQL Server virtuális ugyanabban a régióban.<br/><br/>Tiltsa le a Azure [geo felesleges tároló](../storage/storage-redundancy.md) (geo-replikáció a tárterület-fiók).|
|[Lemezen](#disks-guidance)|2 [P30 lemez](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) (1-naplófájlokat; 1-adatfájlok és TempDB) legalább használjon.<br/><br/>Kerülje a operációs rendszer vagy a ideiglenes lemezt adatbázis tárolási és naplózás.<br/><br/>Engedélyezés olvassa el a szolgáltatója, az adatfájlok és TempDB lemez(ek) gyorsítótárazás.<br/><br/>Ne engedélyezze a naplófájl szolgáltatója lemez(ek) gyorsítótárazás.<br/><br/>Fontos: Az SQL Server szolgáltatás leállítása az Azure virtuális lemez gyorsítótár beállításainak megváltoztatása közben.<br/><br/>Több Azure adatok lemezre nő IO az átviteli megszerezni paritásos.<br/><br/>A formátum terhelés méretű ismertetését.|
|[I/O](#io-guidance)|Adatbázis lap tömöríteni.<br/><br/>Az adatfájlok azonnali fájl inicializálni engedélyezése.<br/><br/>Korlátozása, és tiltsa le a adatbázishoz kibővítését.<br/><br/>Tiltsa le a autoshrink az adatbázishoz.<br/><br/>Helyezze a adatok lemezt, beleértve a rendszer adatbázisok minden adatbázisban.<br/><br/>Ugrás az SQL Server hiba naplókban és a nyomkövetési fájl könyvtárak adatok lemezre.<br/><br/>A telepítő alapértelmezett biztonsági mentése és az adatbázis helye.<br/><br/>Zárolt lapok engedélyezése.<br/><br/>Az SQL Server teljesítmény javítások vonatkoznak.|
|[Ez a funkció adott](#feature-specific-guidance)|Biztonsági másolatot készíthet közvetlenül blob-tárolóhoz.|

*Hogyan* és *Miért* , hogy ezek optimalizálásokat a további tudnivalókért olvassa el a részletek és a következő szakaszokban található útmutatást.

## <a name="vm-size-guidance"></a>Virtuális méret útmutató

Teljesítmény bizalmas alkalmazások esetén ajánlott az alábbi [virtuális gépeken futó méretű](virtual-machines-windows-sizes.md)használható:

- **SQL Server Enterprise Edition**: DS3 vagy újabb

- **SQL Server Standard és a webes verziója esetén**: DS2 vagy újabb


## <a name="storage-guidance"></a>Tárterület útmutató

Tartományi-sorozat (együtt DSv2-sorozat és GS-adatsor) VMs támogatási [Prémium tároló](../storage/storage-premium-storage.md). Az összes gyártási munkaterhelésekből prémium tároló ajánlott.

> [AZURE.WARNING] Szabványos tárolási eltérő késések és sávszélességre van, és csak a fejlesztői/próba munkaterhelésekből ajánlott. Gyártási munkaterhelésekből prémium tárterületet kell használni.

Ezenkívül azt javasoljuk, hogy az azonos adatközpontban, mint az SQL Server virtuális gépeken futó átadás késési idejének csökkentése hozza létre a Azure tárterület-fiókot. Tároló fiók létrehozásakor, több lemezre egységes írási sorrendje nem biztos tiltsa le geo-replikáció. Ehelyett fontolja meg, hogy egy SQL Server katasztrófa helyreállítási technológia között két Azure adatközpontokban. További tudnivalókért olvassa el a [magas rendelkezésre állásának és a helyreállítás az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-high-availability-dr.md)című témakört.

## <a name="disks-guidance"></a>Lemezen útmutató

Az Azure virtuális tartalmazza az három fő lemez típusát:

- **Operációs rendszer lemezre**: Amikor létrehoz egy Azure virtuális gép, a platform fog csatolása legalább egy lemezre (felirata a **C** meghajtó) a virtuális az operációs rendszer lemezre. Ez a lemez egy virtuális, oldal blob-tárolóban lévő tárolja.
- **Ideiglenes lemez**: Azure virtuális gépeken futó tartalmaz egy másik lemezre nevű ideiglenes lemez (a, **D**felirata: meghajtó). Ez a lemezen a csomóponton ideiglenes terület használható.
- **Adatok lemez**: akkor is csatolhat további lemezt a virtuális gép adatokat tartalmazó, és ezek kíván tárolni a tárhely, oldal BLOB.

Az alábbi szakaszok ismertetik a javaslatok ezek különböző lemezt használ.

### <a name="operating-system-disk"></a>Operációs rendszer lemez

Az operációs rendszer lemez egy virtuális indítsa el, és csatlakoztassa az operációs rendszer fut verzióként és a **C** meghajtó felirata van.

Az operációs rendszer lemezen házirend gyorsítótárazás alapértelmezés szerint **Írható/olvasható**. Teljesítmény bizalmas alkalmazások azt javasoljuk, hogy adatokat lemez helyett használhatja az operációs rendszer lemezen. Az alábbi adatokat lemezen című szakaszában olvashat.

### <a name="temporary-disk"></a>Ideiglenes lemez

Az ideiglenes meghajtót, a **D**felirata: meghajtó, nem Azure blob-tárolóhoz megőrződnek. Ne tárolja a felhasználó adatbázisfájlok vagy a felhasználó tranzakció naplófájlok a **D**: meghajtóra.

D-sorozat Dv2 adatsort, és G-sorozat VMs a következő VMs ideiglenes meghajtón SSD-alapú. Ha a terhelést helykihasználást eredményez nehéz TempDB (pl. az ideiglenes objektumok vagy összetett illesztések), TempDB tárolja a **D** meghajtó eredményezhet magasabb TempDB átviteli és alsó TempDB késés.

Prémium tároló (DS-sorozat DSv2-sorozat és GS-adatsor) támogató VMs ajánlott TempDB és/vagy pufferelési készlet bővítmények tárolja, amely támogatja a prémium tároló olvasási gyorsítótárazás engedélyezve van a lemezen. Van egy kivétel ez ajánlási; Ha a TempDB használatát írási igénylő, TempDB tárolja a helyi meghajtón **D** , amely szintén a következő gépi méretű SSD-alapú nagyobb teljesítmény érhet el.

### <a name="data-disks"></a>Adatok lemez

- **Az adatok és a naplófájlok adatok lemez használata**: legalább 2 prémium tárolási használata [P30 lemezre](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage) hol egy lemezre a napló fájlokat, míg a többi tartalmazza, az adatok fájlokat és a TempDB.

- **Lemezcsíkozás**: további átviteli, a további adatokat lemez hozzáadása és lemezcsíkozás használható. A lemez adatok számának meghatározására, szüksége hozzáadva elemezheti az adatokat, és jelentkezzen be lemez(ek) elérhető IOPS számát. Ezeket az információkat, tanulmányozza a táblát a IOPS egy [virtuális mérete](virtual-machines-windows-sizes.md) és méretét, a következő cikkben: [Prémium tárterületet használ lemezt](../storage/storage-premium-storage.md). Használja a következő következő irányelvek:

    - A Windows 8/Windows Server 2012 vagy újabb verzió használja a [Tárhelyek](https://technet.microsoft.com/library/hh831739.aspx). Csíkok méretezési az OLTP munkaterhelésekből 64 KB és az adatok raktári munkaterhelésekből 256 KB elkerülése érdekében a teljesítményre gyakorolt hatás partíciót átrendeződését miatt. Emellett meg az oszlopok száma = fizikai lemezen száma. A rendelkezésre álló tárterület méretének és a 8-nál több lemezre konfigurálásához PowerShell (nem a Kiszolgálókezelő felhasználói felület) kell használnia explicit módon beállított lemezre számát egyező oszlopok számát. [Tárhelyek](https://technet.microsoft.com/library/hh831739.aspx)konfigurálásával kapcsolatos további tudnivalókért olvassa el a [Tárhely szóközöket parancsmagok a Windows PowerShellben](https://technet.microsoft.com/library/jj851254.aspx)

    - Windows 2008 R2 vagy korábbi verziójában dinamikus lemez (OS, amelyben kötet) is használhatja és csíkok méretének mindig 64 KB. Figyelje meg, hogy a Windows 8/Windows Server 2012 kezdve elavult ezt a beállítást. Kapcsolatos információért olvassa el a támogatási utasítást a [virtuális lemez szolgáltatás állapota megy Windows tárhely kezelése API-hoz](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx).

    - Ha a terhelést nem intenzív napló, és nem szükséges külön IOPs, csak egy tárolókészlethez beállíthatja. Egyéb esetben hozzon létre két tárterület-készletek egyet a napló fájlokat, illetve egy másik tárolókészlethez az adatok fájlokat és a TempDB. A lemez társított egyes betöltés elvárásainak alapján tárolókészlethez számának meghatározására. Ne feledje, hogy különböző virtuális méretű engedélyezi-e a csatolt adatok lemezt különböző számú. További tudnivalókért lásd: a [virtuális gépeken futó méretét](virtual-machines-windows-sizes.md).

    - Ha nem használ prémium tárterület (fejlesztők/Tesztelési forgatókönyvek), az ajánlási lemezcsíkozás adatok lemezre támogatják a [virtuális mérete](virtual-machines-windows-sizes.md) legfeljebb hány hozzáadása és.

- **Gyorsítótár-házirend**: prémium tároló adatok lemezt, gyorsítótárazása olvasási az adatfájlok és TempDB szolgáltatója csak az adatok lemezen. Ha nem használ prémium tárhely, nem gyorsítótárazása bármely bármely adatok lemezen. A lemez gyorsítótárazás beállításával, tanulmányozza az alábbi témaköröket: [Set-AzureOSDisk](https://msdn.microsoft.com/library/azure/jj152847) és a [Set-AzureDataDisk](https://msdn.microsoft.com/library/azure/jj152851.aspx).

    >[AZURE.WARNING] A gyorsítótár beállításának Azure virtuális lemezre bármilyen adatbázis sérülése lehetőségét elkerülése érdekében az SQL Server szolgáltatás leállítása

- **NTFS terhelés egység mérete**: az adatok lemez formázásakor 64-KB terhelés egység mérete az adatok és a naplófájlok, valamint a TempDB használata ajánlott.

- **Lemezen projektirányítási ajánlott eljárásokról**: amikor adatok lemezen eltávolítása vagy módosítása a gyorsítótár típusának megfelelő leállítása az SQL Server szolgáltatást a módosítás alatt. Ha a gyorsítótár beállításai az operációs rendszer lemezen megváltoznak, Azure leállítja a virtuális, a gyorsítótár típusa változik, és újraindul a virtuális. Adatok lemezen gyorsítótár beállításainak megváltozásakor a virtuális nem leállítja, de az adatok lemez a módosítás alatt a virtuális le, és ezután azt visszazárni.

    >[AZURE.WARNING] Sikertelen az SQL Server szolgáltatás leállítása ezek a műveletek során okozhatják adatbázis.

## <a name="io-guidance"></a>I/O útmutató

- A legjobb eredményt prémium adathordozós érhetők el, amikor az alkalmazás és a kérések parallelize meg. Prémium tároló esetekről a IO várólista mélységet 1,-nél nagyobb, látni fogja a soros kérelmek egy téma szerint rendezett vagy kis teljesítmény nyereség (akkor is, ha a tárhely intenzív) készült. Például ez hatással lehetnek a teljesítmény elemzése eszközöket, például a SQLIO egyetlen összefűzött próba eredménye.

- Fontolja meg inkább [Adatbázis lap tömörítés](https://msdn.microsoft.com/library/cc280449.aspx) , segítheti a I/O intenzív munkaterhelésekből teljesítményének javítása. Az adatok tömörítés azonban növelheti a Processzor felhasználása az adatbázis-kiszolgáló.

- Fontolja meg a fájl azonnali inicializálni az eredeti fájlt terhelés működéséhez szükséges idő csökkentése érdekében. Azonnali fájl inicializálni kihasználhatja az SQL Server (MSSQLSERVER) szolgáltatásfiók SE_MANAGE_VOLUME_NAME megadása, és vegye fel a **Végre hangerejének karbantartási műveleteket** biztonsági házirend. Ha használja az SQL Server-platform képként Azure-, nem a **Végre hangerejének karbantartási műveleteket** biztonsági házirendek: Adja hozzá az alapértelmezett-szolgáltatási fiók (NT Service\MSSQLSERVER). Más szóval azonnali fájl inicializálni nincs engedélyezve egy SQL Server Azure platform képe. Az SQL Server-szolgáltatási fiók felvétele a **Végre hangerejének karbantartási műveleteket** biztonsági házirendet, után indítsa újra az SQL Server szolgáltatást. Biztonsági megfontolások az ezzel a szolgáltatással is okozhatja. További tudnivalókért lásd: az [Adatbázis-fájl inicializálni](https://msdn.microsoft.com/library/ms175935.aspx).

- **kibővítését** csupán a váratlan növekedés eseményre számít. Az adatok, és jelentkezzen be a NÖV kibővítését a napi szintű nem kezeli. Kibővítését használata esetén a előre nagyobb a fájl mérete kapcsoló használata.

- Ellenőrizze, hogy **autoshrink** le van tiltva, amelyek teljesítmény negatív hatással lehetnek a szükségtelen terhelést elkerülése érdekében.

- Helyezze a adatok lemezt, beleértve a rendszer adatbázisok minden adatbázisban. További tudnivalókért olvassa el a [Rendszer adatbázisok áthelyezése](https://msdn.microsoft.com/library/ms345408.aspx)című témakört.

- Ugrás az SQL Server hiba naplókban és a nyomkövetési fájl könyvtárak adatok lemezre. Ehhez az SQL Server Configuration Manager kattintson a jobb gombbal az SQL Server-példányt, és válassza a Tulajdonságok parancsot. A hiba naplókban és a nyomkövetési fájlbeállításai az **Indítási paraméterek** lapon is módosíthatók. A **Speciális** lapon a kiírása könyvtár van megadva. Az alábbi képernyőképen látható találja meg a hibák naplója indítási paraméter.

    ![SQL-hibaüzenethez képernyőképe](./media/virtual-machines-windows-sql-performance/sql_server_error_log_location.png)

- A telepítő alapértelmezett biztonsági mentése és az adatbázis helye. Ez a témakör a javaslatok használata, és végezze el a módosításokat a Tulajdonságok ablakba. Című cikkben olvashat [megtekintése és módosítása az alapértelmezett helyek adatok és a naplófájlok (SQL Server Management Studio)](https://msdn.microsoft.com/library/dd206993.aspx). Az alábbi képernyőképen mutatja be, hogy hol szeretné a módosításokat.

    ![SQL-adatok naplója és biztonsági mentése](./media/virtual-machines-windows-sql-performance/sql_server_default_data_log_backup_locations.png)

- Zárolt lapok csökkentheti IO és lapozási tevékenységek engedélyezése. További tudnivalókért olvassa el a [engedélyezése a zárolás lapok memória beállítás (Windows)](https://msdn.microsoft.com/library/ms190730.aspx)című témakört.

- Ha az SQL Server 2012 futtatja, telepítse a Service Pack 1 összesítő frissítés 10. A frissítés teljesítményproblémákat i/o-javítása tartalmaz, jelölje ki az SQL Server 2012 ideiglenes table utasítás végrehajtása esetén. Tudnivalókért lásd a [Tudásbázis-cikkünket](http://support.microsoft.com/kb/2958012).

- Fontolja meg az adatfájlokról tömörítheti, ha viszi be, illetve arról az Azure.

## <a name="feature-specific-guidance"></a>Speciális útmutatókat szolgáltatás

Bizonyos környezetekben érhetnek el további teljesítmény előnyökkel jár a speciális konfigurációs módszereket. Az alábbi lista kiemeli az SQL Server-funkciókkal, amelyek segítséget nyújtanak a jobb teljesítmény elérése érdekében:

- **Biztonsági másolat Azure tárolóhoz**: az operációs rendszert futtató Azure virtuális gépeken futó SQL Server biztonsági másolatok végrehajtásakor [SQL Server biztonsági másolat URL-címét](https://msdn.microsoft.com/library/dn435916.aspx)is használhatja. Ez a szolgáltatás érhető el az SQL Server 2012 SP1 CU2 kezdődő és ajánlott a csatlakoztatott adatok lemezt biztonsági. Amikor, biztonsági mentés és visszaállítás/az Azure tárolására, kövesse az [SQL Server biztonsági mentés ajánlott eljárások a URL-címe és hibaelhárítás, valamint az Azure tárolója tárolja biztonsági mentés visszaállítása](https://msdn.microsoft.com/library/jj919149.aspx)a megadott ajánlások. Automatizálható a biztonsági mentése [Az automatikus biztonsági mentés az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-classic-sql-automated-backup.md)használatával.

    Előtt SQL Server 2012 [SQL Server Azure eszköz biztonsági másolat](https://www.microsoft.com/download/details.aspx?id=40740)is használhatja. Ez az eszköz használatával több biztonsági csíkok célok biztonsági átviteli növelése segítséget.

- **Az SQL Server Azure-ban adatfájlok**: az új szolgáltatás, [SQL Server Azure-ban adatfájlok](https://msdn.microsoft.com/library/dn385720.aspx)érhető el az SQL Server 2014-es verziótól kezdve. Teljesítmény hasonló jellemzőkkel rendelkeznek, mint a Azure adatok lemez használata SQL Servert futtató az Azure-ban az adatfájlok használatával mutatja be.

## <a name="next-steps"></a>Következő lépések

Ha érdekli, az SQL Server és a prémium tároló felfedezése további mély, ismertető [Használata Azure prémium tárhely a virtuális gépeken futó SQL Server](virtual-machines-windows-classic-sql-server-premium-storage.md).

Biztonsággal kapcsolatos gyakorlati tanácsok olvassa el a [Biztonsági megfontolások az Azure virtuális gépeken futó SQL Server](virtual-machines-windows-sql-security.md)című témakört.

Tekintse át az SQL Server virtuális gép témakörök az [SQL Server Azure virtuális gépeken futó áttekintése](virtual-machines-windows-sql-server-iaas-overview.md).
