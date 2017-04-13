<properties
    pageTitle="Azure köteg szolgáltatás alapjai |} Microsoft Azure"
    description="Tudnivalók a nagyméretű szélességi és HPC munkaterhelésekből az Azure köteg szolgáltatás használatával"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/22/2016"
    ms.author="marsma"/>

# <a name="basics-of-azure-batch"></a>Azure köteg alapjai

Azure köteg lehetővé teszi, hogy nagyszabású a párhuzamos és a nagy teljesítményű számítások (HPC) alkalmazások hatékony futtatását a felhőben. Egy platform-szolgáltatás, ütemezi a számítási igényű munkát futtatni szeretne a virtuális gépeken futó felügyelt gyűjteménye, és is automatikusan skála kiszámítására az igényeknek megfelelően a feladatok erőforrásokat.

A köteg szolgáltatás definiálhatók Azure számítási erőforrások párhuzamosan, és a méretezés az alkalmazások végrehajtani. Igény szerinti vagy ütemezett futtathatók, feladatok, és nem kell manuálisan, adja meg, és létrehozása és kezelése egy HPC fürthöz, egyes virtuális gépeken futó, virtuális hálózatok vagy egy bonyolult feladat ütemezési infrastruktúra tevékenység.

## <a name="use-cases-for-batch"></a>Használati eset köteg törlése

Köteg, felügyelt Azure szolgáltatás, amellyel *köteg feldolgozása* vagy *Köteg számítások*– néhány kívánt eredményét a hasonló feladatokat nagy mennyiségű futtatása. Azoknak a szervezeteknek, rendszeresen feldolgozása, átalakítás és elemzése a nagy mennyiségű adattal leggyakrabban használt a köteg számítások.

Köteg megfelelően működik alapvetően párhuzamos (más néven "embarrassingly párhuzamos") alkalmazások és munkaterhelésekből. Több tevékenység egyidejű vonhatnak több számítógépen, egyszerűen osztották alapvetően párhuzamos munkaterhelésekből.

![A párhuzamos feladatok][1]<br/>

Néhány példa a munkaterhelésekből, ez az eljárás használatával gyakran feldolgozása a következők:

* Pénzügyi kockázatkezelési modellezési
* Környezet és hidrológia adatelemzés
* Képek megjelenítése, elemzés és feldolgozása
* Multimédia-kódolás és transcoding
* Genetikai szekvencia-elemzés
* Műszaki terhelési elemzése
* Szoftver tesztelése

Köteg is végén csökkentése lépés a párhuzamos számításokat, és hajtsa végre az összetettebb HPC-munkaterhelésekből, például az [Üzenet továbbítása felület (MPI)](batch-mpi.md) alkalmazást.

Köteget, és az Azure megoldást HPC lehetőségekért összehasonlítása a [köteget, és a HPC megoldások](batch-hpc-solutions.md)című témakör tartalmaz.

## <a name="developing-with-batch"></a>A köteg kialakítása

A köteg párhuzamos munkaterhelésekből feldolgozása általában befejeződött programozás útján a [Köteg API -khoz](#batch-development-apis)használatával. A köteg API-khoz, és létrehozása és kezelése (virtuális gépeken futó) számítási csomópontok készletek feladatok ütemezése és a feladatok azokat a csomópontokat futtatni. Egy ügyfél alkalmazás vagy szolgáltatás, hogy a szerző elkészíti a használja a köteg API-k használatával kommunikál a köteg szolgáltatást.

Nagyméretű munkaterhelésekből folyamat a szervezet számára, vagy adja meg a szolgáltatás előtér ügyfeleinek, hogy futtathatnak projektek és tevékenységek – igény vagy időközönként – egy, több száz vagy akár a csomópontok ezer hatékony. Az eszközök, például az [Azure Data Factory](../data-factory/data-factory-data-processing-using-batch.md)kezeli nagyobb munkafolyamat részeként köteg is használhatja.

> [AZURE.TIP] Amikor készen áll alaposabban meg a köteg API-hoz a szolgáltatásokat biztosít a részletesebb megértéséhez, kivétele a [Köteg szolgáltatás áttekintése a fejlesztők számára](batch-api-basics.md).

### <a name="azure-accounts-youll-need"></a>Azure fiókok szüksége lesz

Ha köteg alkalmazásokat fejleszt, az alábbi fiókok Microsoft Azure megadnia.

- **Azure-fiók és az előfizetés** – Ha még nem rendelkezik egy Azure-előfizetést, az [MSDN előfizetői összekapcsolhatók]is aktiválhatja[msdn_benefits], vagy egy [ingyenes Azure-fiók][free_account]. Amikor létrehoz egy fiókot, alapértelmezett előfizetés létrejön.

- Hitelesítő adatok **Köteg fiók** – Ha az alkalmazások használata a köteg szolgáltatás, a fiók nevét, az URL-címet, és a fiók hívóbetű használják. A köteg erőforrások készletek, például kiszámítania csomópontok, feladatok, és a feladatok fiókhoz társítva egy köteg. Azt is megteheti, hogy a [Köteg fiók létrehozása](batch-account-create-portal.md) az Azure-portálon.

- **Tárterület-fiókot** – köteg beépített támogatja az [Azure-tárolóban]lévő fájlok használata[azure_storage]. Szinte minden köteg a példában az Azure tároló – a tevékenységek futtatott programok és az adatok, amelyek a feldolgozás átmeneti tárolására szolgáló, és az általuk létrehozott kimeneti adatok tárolására. Tárterület-fiók létrehozása, tanulmányozza a [kapcsolatos Azure tároló fiókok](./../storage/storage-create-storage-account.md).

### <a name="batch-development-apis"></a>Köteg fejlesztési API-hoz

Az alkalmazások és szolgáltatások állíthatnak közvetlen REST API-hívást, használja az alábbi ügyfél-tárak, vagy mindkettőt kezelése közül egy vagy több számítja ki, erőforrások és a Futtatás párhuzamos terhelés a méretezés a köteg szolgáltatással.

| API    | API-hivatkozás | Letöltés | Mintakódok |
| ----------------- | ------------- | -------- | ------------ |
| **A köteg többi** | [AZ MSDN WEBHELYEN][batch_rest] | A #HIÁNYZIK | [AZ MSDN WEBHELYEN][batch_rest] |
| **A köteg .NET**    | [AZ MSDN WEBHELYEN][api_net] | [NuGet][api_net_nuget] | [GitHub][api_sample_net] |
| **Köteg Python**  | [readthedocs.IO][api_python] | [PyPI][api_python_pypi] |[GitHub][api_sample_python] |
| **Köteg Node.js** | [github.IO][api_nodejs] | [npm][api_nodejs_npm] | - |
| **Java köteg** (előzetes verzió) | [github.IO][api_java] | [Maven tesztelése][api_java_jar] | [GitHub][api_sample_java] |

### <a name="batch-resource-management"></a>Erőforrás-kezelés köteg

Mellett az ügyfélprogram API-khoz is használhatja az alábbi erőforrások kezelése a köteg számla belüli.

- [A köteg PowerShell-parancsmagok][batch_ps]: az Azure köteg parancsmagok az [Azure PowerShell](../powershell-install-configure.md) modul lehetővé teszi azok kezelése a PowerShell köteg erőforrásokat.

- [Azure CLI](../xplat-cli-install.md): az Azure parancssori kezelőfelületről Azure egy platformok toolset, amelybe rendszerhéj-parancsok használata a sok Azure szolgáltatást, többek között a köteget.

- [Köteg Management .NET](batch-management-dotnet.md) ügyfél tár: szintén kapható [NuGet][api_net_mgmt_nuget], programozás útján kezelése a köteg fiókok, kvóták és alkalmazáscsomagok a köteg Management .NET ügyfél tár is használhatja. Hivatkozás az [MSDN webhelyen]a könyvtár[api_net_mgmt].

### <a name="batch-tools"></a>Köteg eszközök

Miközben össze a köteg használó megoldások nem szükséges, az alábbiakban néhány értékes eszköz használata közben összeállítását, és a köteg-alkalmazások és szolgáltatások hibakeresése során.

 - [Azure portál][portal]: hozhat létre, figyelésére és törölhet köteg készletek, feladatok és az Azure portál köteg pengéit tevékenységek. Megtekintheti az állapotinformációkat, e, és más erőforrások, miközben a feladat futtatása, sőt akár fájlokat letölteni a készletek számítási csomópontjának (letöltése sikertelen tevékenység `stderr.txt` közben például a hibaelhárítás). Jelentkezzen be a csomópontok kiszámításához használt távoli asztali RDP-fájlokat is letöltheti.

 - [Azure köteg Explorer][batch_explorer]: köteg Explorer hasonló köteg erőforrás management szolgáltatást nyújt az Azure-portálra, de a Windows Presentation Foundation (WPF) ügyfélalkalmazás önálló. A köteg .NET minta alkalmazásokban elérhető [GitHub][github_samples], Visual Studio 2015 vagy a fenti összeállítása azt, és annak segítségével a Tallózás gombra, és erőforrások köteg fiókban kidolgozása és a köteg megoldások hibakeresése során. Feladat megtekintése, a készlet és a tevékenység részleteit, töltse le a számítási csomópontok fájlokat, és csatlakozzon csomópontok távolról távoli asztali RDP-fájlokat letöltheti a köteg Intézővel.

 - [Microsoft Azure tároló Explorer][storage_explorer]: nem feltétlenül egy köteg Azure eszközt, miközben a tárhely Intéző egy másik értékes eszköz, hogy a fejlesztés és a köteg megoldások hibakeresése során.

## <a name="scenario-scale-out-a-parallel-workload"></a>Alkalmazási helyzetek: Skála meg a párhuzamos terhelést

Gyakori megoldást, amelyet használ a köteg API-k használata a köteg szolgáltatással magában foglalja a méretezés kifelé alapvetően párhuzamos munka – például a képek 3D jelenetek – a számítási csomópontok erőforráskészlethez tartozik a megjelenítését. A számítási csomópontok készlet lehet a "leképezési farm" biztosítja az tízesre, több száz vagy akár több ezer magmintákat a leképezés feladat például.

A következő ábrán egy közös köteg munkafolyamat ügyfélalkalmazás vagy párhuzamos terhelési futtatásához köteg szolgáltatott szolgáltatás.

![Köteg megoldás munkafolyamat][2]

Gyakori ebben az esetben az alkalmazás vagy szolgáltatás folyamatok Azure kötegben számítási terhelési hajt végre az alábbi lépéseket:

1. Töltse fel a **bemeneti fájlok** és az **alkalmazás** dolgozza fel ezeket a fájlokat a tárhely Azure-fiókjába. A beviteli fájlokat lehet adatokat dolgozza fel az alkalmazást, például adat pénzügyi modellje vagy videofájlok alakíthatók át. Az alkalmazás fájljait lehet bármely alkalmazásban, amellyel az adatokat, például egy alkalmazás 3D-leképezés vagy media transcoder feldolgozása.

2. A köteg **készlet** létrehozása a számítási csomópontok a köteg fiókjában – a csomópontok a virtuális gépeken futó, amely a feladatok hajtja végre. Azure tároló az alkalmazás telepítése, ha a csomópontok Bekapcsolódás a készlet (az Ön által feltöltött lépésben #1 alkalmazás) tulajdonságait, például a [méret csomópontot](./../cloud-services/cloud-services-sizes-specs.md), az operációs rendszer és a helyet ad meg. Dinamikusan is beállíthatja a készlet [automatikus méretezése](batch-automatic-scaling.md)– a készletben – a terhelést a feladatok generáló válaszul számítási csomópontok számának módosítása.

3. Hozzon létre egy köteg **feladat** futtatni szeretne a terhelést a számítási csomópontok készlete. Amikor létrehoz egy feladatot, akkor azt társítása egy köteg készlet.

4. **Tevékenységek** hozzáadása a projekthez. Tevékenységek hozzáadása a feladatot, amikor a köteg szolgáltatás automatikusan ütemezi a tevékenységek végrehajtásához a készletben számítási csomóponton. Minden tevékenység használja az alkalmazás, a bemeneti fájlok feldolgozása feltöltött.

    - 4a. Mielőtt egy feladatot hajt végre, azt az adatokat (a bemeneti fájlok) és a számítási csomópontra hozzá van rendelve folyamat töltheti le. Ha az alkalmazás nem már telepítve van a csomóponton (lásd: a #2), letölthető itt helyette. Amikor elkészült a letöltéseket, a feladatok hajtsa végre a tevékenységhez rendelt csomópontot.

5. Futtassa a feladatokat, mint a feladatot, és a tevékenységekhez nyomon, hogy a köteg is lekérdezhetők. Az ügyfél-alkalmazás vagy szolgáltatás kommunikál a köteg szolgáltatás HTTPS, és mivel előfordulhat, hogy a számítási csomópontok ezer futó feladatok ezer figyelése, ne felejtse el [a köteget szolgáltatás hatékony lekérdezés](batch-efficient-list-queries.md).

6. A kész feladatokat, mint az eredményül kapott adatokat Azure-tárolóhoz feltöltheti őket. Fájlok közvetlenül a számítási csomópontokat is meghallgathatja.

7. A figyelés azt észleli, hogy befejeződött-e a projekt feladatait, ha az ügyfél-alkalmazás vagy szolgáltatás töltheti feldolgozásra vagy kiértékelés kimeneti adatai.

Tartsa szem előtt az csak egyirányú használni a köteget, és ebben az esetben csak a rendelkezésre álló szolgáltatásokat néhány ismerteti. [Több tevékenység párhuzamosan](batch-parallel-node-tasks.md) minden számítási csomópont hajthat végre, és a [előkészítése és a Befejezés projektfeladatok](batch-job-prep-release.md) segítségével a csomópontok előkészítése a feladatokat, majd utána jelenjenek meg.

## <a name="next-steps"></a>Következő lépések

Most, hogy a köteg szolgáltatás átfogó, pedig alaposabban megtudhatja, hogyan vele a számítási igényű párhuzamos munkaterhelésekből feldolgozása.

- Olvassa el a [köteget szolgáltatás áttekintése a fejlesztők számára](batch-api-basics.md), a köteg használandó előkészítése az alapvető információk. A cikk további információt az köteg szolgáltatás erőforrások, mint a készletek, a csomópontok, a feladatok, és a tevékenységek és a sok API-szolgáltatások, amelyek segítségével használhatja a köteg-alkalmazás létrehozása során tartalmazza.

- [Első lépések az Azure köteg tárat .NET](batch-dotnet-get-started.md) C# és a köteg .NET-tár használata végrehajtása egy egyszerű terhelést közös köteg munkafolyamat használata című témakörből. Ez a cikk az első kivételével közben a köteg szolgáltatás használatának elsajátítása egyikét kell lennie. Az oktatóprogram [Python verzióját](batch-python-tutorial.md) is van.

- Töltse le a [mintakódok a GitHub] [ github_samples] hogyan C# és is Python is csatlakozhatnak a köteg az ütemterv és a folyamat minta munkaterhelésekből megjelenítéséhez.

- Nézze meg a [Köteg tanulási javaslat] [ learning_path] eléréséhez a rendelkezésre álló erőforrások arról, amint egyre köteg végezhető.

[azure_storage]: https://azure.microsoft.com/services/storage/
[api_java]: http://azure.github.io/azure-sdk-for-java/
[api_java_jar]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-batch%22
[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_nuget]: https://www.nuget.org/packages/Azure.Batch/
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_net_mgmt_nuget]: https://www.nuget.org/packages/Microsoft.Azure.Management.Batch/
[api_nodejs]: http://azure.github.io/azure-sdk-for-node/azure-batch/latest/
[api_nodejs_npm]: https://www.npmjs.com/package/azure-batch
[api_python]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html
[api_python_pypi]: https://pypi.python.org/pypi/azure-batch
[api_sample_net]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp
[api_sample_python]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[api_sample_java]: https://github.com/Azure/azure-batch-samples/tree/master/Java/
[batch_ps]: https://msdn.microsoft.com/library/azure/mt125957.aspx
[batch_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[free_account]: https://azure.microsoft.com/free/
[github_samples]: https://github.com/Azure/azure-batch-samples
[learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[msdn_benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[storage_explorer]: http://storageexplorer.com/
[portal]: https://portal.azure.com

[1]: ./media/batch-technical-overview/tech_overview_01.png
[2]: ./media/batch-technical-overview/tech_overview_02.png
