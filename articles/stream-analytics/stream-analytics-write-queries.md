<properties 
    pageTitle="Értékáram-elemzés a hatékony szövegalkotás lekérdezések |} Microsoft Azure" 
    description="Értékáram-elemzés és a lekérdezés adatai-lekérdezéseket írni |} tanulási javaslat szakaszában."
    keywords="Hogyan lekérdezés adatait,-lekérdezéseket írni, írja be a lekérdezés lekérdezések írása"
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

# <a name="how-to-write-queries-in-stream-analytics"></a>Értékáram-elemzés a hatékony szövegalkotás lekérdezések

Azure Értékáram-elemzés logikája feldolgozása adatfolyam lekérdezések írása lekérdezésként"folyamatos" előtt a feladat elindul, és végre adatok, mert a feladat eléri meghatározott végrehajtani. A átalakítási egy SQL-szerű lekérdezési nyelv, amely nagymértékben csak egy részhalmazát az SQL-T, mint az [ablakok elrendezése](https://msdn.microsoft.com/library/azure/dn835019.aspx) időbeli szemantikáját kifejezéséhez használt néhány nyelvének hozzáfűzött kiterjesztésű van megadva.

## <a name="writing-queries"></a>Lekérdezések írt: ##

1. A adatfolyam Analytics feladatok az Azure adatkezelési portálon kattintson a **lekérdezés**gombra.

    ![Lekérdezés kiválasztása](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  

    Az Azure-portálon kattintson a **lekérdezés**gombra.

    ![Jelölje be a lekérdezés villámnézetét megjelenítő](./media/stream-analytics-write-queries/query-preview-portal.png)  

2.  Új feladat van egy lekérdezési sablon használatának megkezdéséhez. Lekérdezési sablon hajt végre, a kimeneti be az összes projekt mezők "átadó" lekérdezés beviteli események.  

    - Ha legalább egy bemeneti és kimeneti a projektre vonatkozóan definiált, lecserélheti a helyőrző "[YourOutputAlias]", és az aliasokat, a bemeneti és kimeneti kívánt mezőket "[YourInputAlias]" először használja. Ezeken kívül is továbbra is tartalom-előállítás és a lekérdezés tesztelése az Azure klasszikus portálon anélkül, hogy a feladat definiáló ráfordítások és a kimeneti értékeket.
    - Ha szeretne egy egyszerű átadó-nél több feldolgozás végrehajtani, módosíthatja a lekérdezésdefiníció. Az első lépések a lekérdezések létrehozásával kapcsolatos, ajánljuk figyelmébe az néhány gyakori lekérdezési mintázatok rögzítésének [Itt](stream-analytics-stream-analytics-query-patterns.md).  
  
    ![Lekérdezés adatok ablakban](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>A lekérdezés adatérvényesítés használatáról: ##

Ellenőrizheti, hogy a lekérdezés úgy viselkednek, futtassa a böngészőben fölé egy vagy több helyi JSON fájlok vizsgált adatokat tartalmazó vártnak. Ezzel nem indítható feladat vagy bármely számlázási következményekkel.

> [AZURE.NOTE] Jelenleg a böngészőben a lekérdezés tesztelése nem támogatott az Azure-portálon.  

1.  Győződjön meg arról, hogy nincsenek-e hibák a lekérdezés (egyéb esetben a vizsgálat gombra letiltjuk), és kattintson a vizsgálat gombra.  

    ![A lekérdezés tesztelése adatok](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  

2.  Adja meg a fájlok minden egyes a bemeneti adatok a lekérdezés által hivatkozott kéri. Ebben a példában a sablon lekérdezés állapotban maradt-van, így a párbeszédpanelen az adatbevitelt "yourinputalias" nevű kér.  

    ![Adatok lekérdezés tesztelése](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  

3.  Keresse meg a próba-fájlt. Több mintafájlok elérhetők [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) , és a saját adatok adatfolyam bemeneti adatok alapján, a mintaadatok függvény a bemenetben lapon keresztül is meghallgathatja mintaadatokat.  

    ![Lekérdezés beviteli](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  

4.  A párbeszédpanel bezárása, után a lekérdezés tesztelése adatok felett fog futni és látni fogja az eredmények a lekérdezéslap alján.  

    ![Lekérdezés összegzése](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
