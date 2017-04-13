 <properties
    pageTitle="Hogyan kötegelés segítségével Azure SQL-adatbázis alkalmazás teljesítményének javítása"
    description="A témakör igazolja, hogy eljárásnak adatbázis-műveletek nagyban imroves sebességének és méretezhetőség: az SQL Azure-adatbázis alkalmazás. Habár e eljárásnak technikák bármely SQL Server-adatbázis működik, a cikk elsősorban a Azure."
    services="sql-database"
    documentationCenter="na"
    authors="annemill"
    manager="jhubbard"
    editor="" />


<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="07/12/2016"
    ms.author="annemill" />

# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a>Kötegelés használata SQL-adatbázis alkalmazás teljesítmény javítása érdekében

Azure SQL-adatbázishoz műveletek jelentősen kötegelés javítja a teljesítmény és méretezhetőség: az alkalmazás. Annak érdekében, hogy a előnyeinek ismertetése, első része ez a cikk bemutatja, néhány példa teszteredményei, amely összehasonlítása egymás után következő és kötegelt kérések SQL-adatbázishoz. A cikk hátralévő technikákat, esetek, és sikeresen kötegelés az Azure alkalmazásokban használandó segít kapcsolatos szempontok jeleníti meg.

**Szerző**: Jason Roth urat, Silvano Coriani, Trent Swanson (teljes méret 180 Inc.)

**Véleményező**: Conor Cunningham, Thomassy Lukács

## <a name="why-is-batching-important-for-sql-database"></a>Miért van kötegelés fontos SQL-adatbázis?
Hívások átirányítása a távoli szolgáltatás kötegelés teljesítmény és méretezhetőség növelése jól ismert stratégiát is. Minden olyan tevékenységre távoli szolgáltatással, például szerializálási, hálózati átvitel és deszerializálás feldolgozási költségek orvosolja. Sok külön tranzakciókat be egy kötegben kis méretűre állítása ezeket a költségeket.

Ebben a cikkben szeretnénk vizsgálja meg a stratégiák és -esetek kötegelés különböző SQL-adatbázishoz. Habár e stratégiák is fontos a helyszíni alkalmazások által használt SQL Server, létezik kötegelés SQL-adatbázis használata kiemelés számos oka lehet:

- Van esetleg nagyobb hálózati késés SQL-adatbázis eléréséhez különösen akkor, ha az elérni kívánt SQL-adatbázis kívüli ugyanazt a Microsoft Azure-adatközponthoz.
- SQL-adatbázis multitenant jellemzői azt jelenti, hogy az adatok access réteg felel meg az adatbázis általános méretezhetőség hatékonyságát. SQL-adatbázis kell megakadályozhatja, hogy az az egyes bérlői felhasználók adatbázis-erőforrások más bérlők hátrányára legaktívabbak. SQL-adatbázis használatát, nagyobb előre definiált kvóták válaszul, csökkentheti a átviteli vagy válasz szabályozása kivételek. Hatékonyság kötegelés, például lehetővé teszik az SQL-adatbázis több munkához e határértékek elérése előtt. 
- Kötegelés akkor is, amelyek több adatbázis (sharding) architektúrákban hatékony. A minden adatbázisban egység interakció hatékonyságát még mindig az általános méretezhetőség kulcsfontosságú tényező. 

SQL-adatbázis használatának előnyeiről a egyik, hogy nem kell kezelni a kiszolgálókat, az adatbázist. Azonban a felügyelt infrastruktúra is azt jelenti, hogy adatbázis optimalizálásokat másképp megfontolni. Megtekintheti az adatbázis hardvereszközt vagy hálózati infrastruktúrát javítása nem. Microsoft Azure határozza meg azokat a környezetben. A fő terület, megadhatja, hogy, hogy az alkalmazás hogyan kommunikáljon SQL-adatbázishoz. Kötegelés az egyik alábbi optimalizálásokat. 

A papír első része megvizsgálja a .NET-alkalmazások által használt SQL-adatbázis eljárásnak különféle technikákat. Az utolsó két szakaszok eljárásnak irányelvek és -esetek tárgyalja.

## <a name="batching-strategies"></a>Stratégiák kötegelés

### <a name="note-about-timing-results-in-this-topic"></a>Megjegyzés: Ez a témakör az időzítés eredmény
>[AZURE.NOTE] Eredményt nem szintek, de van kialakítva, hogy **relatív teljesítmény**ábrázolására. Az időzítés legalább 10 próba futtatása esetén átlagosan alapulnak. Műveletek szúr be egy üres táblázatba. Ezek a vizsgálatok volt mért előtti-V12, és ezek nem feltétlenül egyeznek meg, amelyek használatával az új [szolgáltatási rétegek](sql-database-service-tiers.md)V12 adatbázisban esetleg tapasztalható kapacitása. A eljárásnak technika relatív előnyeit hasonló kell lennie.

### <a name="transactions"></a>Tranzakciók
Úgy tűnik meglepő által tranzakciók tárgyal kötegelés ismerkedést megkezdéséhez. De ügyféloldali tranzakciók felhasználása finom kiszolgálóoldali eljárásnak hatása van, amely javítja a teljesítményt. És a tranzakciók hozzáadhatók csak néhány kódsorokat, így gyorsan egymást követő műveletek a teljesítmény javítása érdekében biztosítanak.

Fontolja meg a következő C# kód, amely tartalmazza a Beszúrás sorozatában, és frissítse a műveleteket egy egyszerű táblázat.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

A következő ADO.NET-kódot egymás után ezeket a műveleteket hajt végre.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
    
        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Legegyszerűbben úgy optimalizálja a kód valamilyen ügyféloldali kötegelés a hívásokat a végrehajtásához. De van egy egyszerű módszer egyszerűen tördelése hívások sorozata tranzakció kód teljesítményének növeléséhez. Az alábbiakban a ugyanazt kódot, amely a tranzakciók használja.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();
    
        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }
    
        transaction.Commit();
    }

Tranzakciók mind az alábbi példák ténylegesen használatban vannak. A példában szereplő minden egyes hívás egy implicit tranzakciók. A második példában egy explicit tranzakció fussa összes a hívásokat. Az [írási javaslatokat megjelenítő tranzakciónaplója](https://msdn.microsoft.com/library/ms186259.aspx)dokumentációjában per naplóbejegyzések tartalmát a lemez, amikor a tranzakciók érvényesítése. Több hívást fogadni tranzakció felvételével, így a tranzakciónaplója az írás késleltetheti mindaddig, amíg a tranzakció. Gyakorlatilag azt mondja engedélyezi az írás a kiszolgáló tranzakciónaplója kötegelés.

A következő táblázat mutatja egyes alkalmi vizsgálati eredmények. Tesztek azonos egymás után következő beszúrása nélkül tranzakcióinak és elvégzett. További szempont az első készlet vizsgálatok futtatta távolról hordozható számítógépről a Microsoft Azure-adatbázishoz. A második készlete vizsgálatok futtatta egy felhőalapú szolgáltatás és az adatbázis, hogy mindkét tartózkodott belül a ugyanazt a Microsoft Azure-adatközponthoz (nyugati Egyesült Államokbeli). A következő táblázat mutatja az időtartam, egymás után következő beszúrása nélkül tranzakciók és.

**A helyszíni az Azure**:

| Műveletek | Nincs tranzakció (ms) | Transaction (ms) |
|---|---|---|
| 1 | 130 | 402 |
| 10 | 1208 | 1226 |
| 100 | 12662 | 10395 |
| 1000 | 128852 | 102917 |


**Azure az Azure (azonos adatközponthoz)**:

| Műveletek | Nincs tranzakció (ms) | Transaction (ms) |
|---|---|---|
| 1 | 21 | 26 |
| 10 | 220 | 56 |
| 100 | 2145 | 341 |
| 1000 | 21479 | 2756 |

>[AZURE.NOTE] Eredmények nem szintek állnak. Lásd az [Ebben a témakörben időzítés eredmények megjegyzést](#note-about-timing-results-in-this-topic).

Az előző vizsgálati eredmények alapján, valójában csökkenti egyetlen művelet tördelése tranzakció teljesítményét. De egyetlen tranzakción belül műveletek számának növelésekor a teljesítmény fokozása több lesz-e megjelölve. A teljesítmény különbség is további észrevehető összes művelet végrehajtásakor belül a Microsoft Azure-adatközponthoz. SQL-adatbázis használata a Microsoft Azure adatközponthoz kívüli megnövelt késleltetése tranzakciók használata a teljesítmény nyereség overshadows.

Noha a tranzakciók használata növelheti a teljesítményt, továbbra is [vegye figyelembe a gyakorlati tanácsok a tranzakciók és a kapcsolatokkal](https://msdn.microsoft.com/library/ms187484.aspx). A lehető legrövidebb tranzakció hagyja, és zárja be az adatbázis-kapcsolatot, a munka befejezése után. A utasítással az előző példában biztosítja, hogy a kapcsolat a későbbi kód blokk befejezésekor nincs megnyitva.

Az előző példa bemutatja, hogyan vehet fel egy helyi tranzakció bármely ADO.NET-kód két sorának együtt. Tranzakcióinak kínálnak gyorsan kód, amely lehetővé teszi a egymás után következő beszúrási, frissítési és törlési művelet a teljesítmény javítása érdekében. Azonban a leggyorsabb teljesítményt, érdemes megfontolni az ügyféloldali kötegelés, például táblázat értékű paraméterek előnyeinek kihasználása tovább a kód megváltoztatása.

ADO.NET-tranzakciók kapcsolatos további tudnivalókért olvassa el a [Helyi tranzakcióinak ADO.NET](https://msdn.microsoft.com/library/vstudio/2k2hy99x.aspx)című témakört.

### <a name="table-valued-parameters"></a>Táblázat értékű paraméterei
Táblázat értékű paraméterek a Transact-SQL-utasítások, tárolt eljárások és a függvények paraméterként felhasználó által definiált táblázatban támogatnak. Ez ügyféloldali eljárásnak módszer lehetővé teszi, hogy küldje el a táblázat értékű paraméter belül több adatsort. Táblázat értékű paramétereket használ, először definiálni egy táblázat típusú. A következő Transact-SQL-utasítást létrehoz egy **MyTableType**nevű táblázat típusú.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );
 

A kód a pontos ugyanaz a neve és a táblázat típusú típusú hoz létre **adattábla** . A szöveg lekérdezésben paraméter adják át a **adattábla** vagy a tárolt eljárás hívása. A következő példa bemutatja az Ez a módszer:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }
    
        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);
                    
        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });
    
        cmd.ExecuteNonQuery();
    }

Az előző példában a **SqlCommand** objektum sorok beszúrása és táblázat értékű paraméter **@TestTvp**. A korábban létrehozott **adattábla** objektum ezt a paramétert a **SqlCommand.Parameters.Add** módszer van rendelve. A teljesítmény a szúr be egy hívás jelentősen kötegelés megnöveli egymás után következő beszúrása fölé.

Az előző példában további javítása, használja a tárolt eljárás szöveges parancs helyett. A következő Transact-SQL-parancs hoz létre a tárolt eljárás, amely a táblázat értékű **SimpleTestTableType** paramétert.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Módosítsa a **SqlCommand** objektum deklaráció az előző példa az alábbi.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

A legtöbb esetben a táblázat értékű paraméterek van más eljárásnak technikákkal egyenértékű vagy jobb teljesítményt. Táblázat értékű paraméterek gyakran helyett, mivel rugalmasabb, mint az egyéb beállításokat. Más technikákkal, például SQL tömeges példányát, például csak az új sorok beszúrása teszi lehetővé. De táblázat értékű paraméterekkel segítségével logika a tárolt eljárás megállapítható, hogy mely sorai frissítések, és amelyek szúrja be. A táblázat típusú tartalmaz egy "Művelet" oszlop, amely jelzi, hogy az adott sor kell beszúrt, frissíthetők, vagy törölhetők is módosíthatja.

A következő táblázat mutatja a táblázat értékű paraméterek használatára vonatkozó alkalmi vizsgálati eredmények ezredmásodpercben.

| Műveletek | A helyszíni az Azure (ms)  | Azure azonos adatközponthoz (ms) |
|---|---|---|
| 1 | 124 | 32 |
| 10 | 131 | 25 |
| 100 | 338 | 51 |
| 1000 | 2615 | 382 |
| 10000 | 23830 | 3586 |

>[AZURE.NOTE] Eredmények nem szintek állnak. Lásd az [Ebben a témakörben időzítés eredmények megjegyzést](#note-about-timing-results-in-this-topic).

A teljesítmény nyereség kötegelés a rendszer azonnal látható. Az előző egymás után következő feltétel 1000 műveletek tartott 129 másodperc kívül a adatközponthoz és a adatközponthoz belül a 21 másodpercre. De táblázat értékű paraméterekkel 1000 műveletek csak 2.6 másodperc kívül a adatközponthoz és idõtartamtól belül az Adatközpont.

Táblázat értékű paraméterek kapcsolatos további tudnivalókért olvassa el a [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb510489.aspx)című témakört.

### <a name="sql-bulk-copy"></a>SQL-tömeges másolása
SQL tömeges példány úgy is nagy mennyiségű adat beillesztése a célul szolgáló adatbázis. .NET-alkalmazásokat a **SqlBulkCopy** osztály segítségével tömeges beszúrás hajtanak végre. **SqlBulkCopy** hasonlít a függvény a parancssori, **Bcp.exe**, vagy a Transact-SQL-utasítást, **TÖMEGES BESZÚRÁSA**. Az alábbi példa mutatja tömeges másolása a sorok a forrásban **adattábla**, tábla, a célként használt táblát az SQL Server, a táblanév.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Vannak bizonyos esetekben, amikor indexeinek előnyben részesített táblázat értékű paraméterek fölé. Összehasonlító táblázatunkból műveletek TÖMEGES BESZÚRÁSA a következő témakörben [Table-Valued paramétereket](https://msdn.microsoft.com/library/bb510489.aspx)és táblázat értékű paramétert.

A következő alkalmi vizsgálati eredmények megjelenítése ezredmásodpercben az **SqlBulkCopy** kötegelés teljesítményét.

| Műveletek | A helyszíni az Azure (ms)  | Azure azonos adatközponthoz (ms) |
|---|---|---|
| 1 | 433 | 57 |
| 10 | 441 | 32 |
| 100 | 636 | 53 |
| 1000 | 2535 | 341 |
| 10000 | 21605 | 2737 |

>[AZURE.NOTE] Eredmények nem szintek állnak. Lásd az [Ebben a témakörben időzítés eredmények megjegyzést](#note-about-timing-results-in-this-topic).

A köteg kisebb méretű a használati táblázat értékű paraméterek outperformed **SqlBulkCopy** osztály. **SqlBulkCopy** azonban végre 12 – 31 %-kal gyorsabb táblázat értékű paraméterek mint 1000 és 10 000 sorok tesztek. Táblázat értékű paraméterek, például **SqlBulkCopy** így egy jó kötegelt szúr be, különösen akkor, ha a teljesítmény műveletek nem kötegelt képest.

ADO.NET tömeges példányban kapcsolatos további tudnivalókért olvassa el a [Tömeges másolás műveletek az SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx)című témakört.

### <a name="multiple-row-parameterized-insert-statements"></a>Több sor BESZÚRÁSA paraméteres kimutatások
A kisebb kötegekben egy alternatív nagy paraméteres utasítást, amely több sor beszúrása összeállításához. A következő példa bemutatja ezt a módszert.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();
    
        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";
    
        SqlCommand cmd = new SqlCommand(insertCommand, connection);
    
        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }
    
        cmd.ExecuteNonQuery();
    }
 

Ez a példa kattintva jelenítse meg a egyszerű fogalma jelenti. További reálisan példa volna ismétlése a lekérdezési karakterlánc és a parancs paraméterek egyidejű összeállításához a szükséges szervezetek. Ön 2100 lekérdezés paraméterei, összesen korlátozott ez korlátozza a sorokat, amelyeket ily módon feldolgozhatók száma.

A következő alkalmi vizsgálati eredmények utasítás a következő típusú ellátása ezredmásodpercben megjelenítése

| Műveletek | Táblázat értékű paraméterek (ms) | Egyetlen-utasítás BESZÚRÁSA (ms) |
|---|---|---|
| 1 | 32 | 20 |
| 10 | 30 | 25 |
| 100 | 33 | 51 |

>[AZURE.NOTE] Eredmények nem szintek állnak. Lásd az [Ebben a témakörben időzítés eredmények megjegyzést](#note-about-timing-results-in-this-topic).

Lehet, hogy ezt a megközelítést némileg gyorsabb tételek, amelyek 100-nál kevesebb sorokat. Bár a fokozása kicsi, ez a módszer akkor a másik lehetőség, amely jól az adott alkalmazás helyzetben működhet.

### <a name="dataadapter"></a>DataAdapter
A **DataAdapter** osztály lehetővé teszi, hogy egy **adatkészlet** objektum módosítása, és küldje, BESZÚRÁSA, frissítése és törlése a műveleteket a módosításokat. Alkalmazás használatakor a **DataAdapter** ily módon, fontos tudni, hogy a hívások külön az egyes különböző művelet álljanak. A teljesítmény javítása érdekében célszerű lehet kötegelt egyszerre műveletek számú **UpdateBatchSize** tulajdonságot használja. További tudnivalókért lásd: [Elvégzéséhez köteg műveletek használata DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Személy keretrendszer
Személy keretrendszer jelenleg nem támogatja kötegelés. A Közösség különböző fejlesztők próbálta megoldások, például a **létrehozva** módszer felülbírálása bemutatására. De megoldásokkal általában összetett és az alkalmazás és az adatmodell testre szabott. A szervezet keretrendszer Codeplex webhelyen a project által jelzett vitafórum lap Ez a szolgáltatás kérésének megfelelően. A vitafórum megtekintéséhez, lásd: [Tervezési értekezleti feljegyzések – 2012 augusztus 2](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
Teljesség érdekében azt úgy érzi, hogy az XML, mint egy eljárásnak stratégia beszélgetni fontos. Az XML használatának azonban semmilyen más módszerekkel előnye és több hátrányai rendelkezik. A megoldás táblázat értékű paraméterek hasonló, de egy XML-fájl vagy a karakterlánc helyett a felhasználó által definiált táblázatban a tárolt eljárás átadott. A tárolt eljárás elemzi a tárolt eljárás parancsa.

Vannak ezt a megközelítést számos hátránya:

- XML használata a lehető lehet és jobban hiba.
- Az XML-adatbázisban elemzése lehet Processzor-igényes.
- A legtöbb esetben ez a módszer lassabb paraméterek táblázat értékű.

Az alábbi okai lehetnek az XML használatának köteg lekérdezések nem ajánlott.

## <a name="batching-considerations"></a>Kötegelés kapcsolatos szempontok
A következő szakaszokban további útmutatást a kötegelés az SQL-adatbázis-alkalmazások használatát.

### <a name="tradeoffs"></a>Kompromisszumok
Attól függően, hogy a architektúra kötegelés is magában foglalja a teljesítmény és tűrőképessége között útján. Tegyük fel hogy az alkalmazási példát, amelyben a szerepkör váratlanul megszakad. Ha megszakad egy sornyi adatot, a hatása kisebb, mint az el nem küldött sorok nagy köteg elvesztése hatását. Ha egy adott időben ablakban az adatbázis küldés előtt a sorok puffer, nincs nagyobb kockázat.

Ez a útján miatt kiértékelésének eredménye műveletek típusú, tétel. A köteg agresszívabb (nagyobb tételek és hosszabb idő windows), hogy a kevésbé fontos adatokkal.

### <a name="batch-size"></a>Köteg mérete
A vizsgálatok volt a szokásos nincs előnye a nagyobb tételek megszakítása kisebb szövegadattömb be. A felosztása gyakran valójában lassabban, mint egy nagy kötegben elküldése eredményeként. Például érdemes megvizsgálni az esetet, amelyhez 1000 sorok beszúrása. A következő táblázat mutatja, hogy mennyi ideig tart táblázat értékű paraméterek használata esetén a kisebb kötegekben osztva 1000 sorok beszúrása.

| Köteg mérete | Iterációk száma | Táblázat értékű paraméterek (ms) |
| -------- | --- | --- |
| 1000 | 1 | 347 |
| 500 | 2 | 355 |
| 100 | 10 | 465 |
| 50 | 20 | 630 |

>[AZURE.NOTE] Eredmények nem szintek állnak. Lásd az [Ebben a témakörben időzítés eredmények megjegyzést](#note-about-timing-results-in-this-topic).

Látható, hogy-e a legjobb teljesítményt 1000 sorok egyszerre küldhetik őket. Más vizsgálatok (Itt ez nem látható) volt a teljesítmény kis nyereség 10000 sor köteg szétválasztani 5000 két csoportba szeretne. De az alábbi teszteket a táblázat séma viszonylag egyszerű, így kell végezze el a teszteket a meghatározott adatokat, és ellenőrizze ezen eredmények köteg méretét.

Más szempont az, hogy az, hogy ha a teljes köteg túl nagy, SQL-adatbázis előfordulhat, hogy szabályozása és véglegesítse a köteg elutasítja. A legjobb tesztelje az adott forgatókönyvek állapítsa meg, van-e egy ideális tétel méretét. Ellenőrizze a tétel méretét konfigurálható futásidőben ahhoz, hogy gyors korrekciók teljesítmény vagy hibák alapján.

A köteg méretének végül a kötegelés járó kockázatokat egyenlege. Ha van ideiglenes (tranziens), vagy hiba jelenik meg a szerepkört, fontolja meg a következmények, a művelet újra próbálkozik, illetve az adatvesztés a köteg.

### <a name="parallel-processing"></a>A párhuzamos feldolgozása
Mi történik, ha a köteg méretének csökkentése megközelítés tartott, de több beszélgetésekben a munka végrehajtásához használt? Ismét a vizsgálatok kiderült, hogy több többszálas kisebb kötegekben általában elvégzett egy nagyobb kötegben rosszabb. A következő teszt megkísérli 1000 sorok beszúrása egy vagy több párhuzamos kötegekben történik. A tesztet jeleníti meg, hogyan további egyidejű kötegekben történik ténylegesen csökkent teljesítményét.

| Tétel méretét [közelítések] | Két szálak (ms) | Négy szálak (ms) | Hat szálak (ms) |
| -------- | --- | --- | --- |
| [1] 1000 | 277 | 315 | 266 |
| 500 [2] | 548 | 278 | 256 |
| 250 [4] | 405 | 329 | 265. |
| 100 [10] | 488 | 439 | 391 |

>[AZURE.NOTE] Eredmények nem szintek állnak. Lásd az [Ebben a témakörben időzítés eredmények megjegyzést](#note-about-timing-results-in-this-topic).

Létezik a teljesítmény miatt párhuzamos csökkenés több lehetséges oka:

- Vannak több egyidejű hálózati hívások egy helyett.
- Több művelet egy táblát ellen kérelem, így akadályozva eredményez.
- Vannak olyan társított költségek többszálas.
- Több kapcsolat megnyitása a költség ez fontosabb, mint a párhuzamos feldolgozási előnyeit.

Különböző táblákból vagy adatbázisok célként, érdemes megállapíthatja, hogy bizonyos szerezhet a stratégia a teljesítmény. Adatbázis sharding vagy szövetségeire ezt a megközelítést példa lenne. Sharding több adatbázisok használ, és különböző adatbázisonként továbbítja. Ha minden kis köteg adatbázisváltás fog, hatékonyabbá tehető majd párhuzamosan a műveletek elvégzéséhez. A teljesítmény nyereség viszont nem elég jelentős döntés alapjául használandó adatbázis sharding használata a megoldás.

Az egyes látványtervek kisebb kötegekben párhuzamos végrehajtását eredményezhet továbbfejlesztett kapacitásának terhelés alatt a rendszer. Ebben az esetben akkor is, ha gyorsabban egy nagyobb kötegben feldolgozása, párhuzamosan több kötegekben feldolgozása lehet hatékonyabban.

Ha párhuzamos végrehajtás, érdemes szabályozása dolgozó szálak maximális számát. Kisebb szám gyorsabb végrehajtási idő, és kevesebb kérelem eredményezhet. További be, hogy ez elhelyezi a céladatbázisban, kapcsolatok és a tranzakciók egyaránt célszerű is.

### <a name="related-performance-factors"></a>Kapcsolódó teljesítmény tényezői
Az adatbázisok teljesítményének tipikus útmutatást is érinti kötegelés. Ha például beszúrása táblázatok, amelyekhez nagy elsődleges kulcsnak vagy sok fürtözött indexek csökken a teljesítmény.

Ha a táblázat értékű paramétereket használ a tárolt eljárás, paranccsal a **NOCOUNT ÁLLÍTSA be** az eljárást az elején. Ez az utasítás nem jelenik meg a visszaküldése az eljárás a szóban forgó sorok számát. Azonban a vizsgálatok, a **SET NOCOUNT ON** e vagy nem hatást vagy teljesítménye csökkent. A próba tárolt eljárás lett egyszerű, egy egyetlen **BESZÚRÁSA** parancsot, és a táblázat értékű paraméter. Akkor lehet, hogy összetettebb tárolt eljárások Ez az utasítás előnyös. De nem feltételezik, hogy a **SET NOCOUNT ON** automatikusan hozzáadja a tárolt eljárás javítja teljesítményét. Az effektus megértéséhez, tesztelje a tárolt eljárás, és a **SET NOCOUNT ON** utasítást nélkül.

## <a name="batching-scenarios"></a>Alkalmazási helyzetek kötegelés
Az alábbi szakaszok ismertetik, hogyan lehet táblázat értékű paraméterek használata három alkalmazás helyzetekben. Az első forgatókönyv bemutatja, hogyan pufferelési kötegelés is működik együtt. A második forgatókönyv egyetlen tárolt eljárás hívás főadat – részletek műveletek végrehajtásával javítja a teljesítményt. A végleges forgatókönyv táblázat értékű paraméterek használata a "UPSERT" művelet mutatja.

### <a name="buffering"></a>Pufferelési
Bizonyos esetekben, amelyek egyértelmű jelölt kötegelés ugyan, forgatókönyv sok sikerült kihasználhatja kötegelés késleltetett feldolgozás szerint. Késedelmes feldolgozás is, hogy az adatok elvész megelőzve váratlan hiba történt nagyobb kockázat végzi. Fontos, hogy a kockázat, és vegye figyelembe a következmények.

Például érdemes megvizsgálni webalkalmazás nyomon követi a navigációs előzmények minden felhasználójának. Minden egyes lap kérésének megfelelően az alkalmazás teheti egy adatbázist, hívja fel a felhasználót a lap nézet felvételéhez. De nagyobb teljesítmény és méretezhetőség: úgy, hogy a felhasználók navigációs tevékenységek pufferelési, és elküldi ezeket az adatokat az adatbázis kötegekben lehet elérni. Az adatbázis-frissítés eltelt idő és/vagy pufferelési méret szerint válthat. Ha például egy szabály megadhatja, hogy a köteg után 20 másodpercre, illetve ha a puffer elér egy 1000 elemek lehet feldolgozni.

Az alábbi kód példában [Reaktív bővítmények - Rx](https://msdn.microsoft.com/data/gg577609) felügyeleti osztály által felvetett pufferolt események feldolgozása. Amikor beírja a puffer, illetve nem időtúllépését, a felhasználói adatok a köteg a szolgáltatás elküldi a táblázat értékű paraméter az adatbázist.

Az alábbi NavHistoryData osztály modellek a felhasználó navigációs adatai. Alapvető információk, például a felhasználói azonosító, az URL-cím elérhető és az access idő tartalmaz.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

A NavHistoryDataMonitor osztály feladata pufferelési a felhasználói navigációs adatokat az adatbázishoz. A módszer, RecordUserNavigationEntry, amely **OnAdded** esemény emelése válaszol tartalmaz. A következő kódot a konstruktor logika hozzon létre egy kezelt az esemény alapján Rx használó jeleníti meg. Majd előfizetője a kezelt gyűjtemény pufferelési módszerrel. A túlterhelés Itt adhatja meg, hogy a puffer 20 másodpercenként vagy 1000 bejegyzéseket is szerepel.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

A kezelő összes pufferolt elemet alakítja a táblázat értékű típusát, és átadja elemtípushoz a tárolt eljárás, hogy a köteg dolgozza fel. A következő kódot teljes definíciója a NavHistoryDataEventArgs és a NavHistoryDataMonitor osztályok is látható.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }
    
    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;
    
        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");
    
            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }
    
        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }
    
        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }
    
            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();
    
                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;
    
                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });
    
                cmd.ExecuteNonQuery();
            }
        }
    }

Pufferelési osztály használatához az alkalmazás létrehoz egy statikus NavHistoryDataMonitor-objektumot. Minden alkalommal, amikor a felhasználó hozzáfér egy oldalra, a hívások a NavHistoryDataMonitor.RecordUserNavigationEntry módszer az alkalmazás. Pufferelési logika gondoskodik a következő bejegyzéseket küld az adatbázis kötegekben folytatódik.

### <a name="master-detail"></a>Fő részletei
Táblázat értékű paraméterek hasznosak egyszerű Beszúrás esetek. Azonban lehet megnehezítheti a köteg szúr be, amely magában foglalja a egynél több táblát. A "főadat/részletek" forgatókönyv jó példa. A fő táblázat azonosítja az elsődleges entitás. Egy vagy több részlet táblát a szervezet további adatait tárolja. Ebben az esetben a külső kulcsú kapcsolatok hivatkozási a kapcsolat adatait egy egyedi fő személyhez. Fontolja meg egy PurchaseOrder és a kapcsolódó OrderDetail tábla egyszerűsített verziója. A Transact-SQL hoz létre a PurchaseOrder tábla négy oszlopot: Rendeléskód, az orderdate (RendelésDátuma), a CustomerID és a állapotát.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Egyes megrendelésekre vonatkozóan egy vagy több termék vásárlások tartalmazza. Ezt az információt a PurchaseOrderDetail táblázatban rögzíthető. A következő Transact-SQL nyelvben öt oszlop létrehozza a PurchaseOrderDetail táblát: Rendeléskód, a OrderDetailID, a termékazonosító, a Listaár és a OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

A Rendeléskód oszlopban a PurchaseOrderDetail táblázatban a PurchaseOrder táblából megrendelés hivatkoznia kell. Idegen kulcs a következő definíció kényszeríti ezt a korlátot.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

Táblázat értékű paraméterek használatához, egy felhasználó által definiált táblázat típusú minden cél táblához kell rendelkeznie.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO
    
    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Adja meg a tárolt eljárás, amely ilyen típusú táblák elfogadja. Ez az eljárás lehetővé teszi, hogy az alkalmazás helyileg köteg rendelések és a rendelés részletei egyetlen hívás. A Transact-SQL biztosít a teljes tárolt eljárás nyilatkozat vásárlás sorrendben példában.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;
    
    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );
     
          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;
    
    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;
    
    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

Ebben a példában a helyi meghajtóra definiált @IdentityLink tábla tárolja az imént beszúrt sorok tényleges Rendeléskód értékeit. Ezek sorrend azonosítók eltérnek az ideiglenes Rendeléskód értékeket a @orders és @details paraméterek táblázat értékű. Emiatt a @IdentityLink táblázatot, majd a Rendeléskód értékeit csatlakozik a @orders a valós Rendeléskód értékeket az új táblázatsorok PurchaseOrder paramétert. Ezt a lépést követően a @IdentityLink megkönnyítheti a táblázat beszúrása a rendelés részletei és a tényleges Rendeléskód, amely megfelel az idegen kulcs korlátozást.

Ez a tárolt eljárás is használható, kódot vagy más Transact-SQL-hívások. Olvassa el a papír egy példa táblázat értékű paraméterek szakaszát. A következő Transact-SQL nyelvben hívja fel a sp_InsertOrdersBatch mutatja.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType
    
    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')
    
    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)
    
    exec sp_InsertOrdersBatch @orders, @details

Ez a megoldás lehetővé teszi, hogy a Rendeléskód értéke 1 kezdődő szabálykészletet használ az egyes köteget. Az ideiglenes Rendeléskód értékek leírják a kapcsolatok, a köteg, de a tényleges Rendeléskód értékek határozzák meg a beillesztési művelet idején. A befejezésre ugyanazt a kimutatásokban az előző példában, és egyedi rendelések létrehozása az adatbázisban. Ezért érdemes megfontolni, hogy megakadályozza, hogy az ismétlődő rendelések használatakor ez a módszer kötegelés további kódot vagy az adatbázis logika hozzáadását.

Ez a példa bemutatja, hogy még összetettebb adatbázis-műveletek diaminta részletez műveletek, például is lehet kötegelt táblázat értékű paramétereket alkalmaz a.

### <a name="upsert"></a>UPSERT
Egy másik eljárásnak forgatókönyv magában foglalja a egyidejű frissítése a meglévő sor- és új sorok beszúrása. Ez a művelet előfordul, hogy egy "UPSERT" (frissítés + insert) művelet nevezik. Külön híváskezdeményezési BESZÚRHAT és FRISSÍTHET, hanem az EGYESÍTÉS utasítás ezt a feladatot a legmegfelelőbb. Az EGYESÍTÉS utasítás végrehajtása mindkét beszúrása és a frissítési egyetlen hívás műveletek.

Táblázat értékű paraméterek kínál az EGYESÍTÉS utasítás frissítések és beszúrása hajthat végre. Például érdemes megvizsgálni egyszerűsített alkalmazott táblázat, amely az alábbi oszlopokat tartalmazza: Alkalmazottkód, az Utónév, a Vezetéknév, a SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))
 
Ez a példa arra, hogy a SocialSecurityNumber egyedi KÖRLEVELEK készítése a több alkalmazott végrehajtásához is használhatja. A felhasználó által definiált táblázat típusú először létre:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Ezután a tárolt eljárás létrehozása, vagy a frissítés végrehajtása, és szúrja be a egyesítési utasítást használja, kódírás. Az alábbi példa a KÖRLEVÉL utasítást használja, a táblázat értékű paraméter @employees, EmployeeTableType típusú. A tartalmát a @employees nem találhatók itt.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

További információ című dokumentáció az EGYESÍTÉS utasítás példákat is tartalmaz. Bár az azonos munka sikerült elvégezni egy többlépéses tárolt eljáráshívás a külön, BESZÚRÁSA, és frissítési műveletek, az EGYESÍTÉS utasítás hatékonyabban. Az adatbázis kódja szerkezetét is közvetlenül anélkül, két adatbázis hívások BESZÚRÁSA és a FRISSÍTÉST az EGYESÍTÉS utasítás használó Transact-SQL-hívások.

## <a name="recommendation-summary"></a>Javaslat összefoglalása

A következő lista ismerteti az ebben a témakörben tárgyalt eljárásnak ajánlások összefoglalása:

- Pufferelési és kötegelés segítségével növelje a teljesítmény és méretezhetőség: az SQL-adatbázis-alkalmazások.
- Ismerje meg a kompromisszumok kötegelés/pufferelési és tűrőképessége között. Szerepkör hiba során a kockázatot egy feldolgozatlan köteg fontos üzleti adatok elvesztése kötegelés teljesítmény előnyeit előfordulhat, hogy lényegesebbnek.
- Megkísérli a késés csökkentése egy egyetlen adatközponthoz belül az adatbázis összes hívások nyomon.
- Ha úgy dönt, hogy egyetlen eljárásnak technika táblázat értékű paraméterek ajánlja fel, a legjobb teljesítményt és rugalmasság.
- A leggyorsabb Beszúrás teljesítménye érdekében kövesse az alábbi általános irányelveket, de az igényektől tesztelése:
    - Sorok < 100 egy egyetlen paraméteres Beszúrás paranccsal.
    - Sorok < 1000 a táblázat értékű paraméterek használata.
    - A > = 1000 sorok, SqlBulkCopy használja.
- A frissítése és törlése a műveletek, táblázat értékű paraméterek használata a tárolt eljárás logika határozza meg, hogy a megfelelő műveletet a táblázat paraméter minden egyes sorához.
- Útmutató a Köteg mérete:
    - A legnagyobb köteg méreteket, az alkalmazás- és üzleti követelmények érvényes használja.
    - A teljesítmény nyereség, nagyobb tételek ideiglenes vagy katasztrofális hibák kockázatok egyenleg. Mit nevezünk újrapróbálkozások következményei és a köteget az adatok elvesztését? 
    - Tesztelje a legnagyobb tétel méretét, ellenőrizze, hogy SQL-adatbázis nem el kell utasítania.
    - A vezérlő kötegelés, például a köteg méretének vagy a pufferelési időkeret beállításainak létrehozására. Ezekkel a beállításokkal rugalmasságot biztosít. A gyártási eljárásnak viselkedését a felhőbeli szolgáltatástól újbóli nélkül módosíthatja.
- Kerülje a párhuzamos végrehajtás tételek, amely egy adatbázis táblájához egyetlen vonatkozni fognak. Ha meg szeretné osztani egy kötegben több dolgozó szálak keresztül, szálak ideális számának meghatározására vizsgálatok futtatása Után egy nincs megadva küszöbérték további szálak fog csökkenti a teljesítményt, hanem növeli.
- Fontolja meg a méret és további felhasználási területei kötegelés végrehajtási így idő pufferelési.

## <a name="next-steps"></a>Következő lépések

Ez a cikk csak a, hogy hogyan adatbázisterv és kódolási kötegelés kapcsolódó technikákat javíthatja az alkalmazás teljesítmény és méretezhetőség. De ez a teljes vonatkozó stratégia egyetlen tényező. További módokon javíthatja a teljesítmény és méretezhetőség olvassa el a [egyetlen adatbázisok Azure SQL-adatbázis teljesítmény útmutatást](sql-database-performance-guidance.md) és [Rugalmas adatbázis-készlet árát és teljesítménybeli szempontok](sql-database-elastic-pool-guidance.md)című témakört.
