<properties 
    pageTitle="Útmutató: A Microsoft Dynamics CRM az alkalmazás az összefüggéseket figyelése" 
    description="Telemetriai beolvasása a Microsoft Dynamics CRM Online alkalmazás mélyebb használatával. Útmutató: a telepítés adatokat, a képi megjelenítések és az Exportálás első." 
    services="application-insights" 
    documentationCenter=""
    authors="mazharmicrosoft" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="11/17/2015" 
    ms.author="awills"/>
 
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Forgatókönyv: Telemetriai engedélyezése a Microsoft Dynamics CRM Online alkalmazás háttérismeretek használatával

Ez a cikk bemutatja, hogyan szeretné telemetriai adatok beolvasása a [Microsoft Dynamics CRM Online](https://www.dynamics.com/) használja a [Visual Studio alkalmazásban az összefüggéseket](https://azure.microsoft.com/services/application-insights/). Módszeren végigvezetjük alkalmazás háttérismeretek parancsprogram hozzáadása az alkalmazás, a teljes folyamat adatok és adatmegjelenítés rögzítéséhez.

>[AZURE.NOTE] [Keresse meg a minta megoldás](https://dynamicsandappinsights.codeplex.com/).

## <a name="add-application-insights-to-new-or-existing-crm-online-instance"></a>Új vagy meglévő CRM Online példány alkalmazás háttérismeretek hozzáadása 

Az alkalmazás figyelése, vegyen fel egy alkalmazás háttérismeretek SDK az alkalmazás. A SDK telemetriai küld az [alkalmazás háttérismeretek portál](https://portal.azure.com), amelyen a hatékony elemzés és a diagnosztikai eszközök, vagy exportálja az adatokat tároló.

### <a name="create-an-application-insights-resource-in-azure"></a>Hozzon létre egy alkalmazás háttérismeretek erőforrás Azure-ban

1. Ismerkedés a [Microsoft Azure-fiók](http://azure.com/pricing). 
2. Jelentkezzen be az [Azure-portálra](https://portal.azure.com) , és új alkalmazás háttérismeretek erőforrás hozzáadása. Ez a hol az adatok feldolgozott és jelenik meg.

    ![Kattintson a +, fejlesztői szolgáltatások alkalmazás az összefüggéseket.](./media/app-insights-sample-mscrm/01.png)

    ASP.NET válassza az alkalmazás típusa.

3. Nyissa meg az első lépések lapon, és nyissa meg a kód parancsfájl.

    ![](./media/app-insights-sample-mscrm/03.png)

**A kódlapot nyitva tartása** , miközben a következő lépésben egy másik böngészőablakban. Hamarosan kell a kódot. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>A JavaScript webes erőforrás létrehozása a Microsoft Dynamics CRM rendszerben

1. Nyissa meg a CRM Online példány, és jelentkezzen be rendszergazdai jogosultságokkal.
2. Nyissa meg a Microsoft Dynamics CRM beállítások, testreszabás, a rendszer testreszabása

    ![](./media/app-insights-sample-mscrm/04.png)
    
    ![](./media/app-insights-sample-mscrm/05.png)


    ![](./media/app-insights-sample-mscrm/06.png)

3. Hozzon létre egy JavaScript erőforrást.

    ![](./media/app-insights-sample-mscrm/07.png)

    Adja meg egy nevet, jelölje ki a **parancsfájl (JScript)** , és nyissa meg a szövegszerkesztőben.

    ![](./media/app-insights-sample-mscrm/08.png)
    
4. Alkalmazás mélyebb másolja a kódot. Győződjön meg arról, hogy másolása során figyelmen kívül hagyja a parancsprogram címkéket tartalmaznak. Olvassa el a képernyőfelvétel alatt:

    ![](./media/app-insights-sample-mscrm/09.png)

    A kódot a műszerezettségi kulcsot, az alkalmazás mélyebb erőforrás azonosítására szolgáló tartalmazza.

5. Mentés és közzététel gombra.

    ![](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Űrlapok eszköz

1. A Microsoft CRM Online az ügyfél űrlap megnyitása

    ![](./media/app-insights-sample-mscrm/11.png)

2. Nyissa meg az űrlap tulajdonságai

    ![](./media/app-insights-sample-mscrm/12.png)

3. Az Ön által létrehozott JavaScript webes erőforrás hozzáadása

    ![](./media/app-insights-sample-mscrm/13.png)

    ![](./media/app-insights-sample-mscrm/14.png)

4. Mentése és közzététele a űrlap testreszabása.


## <a name="metrics-captured"></a>Rögzített mérőszámok

Most már beállított telemetriai rögzítése a képernyő. Használja, amikor adatokat küld a alkalmazás háttérismeretek erőforrás.

Az alábbiakban a minták, látni fogja az adatok.

#### <a name="application-health"></a>Alkalmazás állapota

![](./media/app-insights-sample-mscrm/15.png)

![](./media/app-insights-sample-mscrm/16.png)

Böngésző kivételeket:

![](./media/app-insights-sample-mscrm/17.png)

Kattintson arra a diagramra, részletesebb megszerezni:

![](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Használat

![](./media/app-insights-sample-mscrm/19.png)

![](./media/app-insights-sample-mscrm/20.png)

![](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Böngészők

![](./media/app-insights-sample-mscrm/22.png)

![](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Feloldás földrajzi helye

![](./media/app-insights-sample-mscrm/24.png)

![](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Sárga ceruzák lap kérelem megtekintése

![](./media/app-insights-sample-mscrm/26.png)

![](./media/app-insights-sample-mscrm/27.png)

![](./media/app-insights-sample-mscrm/28.png)

![](./media/app-insights-sample-mscrm/29.png)

![](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Minta kódot.

[Keresse meg a minta kódot](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>A Power BI

Lehetőség van még mélyebb elemzés, ha [exportálja az adatokat a Microsoft Power bi-bA](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>Példa a Microsoft Dynamics CRM megoldás

[Az alábbiakban a minta megoldást szerepelni fog a Microsoft Dynamics CRM] (https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>tudj meg többet

* [Mi az alkalmazás az összefüggéseket?](app-insights-overview.md)
* [Weblapok alkalmazás Hírcsatornájában](app-insights-javascript.md)
* [További minták és forgatókönyvek](app-insights-code-samples.md)

 
