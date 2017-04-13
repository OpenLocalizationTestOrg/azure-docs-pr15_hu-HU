<properties
   pageTitle="Feladatok útmutatást háttér |} Microsoft Azure"
   description="Háttér útmutatást, hogy egymástól függetlenül, hogy a felhasználói felület Futtatás tevékenységek."
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="masashin"/>

# <a name="background-jobs-guidance"></a>Háttér feladatok útmutató

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>– Áttekintés

Alkalmazások számos különböző típusú megkövetelése a háttérben futó feladatokat, amelyek a felhasználói felületét függetlenül futnak. Többek között köteg feladatok, tőkeigényes feldolgozás feladatok és hosszabb ideig futó folyamatokhoz, mint a munkafolyamatokat. Háttér feladatok hajthatók végre felhasználói beavatkozás – az alkalmazás kérése nélkül is elindítani a feladatot, és folytassa a felhasználóktól interaktív összehívások. Ez segít a betöltés az alkalmazás felhasználói felület, amely javíthatja az elérhetőség és interaktív válasz idő csökkentése céljából.

Például ha az alkalmazás felhasználó által feltöltött képek miniatűrjei létrehozásához szükséges, képes művelet háttér feladatként és mentése a miniatűr tárhely, ha befejeződött a--anélkül, hogy a felhasználó a folyamat léptetése erre szolgáló hátralévő. Az adott dokumentumkészletből ugyanúgy a felhasználó úgy, hogy a megfelelő sorrendben kezdeményezhetnek, amely a sorrendben, folyamatok, miközben a felhasználói felület lehetővé teszi, hogy a felhasználó folytatja a web app böngészési háttér munkafolyamat. A háttérben futó feladat befejezésekor képes a rendelések tárolt adatok frissítése és e-mail küldése a felhasználót, hogy a sorrend nyugtázza.

Mikor érdemes megfontolni, hogy háttér feladatként feladat végrehajtásához a fő feltétel-e e a tevékenység futtatását is lehetővé teszi a felhasználói felületen és felhasználói beavatkozás nélkül erre szolgáló várniuk elvégzendő feladathoz. Előfordulhat, hogy a felhasználó vagy a felhasználói felület türelmet, azok befejezetlen igénylő feladatokkal nem megfelelő háttér feladatként.

## <a name="types-of-background-jobs"></a>Háttér feladatok típusai

Háttér feladatok rendszerint a következő típusú feladatokat közül:

- Processzor-igényes feladatok, például a matematikai számítások és a szerkezeti modell elemzési.
- E/O-igényes feladatok, például tároló tranzakciók sorozata végrehajtása vagy fájlok indexelés.
- Köteg feladatokat, például éjszakai adatok frissítések vagy ütemezett feldolgozása.
- Hosszabb ideig futó munkafolyamatok, például a sorrend lehetőséget, vagy a szolgáltatások és rendszerek kiépítési.
- Ha a tevékenység elkészül feldolgozásra biztonságosabbá helyre érzékeny--adatfeldolgozás. Ha például nem célszerű webalkalmazást bizalmas adatainak feldolgozása. Ehelyett például [forgalomirányító](http://msdn.microsoft.com/library/dn589793.aspx) mintát segítségével előfordulhat, hogy az adatok átvitele egy elszigetelt háttér folyamat, amely a védett tároló hozzáféréssel rendelkezik.

## <a name="triggers"></a>Eseményindítók

Háttér feladatok többféle módon kezdeményezheti. A következő kategóriák közül esnek:

- [**Eseményvezérelt indítók**](#event-driven-triggers). A tevékenység egy eseményt, általában egy felhasználó vagy az munkafolyamatokban lépés által készített műveletet reagálni elindul.
- [**Ütemezés-alapú indítók**](#schedule-driven-triggers). A tevékenység időzítőt alapuló ütemezés hivatkoznak. Ez lehet egy ismétlődő ütemezzen, illetve egy egyszeri meghíváshoz, később meg van adva.

### <a name="event-driven-triggers"></a>Eseményvezérelt indítók

Eseményvezérelt meghívási használja az eseményindító indítja el a háttér feladatot. Eseményvezérelt indítók segítségével többek között:

- A felhasználói felület vagy egy másik feladat helyez el egy üzenetet várólista. Az üzenet megtörtént, például a felhasználó úgy, hogy a megfelelő sorrendben művelet adatait tartalmazza. A háttér tevékenység ebben a sorban lévő figyeli, és egy új üzenet érkezéséről észlel. Ez a felolvassa az üzenet, és a háttérben futó feladat a bemeneti adataiként használja az adatokat.
- A felhasználói felület vagy egy másik feladat menti, vagy frissíti egy értéket tároló. A háttér feladat figyeli tárolására, és észleli a módosításokat. Ez beolvassa az adatokat, és a háttérben futó feladat a bemeneti adataiként használja.
- A felhasználói felület vagy a állást változtat módosít egy kérelmet zárólap, például egy HTTPS URI és az API-val, amely egy webszolgáltatásból van közzétéve. Továbbítja az adatokat, végezze el a háttér feladatot a kérelem részeként szükséges. Az endpoint vagy a webes szolgáltatás elindítja a háttér tevékenységhez, amely az adatokat használja annak a bemeneti adataiként.

Eseményvezérelt meghívási alkalmas feladatok tipikus közé feldolgozása, munkafolyamatok, adatokat küld a távoli szolgáltatás, e-mailek küldése és új felhasználók multitenant alkalmazásokban kiépítési képe.

### <a name="schedule-driven-triggers"></a>Ütemezés-alapú indítók

Ütemezés-alapú meghívási indítsa el a háttér tevékenység időzítőt használja. Ütemezés-alapú indítók segítségével többek között:

- Az alkalmazáson belül, illetve az alkalmazás operációs rendszer részeként helyileg futtató időzítőt rendszeresen háttér tevékenység elindítja.
- Egy másik alkalmazás vagy Azure ütemezőt, például egy időzítőszolgáltatás futtató időzítőt kérést küld az API-val, vagy a webes szolgáltatásainak rendszeresen. Az API-val, vagy a webes szolgáltatás indítja el a háttér feladatot.
- Egy külön folyamatot vagy egy alkalmazás elindul a háttérben tevékenység egyszer az adott időben késleltetés után, vagy egy adott időpontban képezheti okozó időzítőt.

Ütemezés-alapú meghívási alkalmas feladatok tipikus közé parancsfájl funkciók (például a felhasználóknak a legutóbbi működésük alapján kapcsolódó termékek listák frissítése), a rendszeres adatkezelési feladatok (például az indexek frissítéséhez vagy halmozott eredményeket), a napi jelentések, az adatok adatmegőrzési törlése és az adatok összefüggések az adatelemzés.

Ütemezés-alapú feladat, amely egyetlen példányt futnia kell használatakor tartsa szem előtt a következőket:

- A számítási példányt az ütemező (például a Windows ütemezett tevékenységek segítségével virtuális gép) futtató átméretezi, ha több példányával az operációs rendszert futtató ütemezőt lesz. Ezek sikerült indítsa el a tevékenység több példányon.
- Ha a tevékenységek között ütemezőt események hosszabb, mint az időszak futtatásához az ütemező kezdheti el a tevékenység egy másik példányát az előzőt futása közben.

## <a name="returning-results"></a>Ad eredményt

Háttér feladatok aszinkron egy külön folyamatot, vagy akár egy külön helyet, a felhasználói felület vagy a folyamat, amely a háttérben tevékenység meghívott. Ideális esetben háttérben futó feladatokat "fire és elfelejti" műveleteket, és a végrehajtás folyamatban van nincs hatással a felhasználói felület vagy a hívó folyamat. Ez azt jelenti, hogy a hívó folyamat nem várja meg, a feladatok végrehajtására. Emiatt azt nem lehet automatikusan észleli a tevékenység befejeződésekor.

Kommunikáció a hívó tevékenység előrehaladását vagy befejezési háttér tevékenység van szüksége, ha a be kell állítani egy eljárást. Néhány példa a következők:

- A felhasználói felület vagy a hívó tevékenységhez, amelyhez megfigyelése, és jelölje be ezt az értéket, szükség esetén is elérhető tároló állapotát jelző érték írni. Egyéb adatok, amelyek a háttérben tevékenység vissza kell térnie a hívó elhelyezhető az azonos tárolóba.
- Hozzon létre egy válasz várólista, amelyek a felhasználói felület vagy a hívó figyeli. A háttér tevékenység üzeneteket küldhet a jelölő állapota és befejezése várakozási sorban található. A háttér tevékenység vissza kell térnie a hívó adatokat elhelyezhető az üzenetbe. Azure Service Bus használja, ha ez a képesség végrehajtásához **ReplyTo** és **CorrelationId** tulajdonságainak is használhatja. További tudnivalókért lásd: [a szolgáltatás Bus Brokered üzenetküldés korrelációs](http://www.cloudcasts.net/devguide/Default.aspx?id=13029).
- Az API vagy végpontot, amelyet a felhasználói felület vagy a hívó elérhet beszerezni az állapotinformációkat háttér tevékenységből nézetéhez. A válasz, a háttérben tevékenység vissza kell térnie a hívó adatait beépíthetők.
- A visszahívási a felhasználói felület vagy a hívó keresztül egy API-t, előre definiált pontokon vagy a befejezéskor állapot jelzése háttér tevékenység van. Ez az valószínűleg az eseményeket, emelt helyileg vagy a közzététel és előfizetés mechanizmusa keresztül. A kérés vagy esemény tartalom beépíthetők, amely a háttérben tevékenység vissza kell térnie a hívó adatait.

## <a name="hosting-environment"></a>Üzemeltetési környezet

Különböző Azure platform szolgáltatások tartomány használatával is megbízhat a háttérben futó feladatokat:

- [**Azure Web Apps és WebJobs**](#azure-web-apps-and-webjobs). A különböző típusú parancsfájlokat vagy a webalkalmazást környezetén belül végrehajtható programokat tartományon alapuló egyéni feladatok végrehajtásához WebJobs is használhatja.
- [**Azure Cloud Services webes és dolgozó szerepkörök**](#azure-cloud-services-web-and-worker-roles). Kód egy szerepkört, amely végrehajtja a háttérben feladatként belül is írhat.
- [**Azure virtuális gépeken futó**](#azure-virtual-machines). Egy Windows szolgáltatást, vagy a Windows Feladatütemező használandó, érdemes közös tárolni a háttérben futó feladatokat belül egy dedikált virtuális számítógépre.
- [**Azure köteget**](./batch/batch-technical-overview.md). Egy platform-szolgáltatás, ütemezi a számítási igényű munkát futtatni szeretne a virtuális gépeken futó felügyelt gyűjteménye, és is automatikusan skála kiszámítására az igényeknek megfelelően a feladatok erőforrásokat.

Az alábbi szakaszok ismertetik az egyes beállításokat részletesebben és szempontok segít a kívánt lehetőséget a használhat.

## <a name="azure-web-apps-and-webjobs"></a>Azure Web Apps és WebJobs

Háttérben futó feladatokat az Azure Web appból, egyéni feladatok végrehajtásához Azure WebJobs is használhatja. WebJobs folytonos folyamat futtatása a web App alkalmazásban a környezetén belül. WebJobs futtatása Azure ütemező vagy külső tényezők, például a tárhely BLOB és az üzenet sorban várakozó módosításai eseményindító esemény válasz is. Feladatok elindítása és leállítása igény és leállítása. Ha nem sikerül egy folyamatosan működő WebJob, a rendszer automatikusan újraindítja. Ismét és hiba műveletek: állítható be.

Ha egy WebJob konfigurálhat:

- Ha azt szeretné, hogy a feladat eseményvezérelt eseménykód válaszolni, mint **futtatáshoz**kell konfigurálása. A parancsfájlt vagy a program a webhely/wwwroot/app_data/feladatok/folyamatos mappájában vannak tárolva.
- Ha azt szeretné, hogy a feladat ütemezése-alapú eseményindító válaszolni, meg kell beállítani **időközönként futtassa**. A parancsfájlt vagy a program webhely/wwwroot/app_data/feladatok/indított mappájában vannak tárolva.
- Ha egy feladat konfigurálásakor az **igény szerinti futtatása** lehetőséget választja, azt nem futtatja a **időközönként futtassa** lehetőségként ugyanazt kódot, indításkor.

Azure WebJobs a védőfalas a web App belül futnak. Ez azt jelenti, hogy azok környezeti változók érhetik el és információt, például a kapcsolati karakterláncot, megoszthatja a web App alkalmazásban. A feladat az egyedi azonosító a feladat futtató számítógép hozzáfér. A kapcsolati karakterlánc **AzureWebJobsStorage** nevű alkalmazás adatok Azure tároló sorban várakozó, BLOB és táblák access és a kommunikációt és üzenetküldési szolgáltatás Bus hozzáférést biztosít. A kapcsolati karakterlánc **AzureWebJobsDashboard** nevű feladat művelet naplófájlokba jegyzéséhez hozzáférést biztosít.

Azure WebJobs a következő jellemzőkkel rendelkeznek:

- **Biztonsági**: WebJobs eső a web app telepítési hitelesítő adatait.
- **Támogatott fájltípusok**: WebJobs megadása parancsfájlok (.cmd), a köteg fájlok (.bat), a PowerShell-parancsfájlokat (.ps1) használatával is bash rendszerhéj parancsfájlok (.sh), PHP parancsfájlok (.php), Python parancsfájlok (.py), JavaScript-kód (.js) és a végrehajtható programok (.exe, .jar és az egyéb).
- **Telepítés**: telepítheti parancsprogramokat és a végrehajtható fájl használatával az Azure portál [WebJobsVs](https://visualstudiogallery.msdn.microsoft.com/f4824551-2660-4afa-aba1-1fcc1673c3d0) hozzáadása a Visual Studio vagy a [Visual Studio 2013 frissítés 4](http://www.visualstudio.com/news/vs2013-update4-rc-vs) (létrehozhat és üzembe ezt a beállítást választja), az [Azure WebJobs SDK](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)használatával, vagy másolja őket közvetlenül a következő helyeken:
  - A végrehajtás kezdeményezése: webhely/wwwroot/app_data/feladatok/indított / {feladat nevét}
  - A folyamatos végrehajtási: webhely/wwwroot/app_data/feladatok/folyamatos / {feladat nevét}
- **Naplózás**: Console.Out INFO (megjelölt) értéke. Hiba Console.Error kell kezelni. Az Azure portal segítségével elérheti a figyelés és diagnosztikai információ. Naplófájlok letöltheti közvetlenül a webhelyről. Mentés a következő helyeken:
  - A végrehajtás kezdeményezése: Vfs/adatok/feladatok/indított/feladat
  - A folyamatos végrehajtási: Vfs/adatok/feladatok/folyamatos/feladat
- **Konfigurációs**: WebJobs beállíthatja a portálon, a REST API-val és a PowerShell használatával. Konfigurációs fájl a feladat parancsfájl, ugyanabban a legfelső szintű könyvtárban settings.job nevű feladat konfigurációs adatokat is használhatja. Példa:
  - {"stopping_wait_time": 60}
  - {"is_singleton": igaz}

### <a name="considerations"></a>Megfontolandó szempontok

- Alapértelmezés szerint WebJobs méretezze át a web app alkalmazással. Feladat egyetlen példányt a **true** **is_singleton** konfigurációs tulajdonságának beállításával is beállíthatja. Egyetlen példányt WebJobs hasznosak azon feladatokhoz, amelyekhez nem szeretné, méretezheti vagy futtatás egyidejű több mint-példányok, például újraindexelés, az adatelemzés és a hasonló feladatokat.
- Milyen következményekkel járnak a web app teljesítményének feladatok minimalizálásához fontolja meg egy üres Azure Web App-példány intenzív host, előfordulhat, hogy hosszú operációs rendszert futtató WebJobs vagy erőforrásra alkalmazás szolgáltatás új csomag létrehozása.

### <a name="more-information"></a>További információ

- [Azure WebJobs ajánlott erőforrások](./app-service-web/websites-webjobs-resources.md) listáit a számos hasznos források, letöltésekre és a minták WebJobs.

## <a name="azure-cloud-services-web-and-worker-roles"></a>Azure Cloud Services webes és dolgozó szerepkörök

Háttérben futó feladatokat egy webes szerepkör belül, illetve egy külön dolgozó szerepkör hajthat végre. Amikor arról dönt hogy dolgozó szerepkörbe használni, fontolja meg a méretezhetőség és a rugalmasság követelményeket, a tevékenység élettartam, engedje fel az cadence, biztonsági, hibatűrést, kérelem, összetettsége és a logikai architektúra. További tudnivalókért lásd: az [Erőforrás összesítés mintát számítja ki](http://msdn.microsoft.com/library/dn589778.aspx).

Többféle módon tudnak megvalósítani a háttérben futó feladatokat a felhőalapú szolgáltatások szerepkör belül:

- Hozzon létre egy végrehajtása a **RoleEntryPoint** osztály szerepkör és módszerek a háttérben futó feladatokat végrehajtani. A feladatok futtassa a WaIISHost.exe környezetében. Töltse be a beállítások segítségével az **CloudConfigurationManager** osztály **GetSetting** metódusát. További tudnivalókért lásd: [életciklusra (Cloud Services)](#lifecycle-cloud-services).
- Indítási feladatok használata a háttérben futó feladatokat végrehajtani, amikor elindítja az alkalmazást. Hogy a feladatokat, hogy továbbra is futtathatók a háttérben, a tulajdonság **taskType** **háttér** (Ha még nem ez, az alkalmazás indítási folyamat fog megállítás, és várja meg a tevékenység befejezéséhez). További tudnivalókért olvassa el a [futtassa az Indítás feladatok Azure-ban](./cloud-services/cloud-services-startup-tasks.md)című témakört.
- Háttérben futó feladatokat, például az indítási feladatként kezdeményezett WebJobs végrehajtásához használja a a WebJobs SDK csomagjában talál. További tudnivalókért lásd: [Ismerkedés az Azure WebJobs SDK a](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md).
- Indítási tevékenység használatával, amely egy vagy több hátteret feladatok végrehajtása Windows-szolgáltatás telepítése. Be kell a **taskType** tulajdonság **háttér** , hogy a szolgáltatás a háttérben hajt végre. További tudnivalókért olvassa el a [futtassa az Indítás feladatok Azure-ban](./cloud-services/cloud-services-startup-tasks.md)című témakört.

### <a name="running-background-tasks-in-the-web-role"></a>Háttérben futó feladatokat a webes szerepkör fut

A fő futtatása a háttérben futó feladatokat webes szerepkör előnye a Mentés szolgáltatója költségek, mert nem szükséges, a további szerepkörök üzembe.

### <a name="running-background-tasks-in-a-worker-role"></a>Háttérben futó feladatokat dolgozó szerepkörbe fut

Háttérben futó feladatokat futó dolgozó szerepkörbe számos előnye van:

- Lehetővé teszi a méretezés külön-külön az egyes: a szerepkör kezelése. Ha például szükség lehet egy webes szerepkör az aktuális terheltsége támogatásához további előfordulásait, de kevesebb példányok dolgozó szerepkör végrehajtja a háttérben futó feladatokat. Háttér tevékenység számítási példányok elkülönítve a felhasználói felület szerepkörök beosztását, csökkentheti üzemeltetési költségek, elfogadható teljesítmény megőrzésével.
- A feldolgozási terhelést a háttérben futó feladatokat a webes szerepkörből offloads azt. A webes szerepkört, amely a felhasználói Felületet biztosít maradhat válaszol, és kevesebb példányok van szükség a támogatási kérelmek a felhasználó adott mennyiségű jelentheti.
- Lehetővé teszi a aggályokat szétválasztására végrehajtásához. Szerepkör-típusokhoz segítségével miként állíthat pontosan meghatározott és kapcsolódó feladatok meghatározott. Így tervezése és a kód könnyebben fenntartása mivel kevesebb kölcsönös függőség kód és funkciók minden szerepkör között.
- Bizalmas folyamatok és az adatok elkülönítése segítséget. Például webes szerepkörök, amelyek a felhasználói felület végrehajtása nem kell lennie, amely kezeli és dolgozó szerepkör által vezérelt adatokhoz való hozzáférés. Ez lehet hasznos biztonsági, erősítése, különösen ha mintát, például a [Forgalomirányító mintát](http://msdn.microsoft.com/library/dn589793.aspx)használja.  

### <a name="considerations"></a>Megfontolandó szempontok

Vegye figyelembe az alábbiakat, hogy hol és hogyan kiválasztásakor bevezetését tervezi a háttérben futó feladatokat Cloud Services webes és dolgozó szerepkörök használata esetén:

- Egy meglévő webes szerepkört a háttérben futó feladatokat tároló mentheti, hogy csak az alábbi műveleteket külön dolgozó szerepkört futó költségét. Jó helyen jár akkor valószínű hatással lehetnek a teljesítmény és az alkalmazás elérhetőségét az kérelem feldolgozó és más erőforrások esetén. A webes szerepkör külön dolgozó szerepkör alapján megakadályozza a milyen következményekkel járnak hosszabb ideig futó vagy az erőforrás-igényes háttérben futó feladatokat.
- Ha Ön üzemelteti a háttérben futó feladatokat a **RoleEntryPoint** osztály használatával, egyszerűen áthelyezheti ez más szerepkört. Például ha az osztályjegyzetfüzet létrehozása a webes szerepkört, és később úgy dönt, kell futtatni a feladatok dolgozó szerepkör, áthelyezheti **RoleEntryPoint** osztály végrehajtása a dolgozó szerepkör be.
- Indítási feladatokat végrehajtani a program vagy parancsfájlt készültek. Üzembe helyezése a háttérben futó feladat végrehajtható programként nehezebben, különösen akkor, ha a függő összeállítások telepítésének is szükséges lehet. Üzembe helyezéséhez és a háttérben futó feladat definiálása az indítási feladatok használatakor parancsfájl használatával könnyebben lehet.
- Kivételek okozó meghiúsító háttér tevékenység egy másik hatása attól függően, hogy azok is módja van:
  - A **RoleEntryPoint** osztály megközelítés használata esetén sikertelen tevékenység okoz a szerepkört, hogy a tevékenység újraindul újraindításához. Ez az alkalmazás elérhetőségét az hatással lehet. Ennek elkerülése érdekében győződjön meg arról, hogy a **RoleEntryPoint** osztály és a háttérben futó feladatokat belül kezelése robusztus kivétel tartalmazzák. Indítsa újra a feladatokhoz, amelyekhez nem sikerül, amennyiben ez esetben a kód használatával, és indítsa újra a szerepkört, csak akkor, ha Ön nem biztonságosan helyreállítása a hibát a kód kivételt throw.
  - Indítási tevékenységek használata esetén, a feladat-végrehajtás kezelése és felelősek ellenőrizni, hogy az nem.
- Kezelés és figyelése indítási feladatok nehezebben, mint a **RoleEntryPoint** osztály megközelítés használatával. Azonban az Azure WebJobs SDK tartalmaz egy irányítópult, így azok könnyebben WebJobs készül megosztani – indítási tevékenységek kezelése.

### <a name="more-information"></a>További információ

- [Kiszámítása az erőforrás összesítés mintával](http://msdn.microsoft.com/library/dn589778.aspx)
- [Az Azure WebJobs SDK – első lépések](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)

## <a name="azure-virtual-machines"></a>Azure virtuális gépeken futó

Előfordulhat, hogy oly módon, hogy megakadályozza, hogy az üzembe helyezéséhez és Azure Web Apps alkalmazások Cloud Services szerepelni fog háttérben futó feladatokat, vagy ezeket a beállításokat nem lehet hasznos. Jellemző példák a Windows-szolgáltatásokkal, és a külső segédeszközök és a végrehajtható programok. Lehet, hogy egy másik példa eltér attól, hogy az alkalmazás szolgáltatója végrehajtás környezethez írt programot. Lehet például végrehajtása Windows vagy a .NET-alkalmazásból szeretné Unix vagy Linux programot. Válassza az Azure virtuális gép operációs rendszerek egy cellatartományból, és a szolgáltatás és a végrehajtható fájl futtatása virtuális a gépen.

Mikor érdemes használni a virtuális gépeken futó választhassa érdekében olvassa el az [Azure alkalmazás szolgáltatások, a Felhőszolgáltatások és a virtuális gépeken futó összehasonlító](./app-service-web/choose-web-site-cloud-service-vm.md). A virtuális gépeken futó beállítások tudni lásd: [Azure virtuális gép és Felhőbeli szolgáltatástól méretét](http://msdn.microsoft.com/library/azure/dn197896.aspx). Operációs rendszerek és virtuális gépeken futó elérhető beépített képek kapcsolatos további tudnivalókért lásd: [Microsoft Azure virtuális gépeken futó piactéren](https://azure.microsoft.com/gallery/virtual-machines/).

A háttér tevékenység egy külön virtuális gépen kezdeményezéséhez lehetőségek közül választhat:

- Közvetlenül a levelezőprogramból igény a tevékenység hajthat végre egy kérést küld, a tevékenység közzététele zárólap. Ezzel adja meg a kívánt adatokat a feladathoz. A végpont elindítja a tevékenységhez.
- Beállíthatja, hogy a feladat futását időközönként a Feladatütemező vagy a választott operációs rendszerben elérhető időzítő használatával. Például a Windows is használhatja a Windows Feladatütemező parancsprogramokat és a feladatokat végrehajtani. Vagy ha az SQL Server virtuális a gépre van telepítve, használhatja az SQL Server-Agent parancsprogramokat és a feladatok végrehajtása.
- A tevékenység kezdeményezéséhez, üzenet, amely a feladat figyeli várólista hozzáadásával vagy egy kérést küld egy API-t, hogy a tevékenység közzététele Azure ütemező is használhatja.

Lásd: a korábbi szakaszban [Eseményindítók](#triggers) , hogyan kezdeményezhet a háttérben futó feladatokat további információt.  

### <a name="considerations"></a>Megfontolandó szempontok

Amikor arról dönt hogy-e a háttérben futó feladatokat az Azure virtuális gépen üzembe, vegye figyelembe az alábbiakat:

- Háttérben futó feladatokat külön Azure virtuális gépen szolgáltatója rugalmasságot biztosít, és lehetővé teszi a kezdeményezési, végrehajtás, ütemezés és erőforrás-hozzárendelés pontosan szabályozható. Azonban futtatókörnyezet költség esetén megnöveli virtuális gépen kell üzembe helyezni csak futtatása a háttérben futó feladatokat.
- Nincs lehetőség a feladatokat az Azure-portálra, és nincs automatikus újraindítás képesség sikertelen tevékenységek – a Lync-figyelheti a virtuális gép egyszerű állapotát és kezelheti a az [Azure Service kezelő parancsmagok](http://msdn.microsoft.com/library/azure/dn495240.aspx)használatával nem. Azonban nem állnak rendelkezésre folyamatok és a számítási csomópontok beszélgetésekben. Általában használatáról virtuális gép esetén kell egy által gyűjtött adatokat, a tevékenység műszerezettségi, és a virtuális gépen az operációs rendszer eljárás végrehajtásához további munkamennyiség. Egy megoldást, amely szükség lehet a [System Center felügyeleti csomag az Azure](http://technet.microsoft.com/library/gg276383.aspx)környezetbe.
- Akkor is érdemes megfontolni, hogy HTTP végpontok keresztül elérhető felügyeleti szondákat létrehozása. A következő szondákat kódját sikerült végezze el az állapot-ellenőrzései, statisztika – és műveleti információk összegyűjtése vagy Szétválogatás hibaüzenet adatainak és vissza felügyeleti alkalmazás. További tudnivalókért lásd: [Állapot végpont figyelése mintát](http://msdn.microsoft.com/library/dn589789.aspx).

### <a name="more-information"></a>További információ

- [Virtuális gépeken futó](https://azure.microsoft.com/services/virtual-machines/) a Azure
- [Azure virtuális gépeken futó – gyakori kérdések](virtual-machines/virtual-machines-linux-classic-faq.md)

## <a name="design-considerations"></a>Tervezési szempontok

Tényező néhány alapvető háttérben futó feladatokat tervezésekor figyelembe. Az alábbi szakaszokból megtudhatja, hogy szétválasztás ütközések és effektusával.

## <a name="partitioning"></a>Szétválasztás

Ha úgy dönt, amelyet fel szeretne venni a háttérben futó feladatokat belül egy meglévő számítási példány (például web App alkalmazásban, webes szerepkör, meglévő dolgozó szerepkör vagy virtuális gépen), figyelembe kell vennie hogyan ez hatással van a számítási példány és a háttér tevékenység magát a minőség tulajdonságait. Ezek a tényezők segítségével döntse el, hogy a tevékenységeket, amelyek a meglévő számítási példány colocate vagy egy különálló számítási példány külön őket:

- **Elérhetőség**: háttérben futó feladatokat előfordulhat, hogy nem kell elérhetőségét, mint a többi az alkalmazás részei azonos szintű különösen a felhasználói felületen és más felhasználói beavatkozás közvetlenül részt. Lehet, hogy alternatív késés, próbálkozott újra csatlakozási hibák, a háttérben futó feladatokat és egyéb adott befolyásolja elérhetősége tényezők, mivel a műveletek sorba állítható. Azonban kell lennie, hogy a biztonsági mentés sikerült letiltani a sorok és az alkalmazás egészének mérete befolyásolja elegendő kapacitása.
- **Méretezhetőség**: háttérben futó feladatokat valószínűleg egy másik méretezhetőség követelmény a felhasználói felület és a interaktív az alkalmazás részei. A felhasználói felület méretezés lehet, hogy csúcsok igény szerint, az alkalmazást, miközben nyitott háttérben futó feladatokat előfordulhat, hogy egy annál kevesebb töltik kevesebb elfoglalt időszakokban számítási példányok száma.
- **Tűrőképessége**: csak a háttérben futó feladatokat tároló példány számítási hiba lehet, hogy nem gyermekekéhez érinti egészének Ha ezeket a feladatokat a kérelem aszinkron, vagy halasztani mindaddig, amíg a tevékenység érhető el újra. A számítási példány és/vagy a tevékenységek megfelelő elvégezve belül kell indítani, ha az alkalmazás felhasználóinak előfordulhat, hogy nem érinti.
- **Biztonsági**: Előfordulhat, hogy háttérben futó feladatokat különböző biztonsági követelményeknek vagy korlátozások, mint a felhasználói felület vagy az alkalmazás részei. Egy külön számítási példány használatával is megadhat egy másik biztonsági környezet tevékenységek. Például forgalomirányító mintázatok segítségével azonosítani a háttérben számítási példányok a felhasználói felületen annak érdekében, hogy a teljes méretűre állíthatja a biztonság és szétválasztása.
- **Teljesítmény**: megadhatja a számítási példány típusú, a háttérben feladatok kifejezetten egyezés a feladatok teljesítmény előírásainak. Ez lehet, hogy egy kisebb drága számítási beállítás használata, ha a tevékenységek nincs szükség a felhasználói felület vagy egy nagyobb példány ugyanolyan feldolgozás lehetőségeket, ha további kapacitás és erőforrások szükségük jelenti.
- **Kezelhetőség**: háttérben futó feladatokat előfordulhat, hogy egy másik fejlesztés és telepítési ritmust a fő alkalmazás kódot vagy a felhasználói felület. Egy külön számítási példányhoz helyezése egyszerűsíti a frissítéseket és a verziószámozás.
- **Költség**: háttér feladatok nő költségek szolgáltatója végrehajtásához példányok hozzáadása számítja ki. A további kapacitás és a költségeket a következő további közötti csökkentés gondosan figyelembe.

Többet olvassa el a [Vezető Election minta](http://msdn.microsoft.com/library/dn568104.aspx) és a [Fogyasztói mintát Gyorskorcsolyázásban](http://msdn.microsoft.com/library/dn568101.aspx).

## <a name="conflicts"></a>Ütközések feloldása

Ha több példányának egy háttér feladatot, akkor lehet, hogy azok fog versenyt az erőforrások és a szolgáltatások, például az adatbázisok és tároló eléréséhez. A párhuzamos hozzáférés erőforrás kérelem, amelyek miatt megtelhet az ütközések a szolgáltatások elérhetőségét a és a tárolóban lévő adatok integritását eredményez. Erőforrás kérelem a pesszimista zárolási megközelítést használatával javíthatja ki. Ez megakadályozza, hogy gyorskorcsolyázásban az adott tevékenységgel egyidejűleg elérése szolgáltatás adatokat vagy rongál példányát.

Ütközések feloldása egy másik megoldást, hogy a háttérben futó feladatokat, egyszeres meghatározásával, úgy, hogy minden eddiginél csak egy példánya fut. Jó helyen jár így nem megbízhatóságának és teljesítményének előnyökkel jár, amelyek a többszörös-példány konfiguráció nyújtanak. Ez a különösen értéke igaz, ha a felhasználói felület egynél több háttér feladat elfoglalt megtartása elegendő munka is megadhat.

Célszerű meggyőződni arról, hogy a háttér tevékenység is automatikusan indítsa újra az és, hogy rendelkezik-e elegendő kapacitása ahhoz, hogy az igény szerinti csúcsok alkalmazkodás elengedhetetlen. Cél hozzárendelése egy számítási példány elegendő erőforrásokkal, egy újabb végrehajtása esetén az igény szerinti kevesebb kérelem tárolható várólista mechanizmusa végrehajtása, illetve az alábbi eljárások együttes használatával.

## <a name="coordination"></a>Összehangolása

A háttérben futó feladatokat lehet, hogy bonyolult, és előfordulhat, hogy több eredményt adó vagy megfelelnek a végrehajtásához az egyedi tevékenységekhez. Gyakori forgatókönyvekben való a tevékenység felosztása kisebb leplezett lépéseket vagy több fogyasztói végrehajtott altevékenységek. Többlépéses feladatok lehet hatékonyabban és rugalmasabbá válik, mivel előfordulhat, hogy egyes lépések több feladat az újból felhasználható. Érdemes emellett egyszerűen hozzáadása, eltávolítása és a lépések sorrendjének módosítása.

Tevékenységek és a lépések koordinálása ellen, de a három közös mintázatok, hogy segítségével iránymutatást adjon a megoldást példányába használható:

- **Az újból felhasználható lépéseket több tevékenység decomposing**. Az alkalmazások számos különböző összetettsége feladatok elvégzéséhez feldolgozásával adatai lehet szükség. Lehet, hogy egy egyszerű, de a kötött megközelítése végrehajtása az alkalmazás Ez egy egységes modulra feldolgozásával végrehajtásához. Ezt a megközelítést azonban valószínűleg csökkentheti a kód újrabontása leképezésének és újbóli használata, ha máshol az alkalmazáson belül szükség-e ugyanarra a feldolgozás részei lehetőségeit. További tudnivalókért lásd: [csövek és a szűrők mintát](http://msdn.microsoft.com/library/dn568100.aspx).
- **A lépéseket a feladat végrehajtását kezelése**. Az alkalmazások előfordulhat, hogy feladatokat lépéseket (amelynek egy része előfordulhat, hogy távoli szolgáltatások meghívása vagy távoli forrásokat) számos alkotó. Előfordulhat, hogy az egyes lépések egymástól független, de azok is orchestrated szerint az alkalmazás logika, amely a tevékenység. További tudnivalókért lásd: az [Ütemező ügynök felügyelő mintát](http://msdn.microsoft.com/library/dn589780.aspx).
- A **tevékenység a lépések, amelyek nem sikerül helyreállítási kezelése**. Az alkalmazások előfordulhat, hogy kell lépéseket (Ez a lyncnek egységes művelet közös definiálása) sorozata által végzett munka a visszavonás egy vagy több lépésekkel nem sikerül. További tudnivalókért lásd: a [Végtermék tranzakció mintát](http://msdn.microsoft.com/library/dn589804.aspx).

## <a name="lifecycle-cloud-services"></a>Életciklus (Cloud Services)

 Ha úgy dönt, hogy a webes és dolgozó szerepkörök használnia, azaz a **RoleEntryPoint** osztály Cloud Services-alkalmazásokhoz háttér feladatok végrehajtására, fontos, az a osztály életciklus megértéséhez megfelelően használatához.

Webes és dolgozó szerepkörök feldolgozzuk különböző fázisainak halmazának indítása, futtatni és leállítása. A **RoleEntryPoint** osztály a, amely jelzi, ha ezeket a lépéseket történnek teljes sorozatának közzététele. Használja ezeket inicializálni, futtatni, és leállítja a egyéni háttérben futó feladatokat. A teljes ciklus van:

- Azure összeállítás szerepkör betöltése, és azt keresi, amelyek **RoleEntryPoint**származik osztály.
- Ha úgy találja, ez az osztály, meghívja **RoleEntryPoint.OnStart()**. Ez a módszer a háttérben futó feladatokat inicializálni felülbírálása
- A **OnStart** metódus befejeződése után a **Application_Start()** az alkalmazás globális fájl, ha ez az Azure hívások bemutatása (például egy webes szerepkör ASP.NET szolgáltatást futtató Global.asax).
- Azure a végrehajtja a **OnStart()**párhuzamosan új előtérben szálon **RoleEntryPoint.Run()** hívások. Indítsa el a háttérben futó feladatokat ezzel a módszerrel felülbírálása
- A Futtatás módszer befejeződik, Azure először felhívja az alkalmazás globális fájl **Application_End()** Ha ez nem tartalmaz adatokat, és kattintson a **RoleEntryPoint.OnStop()**hívások. Állítsa le a háttérben futó feladatokat, karbantartása: erőforrások, objektumok rendelkezési és zárja be a kapcsolatok, a feladatok használt **OnStop** módszer felülbírálása
- Az Azure dolgozó szerepkör host folyamat leállt. A szerepkör ezen a ponton a rendszer lesz, és újraindul.

További részletek és példa **RoleEntryPoint** osztály módszerekkel című témakörben talál [Erőforrás összesítés mintát számítja ki](http://msdn.microsoft.com/library/dn589778.aspx).

## <a name="considerations"></a>Megfontolandó szempontok

Ha azt tervezi, hogyan futtatható háttérben futó feladatokat az interneten vagy dolgozó szerepkörbe, vegye figyelembe az alábbiakat:

- Az alapértelmezett **futtatása** metódus végrehajtása a **RoleEntryPoint** osztály hívást kezdeményez **Thread.Sleep(Timeout.Infinite)** őrzi meg a szerepkör életben végtelen időre szóló tartalmazza. Felülírják az (Ez általában a háttérben futó feladatokat végrehajtani szükséges), a **Futtatás** módszer, ha nem engedélyeznie kell a Kilépés a módszer, hacsak nem szeretné a szerepkör-példány Lomtár kódot.
- Egy tipikus végrehajtása a **Futtatás** módszer tartalmaz programkódot, a háttérben futó feladatokat, illetve egy hurok szerkezetet, amely rendszeresen ellenőrzi a háttérben futó feladatokat állapotának indításához. Bármelyik sikertelen, és figyelje, amely jelzi, hogy befejeződött-e feladatok lemondási tokenek is indítani.
- Háttér tevékenység esetén nem kezelt kivételt okoz, ha ezt a feladatot kell hasznosítani miközben továbbra is futtatása a szerepkör a bármely más háttérben futó feladatokat. Azonban, ha a tevékenységhez, például megosztott tárhely, kívüli objektumokat sérülésének okozza a kivétel az **RoleEntryPoint** osztályához kezelje a kivétel, minden tevékenység mondta le kell lennie, és a **Futtatás** módszer végén kell tenni. Azure fog indítsa újra a szerepkör.
- A **OnStop** módszert szüneteltetése vagy törlése a háttérben futó feladatokat és erőforrások karbantartása. Ez lehet, hogy hosszabb ideig futó vagy többlépéses feladat leállítása magában. Nagyon fontos fontolja meg, hogyan ezt megteheti adatok következetlenségekre elkerülése érdekében. Ha egy szerepkör-példány valamilyen okból a felhasználó által kezdeményezett leállítása eltérő leáll, a kódot a **OnStop** módszer fut Kényszerített megszakítás előtt öt percen belül kell elvégezni. Győződjön meg arról, hogy a kód abban az időpontban el tud végezni, vagy elviseli nem fut száma.  
- Az Azure terheléselosztó elindul, mely irányítja a forgalmat a szerepkör-példányt, ha a **RoleEntryPoint.OnStart** módszer értéket adja vissza az érték **Igaz**. Ezért fontolja meg, az összes inicializálni kód illusztráló **OnStart** módszer, hogy a szerepkör-példányok, amely nem sikerült inicializálni nem kapja meg a minden forgalom.
- Indítási feladatok mellett a **RoleEntryPoint** osztály módszereket használhatja. Azok a beállítások, mivel az alábbi műveleteket hajtja végre, mielőtt a szerepkör bármely kéréseket fogad a Azure terheléselosztó módosítani kell inicializálni indítási feladatok kell használni. További tudnivalókért olvassa el a [futtassa az Indítás feladatok Azure-ban](./cloud-services/cloud-services-startup-tasks.md)című témakört.
- Ha hiba lép fel egy indítási tevékenység, azt a szerepkört folyamatosan újraindítása előfordulhat, hogy lehetséges. Ez megakadályozhatja, hogy Ön egy virtuális IP (virtuális) cím felcserélése vissza egy korábban szakaszos verziót a hajt végre, mivel a csere a szerepkör kizárólagos hozzáférés szükséges. Ez nem lehet beolvasni a szerepkör újraindítása közben. Megoldásához:
    -  A szerepkör a **OnStart** és **futtatása** módszerek elejére hozzá a következő kódot:

    ```C#
    var freeze = CloudConfigurationManager.GetSetting("Freeze");
    if (freeze != null)
    {
        if (Boolean.Parse(freeze))
        {
            Thread.Sleep(System.Threading.Timeout.Infinite);
        }
    }
    ```

   - Logikai értékként **rögzítése** beállítás a definíció hozzáadása a ServiceDefinition.csdef és ServiceConfiguration. *.cscfg fájlok a szerepkört, és állítsa be* *hamis* *. Ha a szerepkör a ismételt újraindítás módba kerül, módosíthatja a beállítást * *Igaz** szerepkör végrehajtás rögzítése, és lehetővé teszi, hogy egy korábbi verziójával kell cserélni.

## <a name="resiliency-considerations"></a>Tűrőképessége kapcsolatos szempontok

Háttér feladatok rugalmassá az alkalmazás megbízható szolgáltatások biztosítása érdekében kell lennie. Tervezés és tervezése a háttérben futó feladatokat, vegye figyelembe az alábbiakat:

- Háttérben futó feladatokat kell tudja biztonságos kezelését szerepkör vagy szolgáltatás újraindítása nélkül rongál adatok vagy az bemutatása eltérést tapasztal az alkalmazásba. A hosszabb ideig futó vagy többlépéses feladatok fontolja meg _mutató ellenőrzése_ való mentésével feladat állapotának állandó tároló vagy üzenetek szükség esetén. Például állapot adatait az üzenet egy sorban marad meg, és fokozatosan frissítése a állapot adatai a tevékenységállapotba, hogy a tevékenység az utolsó ismert jó ellenőrzés – helyett az elejétől újraindítását is feldolgozása. Azure Service Bus sorban várakozó használatakor a üzenet munkamenetek ahhoz, hogy az ugyanolyan eseteket is használhatja. Munkamenetek mentéséhez és az alkalmazás feldolgozási állapot beolvasásához a [SetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.setstate.aspx) és [GetState](http://msdn.microsoft.com/library/microsoft.servicebus.messaging.messagesession.getstate.aspx) módszerek használatával teszi lehetővé. Megbízható többlépéses folyamatokat és munkafolyamatokat tervezésével kapcsolatos további tudnivalókért olvassa el a [Feladatütemező ügynök felügyelő mintát](http://msdn.microsoft.com/library/dn589780.aspx)című témakört.
- Webhely vagy dolgozó szerepkörök több háttérben futó feladatokat tárolni használatakor tervezése a be a **Futtatás** módszer figyelheti a sikertelen vagy memóriavesztésének előfordulási helyét feladatok, és újra kell indítania őket. Ha ez nem gyakorlati, és használja a dolgozó szerepkör, kényszerítse a dolgozó szerepkört, indítsa újra a **Futtatás** módszer bezárása kattintva.
- Nagyobb, mint a szokásos terhelés alatt használatakor sorban várakozó kommunikáció háttérben futó feladatokat, a sorokat egy pufferelési tárolni a feladatokat, miközben az alkalmazás küldött kérések működhet van. Ezzel a feladatok tájékozódást segíti, hogy a felhasználói felületen a kisebb elfoglalt időszakokban. Azt is jelenti, hogy a szerepkör újrafelhasználás nem blokkolja a felhasználói felület. További tudnivalókért lásd: a [Betöltés simítása mintát várólista alapú](http://msdn.microsoft.com/library/dn589783.aspx). Ha bizonyos feladatok fontosabb, mint a többi, fontolja meg, a [Prioritás várólista mintát](http://msdn.microsoft.com/library/dn589794.aspx) annak érdekében, hogy az alábbi műveleteket futtassa a kevésbé fontos melyeket előtt végrehajtása.
- Háttérben futó feladatokat, vagy folyamat üzeneteket által kezdeményezett következetlenségekre, például sorrendje érkező üzenetek, a üzenetek többször okozó hibát (gyakran néven _poison üzenetek_) és az üzenetek kézbesítési többször kezelése kell megtervezni. Vegye figyelembe a következőket:
  - Az üzenetek egy adott sorrendben, például amelyeket módosíthatja az adatok alapján a meglévő adatok érték (például érték hozzáadása egy meglévő értéket) fel kell dolgozni, előfordulhat, hogy nem érkeznek meg az eredeti rendelés küldött volt. Azt is megteheti azok előfordulhat, hogy kell kezelnie miatt egyes példányon változó terhelések eltérő sorrendű háttér tevékenység másik példányát. Egy adott sorrendben kell dolgozni kell üzenetekben sorszámot, billentyűt vagy valamilyen más jelzése, amelyekkel a háttérben futó feladatokat győződjön meg arról, hogy ezek a megfelelő sorrendben feldolgozása. Azure Service Bus használja, ha a üzenet munkamenetek kézbesítési sorrendjének zökkenőmentes is használhatja. Célszerű azonban általában hatékonyabb, ha lehetséges, a folyamat megtervezéséhez az, hogy az üzenetek sorrendje nem számít.
  - Háttér tevékenység általában a várakozási sorban található, amelynek ideiglenesen elrejti őket más üzenet fogyasztói üzenetek fog Bepillantás. Ezután törlése az üzenetek sikeresen feldolgozása után. Háttér tevékenység sikertelen, amikor az üzenet feldolgozása, ha az üzenet meg fog jelenni a sorban kattintson a betekintő időtúllépési lejárta után. A tevékenység vagy a következő feldolgozás ciklus példány alatt egy másik példányában fog dolgozható. Ha az üzenet egységes az hibát okoz a fogyasztói, blokkolja a tevékenység, a sor, és végül magát az alkalmazás, ha a sor megtelik. Ezért fontosságú észlelni és eltávolítani a poison üzeneteket a várakozási sorban található. Azure Service Bus használatakor hibát okozó üzenetek áthelyezhető automatikusan vagy manuálisan egy társított halottlevél.
  - Sorok _legalább egyszer_ kézbesítési mechanizmusok a garantált, de azok előfordulhat, hogy használhat az előadáshoz ugyanezt a hibaüzenetet többször. Ezenkívül a háttérben sikerül után egy üzenet feldolgozása, de a sorból törlés előtt, az üzenet válnak újra feldolgozásra érhető el. Háttérben futó feladatokat idempotent, ami azt jelenti, hogy, hogy ugyanezt a hibaüzenetet többször feldolgozása nem okozhat hiba vagy eltérést tapasztal az alkalmazás adatokat kell lennie. Bizonyos műveletek természetesen idempotent, például egy tárolt értékének beállítása egy adott új értékre. Jó helyen jár a műveletek, például érték hozzáadása egy meglévő tárolt érték nélküli annak ellenőrzése, hogy a tárolt érték továbbra is ugyanaz, mint amikor az üzenetet eredetileg küldték okoz következetlenségek. Azure Service Bus sorban várakozó beállítható úgy, hogy automatikusan eltávolítása ismétlődő üzeneteket.
  - Néhány üzenetkezelő rendszer esetén Azure tároló sorban várakozó és Azure Service Bus sorok, például egy üzenet beolvasni a várólista hányszor jelző de-queue darab tulajdonság támogatja. Ez lehet az ismételt és poison üzenetek kezelésére. További tudnivalókért lásd: [Aszinkron üzenetküldés alapozó](http://msdn.microsoft.com/library/dn589781.aspx) és [Idempotencia mintázatok](http://blog.jonathanoliver.com/2010/04/idempotency-patterns/).

## <a name="scaling-and-performance-considerations"></a>Méretezési és teljesítménybeli szempontok

Háttérben futó feladatokat kell kínálnak biztosítja nem blokkolása az alkalmazást, és nem okozó következetlenségek késleltetett művelet miatt, terhelés alatt a rendszer esetén elegendő teljesítményét. Általában a teljesítmény növelése méretezéssel a számítási példányai, amelyek a háttérben futó feladatokat. Tervezés és tervezése a háttérben futó feladatokat, vegye figyelembe az alábbiakat méretezhetőség és a teljesítmény körül:

- Azure támogatja autoscaling (méretezés és újra méretezési) aktuális igény szerinti és a betöltés – alapján és előre meghatározott időközönként, Web Apps alkalmazások, Cloud Services webszolgáltatási és dolgozó szerepkörök és virtuális gépeken futó szolgáltatott telepítések. Ez a funkció használatával gondoskodjon arról, hogy az alkalmazás egészének elegendő teljesítmény funkciók futtatókörnyezet költségek minimalizálása közben.
- Amennyiben a háttérben futó feladatokat van egy másik teljesítményét képesség a további kijelzők Cloud Services alkalmazás (például a felhasználói felület vagy összetevők, például az adat-hozzáférési réteg), a háttérben futó feladatokat közös külön dolgozó szerepkörben szolgáltatója lehetővé teszi, hogy a felhasználói felület és a háttér tevékenység szerepkörök, ha egymástól függetlenül kezelheti a betöltés. Ha több háttérben futó feladatokat egymástól szignifikánsan teljesítmény funkciók, fontolja meg a be külön dolgozó szerepkörök osztani őket, és önállóan méretezés szerepkör típusonként. Megjegyzendő, hogy ez megnövelheti futtatókörnyezet költségek minden tevékenység kombinálásával be kevesebb szerepkörök és összehasonlítása.
- Egyszerűen méretezés a szerepkörök előfordulhat, hogy nem elegendő terhelés alatt a teljesítmény az adatvesztés elkerülése érdekében. Is szükség lehet tároló sorban várakozó és más erőforrások: a teljes egyetlen pont megakadályozására méretezni a egy szűk valamit lánc feldolgozása. Egyéb korlátozások, például a maximálisan tárolására és más szolgáltatások támaszkodó az alkalmazás és a háttérben futó feladatokat célszerű is.
- Háttérben futó feladatokat méretezés kell megtervezni. Például kell dinamikusan észleli a figyelni vagy üzeneteket küldeni a megfelelő várólista használatban tároló sorok számát.
- Alapértelmezés szerint WebJobs méretezze át a társított Azure Web Apps-példányt tartson. Jó helyen jár, ha azt szeretné, hogy egy WebJob csak egy példánya futtatható, létrehozhat egy Settings.job fájlt JSON adatokat tartalmazó **{"is_singleton": igaz}**. Ez csak az a WebJob, egy példánya futtatható az Azure kezd, akkor sem, ha több példányban a társított web App alkalmazásban. Ez akkor lehet hasznos technika ütemezett feladatokhoz, amely csak egyetlen példányt futnia kell.

## <a name="related-patterns"></a>Kapcsolódó mintázatok

- [Aszinkron üzenetben alapozó](http://msdn.microsoft.com/library/dn589781.aspx)
- [Autoscaling útmutató](http://msdn.microsoft.com/library/dn589774.aspx)
- [Végtermék tranzakció mintával](http://msdn.microsoft.com/library/dn589804.aspx)
- [Versengő fogyasztói mintával](http://msdn.microsoft.com/library/dn568101.aspx)
- [Útmutató szétválasztás kiszámítása](http://msdn.microsoft.com/library/dn589773.aspx)
- [Kiszámítása az erőforrás összesítés mintával](http://msdn.microsoft.com/library/dn589778.aspx)
- [Forgalomirányító mintával](http://msdn.microsoft.com/library/dn589793.aspx)
- [Kitöltés Election mintával](http://msdn.microsoft.com/library/dn568104.aspx)
- [Csövek és a szűrők mintával](http://msdn.microsoft.com/library/dn568100.aspx)
- [Prioritás várólista mintával](http://msdn.microsoft.com/library/dn589794.aspx)
- [Minta simítási várólista alapú betöltése](http://msdn.microsoft.com/library/dn589783.aspx)
- [Ütemezőt ügynök felügyelő mintával](http://msdn.microsoft.com/library/dn589780.aspx)

## <a name="more-information"></a>További információ

- [Azure alkalmazások dolgozó szerepkörök méretezése](http://msdn.microsoft.com/library/hh534484.aspx#sec8)
- [Háttérben futó feladatokat végrehajtása](http://msdn.microsoft.com/library/ff803365.aspx)
- [Azure szerepkör indítási életciklus](http://blog.syntaxc4.net/post/2011/04/13/windows-azure-role-startup-life-cycle.aspx) (blogbejegyzés)
- [Azure felhőalapú szolgáltatások szerepkör életciklus](http://channel9.msdn.com/Series/Windows-Azure-Cloud-Services-Tutorials/Windows-Azure-Cloud-Services-Role-Lifecycle) (videó)
- [Az Azure WebJobs SDK – első lépések](./app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
- [Azure sorban várakozó és szolgáltatás Bus sorban várakozó - összehasonlítás és ellentétben](./service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
- [Hogyan engedélyezhető a egy felhőalapú szolgáltatásba diagnosztika](./cloud-services/cloud-services-dotnet-diagnostics.md)
