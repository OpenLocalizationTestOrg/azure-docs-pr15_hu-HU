<properties
   pageTitle="A Windows Visual Studio segítségével a Docker ügyfél hibáinak elhárítása |} Microsoft Azure"
   description="Visual Studio létrehozása és web Apps alkalmazások telepítése a Windows Docker Visual Studio segítségével történő használatakor felmerülő problémák elhárítása"
   services="azure-container-service"
   documentationCenter="na"
   authors="mlearned"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="06/08/2016"
   ms.author="allclark" />

# <a name="troubleshooting-visual-studio-docker-development"></a>Visual Studio Docker fejlesztési – hibaelhárítás

Amikor Visual Studio Tools Docker előzetes verzió, előfordulhatnak problémák miatt az előnézeti természeti.
Az alábbi táblázat néhány gyakori problémák és megoldásuk.


## <a name="unable-to-validate-volume-mapping"></a>Nem lehet mennyiségi megfeleltetés ellenőrzése
Mennyiségi hozzárendelése a Forráskód és az alkalmazás bináris oszthat meg a tároló alkalmazás, a mappában van szükség.  Mennyiségi adott hozzárendelések a docker-compose.dev.debug.yml és docker-compose.dev.release.yml fájlokban találhatók. Fájlok a számítógépről a megváltoznak, mint a tárolók változásainak ezeket a hasonló mappaszerkezet.

Ahhoz, hogy a kötet megfeleltetés, nyissa meg a **beállítások...** "moby" Docker a Windows tálca ikonjáról, és válassza a **Megosztott meghajtók** lehetőséget.  Győződjön meg arról, hogy a projekt tárolja, amelyek betűjele, valamint a használata % helye betűjele ellenőrzése őket, és válassza az **alkalmazás**által megosztott.

Ha mennyiségi megfeleltetés működik, miután a területet megosztott teszteléséhez Újraépítés és a Visual Studióban, vagy próbálja meg az F5 egy parancs a következő kérdés:

*A Windows parancssor*

*[Megjegyzés: Ez azt feltételezi, hogy a Users mappát az "C"-meghajtón található, és hogy azt a megosztott.  Ha egy másik meghajtóra megosztotta frissítése szükség szerint]*
```
docker run -it -v /c/Users/Public:/wormhole busybox
```

*A Linux tárolóban*

```
/ # ls
```

Meg kell jelennie a könyvtárak a felhasználók/nyilvános mappából listáját.
Ha nem jelenik meg, és a /c/Users/Public nem üres, mennyiségi megfeleltetés nem megfelelően van beállítva. 

```
bin       etc       proc      sys       usr       wormhole
dev       home      root      tmp       var
```

A féreglyuk könyvtár tartalmának megtekintéséhez átalakítása az `/c/Users/Public` címtár:

```
/ # cd wormhole/
/wormhole # ls
AccountPictures  Downloads        Music            Videos
Desktop          Host             NuGet.Config     desktop.ini
Documents        Libraries        Pictures
/wormhole #
```

**Megjegyzés:** *Linux VMs dolgozik, amikor a tároló fájl rendszer-és nagybetűk.*

##<a name="build--prepareforbuild-task-failed-unexpectedly"></a>Szerkesztés: "PrepareForBuild" tevékenység nem sikerült váratlanul.

Microsoft.DotNet.Docker.CommandLine.ClientException: Hiba történt, amelyhez kapcsolódni:

Ellenőrizze, hogy az alapértelmezett docker állomás fut-e. Nyisson meg egy parancssort, majd hajtsa végre:

```
docker info
```

Ha ez a függvény hibát jelez próbálja meg a **Docker a Windows** asztali alkalmazás.  Ha az asztali alkalmazás fut majd a **moby** ikonjára a tálca legyenek láthatók. Kattintson a tálca ikonra a jobb gombbal, és nyissa meg a **beállításokat**.  Kattintson a lap **alaphelyzetbe állítása** , majd **Indítsa újra Docker …**.

##<a name="manually-upgrading-from-version-031-to-040"></a>Kézi frissítése verzióról 0.31 való 0,40


1. A projekt biztonsági mentése
1. Törölje a projekt a következő fájlokat:

    ```
      Dockerfile
      Dockerfile.debug
      DockerTask.ps1
      docker-compose-yml
      docker-compose.debug.yml
      .dockerignore
      Properties\Docker.props
      Properties\Docker.targets
    ```

1. Zárja be a megoldást, és távolítsa el a következő sort a .xproj-fájlból:

    ```
      <DockerToolsMinVersion>0.xx</DockerToolsMinVersion>
      <Import Project="Properties\Docker.props" />
      <Import Project="Properties\Docker.targets" />
    ```

1. Nyissa meg a megoldást
1. Távolítsa el a következő sort a Properties\launchSettings.json fájlból:

    ```
      "Docker": {
        "executablePath": "%WINDIR%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
        "commandLineArgs": "-ExecutionPolicy RemoteSigned .\\DockerTask.ps1 -Run -Environment $(Configuration) -Machine '$(DockerMachineName)'"
      }
    ```

1. Távolítsa el a kapcsolódó Docker project.json a publishOptions a következő fájlokat:

    ```
    "publishOptions": {
      "include": [
        ...
        "docker-compose.yml",
        "docker-compose.debug.yml",
        "Dockerfile.debug",
        "Dockerfile",
        ".dockerignore"
      ]
    },
    ```

1. Az előző verziójának és telepítése 0,40 Docker eszközök, majd a **Docker támogatási-> Hozzáadás** ismét a helyi menüből a ASP.Net Core webhelyen vagy a New. Ezzel hozzáadja az új szükséges Docker eltérések vissza a projektben. 

## <a name="an-error-dialog-occurs-when-attempting-to-add-docker-support-or-debug-f5-an-aspnet-core-application-in-a-container"></a>Egy hiba párbeszédpanel akkor fordul elő, amikor megpróbál **Docker támogatási-> Hozzáadás gombra** , vagy hibakeresési (F5) tárolóban Core ASP.NET-alkalmazások

Azt van alkalmanként látható eltávolítása és extensions telepítése a Visual Studio MEF (felügyelt bővítési keretrendszer) gyorsítótár előfordulhat, hogy sérült. Ha ez akkor fordulhat elő, a különböző hibát okozhat Docker támogatási hozzáadása párbeszédpanel, illetve kísérel meg, vagy az (F5) az ASP.NET Core alkalmazás hibakeresése. Ideiglenes megoldásként hajtsa végre az alábbi lépéseket, törlése és újragenerálása a MEF gyorsítótár.

1. Zárja be a Visual Studio összes előfordulását
1. Nyissa meg a %USERPROFILE%\AppData\Local\Microsoft\VisualStudio\14.0\
1. Az alábbi mappák törlése
     ```
       ComponentModelCache
       Extensions
       MEFCacheBackup
    ```
1. Nyissa meg a Visual Studio
1. Próbálja meg ismét az alkalmazási példát 
