<properties 
   pageTitle="Azure Cloud Services hibakeresési |} Microsoft Azure"
   description="Azure Cloud Services hibakeresése"
   services="visual-studio-online"
   documentationCenter="n/a"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="debugging-cloud-services"></a>Hibakeresési felhőszolgáltatások

Különböző módszerekkel is használhatja az Azure alkalmazás hibakeresése a Microsoft Visual Studio és az Azure SDK Azure eszközeivel:

- Is hibakeresése Visual Studio az Azure alkalmazások fejlesztéséhez, amikor ugyanúgy, mint bármely Visual C# vagy Visual Basic alkalmazásban. További tudnivalókért olvassa el a [hibakeresési a felhőalapú szolgáltatás, a helyi számítógépen](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer)című témakört.

- Azure diagnosztika segítségével részletes információk naplózása belül szerepkörök, futó kódból, hogy a szerepkörök futtatja a fejlesztői környezet vagy Azure-ban. További tudnivalókért olvassa el a [Collecting naplózás Azure diagnosztika segítségével adatokat](http://go.microsoft.com/fwlink/p/?LinkId=400450)című témakört.

- Írja be a .NET-keretrendszer 4 vagy a .NET-keretrendszer 4.5 célzott szerepkörök Visual Studio Enterprise szolgáltatást használ, ha IntelliTrace engedélyezheti, hogy a Visual Studio Azure felhő szolgáltatásainak üzembe időben. IntelliTrace tartalmaz, amely a Visual Studio segítségével a alkalmazás hibakeresése, mintha voltak rendszert futtató Azure naplót. További tudnivalókért lásd: a [hibakeresési IntelliTrace és a Visual Studio közzétett felhőalapú szolgáltatás]( http://go.microsoft.com/fwlink/p/?LinkId=623016).

- Lehetősége van engedélyezni a távoli hibakeresés a felhőalapú szolgáltatások amikor rendszerbe a felhőalapú szolgáltatást a Visual Studio időben. Ha úgy dönt, hogy engedélyezze a távoli hibakeresés telepítés, távoli hibakeresési szolgáltatás a virtuális gépeken futó minden szerepkör-példány telepítve. Az alábbi szolgáltatások, például msvsmon.exe, ne teljesítményét és többletköltségek eredményez. További tudnivalókért olvassa el a [hibakeresési Azure-ban egy felhőalapú szolgáltatásba](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure)című témakört.



