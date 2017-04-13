<properties
    pageTitle="Adatok gyári oktatóprogram: első adatok folyamat |} Microsoft Azure"
    description="Azure Data Factory oktatóprogram megtudhatja, hogyan hozhat létre és struktúra parancsfájl használatával a Hadoop fürthöz adatokat feldolgozó adatok gyár ütemezése."
    services="data-factory"
    keywords="Azure adatok gyári oktatóanyag, hadoop fürtre, hadoop-struktúra"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="spelluru"/>

# <a name="tutorial-build-your-first-pipeline-to-process-data-using-hadoop-cluster"></a>Oktatóprogram: Az első folyamat folyamat adatokhoz Hadoop fürt összeállítása 
> [AZURE.SELECTOR]
- [Áttekintés és a vonatkozó követelmények](data-factory-build-your-first-pipeline.md)
- [Azure portál](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [A PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Erőforrás-kezelő sablon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-VAL](data-factory-build-your-first-pipeline-using-rest-api.md)

Ebben az oktatóprogramban az első Azure adatok gyári az adatokat egy Azure hdinsight szolgáltatáshoz (Hadoop) fürt struktúra parancsfájl futtatásával feldolgozó egy adatok folyamat hoz létre. 

> [AZURE.NOTE] Ez a cikk nem nyújt Azure Data Factory elrendezését. A szolgáltatás egy elvi áttekintése című témakörben [Azure Data Factory](data-factory-introduction.md). Olvassa el a [Data Factory tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/data-factory/) egy ajánlott módszereket, amelyekkel navigálhat a saját tartalom a kapcsolatos adatok gyári.

## <a name="whats-covered-in-this-tutorial"></a>Mi ebből az oktatóanyagból alá tartozó? 
**Azure Data Factory** írása adatok **mozgását** és adatok **feldolgozási** tevékenység szerint (más néven adatok folyamatok) adatalapú munkafolyamatok teszi lehetővé. Megtudhatja, hogyan hozhat létre az adatok adatfeldolgozás (vagy más átalakítási) folyamat első tevékenységhez. Ez a tevékenység átalakítása és elemzése a minta webes naplók HDInsight Hadoop fürt használja.  

Ebben az oktatóanyagban hajtsa végre az alábbi lépéseket:

1.  Hozzon létre egy **adatok gyári**. Adatok gyár egyezik-e egy vagy több adat csővezetékek áthelyezése és a folyamat adatokat. 
2.  Hozzon létre **csatolt szolgáltatást**. Létrehozhat egy csatolt egy adattár, amelyre a hivatkozás vagy a számítási szolgáltatást az adatok gyári. Egy adattár, például az Azure tárolás tartalmazza a tevékenységek bemeneti és kimeneti adatai a során. Számítási fürt HDInsight Hadoop például szolgáltatás folyamatok/átalakítók adatokat.    
3.  Beviteli létrehozása, és a kimeneti **adatkészleteket**. Egy beviteli adatkészlet egy tevékenység, a során a bemeneti pedig egy kimenet adatkészlet a kimenet a tevékenység.
3.  A **folyamat**létrehozása. Egy folyamat beállíthatja, hogy egy vagy több tevékenységeket (Példa: másolás tevékenység, a HDInsight-struktúra tevékenység). Ez a példa a HDInsight-struktúra tevékenység, amelyet a struktúra parancsfájl futtat a HDInsight Hadoop fürt használja. A parancsfájl először létrehoz egy olyan táblát, a nyers webes naplóadatokat Azure blob-tárolóban lévő hivatkozások, és ezután partíciók a nyers adatokból év és hónap szerint.

    Azure Data Factory által támogatott tevékenységek két típusa van. Azok: [adatok mozgását tevékenységeket](data-factory-data-movement-activities.md) és [adatok átalakítása tevékenységeket](data-factory-data-transformation-activities.md). A Másolás tevékenység csak egy adatok mozgását tevékenység nem. Ebben az oktatóanyagban, ne használja a Másolás tevékenységet. A Másolás tevékenység használatáról oktatóanyagban című [oktatóanyag: adatok másolása az egy Azure SQL Azure blob-](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Ebben az oktatóanyagban használhatja HDInsight-struktúra tevékenység az egyik Data Factory által támogatott adatok átalakítása tevékenységek.  
 
Az alábbiakban a **diagram nézetben** a minta data factory generál ebben az oktatóanyagban. 

![Diagram nézetben a Data Factory oktatóprogram](./media/data-factory-build-your-first-pipeline/data-factory-tutorial-diagram-view.png)

Ebben az oktatóanyagban a **adfgetstarted** Azure blob-tároló mappájában **inputdata** input.log nevű egy fájlt tartalmazza. A naplófájl három hónappal a bejegyzéseket tartalmaz: január, február és március a 2016-ban. Az alábbiakban a minta sorokat az egyes a bemeneti fájlban. 

    2016-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871 
    2016-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
    2016-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871

Ha a fájlt a folyamat HDInsight-struktúra tevékenységeket használó által feldolgozott, a tevékenység egy struktúrát parancsfájlt futtatja a HDInsight fürt, hogy a partíciók adatok beviteléhez év és hónap szerint. A parancsfájl hoz létre három kimeneti mappát tartalmazó fájl az egyes a bejegyzéseket.  

    adfgetstarted/partitioneddata/year=2016/month=1/000000_0
    adfgetstarted/partitioneddata/year=2016/month=2/000000_0
    adfgetstarted/partitioneddata/year=2016/month=3/000000_0

A fenti minta sorokból, az első, amelyik (2016-01-01) a hónap a 000000_0 fájlt írott = 1 mappát. Hasonlóképpen, a másodikat pedig íródott a fájlt a hónap = 2 mappát, és a harmadik egyik írott a fájlt a hónap = 3 mappát.  


## <a name="pre-requisites"></a>Előzetes követelmények
Mielőtt elkezdené ebben az oktatóanyagban, az alábbi előfeltételek kell rendelkeznie:

1.  **Azure előfizetés** – Ha nincs telepítve az Azure előfizetéssel, létrehozhat egy ingyenes próba-fiókkal mindössze néhány perc. [Ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/) témakörben meg, hogy miként szerezhet be egy ingyenes próba-fiókkal.

2.  **Azure tároló** – az adatok tárolásának ebben az oktatóanyagban Azure tároló fiókot használ. Ha nincs Azure tárterület-fiókkal, ismertető [tároló fiók létrehozása](../storage/storage-create-storage-account.md#create-a-storage-account) . Miután létrehozta a tárterület-fiókot, vegye figyelembe a **fiók nevét** , és az **access billentyűt**lenyomva. Lásd: a [nézet, a másolás és a hívóbetűk újragenerálása tárhelyet](../storage/storage-create-storage-account.md#view-and-copy-storage-access-keys). 

### <a name="upload-files-to-azure-storage-for-the-tutorial"></a>Fájlok feltöltése a az oktatóprogram az Azure-tárolóhoz
Az oktatóprogram elindítása, előtt kell Azure tárterület-fiókját az mintafájlok előkészítése az oktatóprogram.

1. Töltse fel a **adfgetstarted** blob-tároló mappájában **parancsfájl** struktúra Adatlekérdezési fájl (HQL).
2. Beviteli fájl feltöltése a **adfgetstarted** blob-tárolóhoz **inputdata** mappájába. 

#### <a name="create-hql-script-file"></a>Hozzon létre HQL parancsprogram 

1. Indítsa el a **Jegyzettömböt** , és illessze be a következőt HQL. A struktúra parancsfájl hoz létre a két tábla: **WebLogsRaw** és **WebLogsPartitioned**. Kattintson a **fájl** menü, és válassza a **Mentés másként**parancsot. Váltson a **C:\adfgetstarted** mappát a merevlemezen. Válassza a * *az összes fájl (*.*) **esetében a** Mentés másként** Írja be a mezőben. Adja meg **partitionweblogs.hql** esetében a ****fájlnevet. Győződjön meg arról, hogy a **kódolás,** kattintson a párbeszédpanel alján mező értéke **ANSI**. Ha nem, állítsa be **ANSI **.  

        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '${hiveconf:inputtable}' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw

Futásidőben, a Data Factory során a struktúra tevékenység adja át értékeket a **inputtable** és **partitionedtable** paramétereket a következő kódtöredékének látható módon:  

        "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
        "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"

A **storageaccountname** az Azure tárterület-fiókja nevét.
 
#### <a name="create-a-sample-input-file"></a>Beviteli mintafájl létrehozása
Jegyzettömb használatával hozhat létre egy **input.log** az alábbi tartalmán a **c:\adfgetstarted** nevű fájlt: 

    #Software: Microsoft Internet Information Services 8.0
    #Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step5.png X-ARR-LOG-ID=9b0c14b1-434d-495a-9b0d-46775194257b 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 37931 871 32
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step6.png X-ARR-LOG-ID=f99cff81-ec38-4a13-b2fe-21b10adca996 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 27146 871 47
    2016-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step1.png X-ARR-LOG-ID=af94d3b4-8e05-4871-82c4-638f866d4e83 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 66259 871 140
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-02-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47
    2016-03-01 02:01:10 SAMPLEWEBSITE GET /blogposts/mvc4/step7.png X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 30184 871 47

#### <a name="upload-input-file-and-hql-file-to-your-azure-blob-storage"></a>Beviteli és HQL fájl feltöltése a Azure Blob-tárolóhoz

Ez a szakasz útmutatás input.log és partitionweblogs.hql fájlok másolása Azure Blob-tárolóhoz **AzCopy** eszközzel. A választási lehetőségek, bármilyen eszközzel (például: [Microsoft Azure tároló Explorer](http://storageexplorer.com/), a feladat végrehajtásához [CloudXPlorer ClumsyLeaf szoftverrel](http://clumsyleaf.com/products/cloudxplorer) .   
     
1. Töltse le a [legújabb verzióját **AzCopy**](http://aka.ms/downloadazcopy)vagy a [legújabb verziót](http://aka.ms/downloadazcopypr). Lásd: [AzCopy használatáról](../storage/storage-use-azcopy.md) a cikk a segédprogrammal útmutatást.
2. Keresse meg a c:\adfgetstarted mappát, és futtassa az alábbi parancsot: 

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\AzCopy" /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/inputdata /DestKey:<storageaccesskey>  /Pattern:input.log

    Ez a parancs a **input.log** fájl feltölti a tárterület-fiókot (**adfgetstarted** tároló és **inputdata** mappa). Cserélje a **storageaccountname** a tárterület-fiókjához, és **storageaccesskey** a nevet a tárolási hívóbetű.

    > [AZURE.NOTE] Ez a parancs **adfgetstarted** az Azure Blob-tárolóban lévő nevű tároló hoz létre, és másolja a **input.log** fájlt a helyi meghajtón a **inputdata** a tároló mappába. 
    
3. Miután sikeresen feltöltötte a fájlt, a kimenet AzCopy az alábbihoz hasonló jelenik meg.
    
        Finished 1 of total 1 file(s).
        [2015/12/16 23:07:33] Transfer summary:
        -----------------
        Total files transferred: 1
        Transfer successfully:   1
        Transfer skipped:        0
        Transfer failed:         0
        Elapsed time:            00.00:00:01
5. A következő parancsot a **partitionweblogs.hql** fájl feltöltése a **adfgetstarted** tároló **parancsfájl** mappájába. Az alábbiakban a parancsot: 
    
        AzCopy /Source:. /Dest:https://<storageaccountname>.blob.core.windows.net/adfgetstarted/script /DestKey:<storageaccesskey>  /Pattern:partitionweblogs.hql

A Előfeltételek befejezése. Adatok gyár használja az alábbi módokon hozhat létre. Kattintson a lapok vagy tetején az oktatóprogram elvégzéséhez az alábbi hivatkozások egyikére. 

- [Azure portál](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [A PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Erőforrás-kezelő sablon](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API-VAL](data-factory-build-your-first-pipeline-using-rest-api.md)