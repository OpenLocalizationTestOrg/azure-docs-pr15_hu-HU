<properties
    pageTitle="Adatok funkciók létrehozása az SQL Server SQL és Python |} Microsoft Azure"
    description="Az SQL Azure folyamat adatainak"
    services="machine-learning"
    documentationCenter=""
    authors="bradsev"
    manager="jhubbard"
    editor="" />

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/19/2016"
    ms.author="bradsev;fashah;garye" />


# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>Az adatok funkciók létrehozása az SQL Server SQL és Python használatával


A dokumentum készítése egy SQL Server virtuális gép Azure tárolt adatok, az adatok hatékonyabb tanuljon algoritmusok segítő funkcióinak mutatja. Ez történik SQL használatával, vagy egy programnyelv, például a Python, mindkettő időrendi itt igazolni használatával.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Ez a **menüben** , amelyek bemutatják, hogyan hozhat létre az adatok szolgáltatásai különböző környezetekben témakörökre mutató hivatkozásokat tartalmaz. Ezt a műveletet a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)a lépés nem.

> [AZURE.NOTE] Gyakorlati például tekintse meg a [következőt: Taxi adatkészlet](http://www.andresmh.com/nyctaxitrips/) , és kérje meg a [következőt: adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) című-végpontok közötti segédlet az IPNB.


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy:

* Az Azure tároló fiókot hozta létre. Ha utasításokat van szüksége, olvassa el az [Azure tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)
* Az SQL Server tárolja az adatokat. Ha nem, olvassa el [az Azure SQL-adatbázishoz Azure gépi tanulási az adatok áthelyezése](machine-learning-data-science-move-sql-azure.md) kapcsolatos tudnivalókat az adatok áthelyezéséhez van.


## <a name="sql-featuregen"></a>Az SQL szolgáltatás előállítása

Ebben a részben azt ismertetik, hogyan a szolgáltatások használata SQL létrehozása:  

1. [Előfordulások megszámlálása szolgáltatás előállítása](#sql-countfeature)
2. [A szolgáltatás generációs binning](#sql-binningfeature)
3. [A szolgáltatások egy egyetlen oszlopból helyezése](#sql-featurerollout)


> [AZURE.NOTE] További funkciókkal hoz létre, ha a meglévő táblához oszlopként hozzáadása őket, vagy hozzon létre egy új táblázatot a Továbbiak és elsődleges kulcsot, az eredeti táblázatot illeszthető.

### <a name="sql-countfeature"></a>Előfordulások megszámlálása szolgáltatás előállítása

A dokumentum létrehozása darab szolgáltatások kétféle módon mutatja be. Az első módszert használja a Feltételes összegzés, valamint a második módszer a "Ha" záradék. Ezek is majd kell csatlakoznia az eredeti tábla (elsődleges kulcs oszlopok használatával) a darab szolgáltatásokat az eredeti adatok mellett.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>A szolgáltatás generációs binning

Az alábbi példa szemlélteti binned szolgáltatások készítése (5 intervallumok használata) binning használható szolgáltatásként helyett numerikus oszlop:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>A szolgáltatások egy egyetlen oszlopból helyezése

Ebben a részben azt szemléltetik bevezetési egyetlen oszlop egy táblázatban további funkciókkal létrehozásához. A példa feltételezi, hogy nincs-e a szélesség és hosszúság oszlop a táblázat, amelyből próbál szolgáltatások készítése.

Az alábbiakban rövid alapozó a szélesség és hosszúság helyadatok (a stackoverflow forrásokat `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`). Ez a hely mezőben előtt featurizing megértéséhez hasznos:

- A bejelentkezési közli velünk e vagyunk Észak- és Dél, kelet vagy nyugati a földgömbön.
- A nullától különböző több száz számjegy közli velünk programmal mutatjuk be hosszúság, nem szélesség!
- A tízesre számjegyet a helyzetben, hogy körülbelül 1000 kilométer adja vissza. Milyen földrész vagy a vagyunk óceán hasznos információt nyújt us.
- Az erőforrás-mennyiség számjegy (egy decimális fok) olyan helyzetben legfeljebb 111 kilométer (60 tengeri mérföld, körülbelül 69 mérföld) adja vissza. Azt is mondja el nagyjából milyen nagy állam vagy mindannyian az ország.
- Az első tizedes ér legfeljebb 11.1 km: azt, különböző a szomszédos nagy város egy nagy városra pozícióját.
- A második tizedes ér legfeljebb 1.1-es km: azt is egy falu elválasztja a Tovább gombra.
- A harmadik tizedes ér legfeljebb 110 m. azt azonosítani tudja nagy mezőgazdasági mező vagy intézményi mellett.
- A negyedik tizedes ér legfeljebb 11 m. egy azonosítania azt. Egy nem javított GPS egység tipikus pontosságát összehasonlíthatók nem zavarja.
- Az ötödik tizedes ér legfeljebb 1.1-es m. azt fák megkülönböztetni egymástól. Ez az engedélyszint kereskedelmi GPS egységekkel pontosság csak a megkülönböztető korrekciós lehet elérni.
- A hatodik tizedes ér legfeljebb értéke 0,11 m., amellyel a tájak, tervezéséhez részletesen struktúrák elrendezése utakat épület. Több mint elég jó glaciers és folyókat mozgásának nyomon kell lennie. Ez a GPS, például differentially korrigált GPS painstaking intézkedéseket érhető el.

A hely információkat is lehet featurized az alábbiak szerint meg a régió, a hely és a Város oszlop különválasztó. Megjegyzendő, hogy egyszer is felhívhatja, ha egy FENNMARADÓ végpontot, például a Bing Maps API a `https://msdn.microsoft.com/library/ff701710.aspx` régió/körzet információkért.

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1       
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

A fenti alapú hely funkciók további használható további darab szolgáltatások készítése a ismertetett módon.


> [AZURE.TIP] A rekordok, használja a választott nyelv programozás útján is beszúrhat. Előfordulhat, hogy szövegadattömb javítható az írási hatékonyság [nézze meg az ehhez pyodbc az alábbi példa](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)a illessze be az adatokat.
Másik lehetőség az adatok beszúrása az adatbázis [BCP segédprogram](https://msdn.microsoft.com/library/ms162802.aspx) használata

### <a name="sql-aml"></a>Kapcsolódás az Azure gépi tanulási

Az újonnan létrehozott funkció meglévő táblához oszlopként hozzáadhatók vagy új táblában tárolt és gépi tanulási az eredeti tábla illesztett. Szolgáltatások is létrehozott, vagy elérhető, ha már létrehozott, modulról az [Adatok importálása](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) az Azure Machine Learning alább látható módon:

![azureml olvasók](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Egy programnyelv, például a Python használatával

Python használatával szolgáltatások létrehozásához, ha az adatokat az SQL Server hasonlít az Azure blob- [környezetben, adatok tudományos folyamat Azure Blob-adatok](machine-learning-data-science-process-data-blob.md)dokumentált Python használatával adatfeldolgozás. Az adatok az adatbázisból származó adatok pandas keretbe tölthető kell, és ezután dolgozható további. Az adatbázis kapcsolódik, és az adatok betöltésének ebben a részben adatok keretbe folyamata a dokumentum azt.

A következő formátumban a kapcsolati karakterlánc Python pyodbc (csere kiszolgálónév, dbname, felhasználónév és jelszó az egyedi értékeket tartalmazó) használ az SQL Server-adatbázis csatlakoztatása használható:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

A [tár Pandas](http://pandas.pydata.org/) Python adatszerkezet és adatelemző eszközök széles körű nyújt a Python programozási adatfeldolgozás. Az alábbi kód beolvassa az eredmény SQL Server-adatbázisból származó adatok Pandas keretbe:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Most már dolgozhat a Pandas adatok keret [létrehozása az Azure blob-tároló adatok Panda használatával szolgáltatásai](machine-learning-data-science-create-features-blob.md)témakörök hatálya alá.
