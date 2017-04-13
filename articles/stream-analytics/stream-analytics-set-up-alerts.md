<properties
    pageTitle="Értékáram-elemzés lekérdezések vonatkozó értesítések beállítása |} Microsoft Azure"
    description="Riasztási ismertetése Értékáram-elemzés"
    keywords="értesítések beállítása"
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


# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Értékáram-elemzés Azure feladatok vonatkozó értesítések beállítása

## <a name="introduction-monitor-page"></a>Bevezetés: Monitor lap

Beállíthatja a riasztások figyelmeztetést jelenít meg, amikor egy mérőszám eléri a megadott feltétel.

Ha például "Ha a kimenet események az utolsó 15 percet < 100, értesítő e-mailt küldhet e-mailek azonosító: xyz@company.com”.

Szabályok beállítható úgy mérési módja miatt a portálon keresztül, vagy mutasson az adatokra művelet naplók konfigurált [programozás útján](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) is lehet.

## <a name="set-up-alerts-through-the-azure-classic-portal"></a>Az Azure klasszikus portálon keresztül értesítések beállítása

Kétféleképpen állíthat be értesítéseket az Azure klasszikus portálon:  

1.  A **Monitor** lapján, a megjelenítő Értékáram-elemzés feladat  
2.  A szolgáltatások műveletek bejelentkezve  

## <a name="set-up-alert-through-the-monitor-tab-of-the-job-in-the-portal"></a>A feladat a portálon Monitor lapján keresztül értesítés beállítása

1.  A monitor lapján jelölje be a mérőszám és alján az irányítópulton a **Szabály hozzáadása** gombra, és a szabályok beállítása.  

    ![Irányítópult](./media/stream-analytics-set-up-alerts/01-stream-analytics-set-up-alerts.png)  

2.  A nevét és leírását a figyelmeztetés meghatározása  

    ![Szabály létrehozása](./media/stream-analytics-set-up-alerts/02-stream-analytics-set-up-alerts.png)  

3.  Írja be a küszöbértékek, kiértékelés riasztási ablakban és a műveletek az értesítésre vonatkozóan  

    ![Feltételek megadása](./media/stream-analytics-set-up-alerts/03-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-through-the-operations-logs"></a>A műveletek naplók keresztül értesítések beállítása

1.  Nyissa meg az [Azure klasszikus portál](https://manage.windowsazure.com)szolgáltatások az **értesítések** fülre.  
2.  Kattintson a **szabály hozzáadása**  

    ![Feltétel](./media/stream-analytics-set-up-alerts/04-stream-analytics-set-up-alerts.png)  

3.  Adja meg a nevét és leírását az értesítésre. Jelölje ki a "Értékáram-elemzés" szolgáltatás típusa, valamint a projekt nevét a szolgáltatás neve.  

    ![Értesítés meghatározása](./media/stream-analytics-set-up-alerts/05-stream-analytics-set-up-alerts.png)  

## <a name="set-up-alerts-in-the-azure-portal"></a>Az Azure-portálon értesítések beállítása ##

Az Azure-portálon keresse meg a Értékáram-elemzés feladat érdeklik értesítés küldése, és kattintson a **Figyelés** szakaszra.  A megnyíló **metrikus** lap, a **Hozzáadás értesítés** parancsra.

  ![Azure portal beállítása](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

Nevezze el a szabályt, és válasszon egy leírást, amely megjelenik az értesítő e-mailt.

Ha bejelöli a mértékek feltételt és a mérőszám érték küszöbértékét fogja választhat.

  ![Azure portál választó metrikus](./media/stream-analytics-set-up-alerts/07-stream-analytics-set-up-alerts.png)  

Értesítések beállítása az Azure-portálon részletesebb [fogadási riasztási értesítés](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)jelenik meg.  

## <a name="get-help"></a>Segítség kérése
További segítségért próbálja meg az [Azure-adatfolyam Analytics-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések

- [Azure Értékáram-elemzés – bevezetés](stream-analytics-introduction.md)
- [Értékáram-elemzés Azure használatának első lépései](stream-analytics-get-started.md)
- [Azure Értékáram-elemzés feladatok méretezése](stream-analytics-scale-jobs.md)
- [Azure adatfolyam Analytics lekérdezés nyelvi hivatkozása](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure adatfolyam Analytics kezelése REST API-útmutató](https://msdn.microsoft.com/library/azure/dn835031.aspx)
