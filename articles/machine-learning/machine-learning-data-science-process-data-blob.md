<properties 
    pageTitle="A speciális analytics Azure blob-adatok feldolgozására |}] A Microsoft Azure" 
    description="Azure Blob-tároló folyamat adatait." 
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
    ms.date="09/19/2016"
    ms.author="fashah;garye;bradsev" /> 

#<a name="heading"></a>Speciális analytics Azure blob adatok feldolgozása

Ez a dokumentum foglalkozik adatok feltárása és áramfejlesztő szolgáltatások Azure Blob-tároló tárolt adatokból. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Az adatok betöltése Pandas adatok keretbe
Feltárása és kezelése a dataset, azt kell letölteni a blob-forrás egy helyi fájlban, amely majd Pandas adatok keretbe lehet betölteni. Ez az eljárás a lépések a következők:

1. Letöltés adatait Azure blob-kódot a következő minta Python blob szolgáltatás használatával. Cserélje ki a változó a kód alatt saját értékekkel: 

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


2. Az adatok keretbe Pandas adatok-a letöltött fájlból olvashatók.

        #LOCALFILE is the file path 
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Most már készen áll az adatok feltárása, és ehhez a DataSet adatkészlethez szolgáltatások létrehozása.


##<a name="blob-dataexploration"></a>Adatok feltárása

Íme néhány példa a Pandas adatok feltárása:

1. Nézze meg a sorok és oszlopok száma 

        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape

2. Nézze meg az alábbi adatkészlet első vagy utolsó néhány sora:

        dataframe_blobdata.head(10)
        
        dataframe_blobdata.tail(10)

3. Ellenőrizze az egyes oszlopok importálták, használja az alábbi mintakódot adattípus
    
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype

4. Az alábbiak szerint ellenőrizze az oszlopok az adatkészlet alapvető statisztikák
 
        dataframe_blobdata.describe()
    
5. Nézze meg minden oszlop értékét a bejegyzések száma a következő

        dataframe_blobdata['<column_name>'].value_counts()

6. Hiányzó értékek számlálása és bejegyzések minden oszlopban a következő mintakód segítségével tényleges száma

        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
     
7.  Ha az adatok egy adott oszlop hiányzó értékek, megtehetjük, hogy azokat a következők szerint:

        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape

    Másik módja, hogy cserélje ki a hiányzó értékek mód függvény a következő:
    
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        

8. A hisztogram mintaterület rekeszek száma változó segítségével rajzoljuk meg a terjesztési egy változó létrehozása 
    
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
        
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    
9. Nézze meg a scatterplot vagy a beépített korrelációs függvény használatával változók közötti összefüggések

        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
        
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    
    
##<a name="blob-featuregen"></a>A szolgáltatás létrehozása
    
Azt is létrehozhat szolgáltatások használatával Python az alábbiak szerint:

###<a name="blob-countfeature"></a>Mutató értéke alapján a szolgáltatás létrehozása

A kockák szolgáltatások az alábbiak szerint hozható létre:

1. Nézze meg a kockák oszlop megoszlása:
    
        dataframe_blobdata['<categorical_column>'].value_counts()

2. Az oszlop értékeit jelző értékek készítése

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. A jelzőoszlopban az eredeti adatok keretben való csatlakozás 
 
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Távolítsa el az eredeti változó magát:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)
    
###<a name="blob-binningfeature"></a>Binning szolgáltatás létrehozása

Binned szolgáltatások létrehozásához, akkor a következőképpen járjunk el:

1. Adjunk hozzá egy számoszlopot bin oszlopok sorozata
 
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
        
2. Logikai változók sorozatát binning konvertálása

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
    
3. Végül csatlakozzon a dummy változók vissza az eredeti adatok keretben

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool) 


##<a name="sql-featuregen"></a>Adatok írása vissza Azure blob, és sok az Azure gép tanulás

Hogy kiaknázzák a az adatokat, és hozta létre a szükséges szolgáltatások, feltöltheti az adatok után (mintát vagy featurized), egy Azure blob, és ezért használja fel a következő lépésekkel Azure gép tanulás: Jegyezze fel az Azure gép tanulás Studio, valamint további szolgáltatások hozható létre. 
1. Az adatok keret helyi fájl írása

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Az adatok a következőképpen Azure blob feltöltése:

        from azure.storage.blob import BlobService
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

3. Az adatok olvasható Azure gép tanulás [Behozatali adatok] felhasználásával a blob objektumból most[ import-data] modul az alábbi képernyőn látható módon:
 
![olvasó blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
 
