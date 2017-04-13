<properties
    pageTitle="Azure blob-tároló adatok Panda használatával funkciók létrehozása |} Microsoft Azure"
    description="Hogy hozhatók létre, amely a Panda Python csomag Azure blob-tárolóban található adatok funkciók."
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
    ms.author="bradsev;garye" />

#<a name="create-features-for-azure-blob-storage-data-using-panda"></a>Azure blob-tároló adatok Panda használatával funkciók létrehozása

A dokumentum létrehozása az Azure blob-tárolóban a [Pandas](http://pandas.pydata.org/) Python csomag használata tárolt adatokhoz szolgáltatásai mutatja. Az adatok behelyezésére Panda adatok keretbe tagolás után azt szemlélteti, hogyan kockák szolgáltatások Python parancsfájlokkal jelző értékekkel és szolgáltatások binning létrehozásához.

[AZURE.INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]Ez a **menüben** , amelyek bemutatják, hogyan hozhat létre az adatok szolgáltatásai különböző környezetekben témakörökre mutató hivatkozásokat tartalmaz. Ezt a műveletet a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/)a lépés nem.


## <a name="prerequisites"></a>Előfeltételek

Ez a cikk feltételezi, hogy az Azure blob-tároló fiók nem hozott létre, és az adatok ott tárolt. Ha szüksége képernyőn megjelenő utasításokat követve állítson be egy fiókot, olvassa el az [Azure tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account)


## <a name="load-the-data-into-a-pandas-data-frame"></a>Adatok betöltése Pandas adatok keret
Ismerje meg, és módosíthatja egy adatkészletet, meg kell le a blob forrásból származó egy helyi fájl, amely ezután Pandas adatok keretekbe tölthető. Kövesse az ebben a példában szereplő lépések a következők:

1. Letöltés az adatokat az Azure blob-kódot a következő példa Python blob-szolgáltatás használatával. A kód alatt a változó cserélje ki az egyedi értékeket:

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

##<a name="blob-featuregen"></a>Szolgáltatás-generálás

A következő két szakaszok bemutatják, hogyan Python parancsfájlokkal kockák szolgáltatása jelző értékekkel és binning létrehozásához.

###<a name="blob-countfeature"></a>Mutató értékének alapú szolgáltatás előállítása

A kockák szolgáltatások hozható létre az alábbi képlettel történik:

1. Nézze meg az eloszlás kockák oszlop:

        dataframe_blobdata['<categorical_column>'].value_counts()

2. Készítése jelző értékek minden egyes oszlopértékek

        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')

3. Bekapcsolódás az eredeti adatok keret jelző oszloppal

            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)

4. Az eredeti változó magát eltávolítása:

        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

###<a name="blob-binningfeature"></a>A szolgáltatás generációs binning

Binned szolgáltatások előállításához az alábbi azt folytassa az alábbi képlettel történik:

1. Oszlopok egy számoszlopot bin sorozata hozzáadása

        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)

2. Logikai változó sorozatát binning konvertálása

        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')

3. Végezetül csatlakozzon az üres változók vissza az eredeti adatok kerethez

        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

##<a name="sql-featuregen"></a>Írás a vissza Azure blob és használata az Azure gépi tanulási más

Miután, hogy megismerkedett az adatokat, és hozta létre a szükséges szolgáltatások, az adatokat is feltölthet (mintát vagy featurized) szeretne egy Azure blob- és felhasználja azt az Azure gépi tanulási az alábbi lépésekkel: figyelje meg, hogy további funkciókkal az Azure gépi tanulási Studio is hozhat létre.
1. Írja be az adatok keret helyi fájl

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Töltse fel az adatok Azure blob az alábbi képlettel történik:

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

3. Most már az adatokat is olvasható a blob az [Adatok importálása](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) Azure gépi tanulási modulról, az alábbi képernyőképen látható módon:

![a képernyőolvasók blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)
