## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Struktúra definíciója a téglalap alakú adatkészleteket megadása
A JSON adatkészleteket struktúra szakaszának téglalap alakú táblázatok (a sorok és oszlopok) **választható** szakaszok és a táblázat oszlopainak gyűjteménye tartalmazza. A szerkezet szakasz bármelyik nyújtó adatainak vonatkozó típusának Dokumentumkonvertálás vagy az oszlop-hozzárendeléseket módon fogja használni. A következő szakaszok ezeket a funkciókat részletesen ismertetik. 

Minden egyes oszlopát az alábbi tulajdonságait tartalmazza:

| A tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| név | Az oszlop nevét. | igen |
| típus | Az oszlop adattípusát. Lásd: a típus Dokumentumkonvertálás szakasz alatt a további részletesen kapcsolatban, hogy mikor kell megadott típus információ | nem |
| kulturális környezettel | .NET a kulturális környezet típusa van megadva, és a .NET-típus Datetime vagy Datetimeoffset használandó alapján. Az alapértelmezett érték "hu-hu".  | nem |
| Formátum | Formátum-karakterlánc típus van megadva, és a .NET használandó írja be a Datetime vagy Datetimeoffset. | nem |

A következő példában a struktúra szakaszát JSON egy három oszlop felhasználóazonosító, a név és a lastlogindate tartalmazó táblát.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Mikor "struktúra" információkat szeretne kapni, és mit szerepeltetni a **struktúra** szakaszt a következő profilképek használja.

- **Strukturált adatforrások** együtt az adatokat, magát (forrásokból, mint például az Oracle, SQL Server Azure táblázat stb.), a "struktúra" szakasz csak akkor, ha azt szeretné, adjon sémával és egy típusú információt adatokat tároló végezze el az adott forrásoszlopokat gyűjtő bizonyos oszlopait oszlop megfeleltetése, hogy nevük nem azonos (lásd: oszlop hozzárendelési szakaszban az alábbi adatokat). 

    Fent említett a típus információ nem kötelező "szerkezet" szakaszban. A strukturált adatforrások, adatainak már elérhető az adatok tárolása adatkészlet definition részeként így nem szerepeltesse adatainak amikor, tartalmazza a "struktúra" szakaszt.
- **Az olvasási adatforrásokhoz (Azure blob-kifejezetten) a séma** lehetősége van arra, hogy nem tárolja az adatokat tartalmazó séma vagy típusú információkat tárolja az adatokat. Az alábbi típusú adatforrások "szerkezetének" kell felvétele 2 a következő esetekben:
    - Teendők Oszlopok megfeleltetése.
    - Másolás tevékenységet forrás az adatkészlet befejeztével "elfoglalt" típusú információt, és adatok gyári típusú információk fogja használni a gyűjtő natív típusok való átalakítást. Lásd: [és Azure Blob-az adatok áthelyezése a](../articles/data-factory/data-factory-azure-blob-connector.md) cikk további információt.

### <a name="supported-net-based-types"></a>Támogatott. NETTÓ-alapú típusai 
Adatok gyári a következő CLS kompatibilis .NET-alapú típusú értékeket támogatja az "elfoglalt" típusú információt olvasási adatforrásokhoz, például Azure blob-séma.

- Int16
- Int32 
- Int64
- Egyetlen
- Dupla
- Decimális
- Byte]
- Logikai
- Karakterlánc 
- Globálisan egyedi azonosítója
- Dátum és idő
- Datetimeoffset
- Időszak 

A Datetime és Datetimeoffset is tetszés szerint megadhatja az egyéni Datetime karakterlánc elemzése megkönnyítésére "kulturális" & "formázása" karakterlánc. Lásd az alábbi típusú átalakításhoz minta.

