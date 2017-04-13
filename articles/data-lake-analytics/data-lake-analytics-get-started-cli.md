<properties 
   pageTitle="Első lépések az Azure adatok tó Analytics Azure parancssori felületéről |} Microsoft Azure" 
   description="Azure parancssori felületének használata tó adattár-fiók létrehozása, U – SQL használatával adatok tó Analytics feladat létrehozásához, és a feladat elküldése. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a>Oktatóprogram: Azure adatok tó Analytics Azure parancssori felület CLI () használata – első lépések

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Megtudhatja, hogy miként Azure CLI segítségével Azure adatok tó Analytics-fiókokat létrehozni, adatok tó Analytics feladatok meghatározása a [U-SQL nyelvben](data-lake-analytics-u-sql-get-started.md)és elküldése az adatok tó Analytics-fiókokat feladatokat. További információt a adatok tó Analytics [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.

Ebben az oktatóanyagban a feladat, amely beolvassa a lap elválasztott értékek (TSV) fájlt, és konvertálja a vesszővel elválasztott értékek (CSV) fájl fejleszthet olyan. Az azonos oktatóprogram más támogatott eszközökkel folyamatát, kattintson a lap tetején lévő ebben a szakaszban.

##<a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban megkezdése előtt a következőket kell rendelkeznie:

- **Az Azure-előfizetés**. Lásd: [Ismerkedés az Azure ingyenes próbaverziót](https://azure.microsoft.com/pricing/free-trial/).
- **Azure CLI**. Lásd: [Telepítse és állítsa be az Azure CLI](../xplat-cli-install.md).
    - Letöltheti és telepítheti az **előzetes verziójú** [Azure CLI eszközök](https://github.com/MicrosoftBigData/AzureDataLake/releases) annak érdekében, hogy ez a bemutató befejezéséhez.
- **Hitelesítés**, a következő parancsot:

        azure login
    Hitelesítés, munkahelyi vagy iskolai fiókkal kapcsolatos további tudnivalókért olvassa el a [Csatlakozás az Azure CLI az Azure-előfizetésbe](../xplat-cli-connect.md).
- **Az erőforrás-kezelő Azure módban váltás**a következő parancsot:

        azure config mode arm
        
## <a name="create-data-lake-analytics-account"></a>Adatok tó Analytics-fiók létrehozása

Adatok tó Analytics-fiókkal kell rendelkeznie, bármilyen feladat futtatása előtt. Adatok tó Analytics-fiók létrehozása, adja meg a következőket:

- **Azure erőforráscsoport**: Azure erőforráscsoport belül A adatok tó Analytics-fiókot kell létrehozni. [Erőforrás-kezelő Azure](../azure-resource-manager/resource-group-overview.md) lehetővé teszi, hogy az erőforrások az alkalmazás csoportosan kezelheti. Telepítése, frissítése, és egyetlen, összehangolt műveletben az alkalmazás törlése az összes erőforrás.  

    Az erőforrások számbavétele csoportosítja az előfizetésben:
    
        azure group list 
    
    Új erőforráscsoport létrehozása:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Adatok tó Analytics fióknév**
- **Hely**: az Azure adatközpontokban, amely támogatja az adatok tó Analytics közül.
- **Alapértelmezett adatok tó fiók**: minden adat tó Analytics-fiók alapértelmezett adatok tó fiókkal rendelkezik.

    A meglévő adatok tó fiók listához:
    
        azure datalake store account list

    Új adatok tó fiók létrehozása:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Az adatok tó fióknév csak kell tartalmaznia a kisbetűket és számokat.



**Adatok tó Analytics-fiók létrehozása**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"

        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Adatok tó Analytics fiók megjelenítése](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)

> [AZURE.NOTE] Az adatok tó Analytics fióknév csak kell tartalmaznia a kisbetűket és számokat.


## <a name="upload-data-to-data-lake-store"></a>Adatok feltöltése tó adattárhoz

Ebben az oktatóanyagban a bizonyos keresési naplók dolgozza.  A keresési napló adatok tó tár vagy a Azure Blob-tárolóhoz tárolható. 

Az Azure-portálon felhasználói felületet biztosít az adatok tó keresési naplófájlt tartalmazza az alapértelmezett fiók néhány mintaadatfájlokat másolhat. Az [Előkészítés forrásadatok](data-lake-analytics-get-started-portal.md#prepare-source-data) feltölteni az adatokat az alapértelmezett adattár tó fiók olvashat.

Töltse fel a fájlokat az asztali cli, használja a következő parancsot:

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Adatok tó Analytics Azure Blob-tárolóhoz is elérheti.  Feltöltéshez Azure Blob-tárolóhoz, olvassa el a [az Azure CLI Azure adathordozós használatával](../storage/storage-azure-cli.md)című témakört.

## <a name="submit-data-lake-analytics-jobs"></a>Adatok tó Analytics feladatok elküldése

Az adatok tó Analytics feladatok U-SQL nyelvben nyelvű kerülnek. Többet szeretne tudni az SQL-U, [U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md) és [U-SQL nyelv reference](http://go.microsoft.com/fwlink/?LinkId=691348)témakörben olvashat.

**Adatok tó Analytics feladat parancsfájl létrehozása**

- Hozzon létre a következő U-SQL nyelvben forgatókönyvvel egy szövegfájlt, és mentse a szövegfájlt munkaállomástól:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    A U-SQL nyelvben parancsfájl beolvassa az adatokat forrásfájl **Extractors.Tsv()**használatával, és ekkor létrehoz egy csv-fájl használatával **Outputters.Csv()**. 
    
    Ne módosítsa a két elérési útját, kivéve, ha a forrásfájl egy másik helyre másolja.  Adatok tó Analytics a kimeneti mappát hoz létre, ha még nem létezik.
    
    Sokkal egyszerűbb a fájlok alapértelmezett tárolt adatok tó fiókok relatív elérési utak használatával. Abszolút elérési út is használhatja.  Ha például 
    
        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
        
    Abszolút elérési út csatolt tároló fiókok fájlok eléréséhez kell használnia.  A csatolt Azure tárterület-fiókjában tárolt fájlok szintaxisa a következő:
    
        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

    >[AZURE.NOTE] Nyilvános BLOB vagy nyilvános tárolók hozzáférési engedélyek Azure Blob-tárolóban jelenleg nem támogatott.      

    
**A feladat elküldése**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"
    
    
A következő parancsokat a feladatok lista, részletek a feladat és feladat megszakítása használható:

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

A feladat befejezése után az alábbi parancsmag használatával a fájlt, és töltse le a fájlt:
    
    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## <a name="see-also"></a>Lásd még:

- Az egyéb eszközök segítségével azonos oktatóprogram megtekintéséhez kattintson a lap választók lap tetején.
- Összetett lekérdezés talál további [elemzés webhely naplók Azure adatok tó Analytics használatával](data-lake-analytics-analyze-weblogs.md).
- Első lépésként U – SQL-alkalmazások fejlesztésével lásd: [adatok tó Tools for Visual Studio segítségével kidolgozása U – SQL-parancsfájlok](data-lake-analytics-data-lake-tools-get-started.md).
- Az SQL-U című témakörben talál [Azure adatok tó Analytics U – SQL nyelv használatába](data-lake-analytics-u-sql-get-started.md).
- Teendőkről olvassa el a [Kezelése Azure adatok tó Analytics Azure portál használatával](data-lake-analytics-manage-use-portal.md)című témakört.
- Adatok tó Analytics áttekinti a [Azure adatok tó Analytics áttekintése](data-lake-analytics-overview.md)című témakörben találhat.

