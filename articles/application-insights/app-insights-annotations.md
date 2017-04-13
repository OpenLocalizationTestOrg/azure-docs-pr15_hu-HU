<properties
    pageTitle="Engedje fel az alkalmazás mélyebb széljegyzeteit |} Microsoft Azure"
    description="Adja hozzá a telepítés, vagy az alkalmazás az összefüggéseket a mértékek explorer diagramok jelölőkkel készíthet."
    services="application-insights"
    documentationCenter=".net"
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/28/2016"
    ms.author="awills"/>

# <a name="release-annotations-in-application-insights"></a>Engedje fel az alkalmazást az összefüggéseket a széljegyzetek

Engedje fel a [Mértékek Explorer](app-insights-metrics-explorer.md) diagramok megjelenítése, ahol telepítette egy új build széljegyzeteket. Azok megkönnyítik az látható, hogy a módosítások volt az alkalmazás teljesítményének hatással. Azok automatikusan létrehozhat a [Visual Studio Team Services rendszer generál](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs), és Ön is [létrehozhatja őket a PowerShell](#create-annotations-from-powershell).

![Példa a széljegyzetek és a kiszolgáló válaszidő látható korrelációs együtt](./media/app-insights-annotations/00.png)

Megjelenés széljegyzetek felhőalapú összeállítása szolgáltatása, majd engedje fel a Visual Studio Team Services szolgáltatás. 

## <a name="install-the-annotations-extension-one-time"></a>A széljegyzetek bővítmény (egyszer) telepítése

Hozhassanak létre a fontos jegyzeteket, kell telepíteni az egyik a sok Team Service bővítmények a a Visual Studio piactéren.

1. Jelentkezzen be a [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) project.
2. A Visual Studio piactéren, az [első megjelenés széljegyzetek kiterjesztése](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), és vegye fel a Team Services-fiókjába.

![At Team Services weblap megnyitása piactér jobb felső. Jelölje ki a képi csapat szolgáltatások, és a Szerkesztés és megjelenés csoportban válassza a lásd: további.](./media/app-insights-annotations/10.png)

Csak kell ehhez egyszer a Visual Studio Team Services fiókjához. Megjelenés széljegyzetek most beállíthatók úgy, hogy minden projekt fiókjában. 

## <a name="get-an-api-key-from-application-insights"></a>Alkalmazás háttérismeretek az API-ja kulcs beolvasása

Minden megjelenés sablon létrehozása a fontos jegyzeteket kívánt ehhez szükséges.


1. Jelentkezzen be a [Microsoft Azure-portálra](https://portal.azure.com) , és nyissa meg az alkalmazást az összefüggéseket erőforrás, hogy az alkalmazás figyeli. (Vagy [Most hozzon létre egy újat](app-insights-overview.md), ha még eddig.)
2. Nyissa meg az **API Access**, és kövesse a **Hírcsatornájában azonosítója**másolatát.

    ![Portal.azure.com nyissa meg az alkalmazást az összefüggéseket erőforrást, és válassza a beállítások lehetőséget. Nyissa meg az API Access. Másolja az alkalmazás azonosítója](./media/app-insights-annotations/20.png)

2. A külön böngészőablakban (vagy hozzon létre) megnyitása a Megjelenés sablont, amely a telepítések kezeli a Visual Studio Team Services. 

    Feladat felvétele, és a menüben válassza az alkalmazás az összefüggéseket megjelenés széljegyzet tevékenység.

    Illessze be az API adatelérési lap oszlopából kimásolt **Azonosítója** .

    ![A Visual Studio Team Services nyissa meg a Megjelenés, jelölje be a Megjelenés definícióját, és válassza a Szerkesztés. Kattintson a feladat hozzáadása, és válassza az alkalmazás Hírcsatornájában megjelenés széljegyzet. Illessze be az alkalmazás Hírcsatornájában azonosítójával.](./media/app-insights-annotations/30.png)

3. Állítsa az **APIKey** mezőt egy változó `$(ApiKey)`.

4. Az API Access lap hozzon létre egy új API kulcsot, és belőle másolatot készíthet.

    ![Kattintson az API Access fel az Azure ablak, API-ja kulcs létrehozása. Ha megjegyzést, írási széljegyzetek ellenőrzése elemre, majd kattintson a kulcs generálása lehetőséget. Másolja az új kulcs.](./media/app-insights-annotations/40.png)

4. Nyissa meg a beállítás lapon a Megjelenés sablon.

    Hozzon létre egy változó definícióját `ApiKey`.

    Illessze be az API-ja kulcs ApiKey változó definition.

    ![A Team Services ablakában válassza a beállítás fülre, majd kattintson a változó hozzáadása. Állítsa be a nevet, ApiKey, és a értékké alakítja, és illessze be az imént létrehozott billentyűt.](./media/app-insights-annotations/50.png)


5. Végezetül **mentése** a Megjelenés definition.

## <a name="create-annotations-from-powershell"></a>Jegyzetek készítése a PowerShell

Létrehozhat széljegyzetek folyamatának szívesen (használata nélkül VIEWBEN Team System) is. 

A [Powershell-parancsprogramot GitHub az](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)első.

Akkor használja, így:

    .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }

Ismerkedés a `applicationId` és egy `apiKey` az alkalmazás mélyebb erőforrás: beállítások megnyitása, API-hozzáférési és másolása az alkalmazás-azonosítóval. Ezután kattintson a API-ja kulcs létrehozása, és másolja a billentyűt. 

## <a name="release-annotations"></a>Engedje fel az széljegyzetek

Ezután a Megjelenés sablon használatával egy új kiadásának telepítéséhez, amikor egy jegyzetet küld alkalmazás mélyebb. A széljegyzetek mértékek Explorer diagramok fog megjelenni.

Kattintson a bármely széljegyzet jelölő kattintva nyissa meg a kibocsátás, többek között a kérési, adatforrás-vezérlő ág részleteit, engedje fel az definícióját, környezet és az egyéb.


![Kattintson a bármely megjelenés széljegyzet jelölőt.](./media/app-insights-annotations/60.png)
