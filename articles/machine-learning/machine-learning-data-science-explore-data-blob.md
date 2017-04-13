<properties 
    pageTitle="Azure blob-tárolóhoz Pandas az adatok vizsgálata |} Microsoft Azure" 
    description="Hogyan lehet az Azure blob-tárolóhoz Pandas használatával tárolt adatok vizsgálata." 
    services="machine-learning,storage" 
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

#<a name="explore-data-in-azure-blob-storage-with-pandas"></a>Azure blob-tárolóhoz Pandas az adatok vizsgálata

A dokumentum bemutatja, hogy miként [Pandas](http://pandas.pydata.org/) Python csomag használata Azure blob-tárolóban tárolt adatok vizsgálata.

A következő **menü** témakörökre mutató hivatkozásokat, amelyek az adatok vizsgálata különböző tárterület-környezetből eszközök segítségével. A feladat végrehajtásához az [Adatok tudományos folyamat]()lépése.

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]


## <a name="prerequisites"></a>Előfeltételek
Ez a cikk tartalma feltételezi, hogy:

* Az Azure tároló fiókot hozta létre. Ha utasításokat van szüksége, olvassa el az [Azure tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)
* Azure blob-tároló fiók tárolja az adatokat. Utasítások van szüksége, olvassa el az [adatok áthelyezése, hogy és Azure-tárhelyről](../storage/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Adatok betöltése a Pandas DataFrame
Ismerje meg, és egy adatkészlet módosítására, azt kell először le a blob forrásból származó a helyi fájlok tölthetők egy Pandas DataFrame majd. Kövesse az ebben a példában szereplő lépések a következők:

1. Letöltés az adatokat az Azure blob-mintával a következő Python kód blob-szolgáltatás használatával. A következő kódrészlet a változó cserélje ki az egyedi értékeket: 

        from azure.storage.blob import BlobService
        import tables
        
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>

        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))


2. Keretbe Pandas adatok – a letöltött fájlból, olvassa el az adatokat.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Most már készen áll az adatok feltárása és készítése a adatkészlet funkciókkal.

##<a name="blob-dataexploration"></a>Adatok feltárása Pandas használatával példák

Íme néhány példa az adatok vizsgálata Pandas használatával módjai:

1. Nézze meg a **sorok és oszlopok száma** 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. **Vizsgálat** az első vagy utolsó néhány **sorok** az alábbi adatkészlet:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Minden egyes oszlopát importálta, mint az alábbi példa kódot a **adattípusának** ellenőrzése
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Jelölje be az alábbi képlettel történik az adatkészlet oszlopait a **egyszerű stat**
 
        dataframe_blobdata.describe()
    
5. Az alábbi képlettel történik meg a minden oszlop értéke bejegyzéseinek száma

        dataframe_blobdata['<column_name>'].value_counts()

6. **Hiányzó értékek száma** és a tényleges száma tételek az egyes oszlopok a következő példa kód használatával

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Ha egy adott oszlop a **hiányzó értékeket** az adatok, húzhatja őket az alábbi képlettel történik:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Hiányzó értékek cseréje másik módja meg, a MÓDUSZ függvény:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. Hozzon létre egy **hisztogramot** ábra használata változó intervallumok száma: egy változó eloszlását ábrázolandó 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Tekintse meg **összefüggések** változók használata egy scatterplot vagy beépített korrelációs függvénnyel között

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
