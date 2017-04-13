<properties 
    pageTitle="Értékáram-elemzés – bevezetés |} Microsoft Azure" 
    description="Tudjon meg többet az adatfolyam Analytics, olyan felügyelt szolgáltatás, amely segít a dolog Internet (IoT) adatfolyam adatainak elemzése a lapra." 
    keywords="Analytics szolgáltatásként, felügyelt szolgáltatásokat, a adatfolyam feldolgozása, a folyamatos átvitelű analytics, Mit nevezünk Értékáram-elemzés"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>


# <a name="what-is-stream-analytics"></a>Értékáram-elemzés fogalma

Azure Értékáram-elemzés egy teljes körű felügyelt és a költség hatékony valós idejű esemény feldolgozás motor, amely segítséget nyújt az adatok mélyebb háttérismeretek zárolásának feloldásához. Értékáram-elemzés egyszerűen állíthatja be a folyamatos átvitelű az eszközök, érzékelők, webhelyek, közösségi hálózat, alkalmazásokat, infrastruktúra-rendszerek és további adatokat a valós idejű analitikus számítások.

Néhány kattintással az Azure-portálon adja meg a bemeneti adatforrás adatfolyam, a kimeneti gyűjtő az eredmények a feladatok, a megjelenítő Értékáram-elemzés feladat készíthet, és egy átalakítási kifejezett SQL-szerű nyelven. Figyelheti, és a feladatok az Azure-portálon egy GB vagy több események feldolgozása másodpercenként a néhány kilobájtban méretezni méretezése és sebességének beállítása.

Értékáram-elemzés használja a Microsoft Research munka kidolgozásában évek erősen a folyamatos átvitelű időérzékeny feldolgozás motorok, valamint az ilyen intuitív jellemzői nyelvi integrációs beállítva.

## <a name="what-can-i-use-stream-analytics-for"></a>Mire van lehetőség adatfolyam elemzésének?
Nagy mennyiségű adatot olyan lépés, a nagy sebesség a hálózaton ma. Azoknak a szervezeteknek, folyamat és a valós idejű adatfolyam működésbe lépnek jelentősen hatékonyság növelése, és a piaci különbséget maguk. Alkalmazási helyzetek: valós idejű adatfolyam analytics található összes ágazatok keresztül: személyre szabott, valós idejű árfolyam kereskedési elemzés és a pénzügyi szolgáltatások cégek; által kínált értesítések valós idejű csalás észlelési; adatok és -Identitáskezelés védelem szolgáltatások; az Internet a dolgokat, vagy IoT; fizikai objektumok beágyazott megbízható bevitel és érzékelők és rákötni által generált adatok elemzése clickstream webstatisztikai; és az ügyfél Ügyfélkapcsolat-kezelés (CRM) alkalmazások kiadására értesítések, ha a felhasználói élmény egy időkereten belüli teljesítménye csökkent. A rugalmas, megbízható és költséghatékony módja a nagyon versenytársak modern üzleti világ sikeres ilyen valós idejű esemény-adatfolyam adatelemzés vállalkozások keres.

## <a name="key-capabilities-and-benefits"></a>Főbb funkciók és előnyei
-   **Egyszerű használat:** Értékáram-elemzés átalakítások leíró egy egyszerű, deklaráció lekérdezés modellt támogatja. Optimalizálhatja az egyszerű használat érdekében, hogy Értékáram-elemzés variant az SQL-T használ, és az ügyfelek számára a technikai bonyodalmainak rendszerek feldolgozása adatfolyam foglalkozik szükségtelenné. [Értékáram-elemzés lekérdezési nyelv](https://msdn.microsoft.com/library/azure/dn834998.aspx) használata a böngészőben a Lekérdezésszerkesztőben, kap intelli-értelemben automatikus kiegészítési gyorsan és egyszerűen végrehajtása az idő sorozat lekérdezések időbeli alapuló illesztést, ablakok összegzések, időbeli szűrők és egyéb gyakori műveletek, például az illesztés, összegzések, előrejelzések és szűrők. Ezenkívül a böngészőben lekérdezés tesztelése ellen, egy adatfájl – minta lehetővé teszi, hogy gyorsan és közelítéses fejlesztési.  

-   **Méretezhetőség:** Értékáram-elemzés is alkalmas nagy rendezvény átviteli legfeljebb 1GB/második kezelése. Integrálása az [Azure esemény hubok](https://azure.microsoft.com/services/event-hubs/) és [Azure IoT hubok](https://azure.microsoft.com/services/iot-hub/) lehetővé teszi a megoldást eseményeket egy második érkező csatlakoztatott eszközök, clickstreams, több millió ingest, és a naplófájlok néhány nevet. Ennek elérése érdekében Értékáram-elemzés használja is hozam 1 MB/s partíciót egy esemény hubok particionáló képességei. Felhasználók csak a kiszámítása az számmá alakítja a lekérdezésdefiníció logikai lépéseit partition, az azt jelenti, hogy tovább mindegyiket particionálva méretezhetőség növeléséhez.  

-   **Megbízhatóság, ismételhetőség és gyors helyreállítás:** A felhőben, a megjelenítő Értékáram-elemzés felügyelt szolgáltatás segít megakadályozni a az adatok elvesztését, és üzemkészségének megelőzve hibák keresztül beépített helyreállítási lehetőségeket biztosít. Együtt az azt jelenti, hogy a belső állapotban a szolgáltatás adja meg a lehetséges archiválja eseményeket, és újbóli alkalmazása, a jövőben, mindig első ugyanazt az eredményt feldolgozása biztosítása megismételhető eredményeit. Térjen vissza az idő és vizsgálja meg a számítások során a kiváltóok-elemzés, lehetőségelemzés stb az ügyfelek számára lehetővé teszi.  

-   **Alacsony költség:** Egy felhőalapú szolgáltatásba, mint Értékáram-elemzés optimalizált felhasználóknak nagyon alacsony költségek áttekintés és a valós idejű analytics-megoldások kezelése. A szolgáltatás ki a fizetés menet közben alapján a folyamatos átvitelű egység használatát és a rendszer által feldolgozott adatok mennyiségét épül. Használati események feldolgozása hangerejének és a számítási power belül a fürt kiépítéstől kezelheti a saját feladatok Értékáram-elemzés mennyisége alapján beállításból származik.  

-   **Hivatkozási adatok:** Értékáram-elemzés lehetőséget nyújt felhasználók adja meg, és a hivatkozási adatok használata. Ennek oka lehet a korábbi adatok vagy ritkábban időbeli változása egyszerűen nem-adatfolyam-adatokat. A rendszer minden bejövő esemény adatfolyam be szeretne kapcsolódni, és más valós idejű átalakítások végrehajtásához okozhatnak esemény adatfolyamok például kezelendő hivatkozási adatok felhasználása egyszerűbbé teszi.  

-   **Felhasználó által definiált függvények:** Értékáram-elemzés integrálása az Azure gépi tanulási függvény hívások határozhat meg a gépi tanulási szolgáltatás Értékáram-elemzés lekérdezés részeként tartalmaz. Értékáram-elemzés a meglévő Azure gépi tanulási megoldások használhatja funkcióinak kibontva. A további tudnivalókért olvassa el a [gépi tanulási integrációs oktatóprogram](stream-analytics-machine-learning-integration-tutorial.md).

-   **Kapcsolat:** Értékáram-elemzés közvetlenül az Azure esemény hubok és Azure IoT veszik fel a szükséges adatfolyam bevitel és az Azure Blob-szolgáltatás korábbi adatok ingest csatlakozik. Eredmények Azure BLOB-tároló vagy táblázatok, Azure SQL-adatbázis, Azure adatok tó tárolja, DocumentDB, esemény hubok, Azure Service Bus témakörök vagy sorok és a Power BI, ahol ez majd formájában jelenik meg, további munkafolyamatok által feldolgozott, az [Azure hdinsight szolgáltatáshoz](https://azure.microsoft.com/services/hdinsight/) keresztül köteg analytics használt vagy dolgozza fel újra a teljes sorozatának, a megjelenítő Értékáram-elemzés írható. Esemény hubok használata esetén is lehet több adatfolyam Analytics együtt más adatforrások és feldolgozása motorok írhat a számítások adatfolyam jellegét megtartásával.  

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Korábban már ismerje meg az adatfolyam Analytics, egy felügyelt szolgáltatást a folyamatos átvitelű analytics az internetes dolgot adataiból. Ezzel a szolgáltatással kapcsolatos további tudnivalókért lásd:

- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)

