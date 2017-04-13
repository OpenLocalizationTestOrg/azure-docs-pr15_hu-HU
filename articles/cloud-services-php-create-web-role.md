<properties
    pageTitle="Webes és dolgozó szerepkörök PHP |} Microsoft Azure"
    description="Segédvonalhoz PHP webes és dolgozó szerepkörök létrehozása az Azure felhőalapú szolgáltatásba, és a PHP futtatókörnyezet konfigurálása."
    services=""
    documentationCenter="php"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

#<a name="how-to-create-php-web-and-worker-roles"></a>Hogyan hozhat létre PHP webes és dolgozó szerepkörök

## <a name="overview"></a>– Áttekintés

Ez az útmutató megtudhatja, hogyan PHP webes vagy dolgozó szerepkörök létrehozása a Windows fejlesztői környezet, egy adott verziójához PHP választhat a rendelkezésre álló "beépített" verziók, PHP beállításainak módosításához, bővítmények engedélyezése, és végül telepítse az Azure. Azt is megtudhatja, hogy miként konfigurálása webes vagy dolgozó szerepkörbe használni egy Ön által PHP futtatókörnyezet (az egyéni konfigurációs és bővítmények).

## <a name="what-are-php-web-and-worker-roles"></a>Mik azok a PHP webes és dolgozó szerepkörök?

Azure biztosít három számítja ki az alkalmazások futtatásához modellek: Azure alkalmazás szolgáltatás, Azure virtuális gépeken futó és Azure Cloud Services. Az összes három modell PHP támogatja. Webes és dolgozó szerepköröket tartalmaz, Felhőszolgáltatások *szolgáltatásként (PaaS) platformot*biztosít. Belül egy felhőalapú szolgáltatásba egy webes szerepkör biztosít a dedikált Internet Information Services (IIS) webkiszolgálót host előtér-webalkalmazások. Dolgozó szerepkör futtathatók felhasználói beavatkozás vagy beviteli független aszinkron, hosszabb ideig futó vagy örökös tevékenységek.

Beállításokkal kapcsolatos további tudnivalókért lásd: [Azure által biztosított üzemeltetési beállítások számítja ki](./cloud-services/cloud-services-choose-me.md).

## <a name="download-the-azure-sdk-for-php"></a>Töltse le az Azure SDK PHP

Az [Azure SDK PHP] több összetevők áll. Ez a cikk ketten fogja használni: Azure PowerShell és az Azure emulátor. Ezt a két összetevőt a Microsoft webes Platform Installer keresztül telepíthető. További információért megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](powershell-install-configure.md).

## <a name="create-a-cloud-services-project"></a>Cloud Services projekt létrehozása

Az első az interneten vagy dolgozó PHP szerepkör létrehozása lépésként projektet Azure szolgáltatás hozzon létre. az Azure szolgáltatás project web és dolgozó szerepkörök logikai tároló szolgál, és a projekt [szolgáltatás meghatározása (.csdef)] és a [szolgáltatás beállítása (.cscfg)] fájlok tartalmaz.

Azure szolgáltatás új projekt létrehozásához Azure PowerShell futtatása rendszergazdaként, és hajtsa végre az alábbi parancsot:

    PS C:\>New-AzureServiceProject myProject

Ez a parancs létrehoz egy új könyvtárat (`myProject`), amelyhez adhat hozzá webes és dolgozó szerepköröket.

## <a name="add-php-web-or-worker-roles"></a>Webes vagy dolgozó szerepkörök PHP hozzáadása

Webes PHP szerepkörbe hozzáadása a projekthez, futtassa a következő parancsot a projekt legfelső szintű címtáron belül:

    PS C:\myProject> Add-AzurePHPWebRole roleName

Egy dolgozó szerepkört a paranccsal:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [AZURE.NOTE] A `roleName` paraméter nem kötelező. Ha nem adja meg, a szerepkör neve automatikusan jönnek létre. Az első webes szerepkör létrehozott lesz `WebRole1`, a második lesz `WebRole2`és így tovább. Az első dolgozó szerepkör létrehozott lesz `WorkerRole1`, a második lesz `WorkerRole2`és így tovább.

## <a name="specify-the-built-in-php-version"></a>Adja meg a beépített PHP-verzió

PHP webes vagy dolgozó szerepkör hozzáadása a projekthez, amikor a projekt konfigurációs fájlok módosulnak, így települnek PHP, ha telepítve van az alkalmazás minden példányában webes vagy dolgozó a. Az alapértelmezés szerint telepített PHP verziója jelenik meg, futtassa a következő parancsot:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

A fenti parancs kimenetét fog megjelenése hasonlít az alább látható módon. Ebben a példában a `IsDefault` jelző `true` az PHP 5.3.17, jelezve, hogy legyen az alapértelmezett PHP-verzió telepítése.

    Runtime Version     PackageUri                      IsDefault
    ------- -------     ----------                      ---------
    Node 0.6.17         http://nodertncu.blob.core...   False
    Node 0.6.20         http://nodertncu.blob.core...   True
    Node 0.8.4          http://nodertncu.blob.core...   False
    IISNode 0.1.21      http://nodertncu.blob.core...   True
    Cache 1.8.0         http://nodertncu.blob.core...   True
    PHP 5.3.17          http://nodertncu.blob.core...   True
    PHP 5.4.0           http://nodertncu.blob.core...   False

Beállíthatja, hogy a PHP futtatókörnyezet verzió valamelyik, az PHP-verziók szerint vannak listázva. Például, hogy állítsa PHP-verziót (nevű szerepkörhöz `roleName`) 5.4.0, használja a következő parancsot:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [AZURE.NOTE] A rendelkezésre álló PHP-verziók a jövőben változhat.

## <a name="customize-the-built-in-php-runtime"></a>A beépített PHP futtatókörnyezet testreszabása

A fent, többek között a módosítását lépését telepített PHP futtatókörnyezet konfigurációja teljes hozzáféréssel rendelkeznek, `php.ini` beállítások és -bővítmények engedélyezése.

A beépített PHP futtatókörnyezet testreszabásához kövesse az alábbi lépéseket:

1. Adja hozzá az új mappába, `php`, hogy a `bin` könyvtár a webes szerepkör. Adott dolgozó szerepkör adja hozzá a szerepkör a legfelső szintű címtár.
2. Az a `php` mappát, hozzon létre egy másik nevű mappát `ext`. Bármelyik elhelyezni `.dll` bővítményfájlok (például `php_mongo.dll`) ebben a mappában engedélyezni kívánt.
3. Adja hozzá a `php.ini` fájlt a `php` mappát. Minden olyan egyéni bővítmények engedélyezése, és állítsa be minden olyan PHP irányelvek ezt a fájlt. Tegyük fel például, kapcsolja be a keresett `display_errors` a és engedélyezése a `php_mongo.dll` kiterjesztése, a tartalmát a `php.ini` fájl lenne, az alábbi képlettel történik:

        display_errors=On
        extension=php_mongo.dll

> [AZURE.NOTE] Nincs explicit módon beállított a beállításokat a `php.ini` fog automatikusan biztosítják fájl a alapértelmezett értéket adhat meg. Jó helyen jár, tartsa szem előtt, hogy is hozzáadhat egy olyan `php.ini` fájlt.

## <a name="use-your-own-php-runtime"></a>A saját PHP futtatókörnyezet használata
Egyes esetekben az egy beépített PHP futtatókörnyezet kijelölését, és konfigurálja úgy, ahogy a fenti helyett érdemes saját PHP futtatókörnyezet szükséges. Például az azonos PHP futtatókörnyezet is használhatja az interneten vagy dolgozó szerepkör-ben a fejlesztői környezet segítségével. Ezzel megkönnyíti az annak érdekében, hogy az alkalmazás nem változik a termelési környezetén viselkedését.

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a>Egy webes szerepkör használni a saját PHP futtatókörnyezet konfigurálása

Egy webes szerepkör használni egy Ön által PHP futtatókörnyezet beállításához kövesse az alábbi lépéseket:

1. Azure Service projekteket létrehozni, és PHP webes szerepkör hozzáadása korábban ebben a témakörben leírt módon.
2. Hozzon létre egy `php` mappa a `bin` mappát, amely a webes szerepkör legfelső szintű könyvtárában, majd adjon hozzá a PHP futtatókörnyezet (az összes bináris, konfigurációs fájl, almappák, stb.) a `php` mappát.
3. (NEM KÖTELEZŐ) Ha a PHP futtatókörnyezet használja a [Microsoft SQL Server PHP-illesztőprogramok][sqlsrv drivers], meg kell telepíteni az [SQL Server natív ügyfél 2012] webes szerepköre konfigurálása[ sql native client] mikor kiépítve. Ehhez a [sqlncli.msi x64 installer] hozzáadása a `bin` a webes szerepkör legfelső szintű mappát. Az indítási parancsfájlt, a következő lépésben leírt meg automatikusan a telepítő fog futni, amikor a szerepkör már kiépítve. Ha a PHP futtatókörnyezet nem használja a Microsoft Drivers PHP az SQL Server, a következő sort eltávolítható a parancsfájlt, a következő lépésben látható:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Konfigurálja az [Internet Information Services (IIS)] indítási feladatot definiálása[ iis.net] a PHP futtatókörnyezet használatával kezelheti a kérelem `.php` lapok. Ehhez nyissa meg a `setup_web.cmd` fájlt (a a `bin` a webes szerepkör legfelső szintű címtár-fájl) szövegszerkesztőben és annak tartalmát a következő parancsfájl cserélje le:

        @ECHO ON
        cd "%~dp0"

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
        SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
        %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"

5. Az alkalmazás fájlok hozzáadása a webes szerepkör legfelső szintű címtárhoz. Ezt fogja az érintett webkiszolgálóra legfelső szintű könyvtárat.

6. Közzététel az alkalmazás a [Közzététel az alkalmazás](#how-to-publish-your-application) szakaszban leírt módon.

> [AZURE.NOTE] A `download.ps1` parancsprogram (a a `bin` a webes szerepkör legfelső szintű könyvtár mappát) saját PHP futtatókörnyezet a fenti lépések után törölheti.

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a>A saját PHP futtatókörnyezet használandó dolgozó szerepkör konfigurálása

Egy Ön által PHP futtatókörnyezet használandó dolgozó szerepkörbe beállításához kövesse az alábbi lépéseket:

1. Azure Service projekteket létrehozni, és PHP dolgozó szerepkör hozzáadása korábban ebben a témakörben leírt módon.
2. Hozzon létre egy `php` mappát a dolgozó szerepkör legfelső szintű, majd adja hozzá a PHP futtatókörnyezet (az összes bináris, konfigurációs fájl, almappák, stb.) a `php` mappát.
3. (NEM KÖTELEZŐ) Ha a PHP futtatókörnyezet használja a [Microsoft SQL Server PHP-illesztőprogramok][sqlsrv drivers], meg kell telepíteni az [SQL Server natív ügyfél 2012] dolgozó szerepköre konfigurálása[ sql native client] mikor kiépítve. Ehhez a [sqlncli.msi x64 installer] hozzáadása a dolgozó szerepkör legfelső szintű címtárhoz. Az indítási parancsfájlt, a következő lépésben leírt meg automatikusan a telepítő fog futni, amikor a szerepkör már kiépítve. Ha a PHP futtatókörnyezet nem használja a Microsoft Drivers PHP az SQL Server, a következő sort eltávolítható a parancsfájlt, a következő lépésben látható:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

4. Határozza meg az indítási feladatot ad a `php.exe` végrehajtható szeretne a szerepkör már kiépítve a dolgozó szerepkör PATH környezeti változó. Ehhez nyissa meg a `setup_worker.cmd` a fájl (a dolgozó szerepkör legfelső szintű könyvtár) szövegszerkesztőben, és cserélje le a tartalmát a következőt:

        @echo on

        cd "%~dp0"

        echo Granting permissions for Network Service to the web root directory...
        icacls ..\ /grant "Network Service":(OI)(CI)W
        if %ERRORLEVEL% neq 0 goto error
        echo OK

        if "%EMULATED%"=="true" exit /b 0

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

        setx Path "%PATH%;%~dp0php" /M

        if %ERRORLEVEL% neq 0 goto error

        echo SUCCESS
        exit /b 0

        :error

        echo FAILED
        exit /b -1

5. Alkalmazás fájlok hozzáadása a dolgozó szerepkör legfelső szintű címtárhoz.

6. Közzététel az alkalmazás a [Közzététel az alkalmazás](#how-to-publish-your-application) szakaszban leírt módon.

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a>Az alkalmazás a számítási és tárolási emulátor futtatása

Az Azure emulátor adja meg a helyi környezet, amelyben tesztelheti az Azure alkalmazás mielőtt beállítaná őket a felhőbe. Néhány a emulátor és az Azure környezet közötti különbségek vannak. Ez jobban, című cikkben talál részletes [használata az Azure tároló irányító fejlesztés és tesztelésére](./storage/storage-use-emulator.md).

Figyelje meg, hogy a számítási irányító használatához helyileg telepített PHP kell rendelkeznie. A számítási irányító az alkalmazásnak a futtatására helyi PHP telepítésének fogja használni.

A projekt a emulátor futtatásához hajtsa végre a következő parancsot a projekt legfelső szintű címtárból:

    PS C:\MyProject> Start-AzureEmulator

A kimenet alábbihoz hasonló jelennek meg:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Megjelenik az alkalmazás a irányító egy webböngészőt megnyitva, majd a helyi cím kimeneti tallózással fut (`http://127.0.0.1:81` a fenti példa kimeneti).

Le szeretné állítani a emulátor, ez a parancs végrehajtása:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Az alkalmazás közzététele

Az alkalmazás közzétételéhez kell először importálja a közzétételi beállításokat az [Importálás-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) parancsmag használatával. Kattintson az alkalmazás közzéteheti a [Közzététel-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) parancsmag használatával. Bejelentkezés tudni megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](powershell-install-configure.md).

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [PHP Developer Center](/develop/php/).

[Azure SDK PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[szolgáltatás-definíció (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[szolgáltatás beállítása (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648
