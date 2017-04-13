<properties
    pageTitle="Egy IoT megoldás készíthet Értékáram-elemzés használatával |} Microsoft Azure"
    description="Első lépések oktatóprogram őrbódét példa, a megjelenítő Értékáram-elemzés IoT megoldás"
    keywords="megoldás IOT ablak függvények"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>

# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Egy IoT megoldás készíthet Értékáram-elemzés használatával

## <a name="introduction"></a>– Bevezetés

Ebből az oktatóanyagból megtanulhatja az Azure Értékáram-elemzés használata az adatok beolvasása a valós idejű az összefüggéseket. A fejlesztők egyszerűen kombinálhatja az adatokat, például kattintson-adatfolyam megjelenítését, naplók és eszköz által létrehozott események adatfolyamok korábbi rekordok vagy vállalati háttérismeretek forrásául hivatkozási adatok. Szolgáltatásként teljes körű felügyelt, valós idejű adatfolyam kiszámítása a Microsoft Azure-ban tárolt a Azure Értékáram-elemzés beépített tűrőképessége, alacsony időtartama és méretezhetőség másolatot, és percben futó biztosít.

Ebben az oktatóanyagban befejeztével lesz képes:

-   Megismerkedhet az Azure Értékáram-elemzés portálon.
-   Állítsa be, és a továbbított feladat üzembe helyezése
-   Adjuk a való életben problémákat, és megoldása őket a Értékáram-elemzés lekérdezésnyelv használatával.
-   Fejleszthet olyan adatfolyam-megoldások a vevők biztonságos Értékáram-elemzés használatával.
-   A figyelhető és naplózható élmény használatával kapcsolatos problémák megoldása.

## <a name="prerequisites"></a>Előfeltételek

Az alábbi előfeltételek oktatóprogram elvégzéséhez a következők szükségesek:

-   Az [Azure PowerShell](../powershell-install-configure.md) legújabb verzióját.
-   Visual Studio 2015 vagy az ingyenes [Visual Studio-Közösség](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
-   Az [Azure előfizetés](https://azure.microsoft.com/pricing/free-trial/)
-   Rendszergazdai jogosultságokkal a számítógépen
-   Töltse le a [TollApp.zip](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) a Microsoft letöltőközpontból.
-   Nem kötelező: A TollApp esemény nyilvántartás-készítő alkalmazás a [GitHub](https://aka.ms/azure-stream-analytics-toll-source) forráskódjának

## <a name="scenario-introduction-hello-toll"></a>Eset bemutatása: "Helló, díjköteles!"


Díjköteles állomáson olyan közös jelenség. Fel, amelyekkel őket a sok gyorsforgalmi hidakat és alagutak világszerte. Minden díjköteles állomás még több díjköteles fülkéit foglalja magában. A kézi fülkéit foglalja magában le kell fizetnie egy attendant a nem ingyenes. Automatikus fülkéit foglalja magában: minden stand fölött érzékelő beolvasása egy RFID kártya, amely szerint adja meg a díjköteles stand a szélvédőkeret, a gépjármű dobozának. Nagyon egyszerűen egy esemény adatfolyamként eredményhalmaz, amelyen a érdekes műveleteket végezheti el ezeket a díjköteles állomásokon keresztül járműveket szakasza ábrázolásához.

![Képet autókat díjköteles fülkéit foglalja magában:](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Bejövő adatok

Ebben az oktatóanyagban működik-e az adatok két adatfolyam megjelenítését. Telepítve van a megjelenési és eltűnési díjköteles állomások érzékelők kiszámítására az első adatfolyam. A második adatfolyam egy statikus keresési adatkészlet gépjármű regisztrációs adatokat tartalmazó értéke.

### <a name="entry-data-stream"></a>Bejegyzés adatfolyam

A bejegyzés adatfolyam díjköteles állomások belépnének autók információkat tartalmazza.


| TollID | EntryTime               | LicensePlate | Állam | Helyezése   | Modell   | VehicleType | VehicleWeight | Díjköteles | Címke       |
|--------|-------------------------|--------------|-------|--------|---------|-------------|---------------|------|-----------|
| 1      | 2014-09-10 12:01:00.000 | JNB 7001     | NY    | Honda  | CRV     | 1           | 0             | 7    |           |
| 1      | 2014-09-10 12:02:00.000 | YXZ 1001     | NY    | Toyota | Camry   | 1           | 0             | 4    | 123456789 |
| 3      | 2014-09-10 12:02:00.000 | ABC 1004     | CT    | Ford   | Taurus  | 1           | 0             | 5    | 456789123 |
| 2      | 2014-09-10 12:03:00.000 | XYZ 1003     | CT    | Toyota | Corolla | 1           | 0             | 4    |           |
| 1      | 2014-09-10 12:03:00.000 | BNJ 1007     | NY    | Honda  | CRV     | 1           | 0             | 5    | 789123456 |
| 2      | 2014-09-10 12:05:00.000 | CDE 1007     | NJ    | Toyota | 4 x 4     | 1           | 0             | 6    | 321987654 |


Az alábbiakban az oszlopok rövid leírása:

| Oszlop | Leírás |
|--------------|--------------------------------------------------------------------|
| TollID       | A díjköteles stand ID azonosítója, a díjköteles stand egyedi módon azonosító            |
| EntryTime    | Dátum és idő a díjköteles stand UTC szerint megadva a gépjármű-bejegyzés |
| LicensePlate | A gépjármű licenc lemez száma                            |
| Állam        | Amerikai Egyesült Államokban állam                                           |
| Helyezése         | A autót gyártója                                 |
| Modell        | A autót modell száma                                 |
| VehicleType  | Bármelyik személy járműveket vagy kereskedelmi járműveket 2 1       |
| WeightType   | Gépjármű tömeg tonna; személy járműveket 0                   |
| Díjköteles         | A díjköteles érték USD                                              |
| Címke          | A fizetés; automatizáló autót az e-címke üres, amennyiben a fizetési végezték manuálisan |


### <a name="exit-data-stream"></a>Kilépés adatfolyam

A Kilépés adatfolyam elhagyása a díjköteles állomás autókat információkat tartalmazza.


| **TollId** | **ExitTime**                 | **LicensePlate** |
|------------|------------------------------|------------------|
| 1          | 2014-09-10T12:03:00.0000000Z | JNB 7001         |
| 1          | 2014-09-10T12:03:00.0000000Z | YXZ 1001         |
| 3          | 2014-09-10T12:04:00.0000000Z | ABC 1004         |
| 2          | 2014-09-10T12:07:00.0000000Z | XYZ 1003         |
| 1          | 2014-09-10T12:08:00.0000000Z | BNJ 1007         |
| 2          | 2014-09-10T12:07:00.0000000Z | CDE 1007         |

Az alábbiakban az oszlopok rövid leírása:


| Oszlop | Leírás |
|--------------|-----------------------------------------------------------------|
| TollID       | A díjköteles stand ID azonosítója, a díjköteles stand egyedi módon azonosító         |
| ExitTime     | Dátum és idő a gépjármű elhagyásakor díjköteles stand UTC szerint megadva |
| LicensePlate | A gépjármű licenc lemez száma                         |

### <a name="commercial-vehicle-registration-data"></a>Kereskedelmi gépjármű regisztrációs adatok

Az oktatóprogram kereskedelmi gépjármű regisztrációs adatbázis statikus pillanatképét.


| LicensePlate | RegistrationId | Lejárt |
|--------------|----------------|---------|
| SVT 6023     | 285429838      | 1       |
| XLZ 3463     | 362715656      | 0       |
| 1005 ÁLLAPOTDÁTUMHOZ     | 876133137      | 1       |
| RIV 8632     | 992711956      | 0       |
| SNY 7188     | 592133890      | 0       |
| ELH 9896     | 678427724      | 1       |

Az alábbiakban az oszlopok rövid leírása:


| Oszlop | Leírás |
|--------------|-----------------------------------------------------------------|
| LicensePlate | A gépjármű licenc lemez száma                         |
| RegistrationId     | A gépjármű regisztrációs azonosító |
| Lejárt | A gépjármű regisztrációs állapotát: Ha gépjármű regisztrációs aktív, 0 1, ha a regisztrációs lejárt                 |


## <a name="set-up-the-environment-for-azure-stream-analytics"></a>Értékáram-elemzés Azure-környezet beállítása

Ebben az oktatóanyagban befejezéséhez szüksége van Microsoft Azure előfizetést. A Microsoft ingyenes próbaverziót Microsoft Azure-szolgáltatások kínál.

Ha nem rendelkezik az Azure-fiók, [kérelem ingyenes próbaverziót](http://azure.microsoft.com/pricing/free-trial/)is.

> [AZURE.NOTE] Jelentkezzen az ingyenes próbaverzióra, mobileszközön használja, amely képes fogadni az SMS-EK és érvényes hitelkártyát szüksége van.

Ügyeljen arra, hogy fel, hogy a legjobb használata a $200 Azure hitelképesség ingyenes, hajtsa végre a Ez a cikk végén "Karbantartása: az Azure-fiók" című szakasz lépéseit.

## <a name="provision-azure-resources-required-for-the-tutorial"></a>Az oktatóprogram szükséges Azure erőforrásokat kiépítése

Ebben az oktatóanyagban csak két esemény hubok *belépési* és *kilépési* adatfolyamok fogadásához. Azure SQL-adatbázis exportálja az eredményt, a megjelenítő Értékáram-elemzés feladatok. Azure tároló gépjármű regisztrációk hivatkozás adatait tárolja.

A Setup.ps1 parancsfájl GitHub TollApp mappájában hozhat létre az összes szükséges források. Érdekében idő azt javasoljuk, hogy futtatja. Ha azt szeretné, ha többet szeretne tudni az ezek az erőforrások beállításáról az Azure-portálon, olvassa el a "Oktatóanyag erőforrások konfigurálása az Azure-portálon" mintája.

Töltse le és mentse a kiegészítő [TollApp](http://download.microsoft.com/download/D/4/A/D4A3C379-65E8-494F-A8C5-79303FD43B0A/TollApp.zip) mappát, és a fájlokat.

Nyissa meg a **Microsoft Azure PowerShell** ablak _rendszergazdaként_. Ha még nincs Azure PowerShell, kövesse a [Telepítse és állítsa be a Azure PowerShell](../powershell-install-configure.md) kattintva telepítheti.

Windows automatikusan akadályozza .ps1 dll és .exe fájlok, kell beállítania az adatvégrehajtás-házirendet, a parancsfájl futtatása előtt. Ellenőrizze, hogy az Azure PowerShell ablak működik, és _rendszergazdaként_. Futtassa a **Set-végrehajtási házirend korlátlan**. Amikor a rendszer kéri, írja be az **Y**.

![Képernyőkép a "Set-végrehajtási házirend korlátlan" Azure PowerShell ablakában fut](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Futtassa a **Get-végrehajtási házirend** kattintva győződjön meg arról, hogy a parancs dolgozott.

![Képernyőkép a "Get-végrehajtási házirend" Azure PowerShell ablakában fut](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Nyissa meg a könyvtár, amelynek a parancsprogramokat és -készítő alkalmazás.

![Képernyőkép a "CD-re .\TollApp\TollApp" az Azure PowerShell ablakában fut](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Típus **.\\ Setup.ps1** az Azure-fiók beállításához létrehozása és konfigurálása az összes szükséges források, és események létrehozásához. A parancsprogram véletlenszerűen felveszi egy területet az erőforrások létrehozásához. Explicit módon adja meg a régió, átviheti a **-hely** paramétert a következő példának megfelelően:

**. \\Setup.ps1-hely "Központi USA"**

![Azure bejelentkezési lapja](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

A parancsfájl a **Bejelentkezési** lapja megnyílik a Microsoft Azure. Adja meg a fiók hitelesítő adatait.

> [AZURE.NOTE] Ha a fiók hozzáféréssel több előfizetéssel rendelkezik, a rendszer kéri, írja be az oktatóprogram használni kívánt előfizetés nevét.

A parancsprogram a futtatásához néhány perc is eltelhet. Miután befejeződött, a kimeneti az alábbi képernyőképen így néz.

![Képernyőkép a kimeneti az Azure PowerShell ablakban a parancsprogram](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Akkor is látható, amely hasonlít az alábbi képernyőképen egy másik ablakban. Ez az alkalmazás események küld az oktatóprogram futtatásához szükséges Azure esemény hubok. Igen nem le az alkalmazás vagy zárja be az ablakot, amíg be nem fejeződik az oktatóprogram.

![Képernyőkép a "Küldés központi eseményadatok"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Most már láthatja az összes létrehozott erőforrások az Azure-portálon kell lennie. Nyissa meg a <https://manage.windowsazure.com>, és jelentkezzen be a fiók hitelesítő adatait.

### <a name="azure-event-hubs"></a>Azure esemény hubok

Kattintson a **Szolgáltatás BUS** az Azure portál bal oldalán a parancsfájl az előző részben létrehozott esemény hubok megjelenítéséhez.

![Szolgáltatás Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Az összes rendelkezésre álló névtér az előfizetés jelenik meg. Kattintson a *tolldata* kezdődik (a példában szereplőhöz tolldata4637388511). Kattintson az **Esemény HUBOK** fülre.

![Esemény hubok fülre az Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

Két *bejegyzés* nevű esemény hubok és a *Kilépés* a névtér létrehozott jelenik meg.

![Képernyőkép a "bejegyzés" és "kilépési" esemény hubok az Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)

### <a name="azure-storage-container"></a>Azure tárterület-tárolóhoz

1. Kattintson a **TÁRHELY** az Azure portál bal oldalán látható a Azure tároló tároló az oktatóprogram használt.

    ![Tárterület menüpont](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)

2. Kattintson a *tolldata* kezdődő (ebben a példában tolldata4637388511). Kattintson a **TÁROLÓK** fülre kattintva megtekintheti a létrehozott tároló.

    ![Tárolók lapon az Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

3. Kattintson a **tolldata** tároló gépjármű regisztrációs adatokat tartalmazó feltöltött JSON-fájl megtekintéséhez.

    ![Képernyőkép: a tároló registration.json fájlban](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image13.png)

### <a name="azure-sql-database"></a>Azure SQL-adatbázis

1. Kattintson az **SQL-ADATBÁZISAIT** az Azure portál bal oldalán látható az oktatóprogram során használt SQL-adatbázishoz.

    ![SQL-adatbázisait](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image14.png)

2. Kattintson a **tolldatadb**.

    ![Képernyőkép: a létrehozott SQL-adatbázishoz](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)

3. Másolja a kiszolgáló nevét, a portszámot nélkül (*kiszolgálónév*., például database.windows.net).

## <a name="connect-to-the-database-from-visual-studio"></a>Kapcsolódás az adatbázist a Visual Studio

Visual Studio segítségével elérheti a lekérdezés eredményében a kimeneti adatbázisban.

Kapcsolódás az SQL-adatbázis (cél) a Visual Studio:

1. Nyissa meg a Visual Studióban, és kattintson az **eszközök** > **Kapcsolódás adatbázis**.

2. Ha a program kéri, kattintson a **Microsoft SQL Server** adatforrás.

    ![Adatforrás módosítása ablak](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)

3. A **kiszolgáló neve** mezőben, illessze be a nevet, amelyet az előző szakaszban az Azure portálról másolta (Ez azt jelenti, hogy a *kiszolgálónév*. database.windows.net).

4. Kattintson az **SQL Server-hitelesítés használata**.

5. A **felhasználónév** mezőbe írja be a **tolladmin** és **123toll!** a **jelszó** mezőbe.

6. Kattintson a **Jelölje ki vagy adja meg az adatbázis nevét**, és jelölje be az adatbázis **TollDataDB** .

    ![Adja hozzá a kapcsolat párbeszédpanel](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)

7. Kattintson az **OK gombra**.

8. Nyissa meg a kiszolgáló Intézőt.

    ![Kiszolgáló Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)

9. Lásd: a négy TollDataDB adatbázisból származó táblázatot.

    ![Az TollDataDB adatbázis táblájához](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Esemény nyilvántartás-készítő alkalmazás: TollApp minta projekt

A PowerShell-parancsprogramot, automatikusan elindul események küldése az TollApp minta alkalmazás használatával. Nem kell végrehajtani, további lépésekre.

Ha érdeklik megvalósítás részleteire, GitHub [Minták/TollApp](https://aka.ms/azure-stream-analytics-toll-source)az TollApp alkalmazás forráskódot is talál.

![Megjelenik a Visual Studióban példakódot képernyőképe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Értékáram-elemzés feladat létrehozása

1. Az Azure-portálon nyissa meg a megjelenítő Értékáram-elemzés, és kattintson az **Új** analytics új feladat létrehozásához a lap bal alsó sarkában.

    ![Az új gomb](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)

2. Kattintson a **gyors létrehozás**gombra. Jelölje ki az ugyanabban a hol találhatók a más erőforrások a parancsfájl által létrehozott régióban.

3. A **Regionális FIGYELÉSE TÁRHELYET fiók** beállításához jelölje be az **Új TÁRTERÜLET-fiók létrehozása** , és adja meg egy egyedi nevet. Azure Értékáram-elemzés a jövőbeni feladatok nyomon adatainak tárolása ebben a fiókban fogja használni.

4. Kattintson **Az ADATFOLYAM ANALYTICS feladat létrehozása** az oldal alján.

    ![Adatfolyam-elemző feladat jelölőnégyzet](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Beviteli források meghatározása

1. Kattintson a létrehozott analytics feladatra a portálon.

    ![Képernyőkép: a analytics feladat a portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image23.jpg)

2. A **ráfordítások** lapján határoznia a forrásadatait.

    ![A ráfordítások lap](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.jpg)

3. Kattintson **a BEVITELI hozzáadása**gombra.

    ![A beviteli beállítást hozzáadása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)

4. **ADATFOLYAM** kattintson az első oldalon.

    ![A adatfolyam beállítás](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image26.png)

5. **Esemény központban** kattintson a varázsló második lapja.

    ![Az esemény központi beállítás](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image27.png)

6. Írja be a **EntryStream** **BEVITELI**adhatnak.

7. Kattintson az **Esemény központi** legördülő, és válassza a "TollData" (például TollData9518658221) kezdődik.

8. Jelölje ki a **bejegyzést** , valamint az esemény központi neve **összes** esemény központi házirend neveként.

    A beállítások fog kinézni:

    ![Esemény központi beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

9. A következő lapon válassza ki a **JSON** **Esemény SZERIALIZÁLÁSI formátum** és **UTF8** értéket **KÓDOLÁS**.

    ![Szerializálási beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image29.png)

10. Kattintson az **OK gombra** a lap alján a varázsló befejezéséhez.

    ![Képernyőkép a EntryStream beviteli az Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image30.jpg)

    Most, hogy a bejegyzés adatfolyam létrehozta, akkor ezeket a lépéseket a Kilépés adatfolyam létrehozása követi. Kattintson a varázsló harmadik lapja ügyeljen arra, hogy az alábbi képernyőképen adja meg az értékek.

    ![A Kilépés adatfolyam beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)

    Két bemeneti adatfolyamok definiált:

    ![Definiált beviteli adatfolyamok az Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.jpg)

    Ezután ad hozzá hivatkozást a bevitt adatoknak az blob-fájl, amely tartalmazza az autós regisztrációs adatok számára.

11. Kattintson a **BEVITELI hozzáadása**gombra, és kattintson a **Hivatkozás ADATAIT**.

    ![A kijelölt hivatkozás adatokat "Hozzáadása egy bemenet" beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image33.png)

12. A következő lapon válassza ki a tárterület-fiókot, **tolldata**kezdődik. A tároló nevét kell **tolldata**, és a blob **Elérési_út minta** területen kell lennie **registration.json**. A fájlnév nagybetűk, és kisbetűre cseréli le kell lennie.

    ![Blog tárhely beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)

13. Jelölje ki az értékeket, a következő lapon az alábbi képernyőképen látható módon, és kattintson az **OK gombra** a varázsló befejezéséhez.

    ![Kijelölés a JSON "páros szerializálási formátum" és a UTF8 "Kódolás"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image35.png)

Most már minden bemeneti adatok alapján határozza meg.

![Képernyőkép: a három definiált bemeneti adatok alapján](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image36.jpg)

## <a name="define-output"></a>Kimeneti meghatározása

1. Kattintson a **kimeneti** fülre, és kattintson a **Egy KIMENET hozzáadása**gombra.

    ![A kimenet lap és a "Hozzáadása olyan eredménye" beállítás](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.jpg)

2. Kattintson az **SQL-adatbázishoz**.

3. Jelölje ki a kiszolgáló nevét, a "Csatlakozás az adatbázist a Visual Studio" szakaszban a használt. Az adatbázis neve **TollDataDB**.

4. A **felhasználónév** mezőbe írja be a **tolladmin** **123toll!** a **jelszó** mezőbe, és **TollDataRefJoin** a **tábla** mezőben.

    ![SQL-adatbázis beállítása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.jpg)

## <a name="azure-stream-analytics-query"></a>Azure adatfolyam analytics-lekérdezés

A **lekérdezés** lap egy SQL-lekérdezés, amely alakítja a bejövő adatokat tartalmazza.

![A lekérdezés lap hozzáadott lekérdezés](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.jpg)

Ebben az oktatóanyagban megkísérli kérdésekre több üzleti adatok és tartalmazza az Azure Értékáram-elemzés a megfelelő választ megadására használható Értékáram-elemzés lekérdezések díjköteles kapcsolódó.

Mielőtt megkezdené a első Értékáram-elemzés feladatok, vizsgáljuk meg néhány forgatókönyveket és a lekérdezés szintaxisát.

## <a name="introduction-to-azure-stream-analytics-query-language"></a>Értékáram-elemzés Azure-lekérdezési nyelv – bevezetés
-----------------------------------------------------

Tegyük fel, amelyet fel kell, adja meg a díjköteles stand járműveket számának. Mivel ez a teljes folyamatos továbbítása, meg kell adnia egy "időtartam." Most módosítsa a kérdés "hány járműveket írja be a díjköteles stand három percenként?" lesz. Ez gyakran nevezik tumbling számának.

Tekintse meg a Azure Értékáram-elemzés lekérdezést, amely erre a kérdésre adott válaszok:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Amint látható, az Azure Értékáram-elemzés, amely olyan, mint az SQL, és adja meg a lekérdezés idő kapcsolatos szempontok néhány bővítmények hozzáad lekérdezési nyelvet használ.

További információt olvashat a lekérdezésben MSDN használt [Időbeosztás](https://msdn.microsoft.com/library/azure/mt582045.aspx) és az [ablakok elrendezése](https://msdn.microsoft.com/library/azure/dn835019.aspx) tartalmazza.

## <a name="testing-azure-stream-analytics-queries"></a>Értékáram-elemzés Azure lekérdezések tesztelése

Most, hogy az első Azure Értékáram-elemzés lekérdezés írt, pedig tesztelje a mintaadatok fájljai a TollApp mappában, a következő helyen található:

**.. \\TollApp\\TollApp\\adatok**

Ez a mappa tartalmazza a következő fájlokat:

- Entry.JSON
- Exit.JSON
- Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>Kérdés 1: A díjköteles stand beírása járműveket száma

1. Nyissa meg az Azure portált, és nyissa meg a létrehozott Azure Értékáram-elemzés feladatok. Kattintson a **LEKÉRDEZÉSEK** fülre, és illessze be a lekérdezés az előző szakaszétól.

    ![A lekérdezés illeszti be a lekérdezés lap](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image40.png)

2. A mintaadatok lekérdezése érvényesítéséhez a **vizsgálat** gombra. A megnyíló párbeszédpanelen Entry.json, a **EntryTime** esemény adatfolyam mintaadatokat tartalmazó fájl megnyitásához.

    ![Képernyőkép: a Entry.json fájl](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)

3. Ellenőrizze, hogy a lekérdezés eredményét van-e a várt módon:

    ![A vizsgálat eredményeit](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.jpg)

## <a name="question-2-report-total-time-for-each-car-to-pass-through-the-toll-booth"></a>2 kérdés: Jelentés egyes autós át a díjköteles stand teljes ideje

Mérje fel, hogy a folyamat, és a felhasználói felület hatékonyságát segít az átlagos egy át a nem ingyenes autós működéséhez szükséges időt.

Keresse meg a teljes időt, szüksége az EntryTime adatfolyam az ExitTime adatfolyam a csatlakozáshoz. A adatfolyamok TollId és LicencePlate oszlopok fog csatlakozni. A **Csatlakozás** operátorral szükséges, hogy adja meg az illesztett események elfogadható idő különbségét leíró időbeli eltérést. **DATEDIFF** függvény segítségével fog adja meg, hogy legyen-e az események legfeljebb 15 percet egymástól. A **DATEDIFF** függvény való kilépéshez és a bejegyzés időpontok számítja ki a díjköteles állomás tölt autót tényleges időpontot is érvényesek. Megjegyzés: a különbség a használat **DATEDIFF** , **Csatlakozás** feltétel, hanem a **SELECT** utasítás az alkalmazásakor.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. A lekérdezés tesztelése, hogy a lekérdezés a **lekérdezés** lapon a projekt frissítése:

    ![A lekérdezés lap frissített lekérdezésben](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.jpg)

2. Kattintson a **vizsgálat** gombra, és adja meg a bemeneti mintafájlok EntryTime és ExitTime.

    ![Képernyőkép: a kijelölt beviteli fájlok](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image44.png)

3. Jelölje be a jelölőnégyzetet, a lekérdezés tesztelése és a kimeneti megtekintése:

    ![A próba eredménye](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>Kérdés 3: A lejárt regisztráció összes kereskedelmi járműveket jelentése

Azure Értékáram-elemzés statikus egy adott időpontban érvényes adatok használatával csatlakozik a időbeli adatfolyamok. Ez a képesség bemutatják, használja a következő példa kérdést.

A kereskedelmi gépjármű regisztrált a díjköteles vállalatot, ha azt is haladnak át a díjköteles stand nélkül ellenőrzésre le. Kereskedelmi gépjármű regisztrációs keresési tábla összes kereskedelmi járműveket regisztrációk lejárt azonosítani kell használni.

    SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN Registration
    ON EntryStream.LicensePlate = Registration.LicensePlate
    WHERE Registration.Expired = '1'

Hivatkozási adatok a lekérdezés tesztelése, kell a hivatkozási adatok, amelyek már elkészült bemeneti forrása határozza meg.

1. A lekérdezés tesztelése, hogy a lekérdezés illessze be a **lekérdezés** lap **tesztelése**kattintson, és adja meg a két bemeneti forrás:

    ![Képernyőkép: a kijelölt beviteli fájlok](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

2. A lekérdezés eredményét megtekintése:

    ![Képernyőkép a lekérdezés eredménye](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image47.png)

## <a name="start-the-stream-analytics-job"></a>Indítsa el a Értékáram-elemzés feladat


Most pedig a konfigurációs Befejezés, és indítsa el a feladatot. Mentse a lekérdezést a kérdés 3, amely hoznak létre, amely megfelel az **TollDataRefJoin** eredménytábla sémájának kimeneti.

Nyissa meg a feladat **IRÁNYÍTÓPULT**, és kattintson a **START**gombra.

![A Start gombra, a feladat irányítópult képernyőképe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.jpg)

A megnyíló párbeszédpanelen a **Kimeneti indítása** idejének módosítása **egyéni**időpontra. Az óra egy órával a pontos idő módosítása Ez a módosítás biztosítja, hogy az esemény-központban összes események feldolgozása, az események létrehozni az oktatóprogram elején kezdete óta. Most kattintson a pipa ikonra kattintva indítsa el a feladat.

![Egyéni idő kiválasztása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

A projekt kezdési néhány percet is igénybe vehet. Megtekintheti az állapotot a legfelső szintű lapon adatfolyam elemzéséhez.

![Képernyőkép: a feladat állapota](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.jpg)

## <a name="check-results-in-visual-studio"></a>A Visual Studióban ellenőrzések eredményei

1. Nyissa meg a Visual Studio kiszolgáló Intézőt, és kattintson a jobb gombbal a **TollDataRefJoin** táblában.
2. Kattintson a **Táblázat adatainak megjelenítése** a kimenet: a feladatok megtekintéséhez.

    ![A kijelölés "Megjelenítése táblázat-adatok" Server Explorer](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Értékáram-elemzés Azure feladatok méretezése

Azure Értékáram-elemzés elastically méretarányra célja, hogy is képes kezelni az adatokat. Az Azure Értékáram-elemzés lekérdezés használhatja a **PARTÍCIÓT BY** záradékot állapítható meg, hogy a rendszer, ebben a lépésben el fogja méretezni. **PartitionId** speciális oszlop, amely a rendszer hozzáadja a bemeneti (esemény központi) partition azonosítója megfelelően.

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Az aktuális feladat leállítása, módosítsa a lekérdezést a **lekérdezés** lapon, és nyissa meg a **mérete** fülre.

    **A folyamatos ÁTVITELŰ egységek** határozzák meg a számítási power, amely a feladat képes fogadni.

2. 6 húzza a csúszkát.

    ![Képernyőkép a 6-adatfolyam erőforrás-mennyiség kijelölése](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.jpg)

3. Nyissa meg a **kimeneti ÉRTÉKEKET** fülre, és módosítsa az SQL-tábla nevét a **TollDataTumblingCountPartitioned**.

Ha a feladat kezd ezután Azure Értékáram-elemzés is munka elosztása további számítási források, jobb teljesítmény elérése. Felhívjuk a figyelmét arra, hogy a TollApp alkalmazás is küld által TollId particionálva eseményeket.

## <a name="monitor"></a>Monitor

A **MONITOR** lapján a futó feladat statisztikájának tartalmazza.

![Képernyőkép a statisztikájának futó feladatok](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image53.png)

**A művelet naplók** az **IRÁNYÍTÓPULT** lap érheti el.

![A "Művelet naplók" beállítás](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image54.jpg)

![Képernyőkép: a művelet naplók, ahol megtekintheti a feladatok állapotát](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image55.png)

További információ a egy adott esemény megtekintéséhez kattintson arra az eseményre, és kattintson a **Részletek** gombra.

![Képernyőkép: a kijelölt esemény részleteinek](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image56.png)

## <a name="conclusion"></a>Elfogadásáról

Ebben az oktatóanyagban jelent meg, a megjelenítő Értékáram-elemzés Azure szolgáltatás. Azt igazolni ráfordítások és a kimeneti értékeket, a megjelenítő Értékáram-elemzés feladathoz beállításáról. A nem ingyenes adatok forgatókönyv használ, az oktatóprogram magyarázata elterjedt típusú adatokat a mozgásvonalak, és hogyan azok megoldhatók egyszerű Értékáram-elemzés Azure SQL-szerű lekérdezések térköz felmerülő problémák. Az oktatóprogram leírt SQL bővítmény tartalmazza az adatok időbeli használatához. Csatlakozás adatfolyamok, hogyan kell a adatfolyam statikus hivatkozási adatok kiegészítése és méretezése lekérdezés nagyobb teljesítmény elérése érdekében hogyan azt mutatott.

Bár ez az oktatóanyag egy jó – bevezetés nyújt, még nem teljes, bármilyen eszközzel. Több lekérdezés mintát SAQL nyelv [közös Értékáram-elemzés szokásai lekérdezés példákat is tartalmaz](stream-analytics-stream-analytics-query-patterns.md), is megkeresheti.
Olvassa el az [online dokumentációt](https://azure.microsoft.com/documentation/services/stream-analytics/) , ha többet szeretne tudni az Azure Értékáram-elemzés.

## <a name="clean-up-your-azure-account"></a>Az Azure-fiók megszüntetése

1. Állítsa le a Értékáram-elemzés feladat az Azure-portálon.

    A Setup.ps1 parancsfájl hoz létre, két esemény hubok és SQL-adatbázishoz. A következő utasítások segítenek karbantartása erőforrások az oktatóanyag végén.

2. Írja be a PowerShell ablakban, a **.\\ Cleanup.ps1** a parancsfájlt, amely törli az oktatóprogram során használt erőforrások indításához.

    > [AZURE.NOTE] Erőforrások azonosítja a nevét. Ellenőrizze, hogy gondosan ellenőrizze az egyes elemek megerősítése eltávolítása előtt.

    ![Képernyőkép: a törlési folyamat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image57.png)
