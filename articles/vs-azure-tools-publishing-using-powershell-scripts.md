<properties
   pageTitle="Használja a Windows PowerShell-parancsfájlokat közzététele fejlesztők és a próba-környezetben |} Microsoft Azure"
   description="Útmutató a Windows PowerShell-parancsfájlokat a Visual Studio segítségével fejlesztési közzététele és tesztelje a környezetben."
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

# <a name="using-windows-powershell-scripts-to-publish-to-dev-and-test-environments"></a>A Windows PowerShell-parancsfájlokat használatával fejlesztők közzététele és a környezetek tesztelése

Webalkalmazás létrehozásakor a Visual Studióban egy Windows PowerShell-parancsprogramot használható később automatizálhatja a közzététel a webhely Azure a Web App Azure App szolgáltatásban vagy a virtuális gép hozhat létre. Szerkesztés és kiterjesztése a Visual Studio-szerkesztőben az igényeinek, az igényeknek megfelelően alakíthatja a Windows PowerShell parancsprogramot, vagy a parancsfájl integrálása a meglévő fejlesztése, tesztelése és parancsfájlok közzétételi.

A parancsfájlok használ, testre szabott verziói (más néven fejlesztők és próba környezetben) a webhelyhez az ideiglenesen használt kiépítése. Például, előfordulhat, hogy állítsa be a webhely-Azure virtuális gépen vagy egy webhelyen, a próba-öt, Reprodukálja a hiba, tesztelje a hibajavítás próbaverzió javasolt módosítás, vagy a bemutató vagy bemutatóra vonatkozó egyéni környezet beállítása az átmeneti tárolásra szolgáló tárolóhely egy adott verzióját. Miután létrehozta a parancsfájl tesz közzé a projektet, hozza létre újból a környezetek azonos ismételt futtatásával a parancsfájl szükség szerint, vagy futtassa a saját build a webalkalmazás létrehozása teszteléshez egy egyéni környezet a.

## <a name="what-you-need"></a>Mire van szüksége

- Azure SDK 2.3-as vagy újabb verziója. Lásd: a [Visual Studio letöltések](http://go.microsoft.com/fwlink/?LinkID=624384) további információt.

A parancsfájlok webhelyen projektek létrehozása az Azure SDK szükségtelen. Ez a funkció nem webes szerepkörök felhőszolgáltatások webes projektekhez.

- Azure PowerShell 0.7.4 vagy újabb verziójában. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](powershell-install-configure.md) további információt.

- [A Windows PowerShell 3.0-s](http://go.microsoft.com/?linkid=9811175) vagy újabb verziójában.

## <a name="additional-tools"></a>A további eszközök

További eszközök és források a Visual Studióban PowerShell használata az Azure fejlesztési érhetők el. Lásd: a [PowerShell Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=404012).

## <a name="generating-the-publish-scripts"></a>A közzététel parancsfájlok létrehozása

A közzététel parancsfájlok, amelyen a webhely, amikor új projektet hoz létre a következő [útmutatóban](./virtual-machines/virtual-machines-windows-classic-web-app-visual-studio.md)virtuális géphez hozhat létre. Is [készítése parancsfájlok web Apps alkalmazások közzététele az Azure alkalmazás szolgáltatás](./app-service-web/web-sites-dotnet-get-started.md).

## <a name="scripts-that-visual-studio-generates"></a>Parancsfájl, Visual Studio generáló

Visual Studio hoz létre, amelyben két a Windows PowerShell-fájlok virtuális gép vagy webhely közzététel parancsfájl **PublishScripts** és a modul, amely tartalmazza a parancsfájlok használható függvények című megoldás szintű mappa. Visual Studio is létrehoz egy fájlt, amely meghatározza a részletek, a projekt telepít JSON formátumban.

### <a name="windows-powershell-publish-script"></a>A Windows PowerShell parancsprogramot közzététele

A közzététel parancsfájl egy webhelyre vagy egy virtuális gép bevezetéshez adott közzététel lépései tartalmazza. Visual Studio biztosít a Windows PowerShell fejlesztéshez színezése szintaxis. Súgó a függvények érhető el, és a függvény a parancsfájl, hogy megfeleljen a változó követelmények szabadon szerkesztheti.

### <a name="windows-powershell-module"></a>A Windows PowerShell-modult

A Windows PowerShell-modult, amely a Visual Studio létrehozza a közzététel parancsfájl használó funkciók tartalmazza. Ezek Azure PowerShell funkciók és elsősorban nem módosítható. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](powershell-install-configure.md) további információt.

### <a name="json-configuration-file"></a>JSON konfigurációs fájl

A JSON-fájl a **konfigurációk** mappában jön létre, és adja meg pontosan Azure szeretne telepíteni, milyen erőforrások konfigurációs adatokat tartalmazza. A fájlt, létrehozza a Visual Studio neve a project-neve-WAWS-dev.json Ha létrehozott egy webhely vagy a projekt neve-virtuális-dev.json létrehozott egy virtuális számítógépre. Az alábbi képen egy webhely létrehozásakor létrehozott JSON konfigurációs fájl. Az értékeket a legtöbb magától értetődő. A webhely neve Azure, hozza létre, akkor előfordulhat, hogy nem egyezik meg a projekt neve.

```
{
"environmentSettings": {
"webSite": {
"name": "WebApplication26632",
"location": "West US"
},
"databases": [
{
"connectionStringName": "DefaultConnection",
"databaseName": "WebApplication26632_db",
"serverName": "YourDatabaseServerName",
"user": "sqluser2",
"password": "",
"edition": "",
"size": "",
"collation": "",
"location": "West US"
}
]
}
}
```
A virtuális gép hoz létre, a JSON konfigurációs fájl az alábbihoz hasonló formátumban. Figyelje meg, hogy egy felhőalapú szolgáltatásba a virtuális gép tároló jön létre. A virtuális gép tartalmazza a szokásos végpontok web Access HTTP- és HTTPS keresztül, valamint a webes telepíteni, amely lehetővé teszi a helyi számítógép zónában, a távoli asztali és a Windows PowerShell közzététele a webhelyen a végpontok.

```
{
"environmentSettings": {
"cloudService": {
"name": "myusernamevm1",
"affinityGroup": "",
"location": "West US",
"virtualNetwork": "",
"subnet": "",
"availabilitySet": "",
"virtualMachine": {
"name": "myusernamevm1",
"vhdImage": "a699494373c04fc0bc8f2bb1389d6106__Win2K8R2SP1-Datacenter-201403.01-en.us-127GB.vhd",
"size": "Small",
"user": "vmuser1",
"password": "",
"enableWebDeployExtension": true,
"endpoints": [
{
"name": "Http",
"protocol": "TCP",
"publicPort": "80",
"privatePort": "80"
},
{
"name": "Https",
"protocol": "TCP",
"publicPort": "443",
"privatePort": "443"
},
{
"name": "WebDeploy",
"protocol": "TCP",
"publicPort": "8172",
"privatePort": "8172"
},
{
"name": "Remote Desktop",
"protocol": "TCP",
"publicPort": "3389",
"privatePort": "3389"
},
{
"name": "Powershell",
"protocol": "TCP",
"publicPort": "5986",
"privatePort": "5986"
}
]
}
},
"databases": [
{
"connectionStringName": "",
"databaseName": "",
"serverName": "",
"user": "",
"password": ""
}
],
"webDeployParameters": {
"iisWebApplicationName": "Default Web Site"
}
}
}
```

A JSON konfiguráció módosítása, hogy mi történik, ha a közzététel parancsfájlt szerkesztheti. A `cloudService` és `virtualMachine` szakaszok szükségesek, de törölhet is a `databases` szakasz, ha már nincs szükség. Az alapértelmezett konfigurációs fájl, amely a Visual Studio létrehozza az üres tulajdonságok nem kötelező; az alapértelmezett konfigurációs fájl értékek rendelkező szükség.

Azure-ban egy gyártási webhely helyett Ha olyan webhely esetében több telepítési környezetekben (más néven helyek) van, a webhely neve tárolóhely nevét a JSON konfigurációs fájl is. Például ha van egy webhelye, amely magában nevű a **saját webhely** és egy tárolóhely, **tesztelje** a URI majd nevű saját webhely-test.cloudapp.net, azonban a helyes nevet, a konfigurációs fájl mysite(test). Csak végezhetők el erre, ha a webhely és a helyek már megtalálható az előfizetés. Nem léteznek, ha a webhely létrehozásához a parancsprogram futtatása a tárolóhely, nélkülire, majd a tárolóhely létrehozása az [Azure klasszikus portálra](http://go.microsoft.com/fwlink/?LinkID=213885), és ezt követően futtassa a módosított webhely nevű. Web Apps alkalmazások telepítési helyek kapcsolatos további tudnivalókért olvassa el a [web Apps alkalmazások Azure App szolgáltatásban környezetekben átmeneti beállítása](./app-service-web/web-sites-staged-publishing.md)című témakört.

## <a name="how-to-run-the-publish-scripts"></a>A közzététel parancsfájlok futtatása

Ha soha nem futtatása előtt a Windows PowerShell-parancsprogramot, az adatvégrehajtás házirendet a parancsfájlok futtatásának engedélyezése először meg kell adnia. Ez a tulajdonság biztonsági meg, hogy a felhasználók nem futtathatnak Windows PowerShell-parancsfájlokat, ha kártevőt vagy vírusokkal, amely magában foglalja a parancsfájlok végrehajtása kitéve is legyenek.

### <a name="run-the-script"></a>Futtassa a

1. A project Web üzembe előkészítés létrehozása. Egy webhely üzembe csomag egy webhelyre vagy virtuális gépen átmásolása kívánt fájlokat tartalmazó tömörített archívum (.zip fájl). A Visual Studióban webes üzembe csomagok hozhat létre bármely webes alkalmazáshoz.

![Webhely létrehozása csomag telepítéséhez](./media/vs-azure-tools-publishing-using-powershell-scripts/IC767885.png)

További tudnivalókért lásd: [Útmutató: a webes telepítőcsomag létrehozása a Visual Studio](https://msdn.microsoft.com/library/dd465323.aspx). Is automatizálhatja a webes üzembe csomag létrehozása, **testreszabása és a közzététel parancsfájlok bővítése** a témakör későbbi szakaszában leírtak szerint.

1. A **Megoldás Explorer**a helyi menü a parancsfájl, és válassza a **PowerShell ISE megnyitása**gombra.

1. Ha az első alkalommal futtatja a Windows PowerShell-parancsfájlokat már ezen a számítógépen, nyisson meg egy parancssorablakot rendszergazdai jogosultságokkal rendelkező, és írja be a következő parancsot:

`Set-ExecutionPolicy RemoteSigned`

1. Jelentkezzen be az Azure a következő parancs használatával.

`Add-AzureAccount`

Amikor a rendszer kéri, adja meg a felhasználónevét és jelszavát.

Ne feledje, hogy automatizálhatja a parancsfájlt, ha ezt a módszert Azure hitelesítő adatok megadása nem használható. Ehelyett hitelesítő adatok megadására a .publishsettings fájl kell használnia. Egyszeri csak, használja a **Get-AzurePublishSettingsFile** parancs a fájl letöltése az Azure, és ezt követően a fájl importálása **Importálása-AzurePublishSettingsFile** használatával. Részletes útmutatásért lásd: [Telepítse és állítsa be a Azure PowerShell](powershell-install-configure.md).

1. (Nem kötelező) Ha szeretné az Azure erőforrások, például a virtuális gép, az adatbázis és a webhely létrehozása a webes alkalmazás közzététel nélkül, használja a **Közzététel-WebApplication.ps1** parancsot a **-konfigurációs** a JSON konfigurációs fájl argumentumé. A parancssor erőforrások létrehozása a JSON konfigurációs fájl használja. Mert más parancssori argumentumok az alapértelmezett beállításokat használ, az erőforrások hoz létre, de nem tegye közzé a webalkalmazás. A – részletes lehetőséget nyújt további információt a Mi történik.

`Publish-WebApplication.ps1 -Verbose –Configuration C:\Path\WebProject-WAWS-dev.json`

1. A **Közzététel-WebApplication.ps1** parancs segítségével ahogy azt az alábbiak egyike jelenik meg a parancsfájlt, és tegye közzé a webalkalmazás. Ha az alapértelmezett beállítások felülbírálása a bármely más, például az előfizetés nevét, tegye közzé csomag neve, a virtuális gép hitelesítő adatok vagy az adatbázis-kiszolgáló adatokat kell megadhatja azokat a paraméterek. Használja a **– részletes** talál további információt a közzétételi folyamat előrehaladását lehetőséget.

```
Publish-WebApplication.ps1 –Configuration C:\Path\WebProject-WAWS-dev-json `
–SubscriptionName Contoso `
-WebDeployPackage C:\Documents\Azure\ADWebApp.zip `
-DatabaseServerPassword @{Name="dbServerName";Password="adminPassword"} `
-Verbose
```

Ha egy virtuális gép készít, a parancs néz ki a következő. Ebben a példában is látható megadása több adatbázis hitelesítő adatait. A virtuális gépeken futó, ezeket a parancsfájlokat hoz létre, a SSL-tanúsítvány származik, de nem egy megbízható legfelső szintű hitelesítésszolgáltató. Ezért akkor használja a **– AllowUntrusted** lehetőséget.

```
Publish-WebApplication.ps1 `
-Configuration C:\Path\ADVM-VM-test.json `
-SubscriptionName Contoso `
-WebDeployPackage C:\Path\ADVM.zip `
-AllowUntrusted `
-VMPassword @{name = "vmUserName"; password = "YourPasswordHere"} `
-DatabaseServerPassword @{Name="server1";Password="adminPassword1"}, @{Name="server2";Password="adminPassword2"} `
-Verbose
```

A parancsprogram adatbázisokat hozhat létre, de az adatbázis-kiszolgáló nem jön létre. Ha szeretne létrehozni az adatbázis-kiszolgálóval, a **New-AzureSqlDatabaseServer** függvény az Azure modulban is használhatja.

## <a name="customizing-and-extending-the-publish-scripts"></a>Testreszabása és a közzététel parancsfájlok bővítése

Testre szabhatja a közzététel parancsfájlt, és a JSON konfigurációs fájl is. A Windows PowerShell-modult **AzureWebAppPublishModule.psm1** függvényei elsősorban nem módosítható. Ha csak szeretne, adjon meg egy másik adatbázist, vagy módosítsa a virtuális gép a tulajdonságok egy része, a JSON konfigurációs fájl szerkesztése Ha szeretne a parancsfájl automatizálásához fejlesztésére, és a projekt vizsgálata bővítése, alkalmazhat függvény rutinhelyőrzőkre a **Közzététel-WebApplication.ps1**.

Automatizálásához fejlesztésére a projekt, adja hozzá a kódot, amely felhívja a MSBuild `New-WebDeployPackage` kód példában látható módon. A MSBuild parancs elérési útja eltérő attól függően, hogy telepítve van a Visual Studio verzióját. Ha a megfelelő elérési utat, a funkció használata a **Get-MSBuildCmd**, ebben a példában látható módon.

### <a name="to-automate-building-your-project"></a>Automatizálható a projekt létrehozása

1. Adja hozzá a `$ProjectFile` paraméter globális paraméter szakaszában.

```
[Parameter(Mandatory = $false)]
  [ValidateScript({Test-Path $_ -PathType Leaf})]
  [String]
  $ProjectFile,
```

1. A függvény másolása `Get-MSBuildCmd` a parancsprogram-fájlba.

```
function Get-MSBuildCmd
{
        process
{

             $path =  Get-ChildItem "HKLM:\SOFTWARE\Microsoft\MSBuild\ToolsVersions\" |
                                   Sort-Object {[double]$_.PSChildName} -Descending |
                                   Select-Object -First 1 |
                                   Get-ItemProperty -Name MSBuildToolsPath |
                                   Select -ExpandProperty MSBuildToolsPath
       
            $path = (Join-Path -Path $path -ChildPath 'msbuild.exe')

        return Get-Item $path
    }
}
```

1. Csere `New-WebDeployPackage` , a következő kód és a helyőrzőket, a vonal hozhat létre, a csere `$msbuildCmd`. Ez a kód nem a Visual Studio 2015. Visual Studio 2013-at használ, ha módosítása a **VisualStudioVersion** tulajdonságot alatti `12.0`.

```
function New-WebDeployPackage
{
    #Write a function to build and package your web application
      
#To build your web application, use MsBuild.exe. For help, see MSBuild Command-Line Reference at: http://go.microsoft.com/fwlink/?LinkId=391339
      
Write-VerboseWithTime 'Build-WebDeployPackage: Start'
      
$msbuildCmd = '"{0}" "{1}" /T:Rebuild;Package /P:VisualStudioVersion=14.0 /p:OutputPath="{2}\MSBuildOutputPath" /flp:logfile=msbuild.log,v=d' -f (Get-MSBuildCmd), $ProjectFile, $scriptDirectory
      
Write-VerboseWithTime ('Build-WebDeployPackage: ' + $msbuildCmd)
      
#Start execution of the build command
$job = Start-Process cmd.exe -ArgumentList('/C "' + $msbuildCmd + '"') -WindowStyle Normal -Wait -PassThru
      
if ($job.ExitCode -ne 0)
{
throw('MsBuild exited with an error. ExitCode:' + $job.ExitCode)
}

#Obtain the project name
$projectName = (Get-Item $ProjectFile).BaseName
      
#Construct the path to web deploy zip package
$DeployPackageDir =  '.\MSBuildOutputPath\_PublishedWebsites\{0}_Package\{0}.zip' -f $projectName
      
      
#Get the full path for the web deploy zip package. This is required for MSDeploy to work
$WebDeployPackage = Resolve-Path –LiteralPath $DeployPackageDir
      
Write-VerboseWithTime 'Build-WebDeployPackage: End'
      
return $WebDeployPackage
}
```

1. Hívja fel a `New-WebDeployPackage` előtt a sor függvény: `$Config = Read-ConfigFile $Configuration` web Apps alkalmazások vagy `$Config = Read-ConfigFile $Configuration -HasWebDeployPackage:([Bool]$WebDeployPackage)` virtuális gépeken futó számára.

```
if($ProjectFile)
{
$WebDeployPackage = New-WebDeployPackage
}
```

1. A testre szabott parancsfájl használatával átadása parancssorból meghívása a `$Project` argumentum, ahogy az alábbi példa parancsot.

```
.\Publish-WebApplicationVM.ps1 -Configuration .\Configurations\WebApplication5-VM-dev.json `
-ProjectFile ..\WebApplication5\WebApplication5.csproj `
-VMPassword @{Name="VMUser";Password="Test.123"} `
-AllowUntrusted `
-Verbose
```

Automatizálhatja a tesztelés az alkalmazás, adja hozzá a kód `Test-WebApplication`. Ügyeljen arra, hogy vegye ki a megjegyzésjeleket a **Közzététel-WebApplication.ps1** , ahová ezeket a funkciókat úgynevezett sorokat. Ha Ön nem adja meg a megvalósítása, manuálisan készítése a projekthez a Visual Studio, és majd futtassa a közzététel Azure közzététele.

## <a name="publishing-function-summary"></a>Közzétételi összesítő függvény

A Windows PowerShell-parancssorában használható függvények, használja a parancs `Get-Help function-name`. A Súgó paraméter Súgó és a példa tartalmazza. Az azonos súgószöveg parancsfájl forrásfájlok **AzureWebAppPublishModule.psm1** és a **Közzététel-WebApplication.ps1**is szerepel. A parancsfájlok és a Súgó honosítva a Visual Studio nyelvet.

**AzureWebAppPublishModule**

|Függvény neve|Leírás|
|---|---|
|AzureSQLDatabase hozzáadása|Létrehoz egy új Azure SQL-adatbázishoz.|
|AzureSQLDatabases hozzáadása|A JSON configuation fájl Visual Studio generáló értékeit hoz létre Azure SQL-adatbázisait.|
|AzureVM hozzáadása|Azure virtuális gép hoz létre, és visszaadja a telepített virtuális URL-CÍMÉT. A függvény a Előfeltételek beállítja, és majd felhívja a **New-AzureVM** függvénnyel (Azure modul) hozzon létre egy új virtuális számítógépre.|
|AzureVMEndpoints hozzáadása|Új beviteli végpontok ad hozzá egy virtuális számítógépre, és az új végpontot virtuális készülék adja eredményül.|
|AzureVMStorage hozzáadása|Létrehoz egy új Azure tárterület-fiókot az aktuális előfizetés. A fiók neve "devtest" egyedi alfanumerikus karakterlánc követ kezdődik. A függvény értéke az új tárterület-fiók nevére. Meg kell adnia, akár egy helyet, vagy az új tároló fiók affinitás csoport.|
|AzureWebsite hozzáadása|Létrehoz egy webhely megadott nevét és helyét. Ez a függvény a **New-AzureWebsite** függvény az Azure modul felvételét. Az előfizetés már nem tartalmazza a megadott névvel rendelkező webhelyet, ha ez a funkció hoz létre a webhelyet, és egy webhely-objektum adja eredményül. Ellenkező esetben eredménye `$null`.|
|Biztonsági másolat-előfizetés|Az aktuális Azure előfizetés menti a `$Script:originalSubscription` parancsfájl hatókör változó. Ez a funkció menti az aktuális Azure előfizetést (szerint kapott `Get-AzureSubscription -Current`) és a tárterület-fiók és az előfizetést, a jelen parancsfájl módosítják (a változóban tárolja `$UserSpecifiedSubscription`) és a tárterület-fiókját, parancsfájl hatókör. Mentse az értékeket, a funkció használata, például: `Restore-Subscription`, ha vissza szeretne állítani az eredeti aktuális előfizetés, tárterület fiók aktuális állapotát, ha az aktuális állapot módosultak.|
|Keresés-AzureVM|A megadott Azure virtuális gép kap.|
|Formátum-DevTestMessageWithTime|A dátum és idő üzenetre lefoglalja. Ez a függvény a hiba és részletes adatfolyamok írt üzenetek készült.|
|Get-AzureSQLDatabaseConnectionString|Összeállítja a kapcsolati karakterlánc, egy Azure SQL-adatbázishoz való csatlakozáshoz.|
|Get-AzureVMStorage|Nevét adja meg az első tároló számla "devtest*" név mintázattal (kis-és nagybetűk) a megadott helyen, vagy a affinitás csoportban. Ha a "devtest*" tárterület-fiók nem egyezik meg, a hely vagy affinitás csoport, a függvény figyelmen kívül hagyja. Adjon meg egy helyet vagy egy affinitás csoport.|
|Get-MSDeployCmd|Adja vissza az MsDeploy.exe eszköz futtatása parancsot.|
|Új AzureVMEnvironment|Megkeresi vagy létrehoz egy virtuális gépet, amely megfelel az értékeket a JSON konfigurációs fájl-előfizetésben.|
|Közzététel WebPackage|Felhasználási MsDeploy.exe webes csomag közzététel. Zip-erőforrások üzembe fel a fájlt. Ez a funkció nem hoz létre bármely kimeneti. Ha nem sikerül MSDeploy.exe a hívást, a függvény a kivételt okoz. Használja a kimeneti részletesebb a **-részletes** lehetőséget.|
|Közzététel WebPackageToVM|Ellenőrzi a paraméterértékeket, és ezután felhívja a **Közzététel-WebPackage** függvényt.|
|Olvasás-ConfigFile|A JSON konfigurációs fájl ellenőrzi, és a kijelölt értékek kivonat táblázatot ad vissza.|
|Visszaállítás-előfizetés|Az aktuális előfizetés visszaállítása az eredeti előfizetés.|
|Próba-AzureModule|Eredménye `$true` telepített Azure modul verzió esetén 0.7.4 vagy újabb verziójában. Eredménye `$false` Ha nincs telepítve a modul vagy korábbi verziójában. Ez a funkció nem paraméterek tartalmaz.|
|Próba-AzureModuleVersion|Eredménye `$true` az Azure modul verzió esetén 0.7.4 vagy újabb verziójában. Eredménye `$false` Ha nincs telepítve a modul vagy korábbi verziójában. Ez a funkció nem paraméterek tartalmaz.|
|Próba-HttpsUrl|A bemeneti URL-cím egy System.Uri objektum alakítja át. Eredménye `$True` Ha https URL-CÍMÉT az abszolút és a rendszer. Eredménye `$false` Ha az URL-cím relatív, a rendszer nem HTTPS vagy a bemeneti karakterlánc nem konvertálható egy URL-CÍMÉT.|
|Próba-tag|Eredménye `$true` esetén tulajdonság vagy módszer az objektum tagja. Ellenkező esetben eredménye `$false`.|
|Az írás-ErrorWithTime|Ha az aktuális idő hibaüzenet jelenik ír. Ez a függvény a hívások a **Formátum-DevTestMessageWithTime** függvénnyel prepend az időt, mielőtt a hiba-adatfolyam az üzenet írása.|
|Az írás-HostWithTime|Beír egy üzenetet, ha az aktuális idő host programhoz (**Írási állomás**). A host program írása hatása változik. A legtöbb program állomáson, a Windows PowerShell írni az alábbi üzenetek szabványos kimeneti.|
|Az írás-VerboseWithTime|Az aktuális időpontot előtaggal részletes üzenet írása. **Írás-részletes**meghívja, mert a üzenet jelenik meg, csak akkor, amikor a parancsfájl fut a **részletes** paraméterrel, vagy ha a **VerbosePreference** beállítás értéke **Folytatás gombra**.|

**Közzététel: webalkalmazás**

|Függvény neve|Leírás|
|---|---|
|Új AzureWebApplicationEnvironment|Azure erőforrások, például egy webhelyre vagy egy virtuális gép létrehoz.|
|Új WebDeployPackage|Ez a funkció nem használható. Ez a függvény a projekt létrehozásához a parancsok vehetők fel.|
|Közzététel AzureWebApplication|Azure webalkalmazás közzététele.|
|Közzététel: webalkalmazás|Hoz létre, és üzembe helyezése a Web Apps alkalmazások, a virtuális gépeken futó, a SQL-adatbázisok és a Visual Studio webes projektet tároló fiókokat.|
|Próba-: webalkalmazás|Ez a funkció nem használható. Ez a függvény a alkalmazásban tesztelje a parancsok vehetők fel.|

## <a name="next-steps"></a>Következő lépések

További tudnivalók [a Windows PowerShell parancsfájlok](https://technet.microsoft.com/library/bb978526.aspx) olvasásával parancsfájlok PowerShell, és tanulmányozza a más [Script Center](https://azure.microsoft.com/documentation/scripts/)Azure PowerShell-parancsfájlokat.
