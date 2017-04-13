<properties 
    pageTitle="Adatgyűjtés egy felhőalapú szolgáltatásba, a számítási irányító a helyi meghajtóra |} Microsoft Azure" 
    services="cloud-services"
    description="Vizsgálja meg a teljesítménnyel kapcsolatos problémák a felhőszolgáltatások és a Visual Studio profilert" 
    documentationCenter=""
    authors="TomArcher" 
    manager="douge" 
    editor=""
    tags="" 
    />

<tags 
    ms.service="cloud-services" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="07/30/2016" 
    ms.author="tarcher"/>

# <a name="testing-the-performance-of-a-cloud-service-locally-in-the-azure-compute-emulator-using-the-visual-studio-profiler"></a>Az Azure számítási irányító, használja a Visual Studio Profilert helyileg egy felhőalapú szolgáltatásba teljesítményét tesztelése

Eszközök és technikák számos felhőszolgáltatások teljesítményének teszteléshez érhetők el.
Azure közzéteheti egy felhőalapú szolgáltatásba, amikor a Visual Studio profilkészítési adatok összegyűjtése és majd elemzés helyi meghajtóra, mint az [Azure alkalmazás adatgyűjtés]lehet[1].
Is használhatja diagnosztika teljesítmény számláló, számos követéséhez [használata teljesítmény számláló Azure-ban]ismertetett módon[2].
Célszerű az alkalmazást helyileg a számítási irányító üzembe helyezése a felhőbe előtt profil is.

Ez a cikk bemutatja a Processzor mintavételnél módja adatgyűjtés, amely helyileg végezhető el a irányító. Processzor mintavételnél akkor módszer adatgyűjtés, amely nem nagyon tolakodó. A kijelölt mintavételnél időközönként a profilert pillanatfelvételt a hívás veremtárának. Az adatok időbeli során gyűjtött, és jelentés látható. Ezt a módszert adatgyűjtés általában azt jelzik, amelyben egy olyan végeredményt intenzív alkalmazásra Processzor munka a legtöbb kell végrehajtania.  Ez lehetőséget nyújt a "meleg elérési út" kiemelése hol az alkalmazás a legtöbb idő tölti.



## <a name="1-configure-visual-studio-for-profiling"></a>1: állítsa be a Visual Studio-adatgyűjtés

Első lépésként néhány hasznos lehet amikor adatgyűjtés Visual Studio beállítási lehetőségek vannak. Értelmezéséhez táblázatkapcsolat profilkészítési jelentések, az alkalmazás és a is rendszer tárak szimbólumok szüksége szimbólumok (.pdb fájlok). Győződjön meg arról, hogy hivatkozás-e a rendelkezésre álló szimbólum kiszolgálók szeretné. Ehhez a Visual Studióban, az **eszközök** menü **Beállítások elemre**kattint, majd válassza a **Hibakeresés**, majd a **szimbólumok**. Győződjön meg arról, hogy a Microsoft-kiszolgálókkal szimbólum szerepel-e a **szimbólum (.pdb) helye**.  Http://referencesource.microsoft.com/symbols, amelyek esetleg további szimbólum fájlok is hivatkozhat.

![Beállítások szimbóluma][4]

Tetszés szerint a jelentéseket, amely csak a saját kód megadásával létrehozza a profilert egyszerűsíthető. Csak a saját kód engedélyezve függvény hívás készleteket egyszerűbb is, hogy a hívások teljesen belső tárakhoz és a .NET-keretrendszer el van rejtve az a jelentéseket. Válassza az **eszközök** menü **Beállítások**. Bontsa ki a **Teljesítmény eszközök** csomópontot, majd válassza az **Általános**lehetőséget. Jelölje be a **Engedélyezése csak a saját kód profilert jelentésekhez**.

![Csak a saját kód beállítása][17]

Egy meglévő projektből vagy egy új projekt használhatja ezeket az utasításokat.  Próbálja ki az alábbiakban ismertetett technikák új projektet hoz létre, ha válassza a projekt C# **Azure felhőalapú szolgáltatásba** , és jelölje be a **Webes szerepkör** és **Dolgozó szerepkör**.

![Azure felhőalapú szolgáltatás project szerepkörök][5]

Céljából, például kód hozzáadása a projektet, amely sok időt vesz igénybe, és bemutat néhány egyértelmű teljesítményprobléma. A következő kódot például hozzáadása dolgozó szerepkör projekthez:

    public class Concatenator
    {
        public static string Concatenate(int number)
        {
            int count;
            string s = "";
            for (count = 0; count < number; count++)
            {
                s += "\n" + count.ToString();
            }
            return s;
        }
    }

Ez a kód felhívja a RunAsync módszer a dolgozó szerepkör RoleEntryPoint eredetű osztály. (Figyelmen kívül hagyja a szinkron futtatás módszer a figyelmeztetést.)

        private async Task RunAsync(CancellationToken cancellationToken)
        {
            // TODO: Replace the following with your own logic.
            while (!cancellationToken.IsCancellationRequested)
            {
                Trace.TraceInformation("Working");
                Concatenator.Concatenate(10000);
            }
        }

Készítése, és futtassa a felhőalapú szolgáltatás helyileg (Ctrl + F5) hibakeresés nélkül a megoldást konfiguráció **kiadásra**. Biztosan megtörténjen az összes fájlok és mappák jönnek létre az alkalmazást futtató helyi meghajtóra, és biztosíthatja, hogy az összes emulátor elindított. Ellenőrizze, hogy fut-e a dolgozó szerepkör kezdje a tálcáról kiszámítania irányító a felhasználói felület.

## <a name="2-attach-to-a-process"></a>2: egy folyamat csatolása

Adatgyűjtés az alkalmazás a Visual Studio 2010 IDE a indításával, hanem a profilert futó folyamatot kell kapcsolni. 

A profilert csatolása folyamat **elemzés** menüben válassza a **Profilert** és **Csatolása/leválasztás**.

![Profil beállítás csatolása][6]

Keresse meg a WaWorkerHost.exe folyamat dolgozó szerepet az.

![WaWorkerHost folyamatábra][7]

Ha a projekt mappa hálózati meghajtón, a profilert megkérdezi, meg kell adnia a másik helyre mentheti a profilkészítési jelentéseket.

 Akkor is csatolhat webes szerepkörbe WaIISHost.exe való csatolásával.
Ha az alkalmazás több dolgozó szerepkör folyamatok, szüksége processID elkülönítik őket. A processID programozás útján lekérdezés Process objektum elérése. Például kód hozzáadása a Futtatás egy szerepkörben RoleEntryPoint eredetű osztály módja, ha megnézheti a napló kiszámítania irányító felhasználói felület, hogy milyen folyamat csatlakozni szeretne.

    var process = System.Diagnostics.Process.GetCurrentProcess();
    var message = String.Format("Process ID: {0}", process.Id);
    Trace.WriteLine(message, "Information");

Indítsa el a kiszámítania irányító a felhasználói felület a naplójának megtekintéséhez.

![Indítsa el a számítási irányító felhasználói felület][8]

A konzol ablak címsora kattintva nyissa meg a dolgozó szerepkör napló konzol ablakban kiszámítania irányító felhasználói felület. Megtekintheti a naplóban folyamat azonosítója.

![Nézet folyamat azonosítója][9]

Egy csatolt, hajtsa végre a lépéseket az alkalmazás felhasználói felületén (ha szükséges) meg megismételni az alkalmazási példát.

Ha le szeretné állítani a adatgyűjtés, válassza a **Adatgyűjtés leállítása** hivatkozásra.

![Adatgyűjtés beállítás leállítása][10]

## <a name="3-view-performance-reports"></a>3: teljesítményét jelentések megtekintése

Az alkalmazás Projektteljesítmény-jelentés jelenik meg.

Ezen a ponton a profilert végrehajtása megszakad, .vsp fájlba menti az adatokat, és megjeleníti a jelentés, amely mutatja az adatok elemzését.

![Profilert jelentés][11]


Ha a meleg elérési String.wstrcpy jelenik meg, kattintson a csak a felhasználó kód megjelenítése a nézet módosítása csak a saját kód.  Ha String.Concat látható, nyomja le az összes kód megjelenítése gombra.

Meg kell jelennie a ÖSSZEFŰZ módszer és egy nagy része a végrehajtási ideje megkezdésének String.Concat.

![Elemzési jelentés][12]

Ha ez a cikk a hozzáadott a karakterlánc összefűzés kódot, érdemes figyelmeztetést a feladatlistában a. Is látható, hogy van-e túl nagy összegű oka az, hogy karakterláncok, amelyeket létrehozott, és értékesített száma szemétgyűjtő figyelmeztető üzenetet.

![Teljesítmény figyelmeztetések][14]

## <a name="4-make-changes-and-compare-performance"></a>4: módosításokat, és hasonlítsa össze a teljesítmény

Összehasonlíthatja a teljesítmény előtt és után kód megváltoztatása is.  A futó folyamat leállítása, és a kódot a karakterlánc összefűzési művelet lecserélése StringBuilder felhasználása szerkesztése:

    public static string Concatenate(int number)
    {
        int count;
        System.Text.StringBuilder builder = new System.Text.StringBuilder("");
        for (count = 0; count < number; count++)
        {
             builder.Append("\n" + count.ToString());
        }
        return builder.ToString();
    }

Végezze el a másik teljesítmény futtatás, és hasonlítsa össze a teljesítmény. A teljesítmény Explorer lefut az esetén az adott munkamenetben akkor is csak válassza ki a két jelentéseket, nyissa meg a helyi menü, és válassza **Teljesítményjelentéseinek összehasonlítása**. Ha szeretne egy másik teljesítmény munkamenetben Futtatás összehasonlítása, a nyissa meg a **adatainak elemzése** menüt, és válassza a **Teljesítményjelentéseinek összehasonlítása**. A megjelenő párbeszédpanelen adja meg a fájlokat.

![Hasonlítsa össze a teljesítmény jelentések beállítás][15]

A jelentések jelölje ki a két futása közötti különbségeket.

![Összehasonlító jelentés][16]

Gratulálok! Ön által néhányan lépések a profilert.

## <a name="troubleshooting"></a>Hibaelhárítás

- Ellenőrizze, hogy még megjelenés építés vannak adatainak összegyűjtése és hibakeresés nélkül.

- Ha a csatolása/leválasztás nincs engedélyezve van a Profilert menüben, futtassa a teljesítmény varázslót.

- Az alkalmazás állapotának megtekintése a kiszámítania irányító a felhasználói felület használatával. 

- Ha problémák lépnek fel a irányító alkalmazások kezdve, vagy csatolásával a profilert, állítsa le a számítási irányító, és indítsa újra. Ha ez nem oldja meg a problémát, próbálkozzon a újraindítása. Ez a probléma akkor fordulhat elő, ha a kiszámítania irányító felfüggesztheti, és távolítsa el a futó telepítések.

- Ha használt bármelyik profilkészítési parancsot a parancssorból, különösen a globális beállításokat, ellenőrizze, hogy meghívták VSPerfClrEnv /globaloff és VsPerfMon.exe leállt.

- Ha az üzenet jelenik meg, mintavételezésnél "PRF0025: nem lett gyűjtött adatokat," Ellenőrizze, hogy a csatolt folyamat Processzor tevékenység van-e. Alkalmazások, amelyek nem mivel foglalkoznak számítási munkáját előfordulhat, hogy hozhatók létre mintavételnél adatokat.  Lehetőség arra is, hogy a folyamat kilépett bármely mintavételnél előtti. Ellenőrizze, hogy egy szerepkört, amely vannak adatgyűjtés módszer a Futtatás nem ér véget.

## <a name="next-steps"></a>Következő lépések

A irányító Azure bináris leírására nem támogatja a Visual Studio profilert, de ha meg szeretné vizsgálni memóriafoglalást, amikor adatgyűjtés is válassza ezt a beállítást. Is megadhatja feldolgozási adatainak összegyűjtése, amely segít, hogy van-e szálak gyorskorcsolyázásban zárolások vagy réteg kapcsolati adatainak összegyűjtése, amely segít között egy alkalmazás leggyakrabban között az adatok réteg dolgozó szerepkörbe rétegek használata esetén teljesítményproblémákat nyomon követése általi lefoglalását idő.  Az alkalmazás által az adatbázis-lekérdezések megtekintése, és a profilkészítési adatok használata az adatbázis használatához javítása érdekében. Réteg kapcsolati adatainak összegyűjtése kapcsolatos további tudnivalókért lásd a blogbejegyzésből [Útmutató: a réteg kapcsolati Profilert használata a Visual Studio csapat rendszer 2010][3].



[1]: http://msdn.microsoft.com/library/azure/hh369930.aspx
[2]: http://msdn.microsoft.com/library/azure/hh411542.aspx
[3]: http://blogs.msdn.com/b/habibh/archive/2009/06/30/walkthrough-using-the-tier-interaction-profiler-in-visual-studio-team-system-2010.aspx
[4]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally09.png
[5]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally10.png
[6]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally02.png
[7]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally05.png
[8]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally010.png
[9]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally07.png
[10]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally06.png
[11]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally03.png
[12]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally011.png
[14]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally04.png 
[15]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally013.png
[16]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally012.png
[17]: ./media/cloud-services-performance-testing-visual-studio-profiler/ProfilingLocally08.png
 