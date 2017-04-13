
<properties
    pageTitle="RemoteApp felhő gyűjtemények - létrehozási elhárítása |} Microsoft Azure"
    description="Megtudhatja, hogy miként RemoteApp felhő webhelycsoport létrehozása hibák elhárítása"
    services="remoteapp"
    documentationCenter=""
    authors="vkbucha"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# <a name="troubleshoot-creating-remoteapp-cloud-collections"></a>Létrehozás RemoteApp felhő gyűjtemények – problémamegoldás

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Ha problémába ütközik a felhőben webhelycsoport létrehozása, olvassa el az alábbi információkat.

## <a name="your-image-is-invalid"></a>A kép érvénytelen. ##
Ha megjelenik a következőhöz hasonló "GoldImageInvalid" üzenet, amikor Ön várakozik Azure kiépítése a webhelycsoport, az azt jelenti, hogy a sablon képe nem felel meg a [kép követelmények definiált](remoteapp-imagereqs.md). Igen lépjen, olvassa el az adott [követelményeket](remoteapp-imagereqs.md), javítása a képet, és próbálja meg ismét a webhelycsoport létrehozása.

## <a name="common-errors-seen-in-the-azure-management-portal"></a>A gyakori hibák láthatók az Azure adatkezelési portálon

    DNS server could not be reached
    ProvisioningTimeout

Gyakran cloud webhelycsoportok létrehozása, mert során fellépő hibák használ egyéni képek.  Ha megjelenik a fenti hibák egyike, és a saját kép létrehozása a webhelycsoport szolgáltatást használ, ellenőrizze az alábbiakat:

- Győződjön meg arról, hogy a saját kép feltöltött megfelel képe.
- Leggyakrabban a közös probléma, hogy a kép nem megfelelően Sysprep segédprogrammal előkészített.  
- Ellenőrizze a képet, indítsa el a Hyper-V belül, vagy próbálkozzon egy IAAS virtuális létrehozása közvetlenül az Azure előfizetés a kép felhasználásával. A virtuális nem sikerül indítsa el, és indul el, ha majd ez általában azt jelzi, hogy a a saját kép előkészítése nem lett megfelelően.  Ellenőrizze, hogy a saját kép készült követő, hogyan hozhat létre egy egyéni sablont képe a RemoteApp

Ha a Microsoft képek az előfizetéshez tartozó egyikét használja, próbálja meg ismét a webhelycsoport létrehozása. Ha a probléma nem szűnik meg majd lépjen kapcsolatba a Microsoft támogatási.

    PlatformImageTrialModeOnly

Ha ez a hibaüzenet jelenik meg ez általában azt jelenti, frissített fizetett fiókhoz, de egy Microsoft által biztosított képet, amely csak a szolgáltatás próba üzemmódban van érvényes próbál. Ebben az esetben próbálkozzon újra a felhőben webhelycsoport létrehozása, de ügyeljen arra, hogy a helyes kép adja meg.
