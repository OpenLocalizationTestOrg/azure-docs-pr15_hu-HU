<properties
   pageTitle="Azure BLOB-tároló olyan adatokat másol tó adattár |} Microsoft Azure"
   description="Adatok másolása Azure BLOB-tároló tó adattár AdlCopy eszközzel"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Adatok másolása Azure BLOB-tároló tó adattárhoz

> [AZURE.SELECTOR]
- [DistCp használata](data-lake-store-copy-data-wasb-distcp.md)
- [AdlCopy használata](data-lake-store-copy-data-azure-storage-blob.md)

Azure tó adattár parancssori eszköz, [AdlCopy](http://aka.ms/downloadadlcopy)adatok másolása a következő forrásokból nyújtja:

* A tároló Azure BLOB tó tárolóba. Adatok másolása tó adattár tároló Azure BLOB AdlCopy nem használható.

* Között két tó adattár Azure-fiók. 

Is használhatja a AdlCopy eszköz két különböző módokban:

* **Önálló**, ahol az eszköz tó adattár az erőforrásokat használó a feladatnak a végrehajtásához.

* **Adatok tó Analytics-fiókja**, ahol az adatok tó Analytics-fiókjához rendelt egységek használják a másoláshoz. Érdemes lehet használja ezt a lehetőséget, ha a Másolás feladatok elvégzéséhez kiszámíthatóan módon keresi.

##<a name="prerequisites"></a>Előfeltételek

Ez a cikk megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).

- **Azure BLOB-tároló** tároló néhány adatokkal.

- **Azure adatok tó Analytics-fiókot (nem kötelező)** – lásd: [első lépések az Azure adatok tó Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) módjáról tó adattár fiók létrehozása.

- **AdlCopy eszközt**. Telepítse a AdlCopy eszköz [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>A AdlCopy eszköz szintaxisa

A következő szintaxissal a AdlCopy eszköz használata

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern 

A paraméterek az alábbi az alábbiakban ismertetjük:

| A beállítás    | Leírás                                                                                                                                                                                                                                                                                                                                                                                                          |
|-----------|------------|
| Forrás    | Adja meg a forrásadatok helyét a tárhely Azure blob. A forrás lehet blob-tároló, blob vagy egy másik tó adattár fiókot.                                                                                                                                                                                                                                                                                                 |
| Cél      | Adja meg a tó adattár cél szeretné másolni.                                                                                                                                                                                                                                                                                                                                                                |
| SourceKey | Adja meg a tárhely hívóbetű tároló Azure blob forrást. Szükség, csak ha az adatforrás blob-tároló vagy blob.                                                                                                                                                                                                                                                                                                                                                 |
| Fiók   | **Nem kötelező**. Használja, ha meg szeretné futtatni a másolási feladatot Azure adatok tó Analytics-fiókom segítségével. Ha használja az alábbi /Account beállítását, de nem ad meg egy adatokat tó Analytics-fiókkal, a AdlCopy alapértelmezett fiókot használ a feladat futtatása. Is ha ezt a beállítást használja, hozzá kell adnia a forrás (Azure tároló Blob) és a cél (Azure tó adattár) adatforrásaként az adatok tó Analytics-fiók.  |
| Erőforrás-mennyiség     |  A projekt másolása használt adatok tó Analytics egységek számát adja meg. Ez a beállítás akkor kötelező, ha a **/Account** beállítás használatával adja meg az adatok tó Analytics-fiókot.
| Minta   | Itt adhatja meg, amely jelzi, mely BLOB vagy másolni kívánt fájlokat regex mintát. AdlCopy használja a kis-és nagybetűket egyező. Az alapértelmezett mintának használják, amikor nincs minta van megadva, hogy az összes elem másolása. Több fájl mintázatot tartalmazó nem támogatott.                                                                                                                                                                                                                                                                                                                                               



## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>Használni AdlCopy (önálló)-tárhely Azure blob adatok másolása

1. Nyisson meg egy parancssort, és keresse meg a könyvtár, ahol AdlCopy telepítve van, a szokásos `%HOMEPATH%\Documents\adlcopy`.

2. A következő parancsot a forrás tároló egy adott blob másolása tó adattár:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Példa:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    
    >[AZURE.NOTE] A fenti szintaxis itt tó adattár fiók mappába másolni kívánt fájlt. Ha a megadott mappa neve nem létezik, AdlCopy eszköz létrehoz egy mappát.

    Az Azure előfizetés, amelyek alapján a tó adattár-fiókja van a hitelesítő adatok megadását kéri. A kibocsátás, az alábbihoz hasonló jelennek meg:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. A következő parancsot a tó adattár fiókra egy tárolóból is másolhatja valamennyi BLOB:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Példa:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>Használható AdlCopy (egyedülálló) adatok másolása egy másik tó adattár fiókból

AdlCopy két tó adattár fiókok között az adatok másolása is használhatja.

1. Nyisson meg egy parancssort, és keresse meg a könyvtár, ahol AdlCopy telepítve van, a szokásos `%HOMEPATH%\Documents\adlcopy`.

2. A következő parancsot egy adott fájl másolása tó adattár fiókok a másikba.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Példa:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

    >[AZURE.NOTE] A fenti szintaxis adja meg a fájlt a cél tó adattár fiók mappába másolni. Ha a megadott mappa neve nem létezik, AdlCopy eszköz létrehoz egy mappát.

    Az Azure előfizetés, amelyek alapján a tó adattár-fiókja van a hitelesítő adatok megadását kéri. A kibocsátás, az alábbihoz hasonló jelennek meg:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

3. A következő parancs másolja át az összes fájl a forrásban tó adattár fiók egy adott mappában a cél tó adattár fiók mappába.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>Használata AdlCopy (adatok tó Analytics-fiók) adatok másolása

Adatok tó Analytics-fiókját az adatok másolása tároló Azure BLOB tó adattár AdlCopy feldolgozás is használhatja. A szokásos használja ezt a lehetőséget a tartomány gigabájt és terabájt az adatok áthelyezésére esetén, és azt szeretné, hogy jobban és kiszámítható teljesítmény kapacitása.

Adatok tó Analytics-fiókja használata AdlCopy-tároló Azure Blob másolni szeretné, a forrás (Azure tároló Blob) hozzá kell adni az adatok tó Analytics-fiók adatforrásként. További adatforrásokhoz hozzáadása a adatok tó Analytics-fiókhoz, tanulmányozza [kezelése adatok tó Analytics fiók adatforrásokhoz](..//data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-account-data-sources).

>[AZURE.NOTE] Ha a másolandó Azure tó adattár fiók adatok tó Analytics-fiókja forrásaként, nem szükséges a tó adattár fiók társítása az adatok tó Analytics-fiókhoz. Az adatforrás tároló társíthatja az adatok tó Analytics-fiók kötelező csak akkor, ha a forrás nem Azure tárterület-fiók.

Futtassa a tó adattár fiókkal történő adatok tó Analytics-tároló Azure blob másolja a következő parancsot:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Példa:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2


Hasonlóképpen futtassa a egy tó adattár fiókkal történő adatok tó Analytics-tároló Azure blob másolja a következő parancsot:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>Használja a mintával egyező adatok másolása AdlCopy használatával

Ebben a szakaszban megismerheti, hogyan AdlCopy használatával forrásból származó adatok másolása (az alábbi példában használjuk Azure Blob-tároló) a cél tó adattár fiókja mintákban. Ha például az alábbi lépésekkel is használhatja a forrás blob összes .csv kiterjesztésű fájlt a megadott helyre másolja.

1. Nyisson meg egy parancssort, és keresse meg a könyvtár, ahol AdlCopy telepítve van, a szokásos `%HOMEPATH%\Documents\adlcopy`.

2. Futtassa a forrás tárolóból tó adattár az adott blob összes *.csv kiterjesztésű fájlt másolja a következő parancsot:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Példa:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

    
## <a name="billing"></a>A számlázás

* A AdlCopy eszköz önálló használatakor számlázott kilépési költségekre való áthelyezéséhez az adatokat, ha a forrás Azure tárterület-fiókhoz, nem pedig az adatok tó üzlet ugyanabban a régióban.

* A AdlCopy eszközzel adatok tó Analytics-fiókjához, ha a [Számlázási adatok tó Analytics-mértékek](https://azure.microsoft.com/pricing/details/data-lake-analytics/) érvényesek lesznek.

## <a name="considerations-for-using-adlcopy"></a>AdlCopy használatával kapcsolatos szempontok

* AdlCopy (verziójához 1.0.5), a Másolás származó adatokat a lekérdezéslépések rendelkező együttesen legfeljebb ezer fájlok és mappák támogatja. Azonban ha problémák a másolása egy nagy adatkészletet, akkor is a fájlok és mappák terjesztése más almappákba, és használja az elérési utat a alszint mappákra forrásaként.

## <a name="next-steps"></a>Következő lépések

- [Biztonságos adattár tó adatok](data-lake-store-secure-data.md)
- [Azure adatok tó Analytics használata tó adattárhoz](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure hdinsight szolgáltatáshoz használata tó adattárhoz](data-lake-store-hdinsight-hadoop-use-portal.md)
