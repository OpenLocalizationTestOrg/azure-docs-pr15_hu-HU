<properties
   pageTitle=".NET telepítése egy felhőalapú szolgáltatás szerepkör |} Microsoft Azure"
   description="Ez a cikk leírja, hogy miként webhelyen való manuális telepítéséhez .NET-keretrendszer felhőalapú szolgáltatás Web és dolgozó szerepkörök"
   services="cloud-services"
   documentationCenter=".net"
   authors="thraka"
   manager="timlt"
   editor=""/>

<tags
   ms.service="cloud-services"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/10/2016"
   ms.author="adegeo"/>

# <a name="install-net-on-a-cloud-service-role"></a>.NET telepítése egy felhőalapú szolgáltatás szerepkör 

Ez a cikk ismerteti, hogyan telepítheti a .NET-keretrendszer felhőalapú szolgáltatás Web és dolgozó szerepkörök. .NET 4.6.1 telepítse az Azure Vendég OS család 4 használhatja ezeket a lépéseket. A vendégként való bekapcsolódáshoz OS kapcsolatos legújabb információkat verziókban [Azure Vendég OS elengedi és SDK kompatibilitási mátrix](cloud-services-guestos-update-matrix.md)témakörben talál.

A .NET telepítése a webes és dolgozó szerepkörök folyamata magában foglalja az a felhő Project és a telepítő indítása a szerepkör indítási feladatok részeként részeként a .NET telepítőcsomagot.  

## <a name="add-the-net-installer-to-your-project"></a>A .NET telepítő hozzáadása a projekthez
- Töltse le a a webes telepítő a .NET-keretrendszer telepíteni szeretné az
    - [.NET 4.6.1 webes Installer](http://go.microsoft.com/fwlink/?LinkId=671729)
- A webes szerepkör
  1. **Megoldás Explorer**, a **szerepkörök** a felhőalapú szolgáltatás projektben kattintson a jobb gombbal a szerepkört, és válassza ki azt **hozzáadása > Új mappa**. *Tároló* nevű mappa létrehozása
  2. Kattintson arra a **bin** mappát, és válassza a **hozzáadása > meglévő elem**. Jelölje ki a .NET telepítő, és vegye fel a bin mappát.
- Adott dolgozó szerepkör
  1. Kattintson jobb gombbal a szerepkört, és válassza ki azt a **hozzáadása > meglévő elem**. Jelölje ki a .NET telepítő, és vegye fel a szerepkör. 

Adja hozzá a fájlokat, ezzel a módszerrel a szerepkör tartalom mappába automatikusan lesz hozzáadva a felhőalapú szolgáltatás csomag és a virtuális gépen egységes helyre rendszerbe. Ismételje meg ezt a folyamatot minden webes és dolgozó szerepkör a felhőszolgáltatásában így összes szerepkörének lesz egy másolata, a telepítő.

> [AZURE.NOTE] Akkor is, ha az alkalmazás célként .NET 4.6 .NET 4.6.1 kell telepíthető a Felhőbeli szolgáltatástól szerepkör. Az Azure Vendég OS frissítések [3098779](https://support.microsoft.com/kb/3098779) és a [3097997](https://support.microsoft.com/kb/3097997)tartalmazza. Fölött a frissítések telepítése a .NET 4.6 problémákat okozhat a .NET-alkalmazások fut, így közvetlenül telepítenie kell a .NET 4.6.1 .NET 4.6 helyett. További tudnivalókért olvassa el a [KB 3118750](https://support.microsoft.com/kb/3118750)című témakört.

![Szerepkör tartalmának telepítőfájl][1]

## <a name="define-startup-tasks-for-your-roles"></a>A szerepkörökhöz indítási feladatok meghatározása
Indítási feladatokat hajtanak végre szerepkör megkezdése előtt teszi lehetővé. A .NET-keretrendszer telepítése az indítási tevékenység keretében biztosítja, hogy telepítve van az alkalmazás kódja bármelyikét futása előtt keretében. További információt az indításkor feladatok lásd: [Indítási feladat futtatása Azure-ban](cloud-services-startup-tasks.md). 

1. Adja hozzá a következő összes szerepkörének csoportban a **WebRole** vagy **WorkerRole** csomópont *ServiceDefinition.csdef* fájl:
    
    ```xml
    <LocalResources>
      <LocalStorage name="NETFXInstall" sizeInMB="1024" cleanOnRoleRecycle="false" />
    </LocalResources>    
    <Startup>
      <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
        <Environment>
          <Variable name="PathToNETFXInstall">
            <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='NETFXInstall']/@path" />
          </Variable>
          <Variable name="ComputeEmulatorRunning">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
          </Variable>
        </Environment>
      </Task>
    </Startup>
    ```

    A fenti konfiguráció futtathatók az konzol parancs *install.cmd* rendszergazdai jogosultságokkal rendelkező, akkor telepítheti a .NET-keretrendszer. A konfiguráció is létrehoz egy LocalStorage *NETFXInstall*nevét. Az indítási parancsfájlt állítja a temp mappa használata a helyi tároló erőforrás, hogy a .NET-keretrendszer telepítő fog letölti és telepíti az erőforrás. Fontos, az erőforrás méretének beállításához legalább 1024MB megfelelően telepíti a keretében biztosítja. További információt az indítási feladatok lásd: [Gyakori Felhőszolgáltatásba indítási feladatok](cloud-services-startup-tasks-common.md) 

2. Hozzon létre egy fájl **install.cmd** , és vegye fel az összes szerepkörök szerint kattintson a szerepkör a jobb gombbal, és válassza **hozzáadása > meglévő elem...**. Így összes szerepkörének célszerű most már rendelkezik, a .NET telepítőfájl, valamint a install.cmd fájlt.
    
    ![Az összes fájl tartalmának szerepkör][2]

    > [AZURE.NOTE] Egy egyszerű szövegszerkesztőben, például a Jegyzettömbben használja ezt a fájlt. Ha a Visual Studio hozzon létre egy szövegfájlt, és nevezze át a ".cmd" kattintva továbbra is jelenléte a UTF-8 bájtos sorrendben be van jelölve, és fut, az első sor parancsfájl hibát eredményez. Mintha létrehozása a Visual Studio segítségével a fájl szabadság hozzáadása egy REM (Megjegyzés) a fájl első sorában, hogy futtatásakor figyelmen kívül. 

3. Adja hozzá a következőt a **install.cmd** fájlt:

    ```
    REM Set the value of netfx to install appropriate .NET Framework. 
    REM ***** To install .NET 4.5.2 set the variable netfx to "NDP452" *****
    REM ***** To install .NET 4.6 set the variable netfx to "NDP46" *****
    REM ***** To install .NET 4.6.1 set the variable netfx to "NDP461" *****
    REM ***** To install .NET 4.6.2 set the variable netfx to "NDP462" *****
    set netfx="NDP461"
    
    REM ***** Set script start timestamp *****
    set timehour=%time:~0,2%
    set timestamp=%date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2%
    set "log=install.cmd started %timestamp%."
    
    REM ***** Exit script if running in Emulator *****
    if %ComputeEmulatorRunning%=="true" goto exit
    
    REM ***** Needed to correctly install .NET 4.6.1, otherwise you may see an out of disk space error *****
    set TMP=%PathToNETFXInstall%
    set TEMP=%PathToNETFXInstall%
    
    REM ***** Setup .NET filenames and registry keys *****
    if %netfx%=="NDP462" goto NDP462
    if %netfx%=="NDP461" goto NDP461
    if %netfx%=="NDP46" goto NDP46
        set "netfxinstallfile=NDP452-KB2901954-Web.exe"
        set netfxregkey="0x5cbf5"
        goto logtimestamp
    
    :NDP46
    set "netfxinstallfile=NDP46-KB3045560-Web.exe"
    set netfxregkey="0x6004f"
    goto logtimestamp
    
    :NDP461
    set "netfxinstallfile=NDP461-KB3102438-Web.exe"
    set netfxregkey="0x6040e"
    goto logtimestamp
    
    :NDP462
    set "netfxinstallfile=NDP462-KB3151802-Web.exe"
    set netfxregkey="0x60632"
    
    :logtimestamp
    REM ***** Setup LogFile with timestamp *****
    md "%PathToNETFXInstall%\log"
    set startuptasklog="%PathToNETFXInstall%log\startuptasklog-%timestamp%.txt"
    set netfxinstallerlog="%PathToNETFXInstall%log\NetFXInstallerLog-%timestamp%"
    echo %log% >> %startuptasklog%
    echo Logfile generated at: %startuptasklog% >> %startuptasklog%
    echo TMP set to: %TMP% >> %startuptasklog%
    echo TEMP set to: %TEMP% >> %startuptasklog%
    
    REM ***** Check if .NET is installed *****
    echo Checking if .NET (%netfx%) is installed >> %startuptasklog%
    set /A netfxregkeydecimal=%netfxregkey%
    set foundkey=0
    FOR /F "usebackq skip=2 tokens=1,2*" %%A in (`reg query "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full" /v Release 2^>nul`) do @set /A foundkey=%%C
    echo Minimum required key: %netfxregkeydecimal% -- found key: %foundkey% >> %startuptasklog%
    if %foundkey% GEQ %netfxregkeydecimal% goto installed
    
    REM ***** Installing .NET *****
    echo Installing .NET with commandline: start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog%  /chainingpackage "CloudService Startup Task" >> %startuptasklog%
    start /wait %~dp0%netfxinstallfile% /q /serialdownload /log %netfxinstallerlog% /chainingpackage "CloudService Startup Task" >> %startuptasklog% 2>>&1
    if %ERRORLEVEL%== 0 goto installed
        echo .NET installer exited with code %ERRORLEVEL% >> %startuptasklog%   
        if %ERRORLEVEL%== 3010 goto restart
        if %ERRORLEVEL%== 1641 goto restart
        echo .NET (%netfx%) install failed with Error Code %ERRORLEVEL%. Further logs can be found in %netfxinstallerlog% >> %startuptasklog%
    
    :restart
    echo Restarting to complete .NET (%netfx%) installation >> %startuptasklog%
    EXIT /B %ERRORLEVEL%
    
    :installed
    echo .NET (%netfx%) is installed >> %startuptasklog%
    
    :end
    echo install.cmd completed: %date:~-4,4%%date:~-10,2%%date:~-7,2%-%timehour: =0%%time:~3,2% >> %startuptasklog%
    
    :exit
    EXIT /B 0
    ```
        
    A telepítési parancsfájlt ellenőrzi, hogy a megadott .NET-keretrendszer verziója már telepítve van a számítógépen a beállításjegyzék lekérdezésével. Ha nincs telepítve a .NET-verziót, majd a .net webes telepítő van indított. Az esetleges problémák elhárítására a parancsfájl naplózza az összes tevékenység *startuptasklog-(currentdatetime) .txt* *InstallLogs* helyi tárolóban lévő nevű fájlt.

    > [AZURE.NOTE] A parancsprogram továbbra is bemutatja, hogyan folytonosságot 4.5.2 .NET vagy .NET 4.6 telepítéséhez. Nincs szükség az meg manuálisan telepíteni a .NET 4.5.2, mivel már az Azure Vendég OS rendszerben elérhető. .NET 4.6 telepítése helyett telepítse közvetlenül .NET 4.6.1 [KB 3118750](https://support.microsoft.com/kb/3118750)miatt.
      

## <a name="configure-diagnostics-to-transfer-the-startup-task-logs-to-blob-storage"></a>Az indítási tevékenység naplók blob-tároló átadni diagnosztika konfigurálása 
Minden olyan telepítési problémáinak egyszerűsítése érdekében Azure diagnosztika bármely az indítási parancsfájlt vagy a .NET telepítő blob-tárolóhoz által létrehozott naplófájlok át is beállíthatja. Ezt a megközelítést egyszerűen a naplófájlok letöltésével blob-tárolóhoz ahelyett, hogy a távoli asztali be a szerepkör a naplókat is megtekintheti.

Diagnosztikai nyissa meg a *diagnostics.wadcfgx* beállítása, és adja hozzá a **könyvtárak** csomópont csoportban a következő: 

```xml 
<DataSources>
 <DirectoryConfiguration containerName="netfx-install">
  <LocalResource name="NETFXInstall" relativePath="log"/>
 </DirectoryConfiguration>
</DataSources>
```

Ez konfigurálja az azure diagnosztika adhatja át a *naplózási* könyvtár az *NETFXInstall* erőforrás csoportban lévő összes fájl a *netfx-telepítés* blob-tárolóban lévő diagnosztika tárterület-fiókba.

## <a name="deploying-your-service"></a>A szolgáltatás üzembe helyezése 
A szolgáltatás telepítésekor az indítási feladatok futtatni, és telepítse a .NET-keretrendszer, ha még nincs telepítve. A szerepkörök közben telepítésére van beállítva, és előfordulhat, hogy még ha, indítsa újra a keretrendszer telepített megkívánja keretében elfoglalt állapotú lesz. 

## <a name="additional-resources"></a>További források

- [A .NET-keretrendszer telepítése][]
- [Útmutató: határozza meg, mely a .NET-keretrendszer verziók vannak telepítve][]
- [.NET-keretrendszer hibaelhárítási telepítések][]

[Útmutató: határozza meg, mely a .NET-keretrendszer verziók vannak telepítve]: https://msdn.microsoft.com/library/hh925568.aspx
[A .NET-keretrendszer telepítése]: https://msdn.microsoft.com/library/5a4x27ek.aspx
[.NET-keretrendszer hibaelhárítási telepítések]: https://msdn.microsoft.com/library/hh925569.aspx

<!--Image references-->
[1]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithinstallerfiles.png
[2]: ./media/cloud-services-dotnet-install-dotnet/rolecontentwithallfiles.png

 
