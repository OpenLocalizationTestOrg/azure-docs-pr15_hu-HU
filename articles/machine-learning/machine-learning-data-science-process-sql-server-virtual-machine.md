<properties 
    pageTitle="Az SQL Azure adatfeldolgozás |} Microsoft Azure" 
    description="Az SQL Azure folyamat adatainak" 
    services="machine-learning" 
    documentationCenter="" 
    authors="garyericson" 
    manager="jhubbard" 
    editor="" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/16/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Az SQL Server Azure virtuális gép folyamat adatainak

A dokumentum bemutatja, hogy miként adatok feltárása és készítése egy SQL Server virtuális gép Azure tárolt adatok funkcióinak. Ez történik adatok wrangling használata SQL vagy egy programnyelv, például a Python használatával.


> [AZURE.NOTE] A minta SQL-utasításait a dokumentumban feltételezik, hogy adatokat az SQL Server. Ha nem, olvassa el a felhőben adatok tudományos folyamat térképen, így megtudhatja, hogy miként adatok áthelyezése SQL Server.

##<a name="SQL"></a>SQL használatával

Azt adja meg a következő adatok wrangling feladatok ebben a részben SQL használatával:

1. [Adatok feltárása](#sql-dataexploration)
2. [Szolgáltatás-generálás](#sql-featuregen)

###<a name="sql-dataexploration"></a>Adatok feltárása
Íme néhány példa SQL-parancsfájlok adatokat tárolja az SQL Server feltárása használható.


> [AZURE.NOTE] Gyakorlati például a [következőt: Taxi adatkészlet](http://www.andresmh.com/nyctaxitrips/) és használhatja a [következőt: adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) című-végpontok közötti segédlet az IPNB hivatkozik.

1. Napi észrevételek megszámlálása

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Ismerkedés a kockák oszlop szintek

    `select  distinct <column_name> from <databasename>`

3. Kapcsolatfelvétel a szintek számát két kockák oszlopok kombinációja 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. A numerikus oszlopok eloszlás beszerzése

    `select <column_name>, count(*) from <tablename> group by <column_name>`


###<a name="sql-featuregen"></a>Szolgáltatás-generálás

Ebben a részben azt ismertetik, hogyan a szolgáltatások használata SQL létrehozása:  

1. [Előfordulások megszámlálása szolgáltatás előállítása](#sql-countfeature)
2. [A szolgáltatás generációs binning](#sql-binningfeature)
3. [A szolgáltatások egy egyetlen oszlopból helyezése](#sql-featurerollout)


> [AZURE.NOTE] További funkciókkal hoz létre, ha a meglévő táblához oszlopként hozzáadása őket, vagy hozzon létre egy új táblázatot a Továbbiak és elsődleges kulcsot, az eredeti táblázatot illeszthető. 

###<a name="sql-countfeature"></a>Előfordulások megszámlálása szolgáltatás előállítása

A dokumentum létrehozása darab szolgáltatások kétféle módon mutatja be. Az első módszert használja a Feltételes összegzés, valamint a második módszer a "Ha" záradék. Ezek is majd kell csatlakoznia az eredeti tábla (elsődleges kulcs oszlopok használatával) a darab szolgáltatásokat az eredeti adatok mellett.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

###<a name="sql-binningfeature"></a>A szolgáltatás generációs binning

Az alábbi példa szemlélteti binned szolgáltatások készítése (5 intervallumok használata) binning használható szolgáltatásként helyett numerikus oszlop:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


###<a name="sql-featurerollout"></a>A szolgáltatások egy egyetlen oszlopból helyezése

Ebben a részben azt szemléltetik bevezetési egyetlen oszlop egy táblázatban további funkciókkal létrehozásához. A példa feltételezi, hogy nincs-e a szélesség és hosszúság oszlop a táblázat, amelyből próbál szolgáltatások készítése.

Az alábbiakban rövid alapozó a szélesség és hosszúság helyadatok (a stackoverflow forrásokat [hogyan kell a szélesség és hosszúság pontosságát?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)). Ez a hely mezőben előtt featurizing megértéséhez hasznos:

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

A helyére vonatkozó adatok lehet featurized az alábbiak szerint meg a régió, a hely és a Város oszlop különválasztó. Figyelje meg, hogy is felhívhatja a többi végpont, például a Bing Maps API érhető el a [végpontjánál hely keresése](https://msdn.microsoft.com/library/ff701710.aspx) a régió/körzet információ.

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

A fenti helyfüggő funkciók további használható további darab szolgáltatások ismertetett módon létrehozásához. 


> [AZURE.TIP] A rekordok, használja a választott nyelv programozás útján is beszúrhat. Előfordulhat, hogy az adatokat szövegadattömb írási hatékonyság (például egy művelet pyodbc használ, akkor olvassa el [A HelloWorld minta python az SQL Server eléréséhez](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)hogyan) hogyan szúrhat be. Másik lehetőség az adatok beszúrása az adatbázisban a [BCP segédprogramot](https://msdn.microsoft.com/library/ms162802.aspx).

###<a name="sql-aml"></a>Kapcsolódás az Azure gépi tanulási

Az újonnan létrehozott funkció meglévő táblához oszlopként hozzáadhatók vagy új táblában tárolt és gépi tanulási az eredeti tábla illesztett. Szolgáltatások hozza létre, vagy webböngészőn hozta létre, ha az [Adatok importálása] [ import-data] modul Azure gépi tanulási alább látható módon:

![azureml olvasók][1] 

##<a name="python"></a>Egy programnyelv, például a Python használatával

Adatok feltárása és készítése a szolgáltatások, ha az adatokat az SQL Server használatával Python hasonlít az Azure blob- [környezetben, adatok tudományos folyamat Azure Blob-adatok](machine-learning-data-science-process-data-blob.md)dokumentált Python használatával adatainak feldolgozása. Az adatok az adatbázisból származó adatok pandas keretbe tölthető kell, és ezután dolgozható további. Az adatbázis kapcsolódik, és az adatok betöltésének ebben a részben adatok keretbe folyamata a dokumentum azt.

A következő formátumban a kapcsolati karakterlánc Python pyodbc (csere kiszolgálónév, dbname, felhasználónév és jelszó az egyedi értékeket tartalmazó) használ az SQL Server-adatbázis csatlakoztatása használható:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

A [tár Pandas](http://pandas.pydata.org/) Python adatszerkezet és adatelemző eszközök széles körű nyújt a Python programozási adatfeldolgozás. Az alábbi kód beolvassa az eredmény SQL Server-adatbázisból származó adatok Pandas keretbe:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Most már dolgozhat a Pandas adatok keret hatálya alá [folyamat Azure Blob-adatok környezetben, adatok tudományos](machine-learning-data-science-process-data-blob.md)című témakörben.

## <a name="azure-data-science-in-action-example"></a>Azure adatok tudományos művelet példában

Egy végpont – útmutató példa az Azure adatok tudományos folyamat egy nyilvános adatkészlet használatával című témakörben [Azure adatok tudományos folyamat működését](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
