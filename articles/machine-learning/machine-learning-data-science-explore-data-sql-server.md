<properties 
    pageTitle="Adatok az SQL Server virtuális gép feltárása a Azure |} Microsoft Azure" 
    description="Hogyan lehet egy Azure virtuális SQL Server gép tárolt adatok vizsgálata." 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="jhubbard" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/13/2016" 
    ms.author="bradsev" /> 

#<a name="explore-data-in-sql-server-virtual-machine-on-azure"></a>Adatok az SQL Server virtuális gép Azure szolgáltatásainak megismerése


A dokumentum bemutatja, hogy miként egy Azure virtuális SQL Server gép tárolt adatok vizsgálata. Ez történik adatok wrangling használata SQL vagy egy programnyelv, például a Python használatával.

A következő **menü** témakörökre mutató hivatkozásokat, amelyek az adatok vizsgálata különböző tárterület-környezetből eszközök segítségével. Ezt a műveletet nem lépés a Cortana Analytics folyamat (kap).

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


> [AZURE.NOTE] A minta SQL-utasításait a dokumentumban feltételezik, hogy adatokat az SQL Server. Ha nem, keresse meg a felhőben adatok tudományos folyamat térképen, így megtudhatja, hogy miként adatok áthelyezése SQL Server.



## <a name="sql-dataexploration"></a>Az SQL-parancsfájlok SQL-adatok feltárása

Íme néhány példa SQL-parancsfájlok adatokat tárolja az SQL Server feltárása használható.

1. Napi észrevételek megszámlálása

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. Ismerkedés a kockák oszlop szintek

    `select  distinct <column_name> from <databasename>`

3. Kapcsolatfelvétel a szintek számát két kockák oszlopok kombinációja 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. A numerikus oszlopok eloszlás beszerzése

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] Gyakorlati például a [következőt: Taxi adatkészlet](http://www.andresmh.com/nyctaxitrips/) és használhatja a [következőt: adatok wrangling IPython jegyzetfüzet és az SQL Server használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) című-végpontok közötti segédlet az IPNB hivatkozik.

##<a name="python"></a>SQL adatait Python

Adatok feltárása és készítése a szolgáltatások, ha az adatokat az SQL Server használatával Python hasonlít az adatfeldolgozás az Azure blob-Python, használja a [folyamat Azure Blob-adatok a adatok tudományos környezetben](machine-learning-data-science-process-data-blob.md)ismertetett módon. Az adatok az adatbázisból töltődnek be a pandas DataFrame kell, és ezután dolgozható további. Az adatbázis kapcsolódik, és az adatok betöltésének az ebben a szakaszban a DataFrame folyamata a dokumentum azt.

A következő formátumban a kapcsolati karakterlánc Python pyodbc (csere kiszolgálónév, dbname, felhasználónév és jelszó az egyedi értékeket tartalmazó) használ az SQL Server-adatbázis csatlakoztatása használható:

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

A [tár Pandas](http://pandas.pydata.org/) Python adatszerkezet és adatelemző eszközök széles körű nyújt a Python programozási adatfeldolgozás. A következő kódot beolvassa az eredmény SQL Server-adatbázisból származó adatok Pandas keretbe:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

Most már dolgozhat a Pandas DataFrame a témakör [az adatok tudományos környezetben folyamat Azure Blob-adatok](machine-learning-data-science-process-data-blob.md)hatálya alá.

## <a name="cortana-analytics-process-in-action-example"></a>Művelet a példában a Cortana Analytics folyamat

Végpont – útmutató példát a Cortana Analytics folyamat egy nyilvános adatkészlet használ, lásd: [a csapat adatok tudományos folyamat működését: az SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).

 
