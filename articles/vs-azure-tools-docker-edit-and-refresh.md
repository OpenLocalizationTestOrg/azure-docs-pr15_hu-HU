<properties
   pageTitle="Helyi Docker tárolóban alkalmazások hibakeresési |} Microsoft Azure"
   description="Megtudhatja, hogy miként helyi Docker tároló futtató-alkalmazás módosítása, szerkesztése és frissítése a tároló frissítése, és állítsa be a hibakeresési töréspontok"
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
   ms.date="07/22/2016"
   ms.author="mlearned" />

# <a name="debugging-apps-in-a-local-docker-container"></a>Helyi Docker tárolóban hibakeresési alkalmazások

## <a name="overview"></a>– Áttekintés
A Visual Studio eszközök Docker fejlesztése következetes módszert kínál a egy, és ellenőrizze a helyi meghajtóra Linux Docker tároló az alkalmazást.
Nem kell indítani a tároló minden alkalommal, amikor használni szeretné, hogy a kód megváltoztatása.
Ez a cikk mutatják be a "Szerkesztés és frissítése" funkció használatáról indítsa el az ASP.NET Core Web app helyi Docker tároló, és módosítsa tetszés szerint, majd frissítse a böngészőt, ezek a módosítások megtekintéséhez.
Azt is megtudhatja, hogyan kell beállítani, a hibakereséshez töréspontok.

> [AZURE.NOTE] A Windows tároló támogatási fog lesz, az elkövetkező kiadásokban

## <a name="prerequisites"></a>Előfeltételek
A következő eszközök telepítésére van szükség.

- [Visual Studio 2015 Update 2](https://go.microsoft.com/fwlink/?LinkId=691978)
- Telepítse [a Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET-Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)

Tárolók Docker futtatásához helyi meghajtóra, szüksége lesz egy helyi docker ügyfél.
Az engedélyezett [Docker eszközkészlet](https://www.docker.com/products/overview#/docker_toolbox) lehet letiltani a Hyper-V igénylő is használhatja, vagy használhatja [A Windows béta Docker](https://beta.docker.com) , amely a Hyper-V használ, és a használatához a Windows 10-es.

Docker eszközkészlet használata esetén szüksége lesz [az Docker ügyfél](./vs-azure-tools-docker-setup.md) konfigurálása

## <a name="1-create-a-web-app"></a>1. a webalkalmazás létrehozása

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. a Docker támogatás hozzáadása

[AZURE.INCLUDE [Add docker support](../includes/vs-azure-tools-docker-add-docker-support.md)]


## <a name="3-edit-your-code-and-refresh"></a>3. a saját kód és a frissítési szerkesztése

Módosítások gyorsan találta, indítsa el az alkalmazást a tárolóban lévő, és továbbra is módosításához, mint az IIS Express a Megtekintés.

1. A megoldás konfiguráció beállítása `Debug` , és nyomja le az ENTER ** &lt;CTRL + F5 >** a docker kép készítése és helyben futtatni.

    Miután a tároló kép épített és Docker tároló fut, a Visual Studio elindítja a Web app, az alapértelmezett böngészőben.
    Ha a Microsoft Edge böngésző használja, vagy más módon van a hibák, lásd: [Hibaelhárítás](vs-azure-tools-docker-troubleshooting-docker-errors.md) című szakaszt.

1. Nyissa meg a névjegy lapot, amely hol megyünk végezze el a módosításokat.

1. Térjen vissza a Visual Studióban, és nyissa meg a `Views\Home\About.cshtml`.

1. A következő HTML-tartalom hozzáadása a végén a fájlt, majd mentse a módosításokat.

    ```
    <h1>Hello from a Docker Container!</h1>
    ```

1.  Megtekintés, a kimeneti ablakban, a .NET-összeállítás befejeződött, és ezek vonalak, lépjen vissza a böngészőben, és frissítse a névjegy lapot.

    ```
    Now listening on: http://*:80
    Application started. Press Ctrl+C to shut down
    ```

1.  A módosítások alkalmazott!

## <a name="4-debug-with-breakpoints"></a>4. a hibakereséshez töréspontok

Gyakran módosítások szüksége lesz további ellenőrzés feljebb helyezése a Visual Studio hibakeresési funkcióját.

1.  Térjen vissza a Visual Studio és megnyitása`Controllers\HomeController.cs`

1.  A About() módszer tartalmának lecserélése a következőre:

    ```
    string message = "Your application description page from wthin a Container";
    ViewData["Message"] = message;
    ````

1.  Töréspont beállítása bal oldalán a `string message`... sor.

1.  Találati ** &lt;F5 >** hibakeresés.

1.  Nyissa meg azt a találatok a Töréspont a névjegy lapot.

1.  Váltás a Visual Studio megtekintése a töréspont, és nézze meg az üzenetet értékét.

    ![][2]

##<a name="summary"></a>Összefoglalás

[Visual Studio 2015 eszközök Docker](https://aka.ms/DockerToolsForVS), amely a hatékonyság a munkát a termelési létrehozásáról a Docker tároló belül fejlesztését helyi meghajtóra, elérheti.

## <a name="troubleshooting"></a>Hibaelhárítás

[Visual Studio Docker fejlesztési – hibaelhárítás](vs-azure-tools-docker-troubleshooting-docker-errors.md)

## <a name="more-about-docker-with-visual-studio-windows-and-azure"></a>További információ a Visual Studióban, a Windows és az Azure Docker

- [Docker Tools for Visual Studio](http://aka.ms/dockertoolsforvs) - tárolóban a .NET Core kód kialakítása
- [Docker Tools for Visual Studio Team Services](http://aka.ms/dockertoolsforvsts) - létre és helyezhetnek üzembe a docker tárolók
- [Docker Tools for Visual Studio Code](http://aka.ms/dockertoolsforvscode) - nyelvi szolgáltatásokat docker fájlokat, a további e2e esetek lesz szerkesztésre
- [Információ a Windows tároló](http://aka.ms/containers)- Windows Server és a Nano Kiszolgálóadatok
- [Azure tároló szolgáltatás](https://azure.microsoft.com/services/container-service/) - [Azure tároló szolgáltatás tartalom](http://aka.ms/AzureContainerService)
-    További példák Docker való használatáról című témakörben talál [Docker használata](https://github.com/Microsoft/HealthClinic.biz/wiki/Working-with-Docker) a [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 csatlakozás. További QuickStarts HealthClinic.biz bemutatója a csomagban a [Azure fejlesztői eszközökkel QuickStarts csomagban](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts)című témakör tartalmaz.

## <a name="various-docker-tools"></a>Különböző Docker eszközök

[Néhány nagyszerű docker eszköz (Tóth Lasker blogjába)](https://blogs.msdn.microsoft.com/stevelasker/2016/03/25/some-great-docker-tools/)

## <a name="good-articles"></a>Jó cikkek

[A NGINX Microservices – bevezetés](https://www.nginx.com/blog/introduction-to-microservices/)

## <a name="presentations"></a>Bemutatók

- [Tóth Lasker: VIEWBEN Live Veszprém 2016 – Docker e2e](https://github.com/SteveLasker/Presentations/blob/master/VSLive2016/Vegas/)
- [ASP.NET Core – bevezetés @ 2016 – hol meg a bemutató készítése](https://channel9.msdn.com/Events/Build/2016/B810)
- [Tárolók, csatorna 9 a .NET-alkalmazások fejlesztése](https://blogs.msdn.microsoft.com/stevelasker/2016/02/19/developing-asp-net-apps-in-docker-containers/)

[2]: ./media/vs-azure-tools-docker-edit-and-refresh/breakpoint.png
