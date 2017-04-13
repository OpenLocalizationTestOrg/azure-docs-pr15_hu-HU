<properties 
    pageTitle="Az adatokat az SQL Server Azure |} Microsoft Azure" 
    description="Mintaadatok az SQL Server Azure" 
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
    ms.date="09/19/2016" 
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Mintaadatok az SQL Server Azure


A dokumentum mutatja az SQL Azure-kiszolgálón tárolt adatok minta SQL vagy a Python programnyelv használatával. Azt is megtudhatja, hogy miként Azure gépi tanulási fájlba menti, feltölteni az Azure blob, és olvassa Azure gépi tanulási Studio mintában szereplő adatok áthelyezése.

A Python mintavételnél csatlakoztatása az SQL Server Azure és a [Pandas](http://pandas.pydata.org/) tárat, végezze el a példákat talál arra, hogy a [pyodbc](https://code.google.com/p/pyodbc/) ODBC-tár használja.

>[AZURE.NOTE] A minta SQL-kódot a dokumentumban azt feltételezi, hogy az adatokat egy SQL Server Azure meg. Ha nem, olvassa el az adatok áthelyezése SQL Server Azure útmutatást [az SQL Server Azure adatok áthelyezése](machine-learning-data-science-move-sql-server-virtual-machine.md) című témakörben.

**Miért példa az adatok?**
Az adatkészlet szeretné elemezni túl nagy méretű, esetén általában célszerű az lefelé kétmintás az adatokat egy kisebb, de képviselő és könnyebben kezelhető méretének csökkentése érdekében. Ez lehetővé teszi az adatok ismertetése, feltárására és szolgáltatás műszaki. A [Csoportwebhely adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) a szerepkörében ahhoz, hogy az adatkezelési és gépi tanulási modellek gyors prototípusának elkészítéséhez.

A **menü** alatti, amelyek az adatokat a különböző tároló környezetekben témakörökre mutató hivatkozásokat. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

A feladat végrehajtásához mintavételnél Ez a lépés a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

##<a name="SQL"></a>SQL használatával

Ez a szakasz SQL használatával végezze el az adatok ellen egyszerű véletlen mintavételnél az adatbázis több lehetőség is kínálkozik ismerteti. Válasszon egy módszert a adatok méretét és az eloszlás alapján.

Az alábbi elemek megjelenítése newid használata SQL Server-alapú a mintavételnél végrehajtásához. A kiválasztott módszerrel, attól függ, hogyan véletlen, amelyet a minta (az alábbi példakódot pk_id-nek tekinti automatikusan létrehozott elsődleges kulcs).

1. Kisebb szigorú véletlenszerűen

        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())

2. További véletlenszerűen 

        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablesample is lehet példákat talál arra szolgál, valamint igazolni alatt. Ennek oka lehet jobb megközelítés, ha az adatok mérete nagy (feltételezve, hogy a különböző oldalakra adatok nem van összefüggésben), és a lekérdezés egy időtartamon befejezéséhez.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

>[AZURE.NOTE] Ismerje meg, és szolgáltatások készítése a mintában szereplő adatokat tárolja őket az új táblázat


###<a name="sql-aml"></a>Kapcsolódás az Azure gépi tanulási

Az [Adatok importálása] Azure gépi tanulási közvetlenül használhatja a minta lekérdezések feletti[ import-data] lefelé kétmintás menet közben az adatokat, és jelenítse meg az Azure gépi tanulási kísérlet modulra. Olvassa el a mintában szereplő adatokat a rendszer olvasó alkalmazásának modulról képernyőképe nézhet ki:
   
![a képernyőolvasók sql][1]

##<a name="python"></a>A programnyelven Python használatával 

Ez a szakasz bemutatja, hogy a [pyodbc tár](https://code.google.com/p/pyodbc/) létrehozására, az ODBC Python az SQL server-adatbázishoz való kapcsolódás használatával. Az adatbázis kapcsolat karakterlánca az alábbi képlettel történik: (cserélje kiszolgálónév, dbname, felhasználónevét és jelszavát a konfigurációban):

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

A [Pandas](http://pandas.pydata.org/) tárba, Python adatszerkezet és adatelemző eszközök széles körű nyújt a Python programozási adatfeldolgozás. Az alábbi kód beolvassa az adatok 0,1 % minta Azure SQL-adatbázis táblájához a egy Pandas adatokat:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

Most már dolgozhat a mintában szereplő adatokat a Pandas adatok keretbe. 

###<a name="python-aml"></a>Kapcsolódás az Azure gépi tanulási

A következő példa kód segítségével az lefelé mintát adatokat is menteni a fájlt, és töltse fel az Azure blob. Az adatok a blob is közvetlenül olvashatók Azure gépi tanulási kísérlet be az [Adatimportálás] [ import-data] modulra. A lépések a következők: 

1. Írja be a pandas adatok keret helyi fájlként

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Azure blob helyi fájl feltöltése

        from azure.storage import BlobService
        import tables

        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
        
        try:
       
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
        
        except:         
            print ("Something went wrong with uploading blob:"+BLOBNAME)

3. Azure blob- [Adatok importálása] Azure gépi tanulási használata olvassák[ import-data] modul az alábbi elhelyezni a képernyőn látható módon:
 
![a képernyőolvasók blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>A csoportwebhely adatok tudományos folyamat művelet példában

A csapat adatok tudományos folyamat végpont – útmutató példa használ egy nyilvános adatkészlet című [csapat adatok tudományos folyamat működését: SQL Server alkalmazás segítségével](machine-learning-data-science-process-sql-walkthrough.md).

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

 [import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
