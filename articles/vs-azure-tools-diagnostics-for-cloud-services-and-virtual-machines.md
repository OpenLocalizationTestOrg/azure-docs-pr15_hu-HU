<properties
   pageTitle="Azure Felhőszolgáltatások és a virtuális gépeken futó diagnosztika konfigurálását |} Microsoft Azure"
   description="Diagnosztikai információ Azure cloude szolgáltatások és a virtuális gépeken futó (VMs) a Visual Studióban hibakereséshez konfigurálása ismerteti."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="configuring-diagnostics-for-azure-cloud-services-and-virtual-machines"></a>Azure Felhőszolgáltatások és a virtuális gépeken futó diagnosztika konfigurálása

Ha egy Azure felhőalapú szolgáltatásba vagy az Azure virtuális gép hibaelhárítása van szüksége, beállíthatja Azure diagnosztika könnyebben Visual Studio segítségével. Azure diagnosztika rögzíti a rendszer és a virtuális gépeken futó és a virtuális gép példányai, amelyek a felhőszolgáltatásában futtassa a naplózás adatokat, és továbbítja az adatokat egy tetszés szerinti tárterület-fiókba. Lásd: a [diagnosztikai naplózás web Apps alkalmazások Azure alkalmazás szolgáltatás engedélyezése](./app-service-web/web-sites-enable-diagnostic-log.md) további információt a diagnosztikai naplózás Azure-ban.

Ez a témakör bemutatja, hogyan engedélyezése és konfigurálása az Azure diagnosztika, és a Visual Studióban, mindkét előtt és után példányban egyaránt Azure virtuális gépeken futó. Azt is megjelennek a diagnosztikai adatok összegyűjtése típusú kijelölése és hogy miként tekintheti meg az információkat gyűjtött azt követően.

Azure diagnosztika az alábbi módon adhatja meg:

- Módosíthatja a diagnosztikai beállítások a **Diagnosztika konfigurációs** párbeszédpanel a Visual Studióban keresztül. A beállítások mentése diagnostics.wadcfgx (az Azure SDK 2.4 vagy korábbi verziójában diagnostics.wadcfg) nevű fájlban. Másik lehetőségként közvetlenül módosíthatók a konfigurációs fájl. Ha kézzel frissíti a fájlt, a konfigurációs módosítások lép érvénybe, hogy a következő alkalommal telepíti a felhőben az Azure szolgáltatás, és a szolgáltatás a irányító Futtatás.

- **Felhőalapú Intéző** vagy **Server Explorer** használata a Visual Studióban futó felhőalapú szolgáltatás vagy virtuális gép diagnosztika beállításainak módosítása.

## <a name="azure-26-diagnostics-changes"></a>Azure 2.6 diagnosztika módosítása

Azure SDK 2.6 projektekhez a Visual Studióban az alábbi módosításokat végzett. (A módosítások vonatkoznak az Azure SDK újabb verzióiban.)

- A helyi irányító diagnosztika támogatja. Ez azt jelenti, hogy diagnosztika adatgyűjtés, és győződjön meg róla, az alkalmazás hoz létre a jobb oldali halad, miközben Ön fejlesztése, és a Visual Studio alkalmazásban tesztelje. A kapcsolati karakterlánc `UseDevelopmentStorage=true` lehetővé teszi, hogy diagnosztika adatgyűjtés futtatása közben a felhőalapú szolgáltatás project a Visual Studióban az Azure tároló irányító használatával. Az összes diagnosztika adatgyűjtés tároló fiók (Development tároló).

- A szolgáltatás beállítása (.cscfg) fájlt a diagnosztika tároló fiók kapcsolati karakterlánc (Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString) még egyszer vannak tárolva. Az Azure SDK 2,5 megadva a diagnosztika tárterület-fiók a diagnostics.wadcfgx fájlt.

Hogyan a kapcsolati karakterlánc művelet sikerült, az Azure SDK 2.4 és a korábbi verziók, és hogy hogyan működik az Azure SDK 2.6 és újabb verzióiban között néhány főbb különbségek vannak.

- Az Azure SDK 2.4 és korábbi verzióiban a kapcsolati karakterlánc használtak, egy futtatókörnyezet a diagnosztika beépülő modul beszerzése a tárterület-fiók adatait a diagnosztikai naplók átviteléhez.

- Az Azure SDK 2.6 és újabb verzióiban a diagnosztikai kapcsolati karakterlánc segítségével Visual Studio a diagnosztika bővítmény konfigurálása a megfelelő tárolási fiókadatok közzététel során. A kapcsolati karakterlánc meghatározhatja, hogy más-más tárolási fiókok másik szolgáltatáscsaládba konfigurációk, amelyekkel a Visual Studio közzétételekor. A diagnosztikai beépülő modul már nem érhető el (után Azure SDK 2,5), mivel azonban a .cscfg fájl saját maga által engedélyezése nem a diagnosztika bővítmény. Engedélyeznie kell a bővítmény külön-külön eszközök, például a Visual Studio vagy PowerShell keresztül.

- A diagnosztikai kiterjesztése a PowerShell egyszerűbbé a csomag kimeneti Visual Studio minden szerepkörhöz diagnosztika bővítményhez nyilvános konfigurációs XML is tartalmaz. Visual Studio a diagnosztika kapcsolati karakterláncot a tárhely fiók adatait a nyilvános konfigurációs bemutató feltöltéséhez használja. A nyilvános config fájlok létrehozásakor a bővítmények mappára a, és kövesse a mintázat PaaSDiagnostics. &lt;RoleName >. PubConfig.xml. Bármely PowerShell-alapú telepítés segítségével a minta megfeleltetése egyes kereséskonfigurációs szerepkörbe.

- A kapcsolati karakterláncot az .cscfg fájlban is használják az [Azure portál](http://go.microsoft.com/fwlink/p/?LinkID=525040) úgy is jelennek meg a **Figyelés** lapon a diagnosztikai adatok eléréséhez. A részletes felügyeleti adatok megjelenítéséhez a portálon szolgáltatás konfigurálása a kapcsolati karakterlánc van szükség.

## <a name="migrating-projects-to-azure-sdk-26-and-later"></a>Azure SDK 2.6 és újabb verzióiban áttelepítése projektek

Fiókneveket Azure SDK 2,5 Azure SDK 2.6 vagy újabb, ha egy .wadcfgx fájl a megadott diagnosztika tárterület-fiókot, majd marad van. Más-más tárolási fiókok különböző tároló konfigurációk rugalmasan előnyeit fognak rendelkezésére állni a kapcsolati karakterlánc manuális hozzáadása a projekthez. Ha a projekt éppen áttelepítette, Azure SDK 2.4 vagy korábbi verzióról Azure SDK 2.6, a diagnosztikai a csatlakozási_karakterlánc megőrződnek. Jó helyen jár tartsa szem előtt a változások hogyan csatlakozási_karakterlánc számít Azure SDK 2.6 adott az előző szakaszban.

### <a name="how-visual-studio-determines-the-diagnostics-storage-account"></a>Hogyan Visual Studio határozza meg, hogy a diagnosztika tárterület-fiók

- Ha a .cscfg fájlban diagnosztika kapcsolati karakterlánc megadott, Visual Studio használja konfigurálása a diagnosztika bővítmény, közzététele, valamint a nyilvános konfigurációs XML-fájlok létrehozása során összecsomagolása esetén.

- Ha nincs diagnosztika kapcsolati karakterláncot az .cscfg fájlban megadva, majd a Visual Studio visszavált fiókkal a tárterület a .wadcfgx fájl a megadott konfigurálása a diagnosztika bővítmény, amikor közzététele, valamint a nyilvános konfigurációs XML-fájlok létrehozása, amikor csomagolása.

- A diagnosztikai kapcsolati karakterláncot az .cscfg fájlban bontásakor a tárterület-fiók a .wadcfgx fájlt. Ha diagnosztika kapcsolati karakterlánc megadva a .cscfg fájlban, Visual Studio jeleníti meg, és figyelmen kívül hagyja a .wadcfgx a tárterület-fiók.

### <a name="what-does-the-update-development-storage-connection-strings-checkbox-do"></a>Mit jelent a "frissítés fejlesztési tárhely a csatlakozási_karakterlánc..." jelölőnégyzet tenni?

A **frissítés fejlesztési tároló kapcsolat** karakterláncok diagnosztika és a Microsoft Azure tároló fiók hitelesítő adatait, a Microsoft Azure való közzétételkor gyorsítótárazás jelölőnégyzetet bármely fejlesztési tároló fiók csatlakozási_karakterlánc frissítse a közzététel során megadott Azure tároló fiók kényelmesen biztosít.

Tegyük fel például, hogy, jelölje be ezt a jelölőnégyzetet, és a diagnosztikai kapcsolati karakterlánc megadja `UseDevelopmentStorage=true`. Azure tegye közzé a projektet, amikor a Visual Studio automatikusan frissíti a diagnosztika kapcsolati karakterláncot a tárterület-fióknak a közzétételi varázsló megadott. Azonban, ha egy valós tárhely a diagnosztika csatlakozási karakterlánc lett megadva, majd a fiók használja helyette.

## <a name="diagnostics-functionality-differences-between-azure-sdk-24-and-earlier-and-azure-sdk-25-and-later"></a>Diagnosztikai funkció eltérések Azure SDK 2.4 és korábbi és Azure SDK 2.5-ös és újabb verziók

Ha frissít a projekt az Azure SDK 2.4 Azure SDK 2.5-ös vagy újabb verziójával, akkor tudatában kell lennie, az alábbi diagnosztikai funkciók különbségeket.

- **Konfigurációs API -khoz is elavult** – diagnosztika programozott konfigurációs Azure SDK 2.4 és korábbi verzióiban érhető el, de az Azure SDK 2.5-ös és újabb verzióiban elavult. Ha a diagnosztikai konfigurációja kód jelenleg definiált kell újrakonfigurálni ezeket a beállításokat az alapoktól az áttelepített projektben ahhoz, hogy diagnosztika folytathatja a munkát a. A diagnosztikai konfigurációs fájl az Azure SDK 2.4 diagnostics.wadcfg, de az Azure SDK 2.5-ös és újabb verzióiban diagnostics.wadcfgx.

- **Az felhő szolgáltatásalkalmazásokat diagnosztika csak szerepkört szintre, de a példány szintjén nem állítható be.**

- **Minden alkalommal telepíti az alkalmazást, a diagnosztikai konfigurációja frissül** – eltérés áll fenn problémák léphetnek fel, ha a diagnosztikai konfigurációja módosítása a kiszolgáló Intézőből, és ezután telepítsen újra az alkalmazást.

- **Az Azure SDK 2.5-ös és újabb, összeomlik kiírása van konfigurálva, a diagnosztikai konfigurációs fájl nem található a** – ha van konfigurálva a kód összeomlik kiírása, is be kell manuálisan át a konfigurációs kódot, hogy a konfigurációs fájl, mert az összeomlást kiírása Azure SDK 2.6 az áttelepítés során nem át.

## <a name="enable-diagnostics-in-cloud-service-projects-before-deploying-them"></a>A felhőalapú szolgáltatás projektek diagnosztika engedélyezése a központi telepítés előtt

A Visual Studióban megadhatja, az Azure, ha a szolgáltatás futtatása előtt üzembe helyezése a irányító szerepkörök diagnosztika adatokat szeretne gyűjteni. A Visual Studióban diagnosztika beállításainak minden módosítása mentve legyen a diagnostics.wadcfgx konfigurációs fájl. A konfigurációs beállítások adja meg a tárterület-fiók diagnosztika adatokat tároló a felhőalapú szolgáltatás telepítésekor.

### <a name="to-enable-diagnostics-in-visual-studio-before-deployment"></a>Ahhoz, hogy diagnosztika Visual Studio a telepítés előtt

1. Az Önt érdeklő szerepkör helyi menüben válassza a **Tulajdonságok parancsot**, és válassza a **beállítás** lapon a szerepkör a **Tulajdonságok** ablak a.

1. A **diagnosztikai** csoportban győződjön meg arról, hogy a **Diagnosztika engedélyezése** jelölőnégyzet be van-e jelölve.

    ![A diagnosztikai engedélyezése beállítás elérése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796660.png)

1. A három pontra (…) gombra kattintva adja meg a tárterület-fiókot, amelyhez a diagnosztikai adatokat tárolja. A kiválasztott tároló fiók diagnosztikai adatok tárolási helye lesz.

    ![Adja meg a tárterület-fiók használata](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796661.png)

1. A **Tárhely kapcsolati karakterlánc létrehozása** párbeszédpanelen adja meg, hogy szeretne összekapcsolni, az Azure tároló irányító egy Azure-előfizetést használ, vagy manuálisan kell megadni a hitelesítő adatait.

    ![Tárterület-fiók párbeszédpanelen](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796662.png)

  - Ha úgy dönt, hogy a Microsoft Azure tároló irányító beállítást választja, a kapcsolati karakterlánc-e beállítva a UseDevelopmentStorage = true.

  - Ha úgy dönt, a az előfizetés beállítás megadhatja a használni kívánt Azure előfizetést és a fiók nevét. Megadhatja, hogy a fiókok kezelése gomb az Azure előfizetések kezelése.

  - Ha úgy dönt, hogy a kézzel megadott hitelesítő adatok beállítás, rákérdez, hogy meg szeretné használni kívánt Azure-fiók kulcsa és nevét.

1. Válassza a **beállítás** gombra a **Diagnosztika konfigurációs** párbeszédpanel megjelenítéséhez. Minden egyes lap (kivéve **az általános** és a **Napló könyvtárak**) összegyűjtheti diagnosztikai adatforrás jelöli. Az alapértelmezett lapot, az **Általános**kínál az alábbi diagnosztikai végzett adatgyűjtés beállításai: **csak a hibák**, a **minden információt**és az **egyéni terv**. Az alapértelmezett beállítást, **csak a hibák**, megnyitja a tárhely a legkevesebb, mert azt nem nem ruházhatja át figyelmeztetések vagy nyomkövetési üzeneteket. Az összes adatot beállítást a legtöbb információ át, és ezért, tárterületet pedig a legtöbb költséges lehetőséget.

    ![Azure diagnosztikai és konfigurációs engedélyezése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

1. Ebben a példában a beállítással az **egyéni terv** , testre szabhatja a gyűjtött adatok.

1. A **Lemez kvóta MB** mezőben megadhatja a szabad terület hozzárendelni kívánt a tárterület-fiókjában diagnosztikai adatok. Ha azt szeretné, módosíthatja az alapértelmezett értéket.

1. Diagnosztikai adatok gyűjtendő egyes lapon jelölje be a **engedélyezése átadása <log type> ** jelölőnégyzetet. Például alkalmazás naplógyűjtés szeretné, ha az **alkalmazás naplók átadása engedélyezése** jelölőnégyzetet, az **Alkalmazás naplók** lapon jelölje be. Is adja meg az egyes diagnosztika adattípus által igényelt igény szerint további információkat. Című **konfigurálása diagnosztika adatforrásokat** a konfigurációs adatok a témakör későbbi az egyes lapokon.

1. Miután engedélyezte a kívánt diagnosztikai adatok gyűjteménye, válassza az **OK** gombra.

1. Futtassa az Azure felhőalapú szolgáltatás project a Visual Studióban, a szokásos módon. Az alkalmazás használata során, a adatok, ha engedélyezte a megadott Azure tároló fiók menti.

## <a name="enable-diagnostics-in-azure-virtual-machines"></a>Azure virtuális gépeken futó diagnosztika engedélyezése

A Visual Studióban megadhatja az Azure virtuális gépeken futó diagnosztika adatokat szeretne gyűjteni.

### <a name="to-enable-diagnostics-in-azure-virtual-machines"></a>Azure virtuális gépeken futó diagnosztika engedélyezése

1. A **Kiszolgáló Explorerben**válassza az Azure csomópontot, és csatlakoztassa a Azure-előfizetéséhez, gombra, ha még nem csatlakozott.

1. Bontsa ki a **virtuális gépeken futó** csomópontot. Hozzon létre egy új virtuális számítógépre, vagy jelöljön ki egy már telepítve van.

1. Válassza a helyi menü az Önt érdeklő virtuális gép **konfigurálása**. Ezzel jeleníti meg az virtuális gép beállításai párbeszédpanelen.

    ![Az Azure virtuális gép konfigurálása](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796663.png)

1. Ha még nincs telepítve, a Microsoft Agent figyelése diagnosztika bővítmény hozzáadása a. Az Azure virtuális gép diagnosztikai adatok gyűjtése a bővítmény segítségével. A telepített bővítmények listában válassza ki a válassza ki az elérhető bővítmény legördülő menü, és válassza ki a Microsoft Agent figyelése diagnosztika.

    ![Az Azure virtuális gép bővítmény telepítése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766024.png)

    >[AZURE.NOTE] A virtuális gépeken futó többi diagnosztika kiterjesztést elérhetők. További tudnivalókért lásd: Azure virtuális Extensions és szolgáltatások.

1. Válassza a **Hozzáadás** gombra, a bővítmény hozzáadása és a **diagnosztikai konfigurációs** párbeszédpanel megjelenítése.

1. Válassza a **beállítás** gombra, adja meg a tárterület-fiókot, és válassza az **OK** gombra.

    Minden egyes lap (kivéve **az általános** és a **Napló könyvtárak**) összegyűjtheti diagnosztikai adatforrás jelöli.

    ![Azure diagnosztikai és konfigurációs engedélyezése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758144.png)

    Az alapértelmezett lapot, az **Általános**kínál az alábbi diagnosztikai végzett adatgyűjtés beállításai: **csak a hibák**, a **minden információt**és az **egyéni terv**. Az alapértelmezett beállítást, **csak a hibák**, megnyitja a tárhely a legkevesebb, mert azt nem nem ruházhatja át figyelmeztetések vagy nyomkövetési üzeneteket. Az **összes adatot** beállítást a legtöbb információ át, és ezért, tárterületet pedig a legtöbb költséges lehetőséget.

1. Ebben a példában a beállítással az **egyéni terv** , testre szabhatja a gyűjtött adatok.

1. A **Lemez kvóta MB** mezőben megadhatja a szabad terület hozzárendelni kívánt a tárterület-fiókjában diagnosztikai adatok. Ha azt szeretné, módosíthatja az alapértelmezett értéket.

1. Diagnosztikai adatok gyűjtendő egyes lapon jelölje be a **engedélyezése átviheti a <log type> ** jelölőnégyzetet.

    Például alkalmazás naplógyűjtés szeretné, ha az **alkalmazás naplók átadása engedélyezése** jelölőnégyzetet, az **Alkalmazás naplók** lapon jelölje be. Is adja meg az egyes diagnosztika adattípus által igényelt igény szerint további információkat. Című **konfigurálása diagnosztika adatforrásokat** a konfigurációs adatok a témakör későbbi az egyes lapokon.

1. Miután engedélyezte a kívánt diagnosztikai adatok gyűjteménye, válassza az **OK** gombra.

1. Mentse a frissített projektet.

    Megjelenik egy üzenet, hogy a virtuális gép frissült a **Microsoft Azure tevékenység naplója** ablakban.

## <a name="configure-diagnostics-data-sources"></a>Diagnosztikai adatforrások konfigurálása

Miután engedélyezte a diagnosztikai adatok gyűjtése, megadhatja pontosan milyen adatforrások szeretne gyűjteni, és milyen adatokat gyűjti össze. Az alábbiakban megtalálja a **Diagnosztika konfigurációs** párbeszédpanel, illetve egyes beállítás azt jelenti, hogy a lapok listáját.

### <a name="application-logs"></a>Alkalmazás naplók

**Alkalmazás naplók** webalkalmazás készített diagnosztikai információkat tartalmaz. Ha az alkalmazás naplók rögzíteni kívánt, jelölje be a a **alkalmazás naplók átadása engedélyezése** jelölőnégyzetet. Növelheti vagy csökkentheti hány perc, amikor az alkalmazás naplók átkerülnek a tárterület-fiókjába **Átadása időszak (perc)** értékének módosításával. Mennyi információ a napló szintű érték megadásával a naplóban rögzített is módosíthatja. Ha például megadhatja a **részletes** további információk, vagy válassza ki a **kritikus** rögzítéséhez csak a kritikus hibák. Ha egy adott diagnosztika szolgáltató, hogy az alkalmazás naplók bocsát, a **Szolgáltató globálisan egyedi azonosítója** mezőbe a szolgáltató globálisan egyedi azonosítója hozzáadásával rögzítheti őket.

  ![Alkalmazás naplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758145.png)

  Lásd: a [diagnosztikai naplózás web Apps alkalmazások Azure alkalmazás szolgáltatás engedélyezése](./app-service-web/web-sites-enable-diagnostic-log.md) alkalmazás naplók kapcsolatban további tudnivalókat.

### <a name="windows-event-logs"></a>Windows-eseménynaplók

Ha a Windows-eseménynaplók rögzíteni kívánt, jelölje be a a **átadás a Windows-eseménynaplók engedélyezése** jelölőnégyzetet. Növelheti vagy csökkentheti hány perc, amikor az eseménynaplók átkerülnek a tárterület-fiókjába **Átviheti időszak (perc)** értékének módosításával. Jelölje be a jelölőnégyzeteket események nyomon követni kívánt típusú.

  ![Eseménynaplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796664.png)

Ha Azure SDK 2.6-os vagy újabb használ, és adjon meg egy egyéni adatforrást szeretne, adja meg azt a **<Data source name>** szöveg mezőbe, és válassza a mellett a **Hozzáadás** gombra. Az adatforrás bekerül a diagnostics.cfcfg fájlt.

Ha az Azure SDK 2,5 használ, és adjon meg egy egyéni adatforrást szeretne, felveheti azt az `WindowsEventLog` szakaszában a diagnostics.wadcfgx fájl, például a következő példának megfelelően.

```
<WindowsEventLog scheduledTransferPeriod="PT1M">
   <DataSource name="Application!*" />
   <DataSource name="CustomDataSource!*" />
</WindowsEventLog>
```
### <a name="performance-counters"></a>Teljesítmény számláló

Számláló teljesítményadatok segítségével keresse meg a rendszer szűk és a rendszer és az alkalmazás teljesítményének finomhangolása. További információt talál [létrehozása és használata teljesítmény számláló az Azure-alkalmazásokban](https://msdn.microsoft.com/library/azure/hh411542.aspx) . Teljesítmény számláló rögzíteni kívánt, ha jelölje be a **Teljesítmény számláló átadása engedélyezése** jelölőnégyzetet. Növelheti vagy csökkentheti hány perc, amikor az eseménynaplók átkerülnek a tárterület-fiókjába **Átadása időszak (perc)** értékének módosításával. Jelölje ki a teljesítmény követni kívánt teljesítménymutatói jelölőnégyzetét.

  ![Teljesítmény számláló](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758147.png)

Nyomon követheti a teljesítmény számláló, amely nem szerepel a listában, írja be a javasolt szintaxissal, és válassza a **Hozzáadás** gombra. A virtuális gépen az operációs rendszer határozza meg, hogy milyen teljesítményt számláló nyomon követhető. Szintaxis kapcsolatos további tudnivalókért olvassa el [a számláló elérési útjának](https://msdn.microsoft.com/library/windows/desktop/aa373193.aspx)című témakört.

### <a name="infrastructure-logs"></a>Infrastruktúra naplók

Ha meg szeretné jeleníteni az Azure diagnosztikai infrastruktúra információt tartalmaznak, infrastruktúra naplók a távelérési, és a RemoteForwarder modul, jelölje be a **infrastruktúra naplók átadása engedélyezése** jelölőnégyzetet. Növelheti vagy csökkentheti hány perc, amikor a naplókat átkerülnek a tárterület-fiókjába átadása időszak (perc) értékének módosításával.

  ![Diagnosztikai naplók-infrastruktúrát](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC758148.png)

  [Naplózás adatok összegyűjtése Azure diagnosztika használatáról](https://msdn.microsoft.com/library/azure/gg433048.aspx) további információt talál.

### <a name="log-directories"></a>Log könyvtárak

Ha meg szeretné jeleníteni a napló könyvtárak, napló könyvtárak Internet Information Services (IIS) kérések gyűjtött adatokat tartalmazó, sikertelen kérelmek, vagy a mappákat, amelyek úgy dönt, jelölje be a **napló könyvtárak átadása engedélyezése** jelölőnégyzetet. Növelheti vagy csökkentheti hány perc, amikor a naplókat átkerülnek a tárterület-fiókjába **Átadása időszak (perc)** értékének módosításával.

Választhat a naplókat, például az **IIS naplókról** és a naplók **Kérése sikertelen volt** a gyűjtendő a jelölőnégyzetéből. Alapértelmezett tároló tároló neveket is közöljük, de a neveket, bármikor módosíthatja, ha azt szeretné.

Ezenkívül rögzítheti a naplók bármely mappájában. Csak a **napló abszolút címtárból** szakaszában adja meg az elérési utat, és válassza ki a a **Címtár hozzáadása** gombra. A naplók a rendszer rögzít a megadott tárolók.

  ![Log könyvtárak](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796665.png)

### <a name="etw-logs"></a>Esemény-nyomkövetés naplók

Ha [Eseményvezérelt Tracing Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803(v=vs.85).aspx) (esemény-nyomkövetés) használatát, és esemény-nyomkövetés naplók rögzítendő, jelölje be az **esemény-nyomkövetés naplók átadása engedélyezése** jelölőnégyzetet. Növelheti vagy csökkentheti hány perc, amikor a naplókat átkerülnek a tárterület-fiókjába **Átadása időszak (perc)** értékének módosításával.

Az események rögzítendő esemény forrásokból, és az esemény jegyzékfájlok megadott feltételeknek. Adja meg a forrás-, **Esemény források** szakaszában adja meg a nevét, és kattintson a **Forrás hozzáadása** gombra. Hasonlóképpen **Esemény-jegyzékfájlok:** szakaszában adja meg az esemény jegyzék, és válassza a az **Esemény cikkét hozzáadása** gombra.

  ![Esemény-nyomkövetés naplók](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766025.png)

  Az esemény-nyomkövetés keretrendszer ASP.NET támogatnak osztályok [System.Diagnostics.aspx] (névtér https://msdn.microsoft.com/library/system.diagnostics (v=vs.110). A Microsoft.WindowsAzure.Diagnostics névtér, amely örökli, és kibővíti a szabványos [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110) osztályok, engedélyezi a [System.Diagnostics.aspx] (https://msdn.microsoft.com/library/system.diagnostics (v=vs.110), a naplózás keretrendszer az Azure környezetben. További tudnivalókért lásd: [készítése vezérlő a naplózáshoz és a nyomkövetés a Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) és [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](./cloud-services/cloud-services-dotnet-diagnostics.md).

### <a name="crash-dumps"></a>Kiírása összeomlik

Ha adatait, amikor egy szerepkör-példány lefagy a rögzíteni kívánt, jelölje be a a **átadása összeomolhat kiírása engedélyezése** jelölőnégyzetet. (ASP.NET kezeli a legtöbb kivételeket, mivel ez a általában hasznos csak dolgozó szerepkörök.) Növelheti vagy csökkentheti az összeomlást kiírása az Office áruház rendelkezésre álló tárterület méretének százalékos **(%) a címtár-kvóta** értékének módosításával. A tároló tároló, ahol a összeomlik kiírása vannak tárolva, és választhatja, hogy meg szeretné jeleníteni a **teljes** vagy **Mini** kiírása módosíthatja.

A folyamatok pillanatnyilag követett sorolja fel. Jelölje ki a rögzíteni kívánt folyamatok tartozó jelölőnégyzeteket. Egy másik folyamat felvétele a listára, írja be a folyamat nevét, és kattintson a **Folyamat hozzáadása** gombra.

  ![Kiírása összeomlik](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766026.png)

  Lásd: [készítése vezérlő a naplózáshoz és a nyomkövetés a Microsoft Azure](https://msdn.microsoft.com/magazine/ff714589.aspx) és [Microsoft Azure diagnosztika rész 4: egyéni naplózás összetevők és Azure diagnosztika 1.3 módosítások](http://justazure.com/microsoft-azure-diagnostics-part-4-custom-logging-components-azure-diagnostics-1-3-changes/) további információt.

## <a name="view-the-diagnostics-data"></a>Diagnosztikai adatok megtekintése

Miután gyűjtött adatok, egy felhőalapú szolgáltatásba vagy egy virtuális gép diagnosztika adatait, tekintheti meg.

### <a name="to-view-cloud-service-diagnostics-data"></a>Felhőalapú szolgáltatás diagnosztikai adatok megtekintése

1. A felhőalapú szolgáltatás, mint a szokásos üzembe, majd futtatásával.

1. Megtekintheti a diagnosztikai adatok egy jelentés Visual Studio hoz létre, vagy a táblázatokban a tárterület-fiókjában. Egy jelentésben szereplő adatok megtekintéséhez nyissa meg a **Felhőben Intéző** vagy **Server Explorer**, a csomópont az Önt érdeklő szerepkör a helyi menü megnyitásához, és válassza a **Diagnosztikai adatok megtekintése**parancsra.

    ![Diagnosztikai adatok megtekintése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748912.png)

    A rendelkezésre álló adatokat tartalmazó jelentés jelenik meg.

    ![Microsoft Azure diagnosztika jelentés a Visual Studio](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796666.png)

    Ha a legfrissebb adatok nem látható, lehet, ha megvárja, amíg a továbbított idő teljen el.

    A **frissítési** hivatkozást azonnal frissíteni az adatokat, vagy válassza a elvégezve automatikusan frissíti az adatot **Automatikus frissítés** legördülő listában. A hiba-adatok exportálása, megnyithatja a számolótáblában vesszővel tagolt fájl létrehozása a **exportálása CSV-fájlok** gombot választva.

    A **Felhő Explorer** vagy a **Kiszolgáló Intézőben**nyissa meg a tárterület-fiókot, amely a példányban van társítva.

1. Nyissa meg a diagnosztika táblákat a tábla megjelenítő, és tekintse meg az Ön által gyűjtött adatokat. Az IIS naplók és egyéni naplók nyissa meg a blob-tárolóhoz. Az alábbi táblázat megtekintésével az Önt érdeklő adatokat tartalmazó tábla vagy blob-tároló is megkeresheti. Mellett az adatokat, a naplófájl, a táblázat bejegyzéseket tartalmaz EventTickCount, DeploymentId, szerepkör vagy RoleInstance azonosíthatja, mely virtuális gép és szerepkör hozza létre az adatokat, és amikor. 

  	|Diagnosztikai adatok|Leírás|Hely|
  	|---|---|---|
  	|Alkalmazás naplók|A kód generáló hívja fel a System.Diagnostics.Trace osztály módszerek naplók.|WADLogsTable|
  	|Eseménynaplók|Ezeket az adatokat a Windows-eseménynaplók a virtuális gépeken származik. Windows adatokat tárol ezeket a naplókat, de az alkalmazások és szolgáltatások is segítségükkel hibák jelentése vagy információk naplózása.|WADWindowsEventLogsTable|
  	|Teljesítmény számláló|Adatok összegyűjtheti bármely teljesítmény számláló, amely a virtuális gépen érhető el. Az operációs rendszer teljesítményét számláló, amelyekkel számos statisztikai adatokat, például memória használatát és processzor idő biztosít.|WADPerformanceCountersTable|
  	|Infrastruktúra naplók|Ezek a naplók a diagnosztikai infrastruktúra magát a jönnek létre.|WADDiagnosticInfrastructureLogsTable|
  	|IIS naplók|Ezek a naplók webes kérelmek rekord. A felhőalapú szolgáltatás forgalom jelentős mennyiségű kapja, ha ezeket a naplókat lehet igazán hosszú, így össze kell gyűjtenie és az adatok tárolására, csak akkor, ha szüksége lenne rá.|Nem sikerült kérelem naplózza a blob-tárolóban üvegvatta-iis-failedreqlogs csoportban egy mozgásvonalat, hogy telepítési, szerepkörök és példány csoportban található. Teljes naplók üvegvatta-iis-naplófájlok alatt található. Az egyes fájlok tételek az WADDirectories táblázatban.|
  	|Kiírása összeomlik|Ezt az információt biztosít a felhőalapú szolgáltatás folyamat (általában dolgozó szerepkör) bináris képeket.|üvegvatta-crush-kiírása blob-tárolóhoz|
  	|Egyéni naplófájlok|Az adatok, akkor az előre definiált naplók.|Megadhatja a kód egyéni naplófájlok helye a tárterület-fiókjában. Ha például egy egyéni blob-tárolóban is megadhat.|

1. Ha bármilyen típusú adatok egésszé lesz csonkítva, is próbálkozhat a puffer adatához növekvő típusa vagy az adatok továbbítása a virtuális számítógépről a tárterület-fiók közötti időtartamot térközszakaszok rövidítése.

1. (nem kötelező) Adatok törlése a tárterület-fiókból alkalmanként az általános tárolási költségek csökkentése érdekében.

1. Egy teljes telepítési ekkor a diagnostics.cscfg fájlt (az Azure SDK 2,5 .wadcfgx) frissül Azure-ban, és a felhőalapú szolgáltatás felveszi a diagnosztikai konfigurációja módosításait. Ha, ehelyett frissíti egy meglévő telepítéshez, a .cscfg fájl Azure-ban nem frissül. Továbbra is módosíthatja diagnosztika beállításainak, azonban a következő szakaszban szereplő lépéseket követve. Egy teljes telepítés elvégzéséhez és a meglévő-telepítés frissítése kapcsolatos további tudnivalókért lásd: az [Azure-alkalmazás varázsló közzététele](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-view-virtual-machine-diagnostics-data"></a>Virtuális gép diagnosztikai adatok megtekintése

1. A virtuális gép helyi menüben válassza a **Diagnosztikai adatok megtekintése**.

    ![Azure virtuális gépen diagnosztikai adatok megtekintése](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC766027.png)

    Ekkor megnyílik az **összefoglaló diagnosztika** ablak.

    ![Azure virtuális gép diagnosztika összefoglalása](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC796667.png)  

    Ha a legfrissebb adatok nem látható, lehet, ha megvárja, amíg a továbbított idő teljen el.

    A **frissítési** hivatkozást azonnal frissíteni az adatokat, vagy válassza a elvégezve automatikusan frissíti az adatot **Automatikus frissítés** legördülő listában. A hiba-adatok exportálása, megnyithatja a számolótáblában vesszővel tagolt fájl létrehozása a **exportálása CSV-fájlok** gombot választva.

## <a name="configure-cloud-service-diagnostics-after-deployment"></a>Telepítés után állítsa be a felhőalapú szolgáltatás diagnosztika

Ha megoldásán dolgozunk a probléma a felhőben, hogy már fut a szolgáltatás, érdemes lehet meg adatgyűjtés eredetileg telepítése a szerepkör előtt. Ebben az esetben is elindítható adatokat szeretne gyűjteni, hogy a kiszolgáló Explorer beállításainak használatával. Egy szerepkört, attól függően, hogy a diagnosztika konfigurációs párbeszédpanel megnyitása a példány vagy a szerepkör a helyi menü egy példányát, vagy az összes előfordulását diagnosztika beállíthatja. Ha úgy állítja be a szerepkör csomópontot, összes előfordulását beállításokon végzett módosítások érvényesek. Ha úgy állítja be a példány csomópontot, beállításokon végzett módosítások csak az adott példányhoz érvényesek.

### <a name="to-configure-diagnostics-for-a-running-cloud-service"></a>Diagnosztikai egy futó felhőalapú szolgáltatás konfigurálása

1. A kiszolgáló Intézőben bontsa ki a **Felhőbeli szolgáltatások** csomópontot, és bontsa ki a csomópontok keresse meg azt a szerepkört, vizsgálja meg a kívánt példány vagy mindkettőt.

    ![Diagnosztikai konfigurálása](./media/vs-azure-tools-diagnostics-for-cloud-services-and-virtual-machines/IC748913.png)

1. A helyi menü egy példány csomópont vagy szerepkör csomópontot válassza a **Frissítés diagnosztika beállításainak**, és válassza a gyűjtendő diagnosztikai beállítások.

    Beállításairól további információt a konfigurációs **konfigurálása diagnosztika adatforrások** című szakaszban, a jelen témakör. Arról, hogy miként diagnosztikai adatok megtekintése, olvassa el **a diagnosztikai adatok megtekintése** az Ez a témakör.

    Ha megváltoztatja az adatok gyűjtése **Kiszolgálót**Explorer, a módosítások érvényben maradnak mindaddig, amíg a felhőalapú szolgáltatás teljesen telepítsen újra. Ha használja az alapértelmezett közzétételi beállításokat, a módosításokat nem írják felül, mivel az alapértelmezett közzététele beállítás-e a meglévő-telepítés frissítése, hanem egy teljes újratelepítés tegye. Győződjön meg arról, hogy a beállítások törölje a jelet a telepítési idő, nyissa meg a **Speciális beállítások** lapon a közzétételi varázsló, és törölje a **telepítés frissítése** jelölőnégyzet jelölését. Telepítsen újra az adott jelölőnégyzet nincs bejelölve, ha a beállítások visszavált a .wadcfgx (vagy .wadcfg) fájlt a beállított a szerepkör a Tulajdonságok szerkesztő keresztül. Ha frissíti a üzembe, az Azure továbbra is a régi beállítások.

## <a name="troubleshoot-azure-cloud-service-issues"></a>Azure felhőalapú szolgáltatás kapcsolatos problémák megoldása

Tapasztal a felhőalapú szolgáltatást a projektek, például egy szerepkört, amely a "elfoglalt" állapotú, beragad problémák többször rendszer akkor hasznosítja újra vagy belső kiszolgálóhiba okoz, eszközök és technikákat diagnosztizálása és megoldása a problémák is használhatja. Gyakori problémák és megoldások adott példák, valamint a fogalmak és eszközök segítségével diagnosztizálása és javítása az ilyen hibák áttekintése [Azure PaaS kiszámítania diagnosztikai adatok](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx)olvashat.

## <a name="q--a"></a>A kérdések és válaszok

**Mi az a puffer méretét, és milyen nagy legyen?**

Minden virtuális gép példányon kvóták korlátozza a diagnosztikai adatok mennyiségétől a helyi fájlrendszerben tárolható. Ezeken kívül ad meg az egyes elérhető diagnosztikai adatok pufferelési méretet. Méret úgy működik, mint az adott típusú adatokat egy egyéni kvóta. Jelölje be a párbeszédpanel alján, megállapítható, hogy a teljes kvóta és a memória maradnak. Ha nagyobb puffer vagy több típusú adatokat ad meg, a általános kvóta fogja megközelíthető. Az általános kvóta módosíthatja a diagnostics.wadcfg/.wadcfgx konfigurációs fájl módosításával. Diagnosztikai adatok megtalálható az alkalmazás adatként az azonos formázáshoz, ha az alkalmazás nagy mennyiségű lemezterületet, ne növelheti a teljes diagnosztika kvóta.

**Mi az, hogy az átadás időszak, és mennyi ideig legyen?**

Az átadás időszak az az időtartam, amely jellemzi letelik adatok között. Minden egyes átadás pont után adatok átkerül a helyi fájlrendszerben virtuális gépen táblázatok a tárterület-fiókjában. Által gyűjtött adatokat a összege meghaladja a kvóta egy átviteli időszak vége előtt, ha a régebbi adatok törlődnek. Érdemes lehet az átadás időszak csökkentheti, ha éppen adatok elvesztése, mert az adatok meghaladó pufferelési méretének vagy a teljes kvóta.

**Milyen időzóna az időbélyegző vannak?**

A helyi időzónát az adatközpont, amelyen a felhőalapú szolgáltatás szerepelnek, az időbélyegző. Az alábbi három időbélyeg oszlopokat a napló táblázatokban használják.

  - **PreciseTimeStamp** az esemény-nyomkövetés időbélyeg az esemény. Ez azt jelenti, hogy az idő az esemény be van jelentkezve a ügyfélprogramból.

  - **IDŐBÉLYEG** lefelé kerekíti a feltöltés gyakoriság oszlopazonosító PreciseTimeStamp. Igen ha a feltöltés gyakoriságot 5 percig tart, és az esemény 00:17:12 idő, IDŐBÉLYEG lesz 00:15:00.

  - **Időbélyeg** , az időbélyegző, amelyen a szervezet létrehozásának az Azure táblázatban.

**Hogyan kezelhetem a költségek diagnosztikai adatok összegyűjtése során?**

Az alapértelmezett beállítások ( **hiba jelenik** meg**naplózási szintjének** és, **időszak átadása** meg az **1 perc**) költség minimalizálásához készültek. A számítási költségek növelik, ha több diagnosztikai adatok összegyűjtése vagy az átadás időszak csökkentése. Nem gyűjt több adat van szükség, és ne felejtse el adatgyűjtés letiltása, ha már nincs szüksége. Akkor is mindig kapcsolhatja be újra, akár futásidőben az előző szakaszban látható módon.

**Hogyan nem sikerült kérelem naplók gyűjtsön az IIS?**

Alapértelmezés szerint az IIS nem naplógyűjtés nem sikerült kérelmet. Beállíthatja, hogy az IIS gyűjthetők össze őket, ha a fájlt a webes szerepkör módosítja.

**Például OnStart RoleEntryPoint módszerek a nem vagyok első nyomkövetési adatok mi a baj?**

RoleEntryPoint módszereinek úgynevezett WAIISHost.exe, nem az IIS környezetében. Emiatt a konfigurációs információi a web.config, amely általában a nyomkövetés lehetővé teszi, hogy nem érvényes. A probléma megoldásához .config fájl hozzáadása a webes szerepkör projekthez, és a kimeneti összeállítás RoleEntryPoint kódját tartalmazó megfelelően a fájl nevét. A .config fájl nevét az alapértelmezett webes szerepkör projekt WAIISHost.exe.config lenne. A következő sorokat majd hozzáadása a fájlhoz:

```
<system.diagnostics>
  <trace>
      <listeners>
          <add name “AzureDiagnostics” type=”Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener”>
              <filter type=”” />
          </add>
      </listeners>
  </trace>
</system.diagnostics>
```

Most a **Tulajdonságok** ablak a tulajdonság **kimeneti könyvtár másolás** **mindig**másolandó.

## <a name="next-steps"></a>Következő lépések

Többet szeretne tudni a diagnosztikai naplózás az Azure, lásd: [Diagnosztika segítségével, így az Azure Felhőszolgáltatások és a virtuális gépeken futó](./cloud-services/cloud-services-dotnet-diagnostics.md) , és [engedélyezze a diagnosztikai naplózás web Apps alkalmazások Azure App szolgáltatásban](./app-service-web/web-sites-enable-diagnostic-log.md).
