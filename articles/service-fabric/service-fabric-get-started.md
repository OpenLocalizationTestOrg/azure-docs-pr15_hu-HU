<properties
   pageTitle="A fejlesztői környezet beállítása |} Microsoft Azure"
   description="A futási idejű, SDK és eszközök telepítése, és hozzon létre egy helyi fejlesztési fürt. Ez a beállítás végeztével készen áll a készíthet alkalmazásokat fogja."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/26/2016"
   ms.author="ryanwi"/>

# <a name="prepare-your-development-environment"></a>Felkészülés a fejlesztői környezet

> [AZURE.SELECTOR]
-[A Windows](service-fabric-get-started.md)
- [Linux](service-fabric-get-started-linux.md)
- [OSX](service-fabric-get-started-mac.md)

 Szerkesztés és [Azure Service háló alkalmazások] futtatásához[ 1] fejlesztési számítógépre telepítése a futtatókörnyezet SDK és eszközök. Akkor is engedélyeznie kell a SDK szerepel a Windows PowerShell-parancsfájlokat végrehajtását.

## <a name="prerequisites"></a>Előfeltételek
### <a name="supported-operating-system-versions"></a>Támogatott operációs rendszerek
Az alábbi operációs rendszer verziója támogatott fejlesztői:

- Windows 7 rendszerben
- Windows 8/Windows 8.1
- A Windows Server 2012 R2
- Windows 10-ben

>[AZURE.NOTE] Windows 7 alapértelmezés szerint a Windows PowerShell 2.0 csak tartalmazza. Szolgáltatás háló PowerShell-parancsmagok PowerShell 3.0-s vagy újabb szükséges. [Töltse le a Windows PowerShell 5.0] is[ powershell5-download] Microsoft Download Center.

## <a name="install-the-runtime-sdk-and-tools"></a>A futási idejű, SDK és eszközök telepítése

A webes Platform Installer szolgáltatás háló fejlesztési két konfigurációk kínál:

- [A szolgáltatás háló futtatókörnyezet SDK és eszközök telepítése (a Visual Studio 2015 frissítés 2-es vagy újabb verzió szükséges) a Visual Studio 2015][full-bundle-vs2015]
- [Telepítse a Service háló futtatókörnyezet és SDK csak (nincs Visual Studio eszközök)][core-sdk]

## <a name="enable-powershell-script-execution"></a>Végrehajtása a PowerShell parancsprogramot engedélyezése

Helyi fejlesztési fürt létrehozásához és üzembe helyezné a Visual Studio alkalmazások szolgáltatás háló használja a Windows PowerShell-parancsfájlokat. Alapértelmezés szerint a Windows blokkolja a parancsfájlok futtatásának. Engedélyezi őket, hogy módosítania kell a PowerShell végrehajtási irányelvek. Nyissa meg a PowerShell rendszergazdaként, és írja be a következő parancsot:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
```

## <a name="next-steps"></a>Következő lépések
Most, hogy befejezte a beállítás be a fejlesztői környezet, indítsa el a összeállítását, és futtasson alkalmazásokat.

- [A Visual Studióban az első szolgáltatás háló-alkalmazás létrehozása](service-fabric-create-your-first-application-in-visual-studio.md)
- [Megtudhatja, hogy miként telepítheti és a helyi fürt alkalmazások kezelése](service-fabric-get-started-with-a-local-cluster.md)
- [Tudnivalók a programozási modellek: megbízható szolgáltatásokat és a megbízható szereplők](service-fabric-choose-framework.md)
- [Nézze meg a szolgáltatás háló mintakódok a GitHub](https://aka.ms/servicefabricsamples)
- [Jelenítse meg a fürt szolgáltatás háló Intézővel](service-fabric-visualizing-your-cluster.md)
- [A szolgáltatás háló tanulási Görbekövető a platform széles ismertetés](https://azure.microsoft.com/documentation/learning-paths/service-fabric/)

[1]: http://azure.microsoft.com/en-us/campaigns/service-fabric/ "Szolgáltatás háló marketingkampány lap"
[2]: http://go.microsoft.com/fwlink/?LinkId=517106 "VIEWBEN RC"
[full-bundle-vs2015]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-VS2015 "VIEWBEN 2015 WebPI hivatkozás"
[full-bundle-dev15]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-Dev15 "Dev15 WebPI hivatkozás"
[core-sdk]:http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK "Alapvető SDK WebPI hivatkozás"
[powershell5-download]:https://www.microsoft.com/en-us/download/details.aspx?id=50395
