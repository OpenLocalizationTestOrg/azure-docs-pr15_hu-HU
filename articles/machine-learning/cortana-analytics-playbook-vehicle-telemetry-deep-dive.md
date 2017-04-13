<properties 
    pageTitle="Gépjármű telemetriai analytics megoldás playbook: mély merülési a megoldásba |} Microsoft Azure" 
    description="A Cortana intelligenciával kapcsolatos funkcióinak használata valós idejű és a prediktív háttérismeretek gépjármű állapot és a vezetői eléréséhez szokások." 
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
    ms.date="09/12/2016" 
    ms.author="bradsev" />


# <a name="vehicle-telemetry-analytics-solution-playbook-deep-dive-into-the-solution"></a>Gépjármű telemetriai analytics megoldás playbook: mély merülési a megoldásba

Ez a **menüben** a e playbook szakaszait mutató hivatkozásokat tartalmaz: 

[AZURE.INCLUDE [cap-vehicle-telemetry-playbook-selector](../../includes/cap-vehicle-telemetry-playbook-selector.md)]

A szakasz gyakorlatokat lefelé a megoldási utasításokat és a Tanács az testreszabási kitaláltak szakaszok mindegyikbe. 

## <a name="data-sources"></a>Adatforrások

A megoldás két különböző adatforrásokat használja:

- **szimulált gépjármű jelek és diagnosztikai adatkészlet** és 
- **gépjármű katalógus**

Egy gépjármű telematika simulatort használja az részeként ebben a megoldást. Diagnosztikai adatok bocsát ki, és azt jelzi, hogy a gépjármű állapotát, és az egy adott pontján vezetői mintának megfelelő időben. Kattintson a saját igényeknek megfelelően alakítható testreszabásokat **Gépjármű telematika simulatort használja a Visual Studio megoldás** letöltése [Gépjármű telematika simulatort használja](http://go.microsoft.com/fwlink/?LinkId=717075) . A gépjármű katalógus tartalmazza azokat a modell hozzárendelése egy VIN az összefoglaló adatkészletet.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig2-vehicle-telematics-simulator.png)

*2 – gépjármű telematika simulatort használja ábra*

Ez a JSON-formátumú adatkészletet, amely tartalmazza a következő sémában.

Oszlop | Leírás | Értékek 
 ------- | ----------- | --------- 
VIN | Gépjármű-azonosító tízjegyű szám | A fő 10 000 tízjegyű gépjármű azonosítószámok listája származik.
Külső hőmérsékleti | A külső hőmérsékleti, ahol a gépjármű adja | A 0-100 tízjegyű szám
Hőmérsékleti motor | A motor színhőmérsékletét a gépjármű | A 0-500 tízjegyű szám
Sebessége | A motor sebessége, amelynél a gépjármű Előfeltételek | A 0-100 tízjegyű szám
Üzemanyag | A gépjármű üzemanyag szintje | Tízjegyű szám, a 0-100 (jelzi üzemanyag százalékos értéke)
EngineOil | A gépjármű motor olaj szintje | Tízjegyű szám, a 0-100 (jelzi a motor olaj százalékos értéke)
Autógumit nyomása | A gépjármű autógumit nyomása | Véletlen számot a 0-50 (jelzi autógumit nyomása százalékos értéke)
Kilométer | A gépjármű a kilométer olvasása | A 0-200000-rel tízjegyű szám
Accelerator_pedal_position | A gépjármű kerékpárok accelerator pozícióját | Tízjegyű szám, a 0-100 (jelzi accelerator százalékos értéke)
Parking_brake_status | Azt jelzi, hogy a gépjármű leparkolni-e, vagy nem | IGAZ vagy hamis
Headlamp_status | Azt jelzi, ha a fényszóró van-e | IGAZ vagy hamis
Brake_pedal_status | Azt jelzi, hogy a fékpedál billentyű lenyomásakor-e | IGAZ vagy hamis
Transmission_gear_position | A gépjármű átviteli fogaskerék pozícióját | Állam: első, második, harmadik, negyedik ötödik, hatodik, hetedik, nyolcadik
Ignition_status | Azt jelzi, hogy a gépjármű állapotát | IGAZ vagy hamis
Windshield_wiper_status | Azt jelzi, hogy a szélvédőkeret ablaktörlő vagy nem van-e kapcsolva | IGAZ vagy hamis
ABS | Azt jelzi, hogy az ABS be kell kapcsolni, vagy nem | IGAZ vagy hamis
Időbélyeg | Az időbélyegző, amikor az adatpont jön létre | Dátum
A város | Hol található a gépjármű | Ebben a megoldásban 4 városok: Verőcei, Redmond, Sammamish, Seattle


A gépjármű modell hivatkozás adatkészlet VIN a modell megfeleltetés tartalmaz. 

VIN | Modell |
--------------|------------------
FHL3O1SA4IEHB4WU1 | Sedan |
8J0U8XCPRGW4Z3NQE | Hibrid |
WORG68Z2PLTNZDBI7 | Családi limuzin |
JTHMYHQTEPP4WBMRN | Sedan |
W9FTHG27LZN1YWO0Y | Hibrid |
MHTP9N792PHK08WJM | Családi limuzin |
EI4QXI2AXVQQING4I | Sedan |
5KKR2VB4WHQH97PF8 | Hibrid |
W9NSZ423XZHAONYXB | Családi limuzin |
26WJSGHX4MA5ROHNL | Kabrió |
GHLUB6ONKMOSI7E77 | Állomás kocsi |
9C2RHVRVLMEJDBXLP | A kompakt autó |
BRNHVMZOUJ6EOCP32 | Kis SUV |
VCYVW0WUZNBTM594J | Masniba |
HNVCE6YFZSA5M82NY | Közepes SUV |
4R30FOR7NUOBL05GJ | Állomás kocsi |
WYNIIY42VKV6OQS1J | Nagy SUV |
8Y5QKG27QET1RBK7I | Nagy SUV |
DF6OX2WSRA6511BVG | Coupe |
Z2EOZWZBXAEW3E60T | Sedan |
M4TV6IEALD5QDS3IR | Hibrid |
VHRA1Y2TGTA84F00H | Családi limuzin |
R0JAUHT1L1R3BIKI0 | Sedan |
9230C202Z60XX84AU | Hibrid |
T8DNDN5UDCWL7M72H | Családi limuzin |
4WPYRUZII5YV7YA42 | Sedan |
D1ZVY26UV2BFGHZNO | Hibrid |
XUF99EW9OIQOMV7Q7 | Családi limuzin
8OMCL3LGI7XNCC21U | Kabrió |
…….  |   |


### <a name="to-generate-simulated-data"></a>Szimulált adatok létrehozásához
1.  Az adatok simulatort használja csomag letöltéséhez kattintson a jobb felső részén a gépjármű telematika simulatort használja csomópontot a nyílra. Mentse, és bontsa ki a fájlokat a számítógépen helyben. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig3-vehicle-telemetry-blueprint.png)*Ábra 3 – gépjármű Telemetriai Analytics megoldás tervrajz*

2.  A helyi számítógépen nyissa meg azt a mappát, ahová a gépjármű telematika simulatort használja csomagot. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig4-vehicle-telematics-simulator-folder.png)*Ábra 4 – gépjármű telematika simulatort használja mappa*

3.  Az alkalmazás **CarEventGenerator.exe**végrehajtása.

### <a name="references"></a>Hivatkozások

[Gépjármű telematika simulatort használja a Visual Studio megoldás](http://go.microsoft.com/fwlink/?LinkId=717075) 

[Azure esemény központi](https://azure.microsoft.com/services/event-hubs/)

[Azure Data Factory](https://azure.microsoft.com/documentation/learning-paths/data-factory/)


## <a name="ingestion"></a>Bevitel
Azure esemény hubok, a megjelenítő Értékáram-elemzés és az adatok gyári kombinációi kapcsolatos vannak károkozásra való ingest a gépjármű jelek, a diagnosztikai események és a valós idejű és analytics köteg. Minden összetevő létrehozása és a megoldás üzembe helyezési részeként beállítva. 

### <a name="real-time-analysis"></a>Valós idejű elemzése
Az események generálja a gépjármű telematika simulatort használja az esemény-központban, az esemény központi SDK használatával vannak közzétéve. A Értékáram-elemzés feldolgozás ingests ezek az események az esemény-központban, és a valós idejű gépjármű állapotát jelző elemezheti az adatokat dolgoz fel. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig5-vehicle-telematics-event-hub-dashboard.png) 

*Ábra 5 - esemény központ irányítópult*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig6-vehicle-telematics-stream-analytics-job-processing-data.png) 

*Ábra 6 - adatfolyam analytics feladat adatok feldolgozása*

Az adatfolyamban analytics feladat;

- az esemény-központban adatainak ingests 
- a hivatkozás adatokkal, amelyek a gépjármű VIN feleltesse meg a megfelelő modell illesztés végrehajtása 
- az Azure blob-tárolóhoz gazdag köteg elemzéséhez állandósul őket. 

A következő adatfolyam analytics lekérdezés az adatok megmaradnak az Azure blob-tárolóhoz szolgál. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig7-vehicle-telematics-stream-analytics-job-query-for-data-ingestion.png) 

*Ábra 7 - adatfolyam analytics feladat lekérdezés adatok szempontjából*

### <a name="batch-analysis"></a>Köteg elemzése
Azt is generál további mennyisége szimulált gépjármű jelek és a diagnosztikai adatkészlet sokoldalúbb köteg ábrázolja. Ez a szükséges ahhoz, hogy jó képviselő adatok parancsfájl mennyiségig. Erre a célra azt egy "PrepareSampleDataPipeline" nevű folyamat Azure Data Factory-munkafolyamaton belül a létrehozásához használt szimulált gépjármű jelek és diagnosztikai adatkészlet egy évvel értékű. Kattintson az [adatok gyári egyéni tevékenységeket](http://go.microsoft.com/fwlink/?LinkId=717077) Data Factory egyéni DotNet tevékenység Visual Studio megoldás saját igényeknek megfelelően alakítható testreszabásokat letöltése. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig8-vehicle-telematics-prepare-sample-data-for-batch-processing.png) 

*Ábra 8 - köteg feldolgozás munkafolyamat előkészítése*

A folyamat áll egy egyéni ADF .net tevékenységét, itt megjelenítése:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig9-vehicle-telematics-prepare-sample-data-pipeline.png) 

*Ábra 9 - PrepareSampleDataPipeline*

Miután sikeresen végrehajtja a a folyamat, és "RawCarEventsTable" adatkészlet van-e jelölve "Kész" egyéves értékű szimulált gépjármű jelek és diagnosztikai adatok darab termék készült. A következő mappába és az "connectedcar" tároló alatt a tárterület-fiókokban létrehozott fájl jelenik meg:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig10-vehicle-telematics-prepare-sample-data-pipeline-output.png) 

*Szám 10 - PrepareSampleDataPipeline kimeneti*

### <a name="references"></a>Hivatkozások

[Azure esemény központi SDK adatfolyam szempontjából](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

[Azure Data Factory adatok mozgását funkciók](../data-factory/data-factory-data-movement-activities.md)
[Azure adatok gyári DotNet tevékenység](../data-factory/data-factory-use-custom-activities.md)

[Azure adatok gyári DotNet tevékenység visual studio megoldás mintaadatok előkészítése](http://go.microsoft.com/fwlink/?LinkId=717077) 


## <a name="partition-the-dataset"></a>Az adatkészlet partition

Az adatok előkészítése lépésben az év és hónap formátumot a nyers félig strukturált gépjármű jelek és diagnosztikai adatkészlet vannak formázva. Ez a szétválasztás ajánl hatékonyabb lekérdezése és méretezhető hosszú távú tároló hibafa fölé egy blob-fiókból a következőre engedélyezésével, mint az első fiók megtelik. 

>[AZURE.NOTE] Ebben a lépésben a megoldás olyan csak parancsfájl alkalmazható.

Bemeneti és kimeneti adatok adatok kezelése:

- A **kimeneti adatai** (címkézett *PartitionedCarEventsTable*) hosszú ideig legyen a eligazodást / "rawest" a "adatok tó" ügyfél-adatok megmaradjanak van. 
- Ez a folyamat **bemeneti adatok** általában volna eldobása, a kimeneti adatokat tartalmaz a bemeneti a teljes funkcióvesztést - csak tárolt (particionálva) jobban későbbi használatra.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig11-vehicle-telematics-partition-car-events-workflow.png)

*Ábra 11 – partíciót autós események munkafolyamat*

A nyers adatokból particionált a struktúra HDInsight tevékenységének használata "PartitionCarEventsPipeline". A mintaadatokat, az 1 az év által generált particionált év és hónap szerint. A partíciók az egyes (összesen 12 partíciót) év gépjármű jelek és diagnosztikai adatok létrehozásához használt. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig12-vehicle-telematics-partition-car-events-pipeline.png)

*Ábra 12 - PartitionCarEventsPipeline*

A következő struktúra parancsfájl, névvel ellátott "partitioncarevents.hql", szétválasztás szolgál, és a letöltött zip "\demo\src\connectedcar\scripts" mappájában található. 


    SET hive.exec.dynamic.partition=true;
    SET hive.exec.dynamic.partition.mode = nonstrict;
    set hive.cli.print.header=true;

    DROP TABLE IF EXISTS RawCarEvents; 
    CREATE EXTERNAL TABLE RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RAWINPUT}'; 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
    ) partitioned by (YearNo int, MonthNo int) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDOUTPUT}';

    DROP TABLE IF EXISTS Stage_RawCarEvents; 
    CREATE TABLE IF NOT EXISTS Stage_RawCarEvents 
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string,
                YearNo                          int,
                MonthNo                         int) 
    ROW FORMAT delimited fields terminated by ',' LINES TERMINATED BY '10';

    INSERT OVERWRITE TABLE Stage_RawCarEvents
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        Year(gendate),
        Month(gendate)

    FROM RawCarEvents WHERE Year(gendate) = ${hiveconf:Year} AND Month(gendate) = ${hiveconf:Month}; 

    INSERT OVERWRITE TABLE PartitionedCarEvents PARTITION(YearNo, MonthNo) 
    SELECT
        vin,            
        model,
        timestamp,
        outsidetemperature,
        enginetemperature,
        speed,
        fuel,
        engineoil,
        tirepressure,
        odometer,
        city,
        accelerator_pedal_position,
        parking_brake_status,
        headlamp_status,
        brake_pedal_status,
        transmission_gear_position,
        ignition_status,
        windshield_wiper_status,
        abs,
        gendate,
        YearNo,
        MonthNo
    FROM Stage_RawCarEvents WHERE YearNo = ${hiveconf:Year} AND MonthNo = ${hiveconf:Month};

*Ábra 13 - PartitionConnectedCarEvents struktúra parancsfájl*

Miután a folyamat sikeresen megy végbe, hozza létre a tárterület-fiókjában a "connectedcar" tároló területen a következő partíciók látni.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig14-vehicle-telematics-partitioned-output.png)

*Ábra 14 - particionált kimeneti*

Az adatok most már van optimalizálva, kezelhető, és készen áll a további feldolgozásra gazdag köteg háttérismeretek eléréséhez. 

## <a name="data-analysis"></a>Adatok elemzése

Ebben a részben, megtudhatja, hogy miként rich analytics speciális gépjármű egészségügyi és szokások ösztönzése összevonhatja Azure Értékáram-elemzés, Azure gépi tanulási, Azure Data Factory és Azure hdinsight szolgáltatáshoz. Vannak az alábbi három részek:

1.  **Gépi tanulási**: Ebben a részben a rendellenességet észlelési kísérlet, amely azt a megoldást igénylő karbantartási karbantartási járműveket és biztonsági problémák miatt visszaírások igénylő járműveket előrejelzésére használt információkat tartalmaz.
2.  **Valós idejű analysis**: ezen alszakasz a valós idejű analitikát, a adatfolyam Analytics lekérdezési nyelvet használ, és a gépi tanulási kísérlet egy egyéni alkalmazásban valós időben operationalizing vonatkozó információkat tartalmazza.
3.  **Köteg analysis**: ezen alszakasz tájékoztatást nyújt a átalakítása kapcsolatban, illetve a köteg adatok az Azure hdinsight szolgáltatáshoz és Azure gépi tanulási által Azure Data Factory operationalized feldolgozása.

### <a name="machine-learning"></a>Gépi tanulási

Itt célunk találnia a járműveket, hogy karbantartási vagy a visszahívási alapján az egyes puszták statisztikákat. A következő feltételezések VÁLLALUNK

- Egyet az alábbi három feltétel teljesülése esetén csak a járműveket, **karbantartási karbantartása**:
    - Autógumit nyomása halkítva
    - Motor olaj szinten halkítva
    - Motor hőmérsékleti fel hangosítva

- Egy, az alábbi feltételek teljesülése esetén a járműveket egy **biztonsági probléma** van, és csak a **visszahívási**:
    - Motor hőmérsékleti magas, de külső hőmérsékleti alacsony
    - Motor hőmérsékleti alacsony azonban külső hőmérsékleti magas

Az előző követelmények alapján, hogy nem hozott létre két külön modell feltárása rendellenességeinek, egy gépjármű karbantartási kimutatására, és egy gépjármű visszahívási kimutatására. A beépített fő összetevő-elemzés (PEM) algoritmus mindkét alábbi modellekben rendellenességet észlelési használatos. 

**Karbantartási észlelési modell**

Egy három jelölők autógumit nyomása, motor olaj vagy motor hőmérsékleti - eleget tesz a megfelelő állapotát, ha a karbantartás észlelési modell a anomáliát jelentést. Emiatt azt csak kell vegye figyelembe az alábbi három változóval létrehozásakor a modell. A kísérlet az Azure gépi tanulási azt először használatával egy **Oszlop kijelölése az adatkészlet** modulra bontsa ki a következő három változóval. Ezután azt használatával a PEM-alapú rendellenességet észlelési modul a rendellenességet észlelési modell. 

Egyszerű összetevő-elemzés (PEM) a gépi tanulási szolgáltatásainak kiválasztása, osztályozás és rendellenességet észlelési alkalmazható egy elfogadott technika. PEM egy sor olyan eset esetleg korrelációs változót tartalmazó alapvető alkatrészek nevű értékhalmaz alakítja át. A fő arról is, modellezési PEM-alapú van a projektadatok egy alsó többdimenziósak területére, hogy a szolgáltatások és anomáliák könnyebben azonosítható.
 
Az észlelési modellhez minden új beviteli a rendellenességet jelentő először kiszámítja annak a eigenvectors a kivetítés, és majd kiszámítja a normalizált újjáépítése hiba. A normalizált a program a rendellenességet pontszámhoz. Minél magasabb ez a hiba, anomalous több példány. 

A karbantartás észlelési probléma az egyes rekordok tekinthető 3 többdimenziósak forgatásokat pont által meghatározott autógumit nyomása motor olaj és motor hőmérsékleti koordinátákat. Ezek rendellenességeinek rögzítéséhez azt is a project a 3-as többdimenziósak helyen az eredeti adatok egy háromdimenziós 2 területére PEM használatával. Így be van állítva a paraméter segítségével PEM 2-nek alkatrészek száma. Ez a paraméter fontos szerepet tölt be PEM-alapú rendellenességet észlelési alkalmazásával. Tervezésére adatok PEM használatával, után is azonosítjuk ezek rendellenességeinek könnyebben.

**A visszahívási rendellenességet észlelési modell** A visszahívási rendellenességet észlelési modell használjuk, az oszlopok kiválasztása az adatkészlet és PEM-alapú rendellenességet észlelési modulok hasonló módon. Kifejezetten azt először kibontása három változóval - motor hőmérsékleti, külső hőmérsékleti és sebességének – az **Oszlopok kiválasztása az adatkészlet** modulról. Mivel a motor hőmérsékleti általában van összefüggésbe sebességét is szerepel a sebesség változó. Ezután használjuk PEM-alapú rendellenességet észlelési modul Project a 3-as többdimenziósak tér át az adatokat egy háromdimenziós 2 területére. A visszahívási feltétel teljesül, és a gépjármű megkívánja visszahívási amikor motor hőmérsékleti és a külső hőmérsékleti erősen negatív közötti kapcsolatot. PEM-alapú rendellenességet észlelési algoritmus használ, azt is rögzíthet rendellenességeket PEM végrehajtása után. 

Ha bármelyik modell képzés, szükség normál adatok, amelyre nincs szükség a karbantartás vagy a visszahívási mint a bemeneti adatok betanítása a PEM-alapú rendellenességet észlelési modell használata. A pontozási kísérlet a használjuk a képzett rendellenességet észlelési modell feltárása a gépjármű karbantartási vagy a visszahívás szükséges-e vagy sem. 


### <a name="real-time-analysis"></a>Valós idejű elemzése

Az alábbi adatfolyam Analytics SQL-lekérdezés például gépjármű sebességét, üzemanyag szint, motor hőmérsékleti, kilométer olvasási, autógumit nyomása, motor olaj szint és mások fontos gépjármű-paraméterek átlagát első szolgál. Átlagok rendellenességeinek észleli, értesítések, a kibocsátás és az adott régióban üzemeltetett járműveket általános állapot feltételeinek meghatározására és majd összehangolására, hogy demográfiai szolgálnak. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig15-vehicle-telematics-stream-analytics-query-for-real-time-processing.png)

Ábra 15 – valós idejű feldolgozás adatfolyam analytics lekérdezése

Az összes átlagokat fölé egy 3 másodperces TumblingWindow kerülnek kiszámításra. Akkor használja TubmlingWindow ebben az esetben óta mozaikszerűen és összefüggő időtartományok szükséges. 

További információk az Azure Értékáram-elemzés "Ablakok elrendezése"-funkcióiról, kattintson az [ablakok elrendezése (Azure Értékáram-elemzés)](https://msdn.microsoft.com/library/azure/dn835019.aspx)gombra.

**Valós idejű előrejelzési szolgáltatása**

Az alkalmazások megtalálható részeként a gépi tanulási modell valós idejű üzemeltető ját. Ez az alkalmazás neve "RealTimeDashboardApp" létrejön, és a megoldás üzembe helyezési részeként beállítva. Az alkalmazás hajtja végre a következőket:

1.  Ha Értékáram-elemzés van közzététele az események mintát folyamatosan esemény központi példányhoz figyeli. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig16-vehicle-telematics-stream-analytics-query-for-publishing.png)*Ábra 16 – adatfolyam analytics lekérdezést az adatok közzététele egy kimenet esemény központi példány* 

2.  Minden olyan esemény, ez az alkalmazás kapja: 

    - A folyamatok gépi tanulási kérés-válasz pontozási Erőforrásrekordok végpont az adatokat. A Erőforrásrekordok végpont automatikusan közzé, a telepítés részeként.
    - A PowerBI adatkészlet a leküldéses API-k használata a Erőforrásrekordok kimenet van közzétéve.

A minta akkor is szeretne egy vonalat a vállalati verzió (üzleti) alkalmazás integrálása a valós idejű analytics folyamat, például az értesítéseket, értesítéseket és üzenetküldés felhasználási területei esetek alkalmazandó.

Kattintson a [RealtimeDashboardApp töltse le](http://go.microsoft.com/fwlink/?LinkId=717078) a testre szabott funkciókra RealtimeDashboardApp Visual Studio megoldás letöltése gombra. 

**A valós idejű irányítópult alkalmazás végrehajtása**

1.  Kattintson a diagram nézetben a PowerBI csomópontra, és kattintson a Tulajdonságok ablaktábla "A valós idejű irányítópult alkalmazás letöltése" hivatkozásra. ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig17-vehicle-telematics-powerbi-dashboard-setup.png)*Ábra 17 – PowerBI irányítópult beállítására vonatkozó utasítások*
2.  Kibontása és a helyben menteni ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig18-vehicle-telematics-realtimedashboardapp-folder.png) *ábra 18 – RealtimeDashboardApp mappa*
3.  Az alkalmazás RealtimeDashboardApp.exe végrehajtása
4.  Érvényes Power BI hitelesítő adatait, jelentkezzen be, és kattintson az elfogadás ![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19a-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png)![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig19b-vehicle-telematics-realtimedashboardapp-sign-in-to-powerbi.png) 

*Ábra 19 – RealtimeDashboardApp: Jelentkezzen be az PowerBI*

>[AZURE.NOTE] Ha meg szeretné kiüríteni a PowerBI adatkészlet, hajtsa végre a RealtimeDashboardApp a "flushdata" paraméterrel: 

    RealtimeDashboardApp.exe -flushdata

### <a name="batch-analysis"></a>Köteg elemzése

Az alábbi célja, hogy hogyan a Contoso motorok használja az Azure számítási funkciók hasznosítására mintát, a használatát működését és gépjármű állapot vezetői szóló gazdag háttérismeretek eléréséhez nagy adatok megjelenítése. Ez teszi lehetővé:

- A felhasználói élmény fokozása, és készítsen belőle olcsóbb, mert az összefüggéseket a vezetői szokásai és üzemanyag hatékony vezetői viselkedése
- Ezzel kapcsolatban beérkező tudnivalók a vevők és a vezetői patters üzleti döntéseket szabályozzák, és adja meg a legjobb osztály-termékek és szolgáltatások

Ez a megoldás azt is célba juttatása a következő mérési módja miatt:

1.  **Olyan szigorú vezetői viselkedését**: azonosítja a trend a modellek, helyekre, vezetői feltételek és az év olyan szigorú vezetői mintázatok az összefüggéseket szükséges időt. Contoso motorok használható ezeket az összefüggéseket marketingkampányok, személyre szabott új szolgáltatások és biztosítási használatát-alapú ösztönzése.
2.  **Üzemanyag hatékony vezetői viselkedését**: azonosítja a trend a modellek, helyekre, vezetői feltételek és az év üzemanyag hatékony vezetői mintázatok az összefüggéseket szükséges időt. Contoso motorok ezeket az összefüggéseket marketingkampányok, ösztönzése az új szolgáltatások és a kezdő és a környezet rövid vezetői szokások illesztőprogramjainak megelőző jelentéskészítés költség. 
3.  **Modellek visszahívás**: visszaírások igénylő által a rendellenességet észlelési gépi tanulási kísérlet operationalizing modellek azonosítja

Nézzük meg ezeket a mértékek részleteit be


**Olyan szigorú vezetői mintával**

A "AggresiveDrivingPatternPipeline" struktúra használatával a modellek, hely, gépjármű, vezetői feltételek és további paramétereket határozza meg, hogy olyan szigorú vezetői mintát mutat nevű folyamat a particionált gépjármű jelek és diagnosztikai adatok feldolgozása.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig20-vehicle-telematics-aggressive-driving-pattern.png) 
*Ábra 20 – Aggressive mintát munkafolyamat ösztönzése*

A struktúra parancsfájl nevű olyan szigorú vezetői feltétel mintát elemzéséhez használható "aggresivedriving.hql" a "\demo\src\connectedcar\scripts" mappát a letöltött zip helyezkedik el. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS CarEventsAggresive; 
    CREATE EXTERNAL TABLE CarEventsAggresive
    (
                vin                         string, 
                model                       string,
                timestamp                   string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,
                brake_pedal_status          string,
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:AGGRESIVEOUTPUT}';



    INSERT OVERWRITE TABLE CarEventsAggresive
    select
    vin,
    model,
    timestamp,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND brake_pedal_status = '1' AND speed >= '50'

*Ábra: 21 – Aggressive ösztönzése mintát struktúra lekérdezés*

Fékezés nagy sebességű minta alapján reckless/olyan szigorú vezetői viselkedés észleli gépjármű a továbbítás fogaskerék pozíció, berendezés kerékpárok állapotát és sebességének kombinációját használja. 

Miután a folyamat sikeresen megy végbe, hozza létre a tárterület-fiókjában a "connectedcar" tároló területen a következő partíciók látni.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig22-vehicle-telematics-aggressive-driving-pattern-output.png) 

*Ábra: 22 – AggressiveDrivingPatternPipeline kimeneti*


**Üzemanyag hatékony vezetői mintával**

A "FuelEfficientDrivingPatternPipeline" nevű folyamat a particionált gépjármű jelek és diagnosztikai adatok feldolgozása. Struktúra azt határozza meg, a modellek, hely, gépjármű, vezetői feltételeket, és más tulajdonságait, amely üzemanyag hatékony vezetői mintát mutat.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig23-vehicle-telematics-fuel-efficient-driving-pattern.png) 

*Ábra 23 – üzemanyag hatékony vezetői mintát munkafolyamat*

A struktúra parancsfájl nevű olyan szigorú vezetői feltétel mintát elemzéséhez használható "fuelefficientdriving.hql" a "\demo\src\connectedcar\scripts" mappát a letöltött zip helyezkedik el. 

    DROP TABLE IF EXISTS PartitionedCarEvents; 
    CREATE EXTERNAL TABLE PartitionedCarEvents
    (
                vin                             string,
                model                           string,
                timestamp                       string,
                outsidetemperature              string,
                enginetemperature               string,
                speed                           string,
                fuel                            string,
                engineoil                       string,
                tirepressure                    string,
                odometer                        string,
                city                            string,
                accelerator_pedal_position      string,
                parking_brake_status            string,
                headlamp_status                 string,
                brake_pedal_status              string,
                transmission_gear_position      string,
                ignition_status                 string,
                windshield_wiper_status         string,
                abs                             string,
                gendate                         string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:PARTITIONEDINPUT}';

    DROP TABLE IF EXISTS FuelEfficientDriving; 
    CREATE EXTERNAL TABLE FuelEfficientDriving
    (
                vin                         string, 
                model                       string,
                city                        string,
                speed                       string,
                transmission_gear_position  string,                
                brake_pedal_status          string,            
                accelerator_pedal_position  string,                             
                Year                        string,
                Month                       string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:FUELEFFICIENTOUTPUT}';



    INSERT OVERWRITE TABLE FuelEfficientDriving
    select
    vin,
    model,
    city,
    speed,
    transmission_gear_position,
    brake_pedal_status,
    accelerator_pedal_position,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from PartitionedCarEvents
    where transmission_gear_position IN ('fourth', 'fifth', 'sixth', 'seventh', 'eight') AND parking_brake_status = '0' AND brake_pedal_status = '0' AND speed <= '60' AND accelerator_pedal_position >= '50'


*Ábra 24 – üzemanyag hatékony vezetői mintát struktúra lekérdezések*

Gépjármű a továbbítás fogaskerék pozíció kombinációját használja, berendezés kerékpárok állapot, sebességétől és accelerator pedál pozíció üzemanyag hatékony vezetői viselkedés gyorsítás, fékezést alapján feltárása gyorsítása a mintázattal. 

Miután a folyamat sikeresen megy végbe, hozza létre a tárterület-fiókjában a "connectedcar" tároló területen a következő partíciók látni.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig25-vehicle-telematics-fuel-efficient-driving-pattern-output.png) 

*Ábra 25 – FuelEfficientDrivingPatternPipeline kimeneti*


**Az előrejelzések visszahívása**

A gépi tanulási kísérlet kiépítéstől és a megoldás üzembe helyezési részeként webszolgáltatás közzétett. A köteg végpont pontozási van a munkafolyamat adatok csatolt gyári szolgáltatásként regisztrált és operationalized adatok gyári köteg tevékenység pontozási használatával kapcsolatos károkozásra.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig26-vehicle-telematics-machine-learning-endpoint.png) 

*Csatolt adatok gyári szolgáltatásként regisztrált ábra 26 – gépi tanulási végpont*

A bejegyzett csatolt szolgáltatás a DetectAnomalyPipeline szolgál az pontszám az adatok rendellenességet észlelési modellt használja. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig27-vehicle-telematics-aml-batch-scoring.png) 

*Ábra 27 – az adatok gyári Azure gépi tanulási köteg pontozási tevékenység* 

Létezik néhány olyan lépés, ez az adatok előkészítése során végzett, hogy a köteg webszolgáltatás pontozási is operationalized. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig28-vehicle-telematics-pipeline-predicting-recalls.png) 

*Ábra 28 – az visszaírások igénylő járműveket előrejelzésére DetectAnomalyPipeline* 

A pontozási befejezése után HDInsight tevékenység folyamat, és az adatok, amelyek a modell egy valószínűség pontszám 0,60 által rendellenességeinek szerint kategorizált vagy újabb összesítése szolgál.

    DROP TABLE IF EXISTS CarEventsAnomaly; 
    CREATE EXTERNAL TABLE CarEventsAnomaly 
    (
                vin                         string,
                model                       string,
                gendate                     string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                fuel                        string,
                engineoil                   string,
                tirepressure                string,
                odometer                    string,
                city                        string,
                accelerator_pedal_position  string,
                parking_brake_status        string,
                headlamp_status             string,
                brake_pedal_status          string,
                transmission_gear_position  string,
                ignition_status             string,
                windshield_wiper_status     string,
                abs                         string,
                maintenanceLabel            string,
                maintenanceProbability      string,
                RecallLabel                 string,
                RecallProbability           string
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:ANOMALYOUTPUT}';

    DROP TABLE IF EXISTS RecallModel; 
    CREATE EXTERNAL TABLE RecallModel 
    (

                vin                         string,
                model                       string,
                city                        string,
                outsidetemperature          string,
                enginetemperature           string,
                speed                       string,
                Year                        string,
                Month                       string              
                                
    ) ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' LINES TERMINATED BY '10' STORED AS TEXTFILE LOCATION '${hiveconf:RECALLMODELOUTPUT}';

    INSERT OVERWRITE TABLE RecallModel
    select
    vin,
    model,
    city,
    outsidetemperature,
    enginetemperature,
    speed,
    "${hiveconf:Year}" as Year,
    "${hiveconf:Month}" as Month
    from CarEventsAnomaly
    where RecallLabel = '1' AND RecallProbability >= '0.60'


Miután a folyamat sikeresen megy végbe, hozza létre a tárterület-fiókjában a "connectedcar" tároló területen a következő partíciók látni.

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig30-vehicle-telematics-detect-anamoly-pipeline-output.png) 

*Ábra 30 – ábra 30 – DetectAnomalyPipeline kimeneti*


## <a name="publish"></a>Közzététel

### <a name="real-time-analysis"></a>Valós idejű elemzése

A lekérdezések a adatfolyam analytics feladat egyik közzé, az események olyan eredménye esemény központi példány. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig31-vehicle-telematics-stream-analytics-job-publishes-output-event-hub.png)

*Ábra 31 – analytics feladat közzé, a kibocsátás adatfolyam esemény központi példány*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig32-vehicle-telematics-stream-analytics-query-publish-output-event-hub.png)

*Ábra 32 – a kimenet közzététele lekérdezés analitikájának adatfolyam esemény központi példány*

Az események követő által a RealTimeDashboardApp a megoldásban felhasznált. Ez az alkalmazás a gépi tanulási kérés-válasz webszolgáltatás valós idejű pontozási használja, és közzé, a PowerBI adatkészlet fogyasztási eredő adatok. 

### <a name="batch-analysis"></a>Köteg elemzése

A köteget, és a valós idejű feldolgozás eredményét az Azure SQL-adatbázistáblák fogyasztási vannak közzétéve. Az Azure SQL-kiszolgálót, adatbázis és a táblázat automatikusan létrehozza a telepítési parancsfájlt részeként. 

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig33-vehicle-telematics-batch-processing-results-copy-to-data-mart.png)

*Ábra 33 – köteg eredmények másolás adatok intelligens munkafolyamat feldolgozása*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig34-vehicle-telematics-stream-analytics-job-publishes-to-data-mart.png)

*Ábra 34 – analytics feladat közzé, adatok intelligens adatfolyam*

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig35-vehicle-telematics-data-mart-setting-in-stream-analytics-job.png)

*Ábra 35 – adatok intelligens adatfolyam analytics-projekt beállítása*


## <a name="consume"></a>Felhasználása

A Power BI gazdag irányítópult ad a megoldás valós idejű adatok és a prediktív analytics megjelenítések. 

A PowerBI jelentéseket és az irányítópult beállításával kapcsolatos részletes útmutatásért kattintson ide. Az utolsó irányítópult néz ki:

![](./media/cortana-analytics-playbook-vehicle-telemetry-deep-dive/fig36-vehicle-telematics-powerbi-dashboard.png)

*Ábra 36 - PowerBI irányítópult*

## <a name="summary"></a>Összefoglalás

A dokumentum tartalmaz egy részletes Lehatolás a gépjármű Telemetriai Analytics megoldás. Ez a lambda architektúra mintájának bővíthető valós idejű és az előrejelzések és műveletek analytics köteg. Használati eset igénylő meleg elérési utat (valós időben) a széles köre vonatkozik a minta és az elemzést hideg elérési utat (köteg). 
