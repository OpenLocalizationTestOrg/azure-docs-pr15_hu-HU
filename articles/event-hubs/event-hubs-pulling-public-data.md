<properties
    pageTitle="Nyilvános adatok adatok Azure esemény hubra |} Microsoft Azure"
    description="Webes minta importálása esemény hubokon áttekintése"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>Nyilvános adatok adatok Azure esemény hubra

Jellemző dolog Internet (IoT) lehetőséget a program az Azure-Azure esemény központi vagy egy IoT központi leküldéses adatok eszközök lehetősége van. Az adott hubok mindkettő belépési pontról az Azure – tárolásához, elemzése, az eszközök elérhetők, a Microsoft Azure számtalan megjelenítése. Azonban mindkettő igényel, hogy rájuk, formázza, JSON, és az adott módon védett leküldéses adatokat. Ekkor megjelenik a következő kérdést. Mi a teendő, ha az adatok forrásból származhatnak nyilvános vagy magánjellegű hol webszolgáltatás vagy valamilyen hírcsatornában megjelenő az adatokat, de nem rendelkezik az azt jelenti, hogy hogyan az adatok közzététele módosítása szeretne? Fontolja meg az időjárási, vagy a forgalmat, illetve részvényárfolyamok - nem tudja megállapítani, NOAA, vagy WSDOT vagy NASDAQ állítsa be az esemény hubon keresztül csatlakozott leküldéses. A probléma megoldásához, hogy már írt, és Megnyitás kifejezéskészletébe kis felhő minta módosítása, és telepítését, amely az adatokat olvashat néhány a forrásból származó és leküldéses meg az esemény-hubon keresztül csatlakozott. Itt végezheti el szeretne, tetszőleges, alá, természetesen a licencfeltételeket a gyártó. Az alkalmazás talál [Itt](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/).

Figyelje meg, hogy ez a példa a kód csak akkor látható, hogyan tipikus internetről származó adatok lekérése a hírcsatornákat, és hogyan kell írni az Azure-esemény hubon keresztül csatlakozott. Ez nem célja, hogy egy gyártási alkalmazást, lehet, és nincs kísérletek nem történtek legyen alkalmas olyan környezetben – strictfly egy DIY, csak a fejlesztői fókuszáló példa. Ezenkívül ez a példa a megléte esetén nem egyenértékű ajánlást, hogy a **leküldéses** , hanem Azure **lekéri** az adatokat kell azt. Érdemes tekintse át a biztonság, a teljesítmény, a használható funkciók körét, és a tényezők költség-végpont architektúra rendezése előtt.

## <a name="application-structure"></a>Alkalmazás-struktúra

Az alkalmazás írott C#, és a [minta leírását](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) tartalmazza az összes módosítást, összeállítása és az alkalmazás közzététele szükséges információk. A következő szakaszokban a Mit jelent az alkalmazás magas szintű áttekintése.

Azon a feltételezésen, hogy az access adatcsatornát a kezdje azt. Érdemes lehet például arra, hogy lekérje a Washington állam Közlekedési Minisztérium forgalom adatainak vagy NOAA, egyéni jelentések megjelenítéséhez vagy más adatokkal, az alkalmazás az adatok egyesítéséhez időjárási adatait. Is kell egy Azure esemény hubhoz be van állítva, és a kapcsolat, hogy hozzáférhessenek szükséges karakterlánc.

A GenericWebToEH megoldást indításakor helyébe a konfigurációs fájl (App.config) dolgot számot kap:

1. Az URL-címet, vagy a az adatok közzététele webhely URL-címek listáját. Ideális esetben egy webhelyet, amely az adatok közzététele a JSON-ban, például a WSDOT által hivatkozott: az [alábbi](http://www.wsdot.wa.gov/Traffic/api/). 
2. Az URL-címet, ha szükséges hitelesítő adatait. Számos nyilvános nem szükséges hitelesítő adatait, vagy a hitelesítő adatok elhelyezése az URL-karakterlánc. Másokat, hogy ki külön-külön. (Tájékoztatjuk, hogy csak adhatja meg a hitelesítő adatokat egy beállítása az alkalmazásban, ha csak egy URL-CÍMÉT, és nem URL-címek listáját adja meg csak akkor fog működni.)
3. A kapcsolati karakterláncot, és a nevére, amelyhez adatokat fog leküldéses, hogy esemény hubok névtér az esemény-központját. Az információk az Azure-portálon is megtalálhatók.
4. Egy meddig alhatnak időköze, ezredmásodpercben az lekérdezési az adatok nyilvános webhely közötti időtartamot. Ez a beállítás csak néhány gondolkodási. Ha túl ritkán lekérdezik, lemaradhat adatok; azonban ha túl gyakran lekérdezik az ismétlődő adatokat jelenhetnek meg, vagy rosszabb még, akkor előfordulhat, hogy blokkolhatja egy nefarious bot szerint. Fontolja meg, milyen gyakran frissüljenek az adatforrás - előfordulhat, hogy az adatok időjárási vagy forgalom frissíthetők 15 percenként, de árfolyam ajánlatok esetleg néhány másodpercenként, attól függően, hogy hol el őket. 
5. Az alkalmazás megállapítani, hogy az adatok elérhető lesz JSON vagy XML jelző. Meg kell nyomni az adatokat egy esemény-hubon keresztül csatlakozott, mivel a alkalmazásnak a modul XML alakítani JSON elküldése előtt.

A konfigurációs fájl elolvasása, után az alkalmazás működésbe ismétlődve – az adatok, ha szükséges, írni az esemény hubon keresztül csatlakozott, és ezután Várakozás meddig alhatnak intervallum előtt végezze el az összes feletti újra konvertálása nyilvános webhely elérése. Kifejezetten:

  * A nyilvános webhely olvasásakor. Fogadó kész – küldés adatok RawXMLWithHeaderToJsonReader osztály példányának Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs használják. Olvassa be a forrás-adatfolyam GetData() módszer, és ezután oszt kisebb darabokra (vagyis rekord) GetXmlFromOriginalText használatával. 
  Ez a módszer olvassa XML szabályos JSON vagy JSON és tömb. App.config MergeToXML konfiguráció majd feldolgozása indítják (alapértelmezett üres =).
  * Adatok fogadására és küldés mint a Process() módszer a Program.cs ismétlődve áll rendelkezésre. 
  Kimeneti eredmények fogad GetData(), után a módszer enqueues tagolt esemény-hubon keresztül csatlakozott.

## <a name="next-steps"></a>Következő lépések

A megoldást üzembe helyezéséhez klónozhatja vagy a [GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/) alkalmazás letöltése, a App.config fájl szerkesztése, létrehozása, és végül a projekt közzététele a. Az alkalmazás közzétett, miután láthatóvá válik a felhőalapú szolgáltatások Azure klasszikus portálon fut, és a konfigurációs beállításai (például a esemény központi célját és meddig alhatnak intervallum) módosíthatja a **beállítás** fülre.

Lásd: további esemény hubok minták az [Azure minták gyűjtemény](https://azure.microsoft.com/documentation/samples/?service=event-hubs) és az [MSDN webhelyen](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5).
