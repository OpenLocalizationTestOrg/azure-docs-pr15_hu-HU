<properties
    pageTitle="Értékáram-elemzés feladatok hibaelhárítása és ábrázolása |} Microsoft Azure"
    description="Megtudhatja, miként jelenítse meg a megjelenítő Értékáram-elemzés feladat folyamat önkiszolgáló hibaelhárítási diagnosztika diagram funkcióval."
    keywords=""
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


# <a name="visualize-and-troubleshoot-stream-analytics-jobs"></a>Értékáram-elemzés feladatok hibaelhárítása és ábrázolása

Értékáram-elemzés, a más felhőalapú technológiák a hibaelhárítás előfordul, hogy szükség van vizsgálja meg a miért egy feladat nem hozhatók létre a várt kimenet (vagy bármely kimeneti ezt a témát). Ezzel szem előtt a megjelenítő Értékáram-elemzés adatfolyam feladat megjelenítése lehetőséget biztosít. Is hasznos modellezési eszközként, és rendelkezik, oldal számára nyújtott előnyökre: ezeket a munka igénylő dokumentációjának.

A Megjelenítés panelen a bemeneti adatok alapján jelennek meg a lekérdezés végrehajtott, és kattintson az összes a kimeneti értékeket konfigurált és. Csatlakozási problémák vagy konfigurációs problémák előfordulhat, hogy több látható, és is hasznos lehet vizuálisan ábrázolhatók az Ön konfigurációjának megjelenítéséhez.

## <a name="using-the-diagnosis-diagram-tool"></a>A diagnosztikai diagram eszközzel

Hozzá szeretne férni a visualizer, egyszerűen kattintson a "Diagnosztizálása diagram" gombra a a "Beállítások" lap, a megjelenítő Értékáram-elemzés feladat.

![Stream-Analytics-troubleshoot-Visualization-Diagnosis-diagram](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-diagnosis-diagram1.png)

Minden bemeneti és kimeneti színkódokkal jelzi az aktuális állapot összetevő alább látható módon.

![Stream-Analytics-troubleshoot-Visualization-input-Map](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-input-map.png)

A felhasználó szeretne tekintse meg az adatok folyamat mintázatok belül egy feladat megértéséhez köztes lekérdezési lépéseket, a képi megjelenítések eszköz a összetevő lépéseket, és a folyamat sorrendjének biztosít a részletezzük, hogy a lekérdezés egy nézetét. A lekérdezés lépésein kattint, megjelenik a lekérdezés szerkesztése ablakban, amint a megfelelő szakaszban. 

![Stream-Analytics-troubleshoot-Visualization-Intermediate-steps](./media/stream-analytics-troubleshoot-visualization/stream-analytics-troubleshoot-visualization-intermediate-steps.png)




## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
