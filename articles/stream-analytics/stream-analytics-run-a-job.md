<properties 
    pageTitle="Értékáram-elemzés a feladatok indítása a folyamatos átvitelű |} Microsoft Azure" 
    description="Hogyan működik a továbbított feladat az Azure Értékáram-elemzés |} tanulási javaslat szakaszában."
    keywords="adatfolyam-feladatok"
    documentationCenter=""
    services="stream-analytics"
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

# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Hogyan tehető függővé egy adatfolyam feladat az Azure Értékáram-elemzés

Amikor egy projekt beviteli, a lekérdezés és a kimeneti összes lett megadva elindíthatja a Értékáram-elemzés feladatot.

A projekt indítása:

1.  Az Azure klasszikus az feladat irányítópult-portálon kattintson a **Start** az oldal alján.

    ![Indítsa el a feladat gomb](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  

    Az Azure-portálon kattintson a **Start** a feladat lap tetején.

    ![Azure portál indítási feladat gomb](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  

2.  Határozza meg, ha ezt a feladatot fog elindulni kimeneti előállító **Indítása kimeneti** érték megadása Az alapértelmezett feladatok, amely a korábban nem lett elindítva a **Projekt kezdési idő**, ami azt jelenti, hogy a feladat azonnal indítása a feldolgozás adatok. **Egyéni** egyszerre korábban (az korábbi adatok fogyasztása) vagy a tervek (a késleltetés feldolgozás később bármikor-ig) is megadhatja. Az esetek, amikor egy projekt korábban elindítható és leállítható, a lehetőség **Le legutóbbi** érhető el folytassa a kimeneti utoljára a feladatot, és az adatvesztés elkerülése érdekében.  

    ![Indítsa el a folyamatos átvitelű feladat ideje](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  

    ![Azure portál indítási adatfolyam feladat ideje](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  

3.  Erősítse meg az értékeket. A feladat állapota megváltozik a *kezdési* és fog hamarosan Ugrás *fut* a feladat megkezdése után. Ellenőrizheti, hogy az **Értesítési központi** **elindítása** művelet előrehaladását:

    ![a folyamatos átvitelű projekt előrehaladásának](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  

    ![A folyamatos átvitelű projekt előrehaladásának Azure portál](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
