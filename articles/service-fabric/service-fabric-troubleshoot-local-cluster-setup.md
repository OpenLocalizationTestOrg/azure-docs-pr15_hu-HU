<properties
   pageTitle="A helyi szolgáltatás háló fürt telepítő hibaelhárítása |} Microsoft Azure"
   description="Ez a cikk bemutatja a helyi fejlesztési fürt hibaelhárítási javaslatokkal csoportja"
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/08/2016"
   ms.author="seanmck"/>

# <a name="troubleshoot-your-local-development-cluster-setup"></a>A helyi fejlesztési fürt telepítő hibaelhárítása

A helyi Azure Service háló fejlesztési fürthöz használata közben belefutna egy problémába, olvassa el az alábbi javaslatokat lehetséges megoldásairól.

## <a name="cluster-setup-failures"></a>Fürt telepítési hiba

### <a name="cannot-clean-up-service-fabric-logs"></a>Nem lehet szolgáltatás háló naplók karbantartása

#### <a name="problem"></a>A probléma

A DevClusterSetup parancsfájl futtatásakor ehhez hasonló hibaüzenet jelenik meg:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Megoldás

Az aktuális PowerShell-ablak bezárása és rendszergazdaként PowerShell új ablak megnyitása. Most már láthatja a parancsfájl sikeres futtatásához.

## <a name="cluster-connection-failures"></a>Fürt csatlakozási hibák

### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Szolgáltatás háló PowerShell-parancsmagok nem ismeri fel az Azure PowerShell

#### <a name="problem"></a>A probléma

Ha megpróbál futtatni bármelyik a szolgáltatás háló PowerShell-parancsmagok, például: `Connect-ServiceFabricCluster` az Azure PowerShell ablakban sikertelen, közli, hogy a parancsmag nem ismerhető fel. Ennek oka az, hogy Azure PowerShell a 32 bites verzióját használja, a Windows PowerShell (még akkor is a 64 bites operációs rendszer verziója), mivel a szolgáltatás háló parancsmagok csak a 64 bites környezetben működik.

#### <a name="solution"></a>Megoldás

Mindig közvetlenül a Windows PowerShell futtassa a szolgáltatás háló parancsmagok.

>[AZURE.NOTE] A legújabb Azure PowerShell létrehozása speciális parancsikont, így a már nem történjen.

### <a name="type-initialization-exception"></a>Írja be a inicializálni kivétel

#### <a name="problem"></a>A probléma

Amikor csatlakozik a PowerShellben a fürthöz, hibaüzenet jelenik meg TypeInitializationException System.Fabric.Common.AppTrace számára.

#### <a name="solution"></a>Megoldás

A path változó nem helyesen állította be a telepítés során. Kijelentkezés a Windows, és jelentkezzen be újra. Ez az elérési út teljesen frissíthetők.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Fürt kapcsolat megszakad az "Objektum zárva"

#### <a name="problem"></a>A probléma

Csatlakozás-ServiceFabricCluster hívásakor sikertelen, a hibaüzenet, jelennek meg:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Megoldás

Az aktuális PowerShell-ablak bezárása és rendszergazdaként PowerShell új ablak megnyitása. Most már láthatja, hogy tud csatlakozni.

### <a name="fabric-connection-denied-exception"></a>Háló kapcsolat megtagadva kivétel

#### <a name="problem"></a>A probléma

Ha a hibakeresése során a Visual Studióban, akkor a FabricConnectionDeniedException hiba.

#### <a name="solution"></a>Megoldás

Ez a hiba általában akkor történik, amikor, próbálja meg a kísérlet az indítsa el a szolgáltatás host folyamat manuálisan, engedélyezése a szolgáltatás háló futtatókörnyezet elindítani az Ön helyett.

Győződjön meg arról, hogy nincs bármely szolgáltatás projektek értéke lehet indítási projekteket a megoldást. Csak a szolgáltatási háló alkalmazás projektek indítási projektként kell állítani.

>[AZURE.TIP] -E, a telepítő, a rendkívül viselkedjen megkezdi a helyi fürtre, visszaállíthatja a helyi fürtre manager rendszer tálca alkalmazás segítségével. Ez a meglévő fürt eltávolítása, és állítsa be egy új. Felhívjuk a figyelmét arra, hogy az összes telepített alkalmazások és a kapcsolódó adatok törlődnek.

## <a name="next-steps"></a>Következő lépések

- [Dátumtáblázatok ismertetése és a rendszer állapotjelentések fürtre – problémamegoldás](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
- [A szolgáltatás háló Intézővel fürt ábrázolása](service-fabric-visualizing-your-cluster.md)
