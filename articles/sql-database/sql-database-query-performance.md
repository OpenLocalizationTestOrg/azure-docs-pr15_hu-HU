<properties 
   pageTitle="Az SQL Azure adatbázis lekérdezési teljesítmény betekintést" 
   description="A legtöbb Processzor használata más lekérdezések lekérdezés teljesítményét figyelve azonosítja Azure SQL-adatbázis." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="08/09/2016"
   ms.author="sstein"/>

# <a name="azure-sql-database-query-performance-insight"></a>Az SQL Azure adatbázis lekérdezési teljesítmény betekintést


Kezelése, és a relációs adatbázisok teljesítménye beállítása nehéz feladat, amely jelentős szakértelmét és időpont befektetési van szükség. Lekérdezési teljesítmény betekintést lehetővé teszi, hogy kevesebb időt vesz adatbázis teljesítményét hibaelhárítási, mert a következő:

- Az adatbázisok (DTU) erőforrás-felhasználás mélyebb betekintést. 
- A legnépszerűbb lekérdezések által Processzor/időtartam/végrehajtás számát adja meg, amelyek esetleg a jobb teljesítmény elérése érdekében kell állítani.
- Az azt jelenti, hogy a részletek, a lekérdezés részletezést szövegük és erőforrás-kihasználtság előzményeinek megtekintése 
- Teljesítményének [SQL Azure-adatbázis Advisor](sql-database-advisor.md) által végzett műveleteket megjelenítő széljegyzetek beállítása  



## <a name="prerequisites"></a>Előfeltételek

- Lekérdezési teljesítmény betekintést az Azure SQL-adatbázis V12 csak érhető el.
- Lekérdezési teljesítmény betekintést igényel, hogy-e az adatbázis aktív [Lekérdezés áruházból](https://msdn.microsoft.com/library/dn817826.aspx) . Tár lekérdezés nem fut, a portálon kéri, hogy kapcsolja be.

 
## <a name="permissions"></a>Engedélyek

Az alábbi [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md) engedélyek használata a lekérdezési teljesítmény betekintést van szükség: 

- A felső erőforrás igénybe lekérdezések és a diagramok megtekintése **olvasó**, **tulajdonosa**, **közreműködői**, **SQL-adatbázis munkatársi**vagy **SQL Server közreműködői** engedélyekre van szükség. 
- Lekérdezés szövege megtekintése **tulajdonosa**, **közreműködői**, **SQL-adatbázis munkatársi**vagy **SQL Server közreműködői** engedélyekre van szükség.



## <a name="using-query-performance-insight"></a>Lekérdezési teljesítmény betekintést használatával

Lekérdezési teljesítmény betekintést könnyen használható:

- [Azure portal](https://portal.azure.com/) megnyitásához, és keresse meg a vizsgálni kívánt adatbázist. 
  - Támogatási és hibaelhárítási, a bal oldali menüben válassza az "Lekérdezési teljesítmény betekintést".
- Az első lapon tekintse át az erőforrás-használata más legnépszerűbb lekérdezések listáját.
- Jelölje ki az egyéni lekérdezés a részletek megtekintéséhez.
- Nyissa meg az [SQL Azure-adatbázis tanácsadó](sql-database-advisor.md) , és ellenőrizze, hogy-e bármely a javaslatokat.
- Csúszkákkal, vagy a zoomot ikonok megfigyelt időközének módosítása.

    ![teljesítmény irányítópultja](./media/sql-database-query-performance/performance.png)

> [AZURE.NOTE] Néhány órát, az adatok lekérdezés áruházból SQL-adatbázis lekérdezési teljesítmény háttérismeretek megadására rögzítéshez szükséges. Ha az adatbázis nincs tevékenység vagy lekérdezés áruházból nem volt aktív egy adott időszakon belül, a diagram üres lesz időszak megjelenítésekor. Ha nem fut lehetővé teheti lekérdezés áruházból bármikor.   



## <a name="review-top-cpu-consuming-queries"></a>Tekintse át a lekérdezések használata más felső Processzor

A [portál](http://portal.azure.com) tegye a következőket:

1. Tallózással keresse meg a SQL-adatbázishoz, és kattintson a **beállítások minden** > **támogatási + hibaelhárítás** > **lekérdezési teljesítmény betekintést**. 

    ![Lekérdezési teljesítmény betekintést][1]

    A legnépszerűbb lekérdezések nézet megnyitásakor, és a felső Processzor igénybe lekérdezések jelennek meg.

1. Kattintson a további információt a diagram körül.<br>A felső sor az adatbázis általános DTU % jeleníti meg, amíg az oszlopok megjelenítése a kijelölt lekérdezést a kijelölt időszakban felhasznált Processzor % (például a **múlt héten** kijelölésekor minden sáv egy napját jelenti).

    ![legnépszerűbb lekérdezések][2]

    Az alsó rács látható a lekérdezések összesített adatok jelöli.

  - Lekérdezés azonosító – belül adatbázis lekérdezés egyedi azonosítója.
  - Processzor lekérdezése kezelt időszakban (összesítő függvényt függ).
  - Időtartam lekérdezése (összesítő függvényt függ).
  - Egy adott lekérdezés végrehajtások száma.

    Jelölje be, vagy törölje az egyes lekérdezések belefoglalásához vagy kizárásához azokat a diagramról jelölőnégyzetek segítségével.

1. Ha az adatok elavult, kattintson a **frissítés** gombra.
1. Csúszkákkal és a gombok használatával megfigyelés időközének módosítása vizsgálja meg a kiugrásainak megfelelő nagyítás:  ![beállításai](./media/sql-database-query-performance/zoom.png)
1. Ha szükséges Ha azt szeretné, hogy egy másik nézetet, választhat **egyéni** lapon és beállítása:
  
  - Metrikus (Processzor, időtartam, végrehajtás darab)
  - Időközönként (utolsó 24 óra, hét, az elmúlt egy hónapban korábbi). 
  - Lekérdezések száma.
  - Összesítő függvényt.

    ![beállítások](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Egyéni lekérdezési részleteinek megtekintése

Lekérdezés részletek megtekintése:

1. Kattintson a legnépszerűbb lekérdezések listáját a lekérdezés.

    ![Részletek](./media/sql-database-query-performance/details.png)

1. A Részletek nézet megnyitásakor, és lekérdezések Processzor felhasználási/időtartam/végrehajtás számának időbeli bontásban.
1. Kattintson a további információt a diagram körül.
  - A diagram felső sort átfogó adatbázis DTU %-os jeleníti meg, és a sávok a kijelölt lekérdezés elfogyasztott Processzor %.
  - Második diagram teljes időtartama a kijelölt lekérdezés szerint jeleníti meg.
  - Alsó diagram végrehajtások száma a kijelölt lekérdezés szerint jeleníti meg.
    
    ![Lekérdezés részletei][3]

1. Tetszés szerint csúszkákkal, gombok nagyítás, vagy kattintson a **Beállítások** a lekérdezés adatok megjelenítésének testreszabása, vagy válasszon másik időtartamot.

## <a name="review-top-queries-per-duration"></a>Legnépszerűbb lekérdezések / időtartam áttekintése

A lekérdezési teljesítmény betekintést legutóbbi módosítás, hogy jelent meg, amelyek segítséget nyújtanak az esetleges szűk azonosítása két új mértékek: időtartam és a végrehajtás számát.<br>

Hosszú futó lekérdezések hosszabb források zárolása blokkolja a többi felhasználó és méretezhetőség korlátozása a legnagyobb lehetősége van. Ezek egyben a legjobb jelöltek optimalizálása.<br>

Hosszú futó lekérdezések azonosítása:

1. **Egyéni** lap megnyitása lekérdezési teljesítmény betekintést kijelölt adatbázis
1. Mértékek kell **időtartam** módosítása
1. Jelölje ki a lekérdezések és megfigyelés intervallum száma
1. Jelölje ki az összesítő függvény
  - **Összeg** teljes megfigyelés időszakban összeadja a lekérdezés-végrehajtási mindig.
  - **Max** mely végrehajtás időtartama legnagyobb egész megfigyelés időközönként lekérdezések keresése.
  - **Avg** megtalálja az összes lekérdezés végrehajtások átlagos végrehajtási ideje, és mutatja, hogy ki a következő átlagokkal tetején. 

    ![lekérdezési időtartam][4]

## <a name="review-top-queries-per-execution-count"></a>Tekintse át a legnépszerűbb lekérdezések egy adatvégrehajtás száma

Nagy számú végrehajtások előfordulhat, hogy nem kell érintő magát adatbázist és lehet, hogy a források használatát alacsony, de alkalmazás általános jelenhet meg lassú.

Egyes esetekben az nagyon magas végrehajtás száma növekszik vezethet ciklikus utakat hálózati. Ciklikus utakat jelentős teljesítménybeli hatással. Azok a fizetnie hálózati késés és alsóbb késés. 

Sok adatalapú webes helyek például erősen elérheti az adatbázis összes felhasználó kérés. Miközben a kapcsolat segít, a nagyobb hálózati forgalmának engedélyezésére egyesítését, és az adatbázis-kiszolgáló terheltsége feldolgozása negatív hatással lehet a teljesítmény.  Általános tanácsokat legyen az ciklikus utakat abszolút legkisebb.

Gyakran azonosítása végrehajtott lekérdezések ("chatty") lekérdezések:

1. **Egyéni** lap megnyitása lekérdezési teljesítmény betekintést kijelölt adatbázis
1. **Végrehajtási száma** lesz Mértékek módosítása
1. Jelölje ki a lekérdezések és megfigyelés intervallum száma

    ![lekérdezés-végrehajtási száma][5]

## <a name="understanding-performance-tuning-annotations"></a>Teljesítmény eszközbeállítási széljegyzetek ismertetése 

Miközben a terhelést a lekérdezési teljesítmény betekintést felfedezése, megfigyelheti függőleges vonal fölött, a diagram ikonjai.<br>

Ezek az ikonok széljegyzetek; teljesítményt befolyásoló [SQL Azure-adatbázis Advisor](sql-database-advisor.md)által végzett műveletek képviselik. Alapvető tudnivalók a kap rámutató jegyzetet, amelyet:

![lekérdezés széljegyzet][6]

Ha szeretne további vagy advisor ajánlási alkalmazni, kattintson a ikonra. Ennek hatására megnyílik a művelet részleteit. Ha egy aktív ajánlási azonnal paranccsal alkalmazhat.

![lekérdezés széljegyzet részletei][7]

### <a name="multiple-annotations"></a>Több széljegyzeteket. ###

Akkor lehet, hogy nagyítási szintet, mert, amelyek egymással közelébe széljegyzetek fog első összecsukott egyikébe. A speciális ikonnal képviseli, és kattintással nyissa meg a új lap, ahol csoportosított listája a széljegyzetek fog jelennek.
A terhelési jobban megértheti használatával történik a lekérdezések és a teljesítmény javítása műveletek segít. 


##  <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>A lekérdezés áruházból konfiguráció a lekérdezési teljesítmény betekintést optimalizálása

Lekérdezési teljesítmény betekintést a használata során előforduló az alábbi lekérdezés áruházból üzenetek:

- "Lekérdezés-tár nincs megfelelően konfigurálva a ebben az adatbázisban. Ide kattintva további."
- "Lekérdezés-tár nincs megfelelően konfigurálva a ebben az adatbázisban. Ide kattintva lecserélheti beállításainak. " 

Az alábbi üzenetek általában jelennek meg, ha nem tud új adatokat szeretne gyűjteni lekérdezés áruházból. 

Első esetben történik, ha lekérdezés áruházból csak olvasható állapotban van, és a paraméterek beállítása optimálisan. A lekérdezés áruházból méretének növelésével, illetve jelölésének eltávolításával a lekérdezés áruházból szerint háríthatja el.

![qds gomb][8]

Második esetben történik lekérdezés áruházból ki van kapcsolva, vagy paraméterei optimálisan nincsenek-e beállítva. <br>Rögzítése és az adatmegőrzési házirend módosítása és a lekérdezés áruházból engedélyezéséhez, az alábbi parancsok végrehajtása, vagy közvetlenül a portálon:

![qds gomb][9]

### <a name="recommended-retention-and-capture-policy"></a>Ajánlott rögzítése és az adatmegőrzési házirendek

Adatmegőrzési házirendek két típusa van:

- Méret alapú – Ha beállítása automatikus azt fogja adatok letisztázásának automatikusan elérésekor maximális méret közelében.
- Ideje alapján - azt fogja állítsa be 30 nap, ami azt jelenti, lekérdezés áruházból fog fogyni a helyet, ha törli lekérdezés adatainak 30 napnál régebbi alapértelmezés szerint

Rögzítés házirend sikerült állíthatók be:

- Az **összes** – rögzíti minden lekérdezés.
- **Automatikus** – ritka lekérdezések és lekérdezések nem jelentős fordítás és a végrehajtás időtartammal rendelkező figyelmen kívül hagyja. Végrehajtási darab, összeállítása és runtime időtartam küszöbértékei belső határozzák meg. Az alapértelmezett beállítás.
- **Nincs** – lekérdezés áruházból leállítja a rögzítés az új lekérdezésekre, azonban továbbra is szedett futtatókörnyezet stat már rögzített lekérdezések.
    
Azt javasoljuk, hogy az összes házirendek automatikus és a 30 napra TISZTÍT házirend beállítása:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
        
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));
    
    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Lekérdezés áruházból méretének növelése Ez az adatbázis kapcsolódik, és kiadása után a lekérdezés által elvégzett sikerült:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Ezek a beállítások alkalmazása végül ellenőrizze a lekérdezés áruházból, az új lekérdezésekre gyűjteni, azonban azt szeretné, hogy csak törölje lekérdezés áruházból. 
> [AZURE.NOTE] A következő lekérdezés végrehajtásakor törli a lekérdezés tárolóban lévő minden aktuális információkat. 


    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Összefoglalás

Lekérdezési teljesítmény betekintést segít megérteni a milyen következményekkel járnak a lekérdezés terhelést, és hogyan vonatkozik adatbázis erőforrás-felhasználás. Ezzel a szolgáltatással használni tudnivalók a lekérdezések használata más tetején, és könnyen azonosítani a lehetőségekből előtt válnak a probléma megoldásához.




## <a name="next-steps"></a>Következő lépések

További javaslatok az SQL-adatbázis gördülékenyebbé kapcsolatban kattintson a **Lekérdezési teljesítmény betekintést** lap [javaslat](sql-database-advisor.md) elemre.

![Teljesítmény Advisor](./media/sql-database-query-performance/ia.png)


<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png

