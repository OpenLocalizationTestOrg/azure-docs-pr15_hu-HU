<properties 
    pageTitle="Mintaadatok az Azure blob-tároló |} Microsoft Azure" 
    description="Mintaadatok az Azure Blob-tárolóhoz" 
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

#<a name="heading"></a>Mintaadatok az Azure blob-tároló


A dokumentum mintavételnél tárolóban lévő adatok Azure blob programozás útján letölti és majd mintavételi Python nyelven íródott eljárások használatával foglalkozik.

**Miért példa az adatok?**
Az adatkészlet szeretné elemezni túl nagy méretű, esetén általában célszerű az lefelé kétmintás az adatokat egy kisebb, de képviselő és könnyebben kezelhető méretének csökkentése érdekében. Ez lehetővé teszi az adatok ismertetése, feltárására és szolgáltatás műszaki. A Cortana Analytics folyamat szerepkörében ahhoz, hogy az adatkezelési és gépi tanulási modellek gyors prototípusának elkészítéséhez.

A **menü** alatti, amelyek az adatokat a különböző tároló környezetekben témakörökre mutató hivatkozásokat. 

[AZURE.INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

A feladat végrehajtásához mintavételnél Ez a lépés a [Csapat adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).


## <a name="download-and-down-sample-data"></a>Töltse le és lefelé mintaadatok
1. Adatok letöltése a blob szolgáltatással az alábbi példa Python kódot Azure blob-tárolóhoz: 

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

2. Olvassa el az adatok keretbe Pandas adatok-feletti letöltött fájlból.

        import pandas as pd

        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Az adatok használatával lefelé minta a `numpy`'s `random.choice` az alábbi képlettel történik:

        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Most már dolgozhat a fenti adatok keret további Ismerkedés céljából később és szolgáltatás generációs 1 % mintával.

##<a name="heading"></a>Töltse fel az adatokat, és elolvasni az Azure gépi tanulási

A következő példa kód segítségével lefelé kétmintás az adatokat, és közvetlenül az Azure Machine Learning használni:

1. Írja be az adatok keret helyi fájlként

        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. A helyi fájl feltöltése egy Azure blob-a következő példa kód használatával:

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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. Az adatok olvassák az Azure blob- [Adatok importálása](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) Azure Machine Learning használ, az alábbi képen látható módon:
 
![a képernyőolvasók blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

 
