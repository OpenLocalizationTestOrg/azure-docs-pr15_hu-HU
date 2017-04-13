<properties
    pageTitle="Visual Studio sablonok az Azure köteg |} Microsoft Azure"
    description="Megtudhatja, hogyan ezeket a Visual Studio projektsablonok segítségével végrehajtása és a számítási igényű munkaterhelésekből futtathatnak Azure köteg"
    services="batch"
    documentationCenter=".net"
    authors="fayora"
    manager="timlt"
    editor="" />

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="09/07/2016"
    ms.author="marsma" />

# <a name="visual-studio-project-templates-for-azure-batch"></a>Visual Studio projektsablonok Azure köteg törlése

A **Feladatkezelő** és a **tevékenység processzor Visual Studio sablonok** köteghez adja meg a segít végrehajtása és a számítási igényű munkaterhelésekből futtassa a legkevesebb munkamennyiség köteg kódot. A dokumentum ismerteti a sablonok és használatuk útmutatást.

>[AZURE.IMPORTANT] Ez a cikk azt ismerteti, hogy csak a következő két sablonokra vonatkozó adatokat, és azt feltételezi, hogy már jól ismert a köteg szolgáltatás és a hozzá kapcsolódó alapfogalmak: készletek, a csomópontok, feladatok és a feladatok, manager projektfeladatok, környezeti változók és egyéb információkat számítja ki. További információt a [Alapjai az Azure köteg](batch-technical-overview.md) [Köteg szolgáltatás áttekintése a fejlesztők](batch-api-basics.md)és [az Azure köteg tárat .NET – első lépések](batch-dotnet-get-started.md)található.

## <a name="high-level-overview"></a>Magas szintű – áttekintés

A Feladatkezelő és a tevékenység processzor-sablonok két hasznos összetevők létrehozásához használható:

* A kezelő projektfeladat, amely egy feladat elosztót, amely is lebontva a feladat több feladat, amely független, párhuzamosan futhat be.

* Egy feladat processzor előfeldolgozása és az alkalmazás parancssori körül utáni feldolgozása végrehajtásához használható.

Például film megjelenítését helyzetben, a feladat elválasztó szeretné alakítani a film egyetlen feladat száz vagy külön feladatokhoz, amelyekhez külön-külön kellene folyamat egyes keretek ezer. Ennek megfelelően a tevékenység processzort szeretne meghívni az megjelenítését és jeleníti meg az egyes szükséges összes függő folyamat keret, valamint hajtsa végre az esetleges további műveleteket (például egy tárolási helye a leképezett keret másolásával).

>[AZURE.NOTE] A Feladatkezelő és a tevékenység processzor sablonokat is független egymáshoz képest, így választva mindkét, illetve csak az egyik, attól függően, hogy a követelményeket, a számítási feladatot, és kattintson a beállítások.

Ahogy az alábbi ábrán látható, a program elküldi ezeket a sablonokat használó számítási feladat három szakaszban keresztül:

1. Az ügyfél kód (például alkalmazás, webes szolgáltatás, stb.) a köteg szolgáltatás Azure megoldást, mely szerint a Feladatkezelő tevékenység a projekt-kezelő program megadása a feladatot nyújt.

2. A köteg szolgáltatás fut, a kezelő projektfeladat számítási csomópontra, és a feladat elválasztó a feladat processzor feladatok, a megadott számú megnyitása a, sok kiszámítania igény csomópontok paramétereket és a feladat elválasztó kódban jellemzői alapján.

3. A tevékenység processzor feladatok független, párhuzamosan zajlik, a bemeneti adatok folyamat, és a kimeneti adatai készítése futnak.

![Az ábrán az látható, hogy hogyan kommunikáljon ügyfél-kódot a köteg szolgáltatással][diagram01]

## <a name="prerequisites"></a>Előfeltételek

A köteg sablonok használatához a következőkre lesz szüksége:

* Egy számítógép Visual Studio 2015, vagy újabb, már telepítve van a azt.

* A köteg sablonokkal, amelyek állnak rendelkezésre a [Visual Studio gyűjtemény] [ vs_gallery] Visual Studio bővítmények szerint. A sablonok két módon lehet:

  * A sablonok a **frissítéseket és bővítmények** párbeszédpanelen a Visual Studio telepítése (további tudnivalókért lásd: a [Keresés és a Visual Studio segítségével bővítmények][vs_find_use_ext]). A **bővítmények és frissítések** párbeszédpanelen keresse meg és a következő két bővítmények letöltése:

    * A feladat elválasztó Azure köteg Feladatkezelő
    * Azure köteg tevékenység processzor

  * Az online gyűjtemény for Visual Studio a sablonok letöltése: [Microsoft Azure köteg projektsablonok][vs_gallery_templates]

* Ha azt tervezi, hogy a projekt-kezelő az [Alkalmazáscsomagok](batch-application-packages.md) funkció használatával, és a köteg tevékenység processzor csomópontok számítja ki, a tárterület-fiók a köteg ügyfélhez csatolni szeretne.

## <a name="preparation"></a>Előkészítése

Azt javasoljuk, hogy a feladat manager, valamint a tevékenység processzor tartalmazó megoldás létrehozásakor, mivel ez megkönnyítik a Feladatkezelő és a tevékenység processzor programok között kód megosztására. Ez a megoldás létrehozásához tegye a következőket:

1. Nyissa meg a Visual Studio 2015, és kattintson a **fájl** > **Új** > **Projekt**.

2. A **sablonok**csoportban bontsa ki az **Egyéb projekttípusok**, válassza a **Visual Studio megoldások**, és válassza az **Üres megoldás**.

3. Írja be az alkalmazás és a megoldás (például "LitwareBatchTaskPrograms") célját leíró nevét.

4. Az új megoldás létrehozásához kattintson az **OK gombra**.

## <a name="job-manager-template"></a>Feladatkezelő sablon

A Feladatkezelő sablon segítségével megvalósítását a kezelő projektfeladat az alábbi műveleteket végezheti el:

* A feladat felosztása több tevékenység.
* A köteg futtatásához feladatokon nyújt.

>[AZURE.NOTE] Többet szeretne tudni a kezelő projektfeladatok a [Köteg szolgáltatás áttekintése fejlesztők számára](batch-api-basics.md#job-manager-task)című témakörben találhat.

### <a name="create-a-job-manager-using-the-template"></a>Hozzon létre egy feladat Manager a sablonnal

A Feladatkezelő a megoldást, amely a korábban létrehozott hozzáadásához kövesse az alábbi lépéseket:

1. Nyissa meg a meglévő megoldás a Visual Studio 2015.

2. A megoldás Intézőben kattintson a jobb gombbal a megoldást, kattintson a **Hozzáadás** > **Új projektet**.

3. A **vizuális C#**kattintson a **felhőben**, és kattintson a **Azure köteg feladat elemre a feladat elválasztó**.

4. Írjon be egy nevet, amelyen láthatók az alkalmazást, és ehhez a projekthez azonosítja a feladat vezetőként (például "LitwareJobManager").

5. A projekt létrehozásához kattintson az **OK gombra**.

6. Végezetül összeállítása kényszerítése Visual Studio töltse be az összes hivatkozott NuGet csomagot, és győződjön meg arról, hogy a projekt érvényes előtt szerkesztésekor azt a projektet.

### <a name="job-manager-template-files-and-their-purpose"></a>Feladatkezelő sablonfájlokat és a céljuk

A Feladatkezelő sablonnal projekt létrehozásakor három csoport kód fájlokat hoz létre:

* A fő programfájl (Program.cs). Ez tartalmazza, a program belépési pontjához és a legfelső szintű kezelésének módját. Ne a szokásos módon módosítania kell a.

* A keret könyvtár. Ez a feladat manager program – paraméterek, a tevékenységek hozzáadása a köteg feladat stb kicsomagolás "újrafelhasználható" munkát felelős fájlokat tartalmazza. Ne a szokásos módon módosítania kell ezeket a fájlokat.

* A feladat elválasztó fájl (JobSplitter.cs). Ez a program helyétől függően az alkalmazás-specifikus logikája felosztása a projekt a tevékenységek.

Természetesen további fájlokat adhat hozzá igény támogatása a feladat elválasztó kód, a feladat logika felosztása összetettségétől függően.

A sablon is létrehoz szabványos .NET projektfájlokat, például egy .csproj fájlt, app.config, packages.config stb.

A többi Ez a szakasz ismerteti a különböző fájlokat és a saját kód szerkezet, és megtudhatja, mit jelent az egyes osztály.

![Visual Studio megoldás Explorer megjelenítése a projekt-kezelő sablon megoldás][solution_explorer01]

**Keret fájlok**

* `Configuration.cs`: Feladat konfigurációs adatok, például a köteg számla részletei, a csatolt tároló fiók hitelesítő adatait, a feladat és a tevékenység adatai és a feladat paraméterei betöltését ágyaz be. Azt is köteg által megadott környezetben változók (lásd: a tevékenységek köteg dokumentációjában környezeti beállítások) keresztül a Configuration.EnvironmentVariable osztály hozzáférést biztosít.

* `IConfiguration.cs`: A konfigurációs osztály végrehajtásának abstracts, hogy a feladat elválasztó, a hamis és mock konfigurációs objektuma segítségével is egység tesztje.

* `JobManager.cs`: Orchestrates a projekt-kezelő program összetevői. Ez a felelős a inicializálása a feladat elválasztó, a feladat elválasztó meghívását, és a feladatokat a tevékenység küldőtől a feladat elválasztó által visszaadott elküldési.

* `JobManagerException.cs`: Jelöl, amely a feladat-kezelő befejezéséhez szükséges hiba. Ha adott diagnosztikai adatok biztosítható, hogy lemondási részeként várható"hibák tördelése JobManagerException szolgál.

* `TaskSubmitter.cs`: Ez az osztály vehet fel a köteg feladat a feladat elválasztó által visszaadott feladatokat végzi el. A JobManager osztály összegzések hatékony, de a megfelelő időben hozzáadása a projekthez tartozó kötegekben történik a feladatok sorrendjének majd meghívja TaskSubmitter.SubmitTasks háttér szálon minden egyes köteghez.

**Feladat osztósávja**

`JobSplitter.cs`: Ez az osztály felosztása a feladat feladatok az alkalmazás-specifikus logikája tartalmazza. A keret elindítja a JobSplitter.Split módszert tevékenység hozzáadása a feladatot, mint a módszerrel adja vissza őket a sorozatában juthat. Ez az a osztály hol fogja tölteni a logika a feladat. A felosztott módszert vissza, amely a feladatokat, amelybe menteni szeretné, hogy a feladat partition CloudTask példányok sorozata végrehajtása.

**Szabványos .NET parancssorban projektfájlok**

* `App.config`: Szabványos .NET alkalmazás konfigurációs fájl.

* `Packages.config`: Szabványos NuGet függőség csomagfájlt.

* `Program.cs`: A program belépési pontjához és a legfelső szintű kezelésének módját tartalmazza.

### <a name="implementing-the-job-splitter"></a>A feladat elválasztó végrehajtása

A Feladatkezelő sablon projektet nyit meg, amikor a projekt lesz a JobSplitter.cs fájl alapértelmezés szerint. A felosztott logika a tevékenységek a terhelést a Split() módszer megjelenítése az alábbi használatával végrehajtása:

```csharp
/// <summary>
/// Gets the tasks into which to split the job. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The job manager framework invokes the Split method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation should return tasks lazily, for example using a C#
/// iterator and the "yield return" statement; this allows tasks to be added
/// and to start running while splitting is still in progress.
/// </summary>
/// <returns>The tasks to be added to the job. Tasks are added automatically
/// by the job manager framework as they are returned by this method.</returns>
public IEnumerable<CloudTask> Split()
{
    // Your code for the split logic goes here.
    int startFrame = Convert.ToInt32(_parameters["StartFrame"]);
    int endFrame = Convert.ToInt32(_parameters["EndFrame"]);

    for (int i = startFrame; i <= endFrame; i++)
    {
        yield return new CloudTask("myTask" + i, "cmd /c dir");
    }
}
```

>[AZURE.NOTE] Magyarázó jelekkel ellátott szakaszában a `Split()` módja a csak a Feladatkezelő sablon kódot, amely szól a logika hozzáadását, módosítását: a feladatok felosztása másik tevékenységeket szakaszát. Sablon eltérő szakasz módosítani szeretné, ha győződjön meg róla, familiarized vannak a köteg működése, és próbálja ki a [köteget mintakódok]néhány[github_samples].

A Split() példányhoz való hozzáférést:

* A feladat paraméterek keresztül a `_parameters` mezőben.
* A CloudJob objektum keresztül, a feladatot, amely a `_job` mezőben.
* A CloudTask objektum manager projektfeladat keresztül, amely a `_jobManagerTask` mezőben.

A `Split()` végrehajtása nem szükséges, közvetlenül a feladat vehet fel tevékenységeket. Ehelyett a kód térjen vissza a CloudTask objektumot egy sorozata, és ezek hozzáadódik a feladat automatikusan az, hogy a feladat elválasztó meghívása keretrendszer osztályok. Közös használata C# 's léptető (`yield return`) szolgáltatás lehetővé teszi a minél korábban a fut, és nem az összes tevékenységhez számolásra Várakozás indításához a feladatok elválasztókat a feladat végrehajtásához.

**Feladat osztási hiba**

Ha a feladat osztási hibát észlel, meg kell tennie vagy:

* A sorozat használatával a C# Leállítás `yield break` kimutatás, amelyben esetben a Feladatkezelő rendszer kezeli sikeres; vagy

* Kivételhibát, a Feladatkezelő rendszer értékként kezeli esetben nem sikerült és előfordulhat, hogy kell ismételni attól függően, hogy hogyan az ügyfél van beállítva).

Mindkét esetben egyetlen tevékenységet sem a projekt elválasztó által visszaadott, és már hozzáadott a köteg feladat futtatására alkalmas lesz. Ha nem szeretné, hogy ez, majd meg a következőket teheti:

* A feladat befejezéséhez előtt, hogy a feladat elválasztó visszaadása

* A tevékenység teljes gyűjteményt előtt, hogy visszahelyezi megfogalmazásához (Ez azt jelenti, hogy vissza egy `ICollection<CloudTask>` vagy `IList<CloudTask>` végrehajtása a feladat elválasztó C# léptető használata helyett)

* Tevékenység függések segítségével a Feladatkezelő sikeres befejezésétől függenek minden tevékenység

**Feladat manager újrapróbálkozások**

Ha nem sikerül a projekt-kezelő, akkor előfordulhat, hogy kell ismételni ügyfél újrapróbálkozási beállításaitól függően köteg szolgáltatás. Az általános ennek oka biztonságos keretében tevékenységek hozzáadása a feladatot, akkor figyelmen kívül hagyja esetleges már meglévő tevékenységek. Jó helyen jár Ha a tevékenységeket számításához drága, nem merülnek fel újraszámítása a feladat; már felvett tevékenységek költsége merülnek fel viszont ha ismételt futtatása nincs garantált azonosítók ugyanaz a feladat létrehozásához kattintson az "ismétlődések figyelmen kívül hagyása" működés fog nem indítása. Ebben az esetben tervezheti meg a feladat elválasztó feltárása a munkát, amely már elvégzett és a ne ismétlődjenek, például egy CloudJob.ListTasks mielőtt megkezdené a hozam feladatok végrehajtásával.

### <a name="exit-codes-and-exceptions-in-the-job-manager-template"></a>Lépjen ki a kódok és a Feladatkezelő sablonban kivételek

Kilépés kódok és a kivételek mechanizmusa határozza meg az így létrejövő programot, és bármilyen problémát tapasztal a program végrehajtását azonosítása segítik. A Feladatkezelő sablon végrehajtja a kilépési kódok és a jelen szakaszban ismertetett kivételek.

A Feladatkezelő sablonnal végrehajtott projekt manager tevékenység három lehetséges kilépési kódok térhet vissza:

| Kód | Leírás |
|------|-------------|
| 0    | A projekt-kezelő sikeresen befejeződött. A feladat elválasztó kód száma, és a feladatok lettek hozzáadva a feladatot. |
| 1    | A kezelő projektfeladat nem sikerült, a program várható"részében kivétellel leállhat. A kivétel volt egy JobManagerException és diagnosztikai információ lefordítani és, ha lehetséges, javasolt a hiba megoldása. |
| 2    | A kezelő projektfeladat kivétellel leállhat váratlan"nem sikerült. A kivétel rögzítve van a normál kimenő, de a projekt-kezelő nem tudott hozzá igény szerint további diagnosztikai vagy remediation információkat. |

Feladat manager feladat sikertelensége esetén bizonyos feladatok továbbra is hozzáadott a szolgáltatás előtt a hiba történt. Az alábbi műveleteket a szokásos módon fog futni. "Feladat osztási hiba" lásd fent vitafórum elérési út kódot.

Az összes kivételek által visszaadott adatok stdout.txt és stderr.txt fájlokba íródott. További tudnivalókért olvassa el a [Hiba kezelésének](batch-api-basics.md#error-handling)című témakört.

### <a name="client-considerations"></a>Ügyfélprogramokkal kapcsolatos szempontok

Ez a szakasz ismerteti a néhány ügyfél végrehajtása követelményeket, a sablon alapján feladat kezelőjének meghívásakor. Megtudhatja, [hogy miként paramétereket és az ügyfél kódból környezeti változók átadni](#pass-environment-settings) részletesen paramétereket és környezeti beállításokat.

**Kötelező hitelesítő adatok**

Annak érdekében, hogy a tevékenységek hozzáadása az Azure köteg feladat, a kezelő projektfeladat Azure köteg fiók URL-címe és kulcs szükséges. Ezeket a környezeti változók YOUR_BATCH_URL és YOUR_BATCH_KEY nevű hatolhat. Beállíthatja, hogy ezek a Feladatkezelő tevékenység környezeti beállításokat. Ha például a C# ügyfél:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    new EnvironmentSetting("YOUR_BATCH_URL", "https://account.region.batch.azure.com"),
    new EnvironmentSetting("YOUR_BATCH_KEY", "{your_base64_encoded_account_key}"),
};
```
**Tárterület hitelesítő adatok**

Az ügyfél általában nem szükséges csatolt tároló fiók hitelesítő adatait a kezelő projektfeladat adja meg, mert a legtöbb feladat menedzserek szükségtelen kifejezetten a csatolt tároló fiók eléréséhez, és (b) a csatolt tároló fiók gyakran kapja meg minden tevékenység, a feladat általános környezet beállítása. Ha nem tartalmazza a csatolt tároló fiók a közös környezeti beállításokat, és a feladat-kezelő csatolt tároló hozzáférésre van szüksége, majd kell megadni a csatolt tároló hitelesítő adatok az alábbi képlettel történik:

```csharp
job.JobManagerTask.EnvironmentSettings = new [] {
    /* other environment settings */
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

**Feladat manager tevékenység beállításai**

Az ügyfél kell beállítania a feladat manager *killJobOnCompletion* jelző **hamis**.

Az ügyfél számára **hamis**értékűre *runExclusive* általában megbízható.

Az ügyfél, hogy a feladat kell használni a *resourceFiles* vagy *applicationPackageReferences* gyűjtemény a számítási csomópontot rendszerbe állított végrehajtható manager (és a szükséges DLL-ek).

Alapértelmezés szerint a Feladatkezelő nem megpróbálja indulhat. Attól függően, hogy a feladat manager logika az ügyfél is engedélyezni szeretné keresztül *kényszerek*újrapróbálkozások/*maxTaskRetryCount*.

**Feladat beállításai**

Ha a projekt elválasztó bocsát ki a tevékenységek, függőségek, az ügyfél értékre kell állítani a feladat usesTaskDependencies igaz.

A feladat elválasztó modell szokatlan az ügyfelek számára kíván a tevékenységek hozzáadása a feladatok felül a feladat elválasztó hoz létre. Az ügyfél kell ezért a szokásos módon állítsa a feladat *onAllTasksComplete* **terminatejob**.

## <a name="task-processor-template"></a>Tevékenység processzor sablon

Tevékenység processzor sablon segítségével végrehajtása egy tevékenység processzor, az alábbi műveleteket végezheti el:

* Állítsa be az egyes köteg tevékenységek futtatásához szükséges információkat.
* Futtassa a köteg feladatokra által igényelt összes műveletet.
* Mentse a tevékenység kimeneti értékeket állandó tárolóhoz.

Bár a tevékenység processzor feladatok futtatásához a köteg nem szükséges, a kulcs egy tevékenység processzor előnye, hogy tartalmaz-e egy csomagolópapír minden tevékenység végrehajtási műveletek végrehajtása egy helyen. Ha például minden tevékenység környezetében több alkalmazás futtatásához szükséges, vagy ha módosítani szeretné másolni állandó tárolóhoz elvégzése után az adatok minden feladat.

A tevékenység processzor által végrehajtott műveletek lehet egyszerű vagy összetett, és a lehető legtöbb vagy kevés igény szerint a terhelést. Ezenkívül minden Feladatműveletek: az egyik tevékenység processzor bevezetésével, könnyen frissítése vagy felvétele az alkalmazások és a terhelést követelmények alapján műveletek. Azonban bizonyos esetekben egy tevékenység processzor előfordulhat, hogy nem lehet a végrehajtásához az optimális megoldás akkor is hozzáadhat a szükségtelen összetettsége, például amikor futó feladatokat, gyorsan indítható el egy egyszerű parancssorból.

### <a name="create-a-task-processor-using-the-template"></a>Hozzon létre egy tevékenység processzor a sablon használatával

A megoldást, amely a korábban létrehozott egy tevékenység processzor hozzáadásához kövesse az alábbi lépéseket:

1. Nyissa meg a meglévő megoldás a Visual Studio 2015.

2. A megoldás Intézőben kattintson a jobb gombbal a megoldást, kattintson a **Hozzáadás**gombra, és kattintson az **Új projekt**.

3. A **vizuális C#**kattintson a **felhőben**, és válassza az **Azure köteg tevékenység processzor**.

4. Adjon egy nevet, amelyen láthatók az alkalmazást, és amely azonosítja a projekt a tevékenység processzor (például "LitwareTaskProcessor").

5. A projekt létrehozásához kattintson az **OK gombra**.

6. Végezetül összeállítása kényszerítése Visual Studio töltse be az összes hivatkozott NuGet csomagot, és győződjön meg arról, hogy a projekt érvényes előtt szerkesztésekor azt a projektet.

### <a name="task-processor-template-files-and-their-purpose"></a>Tevékenység processzor sablonfájlok és a céljuk

A tevékenység processzor sablonnal projektet hoz létre, ha ezután három csoportba kód fájlokat hoz létre:

* A fő programfájl (Program.cs). Ez tartalmazza, a program belépési pontjához és a legfelső szintű kezelésének módját. Ne a szokásos módon módosítania kell a.

* A keret könyvtár. Ez a feladat manager program – paraméterek, a tevékenységek hozzáadása a köteg feladat stb kicsomagolás "újrafelhasználható" munkát felelős fájlokat tartalmazza. Ne a szokásos módon módosítania kell ezeket a fájlokat.

* A tevékenység processzor-fájl (TaskProcessor.cs). Ez a program helyétől függően az alkalmazás-specifikus összefüggés-tevékenység végrehajtása (általában, amelyet egy meglévő végrehajtható fájl hívását). Előtti és utáni feldolgozása programkódját, például az eredmény fájl, további adatokat letöltésének vagy feltöltésének is helye.

Természetesen további fájlokat adhat hozzá igény támogatása a tevékenység processzor kód, a feladat logika felosztása összetettségétől függően.

A sablon is létrehoz szabványos .NET projektfájlokat, például egy .csproj fájlt, app.config, packages.config stb.

A többi Ez a szakasz ismerteti a különböző fájlokat és a saját kód szerkezet, és megtudhatja, mit jelent az egyes osztály.

![A tevékenység processzor sablon megoldást megjelenítő Visual Studio megoldás Explorer][solution_explorer02]

**Keret fájlok**

* `Configuration.cs`: Feladat konfigurációs adatok, például a köteg számla részletei, a csatolt tároló fiók hitelesítő adatait, a feladat és a tevékenység adatai és a feladat paraméterei betöltését ágyaz be. Azt is köteg által megadott környezetben változók (lásd: a tevékenységek köteg dokumentációjában környezeti beállítások) keresztül a Configuration.EnvironmentVariable osztály hozzáférést biztosít.

* `IConfiguration.cs`: A konfigurációs osztály végrehajtásának abstracts, hogy a feladat elválasztó, a hamis és mock konfigurációs objektuma segítségével is egység tesztje.

* `TaskProcessorException.cs`: Jelöl, amely a feladat-kezelő befejezéséhez szükséges hiba. Ha adott diagnosztikai adatok biztosítható, hogy lemondási részeként várható"hibák tördelése TaskProcessorException szolgál.

**Tevékenység processzor**

* `TaskProcessor.cs`: A tevékenység fut. A keret elindítja a TaskProcessor.Run módszerrel. Ez az a osztály hol fogja tölteni az alkalmazás-specifikus logika a tevékenység. A Futtatás módszert végrehajtása:
  * Elemezni, és a tevékenység paramétereket ellenőrzése
  * A parancssorból a meghívni kívánt külső program írása
  * Bármely, a hibakereséshez megkövetelheti diagnosztikai információk naplózása
  * Egy adott parancssorból folyamat elindítása
  * Várja meg a folyamat való kilépéshez
  * A kilépési kódot határozza meg, ha sikeres vagy sikertelen volt a folyamat rögzítése
  * Bármely állandó tárolóhoz megtartani kívánt kimeneti fájlok mentése

**Szabványos .NET parancssorban projektfájlok**

* `App.config`: Szabványos .NET alkalmazás konfigurációs fájl.
* `Packages.config`: Szabványos NuGet függőség csomagfájlt.
* `Program.cs`: A program belépési pontjához és a legfelső szintű kezelésének módját tartalmazza.

## <a name="implementing-the-task-processor"></a>A tevékenység processzor végrehajtása

A tevékenység processzor sablon projektet nyit meg, amikor a projekt lesz a TaskProcessor.cs fájl alapértelmezés szerint. Alkalmazhat a tevékenységek futtatása logika a terhelést a módszerrel a Run() alább látható módon:

```csharp
/// <summary>
/// Runs the task processing logic. This is where you inject
/// your application-specific logic for decomposing the job into tasks.
///
/// The task processor framework invokes the Run method for you; you need
/// only to implement it, not to call it yourself. Typically, your
/// implementation will execute an external program (from resource files or
/// an application package), check the exit code of that program and
/// save output files to persistent storage.
/// </summary>
public async Task<int> Run()

{
    try
    {
        //Your code for the task processor goes here.
        var command = $"compare {_parameters["Frame1"]} {_parameters["Frame2"]} compare.gif";
        using (var process = Process.Start($"cmd /c {command}"))
        {
            process.WaitForExit();
            var taskOutputStorage = new TaskOutputStorage(
            _configuration.StorageAccount,
            _configuration.JobId,
            _configuration.TaskId
            );
            await taskOutputStorage.SaveAsync(
            TaskOutputKind.TaskOutput,
            @"..\stdout.txt",
            @"stdout.txt"
            );
            return process.ExitCode;
        }
    }
    catch (Exception ex)
    {
        throw new TaskProcessorException(
        $"{ex.GetType().Name} exception in run task processor: {ex.Message}",
        ex
        );
    }
}
```
>[AZURE.NOTE] A jelekkel ellátott szakaszt a Run() módszert tevékenység processzor sablon kód számára, hogy a tevékenységek futtatása logika a terhelést a hozzáadásával módosítani csak része. Ha azt szeretné, a sablon a különböző szakaszok módosítását, kérjük, először megismerkedhet köteg működése a köteg dokumentációt megtekintésével és a köteg mintakódok néhány ki szeretné próbálni.

A Run() módszer a felelős a várja meg az összes folyamat befejezéséhez, egy vagy több folyamat elindítása a parancssorból indítása az eredmények mentése, és végül visszaadása egy adott hibakóddal. A Run() módszer az, hogy hol a feldolgozás logikájának megvalósítása a tevékenységekhez. A tevékenység processzor keretrendszer elindítja a Run() módszer ki; nem kell fordulnia, azt saját magának.

A Run() példányhoz való hozzáférést:

* A tevékenység paraméterek keresztül a `_parameters` mezőben.
* A feladatot, és a tevékenység azonosítók keresztül a `_jobId` és `_taskId` mezőket.
* A tevékenység konfiguráció keresztül a `_configuration` mezőben.

**Feladat sikertelensége**

Hiba esetén a Run() módszer szerint Kivétel kiváltása kiléphet, de ez a legfelső szintű kivétel kezelő elhagyja a tevékenység kilépési kódot közül. Ha szabályozhatja a kilépési kódot, így például különböztethetjük a különböző típusú hiba, a diagnosztikai célokra, vagy mert néhány hiba módban kell megszünteti a feladatot, és mások nem kell kell majd kell kilép Run() módszer visszaküldi, a nullától kilépési kódot. Ez lesz a tevékenység kilépési kódot.

### <a name="exit-codes-and-exceptions-in-the-task-processor-template"></a>Lépjen ki a kódok és a tevékenység processzor sablonban kivételek

Kilépés kódok és a kivételek mechanizmusa határozza meg az így létrejövő programot, és bármilyen problémát tapasztal a program végrehajtását azonosítása segítik. A tevékenység processzor sablon végrehajtja a kilépési kódok és a jelen szakaszban ismertetett kivételek.

A tevékenység processzor sablonnal végrehajtott tevékenység processzor tevékenység három lehetséges kilépési kódok térhet vissza:

| Kód | Leírás |
|------|-------------|
|  [Process.ExitCode][process_exitcode] | A tevékenység processzor futtatta száma. Megjegyzendő, hogy ez nem jelenti azt, hogy a program indításakor sikeres volt – csak, hogy a tevékenység processzor sikeresen aktiváló és elvégzett bármely utáni feldolgozás kivételek nélkül. A kilépési kódot szerinti függ, hogy a meghívott program – általában kilépési kódot 0 azt jelzi, hogy a program sikeres és bármely más kilépési kódot azt jelenti, hogy a program nem sikerült. |
| 1    | A tevékenység processzor nem sikerült, a program várható"részében kivétellel leállhat. A kivétel lefordítani egy `TaskProcessorException` és diagnosztikai információ és, ha lehetséges, javasolt a hiba megoldása. |
| 2    | A tevékenység processzor kivétellel leállhat váratlan"nem sikerült. A kivétel rögzítve van a normál kimenő, de a tevékenység processzor nem tudott hozzá igény szerint további diagnosztikai vagy remediation információkat. |

>[AZURE.NOTE] Ha a program, jelenik meg, hogy az adott hiba módban használja a kilépési kódok 1-2, majd kódokkal Kilépés 1 és 2 tevékenység processzor hibákat nem egyértelmű. A tevékenység processzor hibakódok Program.cs fájl kivétel eseteit szerkesztésével módosíthatja megkülönböztető kilépési kódok.

Az összes kivételek által visszaadott adatok stdout.txt és stderr.txt fájlokba íródott. További tudnivalókért lásd a hiba kezelésének, köteg dokumentációjában.

### <a name="client-considerations"></a>Ügyfélprogramokkal kapcsolatos szempontok

**Tárterület hitelesítő adatok**

Ha a tevékenység processzor Azure blob-tárolóhoz továbbra is fennáll a kimeneti értékeket, például a fájl szabályok segítő tár használatával való majd azt hozzáférésre van szüksége *bármelyik* a felhőalapú tárolási fiók hitelesítő adatait *vagy* egy blob tároló URL-címet, amely tartalmazza a megosztott access aláírás (Társítások). A sablonban szerepel a közös környezeti változók keresztül hitelesítő adatok megadása támogatása. Az ügyfelek továbbíthatja tároló hitelesítő adatok az alábbi képlettel történik:

```csharp
job.CommonEnvironmentSettings = new [] {
    new EnvironmentSetting("LINKED_STORAGE_ACCOUNT", "{storageAccountName}"),
    new EnvironmentSetting("LINKED_STORAGE_KEY", "{storageAccountKey}"),
};
```

A tároló fiók majd érhető el a TaskProcessor osztály keresztül a `_configuration.StorageAccount` tulajdonság.

Ha inkább tároló URL-cím használata a Társítások, is átadhatja a feladat általános környezet beállítása keresztül, de a tevékenység processzor sablon jelenleg nem tartalmaz a beépített támogatása.

**Tárolás beállítása**

Javasolt, hogy az ügyfél vagy egy feladat manager tevékenység létrehozása bármely tárolók a tevékenységek hozzáadása a projekthez előtt szükséges feladatokat. Ez a kötelező az Társítások tároló URL-címet használja, ha olyan URL-cím nem tartalmaz hozza létre a tárolót engedéllyel. Ajánlott, ha a sikeres tároló fiók hitelesítő adatait, mivel minden tevékenység nem kell a tároló CloudBlobContainer.CreateIfNotExistsAsync telefonál menti.

## <a name="pass-parameters-and-environment-variables"></a>A sikeres paramétereket és környezeti változók

### <a name="pass-environment-settings"></a>A sikeres környezeti beállítások

Egy ügyfél is át adatokat a vezető projektfeladat környezeti beállítások formájában. Ez az információ majd használhatja a kezelő projektfeladat részeként a számítási feladatot fog futni tevékenység processzor feladatokat létrehozásakor. Példák a információkat átviheti környezeti beállítások a következők:

* Tárterület fiók nevét, és a fiók kulcsok
* Köteg fiók URL-címe
* Köteg fiókkulcs

A köteg szolgáltatás rendelkezik-e egy egyszerű eljárás környezeti beállítások átadni manager projektfeladat használatával a `EnvironmentSettings` tulajdonság a [Microsoft.Azure.Batch.JobManagerTask][net_jobmanagertask].

Például, hogy a `BatchClient` példány köteg fiókot, miközben a környezeti változók az ügyféltől az URL-CÍMEK és a köteg fiók megosztott kulcs hitelesítő kód átviheti. Hasonlóképpen a tárterület-fiókot a köteg fiók csatolt eléréséhez átviheti a tárterület-fiók nevét, és a tárhely fiókkulcs környezet változók.

### <a name="pass-parameters-to-the-job-manager-template"></a>A Feladatkezelő sablonba paraméterekkel

Sok esetben célszerű Feladatonkénti paramétereket át a kezelő projektfeladat, a folyamat felosztása feladat ellenőrzése vagy állítsa be a projekt a tevékenységek. Ehhez parameters.json nevű a kezelő projektfeladat erőforrás-fájlként JSON fájl feltöltésekor. A paraméterek majd előfordulhat, hogy az elérhető a `JobSplitter._parameters` mező a projekt Manager sablonban.

>[AZURE.NOTE] A beépített paraméter kezelő csak a karakterlánc-karakterlánc szótárak támogatja. Ha összetett JSON értékeket adják át a paraméter értékként, szüksége lesz adják át az alábbi karakterláncként és elemezni a őket a feladat elválasztó vagy módosítsa az keretrendszer `Configuration.GetJobParameters` módot.

### <a name="pass-parameters-to-the-task-processor-template"></a>A tevékenység processzor sablonba paraméterekkel

A paramétereket is át a tevékenység processzor sablonnal végrehajtott egyes tevékenységek. Ugyanúgy, mint a projekt manager sablonnal a tevékenység processzor sablont keres nevű erőforrásfájl

Parameters.JSON, és ha található, mint a paraméterek szótár betölti azt. Néhány lehetőség a paramétereket át a tevékenység processzor feladatokat:

* A feladat paraméterek JSON újrafelhasználása. Ez hogyan működik jól, ha csak a paraméterei feladat szintű (például egy leképezési magasság és a szélesség) is. Ehhez, egy CloudTask létrehozása a projekt elválasztó hozzáadása a parameters.json file-objektum a kezelő projektfeladat ResourceFiles (`JobSplitter._jobManagerTask.ResourceFiles`) a CloudTask ResourceFiles gyűjteményéhez.

* Készítése a feladat elválasztó végrehajtását részeként tevékenység-specifikus parameters.json dokumentum feltöltése és hivatkozás, hogy a tevékenység, erőforrás fájlok gyűjteményében a blob. Ez a másik tevékenységeket más paraméterekkel van szükség. Példa lehet, ahol a keret index átadott paraméter a feladatot 3D-leképezés példa.

>[AZURE.NOTE] A beépített paraméter kezelő csak a karakterlánc-karakterlánc szótárak támogatja. Összetett JSON értékeket adják át a paraméter értékként szeretné, ha szüksége lesz adják át az alábbi karakterláncként és elemezni a őket a tevékenység processzor vagy módosítsa az keretrendszer `Configuration.GetTaskParameters` módot.

## <a name="next-steps"></a>Következő lépések

### <a name="persist-job-and-task-output-to-azure-storage"></a>Továbbra is fennáll a feladatot, és a tevékenység kimeneti Azure-tárolóhoz

Egy másik hasznos egy köteg megoldás fejlesztés alatt álló eszköze [Azure köteg fájl szabályok][nuget_package]. A .NET osztálytár (jelenleg az előzetes verzió) a köteg .NET-alkalmazásokban segítségével tárolása és feladat kimeneti értékeket szeretne és Azure-tárhelyről beolvasásához. A tár és a használat teljes vitafórum [feladat Azure köteg továbbra is fennáll, és a tevékenység kimeneti](batch-task-output.md) tartalmazza.

### <a name="batch-forum"></a>Köteg-fórum

Az [Azure köteg fórum] [ forum] MSDN webhelyen található kiváló hely az megvitatása a köteget, és a szolgáltatással kapcsolatos kérdéseket. Központi a fölé a hasznos "öntapadó" bejegyzéseket, és a, amíg a köteg megoldások generál merülnek fel.

[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[net_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobmanagertask.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[process_exitcode]: https://msdn.microsoft.com/library/system.diagnostics.process.exitcode.aspx
[vs_gallery]: https://visualstudiogallery.msdn.microsoft.com/
[vs_gallery_templates]: https://go.microsoft.com/fwlink/?linkid=820714
[vs_find_use_ext]: https://msdn.microsoft.com/library/dd293638.aspx

[diagram01]: ./media/batch-visual-studio-templates/diagram01.png
[solution_explorer01]: ./media/batch-visual-studio-templates/solution_explorer01.png
[solution_explorer02]: ./media/batch-visual-studio-templates/solution_explorer02.png
