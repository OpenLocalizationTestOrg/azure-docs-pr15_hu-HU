<properties 
    pageTitle="Az adatok terabájt betöltése SQL adatraktár |} Microsoft Azure" 
    description="Bemutatja, hogyan 1 TB-nyi adatok töltődnek be a Azure SQL-adatraktár a 15 perccel az Azure Data Factory" 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="jingwang"/>

# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-azure-data-factory-copy-wizard"></a>1 TB betöltése Azure SQL-adatraktár a 15 perccel az Azure Data Factory [másolás Wizard]
[Azure SQL-adatraktár](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) alkalmas nagy mennyiségű relációs és a nem relációs adatok feldolgozásának felhőalapú, méretezési adatbázis.  Nagymértékben párhuzamos feldolgozási (MPP) architektúrára épül, SQL adatraktár van optimalizálva vállalati adatok raktári munkaterhelésekből.  Rugalmasan tároló méretezni, és önállóan számítja ki a felhőben rugalmasság is kínál.

Első lépések – Azure SQL-adatraktár ettől kezdve könnyebben **Azure adatok gyár**valaha használata helyett.  Azure Data Factory egy teljes körű felügyelt felhőalapú adatok integrálása szolgáltatása, amelynek feltöltése egy SQL adatraktár mentése, miközben SQL adatraktár értékelése és a fölött analytics-megoldások fejlesztésére értékes időt, és a meglévő rendszer adataival használható.  Az alábbiakban Azure SQL Azure-adatok gyári használatával adatraktár az adatok betöltésének kulcs előnyökkel jár:

- **Könnyen beállítása**: 5 lépésből intuitív varázsló nem parancsfájlok szükséges.
- **Gazdag adattár támogatás**: a helyszíni és felhőalapú adatokat tárolja széles körű támogatása beépített.
- **Biztonságos és felelnie**: adatok HTTPS- vagy készült ExpressRoute átvitt és globális szolgáltatás jelenléti biztosítja az adatok soha ne hagyja a földrajzi határ
- **Unparalleled teljesítmény PolyBase használatával** – használatával Polybase a leghatékonyabb mód, ha az adatokat az Azure SQL-adatraktár. Az átmeneti tárolásra szolgáló blob-szolgáltatást használja, az összes típusú adatokat tárolja Azure Blob-tárolóhoz, amely alapértelmezés szerint támogatja a Polybase mellett a nagy terhelés sebesség érhet el.

Ez a cikk bemutatja, hogyan betöltése 1 TB-adatok az Azure Blob-tárolóhoz Azure SQL-adatraktár a 15 perces feletti 1.2-es GB/s átviteli a adatok gyári másolás varázsló segítségével.

Ez a cikk részletes való áthelyezéséhez nyújt útmutatást adatok az Azure SQL-adatraktár a Másolás varázslóval. 

> [AZURE.NOTE] Lásd: a [adatokat, és az Azure SQL Azure-adatok gyári használatával adatraktár](data-factory-azure-sql-data-warehouse-connector.md) funkciók kapcsolatos általános tudnivalókat az adatok áthelyezése és Azure SQL-adatraktár az adatok gyári cikk. 
> 
> Azure portálon, Visual Studióban, PowerShell, folyamatok is készíthet stb. Lásd: [oktatóanyag: adatok másolása az Azure Blob az Azure SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) rövid ismertetését megtalálja a lépésenkénti útmutatást adunk a Másolás tevékenység használata Azure Data Factory.  

## <a name="prerequisites"></a>Előfeltételek
- Azure Blob-tárolóhoz: a kísérlet Azure Blob-tároló (GRS) használja a TPC-H tesztelés adatkészlet tárolásához.  Ha nem rendelkezik Azure tárterület-fiókkal, megtudhatja, [hogy miként tárterület-fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account).
- [A H TPC](http://www.tpc.org/tpch/) adatok: fogjuk használni a tesztelés adatkészlet TPC-H.  Megtennie, akkor használja `dbgen` az eszközkészlet TPC-H, amely segít, hogy az adatkészlet készítése.  Forráskód vagy letöltheti `dbgen` [TPC](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) eszközökről összeállítása saját magát, és töltse le a lefordított bináris [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Futtassa a következő parancsok végrehajtása az 1 TB-strukturálatlan fájl létrehozásához dbgen.exe `lineitem` várható táblázat 10 fájlok között:
    - `Dbgen -s 1000 -S **1** -C 10 -T L -v`
    - `Dbgen -s 1000 -S **2** -C 10 -T L -v`
    - …
    - `Dbgen -s 1000 -S **10** -C 10 -T L -v` 

    Azure Blob a létrehozott fájlok másolása gombra.  [Áthelyezés adatokat egy helyszíni fájlrendszerből Azure Data Factory használatával](data-factory-onprem-file-system-connector.md) , hogyan érheti el, amely ADF másolással.   
- Azure SQL-adatraktár: Ez a kísérlet betölti adatok a 6000 DWUs készült Azure SQL adatraktár

    Olvassa el az létrehozása [Az Azure SQL-adatraktár](../sql-data-warehouse/sql-data-warehouse-get-started-provision/) adatraktár SQL-adatbázis létrehozásához részletes útmutatást.  A lehetséges betöltés legjobb teljesítményt Polybase használata SQL adatraktár be, azt válassza az adatok raktári egységek (DWUs) a teljesítmény beállítással, amely 6000 DWUs engedélyezett maximális számát.

    > [AZURE.NOTE] 
    > Az Azure Blob betöltésekor teljesítmény betöltése adatai közvetlenül arányos kell konfigurálni az SQL adatraktár DWUs számát:
    > 
    > 1 TB betöltése az 1000 DWU SQL adatraktár megnyitja 87min (~ 200MBps átviteli) 1 TB betöltése a 2000 DWU SQL adatraktár megnyitja 46min (~ 380MBps átviteli) 1 TB betöltése be 6000 DWU SQL adatraktár megnyitja 14min (~1.2GBps sebesség) 

    Szeretne készíteni egy SQL adatraktár 6000 DWUs, a csúszkát teljesítmény teljes mértékben jobbra:

    ![Teljesítmény csúszka](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    Egy meglévő adatbázis, amely nincs beállítva a 6000 DWUs méretezheti azt Azure portál használatával.  Nyissa meg azt az adatbázist az Azure-portálon, és nem jelenik meg a **Méretezés** gomb az **Áttekintés** panelen, az alábbi képen látható:

    ![Méretezés gomb](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    A legnagyobb érték **skála** gombra kattintva nyissa meg a következő panelt, húzza a csúszkát, és kattintson a **Mentés** gombra.

    ![Méretezés párbeszédpanel](media/data-factory-load-sql-data-warehouse/scale-dialog.png)
    
    A kísérlet adatok betölti az Azure SQL-adatraktár használatával `xlargerc` erőforrás osztály.

    Legjobb lehetséges átviteli eléréséhez másolás kell egy SQL adatraktár felhasználójának hajtható végre `xlargerc` erőforrás osztály.  Megtudhatja, hogy miként ezt megteheti a következő [Módosítás egy felhasználó erőforrás osztály példa](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#change-a-user-resource-class-example).  

- Az alábbi DDL utasítás futtatásával hozzon létre a célként megadott táblázat séma Azure SQL-adatraktár adatbázisban:

        CREATE TABLE [dbo].[lineitem]
        (
            [L_ORDERKEY] [bigint] NOT NULL,
            [L_PARTKEY] [bigint] NOT NULL,
            [L_SUPPKEY] [bigint] NOT NULL,
            [L_LINENUMBER] [int] NOT NULL,
            [L_QUANTITY] [decimal](15, 2) NULL,
            [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
            [L_DISCOUNT] [decimal](15, 2) NULL,
            [L_TAX] [decimal](15, 2) NULL,
            [L_RETURNFLAG] [char](1) NULL,
            [L_LINESTATUS] [char](1) NULL,
            [L_SHIPDATE] [date] NULL,
            [L_COMMITDATE] [date] NULL,
            [L_RECEIPTDATE] [date] NULL,
            [L_SHIPINSTRUCT] [char](25) NULL,
            [L_SHIPMODE] [char](10) NULL,
            [L_COMMENT] [varchar](44) NULL
        )
        WITH
        (
            DISTRIBUTION = ROUND_ROBIN,
            CLUSTERED COLUMNSTORE INDEX
        )

Befejezett kapcsolatban előzetesen szükséges lépéseket azt készen állnak a Másolás tevékenység, a Másolás varázslóval konfigurálása.

## <a name="launch-copy-wizard"></a>Másolás varázsló elindítása

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com).
2.  Kattintson a **+ Új** a bal felső sarokban kattintson a **üzletiintelligencia- + analytics**, és kattintson az **Adatok gyári**. 
6. Kattintson az **új adatok gyári** lap:
    1. Írja be a **név** **LoadIntoSQLDWDataFactory** .
        Az Azure adatok gyári neve globálisan egyedinek kell lennie. Ha a hibakód: **nem érhető el adatok gyári neve "LoadIntoSQLDWDataFactory"**, módosíthatja az adatok gyári (például yournameLoadIntoSQLDWDataFactory) nevét, és próbáljon meg ismét létrehozni. [Adatok Factory - elnevezési szabályai](data-factory-naming-rules.md) témakört vonatkozó adatok gyári eltérések elnevezési szabályokat.  
     
    2. Jelölje ki az Azure **előfizetés**.
    3. Erőforráscsoport hajtsa végre a következő lépések egyikét: 
        1. Jelölje be a **meglévő használata** jelölje ki a meglévő erőforráscsoport.
        2. Válassza az **Új** erőforráscsoport a nevét.
    3. Jelölje ki az adatok gyári **helyét** .
    4. Jelölje be a lap alján az **Irányítópult PIN-kód** jelölőnégyzetet.  
    5. Kattintson a **létrehozása**gombra.
10. A létrehozását követően jelenik meg az **Adatok gyári** lap az alábbi képen látható módon:

    ![Adatok factory kezdőlapján](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
11. Kattintson az adatok Factory kezdőlapján az **adatok másolása** csempére **Másolás varázsló**elindításához. 

    > [AZURE.NOTE] Jelenik meg, hogy a böngésző marad, a "Engedélyező...", ha letiltása vagy törölje belőle a jelet **tiltása a külső cookie-k és webhelyadatok** beállítást (vagy) tarthatja engedélyezve és hozható létre kivétel a **login.microsoftonline.com** és próbálkozzon újra indítása a varázsló.


## <a name="step-1-configure-data-loading-schedule"></a>Lépés: 1: Adatok betöltése ütemezés konfigurálása
Első lépésként az adatok betöltésének ütemezés konfigurálása.  

A **Tulajdonságok** lapon:
1. **CopyFromBlobToAzureSqlDataWarehouse** megadása a **tevékenység neve**
2. Jelölje be a **Futtatás egyszer** lehetőséget.   
3. Kattintson a **Tovább**gombra.  

![Másolja a varázsló - Tulajdonságok lap](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>Lépés: 2: Állítsa be a forrás
Ebből a szakaszból megtudhatja, hogy a lépéseket követve állítsa be a forrás: Azure Blob-1 TB TPC-H sort tartalmazó fájlok elemet.

**Azure Blob-tárolóhoz** válassza az adatok tárolására, és kattintson a **Tovább**gombra.

![Másolja a varázsló - az adatforrás kiválasztása lapon](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

Töltse ki az Azure Blob-tároló fiók kapcsolati adatait, és kattintson a **Tovább**gombra.

![Varázsló - adatforrások kapcsolat adatainak másolása](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

Válassza a TPC-H sortétel fájlokat tartalmazó **mappát** , és kattintson a **Tovább**gombra.

![Másolja a varázsló - bemeneti mappa kiválasztása](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

Alapján a **következő**gombra kattintott, a fájl formázási beállításokat előforduló automatikusan.  Győződjön meg arról, hogy az adott oszlop határolójel "|}"helyett az alapértelmezett vessző",".  Miután meg van megtekintése az adatokat, kattintson a **Tovább** gombra.

![Másolja a varázsló - fájl formátuma beállításai](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>Lépés 3: Állítsa be a cél
Ez a szakasz megtudhatja, hogy miként állítsa be a cél: `lineitem` az Azure SQL-adatraktár adatbázistáblába.

Válassza az **Azure SQL-adatraktár** céltár, és kattintson a **Tovább**gombra.

![Varázsló - választó céltárat adatok másolása](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

Töltse ki az Azure SQL-adatraktár kapcsolati adatait.  Győződjön meg arról, akkor adja meg a felhasználót, hogy tagja a szerepkörnek `xlargerc` (lásd a **vonatkozó követelmények** részletes útmutató), és kattintson a **Tovább**gombra. 

![A varázsló - cél csatlakozási adatok másolása](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

Válassza a célként használt táblát, és kattintson a **Tovább**gombra.

![Másolja a varázsló - táblázat leképezés lapján](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

Elfogadhatja az alapértelmezett beállításokat, az Oszlopok megfeleltetése, és kattintson a **Tovább**gombra.

![Másolja a varázsló - séma leképezés lapján](media/data-factory-load-sql-data-warehouse/schema-mapping.png)


## <a name="step-4-performance-settings"></a>Lépés: 4: Teljesítmény beállításai

**Engedélyezés polybase** alapértelmezés szerint be van jelölve.  Kattintson a **Tovább**gombra.

![Másolja a varázsló - séma leképezés lapján](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>Lépés 5: Telepítése, és figyelemmel követheti a betöltés eredménye
Kattintson a üzembe **Befejezés** gombra. 

![Másolja a varázsló - összefoglalási lapja](media/data-factory-load-sql-data-warehouse/summary-page.png)

Kattintson a telepítés befejezése után `Click here to monitor copy pipeline` a másolás, futtassa a haladás figyelemmel.

Jelölje ki a a **Tevékenység Windows** listában létrehozott másolása során.

![Másolja a varázsló - összefoglalási lapja](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

A részletek futtassa a **Tevékenység ablak Explorer** kattintson a jobb oldali ablaktábla, többek között az adatok mennyiségi forrásból származó olvashatók, és írja be a cél, időtartam és az átlag átviteli futtatáshoz példány megtekintése.

Az alábbi képernyőfelvételen az látható, mint 14 percben hatékony elérése 1.22 GB/s átviteli Azure Blob-tárolóhoz SQL adatraktár az 1 TB másolását tartott!

![Másolja a varázsló - sikeres párbeszédpanel](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Ajánlott eljárások
Íme néhány gyakorlati tanácsok az Azure SQL-adatraktár adatbázis fut:

- Nagyobb erőforrás osztály használja, az INDEX létrehozása csoportosított COLUMNSTORE betöltésekor.
- A hatékonyabb illesztések fontolja meg kivonat terjesztési kerek Ágnes terjesztési alapértelmezett helyett válassza oszlop szerint.
- A gyorsabb betöltése sebessége fontolja meg halommemória tranziens adatokhoz.
- Hozzon létre a statisztika Azure SQL-adatraktár betöltése után.

Lásd: [Gyakorlati tanácsok az Azure SQL-adatraktár](../sql-data-warehouse/sql-data-warehouse-best-practices.md) további információt. 

## <a name="next-steps"></a>Következő lépések
- [Az adatok gyári másolás varázsló](data-factory-copy-wizard.md) – Ez a cikk ismerteti a másolás varázsló részleteit. 
- [Másolás tevékenységet a teljesítmény és a beállítási útmutatója](data-factory-copy-activity-performance.md) – Ez a cikk a teljesítmény mérési és a beállítási útmutatója tartalmaz.

