<properties
    pageTitle="Irányítópult a Power BI Értékáram-elemzés a |} Microsoft Azure"
    description="A valós idejű adatfolyam Power BI-Irányítópult segítségével üzleti intelligenciával kapcsolatos funkciók összegyűjtése és Értékáram-elemzés feladat nagy mennyiségű adatainak elemzése."
    keywords="statisztika irányítópultján, valós idejű irányítópult"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Adatfolyam-elemzés és a Power BI: valós idejű analytics irányítópult-adatfolyam

Azure Értékáram-elemzés lehetővé teszi, hogy az egyik a vezető üzletiintelligencia-eszközeinek, a Microsoft Power BI előnyeit. Megtudhatja, hogy miként Azure Értékáram-elemzés segítségével nagy mennyiségű adatfolyam adatok és a valós idejű Power BI-statisztika irányítópultján a betekintést get elemezheti.

[Microsoft Power BI](https://powerbi.com/) segítségével gyorsan élő Irányítópult összeállítása. [Nézzen meg egy videót, mely bemutatja az alkalmazási példát](https://www.youtube.com/watch?v=SGUpT-a99MA).

Ebben a cikkben megtudhatja, hogyan létrehozása a saját egyéni Üzletiintelligencia-eszközök használatával a Power BI-kimenetként az Azure Értékáram-elemzés feladatokhoz csatlakozást és a valós idejű irányítópultok.

## <a name="prerequisites"></a>Előfeltételek

* Microsoft Azure-fiók
* A beviteli adatfolyam adatainak használhatnak, a megjelenítő Értékáram-elemzés feladathoz. Értékáram-elemzés fogadja el a beviteli Azure esemény hubok vagy Azure Blob-tárolóból.  
* A Power BI munkahelyi vagy iskolai fiók

## <a name="create-azure-stream-analytics-job"></a>Értékáram-elemzés Azure-feladat létrehozása

Az [Azure klasszikus portálon](https://manage.windowsazure.com)kattintson a **új Data Services, Értékáram-elemzés, gyors létrehozásához**.

Adjon meg az alábbi értékeket, majd kattintson a **Értékáram-elemzés létrehozása feladat**:

* A **Projekt neve** - írja be a feladat nevét. Ha például **DeviceTemperatures**.
* **Régió** - jelölje ki a régió, amelyre a feladat található. Fontolja meg úgy, hogy a feladatot, és az esemény-központban optimális teljesítménye érdekében és adatátvitel költségei területek között felmerülése elkerülése ugyanazon régióban.
* **Tárterület-fiókot** – válassza ki a kívánt fut ez régión belüli összes Értékáram-elemzés feladatok nyomon adatok tárolására tároló-fiókot. Ha a válasszon ki egy meglévő tárterület-fiókot vagy hozzon létre egy újat.

**Értékáram-elemzés** kattintson a bal oldali ablaktáblában a Értékáram-elemzés feladatok listáját.

![graphic1][graphic1]

> [AZURE.TIP] Az új feladat látható állapotban **Nem kezdődött el**. Figyelje meg, hogy a lap alján kattintson a **Start** gomb nem érhető el. Az elvárt működés, meg kell adnia a feladat beviteli, a kimeneti, a lekérdezést, és így tovább előtt: a feladatot.

## <a name="specify-job-input"></a>Adja meg a projekt beviteli

Ebben az oktatóanyagban azt is feltételezve, esemény központi egy bemeneteként JSON szerializálási és UTF-8 kódolást használja.

* Kattintson a projekt nevére.
* Kattintson a **bemeneti adatok alapján** , a lap tetején, és kattintson a **Beviteli hozzáadása**. A megnyíló párbeszédpanelen azt ismerteti, hogy állítsa be a bemeneti számos keresztül.
*   Jelölje ki a **adatfolyam**, és kattintson a jobb oldali gombra.
*   Jelölje ki az **Esemény központi**, és kattintson a jobb oldali gombra.
*   Írja be vagy válassza ki az alábbi értékeket, a harmadik lapon:
  * **Beviteli Alias** - adjon meg egy rövid nevet, a bemeneti projektre vonatkozóan. Figyelje meg, hogy fogja használni, akkor ezt a nevet a lekérdezésben később.
  * **Esemény központi** – Ha az esemény-központban létrehozott ugyanabban az előfizetésben, a megjelenítő Értékáram-elemzés feladat, jelölje be az esemény-központban névtér.
*   Ha az esemény központi másik előfizetést, jelölje be a **Használati események központi egy másik előfizetésből** , és kézzel írja be a **Szolgáltatás Bus Namespace**, **Esemény központi neve**, **Esemény központi házirend neve**, **Esemény központi házirend kulcs**és **Események központi partíciót száma**adatait.

> [AZURE.NOTE]  Ez a példa használja az alapértelmezett szám partíciók, melyiket érdemes 16.

* **Esemény központi neve** - válassza az Azure-esemény van központban nevét.
* **Esemény központi házirend neve** - jelölje ki az esemény központi házirendet a használja az esemény-központban. Győződjön meg arról, hogy az ezzel a házirenddel az engedélyek kezelése.
*   **Esemény központi fogyasztói csoport** – hagyja üresen ezt, vagy adjon meg egy fogyasztói csoportot, ha rendelkezik a a esemény-központját. Ne feledje, hogy minden egyes egy esemény hubhoz fogyasztói csoportját is csak 5 olvasók egyszerre. Igen döntse el, a feladatok a jobb oldali fogyasztói csoport lehetőséget. Ha üresen hagyja a mezőt, akkor használja az alapértelmezett fogyasztói csoportot.

*   Kattintson a jobb gombbal.
*   Adja meg az alábbi értékeket:
  * **Esemény szerializáló formátum** - JSON
  * **Kódolást** - UTF8
*   Kattintson az ellenőrzés gombra a forrás hozzáadása, és ellenőrizze, hogy Értékáram-elemzés sikeresen csatlakozhat az esemény-központban.

## <a name="add-power-bi-output"></a>A Power BI kimeneti hozzáadása

1.  Kattintson a **kimeneti** a lap tetején, és kattintson a **Kimeneti hozzáadása**. A Power BI-kimeneti beállítások állapotúként jelenik meg.

    ![graphic2][graphic2]  

2.  Jelölje ki a **Power BI** , és kattintson a jobb oldali gombra.
3.  A képernyő, az alábbihoz hasonló jelennek meg:

    ![graphic3][graphic3]  

4.  Ebben a lépésben adja meg a megjelenítő Értékáram-elemzés feladat kimeneti egy munkahelyi vagy iskolai fiókjával. Ha már a Power BI-fiókja, válassza a **Most már engedélyezni**. Ha nem, válassza a **regisztráció**. [Az alábbiakban jó blog útmutató a Power BI feliratkozás részleteit alapján](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Ezután jelennek meg a képernyőn, például a következőket:

    ![graphic4][graphic4]  

Adja meg a következő értékeket:

* **Kimeneti Alias** – elhelyezhet bármely kimeneti alias, amely rendszerről hivatkozik. A kimenet alias különösen akkor hasznos, ha úgy dönt, hogy a feladat több kimeneti értékeket. Ebben az esetben akkor ezt a lekérdezés kimenő hivatkozik. Ha például használata a kimeneti alias érték = "OutPbi".
* **Adatkészlet neve** - adja meg, amelyet a Power BI kimeneti, hogy az adatkészlet nevét. Ha például használata "pbidemo".
*   **Táblázat neve** - adja meg a kívánt tábla neve alatt az adatkészlet a Power BI kimenet. Tegyük fel, hogy a "pbidemo" hívja meg. Jelenleg Értékáram-elemzés feladatok Power BI kimenetét csak lehet egy táblából egy adatkészletet.
*   **Munkaterület** – jelölje ki a Power BI bérlői webhely, amely alatt az adatkészlet létrejön a munkaterület.

>   [AZURE.NOTE] Meg kell nem kifejezetten létrehozni a adatkészlet és a tábla a Power BI-fiókjában. Azok automatikusan létrejön a Értékáram-elemzés feladatok kezdés és a feladat szivattyúzó kimeneti indul el, a Power BI. A feladat lekérdezés nem ad vissza találatokat, ha az adathalmazban és a táblázat nem létrejön.

*   Kattintson **az OK**gombra **A kapcsolat tesztelése** gombra, és most kimeneti konfigurációjának importálása befejeződött.

>   [AZURE.WARNING] Ügyeljen arra is, hogy a Power BI már egy adatkészlet és neve megegyezik egy, a megjelenítő Értékáram-elemzés feladat a megadott táblázat, ha a meglévő adatok felülíródnak.


## <a name="write-query"></a>Lekérdezés írása

Nyissa meg a **lekérdezés** lap a feladat. Írja be a lekérdezés, amelynek kimenete szeretné a Power BI szolgáltatásban. Például ez lehet például a következő SQL-lekérdezés:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Indítsa el a feladatok. Ellenőrizze, hogy az esemény központi események fogad, és a lekérdezés a várt eredményt hoz létre. Ha a lekérdezés exportálja a sorok 0, a Power BI adatkészlet és táblák nem automatikusan létrejön.

## <a name="create-the-dashboard-in-power-bi"></a>Az irányítópult létrehozása a Power BI szolgáltatásban

Nyissa meg a [Powerbi.com](https://powerbi.com) , és jelentkezzen be a munkahelyi vagy iskolai fiókjával. Értékáram-elemzés feladat lekérdezés exportálja az eredményt, ha jelennek meg az adatkészlet már létezik:

![graphic5][graphic5]

Az irányítópult létrehozására, nyissa meg a irányítópultok lehetőséget, és hozzon létre egy új irányítópult.

![graphic6][graphic6]

Ez a példa azt fogja, hogy címkét "Bemutató irányítópult" azt.

Most kattintson a az adatkészlet hozta létre a Értékáram-elemzés feladatok (a példában szereplőhöz aktuális pbidemo). Ekkor megnyílik egy oldalra ez adatkészlet fölött diagram létrehozása. Íme egy példa a jelentéseket hozhat létre, de:

Jelölje ki a ∑ temp, és az idő mezőket. Fog automatikusan felkeresik érték és a tengely a diagramhoz:

![graphic7][graphic7]

Az adatokkal akkor kap egy diagramon az alábbi:

![graphic8][graphic8]

Az érték csoportban kattintson a legördülő lefelé temp az, és válassza az **átlag** a hőmérsékleti, és kattintson a diagramon, kattintson a **Megjelenítés** , és válassza a **Vonaldiagram**:

![graphic9][graphic9]

Most fog kapni átlag vonaldiagram idővel.  Az alábbi a PIN-kód lehetőséget a használja, akkor rögzítheti ezt a korábban létrehozott irányítópult:

![graphic10][graphic10]

Most már az irányítópult kiemelt jelentés megtekintésekor ekkor megjelenik a jelentés frissítésével valós időben. Próbálja meg az adatokat az eseményeket – Nyárs temp vagy valami hasonlóan módosítása, és megjelenik az irányítópult megjelenjen a valós idejű hatása.

Megjegyzés, hogy ebben az oktatóanyagban igazolni hogyan hozhat létre, de az egyik típusú diagram adatkészletet. A Power BI segíthetnek létrehozása más felhasználói Üzletiintelligencia-eszközök a szervezet számára. Lássunk még egy példát a Power BI-irányítópult nézze meg a [Power BI – első lépések](https://youtu.be/L-Z_6P56aas?t=1m58s) videót.

További információ a Power BI kimeneti beállítása, és használja a Power BI-csoportok olvassa el a [Power BI szakasz](stream-analytics-define-outputs.md#power-bi) [ismertetése Értékáram-elemzés kimenet](stream-analytics-define-outputs.md "ismertetése Értékáram-elemzés exportálja"). Ha többet szeretne tudni az irányítópultok létrehozása a Power BI egy másik hasznos erőforrás [irányítópultok a Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Korlátozások és a gyakorlati tanácsok

Power BI alkalmaz a párhuzamos és az átvitel kényszerek, itt leírt módon:(https://powerbi.microsoft.com/pricing "Power BI árak") [https://powerbi.microsoft.com/pricing]

Miatt a Power BI hajtanak végre magát leginkább természetesen azokra az esetekre, ahol Azure Értékáram-elemzés tartalmaz-e jelentős adatok betöltése csökkentése.
Azt javasoljuk, hogy TumblingWindow vagy HoppingWindow segítségével győződjön meg arról, hogy adatokat leküldéses lenne legfeljebb 1 leküldéses/második és, hogy a lekérdezés hajtanak végre az átviteli követelmények belül – a ablak ad a másodpercben megadott érték kiszámítása a következő egyenlet alapján is használhatja:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Példaként – Ha adatok küldése minden második 1000 eszközzel rendelkezik, a Power BI Pro Termékváltozat 1,000,000 sorok/óra támogató dolgozik, és szeretne egy eszközt átlagos adatok beolvasása a Power BI végezheti el kell nagyobb leküldéses eszköz / 4 másodpercenként (mint alább):  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Ami azt jelenti, hogy változna az eredeti lekérdezést:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI nézetének frissítése

A gyakori kérdésekre adott "Miért nem az irányítópulton automatikusan frissíti a PowerBI?".

Cél, PowerBI a csatlakozást a kérdések és válaszok kérő egy kérdést, például "A maximális érték szerint temp időbélyeg esetén ma" és az irányítópult mozaik rögzíteni.

### <a name="renew-authorization"></a>Engedély megújítása

Szüksége lesz, ha a jelszó megváltozott a feladatok létrehozásának vagy az utolsó hitelesített újból hitelesíteni a Power BI-fiókját. Ha többtényezős hitelesítés (MFA) be van állítva az Azure Active Directory (AAD) bérlőjén is szüksége lesz a Power BI engedélyezési minden 2 hétből megújítása. A jelenség, a probléma nem feladat kibocsátás és a "hitelesítés felhasználói hiba" a művelet naplókban:

![graphic12][graphic12]

Hasonlóképpen ha a feladat megkísérli indítása, miközben a token lejárt, hiba lép fel, és a projekt kezdési sikertelen lesz. A hiba lesz valami hasonló alatt:

![PowerBI adatérvényesítési hiba](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

A probléma megoldásához a futó feladat leállítása, és nyissa meg a Power BI kimeneti.  A "Megújítása engedélyezési" hivatkozásra, és indítsa újra a feladat leállítása utoljára adatvesztés elkerülése érdekében.

![Megújítás a PowerBI érvényesítése](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Miután az engedélyezés frissítése a Power BI egy zöld riasztás engedélyezési területén jelennek meg:

![Megújítás a PowerBI érvényesítése](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
