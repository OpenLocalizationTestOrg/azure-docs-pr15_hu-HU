<properties 
    pageTitle="Az Azure felhőszolgáltatásba vagy a virtuális gép a Visual Studio hibakeresési |} Microsoft Azure"
    description="Egy felhőalapú szolgáltatásba vagy Visual Studio virtuális gép hibakeresése"
    services="visual-studio-online"
    documentationCenter="na"
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

# <a name="debugging-an-azure-cloud-service-or-virtual-machine-in-visual-studio"></a>Az Azure felhőszolgáltatásba vagy a virtuális gép a Visual Studióban hibakeresése

Visual Studio lehetőségeket is biztosít különböző Azure felhőszolgáltatások és a virtuális gépeken futó hibakeresése során.



## <a name="debug-your-cloud-service-on-your-local-computer"></a>A felhőalapú szolgáltatás, a helyi számítógépen hibakeresése

Időt takaríthat, és az Azure használatával pénzt szeretné hibakeresése a felhőalapú szolgáltatás, a helyi számítógépen irányító számítja ki. Helyben hibakeresési szolgáltatást, mielőtt beállítaná őket, amelyet javíthatja megbízhatóságának és teljesítményének fizet a számítási idő nélkül. Azonban néhány hiba akkor fordulhat elő csak futtatásakor egy felhőalapú szolgáltatásba Azure-ban magát. A hibák is hibakeresési, ha engedélyezi a távoli hibakeresés, tegye közzé a szolgáltatást, és csatolja a debugger szerepkör-példányhoz.

A irányító keltő a szűrő megőrzi az Azure kiszámítania szolgáltatás, és a helyi környezetben fut, így Ön tesztelése és hibakeresés a felhőalapú szolgáltatást, mielőtt beállítaná őket. A irányító a szerepkör-példányok életciklusának kezeli, és szimulált erőforrások, például helyi tároló hozzáférést biztosít. Hibakeresési vagy a szolgáltatás futtatásakor a Visual Studio, automatikusan elindul az irányító háttér alkalmazásként, és ezután üzembe helyezése a szolgáltatásban való a irányító. A irányító megtekintése a szolgáltatásban, a helyi környezetben végrehajtásakor is használhatja. A teljes verzióját vagy a sürgős verziója a irányító futtathatja. (Azure 2.3 kezdve, az elsőbbségi a irányító verziója az alapértelmezett.) Lásd: [futtatni, és egy felhőalapú szolgáltatásba helyileg hibakeresési Express irányító használatával](https://msdn.microsoft.com/library/dn339018.aspx).

### <a name="to-debug-your-cloud-service-on-your-local-computer"></a>A felhőalapú szolgáltatás, a helyi számítógépen hibakeresése

1. Válassza a menüsáv **hibakeresési**, **Hibakeresési indítsa el** az Azure felhőalapú szolgáltatás project futtatásához. Alternatív megoldásként nyomja le az F5. Megjelenik egy üzenet, amely a kiszámítania irányító indítása folyamatban van. A irányító indításakor a a tálcaikonjára megerősíti.

    ![Azure irányító a tálcán](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC783828.png)

1. Jelenítse meg a felhasználói felületet számítási irányító úgy, hogy megnyitja a helyi menü az Azure ikonja az értesítési területen, és válassza a **Kiszámítania irányító UI megjelenítése**.

    A bal oldali ablaktáblában a felhasználói felületének jeleníti meg a szolgáltatások, a számítási irányító és az egyes szolgáltatásokhoz futtató szerepkör példányok jelenleg telepített. A szolgáltatás vagy életciklus, a naplózás és a diagnosztikai adatok megjelenítéséhez a jobb oldali szerepkörök kiválasztása Ha a fókusz egy része-ablak felső szélén található kibővíti a jobb oldali kitöltéséhez.

1. Végezze el az alkalmazás kijelölése **hibakeresési** menü parancsai, és töréspontok állítsa be a kódot. Az alkalmazás a hibakeresőben álljon, mint az ablaktáblák frissülnek, az alkalmazás az aktuális állapotát. Amikor leállítja hibakeresése során, az alkalmazás telepítési törlődik. Ha az alkalmazás tartalmaz egy webes szerepkört, és az indítási művelet tulajdonság a webböngészőben indításához állított be, a Visual Studio a webalkalmazás elindítja a böngészőben. Ha módosítja a szolgáltatás beállításai szerepet példányainak száma, kell, állítsa le a felhőalapú szolgáltatást, és indítsa újra, hogy ezek a szerepkör új példányokat is hibakeresése hibakeresése során.

    **Megjegyzés:** Amikor leállítja fut, vagy a szolgáltatás hibakeresési, a helyi számítási irányító és tároló irányító nem leállt. Le kell állítania őket kifejezetten az értesítési területen.


## <a name="debug-a-cloud-service-in-azure"></a>Azure-ban egy felhőalapú szolgáltatásba hibakeresése

Szeretné hibakeresése távoli számítógépről egy felhőalapú szolgáltatásba, engedélyeznie kell funkció explicit módon, hogy kötelező, a szerepkör példánya futtatható virtuális gépeken telepített szolgáltatások (például msvsmon.exe) a felhőalapú szolgáltatás telepítésekor. A szolgáltatás közzétételekor távoli hibakeresés nem engedélyezi, esetén a szolgáltatás közzé a távoli hibakeresés engedélyezve van.

Engedélyezése a távoli hibakeresése során egy felhőalapú szolgáltatásba, hogy ne csökkentett teljesítményt mutat, vagy nem merülnek fel további díjak. Ne használhatja távoli hibakeresés egy gyártási szolgáltatása, mivel az ügyfelek, akik a szolgáltatás használatához előfordulhat, hogy befolyásolhatják.

>[AZURE.NOTE] Egy felhőalapú szolgáltatásba, a Visual Studio közzétételekor szolgáltatás bármely szerepkörök, a .NET-keretrendszer 4 vagy a .NET-keretrendszer 4.5 városrész engedélyezheti **IntelliTrace** . **IntelliTrace**vizsgálja meg, hogy az elmúlt egy szerepkör-példány e események és Reprodukálja kezdve az környezetben. [Egy közzétett felhőszolgáltatásba IntelliTrace és a Visual Studio hibakeresés](http://go.microsoft.com/fwlink/?LinkID=623016) és [Segítségével IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx)témakörben talál.

### <a name="to-enable-remote-debugging-for-a-cloud-service"></a>Ahhoz, hogy a távoli hibakereséshez egy felhőalapú szolgáltatásba

1. Nyissa meg a helyi menü az Azure projekt, és válassza a **Közzététel**gombra.

1. Jelölje ki a **fejlesztői** környezet, illetve a **hibakeresési** beállításait.

    Ez az útmutató csak. A próba-verziót tartalmazó környezetek futtatásához munkakörnyezetben választhatja. Azonban káros hatással lehet felhasználókat Ha engedélyezi a távoli hibakeresés az éles üzemi környezetben. Megadhatja, hogy a Megjelenés konfigurációja, de a hibakeresési konfiguráció teszi egyszerűbbé hibakeresése során.

    ![Válassza ki a hibakeresési konfiguráció](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746717.gif)

1. Kövesse a szokásos, de jelölje be az **Összes szerepkörének távoli Debugger engedélyezése** jelölőnégyzetet a **Speciális beállítások** lapon.

    ![Konfigurációs hibakeresése](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746718.gif)

### <a name="to-attach-the-debugger-to-a-cloud-service-in-azure"></a>A debugger csatolhat egy felhőalapú szolgáltatásba, Azure-ban

1. Kiszolgáló Explorerben bontsa ki a felhőben szolgáltatásbeli csomópontot.

1. Nyissa meg a helyi menü, szerepkör vagy szerepkör-példányt, amelyre a csatolni kívánt, és válassza a **Debugger csatolni**.

    Szerepkör hibakeresése, ha a Visual Studio debugger csatol minden példányában szerepkörhöz. A debugger megszakítja a az első szerepkör-példány, lefut a sort, és az adott töréspont bármely feltételeknek megfelelő töréspont. Ha egy példány, a debugger rendeli, csak a példány és oldaltörések csak akkor, amikor az adott példányhoz futtatja a sort, és a töréspont feltételeknek megfelelő Töréspont a hibakeresése.

    ![Debugger csatolása](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746719.gif)

1. A debugger csatol egy példány, miután hibakeresési a szokásos módon. A debugger automatikusan csatolja a szerepkör a megfelelő host folyamat. Attól függően, hogy mi a feladata a debugger csatol w3wp.exe, WaWorkerHost.exe vagy WaIISHost.exe. Ha ellenőrizni szeretné a folyamatot, amelyhez a debugger csatlakozik, bontsa ki a példány csomópontot, a kiszolgáló Intézőben. Lásd: [Azure szerepkör architektúra](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) Azure folyamatok kapcsolatban további tudnivalókat.

    ![Jelölje be a kódot párbeszédpanelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Azonosítja a folyamatok, amelyhez a debugger csatlakozik, a menüsor, válassza a hibakeresési, a Windows, a folyamatok, eljárások párbeszédpanel megnyitásához. (Billentyűzet: Ctrl + Alt + Z) Egy adott folyamat leválasztása, nyissa meg a helyi menüt, és válassza a **Leválasztása folyamat**. Vagy, a kiszolgáló Intézőben keresse meg a példány csomópontot, keresse meg a folyamat, nyissa meg a helyi menüt, és válassza a **Folyamat leválasztása**.

    ![Folyamatok hibakeresése](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC690787.gif)

>[AZURE.WARNING] Hosszú leáll, amikor a távoli töréspontok elkerülése hibakeresése során. Azure egy folyamat, amely nem válaszol, néhány percnél hosszabb le van állítva, és leállítja a forgalom küld az adott példányhoz kezeli. Ha túl sokáig leállítja a msvsmon.exe leválasztja az eljárásból.

Az összes folyamatok hibakereső leválasztása a példány vagy szerepkör, nyissa meg a helyi menü, szerepkör vagy, hibakeresése példányt, és válassza a **Debugger leválasztása**.

## <a name="limitations-of-remote-debugging-in-azure"></a>Azure-ban a távoli hibakereséshez korlátai

Azure SDK 2.3, a távoli hibakeresés rendelkezik az alábbi korlátozások vonatkoznak.

- A távoli hibakeresés engedélyezve van, nem tehető közzé egy felhőalapú szolgáltatásba, amelyben minden szerepkör legfeljebb 25 példánya van.

- A debugger 30400 való 30424, a 31400 való 31424 és a 32400 való 32424 portot használja. Próbál meg valamelyik portot használja, ha nem tudja közzétenni a szolgáltatást, és a következő hibaüzenetek egyike fog megjelenni a tevékenység naplója Azure: 

    - A hiba érvényesítése szemben a .csdef fájlt a .cscfg fájlt. 
    A foglalt tartomány "porttartományt" végpont szerepkör "szerepkör" Microsoft.WindowsAzure.Plugins.RemoteDebugger.Connector átfedésben egy korábban definiált port vagy tartomány.
    - Felosztás sikertelen volt. Próbálja meg később, csökkentse a virtuális méretének vagy szerepkör példányainak száma, vagy próbálja üzembe helyezése más területére.


## <a name="debugging-azure-virtual-machines"></a>Azure virtuális gépeken futó hibakeresése

A Visual Studióban kiszolgáló Intézővel Azure virtuális gépeken futó programokból is hibakeresési. Ha engedélyezi a távoli hibakeresés Azure virtuális-gépen, Azure telepíti a távoli hibakeresési bővítmény a virtuális gépen. Ezután a virtuális gépen folyamatok csatolása és hibakeresés a szokásos módon.

>[AZURE.NOTE] Az Azure erőforrás-kezelő objektumhalomban keresztül létrehozott virtuális gépeken futó felhőalapú Visual Studio 2015 Explorerrel távolról indítja is. További információért témakörökben [kezelése Azure felhő Intézővel](http://go.microsoft.com/fwlink/?LinkId=623031).

### <a name="to-debug-an-azure-virtual-machine"></a>Az Azure virtuális gép hibakeresése

1. A kiszolgáló Intézőben bontsa ki a virtuális gépeken futó csomópontot, és jelölje ki a virtuális gép szeretné hibakeresése, amelyet a csomópontot.

1. Nyissa meg a helyi menü, és jelölje be a **Hibakeresési engedélyezése**. Amikor a program megkérdezi, hogy meg arról, hogy ha szeretné hibakeresése során a virtuális gépen engedélyezése, válassza az **Igen gombra**.

    Azure a virtuális gépen szeretné hibakeresése engedélyezése a távoli hibakeresési bővítmény telepítése.

    ![Virtuális gép engedélyezése hibakeresési parancs](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure tevékenységnapló](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. A távoli hibakeresési bővítmény befejezése telepítése után nyissa meg a virtuális gép helyi menüt, és válassza a **Debugger csatolása**

    Azure megkapja a folyamatok listáját a virtuális gépen, és megjeleníti azokat a csatolása folyamat párbeszédpanelt.

    ![Csatolása debugger gomb](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. **Folyamat csatolása** párbeszédpanelen jelölje ki, **Jelölje ki** a lista korlátozása a találatok csak szeretné hibakeresése kód típusú megjelenítéséhez. Akkor is hibakeresési 32 vagy 64 bites felügyelt kódot, natív kódot vagy mindkettőt.

    ![Jelölje be a kódot párbeszédpanelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Jelölje ki a folyamatok szeretné hibakeresése a virtuális gépen, és válassza a **Csatolás**. Például a w3wp.exe folyamat előfordulhat, hogy válassza, ha azt szeretné hibakeresése a virtuális gépen webalkalmazást. [Hibakeresési egy vagy több folyamatot a Visual Studio](https://msdn.microsoft.com/library/jj919165.aspx) és [Azure szerepkör architektúra](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) talál további információt.

## <a name="create-a-web-project-and-a-virtual-machine-for-debugging"></a>Webes projekt és hibakeresés virtuális gép létrehozása

A Azure projekt közzététele, előtt hasznosak lehetnek, tesztelje a tárolókban található környezetben hibakeresése és tesztelése a eseteket támogató, és tudja telepíteni az tesztelése és a programok figyelése. Ehhez egy úgy, hogy az alkalmazás virtuális gépen távolról hibakeresési.

Visual Studio ASP.NET projektek ajánlat létrehozásához használható az alkalmazás teszteléséhez praktikus virtuális gép beállítást. A virtuális gép gyakran szükség végpontok például PowerShell, távoli asztali és WebDeploy tartalmazza.

### <a name="to-create-a-web-project-and-a-virtual-machine-for-debugging"></a>Webes projekt és hibakeresés virtuális gép létrehozása

1. A Visual Studióban hozzon létre egy új ASP.NET webalkalmazást.

1. Az új ASP.NET projekt párbeszédpanelen Azure csoportban válassza a **virtuális gép** a legördülő listából. Hagyja bejelölve a **távoli erőforrások létrehozása** jelölőnégyzetet. Válassza az **OK gombra** a folytatáshoz.

    Megjelenik az **Azure virtuális gép létrehozása** párbeszédpanel.


    ![ASP.NET webes projekt párbeszédpanelen létrehozása](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746723.png)

    **Megjegyzés:** Meg kell adnia bejelentkezni az Azure-fiókjába, ha még nincs bejelentkezve.

1. Jelölje ki a virtuális gép különböző beállításokat, és kattintson **az OK**gombra. [Virtuális gépeken futó]( http://go.microsoft.com/fwlink/?LinkId=623033) talál további információt.

    A DNS-név név lesz a virtuális gép nevét. 

    ![Virtuális gép létrehozása az Azure párbeszédpanel](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746724.png)

    Azure a virtuális gép, majd a rendelkezések hoz létre, és a végpontok, például a távoli asztali és a webhely üzembe konfigurálása



1. A virtuális gép teljesen konfigurálása után, jelölje ki a virtuális gép csomópontot a kiszolgáló Intézőben.

1. Nyissa meg a helyi menü, és jelölje be a **Hibakeresési engedélyezése**. Amikor a program megkérdezi, hogy meg arról, hogy ha szeretné hibakeresése során a virtuális gépen engedélyezése, válassza az **Igen gombra**. 

    Azure a virtuális gépen szeretné hibakeresése engedélyezése a távoli hibakeresési bővítmény telepítése.

    ![Virtuális gép engedélyezése hibakeresési parancs](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746720.png)

    ![Azure tevékenységnapló](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746721.png)

1. A projekt közzététele a [Útmutató: a webes projekt segítségével egyetlen kattintással közzététele a Visual Studióban üzembe](https://msdn.microsoft.com/library/dd465337.aspx). Mert szeretné hibakeresése virtuális gépen a **Webhely közzététele** varázsló a **Beállítások** lapon jelölje be **a hibakeresési** a konfigurációs. Ezzel biztosíthatja, hogy kód szimbólumok elérhetők hibakeresése során.

    ![Közzététel beállításai](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718349.png)

1. A **Fájl közzé beállítások**válassza a **célhelyen további fájlok törlése** , ha a projekt már korábban rendszerbe állított.

1. Miután közzététele a project Server Explorer, a virtuális gép helyi menüjében válassza ki a **Debugger csatolása**

    Azure megkapja a folyamatok listáját a virtuális gépen, és megjeleníti azokat a csatolása folyamat párbeszédpanelt.

    ![Csatolása debugger gomb](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC746722.png)

1. **Folyamat csatolása** párbeszédpanelen jelölje ki, **Jelölje ki** a lista korlátozása a találatok csak szeretné hibakeresése kód típusú megjelenítéséhez. Akkor is hibakeresési 32 vagy 64 bites felügyelt kódot, natív kódot vagy mindkettőt.

    ![Jelölje be a kódot párbeszédpanelen](./media/vs-azure-tools-debug-cloud-services-virtual-machines/IC718346.png)

1. Jelölje ki a folyamatok szeretné hibakeresése a virtuális gépen, és válassza a **Csatolás**. Például a w3wp.exe folyamat előfordulhat, hogy válassza, ha azt szeretné hibakeresése a virtuális gépen webalkalmazást. [Hibakeresési egy vagy több folyamatot a Visual Studióban](https://msdn.microsoft.com/library/jj919165.aspx) talál további információt.

## <a name="next-steps"></a>Következő lépések

- Hívások és események naplója gyűjt a Megjelenés kiszolgáló **Intellitrace** használata Lásd: [egy közzétett Felhőszolgáltatásba IntelliTrace és a Visual Studio hibakeresése során](http://go.microsoft.com/fwlink/?LinkID=623016).
- **Azure diagnosztika** segítségével részletes információk naplózása belül szerepkörök, futó kódból, hogy a szerepkörök futtatja a fejlesztői környezet vagy Azure-ban. Lásd: [Collecting naplózás Azure diagnosztika segítségével adatokat](http://go.microsoft.com/fwlink/p/?LinkId=400450).
