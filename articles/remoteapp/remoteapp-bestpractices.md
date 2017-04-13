<properties
    pageTitle="Azure RemoteApp gyakorlati tanácsokat |} Microsoft Azure"
    description="Gyakorlati tanácsok a konfigurálása és Azure RemoteApp használata."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Gyakorlati tanácsok a konfigurálása és Azure RemoteApp használata

> [AZURE.IMPORTANT]
> Azure RemoteApp megszakad alatt. Olvassa el a további információt a [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) .

Az alábbi információk segíthetnek beállítása és használata az Azure RemoteApp megtartják.

## <a name="connectivity"></a>Kapcsolat


- Mindig a legújabb ügyfél verzióját használja. Csatlakozási problémákat tapasztal, és más csökkentett élményt okozhat a régebbi ügyfelek használatával. Az eszköz az alkalmazás automatikus frissítések engedélyezése biztosítására, hogy a legutóbbi ügyfélprogram mindig telepítve van.
- Mindig használja a stabil és a megbízható internetkapcsolat elérhető.  
- Csak a támogatott proxyn a csatlakozási optimális teljesítmény elérése érdekében.  A SOCKS proxy nem támogatott.

## <a name="applications"></a>Alkalmazások


- Mentse és zárja be a távoli alkalmazás formájában használva alkalmazást, ha végzett az alkalmazással. Az adatok elvesztését eredményezhet nem bezárja az alkalmazást.
- Egyéni alkalmazások érvényesítés előtt Azure RemoteApp használni őket. Ide tartoznak a több elem munkamenet platformon működik, és nem felhasználni a szükségtelen erőforrások, például a memória és Processzor, amely egy másik felhasználó ugyanazt a webhelycsoportban előfordulhat, hogy starve biztosítása. Információ töltse le, és tekintse át az [Alkalmazás kompatibilitási gyakorlati tanácsok a távoli asztali szolgáltatásokat](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfigurálása és kezelése


- A webhelysablonok képét naprakészen tartása, telepítése, szoftverfrissítések és egyéb fontos javításokat szükség szerint. Ezzel biztosíthatja, hogy szerint Azure RemoteApp automatikus-méretarányok közül való felel meg a beosztását, minden egyes példány telepítve.  
- Legyen az Active Directory összevonási szolgáltatások (AD FS) telepítési biztonságos és megbízható. Egyéb esetben ügyfél hitelesítési előfordulhat, hogy nem, megakadályozza, hogy a felhasználók Azure RemoteApp elérése.
- Állítsa be a sablon képeket a telepített alkalmazások, a szerepkörök vagy a szolgáltatások, hogy az állapot nélküli. Nem azok kell támaszkodhat bármely állandó állapotban éppen egy RemoteApp szolgáltatásban a virtuális gépeken futó példányát.
    - A felhasználói profilok és más tárolási helye a szolgáltatást, hogy a külső minden felhasználói adat tárolni, például a helyszíni fájl megosztások vagy a onedrive-on.
    - Tár megosztott tárolási helye a szolgáltatást, hogy a külső adatok, például a helyszíni fájl megosztások vagy OneDrive.
    - A sablon kép helyett egy szolgáltatásból egyes virtuális gépeken bármely rendszer szintű beállításainak konfigurálása
    - Tiltsa le a közzétett alkalmazások automatikus frissítéseket - inkább manuálisan alkalmazza őket a sablon képe és tesztelni őket, mielőtt beállítaná a sablonból.
