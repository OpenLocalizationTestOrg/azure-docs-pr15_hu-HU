<properties 
    pageTitle="Gépi tanulási alkalmazás: rendellenességet észlelési szolgáltatás |} Microsoft Azure" 
    description="Rendellenességet észlelési API képen épülnek fel a Microsoft Azure gépi tanulási rendellenességeinek észleli az idő adatsor vannak egyenletesen sorközt szeretne használni az idő numerikus értékű." 
    services="machine-learning" 
    documentationCenter="" 
    authors="alokkirpal" 
    manager="jhubbard"
    editor="cgronlun" /> 

<tags 
    ms.service="machine-learning" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="multiple" 
    ms.date="10/11/2016" 
    ms.author="alokkirpal"/>


# <a name="machine-learning-anomaly-detection-service"></a>Gépi tanulási rendellenességet észlelési szolgáltatás#

##<a name="overview"></a>– Áttekintés

[Rendellenességet észlelési API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) Azure gépi tanulási rendellenességeinek észleli az idő adatsor vannak egyenletesen sorközt szeretne használni az idő numerikus értékű, a következővel képen. 

Ez az API észleli az alábbi típusú anomalous mintázatok az idő adatsor:

* **Pozitív és negatív trendek**: például amikor egy növekvő tendenciát számítások memóriahasználat figyelése esetleg nem lesz a fontos, lehet, hogy a fejlesztő, tájékoztató

* **A dinamikus tartományban található értékek módosítások**: például figyelése a kivételek által egy felhőalapú szolgáltatásba, amikor a dinamikus tartományban található értékek módosításokat jelezheti a szolgáltatás állapotát jelző instabilitása és

* **Napra és esik**: például figyelése a bejelentkezési hibák a szolgáltatás vagy egy e-kereskedelmi webhelyen elvetése számát, amikor kiugrásainak megfelelő vagy immerzióban jelezheti szokatlan viselkedést.

Ezek gépi tanulási benne változások nyomon követése ilyen értékeket az értékeket az időt és a jelentés folyamatban lévő módosítások fölé rendellenességet értékek szerint. Alkalmi küszöb finombeállítása nincs szükségük, és az eredmények használható hamis pozitív ráta szabályozhatja. API akkor lehet hasznos több helyzetekben szolgáltatás figyelését nyomon követésével KPI időbeli, például rendellenességet kimutatását időbeli – például keresések számát, kattintással teljesítményét figyelve keresztül memória, Processzor, például számláló fájl beolvassa, stb számú mértékek figyelése használatát.

Első lépések hasznos eszközök megtalálható a rendellenességet észlelési ajánlja fel. 

* A [webes alkalmazás](http://anomalydetection-aml.azurewebsites.net/) segítségével értékeli, és jelenítse meg az eredményeket az rendellenességet észlelési API-khoz az adatokon. 

* A [példakódot](http://adresultparser.codeplex.com/) programozás útján az API érhetik el és elemezni az eredmények a C# mutatja.

>[AZURE.NOTE]
>Próbálja meg [Ez az API](https://datamarket.azure.com/dataset/aml_labs/anomalydetection) hajtott **Informatikai rendellenességet háttérismeretek megoldás**
>
>A végpont megoldás Azure-előfizetéséhez <a href="https://gallery.cortanaintelligence.com/Solution/Anomaly-Detection-Pre-Configured-Solution-1" target="_blank">rendszerbe megszerezni **Ez a kiindulópont >**</a>


##<a name="api-definition"></a>API meghatározása

A szolgáltatás a többi-alapú API segítségével többek között a webhelyen vagy mobilalkalmazás, R, a Python, az Excel programban különböző módokon felhasználható HTTPS stb.  Az idő adatsorok keresztül REST API-hívás kezdeményezése a szolgáltatás elküldi, és a fent ismertetett típusok három rendellenességet kombinációi fusson. A szolgáltatás, amely zökkenőmentes méretezze át a vállalat igényeinek, és itt SLA Azure gépi tanulási platformon fut.

###<a name="headers"></a>Élőfejek
Győződjön meg róla, a megfelelő fejlécek szerepeltetni a kérelmet, amelyet a következő lesz:

    Authorization: Basic <creds>
    Accept: application/json
               
    Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

A fiókkulcs a fiókjából az [Azure Datamarket](https://datamarket.azure.com/account/keys)a található. 

###<a name="score-api"></a>Pontszámhoz API

Az eredmény API rendellenességet észlelési futó nem szezonális idő adatsor szolgál. Az API rendellenességet benne számos futtatja az adatokat, és azok rendellenességet értékek adja eredményül. Az alábbi ábrán egy példát a pontszámhoz API észlelését rendellenességeinek. Ez az idősorok 2 különböző webhelyszintű módosításokat végezhet, és a 3 kiugrásainak megfelelő tartalmaz. A piros pontra, ahol szintjének módosítása észlelt, miközben a fekete pontot megjelenítése az észlelt kiugrásainak megfelelő idő megjelenítése.

![Pontszámhoz API][1]
    
**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Példa összehívás törzsében**

Az alábbi összehívásban elküldjük Önnek egy idősorok (látható csonkolt) adatpontok 3/1/2016 – 3/10/2016-ból és a nyársra benne határozza meg az alábbi adatpontok esetén anomalous észlelését szabályzó API-nak. 

    {
      "data": [
        [ "9/21/2014 11:05:00 AM", "3" ],
        [ "9/21/2014 11:10:00 AM", "9.09" ],
        [ "9/21/2014 11:15:00 AM", "0" ]
      ],
      "params": {
        "tspikedetector.sensitivity": "3",
        "zspikedetector.sensitivity": "3",
        "trenddetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "postprocess.tailRows": "2"
      }
    }

A válasz, a következő lesz: 

    {
      "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
      "ADOutput":"{
        "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
        "ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
        "Values":[
          ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
          ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
        ]
      }"
    }

###<a name="scorewithseasonality-api"></a>ScoreWithSeasonality API

A ScoreWithSeasonality API rendellenességet észlelési futó szezonális mintázatok rendelkező idősorok szolgál. Ez az API akkor lehet hasznos eltérések feltárása a szezonális mintázatok.  

Az alábbi ábrán látható példa szezonális idő sorozat észlelt rendellenességeket. Az idősorok tartalmaz egy Nyárs (1st fekete pont), (2. a fekete pontozott és egy végén) két immerzióban és egy szintjének módosítása (vörös pont). Ne feledje, hogy mindkét közepén az idősorok és szintjének módosítása a dip csak discernable szezonális összetevőket a sorozatból eltávolítása után.

![A szezonalitás API][2]

**URL-CÍME**

    https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Példa összehívás törzsében**

Az alábbi összehívásban elküldjük Önnek a idősorok (látható csonkolt) adatpontok 3/1/2016 – 3/10/2016-ból és a paraméterek az alábbi adatpontok esetén anomalous észlelését szabályzó API-nak. 

    {
      "data": [
        [ "9/21/2014 11:10:00 AM", "9" ],
        [ "9/21/2014 11:15:00 AM", "12" ],
        [ "9/21/2014 11:20:00 AM", "10" ]
      ],
      "params": {
        "preprocess.aggregationInterval": "0",
        "preprocess.aggregationFunc": "mean",
        "preprocess.replaceMissing": "lkv",
        "postprocess.tailRows": "1",
        "zspikedetector.sensitivity": "3",
        "tspikedetector.sensitivity": "3",
        "upleveldetector.sensitivity": "3.25",
        "bileveldetector.sensitivity": "3.25",
        "trenddetector.sensitivity": "3.25",
        "detectors.historywindow": "500",
        "seasonality.enable": "true",
        "seasonality.transform": "deseason",
        "seasonality.numSeasonality": "1"
      }
    }

A válasz, a következő lesz: 

    {
        "odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
        "ADOutput": "{
            "ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
            "ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
            "Values":[
                ["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
            ]
        }"
    }

###<a name="detectors"></a>Benne

A rendellenességet észlelési API benne 3 fő kategóriába támogatja. Adott bemeneti paramétereket és a kimeneti értékeket az egyes jelentő tájékoztatást a következő táblázatban található.

|Jelentő kategória|Jelentő|Leírás|Bemeneti paramétereket|Kimeneti értékeket
|---|---|---|---|---|
|Nyárs benne|TSpike jelentő|Észleli, hogy az első és harmadik quartiles vannak kiugrásainak megfelelő és immerzióban, amennyiben az értékek alapján|*tspikedetector.sensitivity:* megnyitja az egész szám értéke a tartományban található 1 – 10 alapértelmezett: 3; A magasabb értékek fog elfog további szélső értékeket, így téve kevésbé érzékeny|TSpike: bináris értékek – "1", ha a Nyárs/dip lép fel, különben a program "0"|
||ZSpike jelentő|Kiugrásainak megfelelő és alapulnak, hogy milyen távol a datapoints áll átlaguktól immerzióban feltárása|*zspikedetector.sensitivity:* egész szám értéke a tartományban található 1 – 10 alapértelmezett készítése: 3; A magasabb értékek fog elfog további szélső értékeket, így kevésbé érzékeny|ZSpike: bináris értékek – "1", ha a Nyárs/dip lép fel, különben a program "0"|
|Lassú Trend jelentő|Lassú Trend jelentő|A set-magánjellegű szerinti lassú pozitív trend feltárása|*trenddetector.sensitivity:* küszöb jelentő pontszámhoz a (alapértelmezett: 3,25, 3,25 – 5 olyan cellatartomány, amely lehetővé teszi jelölje be ezt a; Minél nagyobb a kisebb érzékeny)|TScore: lebegőpontos szám, amely a trend rendellenességet pontszámhoz|
|Benne szintjének módosítása|Egyirányú szintjének módosítása jelentő|Felfelé szint észleli a set-magánjellegű szerinti módosítása|*upleveldetector.sensitivity:* küszöb jelentő pontszámhoz a (alapértelmezett: 3,25, 3,25 – 5 olyan cellatartomány, amely lehetővé teszi jelölje be ezt a; Minél nagyobb a kisebb érzékeny)|PScore: lebegő rendellenességet pontszámhoz felfelé a jelző szám szint módosítása|
||Kétirányú szintjének módosítása jelentő|A set-magánjellegű szerinti felfelé és lefelé is szintű módosítása feltárása|*bileveldetector.sensitivity:* küszöb jelentő pontszámhoz a (alapértelmezett: 3,25, 3,25 – 5 olyan cellatartomány, amely lehetővé teszi jelölje be ezt a; Minél nagyobb a kisebb érzékeny)|RPScore: lebegőpontos szám képviselő rendellenességet pontszámhoz a felfelé és lefelé szint módosítása

###<a name="parameters"></a>Paraméterek

Ezeket a bemeneti paramétereket kapcsolatban további részletes tudnivalókat az alábbi táblázatban szereplő:

|Bemeneti paramétereket|Leírás|Alapértelmezett beállítás|Típus|Érvényes tartományon|Javasolt tartomány|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Összesítési intervallum (másodpercben) értékek bemeneti idősorok|0 (nem összesítést van végrehajtani)|egész szám|0: összesítés > 0 egyébként kihagyása|1 nap, idősort – függő 5 percig tart
|preprocess.aggregationFunc|A megadott AggregationInterval az adatok összesítése használt függvény|középérték|felsorolásos|középérték, SZUM, hossza|A #HIÁNYZIK|
|preprocess.replaceMissing|Hiányzó adatok imputálására értékek|LKV (a legutolsó érték)|felsorolásos|nulla, lkv, középérték|A #HIÁNYZIK|
|detectors.historyWindow|Rendellenességet pontszámhoz kiszámítása használható előzményeket (az adatpontok száma)|500|egész szám|10-2000|Függő idősorok|
|upleveldetector.Sensitivity|Magánjellegű felfelé szint jelentő módosítása. |a 3,25|dupla|Nincs lehetőség|A 3,25-5(Lesser values mean more sensitive)|
|bileveldetector.Sensitivity|Magánjellegű kétirányú szint jelentő módosítása. |a 3,25|dupla|Nincs lehetőség|A 3,25-5(Lesser values mean more sensitive)|
|trenddetector.Sensitivity|Pozitív trend jelentő megkülönböztetése. |a 3,25|dupla|Nincs lehetőség|A 3,25-5(Lesser values mean more sensitive)|
|tspikedetector.Sensitivity |TSpike jelentő megkülönböztetése|3|egész szám|1 – 10|3 – 5(Lesser values mean more sensitive)|
|zspikedetector.Sensitivity |ZSpike jelentő megkülönböztetése|3|egész szám|1 – 10 |3 – 5(Lesser values mean more sensitive)|
|seasonality.enable|Hogy szezonalitás analysis van-e elvégzendő|Igaz|logikai érték|IGAZ, hamis|Függő idősorok|
|seasonality.numSeasonality |Az általuk észlelt periodikus ciklus maximális száma|1|egész szám|1, 2|1-2|
|seasonality.Transform |Hogy szezonális (és) trend összetevők el kell távolítani a rendellenességet észlelési alkalmazása előtt|deseason|felsorolásos|nincs, deseason, deseasontrend|A #HIÁNYZIK|
|postprocess.tailRows |A kimenet eredmények megmaradjanak a legújabb adatpontok száma|0|egész szám|a 0 (minden adatpontjánál tartani), vagy adjon meg kell figyelni az eredmények pontok száma|A #HIÁNYZIK|

###<a name="output"></a>Kimenet
Az API összes benne az alábbiakon futtatható az idő adatsorok és rendellenességet értékek és a bináris Nyárs jelölők az egyes szakaszokban időt adja eredményül. Az alábbi táblázat felsorolja az API-t a kimeneti értékeket. 

|Kimeneti értékeket|Leírás|
|---|---|
|Idő|A nyers vagy adatok összesített (és/vagy) imputált időbélyegeket Ha összesítési (és/vagy) hiányzó adatok imputálási érvényesül|
|OriginalData|Értékek nyers adatokból vagy összesített (és/vagy) imputált adatok, ha összesítési (és/vagy) hiányzó adatok imputálási érvényesül|
|ProcessedData|Az alábbiak egyikét: <ul><li>Idősorok szezonálisan korrigált, ha jelentős szezonalitás észlelt és deseason lehetőség kiválasztva;</li><li>szezonálisan igazítani és detrended idősort, ha a jelentős szezonalitás észlelt és deseasontrend lehetőség kiválasztva</li><li>egyéb esetben ez ugyanaz, mint OriginalData</li>|
|TSpike|Bináris jelölő jelzi, hogy TSpike jelentő észleli a Nyárs|
|ZSpike|Bináris jelölő jelzi, hogy ZSpike jelentő észleli a Nyárs|
|Pscore|Felfelé szintjén rendellenességet pontszámhoz jelző lebegőpontos szám módosítása|
|Palert|1 és 0 értéket, jelezve, hogy van egy felfelé szintet a bemeneti magánjellegű alapján rendellenességet módosítása|
|RPScore|A rögzítetlen számot képviselő rendellenességet pontszám a kétirányú szintjének módosítása|
|RPAlert|jelző kétirányú szintű 1 és 0 értéket a bemeneti magánjellegű alapján rendellenességet módosítása|
|TScore|A pozitív trend pontszám egy lebegőpontos szám képviselő rendellenességet|
|TAlert|1 és 0 van feltüntetve értéke egy szám pozitív trend rendellenességet a bemeneti magánjellegű alapján|


A kimenet egy [egyszerű elemző](https://adresultparser.codeplex.com/) elemezhető –, amely bemutatja, hogyan csatlakozhat az API-nak, és a kimeneti elemezni példakódot rendelkezik. Észlelt rendellenességeket lehet egy irányítópulton formájában jelenik meg, illetve átadni korrekciós műveletek emberi szakértői vagy beépül a Feladatkezelő rendszer.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 
