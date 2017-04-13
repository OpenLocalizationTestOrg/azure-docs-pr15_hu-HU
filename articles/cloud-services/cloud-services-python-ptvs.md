<properties
    pageTitle="A Visual Studio web és dolgozó szerepkörök Python |} Microsoft Azure"
    description="Azure felhőalapú szolgáltatást, többek között a webes és dolgozó szerepeket létrehozása a Python Tools for Visual Studio segítségével áttekintése."
    services="cloud-services"
    documentationCenter="python"
    authors="thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="python"
    ms.topic="hero-article"
    ms.date="08/03/2016"
    ms.author="adegeo"/>


# <a name="python-web-and-worker-roles-with-python-tools-for-visual-studio"></a>A Python Tools for Visual Studio web és dolgozó szerepkörök Python

Ez a cikk áttekintést [Python Tools for Visual Studio][]segítségével Python webes és dolgozó szerepkörök segítségével. Megtanulhatja, hogyan készíthetnek és helyezhetnek üzembe a Python használó egyszerű Felhőszolgáltatásában Visual Studio segítségével.

## <a name="prerequisites"></a>Előfeltételek

 - Visual Studio 2013 vagy a Skype 2015
 - [Python Tools for Visual Studio][] (PTVS)
 - [Azure SDK eszközök és 2013-ban][] vagy [VIEWBEN 2015 Azure SDK eszközök][]
 - [Python 2.7 32 bites][] vagy [Python 3.5-ös 32 bites][]

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-are-python-web-and-worker-roles"></a>Mik azok a Python webes és dolgozó szerepkörök?

Azure biztosít három számítja ki az alkalmazások futtatásához modellek: [Azure alkalmazás szolgáltatás funkciója Web Apps alkalmazások][execution model-web sites], [Azure virtuális gépeken futó][execution model-vms], és [Azure Cloud Services][execution model-cloud services]. Az összes három modell Python támogatja. Felhőszolgáltatások, amelyek lehetnek a webes és dolgozó szerepkörök, *szolgáltatásként (PaaS) platformot*biztosítanak. Belül egy felhőalapú szolgáltatásba egy webes szerepkör Internet Information Services (IIS) dedikált webkiszolgálóra host előtér-webalkalmazások, hogy biztosítja, amíg dolgozó szerepkörbe futtatását is lehetővé teszi a felhasználói beavatkozás vagy beviteli független aszinkron, hosszabb ideig futó vagy örökös feladatokat.

További tudnivalókért lásd: [Mi az, hogy egy felhőalapú szolgáltatásba?].

> [AZURE.NOTE]*Looking egy egyszerű webhely létrehozásához?*
A forgatókönyv csak egy egyszerű webhelyhez előtér jár, ha fontolja meg inkább a könnyű Web Apps alkalmazások funkció Azure App szolgáltatásban. Egyszerűen frissíthet egy felhőalapú szolgáltatásba, a webhely megnő, és módosítása az igényeknek megfelelően alakíthatja. Lásd: a <a href="/develop/python/">Python Developer Center</a> projektvezetési cikkeket talál a Web Apps alkalmazások funkció az Azure alkalmazás szolgáltatás fejlesztése terjed ki.
<br />


## <a name="project-creation"></a>Projekt létrehozása

A Visual Studióban jelölje be az **Új projekt** párbeszédpanel **Python** **Azure Felhőszolgáltatásba** .

![Az új projekt párbeszédpanel](./media/cloud-services-python-ptvs/new-project-cloud-service.png)

Az Azure Felhőszolgáltatásában varázslóban hozhat létre új webes és dolgozó szerepköröket.

![Azure felhő szolgáltatási párbeszédpanel](./media/cloud-services-python-ptvs/new-service-wizard.png)

Csatlakozás Azure tárterület-fiók vagy Azure Service Bus újrafelhasználható kódot a dolgozó szerepkör sablon megtalálható.

![Felhőalapú szolgáltatás megoldás](./media/cloud-services-python-ptvs/worker.png)

Webhely vagy dolgozó szerepkörök bármikor felveheti egy meglévő felhőalapú szolgáltatást.  Megadhatja, hogy a megoldás hozzáadása a meglévő projektek, vagy hozzon létre új dokumentumokat.

![Szerepkör parancs hozzáadása](./media/cloud-services-python-ptvs/add-new-or-existing-role.png)

A felhőalapú szolgáltatás más nyelven végrehajtott szerepkörök is tartalmazhat.  Ha például végrehajtott Django, használja a Python vagy C# dolgozó szerepkörök Python webes szerepkör is.  Egyszerűen kommunikálhat a szerepkörök szolgáltatás Bus sorban várakozó vagy tároló sorok között.

## <a name="install-python-on-the-cloud-service"></a>A felhőbeli szolgáltatástól Python telepítése

>[AZURE.WARNING] A telepítő parancsprogramok vannak telepítve a Visual Studio (az Ez a cikk legutolsó frissítés) nem működnek. Ez a szakasz ismerteti a megoldás.

A telepítő parancsfájlok fő problémája, amely már nem telepíthető python. Első lépésként meghatározzák, hogy két [indítási feladatok](cloud-services-startup-tasks.md) a [ServiceDefinition.csdef](cloud-services-model-and-package.md#servicedefinitioncsdef) fájlban. Az első tevékenység (**PrepPython.ps1**) letölti és telepíti a Python futtatókörnyezet. A második tevékenység (**PipInstaller.ps1**) fut mezőpont függőségek szükség lehet telepíteni.

A parancsfájlok, az alábbi írt Python 3.5-ös kiválasztásával. Ha a verziót használni kívánt 2.x a python, állítsa a **PYTHON2** változó fájlt **a** a két indítási feladatokat és a futtatókörnyezet tevékenység: `<Variable name="PYTHON2" value="<mark>on</mark>" />`.


```xml
<Startup>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
  </Task>

  <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
    <Environment>
      <Variable name="EMULATED">
        <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
      </Variable>
      <Variable name="PYTHON2" value="off" />
    </Environment>
    
  </Task>

</Startup>
```

A **PYTHON2** és **PYPATH** változók kell adható hozzá a dolgozó indítási feladatot. A **PYPATH** változó csak akkor használja, ha a **PYTHON2** változó **be**van állítva.

```xml
<Runtime>
  <Environment>
    <Variable name="EMULATED">
      <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
    </Variable>
    <Variable name="PYTHON2" value="off" />
    <Variable name="PYPATH" value="%SystemDrive%\Python27" />
  </Environment>
  <EntryPoint>
    <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
  </EntryPoint>
</Runtime>
```

#### <a name="sample-servicedefinitioncsdef"></a>Minta ServiceDefinition.csdef

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="AzureCloudServicePython" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2015-04.2.6">
  <WorkerRole name="WorkerRole1" vmsize="Small">
    <ConfigurationSettings>
      <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" />
      <Setting name="Python2" />
    </ConfigurationSettings>
    <Startup>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PrepPython.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
      <Task executionContext="elevated" taskType="simple" commandLine="bin\ps.cmd PipInstaller.ps1">
        <Environment>
          <Variable name="EMULATED">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
          <Variable name="PYTHON2" value="off" />
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="EMULATED">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
        </Variable>
        <Variable name="PYTHON2" value="off" />
        <Variable name="PYPATH" value="%SystemDrive%\Python27" />
      </Environment>
      <EntryPoint>
        <ProgramEntryPoint commandLine="bin\ps.cmd LaunchWorker.ps1" setReadyOnProcessStart="true" />
      </EntryPoint>
    </Runtime>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
  </WorkerRole>
</ServiceDefinition>
```



Ezután hozzon létre a **PrepPython.ps1** **PipInstaller.ps1** fájlok és a **. / bin** szerepköre mappájában.

#### <a name="preppythonps1"></a>PrepPython.ps1

Ez a parancsfájl python telepíti. A **PYTHON2** felhasználásához változó értéke **és Python 2.7** telepítve, egyéb esetben Python 3.5-ös telepítve van.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if python is installed...$nl"
    if ($is_python2) {
        & "${env:SystemDrive}\Python27\python.exe"  -V | Out-Null
    }
    else {
        py -V | Out-Null
    }

    if (-not $?) {

        $url = "https://www.python.org/ftp/python/3.5.2/python-3.5.2-amd64.exe"
        $outFile = "${env:TEMP}\python-3.5.2-amd64.exe"

        if ($is_python2) {
            $url = "https://www.python.org/ftp/python/2.7.12/python-2.7.12.amd64.msi"
            $outFile = "${env:TEMP}\python-2.7.12.amd64.msi"
        }
        
        Write-Output "Not found, downloading $url to $outFile$nl"
        Invoke-WebRequest $url -OutFile $outFile
        Write-Output "Installing$nl"

        if ($is_python2) {
            Start-Process msiexec.exe -ArgumentList "/q", "/i", "$outFile", "ALLUSERS=1" -Wait
        }
        else {
            Start-Process "$outFile" -ArgumentList "/quiet", "InstallAllUsers=1" -Wait
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Already installed"
    }
}
```

#### <a name="pipinstallerps1"></a>PipInstaller.ps1

Ez a parancsfájl mezőpont felfelé meghívja, és telepíti az összes a függőségeket a **requirements.txt** fájlban. A **PYTHON2** felhasználásához változó értéke **és Python 2.7** szolgálnak, egyéb esetben Python 3.5-ös használja.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated){
    Write-Output "Checking if requirements.txt exists$nl"
    if (Test-Path ..\requirements.txt) {
        Write-Output "Found. Processing pip$nl"

        if ($is_python2) {
            & "${env:SystemDrive}\Python27\python.exe" -m pip install -r ..\requirements.txt
        }
        else {
            py -m pip install -r ..\requirements.txt
        }

        Write-Output "Done$nl"
    }
    else {
        Write-Output "Not found$nl"
    }
}
```

#### <a name="modify-launchworkerps1"></a>LaunchWorker.ps1 módosítása

>[AZURE.NOTE] **Dolgozó szerepkör** projekt, amíg **LauncherWorker.ps1** fájl az indítási fájlját végrehajtásához szükséges. **Webes szerepkör** projektben a indítási fájl inkább a projekt tulajdonságainak definiálva.

A **bin\LaunchWorker.ps1** eredetileg előkészületre sok tegye, de nem igazán működik. Cserélje ki az alábbi parancsfájlt, hogy a fájl tartalmát.

Ez a parancsfájl a **worker.py** fájlt a python projekt hívja fel. A **PYTHON2** felhasználásához változó értéke **és Python 2.7** szolgálnak, egyéb esetben Python 3.5-ös használja.

```powershell
$is_emulated = $env:EMULATED -eq "true"
$is_python2 = $env:PYTHON2 -eq "on"
$nl = [Environment]::NewLine

if (-not $is_emulated)
{
    Write-Output "Running worker.py$nl"

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
else
{
    Write-Output "Running (EMULATED) worker.py$nl"

    # Customize to your local dev environment

    if ($is_python2) {
        cd..
        iex "$env:PYPATH\python.exe worker.py"
    }
    else {
        cd..
        iex "py worker.py"
    }
}
```

#### <a name="pscmd"></a>PS.cmd

A Visual Studio sablonok hozza létre a **ps.cmd** fájl a **. / bin** mappát. A parancsprogram hívja meg a fenti PowerShell-csomagolópapír parancsfájlok, és a PowerShell-csomagolópapír nevű neve alapján naplózás biztosít. Ha ez a fájl nem lett létrehozott, az alábbiakban mit kell azt. 

```bat
@echo off

cd /D %~dp0

if not exist "%DiagnosticStore%\LogFiles" mkdir "%DiagnosticStore%\LogFiles"
%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Unrestricted -File %* >> "%DiagnosticStore%\LogFiles\%~n1.txt" 2>> "%DiagnosticStore%\LogFiles\%~n1.err.txt"
```



## <a name="run-locally"></a>Futtassa a helyi meghajtóra

Ha a felhőalapú szolgáltatás projekt beállítása az indítási projekt, és nyomja le az F5, a felhőalapú szolgáltatást a helyi Azure irányító fog futni.

Bár PTVS támogatja a irányító, hibakeresése során (például töréspontok) a indítása nem fognak működni.

Szeretné hibakeresése a webes és dolgozó szerepkörök, a szerepkör-projekt beállítása az indítási projekt, és helyette hibakeresési, amely.  Is beállíthatja, hogy több indítási projekt.  Kattintson a jobb gombbal a megoldást, és válassza az **Indítás projektek beállítása**.

![Megoldás indítási projekt tulajdonságai](./media/cloud-services-python-ptvs/startup.png)

## <a name="publish-to-azure"></a>Azure közzététele

Való közzétételéhez kattintson a jobb gombbal a felhőalapú szolgáltatás project a megoldás, és válassza a **Közzététel**.

![Microsoft Azure bejelentkezés közzététele](./media/cloud-services-python-ptvs/publish-sign-in.png)

Kövesse a varázsló lépéseit. Ha kell, engedélyezze a távoli asztal. Távoli asztali akkor hasznos, ha hibakeresési valamit, amit szeretne.

Amikor befejezte a beállítások konfigurálásával, kattintson a **Közzététel**gombra.

Néhány előrehaladását megjelennek a kimeneti ablakban, majd megjelenik a Microsoft Azure tevékenység naplója ablakban.

![Microsoft Azure tevékenység naplója ablak](./media/cloud-services-python-ptvs/publish-activity-log.png)

Telepítési fog néhány percet igénybe vehet, majd a webhely, illetve dolgozó szerepkörök Azure futnia kell!

### <a name="investigate-logs"></a>Vizsgálja meg a naplók

Miután a felhőbe szolgáltatás virtuális gép elindul, és Python telepíti, megnézheti a naplók bármilyen hiba üzenetek kereséséhez. Ezek a naplók találhatók a **C:\Resources\Directory\\{szerepkör} \LogFiles** mappát. **PrepPython.err.txt** legalább egy hiba lesz a vágólapra, amikor megpróbálja észleli, ha telepítve van a Python és **PipInstaller.err.txt** előfordulhat, hogy panaszt mezőpont elavult változatáról.

## <a name="next-steps"></a>Következő lépések

További információ a Visual Studio az Python eszközök webes és dolgozó szerepkörök használatáról az PTVS dokumentációjában olvasható:

- [Felhőalapú szolgáltatás projektek][]

A webes és dolgozó szerepkörök Azure tároló vagy szolgáltatás Bus, például Azure szolgáltatásai használatáról további információt az alábbi cikkekben talál.

- [BLOB-szolgáltatás][]
- [Táblázat szolgáltatás][]
- [Várólista szolgáltatást.][]
- [A szolgáltatás Bus sorban várakozó][]
- [Szolgáltatás Bus témakörök][]


<!--Link references-->

[Mi az, hogy egy felhőalapú szolgáltatásba?]: cloud-services-choose-me.md
[execution model-web sites]: ../app-service-web/app-service-web-overview.md
[execution model-vms]: ../virtual-machines/virtual-machines-windows-about.md
[execution model-cloud services]: cloud-services-choose-me.md
[Python Developer Center]: /develop/python/

[BLOB-szolgáltatás]: ../storage/storage-python-how-to-use-blob-storage.md
[Várólista szolgáltatást.]: ../storage/storage-python-how-to-use-queue-storage.md
[Táblázat szolgáltatás]: ../storage/storage-python-how-to-use-table-storage.md
[A szolgáltatás Bus sorban várakozó]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md
[Szolgáltatás Bus témakörök]: ../service-bus-messaging/service-bus-python-how-to-use-topics-subscriptions.md


<!--External Link references-->

[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs
[Felhőalapú szolgáltatás projektek]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Szoftverfejlesztési eszközök és 2013-hoz készült]: http://go.microsoft.com/fwlink/?LinkId=323510
[Azure SDK eszközök VIEWBEN 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[A 32 bites 2.7 Python]: https://www.python.org/downloads/
[Python 3.5-ös 32 bites]: https://www.python.org/downloads/
