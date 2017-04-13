<properties 
    pageTitle="Gyári Adatfüggvényei és a rendszer változók |} Microsoft Azure" 
    description="Azure Data Factory és a rendszer változók listája" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"
    services="data-factory"
/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016" 
    ms.author="shlo"/>

# <a name="azure-data-factory---functions-and-system-variables"></a>Azure adatok Factory - függvények és a rendszer változói
Ez a cikk a függvények és változó támogatják az Azure Data Factory információt tartalmaz.
  
## <a name="data-factory-system-variables"></a>Adatok gyári rendszer változói

Változó neve | Leírás | Objektum hatóköre | JSON hatókör és használati eset
------------- | ----------- | ------------ | ------------------------
WindowStart | Időközönként futtassa az ablak aktuális tevékenység megkezdése | tevékenység | <ol><li>Adja meg az adatok kijelölés lekérdezések. Összekötő cikkekben hivatkozott [Adatok mozgását tevékenységek](data-factory-data-movement-activities.md) című témakörben olvashat bővebben.</li><li>A paramétereket át struktúra parancsprogram (minta a fent látható).</li>
WindowEnd | Időközönként futtassa az ablak aktuális tevékenység végére | tevékenység | ugyanaz, mint feljebb
SliceStart | Az éppen előállított szeletre időközönként elejére | tevékenység<br/>adatkészlet | <ol><li>Adja meg a dinamikus mappa elérési utakat és a fájlnevekben [Azure Blob](data-factory-azure-blob-connector.md) - és a [fájlrendszerben adatkészleteket](data-factory-onprem-file-system-connector.md)használata közben.</li><li>Adja meg a bemeneti függőségek tevékenység bemenetben gyűjteményben adatok gyári funkciókkal.</li></ol>
SliceEnd | Az aktuális adatokat szeletek éppen előállított időközönként végére | tevékenység<br/>adatkészlet | ugyanaz, mint feljebb. 

> [AZURE.NOTE] Adatok gyári jelenleg csak az, hogy pontosan meghatározott a tevékenység az ütemterv egyezik-e az ütemezésben érhetők el a kimeneti adatkészlet megadott. Ez azt jelenti, hogy WindowStart, WindowEnd és SliceStart és SliceEnd mindig feleltesse meg az egy időszakra és egy kimeneti szelet.
 
## <a name="data-factory-functions"></a>Adatok gyári függvények 

Az adatok gyári mentén a fent említett rendszer változói függvények a következő célokra használhatók:

1.  Adja meg az adatok kijelölés lekérdezések (lásd: a [Tevékenységekre vonatkozó adatok mozgását](data-factory-data-movement-activities.md) cikk által hivatkozott összekötő cikkeket.

    Meghívásához adatok gyári függvény szintaxisa: ** $$ ** adatok kijelölés lekérdezések és a tevékenység és adatkészleteket egyéb tulajdonságait.  
2. Beviteli függőségek gyári függvényekkel tevékenység megadása a webhelycsoport bemeneti (lásd a fenti minta).

    $ a $ beviteli függőség kifejezések megadására szolgáló nincs szükség.   

A következő példa **sqlReaderQuery** tulajdonság JSON-fájlban a **Text.Format** függvény által visszaadott érték van rendelve. Ez a példa is használ, a rendszer változó nevű **WindowStart**, amelynek a kezdési időpontot, a Futtatás ablak tevékenység jelöli.
    
    {
        "Type": "SqlSource",
        "sqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
    }

### <a name="functions"></a>Függvények

Az alábbi táblázatokban Azure Data Factory összes függvénye:

Kategória | Függvény | Paraméterek | Leírás
-------- | -------- | ---------- | ----------- 
Idő | AddHours(X,Y) | X DateTime <br/><br/>Y int | Hozzáadja az adott idő alatt X Y óra. <br/><br/>Példa: 9/5/2013 12:00:00 PM + 2 órán = 9/5/2013 2:00:00 PM
Idő | AddMinutes(X,Y) | X DateTime <br/><br/>Y int | X Y perc ad.<br/><br/>Példa: 9/15/2013 12:00:00 PM + 15 percet = 9/15/2013 12:15:00 PM
Idő | StartOfHour(X) | X Datetime | Kapja meg a kezdési időpontot az óraként X óra része képviseli. <br/><br/>Példa: StartOfHour 9/15/2013 05:10:23 du van 9/15/2013 05:00:00 PM
Dátum | AddDays(X,Y) | X DateTime<br/><br/>Y int | X Y nap hozzáadása.<br/><br/>Példa: 9/15/2013 12:00:00 PM + 2 nap = 9/17/2013 12:00:00 PM
Dátum | AddMonths(X,Y) | X DateTime<br/><br/>Y int | X Y hónapok ad.<br/><br/>Példa: 9/15/2013 12:00:00 PM + 1 hónap = 10/15/2013 12:00:00 PM 
Dátum | AddQuarters(X,Y) | X DateTime <br/><br/>Y int | Összeadja az Y * x 3 hónap.<br/><br/>Példa: 9/15/2013 12:00:00 PM + negyedév = 12/15 1/2013 12:00:00 PM
Dátum | AddWeeks(X,Y) | X DateTime<br/><br/>Y int | Összeadja az Y * x napján<br/><br/>Példa: 9/15/2013 12:00:00 PM + 1 hét = 9/22-es és 2013-at 12:00:00 PM
Dátum | AddYears(X,Y) | X DateTime<br/><br/>Y int | X Y évek ad.<br/><br/>Példa: 9/15/2013 12:00:00 PM + év = 9/15 1/2014 12:00:00 PM
Dátum | Day(X) | X DateTime | A napi összetevője X kap.<br/><br/>Példa: Nap 9/15/2013 12:00:00 PM 9. 
Dátum | DayOfWeek(X) | X DateTime | A hét napja összetevőjét az X kap.<br/><br/>Példa: DayOfWeek 9/15/2013 a 12:00:00 PM vasárnap.
Dátum | DayOfYear(X) | X DateTime | A napot az év X év része képviseli kap.<br/><br/>Példa:<br/>12/1/2015: a Skype 2015 335 napjára<br/>12/31/2015: a Skype 2015 365 napjára<br/>12/31/2016: 2016 (termékek év) 366 napjára
Dátum | DaysInMonth(X) | X DateTime | Megkapja a nap, a hónap X paraméter hónap része képviseli.<br/><br/>Példa: DaysInMonth 9/15/2013 olyan 30 mivel nincsenek olyan 30 nap szeptember hónapban.
Dátum | EndOfDay(X) | X DateTime | X (nap összetevő) napjának befejezési dátum-idő kap.<br/><br/>Példa: EndOfDay 9/15/2013 05:10:23 du van 9/15/2013 11:59:59 du.
Dátum | EndOfMonth(X) | X DateTime | A hónap X paraméter hónap részét végére kap. <br/><br/>Példa: EndOfMonth 9/15/2013 05:10:23 du értéke 9/30/2013 11:59:59 PM (a szeptember hónap végét megadó dátum idő)
Dátum | StartOfDay(X) | X DateTime | Kapja meg a kezdő napjának X paraméter nap része képviseli.<br/><br/>Példa: StartOfDay 9/15/2013 05:10:23 du van 9/15/2013 12:00:00 de.
Dátum és idő | FROM(X) | X-karakterlánc | Elemzi a karakterláncot X egy dátum-idő.
Dátum és idő | Ticks(X) | X DateTime | Az osztások X paraméter tulajdonsága kap. Egy osztásjelek 100 nanoszekundumban egyenlő. Ez a tulajdonság értékét jelöli január 1, 12:00:00 éjfél óta eltelt 0001 osztások száma. 
Szöveg | Format(X) | X karakterlánc típusú változóban | Formázza a szöveget.

#### <a name="textformat-example"></a>Példa Text.Format

    "defines": { 
        "Year" : "$$Text.Format('{0:yyyy}',WindowStart)",
        "Month" : "$$Text.Format('{0:MM}',WindowStart)",
        "Day" : "$$Text.Format('{0:dd}',WindowStart)",
        "Hour" : "$$Text.Format('{0:hh}',WindowStart)"
    }

Lásd: [egyéni dátum és idő formátum-karakterláncot](https://msdn.microsoft.com/library/8kb3ddd4.aspx) témakör ismerteti a különböző formázási beállításokat használhatja (például: éé összehasonlítása éééé). 

> [AZURE.NOTE] Amikor egy másik függvényen belül függvény, nem kell használni **$$** előtag a belső függvény. Példa: $$Text.Format ("PartitionKey eq \\" my_pkey_filter_value\\"és RowKey ge \\" {0:yyyy-MM-dd óó}\\", Time.AddHours (SliceStart -6)). Ebben a példában figyelje meg, hogy **$$** előtag nem használható a **Time.AddHours** függvényt. 
  

