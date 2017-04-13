<properties
    pageTitle="Töltse fel az adatok HDInsight Hadoop feladatok |} Microsoft Azure"
    description="Megtudhatja, hogy miként tölthet fel, és adatainak Hadoop feladatok az Azure CLI, Azure tároló Intéző, Azure PowerShell, Hadoop parancssori vagy Sqoop HDInsight elérése."
    services="hdinsight,storage"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jgao"/>



#<a name="upload-data-for-hadoop-jobs-in-hdinsight"></a>Töltse fel az adatok HDInsight a Hadoop feladatokhoz

Azure hdinsight szolgáltatáshoz biztosít a teljes szolgáltatáskészletet nyújtó Hadoop elosztott fájlrendszer (Fájlrendszerhez) Azure Blob-tárolóhoz fölé. Ahhoz, hogy tal zökkenőmentesen ügyfelek Fájlrendszerhez kiterjesztése szolgál. Lehetővé teszi a skype_for_businesshoz közvetlenül az adatokon kezeli működtetéséhez Hadoop ökológiai összetevői. Azure Blob-tárolóhoz és Fájlrendszerhez rendszer egyedi fájl tárolására szolgáló adatok és számítások adatokon optimalizált. Információt milyen előnyökkel jár a Azure Blob-tároló használatára című témakörben talál [Azure blobtárolóhoz használata a HDInsight][hdinsight-storage].

**Előfeltételek**

Mielőtt elkezdené, vegye figyelembe az alábbi követelményeknek:

* Az Azure hdinsight szolgáltatáshoz fürt. Útmutatásért lásd: [első lépések az Azure hdinsight szolgáltatáshoz] [ hdinsight-get-started] vagy [rendelkezést HDInsight fürt][hdinsight-provision].

##<a name="why-blob-storage"></a>Miért blob-tároló?

Azure hdinsight szolgáltatáshoz fürt általában MapReduce feladatok futtatásához van telepítve, és a fürt megszakadnak, ezek a feladatok befejezése után. Az adatok a Fájlrendszerhez megőrzési fürt számítások befejezése után egy drága módja az adatok tárolására lenne. Azure Blob-tárolóhoz egy könnyen hozzáférhető, nagyon méretezhető, nagy beosztását, alacsony költség és megosztható tárolási lehetőség az adatok HDInsight lehet feldolgozni található. Adatok tárolása blob lehetővé teszi, hogy a HDInsight fürt biztonságosan kiadandó adatvesztés nélkül kiszámítása használható.

###<a name="directories"></a>Könyvtárak

Azure Blob-tároló tárolja az adatokat, kulcs/érték párokká, és nincs címtár hierarchiája. Azonban a "/" karakter használható belül a kulcs neve lehetőségtől, mintha egy fájlt a címtár-szerkezet tárolja. HDInsight ezek látja, mintha tényleges könyvtárak is.

Egy blob-kulcs lehet, például *input/log1.txt*. Nincs tényleges "beviteli" könyvtár létezik, de a kulcs nevét a "/" karakter jelenléte miatt az alábbi módokon, az elérési útra rendelkezik.

Emiatt Azure Explorer eszközök használatakor Észreveheti néhány 0 bájt fájlt. Ezek a fájlok két célokra szolgál:

- Ha üres mappák, azok a mappa megléte jele. Azure Blob-tárolóhoz elég intelligens tudnivalók, ha élőlá/sáv nevű blob létezik, az **élőlá**nevű mappa található. Azonban csak úgy lehet **élőlá** nevű mappa ürítése időtartamon úgy, hogy a speciális 0 bájt fájl helyen.

- A Hadoop fájlrendszer, különösen az engedélyek és a tulajdonosok a mappák van szükség speciális metaadatokkal rendelkeznek.

##<a name="command-line-utilities"></a>Parancssori

Microsoft Azure Blob-tárolóhoz készült alábbi segédprogramok nyújtja:

| Eszköz | Linux | OS X | A Windows |
| ---- |:-----:|:----:|:-------:|
| [Azure parancssor][azurecli] | ✔ | ✔ | ✔ |
| [Azure PowerShell][azure-powershell] | | | ✔ |
| [AzCopy][azure-azcopy] | | | ✔ |
| [Hadoop parancs](#commandline) | ✔ | ✔ | ✔ |

> [AZURE.NOTE] Külső Azure-parancs csak akkor áll rendelkezésre a HDInsight fürt, és csak az adatok betöltése a helyi fájlrendszerből történő Azure Blob-tárolóhoz engedélyezi a Hadoop közben az Azure CLI Azure PowerShell és AzCopy összes is használható.

###<a id="xplatcli"></a>Azure CLI

Az Azure CLI platformok segédprogram lehetővé teszi, hogy az Azure szolgáltatások kezelése. Töltse fel az adatok Azure Blob-tárolóhoz kövesse az alábbi lépéseket:

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

1. [Telepítse és állítsa be az Azure CLI for Mac, Linux és a Windows](../xplat-cli-install.md).

2. Nyissa meg egy parancssort, bash vagy más rendszerhéj, és használja az alábbi Azure-előfizetéséhez hitelesítést végezni.

        azure login

    Amikor a rendszer kéri, írja be a felhasználónevét és jelszavát az előfizetés.

3. Adja meg az előfizetéshez tartozó tárterület-fiókok listáját a következő parancsot:

        azure storage account list

4. Jelölje ki a tárterület-fiókot, amely tartalmazza a blob, amellyel dolgozni szeretne, majd használja a következő parancsot sikerült beolvasni a kulcsot ehhez a fiókhoz:

        azure storage account keys list <storage-account-name>

    Ez **az elsődleges** és **másodlagos** kulcsok vissza. Másolja az **elsődleges** kulcs értékét, mert fog szerepelni a következő lépésekkel.

5. A következő parancs segítségével a tárhely számla belüli blob-tárolók listáját:

        azure storage container list -a <storage-account-name> -k <primary-key>

6. Az alábbi parancsok segítségével a fájlok feltöltése és letöltése a blob szeretne:

    * Fájl feltöltése:

            azure storage blob upload -a <storage-account-name> -k <primary-key> <source-file> <container-name> <blob-name>

    * Fájlok letöltése:

            azure storage blob download -a <storage-account-name> -k <primary-key> <container-name> <blob-name> <destination-file>

> [AZURE.NOTE] Mindig dolgozni fog tároló ugyanazzal a fiókkal, ha az alábbi környezeti változók helyett adja meg a fiók beállítása, és a minden parancs kulcs:
>
> * **AZURE\_tároló\_fiók**: A tárterület-fiók neve
>
> * **AZURE\_tároló\_ACCESS\_kulcs**: A tárterület fiókkulcs

###<a id="powershell"></a>Azure PowerShell

Azure PowerShell a parancsfájlok futtatásának környezet ellenőrzése és a telepítési és Azure-ban a feladatok kezelésének automatizálásához használható. Azure PowerShell futtatásához munkaállomástól beállításával kapcsolatos további tudnivalókért lásd [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).

[AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

**Helyi fájl feltöltéséhez Azure Blob-tárolóhoz**

1. Nyissa meg az Azure PowerShell konzolt a utasításai [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md).
2. Az értékeket az első öt változót állítsa be a következőt:

        $resourceGroupName = "<AzureResourceGroupName>"
        $storageAccountName = "<StorageAccountName>"
        $containerName = "<ContainerName>"

        $fileName ="<LocalFileName>"
        $blobName = "<BlobName>"

        # Get the storage account key
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        # Create the storage context object
        $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

        # Copy the file from local workstation to the Blob container
        Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob $blobName -context $destContext

3. A parancsprogram beillesztése az Azure PowerShell konzol futtatásával másolja át a fájlt.

Példa a HDInsight, végezhető létrehozott PowerShell-parancsfájlokat [HDInsight eszközök](https://github.com/blackmist/hdinsight-tools)jelennek meg.

###<a id="azcopy"></a>AzCopy

AzCopy parancssori eszköz, amely az adatok átvitele be- és kijelentkezés a Azure tároló fiók egyszerűbbé lett tervezve. Különálló eszközként használni, vagy egy meglévő alkalmazásban ez az eszköz beépítése. [Töltse le a AzCopy][azure-azcopy-download].

AzCopy szintaxisa:

    AzCopy <Source> <Destination> [filePattern [filePattern...]] [Options]

További tudnivalókért lásd: [AzCopy - Azure BLOB-fájlok feltöltése és letöltése][azure-azcopy].


###<a id="commandline"></a>Hadoop parancssor

A Hadoop parancssori csak akkor hasznos, adatok tárolására blob-tárolóhoz be, amikor az adatok már szerepel a központi csomópont.

A Hadoop parancs használatához, először csatlakoznia kell a headnode, az alábbi módszerek egyikével:

* **Windows-alapú HDInsight**: [Csatlakozás távoli asztali változatában.](hdinsight-administer-use-management-portal.md#connect-to-hdinsight-clusters-by-using-rdp)

* **Linux-alapú HDInsight**: csatlakozhat SSH ([a SSH parancs](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster) vagy [gitt](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster))

Miután létrejött, a következő szintaxissal fájl feltöltése tárhelyet.

    hadoop -copyFromLocal <localFilePath> <storageFilePath>

Ha például`hadoop fs -copyFromLocal data.txt /example/data/data.txt`

Mivel az alapértelmezett fájlrendszer HDInsight az Azure Blob-tárolóban lévő, /example/data.txt valójában Azure Blob-tárolóban lévő. Akkor is érdemes lehet a fájlt:

    wasbs:///example/data/data.txt

vagy

    wasbs://<ContainerName>@<StorageAccountName>.blob.core.windows.net/example/data/davinci.txt

Más Hadoop parancsok használata a fájlok listáját olvassa el a [http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html](http://hadoop.apache.org/docs/r2.7.0/hadoop-project-dist/hadoop-common/FileSystemShell.html) című témakört.

> [AZURE.WARNING] Az alapértelmezett HBase fürt, a blokkolja a böngészik adatok írása 256KB méretét. Amíg ez nem gond HBase API-hoz vagy a REST API-k használata során, használja a `hadoop` vagy `hdfs dfs` parancsok írni az adatok ~ 12GB-nál nagyobb hibát eredményez. Című [írási blob-tároló kivétel](#storageexception) alatti további információt.

##<a name="graphical-clients"></a>A grafikus ügyfelek

Még több alkalmazások vannak, amely egy grafikus felületet biztosít az Azure tároló használata. Az alábbiakban néhány olyan ezeket az alkalmazásokat listája:

| Ügyfél | Linux | OS X | A Windows |
| ------ |:-----:|:----:|:-------:|
| [Microsoft Visual Studio eszközök hdinsight szolgáltatáshoz](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources) | ✔ | ✔ | ✔ |
| [Azure tároló Explorer](http://storageexplorer.com/) | ✔ | ✔ | ✔ |
| [Felhőalapú tárolási Studio 2](http://www.cerebrata.com/Products/CloudStorageStudio/) | | | ✔ |
| [CloudXplorer](http://clumsyleaf.com/products/cloudxplorer) | | | ✔ |
| [Azure Explorer](http://www.cloudberrylab.com/free-microsoft-azure-explorer.aspx) | | | ✔ |
| [Cyberduck](https://cyberduck.io/) |  | ✔ | ✔ |

###<a name="visual-studio-tools-for-hdinsight"></a>Visual Studio Tools for hdinsight szolgáltatáshoz

További tudnivalókért lásd: [Nyissa meg a csatolt erőforrásokat](hdinsight-hadoop-visual-studio-tools-get-started.md#navigate-the-linked-resources).

###<a id="storageexplorer"></a>Azure tároló Explorer

*Azure tároló Explorer* rendkívül hasznos eszköz, vizsgálata és BLOB adatainak módosítása. Ingyenes, nyissa meg a forrást, amely letölthető a [http://storageexplorer.com/](http://storageexplorer.com/)eszköz. Ezt a hivatkozást a forráskódot érhető el.

Az eszköz használata előtt ismernie kell a tároló Azure nevét és a fiók fiókkulcs. Ezek az információk beszerzése című témakör nyújt tájékoztatást a "hogyan: nézet, a másolás és a újragenerálása tároló hívóbillentyűk" [létrehozása, kezelése, és a tárhely fiók törlése]a szakasz[azure-create-storage-account].  

1. Azure tároló Explorerben futtatni. Ha most először van adódott a tárhely Explorer, a, a__tárhely fiók nevét__ , és a __tárhely fiókkulcs___ bekéri. Ha futtatta használat előtt adjon meg egy új nevet a tárolási fiók és billentyűt a __Hozzáadás__ gombra.

    Írja be a nevét, és a tárterület-fiókom billentyűt a HDinsight fürt által használt, és válassza a __MENTÉS, és nyissa meg__.

    ![HDI. AzureStorageExplorer][image-azure-storage-explorer]

5. Tárolók bal oldalán, a felület listájában kattintson a tároló a HDInsight fürt társított nevére. Alapértelmezés szerint ez a HDInsight fürt neve, de ha megadott egy egyedi nevet a fürt létrehozásakor eltérőek lehetnek.

6. Az eszköz sávon kattintson a Feltöltés ikonra.

    ![Feltöltés ikon ki van emelve eszköztárán](./media/hdinsight-upload-data/toolbar.png)

7. Adja meg a feltölteni kívánt fájlt, és kattintson a **Megnyitás**gombra. Amikor a rendszer kéri, válassza a __Töltse fel__ a tárhely tároló legfelső szintű töltse fel a fájlt. Ha szeretné feltölteni a fájlt egy adott helyre, a __címzett__ mezőben adja meg az elérési utat, és válassza a __Feltöltés__.

    ![Fájl feltöltése párbeszédpanel](./media/hdinsight-upload-data/fileupload.png)
    
    Amikor a fájl befejeződik feltöltése, a feladatok a HDInsight fürt használhatja.

##<a name="mount-azure-blob-storage-as-local-drive"></a>Helyi meghajtóra, csatlakoztatási Azure Blob-tárolóhoz

Lásd: a [helyi meghajtón, csatlakoztatási Azure Blob-tárolóhoz](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/09/mount-azure-blob-storage-as-local-drive.aspx).

##<a name="services"></a>Szolgáltatások

###<a name="azure-data-factory"></a>Azure Data Factory

Az Azure Data Factory szolgáltatás szerkesztési tárhely, adatfeldolgozás és adatok mozgását adatszolgáltatások egyesíti, méretezhető és megbízható adatok gyártási folyamatok be egy teljes körű felügyelt szolgáltatást.

Azure Data Factory használható adatok áthelyezése Azure Blob-tárolóhoz, vagy közvetlenül a HDInsight funkciókat, például a struktúra, és malac használni adatok folyamatok létrehozásához.

További tudnivalókért lásd: [Azure Data Factory dokumentációt](https://azure.microsoft.com/documentation/services/data-factory/).

###<a id="sqoop"></a>Apache Sqoop

Sqoop adatok Hadoop és a relációs adatbázisok közötti átviteléhez tervezett eszköz. Az SQL Server, MySQL vagy Oracle be a Hadoop elosztott fájlrendszer () Fájlrendszerhez, az adatokat a Hadoop MapReduce vagy struktúra, és majd exportálja az adatokat egy RDBMS az importálandó adatokat egy relációs adatbázis-kezelő rendszer (RDBMS), felhasználhatja azt.

További tudnivalókért lásd: [A HDInsight használata Sqoop][hdinsight-use-sqoop].

##<a name="development-sdks"></a>Fejlesztési SDK

Azure Blob-tárolóhoz az Azure SDK át a következő programnyelv használatával is elérhető:

* .NET
* Java
* NODE.js
* PHP
* Python
* Fonetikus

Az Azure SDK telepítésével kapcsolatos további tudnivalókért lásd: a [Azure-letöltések](https://azure.microsoft.com/downloads/)

## <a name="troubleshooting"></a>Hibaelhárítás

### <a id="storageexception"></a>Írás a blob-tároló kivétel

__A jelenség__: használata esetén a `hadoop` vagy `hdfs dfs` írni a fájlokat, amelyek ~ 12 GB parancsok vagy nagyobb egy HBase fürt előfordulhatnak a következő hiba: 

    ERROR azure.NativeAzureFileSystem: Encountered Storage Exception for write on Blob : example/test_large_file.bin._COPYING_ Exception details: null Error Code : RequestBodyTooLarge
    copyFromLocal: java.io.IOException
            at com.microsoft.azure.storage.core.Utility.initIOException(Utility.java:661)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:366)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:350)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:471)
            at java.util.concurrent.FutureTask.run(FutureTask.java:262)
            at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
            at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
            at java.lang.Thread.run(Thread.java:745)
    Caused by: com.microsoft.azure.storage.StorageException: The request body is too large and exceeds the maximum permissible limit.
            at com.microsoft.azure.storage.StorageException.translateException(StorageException.java:89)
            at com.microsoft.azure.storage.core.StorageRequest.materializeException(StorageRequest.java:307)
            at com.microsoft.azure.storage.core.ExecutionEngine.executeWithRetry(ExecutionEngine.java:182)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlockInternal(CloudBlockBlob.java:816)
            at com.microsoft.azure.storage.blob.CloudBlockBlob.uploadBlock(CloudBlockBlob.java:788)
            at com.microsoft.azure.storage.blob.BlobOutputStream$1.call(BlobOutputStream.java:354)
            ... 7 more

__OK__: a HDInsight HBase alapértelmezett 256 KB blokk mérete fürtök Azure tárolóhoz írásakor. HBase API-hoz vagy a REST API-khoz működik, míg eredményez hiba használata esetén a `hadoop` vagy `hdfs dfs` parancssori.

__Megoldás__: használja `fs.azure.write.request.size` nagyobb méretű blokkokból álló megadásához. Ezt megteheti használati alapon használatával a `-D` paraméter. Az alábbi képen a paraméter használata a `hadoop` parancsot:

    hadoop -fs -D fs.azure.write.request.size=4194304 -copyFromLocal test_large_file.bin /example/data

Megnövelheti a értéke is `fs.azure.write.request.size` globálisan Ambari használatával. Az alábbi lépésekkel módosítsa az értéket a felhasználói felület Ambari webes használható:

1. A böngészőben nyissa meg a fürt Ambari webes felületének. Ez a https://CLUSTERNAME.azurehdinsight.net, ahol __CLUSTERNAME__ a csoport nevére.

    Amikor a rendszer kéri, adja meg a rendszergazda nevét és jelszavát a fürt.

2. A képernyő bal oldalán jelölje be a __Fájlrendszerhez__, és válassza az __konfigurációk__ lehetőséget.

3. __Szűrés...__ mezőjébe írja be `fs.azure.write.request.size`. Ekkor megjelenik a mezőre, és az aktuális értékét a lap közepén.

4. Módosítsa a értékét 262144 (256KB) az új értékre. Ha például 4194304 (4MB).

![Az érték keresztül Ambari webes felületének módosítása képe](./media/hdinsight-upload-data/hbase-change-block-write-size.png)

Ambari használatával kapcsolatos további tudnivalókért olvassa el a [HDInsight kezelése fürt Ambari webes felületének használata](hdinsight-hadoop-manage-ambari.md)című témakört.

## <a name="next-steps"></a>Következő lépések

Most, hogy megismeri beszerzése az adatok HDInsight-be, olvassa el az alábbi cikkekben olvashat elemzést végezhet:

* [Első lépések az Azure hdinsight szolgáltatáshoz][hdinsight-get-started]
* [Hadoop feladatok programozás útján elküldése][hdinsight-submit-jobs]
* [HDInsight struktúra használata][hdinsight-use-hive]
* [Malac használata hdinsight szolgáltatáshoz][hdinsight-use-pig]




[azure-management-portal]: https://porta.azure.com
[azure-powershell]: http://msdn.microsoft.com/library/windowsazure/jj152841.aspx

[azure-storage-client-library]: /develop/net/how-to-guides/blob-storage/
[azure-create-storage-account]: ../storage/storage-create-storage-account.md
[azure-azcopy-download]: ../storage/storage-use-azcopy.md
[azure-azcopy]: ../storage/storage-use-azcopy.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md

[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-provision]: hdinsight-provision-clusters.md

[sqldatabase-create-configure]: ../sql-database-create-configure.md

[apache-sqoop-guide]: http://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

[Powershell-install-configure]: ../powershell-install-configure.md

[azurecli]: ../xplat-cli-install.md


[image-azure-storage-explorer]: ./media/hdinsight-upload-data/HDI.AzureStorageExplorer.png
[image-ase-addaccount]: ./media/hdinsight-upload-data/HDI.ASEAddAccount.png
[image-ase-blob]: ./media/hdinsight-upload-data/HDI.ASEBlob.png
