<properties 
   pageTitle="Egy felhőalapú szolgáltatásba teljesítményét tesztelve |} Microsoft Azure"
   description="Egy felhőalapú szolgáltatásba, használja a Visual Studio profilert teljesítményének tesztelése"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />


# <a name="testing-the-performance-of-a-cloud-service"></a>Egy felhőalapú szolgáltatásba teljesítményének tesztelése 

##<a name="overview"></a>– Áttekintés

Egy felhőalapú szolgáltatásba teljesítményének tesztelheti az alábbi módon:

- Azure diagnosztika-összehívások és a kapcsolatokkal vonatkozó információk gyűjtését, és tekintse át a webhely statisztikai adatokat megjelenítő hogyan hajtja végre a szolgáltatást a felhasználói szemszögéből használata Első lépések, lásd: [az Azure Felhőszolgáltatások és a virtuális gépeken futó konfigurálása diagnosztika]( http://go.microsoft.com/fwlink/p/?LinkId=623009).

- A Visual Studio profilert segítségével a szolgáltatás működésével számítási tulajdonságát mélyreható elemzést. Ez a témakör leírja, mint a profilert teljesítmény mérjük Azure szolgáltatás fut is használhatja. Teljesítmény mérésére szolgáltatás helyileg egy számítási irányító fut, a profilert használatáról további tudnivalókért lásd [teljesítményét tesztelve egy Azure felhőalapú szolgáltatás helyileg a kiszámítania irányító használata a Visual Studio Profilert](http://go.microsoft.com/fwlink/p/?LinkId=262845).



## <a name="choosing-a-performance-testing-method"></a>A teljesítmény tesztelése módszer kiválasztása

###<a name="use-azure-diagnostics-to-collect"></a>Azure diagnosztika segítségével összegyűjtése:###

- Weblapok vagy szolgáltatásokat, például kérelmek és a kapcsolatokkal statisztikáját.

- Szerepköreiről, például, hogy milyen gyakran szerepkörbe újraindításáig statisztikáját.

- Információ a memóriahasználat, például, hogy mi a szemétgyűjtő idő vagy a memória százalékos teljes futó szerepkörbe készlete.

###<a name="use-the-visual-studio-profiler-to"></a>A Visual Studio profilert, hogy használja:###

- Határozza meg, hogy mely funkciók a legtöbbet a időt vehet igénybe.

- Mérje le mennyi időt, minden olyan végeredményt intenzív program részét elfoglalja.

- Részletes teljesítményjelentéseinek szolgáltatás két verzióinak összehasonlítása

- Memóriafoglalást egyes memória hozzárendelések szintnél részletesebben elemezni.

- Verzió-ellenőrzési problémák többszálas kódban elemezheti.

Ha használja a profilert, összegyűjtheti adatok, egy felhőalapú szolgáltatás futtatásakor helyileg vagy Azure-ban.

###<a name="collect-profiling-data-locally-to"></a>Adatgyűjtés profilkészítési helyben történő:###

- Tesztelje a teljesítmény egy felhőszolgáltatásba, például az adott dolgozó szerepkört, reálisan szimulált terhelést nem igénylő végrehajtása egy részének.

- Egy felhőalapú szolgáltatásba teljesítményének tesztelése önmagában ellenőrzött feltételek.

- Mielőtt beállítaná őket a Azure, tesztelje a egy felhőalapú szolgáltatásba teljesítményét.

- A meglévő telepítések megkímélheti magánjellegű, tesztelje a egy felhőalapú szolgáltatásba teljesítményét.

- A szolgáltatás teljesítményének tesztelése Azure-ban futó díjai nélkül.

###<a name="collect-profiling-data-in-azure-to"></a>Profilkészítési adatgyűjtés az Azure-ban:###

- Tesztelje a szimulált vagy valós terhelés alatt egy felhőalapú szolgáltatásba teljesítményét.

- Ez a témakör későbbi használatával az profilkészítési adatgyűjtés műszerezettségi metódusát.

- A szolgáltatás teljesítményének tesztelése, amikor a szolgáltatás fut gyártási azonos környezetben.

Általában hasonlóan a betöltés próba felhőszolgáltatások a normál vagy a terhelési feltételekkel.

## <a name="profiling-a-cloud-service-in-azure"></a>Adatgyűjtés egy felhőalapú szolgáltatásba, Azure-ban

A felhőalapú szolgáltatás, a Visual Studio közzétételekor profil a szolgáltatás, és adja meg a profilkészítési beállításokat, amelyekkel a kívánt adatokat. A profilkészítési munkamenet szerepkörbe minden példányában el van indítva. Kapcsolatban további információk arról, hogy miként teheti közzé a Visual Studio szolgáltatásban, [közzétételi, ha egy Azure felhőalapú szolgáltatásba, a Visual Studio](https://msdn.microsoft.com/library/azure/ee460772.aspx).

További információ a teljesítmény adatgyűjtés a Visual Studióban, című cikkben talál részletes [Útmutató kezdők teljesítmény adatainak összegyűjtése](https://msdn.microsoft.com/library/azure/ms182372.aspx) és [Alkalmazás teljesítmény elemzése adatgyűjtés eszközök segítségével](https://msdn.microsoft.com/library/azure/z9z62c29.aspx).

>[AZURE.NOTE] Lehetősége van engedélyezni a IntelliTrace vagy adatainak összegyűjtése a felhőalapú szolgáltatás közzétételekor. Mindkettő nem engedélyezett.

###<a name="profiler-collection-methods"></a>Profilert webhelycsoportok módszerek

A webhelycsoport különböző módszereket használható adatgyűjtés, a teljesítménnyel kapcsolatos problémákat alapján:

- **Processzor mintavételnél** – ezzel a módszerrel gyűjti össze, amelyek hasznos Processzor kihasználtsági problémák kezdeti elemzésének alkalmazás statisztika. Processzor mintavételnél a javasolt lehetőség a legtöbb teljesítmény vizsgálat indításához. Az alkalmazást, amikor Ön adatgyűjtés Processzor mintavételnél adatainak összegyűjtése alacsonyabb hatással van.

- **Műszerezettségi** – Ez a módszer gyűjti össze, amely hasznos lehet a fókuszban lévő elemzésre és elemzéséhez bemeneti és kimeneti teljesítménnyel kapcsolatos problémák részletes adatokat. A műszerezettségi módszer rekordok minden bejegyzést, kilépés, és a függvény a függvények modulban adatgyűjtés futtatása során. Ez a módszer akkor lehet hasznos, a kód egy részét részletes adatokat gyűjt és a bemeneti és kimeneti műveletek alkalmazás teljesítményre gyakorolt hatásának ismertetése. Ez a módszer le van tiltva a 32 bites operációs rendszert futtató számítógép. Ez a beállítás csak akkor, ha futtatja a felhőbeli szolgáltatástól Azure,-ban nem helyileg a számítási irányító érhető el.

- **.NET memóriafoglalást** – ezzel a módszerrel .NET-keretrendszer memória terhelés adatok a módszer adatgyűjtés mintavételnél használatával gyűjti össze. Gyűjtött adatokat tartalmaz, a szám és a felosztott objektumok méretét.

- **Feldolgozási** – ezzel a módszerrel a erőforrás kérelem adatok és folyamatok és szálak adatvégrehajtás-adatok elemzése a több szálon történő és több folyamat alkalmazások hasznos gyűjti össze. Verzió-ellenőrzési módszer által gyűjtött adatokat az egyes események, hogy a kód, például amikor egy szál megvárja, amíg blokkok végrehajtását zárolt az access-alkalmazás erőforrást kell választani. Ez a módszer akkor lehet hasznos több szálon történő alkalmazások elemzéséhez használható.

- **Réteg kapcsolati adatainak összegyűjtése**, amely biztosítja a függvények kommunikálni egy vagy több adatbázis több többszintű alkalmazások felhívja szinkron ADO.NET adatvégrehajtás időpontok további információt is engedélyezheti. Réteg kapcsolati adatok profilkészítési módszerek bármelyikével összegyűjtheti. További információt a réteg kapcsolati adatainak összegyűjtése megtekintése [réteg találja](https://msdn.microsoft.com/library/azure/dd557764.aspx).

## <a name="configuring-profiling-settings"></a>Profilkészítési beállításainak konfigurálása

Az alábbi ábra mutatja az Azure-alkalmazások közzététele párbeszédpanelen a profilkészítési beállításainak konfigurálása.

![Adatgyűjtés beállításainak konfigurálása](./media/vs-azure-tools-performance-profiling-cloud-services/IC526984.png)

>[AZURE.NOTE] Ha engedélyezni szeretné a **adatgyűjtés engedélyezése** jelölőnégyzetet, a profilert, tegye közzé a felhőalapú szolgáltatást használó helyi számítógépen telepítve kell rendelkeznie. Alapértelmezés szerint a profilert telepítve van a Visual Studio telepítésekor.

### <a name="to-configure-profiling-settings"></a>Profilkészítési beállítások konfigurálása

1. A megoldás Intézőben nyissa meg a helyi menü a Azure projekthez, és válassza a **Közzététel**gombra. A lépések részletes leírását a arról, hogy miként teheti közzé egy felhőalapú szolgáltatásba olvassa el a [közzétételi egy felhőalapú szolgáltatás, az Azure eszközeivel](http://go.microsoft.com/fwlink/p?LinkId=623012)című témakört.

1. **Azure-alkalmazások közzététele** párbeszédpanelen kattintson a **Speciális beállítások** lapon.

1. Adatgyűjtés engedélyezéséhez jelölje be a **adatgyűjtés engedélyezése** jelölőnégyzetet.

1. A profilkészítési beállításainak megadásához válassza a **Beállítások** hivatkozásra. Megjelenik a adatgyűjtés beállításai párbeszédpanel.

1. **Adatgyűjtés milyen módon szeretné használni** a beállítás gombok közül válassza ki a adatainak összegyűjtése, hogy szüksége van.

1. Az adatgyűjtési réteg kapcsolati profilkészítési, jelölje be a **Engedélyezése réteg kapcsolati adatainak összegyűjtése** jelölőnégyzetet.

1. A beállítások mentéséhez válassza az **OK** gombra.

    Ez az alkalmazás közzétételekor ezeket a beállításokat a profilkészítési minden szerepkör-munkamenet létrehozásához használt.

## <a name="viewing-profiling-reports"></a>Adatgyűjtés jelentések megtekintése

A profilkészítési munkamenet létrehozása a szerepkör a felhőszolgáltatásában minden példányában. A Visual Studio minden munkamenet profilkészítési jelentések megtekintéséhez megtekintése a kiszolgáló Intézőt, és válassza a szerepkörbe egy példányának jelölje ki a Azure kiszámítania csomópontot. Ezután megtekintheti a profilkészítési jelentést, az alábbi ábrán látható módon.

![Adatgyűjtés az Azure jelentés megtekintése](./media/vs-azure-tools-performance-profiling-cloud-services/IC748914.png)

### <a name="to-view-profiling-reports"></a>Profilkészítési jelentések

1. Kiszolgáló Intéző ablakának megjelenítése a Visual Studióban, a menüsávon kattintson a nézet, Server Explorer.

1. Válassza az Azure kiszámítania csomópontot, és válassza a az Azure környezetben csomópont a profil Visual Studio közzétételekor kijelölt felhőalapú szolgáltatást.

1. Egy példány profilkészítési jelentések megtekintéséhez válassza a szerepkör a szolgáltatásban, nyissa meg a helyi menü egy adott példányhoz tartozó, és válassza a **Adatgyűjtés jelentés megtekintése**parancsra.

    A jelentést, a .vsp fájlok letöltése most az Azure, és a letöltés állapotát az Azure tevékenységnapló jelenik meg. A letöltés befejeztével megjelenik-e a profilkészítési jelentés egy lapon a szerkesztőben for Visual Studio nevű <Role name> _<Instance Number>_ <identifier>.vsp. Megjelenik a jelentés összesített adatai.

1. A jelentés különböző nézeteinek megjelenítéséhez a jelenlegi nézet listában válassza ki a kívánt nézetre. További tudnivalókért olvassa el a [Eszközök jelentési nézetekkel adatainak összegyűjtése](https://msdn.microsoft.com/library/azure/bb385755.aspx)című témakört.

## <a name="next-steps"></a>Következő lépések

[Hibakeresési Cloud Services](https://msdn.microsoft.com/library/azure/ee405479.aspx)

[A Visual Studio-Azure Felhőszolgáltatásba közzététele](https://msdn.microsoft.com/library/azure/ee460772.aspx)

