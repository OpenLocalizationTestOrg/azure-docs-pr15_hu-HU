<properties
   pageTitle="Meglévő végrehajtható telepítse az Azure Service háló |} Microsoft Azure"
   description="Egy létező alkalmazás végrehajtható, vendégként csomag, így a szolgáltatás háló fürtre telepíthető módjáról útmutató"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="na"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>

# <a name="deploy-a-guest-executable-to-service-fabric"></a>A vendégként való bekapcsolódáshoz végrehajtható szolgáltatás háló telepítése

Bármilyen típusú alkalmazást, például a node.js, Java vagy natív alkalmazások Azure Service háló futtatását is lehetővé teszi. A vendégként való bekapcsolódáshoz végrehajtható, ilyen típusú alkalmazások szolgáltatás háló hivatkozik.
A vendégként való bekapcsolódáshoz végrehajtható például állapot nélküli services szolgáltatás háló által kell kezelni. Emiatt elérhetőségéről és az egyéb mértékek alapján egy fürt csomóponton kerülnek. Ez a cikk ismerteti, hogyan csomag, és helyezhetnek üzembe a végrehajtható vendégként szolgáltatás háló fürtre, a Visual Studio vagy parancssori segédprogramot.

Ez a cikk foglalkozunk végrehajtható vendégként csomag és szolgáltatás háló kell üzembe vonatkozó lépéseket.  

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>A szolgáltatás háló végrehajtható vendégként használatának előnyei

Létezik, amely beépített vendégként szolgáltatás háló fürt végrehajtható fájl futtatása számos előnye:

- Magas elérhető. A szolgáltatás háló futó alkalmazások erősen hozzáférhetők. Szolgáltatás háló biztosítja, hogy az alkalmazás a példánya fut.
- Állapot-ellenőrzése. Szolgáltatás háló rendszerállapot figyelése észleli, ha egy alkalmazás fut, és diagnosztika adatait találja a hiba esetén.   
- Alkalmazás életciklus-kezelése. Amellett, amelyekből frissítések nem legrövidebb leállás, szolgáltatás háló, ha a frissítés során jelentett hibás állapot esemény itt automatikus visszaállítás az előző verzióhoz képest.    
- Sűrűségfüggvény eredménye. A minden alkalmazás futtatásához a saját hardveren szükségtelenné fürt futtatását is lehetővé teszi több alkalmazást.


## <a name="overview-of-application-and-service-manifest-files"></a>Alkalmazás és a szolgáltatás jegyzékfájlok – áttekintés

Üzembe helyezése a végrehajtható Vendég részeként a szolgáltatás háló csomagolást megértéséhez hasznos lehet és telepítési modell, mint azt feljebb említettük [alkalmazásmodell](service-fabric-application-model.md). A szolgáltatás háló összecsomagolása modell támaszkodik két XML-fájlok: az alkalmazás és a szolgáltatás jegyzékfájlok. A ApplicationManifest.xml és ServiceManifest.xml fájlok sémadefiníciója a a szolgáltatás háló SDK csomagjában talál a *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*telepítve van.

* **Alkalmazás cikkét** Az alkalmazás jegyzék az alkalmazás leírására szolgál. A szolgáltatások, amelyek megírásához és más paramétereit határozza meg, hogyan az egy vagy több szolgáltatás kell telepíteni, például példányainak száma felhasznált jeleníti meg.

  A szolgáltatás háló az alkalmazások telepítési és frissítésére időegység. Az alkalmazás hol kezelik esetleges hibákat és lehetséges visszagörgetése egységként frissíthetők. Szolgáltatás háló biztosítja, hogy a frissítési folyamat vagy sikeres, vagy, ha a frissítés sikertelen lesz, nem hagyja az alkalmazás ismeretlen stabil állapotú.

* **Szolgáltatás cikkét** A szolgáltatás jegyzék szolgáltatás összetevői ismerteti. Adatok, például a név és a szolgáltatást, és a saját kód konfigurációs adatok, és típusú tartalmazza. A szolgáltatás jegyzék is tartalmaz néhány további paramétereket használt a szolgáltatás beállítása, ha telepítve van.


## <a name="application-package-file-structure"></a>Alkalmazás csomag fájlszerkezet
Az alkalmazás szolgáltatás háló alkalmazás telepítéséhez kell az kövesse az egy előre definiált címtár-struktúra. A következő példa, hogy a struktúra.

```
|-- ApplicationPackageRoot
  	|-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```

A ApplicationPackageRoot, amely definiálja az alkalmazás ApplicationManifest.xml fájlt tartalmazza. Az egyes szolgáltatásokhoz, az alkalmazáshoz tartozó alkönyvtáraként használatos összes az eltérések a szolgáltatás által igényelt – a ServiceManifest.xml, és általában a következő három könyvtárakat tartalmaznak:

- A *kódot*. Ezt a címtárat a szolgáltatáskód tartalmazza.
- A *Config*. Ezt a címtárat tartalmaz Settings.xml fájl (és más fájlokat, ha szükséges) a szolgáltatás elérhető-e az adott beállításainak beolvasásához futásidőben.
- *Adatok*. Ez a címtárhoz további adattárolásra további helyi szüksége van a szolgáltatás. Megjegyzés: Adatok rövid életű adattárolásra csak használandó. Szolgáltatás háló nem másolás/replikáció adatok könyvtárra Ha a szolgáltatás lehet áthelyezni – például a feladatátvételkor.

Megjegyzés: Nem kell létrehozni a `config` és `data` könyvtárak, ha nincs szükség.

## <a name="packaging-an-existing-executable"></a>Meglévő végrehajtható csomagolása

A vendégként való bekapcsolódáshoz végrehajtható csomagolása, amikor a Visual Studio project-sablon vagy [Alkalmazáscsomag létrehozása manuálisan](#manually)választhatja. Visual Studio segítségével, az alkalmazás csomag szerkezetének és jegyzékfájlok hozza létre az új projekt varázsló meg.

>[AZURE.NOTE] A csomag végrehajtható szolgáltatás be egy meglévő Windows legegyszerűbben a Visual Studio használni.

## <a name="using-visual-studio-to-package-an-existing-executable"></a>Visual Studio segítségével csomag egy meglévő végrehajtható fájl

Visual Studio szolgáltatás háló fürtre végrehajtható vendégként telepítésében való segítségnyújtásra szolgáltatás háló szolgáltatás sablon biztosít.

Hajtsa végre az alábbi lépéseket a közzététel befejezéséhez:

1. Válassza a fájl -> Új projekt és háló szolgáltatásalkalmazás létrehozása.
2. A vendégként való bekapcsolódáshoz végrehajtható válassza a szolgáltatás sablonként.
3. Kattintson a Tallózás gombra, és jelölje ki a mappát, és a végrehajtható fájl, és töltse ki a többi a paraméterek létrehozása a szolgáltatást.
    - A mappa összes tartalom másolása a Visual Studio projekthez, amely olyan, akkor hasznos, ha nem változtatja meg a végrehajtható fájl *Kód csomag viselkedés* beállíthatók. Ha várhatóan a végrehajtható fájl módosításához és az azt jelenti, hogy új buildjeiben dinamikusan elhozatala szeretné, válassza a hivatkozás azt a mappát, helyette. Megjegyzés: az alkalmazás projekt létrehozása a Visual Studióban, használjon csatolt mappák. Ez a Project programban végrehajtható Vendég a forrás célhelyét frissíteni kell lehetővé teszi a forráshely mutató hivatkozásokat tartalmaz lévő Szerkesztés alkalmazáscsomag részévé válnak frissítések problémákat.
    - A *program* - válassza ki a végrehajtható futtatásával indítsa el a szolgáltatást.
    - *Argumentumok* – adja meg, hogy a végrehajtható fájl átkerülnek az argumentumokat. Lehet, hogy a paraméterek argumentumok listáját.
    - *WorkingFolder* – Itt adhatja meg a munkakönyvtár elindításának fog folyamat. Három értékeket adhatja meg:
        - `CodeBase`Itt adhatja meg, hogy a munkakönyvtár fog állíthatók be a kódot címtár alkalmazáscsomag (`Code` címtár-a megelőző látható fájlszerkezet).
        - `CodePackage`Megadja, hogy a munkakönyvtár fog állíthatók be a alkalmazáscsomag legfelső szintű (`GuestService1Pkg` a megelőző látható fájlszerkezet).
        - `Work`Megadja, hogy a fájlok munka nevű alkönyvtáraként kerülnek
4. Nevezze el a szolgáltatás, és kattintson az OK gombra.
5. Ha a szolgáltatás zárólap kommunikációhoz, most vehet a protokoll, portokkal és írja be a ServiceManifest.xml fájlt. e.g. `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Most előkészítés és közzétételéhez művelet szemben a helyi fürtre hibakeresése során a Visual Studióban a megoldást. Ha készen áll, az alkalmazás a távoli fürtre közzététele vagy beadása a megoldást a verziókövetés.
7. Ebből a cikkből megtudhatja, hogy miként megtekintése, vendégként való bekapcsolódáshoz végrehajtható szolgáltatás fut a szolgáltatás háló Intézőben végén megnyitásához.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-an-existing-executable"></a>Manuális csomagolása, és egy meglévő végrehajtható fájl telepítése
Manuális csomagolása egy Vendég végrehajtható folyamata a következő lépések alapján történik:

1. A csomag címtár tagoláshoz.
2. Adja hozzá az alkalmazás kódot és konfigurációs fájlokat.
3. A szolgáltatás nyilvánvalóan fájl szerkesztése.
4. Az alkalmazás nyilvánvalóan fájl szerkesztése.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](http://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>A csomag címtár szerkezet létrehozása
A címtár-szerkezet létrehozásával kell kezdenie ismertetett módon.

### <a name="add-the-applications-code-and-configuration-files"></a>Az alkalmazás kód és a konfigurációs fájl hozzáadása
Miután létrehozta a címtár-struktúra, felveheti az alkalmazás kód és a konfigurációs fájlokat a kód és a config könyvtárak csoportban. További könyvtárak vagy a kódot vagy config könyvtárak alkönyvtárat is létrehozhat.

Szolgáltatás háló hajtja végre a tartalom, az alkalmazás legfelső szintű könyvtár egy xcopy szándékos, nincs más, mint két felső könyvtárak, a kód és a beállítások használatához előre definiált struktúra. (Is választhat másik nevek tetszés. További részletek találhatók a következő szakaszban.)

>[AZURE.NOTE] Győződjön meg arról, hogy az összes fájl/függőségek az alkalmazásnak. Szolgáltatás háló a fürt, ahol az alkalmazásszolgáltatások szeretnék telepíthető másolja át az összes csomóponton alkalmazáscsomag tartalmát. A csomag az alkalmazás futtatásához szükséges összes kódot kell tartalmaznia. Nem ajánlott feltételezve, hogy a függőségeket már telepítve vannak.

### <a name="edit-the-service-manifest-file"></a>A szolgáltatás nyilvánvalóan fájl szerkesztése
A következő lépés, ha meg szeretné jeleníteni a következő információkat a szolgáltatás nyilvánvalóan fájl szerkesztése:

- Szolgáltatás-típus nevére. Ez a szolgáltatás azonosítja használ szolgáltatási háló jelző Azonosítóra.
- A parancs használatával indítsa el az alkalmazást (ExeHost).
- Bármely beállítása futtatható kell, hogy a parancsprogram felfelé/állítsa be az alkalmazás (SetupEntrypoint).

Az alábbi képen egy `ServiceManifest.xml` fájl:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Nézzünk a fájlt, módosítania kell a különböző részei:

#### <a name="updating-the-servicetypes"></a>A ServiceTypes frissítése

```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

- Egy tetszőleges nevére, amelynek a is választhat `ServiceTypeName`. Az érték szerepel a `ApplicationManifest.xml` a szolgáltatás azonosítja a fájlt.
- Meg kell adnia `UseImplicitHost="true"`. Az attribútum Ez a szolgáltatás háló, hogy a szolgáltatás alapuló önálló alkalmazást, ezért végezze el az összes szolgáltatás háló igények indítsa el a folyamat, és figyelemmel állapotát.

#### <a name="updating-the-codepackage"></a>A CodePackage frissítése
A CodePackage elem Itt adhatja meg, a szolgáltatáskód helyét (és verziója).

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

A `Name` elemét adja meg a könyvtár nevét, amely tartalmazza a szolgáltatáskód alkalmazáscsomag szolgál. `CodePackage`szintén a `version` attribútum. Megadhatja a kód – verziójában is használható, és a potenciálisan is használható a szolgáltatáskód frissítés szolgáltatás háló alkalmazás életciklus kezelési infrastruktúra használatával.
#### <a name="optional-updating-the-setupentrypoint"></a>Nem kötelező: A SetupEntrypoint frissítése

```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
Az SetupEntrypoint elemet kell végrehajtani, mielőtt a szolgáltatáskód elindítása végrehajtható vagy köteg fájl megadására szolgál. (Nem kötelező), így nem kell része lesz, ha a inicializálni/telepítés nem szükséges. A SetupEntryPoint végrehajtása, valahányszor a szolgáltatás újraindítása.

Nincs csak egy SetupEntrypoint, így a telepítő/config parancsfájlok kell csoportosítani egyetlen parancsfájlt, ha az alkalmazás beállítása/config több parancsfájlok van szüksége. A SetupEntrypoint bármilyen típusú fájlt – programfájlok, a köteg fájlokat és a PowerShell-parancsmagok hajthat végre. További információra kíváncsi olvassa el a [SetupEntryPoint konfigurálása](service-fabric-application-runas-security.md)című témakört.

Az előző példában a SetupEntrypoint futtat egy köteg nevű fájlt `LaunchConfig.cmd` található, amely a `scripts` alkönyvtárába a kód (feltételezve, hogy a WorkingFolder elem CodeBase van beállítva).

#### <a name="updating-the-entrypoint"></a>A belépési frissítése

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

A `Entrypoint` a szolgáltatás nyilvánvalóan fájl elemének segítségével megadhatja, hogy milyen indítsa el a szolgáltatás. A `ExeHost` elem végrehajtható (és argumentumok) adja meg, hogy csak indítsa el a szolgáltatás használhatók.

- `Program`a szolgáltatás indítása kell végrehajtani a végrehajtható nevét adja meg.
- `Arguments`Itt adhatja meg, hogy a végrehajtható fájl átkerülnek az argumentumokat. Lehet, hogy a paraméterek argumentumok listáját.
- `WorkingFolder`Itt adhatja meg a munkakönyvtár elindításának fog folyamat. Három értékeket adhatja meg:
    - `CodeBase`Itt adhatja meg, hogy a munkakönyvtár fog állíthatók be a kódot címtár alkalmazáscsomag (`Code` könyvtárban az előző fájlszerkezet lévő).
    - `CodePackage`Megadja, hogy a munkakönyvtár fog állíthatók be a alkalmazáscsomag legfelső szintű (`GuestService1Pkg` az előző fájlszerkezet).
  - `Work`Megadja, hogy a fájlok munka nevű alkönyvtáraként kerülnek

A WorkingFolder akkor hasznos lehet ahhoz, hogy a helyes munkakönyvtár beállítása az, hogy a relatív elérési utak használható az alkalmazás vagy a inicializálni parancsfájlok.

#### <a name="updating-the-endpoints-and-registering-with-naming-service-for-communication"></a>A végpontok frissítése, és a kommunikáció rögzítése kiosztási szolgáltatással

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Az előző példában a `Endpoint` elem azt adja meg, hogy az alkalmazás figyelheti a a végpontok. Ebben a példában az Node.js alkalmazás figyel http port 3000.

Továbbá megkérheti a szolgáltatás háló közzététele a végpont a elnevezési szolgáltatás, így más szolgáltatások, észleljék az Ez a szolgáltatás végpontjának címet. Ez lehetővé teszi, hogy tudja szolgáltatásokat, amelyek a vendégként való bekapcsolódáshoz végrehajtható közötti kommunikációt.
A közzétett végpontjának címe van az űrlap `UriScheme://IPAddressOrFQDN:Port/PathSuffix`. `UriScheme`és `PathSuffix` nem kötelező attribútum. `IPAddressOrFQDN`az a IP-cím vagy teljesen minősített tartománynév a csomópont a végrehajtható fájl a gyermekelemeiként tárolja, és számítható ki.

A következő példában ha telepíti a szolgáltatást, a szolgáltatás háló Intéző látni hasonló zárólap `http://10.1.4.92:3000/myapp/` szolgáltatás példány közzé, vagy ha megjelenik egy helyi számítógép `http://localhost:3000/myapp/`. 

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Ezek a címek használhatja az, [fordított proxykiszolgáló](service-fabric-reverseproxy.md) szolgáltatások közötti kommunikációt.

### <a name="edit-the-application-manifest-file"></a>Az alkalmazás nyilvánvalóan fájl szerkesztése

Miután beállította a a `Servicemanifest.xml` fájl, akkor a módosítások befejezése a `ApplicationManifest.xml` fájl annak érdekében, hogy a megfelelő szolgáltatás típusa és nevét használja.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport

Az a `ServiceManifestImport` elem adhatja meg az alkalmazás szerepeltetni kívánt egy vagy több szolgáltatást. A hivatkozott szolgáltatások `ServiceManifestName`, a könyvtár nevét adja meg, amelyek hol a `ServiceManifest.xml` fájl található.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Naplózás beállítása
A vendégként való bekapcsolódáshoz végrehajtható célszerű konzol naplók megtudhatja, hogy ha az alkalmazás és a konfigurációs parancsfájlok megjelenítése a hibák láthatja.
Konzol átirányítása beállíthatók a `ServiceManifest.xml` fájl használatával a `ConsoleRedirection` elemet.

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

* `ConsoleRedirection`konzol kimeneti (stdout és stderr) átirányítása munkakönyvtárat, ellenőrizze, hogy nincsenek-e hibák a telepítő vagy az alkalmazás a szolgáltatás háló fürt végrehajtása során használható használható.

    * `FileRetentionCount`határozza meg, hogy hány fájlt a program menti a munkakönyvtár. 5-öt, például azt jelenti, hogy az előző öt végrehajtások a naplófájlok a munkakönyvtár találhatók.
    * `FileMaxSizeInKb`Adja meg a naplófájlok maximális méretét.

Naplófájlok a program menti a szolgáltatás használata könyvtárak közül. Annak megállapításához, hogy hol találhatók a fájlokat, kell szolgáltatás háló Intézővel határozza meg, hogy a szolgáltatás fut, és mely munkakönyvtár van használatban mely csomópontot. Ez a cikk későbbi részében ezt a folyamatot vonatkozik.

## <a name="deployment"></a>Telepítési
Az utolsó lépésként az alkalmazás telepítéséhez. Az alábbi PowerShell parancsprogramot mutatja a helyi fejlesztési fürthöz az alkalmazások telepítése, és kezdjen új szolgáltatás háló szolgáltatás.

```PowerShell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```
A szolgáltatás háló szolgáltatás telepíthető különféle "konfigurációk." Egy vagy több példányként telepíthető, vagy például oly módon, hogy van-e a szolgáltatás háló fürt minden csomóponton egy példánya telepíthető.

A `InstanceCount` paramétere: a `New-ServiceFabricService` parancsmag használatával adja meg, hány példánya a szolgáltatást a szolgáltatás háló fürt kell indítani. Beállíthatja, hogy a `InstanceCount` érték, amely telepít alkalmazás típusától függően. A leggyakoribb két forgatókönyvet a következők:

* `InstanceCount = "1"`. Ebben az esetben a szolgáltatás csak egy példánya telepítve van a fürt. Szolgáltatás háló ütemező határozza meg, hogy melyik csomópont a szolgáltatás fog a telepíthető.

* `InstanceCount ="-1"`. Ebben az esetben a szolgáltatás egy példánya telepítve van a szolgáltatás háló fürt minden csomóponton. Az eredmény a fürt a szolgáltatás egyes csomópont (és csak egy) példánya problémákat.

Ennek oka a hasznos konfiguráció előtér-alkalmazások (például egy FENNMARADÓ végpontot) ügyfélalkalmazásokban kell csatlakoznia "" a fürt használni a végpont csomópontok közül. Ez a beállítás is használható, ha például a szolgáltatás háló fürt csomópontjait csatlakozik egy terheléselosztó, az ügyfél-forgalmat a szolgáltatást a fürt csomópontjait futtató legyen elosztva.

## <a name="check-your-running-application"></a>Jelölje be a futó alkalmazás

A szolgáltatás háló Intézőben azonosítsa a csomópontot, ahol a szolgáltatást futtató. Ebben a példában a Node1 futása:

![Csomópontot, ahol a szolgáltatást futtató](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Ha a nyissa meg azt a csomópontot, és tallózással keresse meg az alkalmazást, megtekintheti az alapvető csomópont adatait, beleértve a lemezen elfoglalt helyét.

![Helyet a lemezen](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Tallózás a címtárhoz kiszolgáló Intézővel, talál a munkakönyvtárat, és a szolgáltatás napló mappára, az alábbi kép látható.

![Napló helye](./media/service-fabric-deploy-existing-app/loglocation.png)


## <a name="next-steps"></a>Következő lépések
Ebben a cikkben egy Vendég végrehajtható csomag és szolgáltatás háló kell üzembe megtanulta azt van. Következő lépésként érdemes az ebben a témakörben további tartalmakat.

- [Minta csomagolása, és üzembe helyezése a GitHub végrehajtható vendégként](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/GuestExe/SimpleApplication), többek között a csomagolást eszköz előzetes mutató hivatkozás
- [Több Vendég végrehajtható fájl terjesztése](service-fabric-deploy-multiple-apps.md)
- [A Visual Studio segítségével első szolgáltatás háló-alkalmazás létrehozása](service-fabric-create-your-first-application-in-visual-studio.md)
