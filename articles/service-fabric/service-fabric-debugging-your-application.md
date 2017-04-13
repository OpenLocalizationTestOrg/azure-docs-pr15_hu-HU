<properties
   pageTitle="Az alkalmazás a Visual Studióban hibakeresése |} Microsoft Azure"
   description="Fejlesztési és a helyi fejlesztési fürthöz hibakeresési őket a Visual Studio növelheti megbízhatóságának és teljesítményének a szolgáltatások."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/21/2016"
   ms.author="vturecek;mikhegn"/>

# <a name="debug-your-service-fabric-application-by-using-visual-studio"></a>A szolgáltatás háló alkalmazás hibakeresése Visual Studio segítségével

## <a name="debug-a-local-service-fabric-application"></a>A helyi szolgáltatás háló alkalmazás hibakeresése

Időt és pénzt takaríthat telepítésével és hibakeresés egy helyi számítógép fejlesztési fürt az Azure Service háló alkalmazást. Visual Studio telepítheti az alkalmazást a helyi fürthöz és a debugger automatikusan csatlakozzon az alkalmazás az összes előfordulását.

1. Indítsa el a helyi fejlesztési fürtre [a szolgáltatás háló fejlesztői környezet beállítása](service-fabric-get-started.md)című témakör lépéseit követve.

2. Nyomja le az **F5 billentyűt** vagy kattintson a **hibakeresési** > **Indítása hibakeresése során**.

    ![Az alkalmazások hibakeresés][startdebugging]

3. A **hibakeresési** menüben található parancsokra kattintva beállíthatja a kódot, és lépés az alkalmazáson keresztül töréspontok.

    > [AZURE.NOTE] Visual Studio csatolja az alkalmazás az összes példányát. Kód éppen, miközben töréspontok több folyamat egyidejű munkamenetek így előfordulhat, hogy első találati. Próbálja meg, a töréspontok letiltása, után azok van találat, azáltal, hogy az egyes töréspont azonosítója szál a feltételes vagy a diagnosztikai események használatával.

4. A **Diagnosztikai események** ablak nyílnak meg automatikusan diagnosztikai események megjeleníthet a lapra.

    ![Valós idejű diagnosztikai események megtekintése][diagnosticevents]

5. A **Diagnosztikai események** ablak az a felhő Intézőben is megnyithatja.  A **Szolgáltatás háló**kattintson a jobb gombbal bármelyik csomópontot, és válassza a **Nézet a folyamatos átvitelű nyomkövetések**.

    ![A diagnosztikai események ablak megnyitása][viewdiagnosticevents]

    Ha szeretné szűrni a nyomkövetési naplók, hogy egy adott szolgáltatásból vagy alkalmazásból, egyszerűen engedélyezése adatfolyam halad meg, hogy adott szolgáltatásból vagy alkalmazásból.

6. A diagnosztikai események az automatikusan generált **ServiceEventSource.cs** fájlban is láthatja, és az alkalmazás kódja úgynevezett.

    ```csharp
    ServiceEventSource.Current.ServiceMessage(this, "My ServiceMessage with a parameter {0}", result.Value.ToString());
    ```

7. A **Diagnosztikai események** ablak támogatja a szűrés, szüneteltetése és a valós idejű események vizsgálata.  A szűrő egy egyszerű karakterlánc keresést az esemény üzenet, beleértve a tartalmát.

    ![Szűrése, mutasson az egérrel, és folytathatja és események valós idejű vizsgálata][diagnosticeventsactions]

8. Hibakeresési szolgáltatások olyan, mint bármely más alkalmazásban. A szokásos módon töréspontok Visual Studio segítségével egyszerűen hibakereséshez állítja. Annak ellenére, hogy megbízható gyűjtemények több csomópontok közötti replikáció, azok is megvalósítása IEnumerable. Ez azt jelenti, hogy a is használhatja az eredmények megjelenítése a Visual Studio hibakeresése során belül már tárolt megjelenítéséhez. Egyszerűen beállításával töréspont bárhol a kódot.

    ![Az alkalmazások hibakeresés][breakpoint]

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="debug-a-remote-service-fabric-application"></a>A távoli szolgáltatás háló alkalmazás hibakeresése

Ha a szolgáltatás háló alkalmazások futnak a szolgáltatás háló fürthöz Azure-ban, tudunk távolról hibakeresési ezek közvetlenül a Visual Studio.

> [AZURE.NOTE] A szolgáltatás [szolgáltatás háló SDK 2.0-s](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/)szükséges.    

<!-- -->
> [AZURE.WARNING] Távoli hibakeresés célja a fejlesztők/vizsgálatot felhasználási területei, és nem használandó éles környezetben miatt futó alkalmazást gyakorolt hatását.

1. Nyissa meg a **Felhőben**Explorer fürt, kattintson a jobb gombbal, és válassza a **Hibakereséshez engedélyezése**

    ![Távoli hibakeresés engedélyezése][enableremotedebugging]

    Ez lesz kikapcsolás kikapcsolási folyamata engedélyezése a távoli hibakeresési bővítmény a fürt csomópontok, valamint a szükséges hálózati beállítások.

2. Kattintson a jobb gombbal a **Felhőben Intézőben**csomópont, és válassza a **Debugger csatolása**

    ![Debugger csatolása][attachdebugger]

3. **Feldolgozási csatolása** párbeszédpanelen válassza ki a szeretné hibakeresése folyamatot, és kattintson a **Csatolás** gombra

    ![Válassza a folyamat][chooseprocess]

    A folyamatot, csatolni kívánt neve megegyezik az a szolgáltatás project összeállítás nevére.

    A debugger futtatása csomópontjait fog csatolhat.
    - Abban az esetben, ha hibakeresés alatt állapot nélküli szolgáltatás a szolgáltatást a csomópontjait összes előfordulását részei a hibakeresési munkamenetet.
    - Állapot-nyilvántartó szolgáltatás vannak hibakeresési, ha csak az elsődleges replika bármely partíciót lesz, az aktív, és ezért kifogott való a. Ha az elsődleges replika hibakeresési munkamenetben helyez át, adott replika feldolgozása továbbra is a hibakeresési munkamenet részét.
    - Annak érdekében, hogy csak a releváns partíciók, illetve egy adott szolgáltatás példányát elfog, feltételes töréspontok csak bonthatja a egy adott partíciót vagy példány is használhatja.

    ![Feltételes töréspont][conditionalbreakpoint]

    > [AZURE.NOTE] A szolgáltatás végrehajtható néven több példányának egy szolgáltatás háló fürtre hibakeresési jelenleg nem támogatjuk.

4. Miután végzett a alkalmazásban, a távoli hibakeresési bővítmény letilthatja kattintson a jobb gombbal a **Felhőben** Explorer fürt, és válassza a **Hibakereséshez letiltása**

    ![Távoli hibakeresés letiltása][disableremotedebugging]

## <a name="streaming-traces-from-a-remote-cluster-node"></a>A folyamatos átvitelű távoli csomópont a nyomkövetési naplók

Is képes adatfolyam halad közvetlenül a Visual Studio távoli fürthöz csomópontot. Ez a szolgáltatás lehetővé teszi adatfolyam esemény-nyomkövetés nyomkövetési események csomóponton egy szolgáltatás háló, közvetlenül a Visual Studióban darab termék készült.

> [AZURE.NOTE] A szolgáltatás [szolgáltatás háló SDK 2.0-s](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015) és [Azure SDK.NET 2.9](https://azure.microsoft.com/downloads/)szükséges.

<!-- -->
> [AZURE.WARNING]A folyamatos átvitelű halad célja a fejlesztők/vizsgálatot felhasználási területei, és nem használandó éles környezetben futó alkalmazást hatással miatt.
> Egy gyártási esetben célszerű a használja arra, Azure diagnosztika segítségével események továbbítása.

1. Nyissa meg a **Felhőben**Explorer fürt, kattintson a jobb gombbal, és válassza a **Folyamatos átvitelű halad engedélyezése**

    ![Távoli adatfolyam halad engedélyezése][enablestreamingtraces]

    Ez lesz kikapcsolás kikapcsolási adatfolyam a nyomkövetés-bővítmény a fürt csomópontok, valamint a szükséges hálózati beállítások engedélyezése folyamatát.

2. Bontsa ki az a **Felhő Explorer** **csomópontok** elemét, kattintson a jobb gombbal a csomópontra a nyomkövetés-adatfolyam, és válassza a **Nézet a folyamatos átvitelű halad**

    ![Távoli streaming nyomkövetési naplók megtekintése][viewremotestreamingtraces]

    Meg szeretné jeleníteni a nyomkövetési naplók csomópont esetében ismételje meg a 2. Egyes csomópontok adatfolyam megjelenik a saját ablakában.

    Most már a szolgáltatás háló, és a szolgáltatások által kibocsátott halad látja. Ha szeretne szűrni, az események csak az adott alkalmazás megjelenítéséhez, egyszerűen írja be az alkalmazás a szűrő neve.

    ![A folyamatos átvitelű nyomkövetési naplók megtekintése][viewingstreamingtraces]

4. Ha adatfolyam halad a fürt végzett, tiltsa le a távoli adatfolyam halad, kattintson a jobb gombbal a **Felhőben** Explorer fürt, és **Letiltja a folyamatos átvitelű halad** kiválasztása

    ![Távoli adatfolyam halad letiltása][disablestreamingtraces]

## <a name="next-steps"></a>Következő lépések

- A [próba szolgáltatás háló szolgáltatás](service-fabric-testability-overview.md).
- [A Visual Studióban a szolgáltatás háló alkalmazások kezelése](service-fabric-manage-application-in-visual-studio.md)parancsra.

<!--Image references-->
[startdebugging]: ./media/service-fabric-debugging-your-application/startdebugging.png
[diagnosticevents]: ./media/service-fabric-debugging-your-application/diagnosticevents.png
[viewdiagnosticevents]: ./media/service-fabric-debugging-your-application/viewdiagnosticevents.png
[diagnosticeventsactions]: ./media/service-fabric-debugging-your-application/diagnosticeventsactions.png
[breakpoint]: ./media/service-fabric-debugging-your-application/breakpoint.png
[enableremotedebugging]: ./media/service-fabric-debugging-your-application/enableremotedebugging.png
[attachdebugger]: ./media/service-fabric-debugging-your-application/attachdebugger.png
[chooseprocess]: ./media/service-fabric-debugging-your-application/chooseprocess.png
[conditionalbreakpoint]: ./media/service-fabric-debugging-your-application/conditionalbreakpoint.png
[disableremotedebugging]: ./media/service-fabric-debugging-your-application/disableremotedebugging.png
[enablestreamingtraces]: ./media/service-fabric-debugging-your-application/enablestreamingtraces.png
[viewingstreamingtraces]: ./media/service-fabric-debugging-your-application/viewingstreamingtraces.png
[viewremotestreamingtraces]: ./media/service-fabric-debugging-your-application/viewremotestreamingtraces.png
[disablestreamingtraces]: ./media/service-fabric-debugging-your-application/disablestreamingtraces.png
