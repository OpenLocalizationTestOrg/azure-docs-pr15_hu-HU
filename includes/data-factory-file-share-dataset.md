## <a name="fileshare-dataset-type-properties"></a>Results () fájlmegosztáson adatkészlet tulajdonságai

Szakaszok és a rendelkezésre álló adatkészleteket definiálása tulajdonságok teljes listáját a [létrehozása adatkészleteket](../articles/data-factory/data-factory-create-datasets.md) témakört is. Szakaszok, például a struktúra, elérhetőségét és egy adathalmaz JSON házirend hasonlítanak diagramtípusokat adatkészlet számára. 

A **typeProperties** szakasz különbözik adathalmaz minden egyes típusára. Biztosít, amely az adatkészlet írja be az adatokat. A következő tulajdonságokat a typeProperties szakaszát egy adatkészlet **Results () fájlmegosztáson** adatkészlet típusú foglalja magában:

A tulajdonság | Leírás | Szükséges
-------- | ----------- | --------
Mappa_útvonala | Sub mappa elérési útját. Escape-karakter használata "\" karakterláncban speciális karakternél. [Minta csatolt szolgáltatás és az adatkészlet definíciók](#sample-linked-service-and-dataset-definitions) talál példákat.<br/><br/>Ez a tulajdonság kombinálhatja **partitionBy** mappa elérési utak szeletet alapján van kezdete/vége dátum-idő. | igen
Fájlnév | Ha azt szeretné, hogy a táblázatot egy adott fájlra a mappában, adja meg a fájl nevét a **Mappa_útvonala** . Ha nem ad meg ez a tulajdonság értékét a táblázat a mappában lévő összes fájl mutat.<br/><br/>Ha egy kimenet adatkészlet fileName nincs megadva, a létrehozott fájl nevét a következő lesz az alábbi ebben a formátumban: <br/><br/>Adatok. <Guid>.txt (Példa: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt | nem
partitionedBy | Adja meg a dinamikus Mappa_útvonala, az idő adatsor filename partitionedBy használható. Ha például Mappa_útvonala az adatok óránként paraméteres. | nem
Formátum | A következő formátumú már támogatott: **TextFormat**, **AvroFormat**, **JsonFormat**és **OrcFormat**. A tulajdonság **típusa** formátum csoportban az egyik ezeket az értékeket. További információt [Tartalmazó TextFormat](#specifying-textformat), [Megadása AvroFormat](#specifying-avroformat), [JsonFormat megadása](#specifying-jsonformat)és [Megadása OrcFormat](#specifying-orcformat) lásd. Ha a fájlokat a másolni kívánt – van közötti fájlalapú tárolók (bináris) példányát, ugorja át a két bemeneti és kimeneti adatkészlet definíciók a formátuma szakaszban. | nem
fileFilter | Adja meg, jelölje be az összes fájl, hanem a Mappa_útvonala fájlok csak egy részhalmazát használandó szűrő.<br/><br/>Engedélyezett értékei a következők: `*` (több karakter) és `?` (egyetlen karakter).<br/><br/>Példák 1:`"fileFilter": "*.log"`<br/>Példa 2:`"fileFilter": 2014-1-?.txt"`<br/><br/> fileFilter akkor alkalmazható, a bemeneti Results () fájlmegosztáson adatkészlet számára. Ez a tulajdonság nem támogatott a Fájlrendszerhez.  | nem
| tömörítés | Adja meg a típus és az adatok tömörítés szintjét. Támogatott típusú: **GZip**, a **Deflate**, és a **BZip2** és a támogatott szintek: **Optimal** és **leggyorsabb**. Tömörítési beállítások jelenleg nem támogatott **AvroFormat** vagy **OrcFormat**adatait. További tudnivalókért lásd: [tömörítés támogatás](#compression-support) szakaszában.  | nem |
| useBinaryTransfer | Adja meg, hogy a bináris adatátvitel üzemmód használata. IGAZ és hamis ASCII bináris üzemmód. Alapértelmezett érték: igaz. Ezt a tulajdonságot csak akkor használható, amikor társított csatolt szolgáltatás típusa: FTP-kiszolgáló. | nem | 
 

> [AZURE.NOTE] fájlnév- és fileFilter egyidejű nem használhatók.

### <a name="using-partionedby-property"></a>PartionedBy tulajdonság használatával

Az előző szakaszban említett, dinamikus Mappa_útvonala, az idő adatsor partitionedBy a fájlnevet is megadhat. A Data Factory makrókat és a rendszer változó SliceStart, az egy adott szeletre logikai időszakhoz jelző SliceEnd megteheti. 

Idő sorozat adatkészleteket, az ütemezés és szeletek kapcsolatos további tudnivalókért lásd: [Létrehozása adatkészleteket](../articles/data-factory/data-factory-create-datasets.md), [Ütemezés és a végrehajtás](../articles/data-factory/data-factory-scheduling-and-execution.md)és [Folyamatok létrehozása](../articles/data-factory/data-factory-create-pipelines.md) cikkeket. 

#### <a name="sample-1"></a>Minta 1:

    "folderPath": "wikidatagateway/wikisampledataout/{Slice}",
    "partitionedBy": 
    [
        { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
    ],

Ebben a példában {szeletet} Data Factory rendszer változó SliceStart (YYYYMMDDHH) formátumban megadott értékkel helyettesíti. A SliceStart elindítása a szelet hivatkozik. A Mappa_útvonala eltér az egyes szeletek. Példa: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104.

#### <a name="sample-2"></a>Minta 2:

    "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
    "fileName": "{Hour}.csv",
    "partitionedBy": 
     [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
    ],

Ebben a példában az év, hónap, nap és SliceStart időpontjának kibontása be külön változók Mappa_útvonala és a fájlnevet tulajdonságok által használt.
