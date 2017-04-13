<properties
    pageTitle="Microsoft Azure IoT Suite áttekintése |} Microsoft Azure"
    description="Bemutatja, hogy hogyan Azure IoT csomagja biztosítja az internetes dolog, amit előre definiált megoldások összegyűjtése, elemezni, adatok tárolására, adja meg a képi megjelenítések és más rendszerekből integrálása."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/09/2016"
     ms.author="dobett"/>

# <a name="what-is-azure-iot-suite"></a>Mi az Azure IoT csomagja?

Az Azure internet dolog (IoT) szolgáltatások széles köre lehetőségeket kínálnak. Az alábbi vállalati minőségű szolgáltatások lehetővé teszi:

- Adatgyűjtés eszközökről
- Adatok adatfolyam megjelenítését a – mozgás elemzése
- Tárolhatja és nagy adatkészletek lekérdezés
- Valós idejű és a korábbi adatok ábrázolása
- Integráció az office-háttér rendszerek

Használhat az előadáshoz alábbi funkciók, Azure IoT csomagja csomagok közös egyéni kiterjesztésű több Azure szolgáltatás *előre definiált megoldások*formájában. Ezekkel az előre beállított megoldásokkal olyan gyakori csökkentheti az időt, hogy a IoT megoldásokat IoT megoldás mintázatok alap példányait. Használja a [IoT szoftverfejlesztői][lnk-sdks], testreszabása és a kiterjesztése a következő megoldások saját igényeinek kielégítéséhez. Is használhatja ezeket a megoldások példák vagy sablonok új IoT megoldások fejlesztésekor.

Az alábbi videó Azure IoT csomagja áttekintése:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT szolgáltatások Azure IoT másik alkalmazásból

Az előre definiált megoldások általában az alábbi szolgáltatásokat használni:

- A Azure IoT programcsomagban Core az [Azure IoT központi] [ lnk-iot-hub] szolgáltatás. Ez a szolgáltatás az eszköz a felhőbe és a felhő-eszköz üzenetküldési szolgáltatásokat nyújt, és a felhőben, és a fő IoT csomagja szolgáltatásokat az átjáró működik-e. A szolgáltatás lehetővé teszi, hogy a méretezés eszközén érkező üzenetek fogadására és parancsok küldeni az eszközöket.

- [Azure Értékáram-elemzés] [ lnk-asa] mozgásvonalas az adatelemzés biztosít. IoT csomagja bejövő telemetriai feldolgozása, összesítést végrehajtani, és események észleli szolgáltatást használja. Az előre definiált megoldások Értékáram-elemzés is használhatja, például a metaadat-alapú vagy parancs válaszok eszközökről adatot tartalmazó tájékoztató üzenetek feldolgozása. A megoldások Értékáram-elemzés segítségével az üzenetek készülékekről folyamat és egyéb szolgáltatásokhoz használhat az előadáshoz azokat az üzeneteket.

- [Azure tároló] [ lnk-azure-storage] és [Azure DocumentDB] [ lnk-document-db] adja meg az adatokat tároló funkciók. Az előre definiált megoldások használja a blob-tárolóhoz telemetriai tárolására és elemzéshez elérhetővé szeretné tenni. A megoldások eszköz metaadatokat tárolhatnak, és megoldást az eszköz kezelési lehetőségek engedélyezése DocumentDB használatával.

- [Azure Web Apps alkalmazások] [ lnk-web-apps] és a [Microsoft Power BI] [ lnk-power-bi] adja meg az adatok megjelenítési funkciók. A Power bi rugalmasság gyorsan a saját interaktív irányítópultok IoT csomagja adatforrásokat használó létrehozását teszi lehetővé.

Egy tipikus IoT megoldás architektúráját áttekintése lásd: [Microsoft Azure és az Internet a dolgot (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Előre definiált megoldások

IoT csomagja előre definiált megoldások, amelyek lehetővé teszik gyorsan – első lépések és közös IoT alkalmazási eseteit, például a *távoli figyelés* feltárása és a *prediktív karbantartási*, amely lehetővé teszi az Azure IoT csomagja tartalmazza. Ezek a megoldások telepítése Azure-előfizetéséhez, és futtassa a teljes, a végpontok közötti IoT példa.

## <a name="next-steps"></a>Következő lépések

Most, hogy mit tehet az IoT csomagja, és Mik azok a fő összetevői, többet szeretne tudni az előre definiált megoldások IoT programcsomagban című témakörben [Mik azok az Azure IoT előre megoldások?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
