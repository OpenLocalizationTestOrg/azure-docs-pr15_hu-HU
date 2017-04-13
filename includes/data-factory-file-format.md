## <a name="specifying-formats"></a>Formázás megadása

Azure Data Factory támogatja a következő formátumú típusú: 

- [Szöveg formázása](#specifying-textformat)
- [JSON formázása](#specifying-jsonformat)
- [Avro formázása](#specifying-avroformat)
- [ORC formázása](#specifying-orcformat)
- [Parquet formázása](#specifying-parquetformat)

### <a name="specifying-textformat"></a>TextFormat megadása

Ha a formátumot **TextFormat**van beállítva, megadhatja a következő **opcionális** tulajdonságok a **formátuma** szakaszban.

| A tulajdonság | Leírás | Megengedett érték | Szükséges |
| -------- | ----------- | -------- | -------- | 
| columnDelimiter | A fájl oszlopai elválasztó karaktert. | Csak egy karakterrel engedélyezve van. Az alapértelmezett érték vessző (","). <br/><br/>Unicode-karakter használata, olvassa el a [Unicode-karakterek](https://en.wikipedia.org/wiki/List_of_Unicode_characters) a megfelelő kódot beszerzése érdekében. Például akkor is érdemes megfontolni a ritka nem nyomtatható karakter, amelyek valószínűleg nem szerepel az adatok használata: Adja meg a "\u0001" Indítsa el a címsor (Rendszerállapot) jelöl, amely. | nem |
| rowDelimiter | Az elválasztó karakter sort a fájlban. | Csak egy karakterrel engedélyezve van. Az alapértelmezett értéke az alábbi értékek közül a Olvasás: ["\r\n", "\r", "\n"] és az írási "\r\n". | nem |
| escapeChar | A speciális karaktert escape-bemeneti fájl tartalmát az elválasztó oszlopot. <br/><br/>EscapeChar és a tábla quoteChar nem adhat meg. | Csak egy karakterrel engedélyezve van. Nincs alapértelmezett érték. <br/><br/>Példa: vesszővel esetén (","), az oszlop határoló, de azt szeretné, hogy a vessző karakter szöveg (például: "Helló, világ"), felvezető karakter: "$" meghatározásával és használható a karakterlánc "$Helló, világ" a forrásban. | nem | 
| quoteChar | Árajánlat karakterláncérték használt elválasztó karakter. Az oszlop- és határoló belül az árajánlat karakter a karakterlánc értéket részeként kezelik. Ezt a tulajdonságot csak be- és a kimeneti adatkészleteket alkalmazható.<br/><br/>EscapeChar és a tábla quoteChar nem adhat meg. | Csak egy karakterrel engedélyezve van. Nincs alapértelmezett érték. <br/><br/>Ha például vesszővel van (","), az oszlop határoló, de azt szeretné, hogy vessző karakter szöveg (Példa: < Helló, világ >), határozhatja meg "(dupla idézőjel), valamint az árajánlat karakter a karakterlánc használata"Helló, világ"a forrásban. | nem |
| nullValue | Egy vagy több karaktert a null érték ábrázolására használt. | Egy vagy több karaktert. Az alapértelmezett értékei "\N" és "NULL" Olvasás és a "\N" írás. | nem |
| encodingName | Adja meg a kódolási nevét. | Érvényes kódolási nevét. Lásd: [Encoding.EncodingName tulajdonság](https://msdn.microsoft.com/library/system.text.encoding.aspx). Példa: a windows-1250 vagy shift_jis. Az alapértelmezett érték UTF-8. | nem | 
| firstRowAsHeader | Megadja, hogy az első sor fejléceként figyelembe. Egy beviteli adatkészlet számára Data Factory beolvassa az első sor fejléceként. Egy kimenet adatkészlet az adatok gyári fejléceként első sorában ír. <br/><br/>Példák [esetek az **firstRowAsHeader** és **skipLineCount** ](#scenarios-for-using-firstrowasheader-and-skiplinecount) talál. | Igaz<br/>Hamis (alapértelmezett) | nem |
| skipLineCount | Azt jelzi, hogy a bemeneti fájlok adatok olvasásakor kihagyásához sorok számát. SkipLineCount és firstRowAsHeader is meg vannak adva, ha a sorok először kimaradnak, és kattintson a fejléc adatainak beviteli fájlból olvassa el. <br/><br/>Példák [esetek az firstRowAsHeader és skipLineCount](#scenarios-for-using-firstrowasheader-and-skiplinecount) talál. | Egész szám | nem | 
| treatEmptyAsNull | Megadja, hogy a NULL értéket vagy üres karakterláncot tekinti a null érték, a bemeneti fájlból adatok olvasásakor. | TRUE (alapértelmezett)<br/>Hamis | nem |  

#### <a name="textformat-example"></a>Példa TextFormat
A következő példában néhány formátum TextFormat számára.

    "typeProperties":
    {
        "folderPath": "mycontainer/myfolder",
        "fileName": "myblobname"
        "format":
        {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";",
            "quoteChar": "\"",
            "NullValue": "NaN"
            "firstRowAsHeader": true,
            "skipLineCount": 0,
            "treatEmptyAsNull": true
        }
    },

Egy escapeChar quoteChar helyett használandó cserélje ki a sort, a következő escapeChar quoteChar:

    "escapeChar": "$",



### <a name="scenarios-for-using-firstrowasheader-and-skiplinecount"></a>Esetek az firstRowAsHeader és skipLineCount

- Nem fájl forrásból származó másol egy szövegfájlt, és szeretné adni a séma metaadatokat tartalmazó élőfej vonal (például: SQL séma). Adja meg az ebben az esetben a kimeneti adathalmazban igaz **firstRowAsHeader** . 
- Egy nem fájl gyűjtő élőfej vonallal tartalmazó szövegfájl másol, és húzza a sor szeretné. Adja meg a bemeneti adathalmazban igaz **firstRowAsHeader** .
- Másol egy szövegfájlt, és szeretné hagyni a néhány vonalak az elejére, amelyek nincsenek adatok vagy a fejlécre adatokat tartalmazzák. Adja meg a **skipLineCount** , hogy hány sort ki kell hagyni. Ha a fájlt a többi olyan élőfej sort tartalmaz, **firstRowAsHeader**is megadhatja. Ha **skipLineCount** és **firstRowAsHeader** is meg vannak adva, a sorok először kimaradnak és majd olvassa el a fejlécadatokat beviteli fájlból

### <a name="specifying-avroformat"></a>AvroFormat megadása
Ha a formátumot AvroFormat van beállítva, nem szükséges bármely tulajdonságokat a formátuma szakaszban a typeProperties szakaszon belül. Példa:

    "format":
    {
        "type": "AvroFormat",
    }

Egy struktúratáblával Avro formátumban használni, akkor is hivatkozhat [Apache struktúra oktatóprogram](https://cwiki.apache.org/confluence/display/Hive/AvroSerDe).

Vegye figyelembe az alábbiakat:  

- [Komplex adatok](http://avro.apache.org/docs/current/spec.html#schema_complex) már nem támogatott (a felsorolások, tömbök, térképek, a szervezetek a rekordokat és a rögzített)

### <a name="specifying-jsonformat"></a>JsonFormat megadása

Ha a formátumot **JsonFormat**van beállítva, megadhatja a következő **opcionális** tulajdonságok a **formátuma** szakaszban.

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| filePattern | Adja meg, a mintázat minden JSON-fájlban tárolt adatok. Engedélyezett értékei a következők: **setOfObjects** és **arrayOfObjects**. Az **alapértelmezett** érték: **setOfObjects**. Lásd az alábbi szakaszok ezeket a mintázatok kapcsolatban további tájékoztatást.| nem |
| encodingName | Adja meg a kódolási nevét. Az érvényes kódolási nevek listája: [Encoding.EncodingName](https://msdn.microsoft.com/library/system.text.encoding.aspx) tulajdonság. Példa: a windows-1250 vagy shift_jis. Az **alapértelmezett** érték: **UTF-8**. | nem | 
| nestingSeparator | Az karakter, amely elválaszthatja egymástól a beágyazási szintek száma. Az alapértelmezett érték "." (pont). | nem | 


#### <a name="setofobjects-file-pattern"></a>setOfObjects fájl mintával

Minden fájl tartalmaz egy objektumot, vagy több sor-tagolt/tömb összefűzéséből objektumokat. Amikor egy kimenet adathalmazban ezt a beállítást választja, a Másolás tevékenység egyetlen JSON fájl az egyes objektumokra (sor tabulátorral tagolt) soronként hoz létre.

**egyetlen objektum** 

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }

**vonal tagolt JSON** 

    {"time":"2015-04-29T07:12:20.9100000Z","callingimsi":"466920403025604","callingnum1":"678948008","callingnum2":"567834760","switch1":"China","switch2":"Germany"}
    {"time":"2015-04-29T07:13:21.0220000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789037573","switch1":"US","switch2":"UK"}
    {"time":"2015-04-29T07:13:21.4370000Z","callingimsi":"466923101048691","callingnum1":"678901578","callingnum2":"345626404","switch1":"Germany","switch2":"UK"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"789037573","callingnum2":"789044691","switch1":"UK","switch2":"Australia"}
    {"time":"2015-04-29T07:13:22.0960000Z","callingimsi":"466922202613463","callingnum1":"123436380","callingnum2":"789044691","switch1":"US","switch2":"Australia"}

**egybeírt JSON**

    {
        "time": "2015-04-29T07:12:20.9100000Z",
        "callingimsi": "466920403025604",
        "callingnum1": "678948008",
        "callingnum2": "567834760",
        "switch1": "China",
        "switch2": "Germany"
    }
    {
        "time": "2015-04-29T07:13:21.0220000Z",
        "callingimsi": "466922202613463",
        "callingnum1": "123436380",
        "callingnum2": "789037573",
        "switch1": "US",
        "switch2": "UK"
    }
    {
        "time": "2015-04-29T07:13:21.4370000Z",
        "callingimsi": "466923101048691",
        "callingnum1": "678901578",
        "callingnum2": "345626404",
        "switch1": "Germany",
        "switch2": "UK"
    }


#### <a name="arrayofobjects-file-pattern"></a>arrayOfObjects fájl minta. 

Fájlonként az objektumok tömb tartalmazza. 

    [
        {
            "time": "2015-04-29T07:12:20.9100000Z",
            "callingimsi": "466920403025604",
            "callingnum1": "678948008",
            "callingnum2": "567834760",
            "switch1": "China",
            "switch2": "Germany"
        },
        {
            "time": "2015-04-29T07:13:21.0220000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789037573",
            "switch1": "US",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:21.4370000Z",
            "callingimsi": "466923101048691",
            "callingnum1": "678901578",
            "callingnum2": "345626404",
            "switch1": "Germany",
            "switch2": "UK"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "789037573",
            "callingnum2": "789044691",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:22.0960000Z",
            "callingimsi": "466922202613463",
            "callingnum1": "123436380",
            "callingnum2": "789044691",
            "switch1": "US",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:24.2120000Z",
            "callingimsi": "466922201102759",
            "callingnum1": "345698602",
            "callingnum2": "789097900",
            "switch1": "UK",
            "switch2": "Australia"
        },
        {
            "time": "2015-04-29T07:13:25.6320000Z",
            "callingimsi": "466923300236137",
            "callingnum1": "567850552",
            "callingnum2": "789086133",
            "switch1": "China",
            "switch2": "Germany"
        }
    ]

### <a name="jsonformat-example"></a>Példa JsonFormat

Ha a következő tartalommal JSON fájl:  

    {
        "Id": 1,
        "Name": {
            "First": "John",
            "Last": "Doe"
        },
        "Tags": ["Data Factory”, "Azure"]
    }

és másolja a vágólapra az alábbi formátumban egy Azure SQL táblába szeretne: 

Azonosító  | Name.First | Name.Middle | Name.Last | Címkék
--- | ---------- | ----------- | --------- | ----
1 | János | NULL | Jakab | ["Adatok Factory", "Azure"]

A beviteli adatkészlet JsonFormat típusú a következők: (csak a fontos részeket, a részleges definíció)

    "properties": {
        "structure": [
            {"name": "Id", "type": "Int"},
            {"name": "Name.First", "type": "String"},
            {"name": "Name.Middle", "type": "String"},
            {"name": "Name.Last", "type": "String"},
            {"name": "Tags", "type": "string"}
        ],
        "typeProperties":
        {
            "folderPath": "mycontainer/myfolder",
            "format":
            {
                "type": "JsonFormat",
                "filePattern": "setOfObjects",
                "encodingName": "UTF-8",
                "nestingSeparator": "."
            }
        }
    }

Ha a szerkezettel nincs megadva, a Másolás tevékenység a szerkezet összeolvasztja alapértelmezés szerint, és minden dolog másolása. 

#### <a name="supported-json-structure"></a>Támogatott JSON-struktúra
Vegye figyelembe az alábbiakat: 

- Név/érték összekapcsolásával gyűjteménye az egyes objektumokra van hozzárendelve egy sornyi adatot táblázatos formában. Objektumok egymásba, és megadhatja, hogy miként egy adatkészlet a szerkezet összeolvasztása beágyazási elválasztó karaktert (.) az alapértelmezés szerint. Lásd az előző, például egy szakasz [JsonFormat példa](#jsonformat-example) .  
- Ha a szerkezettel az adatok gyári adathalmazban nincs megadva, a Másolás tevékenység észleli az sémát az első objektumra, és a teljes objektum összeolvasztása. 
- Ha a JSON bemenet tömbben van, a Másolás tevékenység alakítja át a teljes tömböt érték karakterlánc. [Oszlopok megfeleltetése](#column-mapping-with-translator-rules)vagy a szűrési ugrani választhat.
- Ha ugyanazon szinten ismétlődő nevek, a Másolás tevékenység az utolsóként választja ki.
- Tulajdonságok neve nagybetűk között. Ugyanaz a neve, de a másik burkolatok két tulajdonságok két külön tulajdonságok számít. 

### <a name="specifying-orcformat"></a>OrcFormat megadása
Ha a formátumot OrcFormat van beállítva, nem szükséges bármely tulajdonságokat a formátuma szakaszban a typeProperties szakaszon belül. Példa:

    "format":
    {
        "type": "OrcFormat"
    }

> [AZURE.IMPORTANT] Ha ORC fájlok nem másolt **,-van** között a helyszíni és felhőbeli adatokat tárolja, telepítenie kell ezeket az JRE 8 (Java-futtatókörnyezet) az átjárót futtató számítógépen. Egy 64 bites átjáró igényel 64 bites JRE, és 32 bites átjáró 32 bites JRE van szükség. A két verzió, az [Itt](http://go.microsoft.com/fwlink/?LinkId=808605)található. Válassza ki a megfelelő típust.

Vegye figyelembe az alábbiakat:

-   Komplex adatok bemutatásához fájltípusok, amelyek nem támogatott (struktúra, Térképen, lista, Unió)
-   ORC fájlhoz [kapcsolódó tömörítési beállítások](http://hortonworks.com/blog/orcfile-in-hdp-2-better-compression-better-performance/)három: nincs ZLIB, SNAPPY. Adatok gyári támogatja az adatok beolvasása az alábbi tömörített formátum valamelyikében ORC fájlból. Akkor használja a tömörítés kodek, a metaadatok olvasni az adatokat. Azonban ORC fájl írásakor Data Factory úgy dönt, ZLIB, amely olyan, az alapértelmezett ORC. Jelenleg nincs lehetőség a működési módjának felülbírálására. 

### <a name="specifying-parquetformat"></a>ParquetFormat megadása
Ha a formátumot ParquetFormat van beállítva, nem szükséges bármely tulajdonságokat a formátuma szakaszban a typeProperties szakaszon belül. Példa:

    "format":
    {
        "type": "ParquetFormat"
    }

> [AZURE.IMPORTANT] Ha Parquet fájlok nem másolt **,-van** között a helyszíni és felhőbeli adatokat tárolja, telepítenie kell ezeket az JRE 8 (Java-futtatókörnyezet) az átjárót futtató számítógépen. Egy 64 bites átjáró igényel 64 bites JRE, és 32 bites átjáró 32 bites JRE van szükség. A két verzió, az [Itt](http://go.microsoft.com/fwlink/?LinkId=808605)található. Válassza ki a megfelelő típust.

Vegye figyelembe az alábbiakat:

-   Komplex adatok bemutatásához fájltípusok, amelyek nem támogatott (térkép lista)
-   Parquet fájlhoz, a következő tömörítés kapcsolatos beállítások: nincs, SNAPPY GZIP és LZO. Adatok gyári támogatja az adatok beolvasása az alábbi tömörített formátum valamelyikében ORC fájlból. A tömörítési kodek a metaadatok akkor olvasni az adatokat. Azonban Parquet fájl írásakor Data Factory úgy dönt, SNAPPY, az alapértelmezett Parquet formátum. Jelenleg nincs lehetőség a működési módjának felülbírálására. 
