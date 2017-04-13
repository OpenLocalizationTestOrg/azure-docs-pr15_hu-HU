<properties 
    pageTitle="Adatok – az adatkezelési átjáró áthelyezése |} Microsoft Azure"
    description="Adatok áthelyezése fiókok között a helyszíni és felhőbeli adatok az átjáró beállítása. Azure Data Factory az adatkezelési átjáró segítségével helyezze át az adatokat." 
    keywords="adatok átjárót, adatok integrálása, helyezze át az adatokat, átjáró hitelesítő adatok"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Adatok áthelyezése fiókok között a helyszíni adatforrások és az adatkezelési átjáró a felhőben
Ez a cikk áttekintést nyújt az adatok integrációja helyszíni adatok tárolók és a felhő adatokat tárolja Data Factory használatával. [Mozgás a tevékenységekre vonatkozó adatok](data-factory-data-movement-activities.md) olvashatók és egyéb adatok gyári core fogalmak épít: [adatkészleteket](data-factory-create-datasets.md) , és [folyamatok](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Az adatkezelési átjáró
Ahhoz, hogy/áruházból a helyszíni adatok áthelyezése adatok a helyszíni számítógépen telepíteni kell az adatkezelési átjáró. Az átjáró is telepítve van az adatokat tároló ugyanazon a gépen vagy egy másik számítógépen mindaddig, amíg az átjáró csatlakozhat az adatokat tároló. 

> [AZURE.IMPORTANT] Lásd: [Az adatkezelési átjáró](data-factory-data-management-gateway.md) a cikk az adatkezelési átjáró kapcsolatban további tájékoztatást.   

A következő forgatókönyv bemutatja, hogyan adatok gyár létrehozását a folyamat, amely az adatokat egy helyszíni **SQL Server** -adatbázis Azure blob-tárolóhoz helyezi át. Az útmutató részeként telepítse, és állítsa be az adatkezelési átjáró a számítógépre. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Útmutató: a felhő a helyszíni adatok másolása
  
## <a name="create-data-factory"></a>Adatok gyári létrehozása
Ebben a lépésben, az Azure portal segítségével létrehozhat egy **ADFTutorialOnPremDF**nevű Azure Data Factory-példányt. 

1.  Jelentkezzen be az [Azure-portálon](https://portal.azure.com). 
2.  Kattintson a **+ Új**kattintson a **üzletiintelligencia- + analytics**, és kattintson az **Adatok gyári**.

    ![Új-DataFactory >](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. Írja be az **új adatok gyári** lap **ADFTutorialOnPremDF** nevét.

    ![Startboard hozzáadása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Az Azure adatok gyári neve globálisan egyedinek kell lennie. Ha a hibakód: **nem érhető el adatok gyári neve "ADFTutorialOnPremDF"**, módosíthatja az adatok gyári (például yournameADFTutorialOnPremDF) nevét, és próbáljon meg ismét létrehozni. Ez a név helyett ADFTutorialOnPremDF használata közben az oktatóprogram elvégzéséhez a hátralévő lépéseket.
    > 
    > Az adatok gyári neve bejegyezhető **DNS** nevével ennélfogva és a jövőben nyilvánosan láthatóvá válnak.
3. Jelölje ki az **Azure előfizetés** , amelyre a data factory létrehozni. 
4.  Jelölje be a meglévő **erőforráscsoport** , vagy hozzon létre egy erőforrás csoportot. Az oktatóprogram, hozzon létre egy névvel ellátott erőforráscsoport: **ADFTutorialResourceGroup**. 
5.  Kattintson a **Létrehozás** gombra az **új adatok gyári** lap.

    > [AZURE.IMPORTANT] Adatok Factory-példányok létrehozásához az [Adatok gyári munkatársi](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) szerepkörök, az előfizetés/erőforrás csoport szintjén tagjának kell lennie. 
11. Létrehozása után jelenik meg az **Adatok gyári** lap az alábbi képen látható módon:

    ![Adatok Factory kezdőlapján](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Átjáró létrehozása
5. Kattintson az **Adatok gyári** lap **Szerző és üzembe** csempe az adatok gyári **szerkesztő** indítása.

    ![Tartalom-előállítás és üzembe csempe](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  A adatok Factory-szerkesztőben kattintson a **... További** az eszköztáron, és kattintson az **új adatok átjáró**. Azt is megteheti a fanézetben kattintson a jobb gombbal az **Adatok átjárók** , és kattintson az **új adatok átjáró**gombra. 

    ![Új adatok átjáró eszköztár](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. A **Létrehozás** lap **adftutorialgateway** adja meg a **nevét**, és kattintson az **OK gombra**.    

    ![Átjárók lap létrehozása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. Kattintson a **beállítás** a lap, **közvetlenül a számítógépre telepíthető**. Ez a művelet az átjáró a telepítőcsomag letöltését, és a telepítést, konfigurálása és regisztrálja az átjárót a számítógépen.  

    > [AZURE.NOTE] 
    > Használja az Internet Explorer vagy egy Microsoft ClickOnce kompatibilis webböngészőben.
    > 
    > Ha a Chrome használata esetén nyissa meg a [Chrome webes tároló](https://chrome.google.com/webstore/)keressen a "ClickOnce" kulcsszó, válasszon egyet a ClickOnce extensions és telepítheti. 
    >  
    > Tegye ugyanezt a Firefox (telepítés bővítmény). Kattintson a **Menü megnyitása** gombra az eszköztáron (**három vízszintes vonalak** jobb felső sarokban lévő), kattintson a **bővítmények**, "ClickOnce" kulcsszóval keresése, válasszon egyet a ClickOnce extensions és telepítheti.    

    ![Az átjáró - lap konfigurálása](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    Ezzel a módszerrel legkönnyebben (egyetlen kattintással) letöltése, telepítése, beállítása és regisztrálja az átjárót egy lépésben. Megjelenik a **Microsoft adatkezelési átjáró konfigurációkezelőjének** alkalmazás telepítve van a számítógépen. Megtalálhatja a végrehajtható **ConfigManager.exe** a mappába: **C:\Program Files\Microsoft adatok kezelése Gateway\2.0\Shared**.

    Is töltse le és telepítse az átjáró manuálisan a hivatkozásokra a lap és az **Új BILLENTYŰT** szövegmezőben látható kulccsal regisztrálja.
    
    Lásd: [Az adatkezelési átjáró](data-factory-data-management-gateway.md) a cikk az átjáró olvashat.

    >[AZURE.NOTE] Telepítse és állítsa be az adatkezelési átjáró a sikeres kattintva a helyi számítógépen rendszergazdának kell lennie. A többi felhasználó vehet az **Adatok adatkezelési átjáró felhasználók** helyi Windows csoporthoz. A csoport tagjai az adatkezelési átjáró konfigurációkezelőjének eszköz használatával az átjáró beállítása. 

5. Várjon néhány percet, és várja meg, a következő értesítő üzenet jelenhet meg:

    ![A sikeres átjáró telepítése](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Indítsa el a számítógépen **Az adatkezelési átjáró konfigurációkezelőjének** alkalmazást. A **Keresés** ablakban írja be az **Adatkezelési átjáró** a segédprogram eléréséhez. Megtalálhatja a végrehajtható **ConfigManager.exe** a mappába: **C:\Program Files\Microsoft adatok kezelése Gateway\2.0\Shared** 

    ![Átjáró Konfigurációkezelőjével](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Győződjön meg arról, hogy látható `adftutorialgateway is connected to the cloud service` üzenetet. Az alul az állapotsor **csatlakoztatva a felhőalapú szolgáltatás** egy **zöld pipa**együtt jeleníti meg.

    A **Kezdőlap** lap is az alábbi műveleteket végezheti el: 
    - **Regisztráljon** az átjáró használatával az Azure portálról külső.FÜGGV gomb használatával. 
    - **Állítsa le** a adatok adatkezelési átjáró Host szolgáltatás az átjárót futtató számítógépen futó. 
    - **Frissítések ütemezése** telepítve legyen a nap egy adott időpontban. 
    - Ha az átjáró lett **utoljára frissítve**megtekintése.
    - Adja meg, hogy az átjáró frissítése telepíthető időt. 

8. Kattintson a **Beállítások** fülre. A tanúsítvány a **tanúsítvány** szakaszban megadott hitelesítő adatokat a helyszíni adatok tárolóhoz a portálon megadott titkosítás/visszafejtés szolgál. (nem kötelező) Kattintson a **Módosítás** saját tanúsítványt használja helyette. Alapértelmezés szerint az átjáró a tanúsítvány, amely automatikusan létrehozott Data Factory szolgáltatás használja.

    ![Átjáró Tanúsítványok beállítása](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Kattintson a **Beállítások** lapon az alábbi műveleteket is elvégezheti: 
    - Tekintse meg vagy exportálja az átjáró által használatban lévő tanúsítvány.
    - Módosítsa a HTTPS-végpont az átjáró által használt.    
    - Állítsa a HTTP-proxy az átjáró való használatra.   
9. (nem kötelező) Kattintson a **Diagnosztika** fülre, jelölje be a **részletes naplózás engedélyezése** jelölőnégyzetet, ha azt szeretné, hogy az átjáró a problémák megoldásához használható részletes naplózás engedélyezése. A naplóadatokat megtalálhatók az **Eseménynapló nevet** , az **alkalmazások és szolgáltatásnaplók** -> **Az adatkezelési átjáró** csomópontot. 

    ![Diagnosztikai lap](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    A **diagnosztikai** lapon is a következő műveleteket végezheti el: 
    
    - **A kapcsolat tesztelése** szakaszokat használhat az átjáró a helyszíni adatforrás.
    - Kattintson a **Naplók megtekintése** az adatkezelési átjáró naplójában az Eseménynapló ablakban parancsra. 
    - Kattintson a **Naplók küldése** Microsoft megkönnyítése érdekében, a problémák elhárítása az utóbbi hét napban a naplók zip fájl feltöltése parancsra. 
10. **Diagnosztika** lapján a **Kapcsolat tesztelése** csoportban jelölje be az **SQL Server** az adatokat tároló típusú, adja meg az adatbázis neve adatbázis-kiszolgáló nevét, adja meg a hitelesítés típusa, írja be a felhasználónevét és jelszavát, és kattintson a **vizsgálat** és ellenőrizze, hogy az átjáró csatlakozhat az adatbázis. 
11. Váltás a böngésző és az **Azure portál**, kattintson az **OK gombra** a **Configure** lap a, majd kattintson az **új adatok átjáró** lap.
6. Meg kell jelennie **adftutorialgateway** **Adatok átjárók** csoportban a bal oldali fastruktúrájú nézetben.  Ha azt választja, meg kell jelennie a társított JSON. 
    

## <a name="create-linked-services"></a>Hozzon létre csatolt szolgáltatások 
Ebben a lépésben hoz létre két csatolt szolgáltatások: **AzureStorageLinkedService** és **SqlServerLinkedService**. A **SqlServerLinkedService** csatolja az adatok gyári egy helyszíni SQL Server-adatbázis és a **AzureStorageLinkedService** csatolt szolgáltatás hivatkozások az Azure blob-tárolni. Ez a forgatókönyv a helyszíni SQL Server-adatbázisból származó adatokat másolja az Azure blob-tároló belül egy folyamat hoz létre. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Csatolt szolgáltatás hozzáadása egy helyszíni SQL Server-adatbázishoz
1.  **Adatok gyári szerkesztő** **új adatok tárolására** kattintson az eszköztáron, és válassza az **SQL Server**. 

    ![Új csatolt SQL Server szolgáltatást](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  A **JSON-szerkesztő** a jobb oldalon hajtsa végre az alábbi lépéseket: 
    1. Adja meg a **átjárónév** **adftutorialgateway**. 
    2. **ConnectionString**tegye a következőket: 
        1. **Kiszolgálónév**adja meg az SQL Server-adatbázist tároló kiszolgáló nevét.
        2. **Adatbázisnév**adja meg az adatbázis nevét.
        3. **Titkosítás** gombra az eszköztáron. Ez a letöltődött, és elindítja a hitelesítő adatok Manager alkalmazást.
        
            ![Hitelesítő adatok Manager alkalmazás](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. **A beállítás hitelesítő adatok** párbeszédpanelen adja meg a hitelesítés típusa, felhasználónevét és jelszavát, és kattintson az **OK gombra**. Ha a kapcsolat sikeres, a titkosított hitelesítő adatokat tárolja a JSON, és a párbeszédpanel bezárása. 
        6. Zárja be a böngésző üres lapot, amely a párbeszédpanel indította el, ha nem automatikusan lezárva, és térjen vissza a lap az Azure portálján. 
  
            Az átjárót futtató számítógépen ezek a hitelesítő adatok **titkosított** , amely az adatok gyári szolgáltatás tulajdonosa tanúsítvány használatával. Ha tanúsítványra van társítva az adatkezelési átjáró inkább használni kívánt, nézze meg [biztonságosan hitelesítő adatainak beállítása](#set-credentials-and-security).    
    1.  Kattintson a **központi telepítés** az SQL Server csatolt szolgáltatás üzembe a parancssávon. Meg kell jelennie a csatolt szolgáltatás a fastruktúrájú nézetben. 
        
        ![A fastruktúrájú nézetben csatolt SQL Server szolgáltatást](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Az Azure tároló ügyfélhez csatolt szolgáltatás hozzáadása
 
1. **Adatok Factory-szerkesztőben**kattintson a **új adatokat tárolja** a parancssávon, majd **Azure tároló**.
2. A **fiók nevét**adja meg a Azure tárterület-fiókja nevét.
3. Írja be a **fiókkulcs**az Azure tárterület-fiók a billentyűt.
4. Kattintson a **Deploy** bevezetését tervezi a **AzureStorageLinkedService**.
   
 
## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben hozzon létre a bemeneti és kimeneti adatkészleteket, a Másolás bemeneti és kimeneti adatait megjelenítő (helyszíni SQL Server-adatbázis = > Azure blob-tárolóhoz). Mielőtt hoz létre adatkészleteket, hajtsa végre az alábbi lépéseket (a lépések részletes leírását a lista követi):

- **A vállalati projektirányítási** a hozzá csatolt szolgáltatásként adatok gyári SQL Server-adatbázisból nevű táblázat létrehozása és beszúrása a minta bejegyzéseit a táblázatba, néhány.
- Hozzon létre **adftutorial** hozzá csatolt szolgáltatásként adatok gyári Azure blob tároló fiók nevű blob-tároló.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Az oktatóprogram helyszíni SQL Server előkészítése

1. A megadott a helyszíni SQL Server-adatbázis csatolt szolgáltatás (**SqlServerLinkedService**) segítségével a következő SQL-parancsfájl a **Vállalati projektirányítási** tábla létrehozása az adatbázisban.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Néhány példa beszúrása a táblázatba: 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Beviteli adatkészlet létrehozása

1. Az **Adatok Factory-szerkesztőben**kattintson **... További**kattintson az **Új adatkészlet** a parancssávon, és kattintson az **SQL Server-tábla**. 
2.  A jobb oldali ablaktáblában a JSON cserélje ki a következő szöveggel:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Vegye figyelembe az alábbiakat: 

    - **típus** értéke **SqlServerTable**.
    - **táblanév** **a vállalati projektirányítási**értékre van állítva.
    - **linkedServiceName** **SqlServerLinkedService** (is létrehozott a csatolt szolgáltatás korábbi az útmutató.) értékre van állítva.
    - Beviteli adatkészlet, amely nem hozza létre az Azure Data Factory egy másik folyamat újra be kell állítani **a külső** **Igaz**. Ez azt jelzi, hogy a bemeneti adatok elő az Azure Data Factory szolgáltatás külső. Tetszés szerint megadhatja a minden olyan külső adatok házirendek az **externalData** elemet a **házirend** szakaszban.    

    Lásd: az [adatokat, és az SQL Server](data-factory-sqlserver-connector.md) JSON tulajdonságok kapcsolatban további tájékoztatást.
2. Kattintson a **központi telepítés** a telepítéshez használni az adatkészlet parancssávon.  


### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása

1.  **Adatok gyári szerkesztő**kattintson az **Új adatkészlet** a parancssávon, és kattintson az **Azure Blob-tárolóhoz**.
2.  A jobb oldali ablaktáblában a JSON cserélje ki a következő szöveggel: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Vegye figyelembe az alábbiakat: 
    
    - **típus** értéke **AzureBlob**.
    - **linkedServiceName** **AzureStorageLinkedService** (is létrehozott a csatolt szolgáltatás a 2) értékre van állítva.
    - **Mappa_útvonala** **adftutorial/outfromonpremdf** értékre van állítva, ahol outfromonpremdf-e a adftutorial tároló mappájában. Ha még nem létezik a **adftutorial** tároló létrehozása 
    - **Elérhetőség** a **óránként** (**gyakoriság** **órára** és **intervallum** értéke **1**) értékre van állítva.  Az adatok gyári szolgáltatás készít egy kimenet szeletre óránként a **Vállalati projektirányítási** táblázat az Azure SQL-adatbázisban. 

    Ha nem ad meg egy **fájlnevet** egy **eredménytábla**, a **Mappa_útvonala** a létrehozott fájlok neve a következő formátumban: adatok. <Guid>.txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **Mappa_útvonala** és **fileName** dinamikusan alapján a **SliceStart** időpontot szeretne beállítani, a partitionedBy tulajdonsággal. A következő példában Mappa_útvonala használja évet, hónapot és napot az a SliceStart (a feldolgozása szeletet kezdetének), valamint fileName a SliceStart órára. Ha például egy szeletet éppen állítanak elő 2014-es – 10-20T08:00:00, a mappanév wikidatagateway/wikisampledataout/2014-es/10/20 van állítva, és a fájlnév 08.csv van beállítva. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    Kapcsolatos további tudnivalók: [Azure Blob-tárolóhoz és az adatok áthelyezése](data-factory-azure-blob-connector.md) JSON tulajdonságait.
2.  Kattintson a **központi telepítés** a telepítéshez használni az adatkészlet parancssávon. Győződjön meg arról, hogy látható-e mindkét az adatkészleteket, a fastruktúrájú nézetben.  

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehoz egy **folyamat** tartalmazó **EmpOnPremSQLTable** használja, mint a bemeneti egy **Példány tevékenység** és **OutputBlobTable** ezt az eredményt.

1.  Az adatok Factory-szerkesztőben kattintson a **... További**, és kattintson az **új folyamat**. 
2.  A jobb oldali ablaktáblában a JSON cserélje ki a következő szöveggel: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                    },
                    "Policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "NewestFirst",
                      "style": "StartOfInterval",
                      "retry": 0,
                      "timeout": "01:00:00"
                    }
                  }
                ],
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > A **Kezdés** tulajdonság értékét cserélje le az aktuális nap és **Záró** érték, a következő napra.

    Vegye figyelembe az alábbiakat:
 
    - A tevékenységek csoportban található csak a tevékenység amelynek **típus** értéke **másolása**
    - A tevékenység **beviteli** **EmpOnPremSQLTable** van állítva, és a tevékenység **kimeneti** **OutputBlobTable**van beállítva.
    - **TypeProperties** csoportban a **forrás típusa** **SqlSource** van megadva, és **BlobSink **van megadva, **gyűjtő típusa**.
    - SQL-lekérdezés `select * from emp` **SqlSource** **sqlReaderQuery** tulajdonsága meg van adva.

     Mindkét indítsa el, és a záró időpontok [ISO formátumban](http://en.wikipedia.org/wiki/ISO_8601)kell lennie. Példa: 2014-10-14T16:32:41Z. A **befejezési** idő nem kötelező, de ebben az oktatóanyagban használjuk. 
    
    Ha nem adja meg a **befejezési** tulajdonság értékét, mint "**Kezdés + 48 óra**" számítja ki. A folyamat végtelen időre szóló futtatásához adja meg a **befejezési** tulajdonság értéke **9/9/9999** . 
    
    Az időtartam, amelyben az adatok szeletek feldolgozása, az egyes Azure Data Factory adatkészlet definiált **elérhetősége** tulajdonságok alapján meghatározása
    
    A példában szereplő vannak 24 adatok szeletek minden szeletre óránként előállított-e.     
2. Kattintson a **központi telepítés** a telepítéshez használni az adatkészlet parancssávon (tábla is egy négyszögletű adatkészlet). Győződjön meg arról, hogy a folyamat kisalkalmazás azt mutatja a fastruktúrájú nézetben a **folyamatok** csomópontot.  
5. Ezután kattintson az **X** kétszeri lenyomásával bezárása kattintva térjen vissza az **Adatokat gyári** lap esetében a **ADFTutorialOnPremDF**rögzítéséhez.

**Gratulálok!** Sikeresen létrehozni az Azure adatok gyári, a csatolt szolgáltatások, a adatkészleteket, és egy folyamat, és a folyamat ütemezett.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Az adatok gyári megtekintése a Diagram nézetben 
1. Az **Azure-portálon**kattintson a **Diagram** csempét a **ADFTutorialOnPremDF** data factory kezdőlapján. :

    ![Diagram hivatkozás](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Meg kell jelennie a diagramot, az alábbi képhez hasonló:

    ![Diagram nézetben](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Nagyítás, Kicsinyítés, a Nagyítás 100 %-ra, a Nagyítás igazítása, automatikus folyamatok és adatkészleteket, és felsorolása megjelenítése (kiemeli a kijelölt elemek felsőbb szintű és lefelé irányuló elemeket).  Objektum (bemeneti és kimeneti adatkészlet vagy folyamat) szeretné megjeleníteni a tulajdonságokat rá duplán. 

## <a name="monitor-pipeline"></a>Monitor folyamat
Ebben a lépésben használatával az Azure portál figyelheti az Azure adatok gyári fog. PowerShell-parancsmagok figyelheti a adatkészleteket és folyamatok is használhatja. Figyelése kapcsolatos részletekért olvassa el a [Monitor és folyamatok kezelése](data-factory-monitor-manage-pipelines.md)című témakört.

5. A diagramon kattintson duplán a **EmpOnPremSQLTable**.  

    ![A szeletek EmpOnPremSQLTable](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Figyelje meg, hogy az adatok szeletre be az összes állapotban vannak **készen áll arra,** mert a folyamat időtartama (a kezdési időpontot a Befejezés időpontja) múltbeli. Is van, mert az SQL Server-adatbázis szúrta be az adatokat, és van a mindig. Győződjön meg arról, hogy nincs szeletek megjelennek a a **probléma szeletek** szakaszban, a képernyő alján. Az összes szelet megtekintéséhez kattintson a **Lásd még** szeletek listájának alján. 
7. Ezután kattintson a **adatkészleteket** lap **OutputBlobTable**.

    ![A szeletek OputputBlobTable](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Kattintson bármelyik szeletre a listából, és meg kell jelennie a **Szeletre** lap. Megjelenik a tevékenység fut, a szeletet. Futtassa a általában csak egy tevékenység látni.  

    ![Adatok szeletet lap](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Ha a szeletet nem **kész** állapotú, megjelenik a felsőbb szintű szeletek, amely nem készen áll, amelyek gátolják az aktuális szelet végrehajtása a **felsőbb szintű szeletek, amelyek nem áll készen** listában.

10. Kattintson a **tevékenység futtatása** **tevékenység futtassa a részletek**megjelenítéséhez a képernyő alján a listából.

    ![Tevékenység futtatása Részletek lap](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Átviteli, időtartam és az átjáró, az adatok átvitelére használt például információkat szeretne látni. 
11. Kattintson az **X** , amíg az összes rögzítéséhez bezárásához 
12. vissza szeretne lépni a Kezdőlap lap a **ADFTutorialOnPremDF**számára.
14. (nem kötelező) Kattintson a **folyamatok** **ADFTutorialOnPremDF**kattintson, és hatoljon beviteli tábla (**felhasznált**) vagy a kimeneti adatkészleteket (**Produced**) keresztül.
15. Eszközeivel használata eszközöket, például a [Microsoft tároló Explorer](http://storageexplorer.com/) például annak ellenőrzéséhez, hogy egy blob fájlt az órát.

    ![Azure tároló Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Következő lépések

- Lásd: [Az adatkezelési átjáró](data-factory-data-management-gateway.md) a cikk az adatkezelési átjáró olvashat.
- Az [adatok másolása az Azure SQL Azure Blob](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) egy adatforrás adattár adatok áthelyezése egy gyűjtő adattár másolás tevékenység használatával kapcsolatban további tudnivalókat olvashat. 
