<properties
   pageTitle="Egy ASP.NET Core Linux Docker tároló távoli Docker állomáshoz telepítése |} Microsoft Azure"
   description="Megtudhatja, hogy miként használatával Visual Studio Tools for Docker az ASP.NET Core web app-Azure Docker Host Linux virtuális futó Docker tárolóhoz"   
   services="azure-container-service"
   documentationCenter=".net"
   authors="mlearned"
   manager="douge"
   editor=""/>

<tags
   ms.service="azure-container-service"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/08/2016"
   ms.author="mlearned"/>

# <a name="deploy-an-aspnet-container-to-a-remote-docker-host"></a>Egy ASP.NET-tároló távoli Docker állomáshoz telepítése

## <a name="overview"></a>– Áttekintés
Docker egy könnyű tároló motor hasonló néhány tipp, amellyel egy virtuális számítógépre, amelyen a host-alkalmazások és szolgáltatások is használhatja.
Ebben az oktatóanyagban végigvezeti a [Visual Studio 2015 Tools Docker](http://aka.ms/DockerToolsForVS) bővítmény használatával a PowerShell használatá Azure Docker állomáson ASP.NET Core alkalmazás telepítéshez használni.

## <a name="prerequisites"></a>Előfeltételek
A következő befejezéséhez szükséges ebben az oktatóanyagban:

- Hozzon létre egy Azure Docker Host virtuális, [Azure docker készülék használata](./virtual-machines/virtual-machines-linux-docker-machine.md) leírtak alapján.
- Telepítse [a Visual Studio 2015 3](https://go.microsoft.com/fwlink/?LinkId=691129)
- [Microsoft ASP.NET-Core 1.0 SDK](https://go.microsoft.com/fwlink/?LinkID=809122)
- [Visual Studio 2015 Tools Docker – előzetes verzió](http://aka.ms/DockerToolsForVS) telepítése

## <a name="1-create-an-aspnet-core-web-app"></a>1. Hozzon létre egy ASP.NET Core web App alkalmazásban
Az alábbi lépésekkel végigvezeti Önt egy egyszerű, ebben az oktatóanyagban használt ASP.NET Core alkalmazások létrehozását.

[AZURE.INCLUDE [create-aspnet5-app](../includes/create-aspnet5-app.md)]

## <a name="2-add-docker-support"></a>2. a Docker támogatás hozzáadása

[AZURE.INCLUDE [create-aspnet5-app](../includes/vs-azure-tools-docker-add-docker-support.md)]

## <a name="3-use-the-dockertaskps1-powershell-script"></a>3. a DockerTask.ps1 PowerShell-parancsprogramot használata 

1.  Nyissa meg a legfelső szintű címtárhoz a projekt egy PowerShell-parancssorában. 

    ```
    PS C:\Src\WebApplication1>
    ```

1.  A távoli érvényesítése host futásának. Meg kell jelennie állapot = futtatása 

    ```
    docker-machine ls
    NAME         ACTIVE   DRIVER   STATE     URL                        SWARM   DOCKER    ERRORS
    MyDockerHost -        azure    Running   tcp://xxx.xxx.xxx.xxx:2376         v1.10.3
    ```

    > [AZURE.NOTE] Ha a Docker béta használ, a szolgáltató nem szerepel.

1.  Az alkalmazás használatával generál a - összeállítás paraméter

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release -Machine mydockerhost
    ```  

    > [AZURE.NOTE] Ha a Docker béta használ, hagyja az - gépi argumentum
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Build -Environment Release 
    > ```  


1.  Futtassa az alkalmazást, használja a - Futtatás paraméter

    ```
    PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release -Machine mydockerhost
    ```

    > [AZURE.NOTE] Ha a Docker béta használ, hagyja az - gépi argumentum
    > 
    > ```
    > PS C:\Src\WebApplication1> .\Docker\DockerTask.ps1 -Run -Environment Release 
    > ```

    Ha befejeződött a docker, az alábbihoz hasonló eredményt kell megjelennie:

    ![Az alkalmazás megtekintése][3]

[0]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/docker-props-in-solution-explorer.png
[1]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/change-docker-machine-name.png
[2]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/launch-application.png
[3]:./media/vs-azure-tools-docker-hosting-web-apps-in-docker/view-application.png
