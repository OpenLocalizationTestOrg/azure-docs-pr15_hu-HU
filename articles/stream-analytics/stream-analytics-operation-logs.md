<properties 
    pageTitle="Hibakeresési művelettel és szolgáltatás naplózza a Értékáram-elemzés |} Microsoft Azure" 
    description="Ennek használata Értékáram-elemzés művelet naplók" 
    keywords="szolgáltatás a naplók"
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

# <a name="debug-stream-analytics-jobs-using-service-and-operation-logs"></a>Értékáram-elemzés feladatok használ szolgáltatási és műveleti naplók hibakeresése

Azure szolgáltatások műveleti naplózási üzeneteket adatkezelési műveletek kapcsolódó rekordok részletei felhasználóknak megadni. Azure Értékáram-elemzés hibakeresés céljából, például a feladat állapota, feladat előrehaladását, ez az információ használható, és hibaüzenetek, hogy a projekt haladásának nyomon követése az idő múlásával a elindíthatja a kimeneti feldolgozása.

## <a name="find-operation-logs-in-the-azure-management-portal"></a>Keresse meg a művelet naplózza az Azure adatkezelési portálon

A művelet naplók kétféleképpen érhető el:  

- Értékáram-elemzés feladat irányítópult  
- Az Azure klasszikus portálon szolgáltatások  

## <a name="dashboard-of-the-stream-analytics-job"></a>Értékáram-elemzés feladat irányítópult

A feladat irányítópult lapon megjelenik a megfelelő naplók Értékáram-elemzés feladat mutató hivatkozást. Ha hivatkozásra kattint, akkor a szűrők oly módon, hogy látható-e az adott feladat legújabb naplók állítja.

  ![Válassza a kezelés szolgáltatásnaplók](./media/stream-analytics-operation-logs/01-stream-analytics-operation-logs.png)  

## <a name="management-services"></a>A szolgáltatások kezelése

A kézi nyissa meg a műveletet naplók Értékáram-elemzés és más szolgáltatásokhoz, az Azure klasszikus portálon:

1.  Kattintson a **Szolgáltatások** az [Azure klasszikus portálon](https://manage.windowsazure.com).
2.  A **Szolgáltatás neve**jelölje ki a **Értékáram-elemzés** **típus** és a feladat nevét.  

  ![Jelölje ki a Értékáram-elemzés](./media/stream-analytics-operation-logs/02-stream-analytics-operation-logs.png)  

## <a name="find-audit-logs-in-the-azure-portal"></a>Keresés a naplókat az Azure-portálon ##

Az Azure-portálon, a megjelenítő Értékáram-elemzés feladathoz működési naplók megkereséséhez kattintson a **Tallózás gombra** , és válassza a **Naplók**.

  ![Azure portál jelölje ki Értékáram-elemzés](./media/stream-analytics-operation-logs/06-stream-analytics-operation-logs.png)  

Ekkor megnyílik a lap összes erőforrás az utóbbi 7 napból eseményeinek megjelenítő előfizetéséhez.  Az események megadása típusú vagy időkereten megtekintéséhez kattintson a **Szűrés** parancs szűrheti.

  ![Azure portál jelölje ki Értékáram-elemzés](./media/stream-analytics-operation-logs/07-stream-analytics-operation-logs.png)  

## <a name="get-log-details"></a>Részletek a napló

Időtartomány és megtekintése a naplókat, a feladat állapota szerint szűrhető.

Az Azure felügyeleti portálon kattintson a kijelölt esemény további adatainak megjelenítéséhez az ablak alján a **Részletek** gombra. 

  ![Válassza a részletek.](./media/stream-analytics-operation-logs/03-stream-analytics-operation-logs.png)  

Az Azure-portálon kattintson a naplóbejegyzés megjelenítéséhez a részletes eseményeket, azon belül.

  ![Azure portálon válassza a részletek](./media/stream-analytics-operation-logs/08-stream-analytics-operation-logs.png)  

Itt a **Részletek** lap megnyitásához az esemény parancsra.

  ![Azure portálon válassza a részletek](./media/stream-analytics-operation-logs/09-stream-analytics-operation-logs.png)  

## <a name="debug-a-failed-job"></a>Sikertelen feladat hibakeresése

Az Azure felügyeleti portálon kattintson a keresés ikonra, és írja be a "nem sikerült". Ez a sikertelen az összes naplók eredményt ad. 

  ![Sikertelen feladat hibakeresése](./media/stream-analytics-operation-logs/04-stream-analytics-operation-logs.png)  

Az Azure-portálon **kritikus** események megtekintéséhez üzenet szintjét szerint is szűrheti.

  ![Azure portál hibakeresési](./media/stream-analytics-operation-logs/10-stream-analytics-operation-logs.png)  

Jelölje ki bármelyik a hibák, és a hiba további információt a **Részletek** gombra.  Közül a hibaüzenetek is részletesen ismertetik a probléma csökkentésében. 

  ![Művelet részletei](./media/stream-analytics-operation-logs/05-stream-analytics-operation-logs.png)  

Abban az esetben, ha kell [ügyfélszolgálatát](https://azure.microsoft.com/support/options/) , vagy adja meg az adatokat a csoportnak a [fórum az MSDN webhelyen](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)keresztül, kérjük, tartsa szem előtt a művelet, konkrétan a **Korrelációs Azonosítót**. 

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
