<properties
   pageTitle="Egy MongoDB használó Node.js alkalmazások telepítése |} Microsoft Azure"
   description="Útmutató több Vendég végrehajtható csomag módjáról Azure Service háló fürthöz terjesztése"
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
   ms.workload="NA"
   ms.date="10/22/2016"
   ms.author="msfussell;mikhegn"/>


# <a name="deploy-multiple-guest-executables"></a>Több Vendég végrehajtható fájl terjesztése

Ez a cikk bemutatja, hogyan csomag és az Azure Service háló több Vendég végrehajtható telepítése. Összeállítását, és egyetlen szolgáltatás háló csomag telepítése olvassa el a [vendégként való szolgáltatás háló végrehajtható](service-fabric-deploy-existing-app.md)telepítéséről.

Ez a forgatókönyv szemlélteti, hogyan lehet a Node.js előtérrészét MongoDB, a tárolót használó alkalmazások telepítése, miközben lépésekkel alkalmazhat bármely alkalmazásban, amely a másik alkalmazás függőséget tartalmaz.   

Visual Studio segítségével több Vendég végrehajtható tartalmazó alkalmazáscsomag kiszámítására. Lásd: a [Visual Studio segítségével csomag egy meglévő alkalmazást](service-fabric-deploy-existing-app.md#using-visual-studio-to-package-an-existing-executable). Miután hozzáadta a első Vendég végrehajtható, az alkalmazás a projekten kattintson a jobb gombbal, és jelölje be a **-> új szolgáltatási háló szolgáltatás hozzáadása** a második Vendég végrehajtható projekt hozzáadása a megoldást. Megjegyzés: Ha úgy dönt, hogy a hivatkozás a forrás a Visual Studio projektben, a Visual Studio megoldás kiépítése fog ellenőrizze, hogy a alkalmazáscsomag naprakészen tartása a forrásban változások. 

## <a name="manually-package-the-multiple-guest-executable-application"></a>A vendégként való bekapcsolódáshoz több végrehajtható alkalmazás manuálisan csomag
Másik lehetőségként manuálisan is csomag végrehajtható vendégként. Ez a cikk a kézi csomagolást használja a szolgáltatás háló összecsomagolása eszközt, amely [http://aka.ms/servicefabricpacktool](http://aka.ms/servicefabricpacktool)címen érhető el.

### <a name="packaging-the-nodejs-application"></a>A Node.js alkalmazás csomagolása
Ez a cikk tartalma feltételezi, hogy Node.js nincs telepítve a szolgáltatás háló fürt csomóponton. Következtében kell Node.exe hozzáadása a legfelső szintű könyvtár a csomópont-alkalmazás előtt csomagolást. (Express webes keretrendszer és motor Jade sablon használatával) Node.js alkalmazása címtár felépítése a következő lehetőségek hasonlóan kell kinéznie:

```
|-- NodeApplication
  	|-- bin
        |-- www
  	|-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
  	|-- public
        |-- images
        |-- etc.
  	|-- routes
        |-- index.js
        |-- users.js
  	|-- views
        |-- index.jade
        |-- etc.
  	|-- app.js
  	|-- package.json
  	|-- node.exe
```

Következő lépésként hozzon létre egy alkalmazáscsomag Node.js alkalmazásához. Az alábbi kód egy szolgáltatás háló alkalmazáscsomag, amely tartalmazza az Node.js alkalmazás hoz létre.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

A paraméterek használt leírását az alábbi van:

- **forrás** mutat, az alkalmazás, amely csomagolt kell lennie a címtárhoz.
- **/ TARGET** határozza meg a könyvtár, amelyben a csomagot kell létrehozni. Ezt a címtárat nem lehet eltérő az adatforrás-címtárból.
- **típustárnevek** határozza meg, hogy az alkalmazás nevét, a meglévő alkalmazást. Fontos, ha meg szeretné érteni, hogy ez megfelelője szolgáltatás néven található a jegyzék, nem pedig a szolgáltatás háló alkalmazás neve.
- **/exe** határozza meg a szolgáltatás háló rovatokhoz, ebben az esetben kell használni végrehajtható `node.exe`.
- **/ma** az argumentumot, indítsa el a végrehajtható fájl használt határozza meg. Node.js nem települ, miközben szolgáltatás háló indítsa el a Node.js webkiszolgáló hajtja végre kell `node.exe bin/www`.  `/ma:'bin/www'`Ez az információ a csomagolást eszköz `bin/ma` node.exe argumentumaként.
- **/AppType** a szolgáltatás háló alkalmazásnév típusa határozza meg.

Keresse meg a címtárhoz a/TARGET paraméterrel megadott, láthatja, hogy az eszköz hozott létre a működő szolgáltatás háló csomag alább látható módon.

```
|--[yourtargetdirectory]
  	|-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
A létrehozott ServiceManifest.xml ekkor megjelenik egy szakaszt, amely ismerteti, hogyan kell indítani az Node.js érintett webkiszolgálóra, az alábbi kódrészletet látható módon:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Az ebben a példában a Node.js webkiszolgáló figyeli porthoz 3000, módosítania kell a végpont adatait a ServiceManifest.xml fájlban alább látható módon.   

```xml
<Resources>
      <Endpoints>
        <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>A MongoDB alkalmazás csomagolása
Most, hogy a Node.js alkalmazás van csomagolása, lépjen tovább, és MongoDB csomag. Az említett előtt, a mesteroldalhoz most lépések amelyek nem kötődnek Node.js és MongoDB. Erre valójában ugyanúgy vonatkoznak, amelyeket egy szolgáltatás háló alkalmazásként közös csomagolt minden alkalmazást.  

MongoDB csomag, kívánt, hogy Ön csomag Mongod.exe és Mongo.exe. Mindkét bináris találhatók a `bin` könyvtár a MongoDB telepítési könyvtár. A címtár-struktúra megjelenése hasonlít az alábbi egy.

```
|-- MongoDB
  	|-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Szolgáltatás háló kell kezdődnie MongoDB hasonló a parancs az alábbi, így kell használni a `/ma` paraméter, amikor MongoDB csomagolást.

```
mongod.exe --dbpath [path to data]
```
> [AZURE.NOTE] Az adatok nem alatt megmarad csomópont hiba esetén, ha a MongoDB adatkönyvtárának elhelyezése a helyi könyvtár a csomópont. Tartós tároló vagy az adatvesztés elkerülése érdekében MongoDB kópiakészlet megvalósítása kell.  

A PowerShell vagy a parancs rendszerhéj a következő paraméterek a csomagolása eszközt futtatni azt:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Annak érdekében, hogy a szolgáltatás háló alkalmazáscsomag MongoDB hozzáadásához kell győződjön meg arról, hogy a/TARGET paraméter mutat, amely már tartalmaz az alkalmazás ugyanabban a könyvtárban az Node.js alkalmazás együtt cikkét. Meg kell győződjön meg arról, hogy használni ApplicationType ugyanazt a nevet.

Vegyük nyissa meg a mappát, és vizsgálja meg, az eszköz hozott.

```
|--[yourtargetdirectory]
  	|-- MyNodeApplication
  	|-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
  	|-- ApplicationManifest.xml
```
Amint látható, az eszköz egy új mappát, MongoDB, hozzáadva az MongoDB bináris tartalmazó könyvtár. Ha megnyit a `ApplicationManifest.xml` fájlt, láthatja, hogy a csomag most már tartalmazza a Node.js alkalmazás és a MongoDB. Az alábbi kód az alkalmazás jegyzék tartalmát mutatja.

```xml
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Az alkalmazás közzététele
Az utolsó lépésként az alkalmazás a helyi szolgáltatás háló fürthöz közzététele az alábbi PowerShell-parancsfájlokat használatával:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Miután sikeresen van közzétéve az alkalmazás a helyi fürtre, azt a szolgáltatást jegyzék Node.js-alkalmazás – például http://localhost:3000 írt port Node.js alkalmazás érheti el.

Ebben az oktatóanyagban hogyan egyszerűen csomag két meglévő alkalmazások egy szolgáltatás háló alkalmazásként van látja. Emellett megtanulta azt telepítéséről szolgáltatás háló, hogy azt előnyeivel szolgáltatás háló funkciókat, például a magas elérhetőségéről és az állapot rendszer integrációs részét.

## <a name="next-steps"></a>Következő lépések

- A [szolgáltatás és tárolók áttekintés](service-fabric-containers-overview.md) tárolók telepítésével kapcsolatos további tudnivalók
