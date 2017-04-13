### <a name="compression-support"></a>A tömörítési támogatás  
Nagyméretű adatkészletek feldolgozása okozhatják adatátviteli, valamint a hálózati szűk. Emiatt tömörített üzletek adatainak is nemcsak adatátvitel felgyorsítása a hálózaton keresztül és takarékos lemezterület, de is hozása jelentős teljesítménybeli fejlesztések a nagy adatfeldolgozás. A tömörítési jelenleg, fájlalapú adatokat tárolja, például Azure Blob- vagy a helyszíni fájlrendszerben használata támogatott.  

> [AZURE.NOTE] Tömörítési beállítások nem támogatott a **AvroFormat**, **OrcFormat**vagy **ParquetFormat**adatait. 

Egy adatkészlet tömörítés megadásához a **tömörítés** tulajdonsággal az adathalmazban JSON a következő példának megfelelően:   

    {  
        "name": "AzureBlobDataSet",  
        "properties": {  
            "availability": {  
                "frequency": "Day",  
                "interval": 1  
            },  
            "type": "AzureBlob",  
            "linkedServiceName": "StorageLinkedService",  
            "typeProperties": {  
                "fileName": "pagecounts.csv.gz",  
                "folderPath": "compression/file/",  
                "compression": {  
                    "type": "GZip",  
                    "level": "Optimal"  
                }  
            }  
        }  
    }  
 
A **tömörítési** szakasz két tulajdonságok foglalja magában:  
  
- **Típusa:** a tömörítési kodek, amely lehet **GZIP**, **Deflate** vagy **BZIP2**.  
- **Szint:** lehet **Optimal** vagy **leggyorsabb**tömörítést. 
    - **Leggyorsabb:** A tömörítési művelet be kell fejeződnie minél gyorsabban, még akkor is, ha az eredményül kapott fájl optimálisan nem tömöríti. 
    - **Optimal**: A tömörítési művelet célszerű lehet optimálisan tömöríteni, akkor is, ha a művelet elvégzéséhez hosszabb ideig tart. 
    
    További információ a [Tömörítési szint](https://msdn.microsoft.com/library/system.io.compression.compressionlevel.aspx) témakört. 

Tegyük fel, hogy a fenti minta adatkészlet használja a Másolás tevékenységének kimenetként, a Másolás tevékenység tömöríti a kimeneti adatai az optimális arányát használatával GZIP kodek, és kattintson az adatok írásához a tömörített pagecounts.csv.gz az Azure Blob-tárolóban lévő fájlba.   

Tömörítés tulajdonság-beviteli adatkészlet JSON ad meg, amikor a folyamat tömörített adatok elolvashatja a forrásból származó, és a kimeneti adatkészlet JSON, adja meg a tulajdonságot a Másolás tevékenység írhat tömörített adatokat a megadott helyre. Íme néhány példa alkalmazási helyzetek: 

- Az Azure blob tömörítve olvasható GZIP adatainak kibontásához, és eredményül kapott adatokat írás Azure SQL-adatbázishoz. Ebben az esetben határozhatja meg a bemeneti Azure Blob adatkészlet a tömörítést JSON tulajdonság. 
- Adatok helyszíni fájlrendszert az egyszerű szöveg fájlból, a tömörítés GZip formátumban, adatainak olvasása és írása a tömörített szeretne egy Azure blob. Egy kimenet Azure Blob adatkészlet a tömörítést JSON tulajdonság ebben az esetben definiálni.  
- Egy GZIP tömörített adatainak beolvasása egy Azure blob, kibontásához, tömörítése BZIP2 használatával és eredmény adatokat írni egy Azure blob. A beviteli Azure Blob-adatkészlet GZIP és a kimeneti adatkészlet tömörítési típus BZIP2 ebben az esetben állítsa be a tömörítés típusú határozza meg.   
