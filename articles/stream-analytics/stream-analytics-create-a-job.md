<properties 
    pageTitle="Értékáram-elemzés létrehozása adatok analytics feldolgozás feladat |} Microsoft Azure" 
    description="Értékáram-elemzés az adatok analytics feldolgozás feladat létrehozásához |} tanulási javaslat szakaszában."
    keywords="adatok elemzések feldolgozása"
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

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Értékáram-elemzés adatok analytics feldolgozás feladat létrehozása

Azure Értékáram-elemzés a legfelső szintű erőforrás egy adatfolyam Analytics feladatot.  Egy vagy több beviteli adatforrásokhoz, a átalakítási kifejező lekérdezés és egy vagy több eredmény írt kimeneti célok áll. Közös ezek engedélyezése a felhasználók a folyamatos átvitelű adatok esetek feldolgozásban adatok elemzések.

Értékáram-elemzés használatának megkezdéséhez: hozzon létre új feladatot megjelenítő Értékáram-elemzés kezdődik.  Ne feledje, hogy ez a művelet nem számlázási következmények mindaddig, amíg a feladat azonnal elindul.

1.  Jelentkezzen be az online [Azure klasszikus portál](http://manage.windowsazure.com) vagy az [Azure-portálon](https://portal.azure.com/).
2.  A portálon: **Kattintson az új gombra**, majd kattintson a **Data Services** vagy az **Adatok Analytics** attól függően, hogy a portálon, és kattintson a **Azure Értékáram-elemzés** vagy **Értékáram-elemzés** , majd **Gyors létrehozása**.

    ![Adatok elemzések feldolgozása feladat varázsló](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Adatok analytics feldolgozás létrehozása](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Adja meg a kívánt konfiguráció, a megjelenítő Értékáram-elemzés projektre vonatkozóan.
    - A **Projekt neve** mezőbe írja be egy nevet, amely azonosítja a Értékáram-elemzés feladatot. Ha a **Projekt neve** érvényesítése, zöld pipa jelzi megjelenik a projekt neve mezőbe. A **Projekt neve** tartalmazhat csak alfanumerikus karaktereket, és a "-" karakter, és a 3 és 63 karakter közé kell lennie.
    - **Régió** az Azure portálon vagy a **hely** az Azure-portálon megadásához használja a földrajzi hely, amelyre a feladat futtatása.
    - Ha használja az Azure-portálra, jelölje be, vagy hozzon létre egy tárhely a **Regionális figyelése tárhelyet fiók**használatához. Ehhez a fiókhoz tároló fut ez a terület összes Értékáram-elemzés feladatok nyomon adatok tárolására szolgál.
    - Ha használja az Azure-portálra, adjon meg egy új vagy meglévő **Erőforráscsoport** tartása az alkalmazáshoz kapcsolódó erőforrásokat.

4.  Amikor az új feladat Értékáram-elemzés beállítások vannak beállítva, kattintson a **Adatfolyam Analytics feladat létrehozása**. Eltarthat néhány percet, a megjelenítő Értékáram-elemzés feladat hozható létre. Az állapot ellenőrzése, hogy figyelheti az értesítések-központban végrehajtását.

    ![Adatok analytics feldolgozás feladat notfications központi](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure portál adatok analytics feldolgozás feladat létrehozása](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Az új feladat **létrehozva**állapotban fog megjelenni. Figyelje meg, hogy a **Start** gomb nem érhető el. A feladat előtt meg kell adnia a feladat beviteli, a lekérdezés és a kimeneti.

    ![Adatok elemzések feldolgozása feladat feladat állapota](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portál adatok analytics feladat feladatállapot feldolgozása](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
