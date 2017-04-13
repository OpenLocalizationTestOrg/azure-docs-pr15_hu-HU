<properties
    pageTitle="Azure diagnosztika áttekintése |} Microsoft Azure"
    description="Azure diagnosztika használható hibakeresése során, mérési teljesítményét, nyomon követése, a felhőszolgáltatások, a virtuális gépeken futó és a szolgáltatás háló forgalom vizsgálata"
    services="multiple"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/02/2016"
    ms.author="robb"/>


# <a name="what-is-microsoft-azure-diagnostics"></a>Mi az Microsoft Azure diagnosztika


Azure diagnosztika, amely lehetővé teszi, hogy a telepített alkalmazások diagnosztikai adatok gyűjtése Azure belül a lehetőség. A diagnosztikai bővítmény számos különböző forrásból is használhatja. Jelenleg támogatott pedig Azure felhőalapú szolgáltatás webes dolgozó szerepkörök, Azure virtuális gépeken futó operációs rendszert futtató Microsoft Windows és a szolgáltatás háló. Egyéb Azure szolgáltatás saját külön diagnosztika van.

## <a name="data-you-can-collect"></a>Adatok összegyűjtheti

Azure diagnosztika összegyűjtheti az alábbi típusú adatokat:

Adatforrás|Leírás
---|---
Teljesítmény számláló | Operációs rendszer és egyéni teljesítmény számláló
Alkalmazás naplók     | Az alkalmazás által írt üzenetek nyomon követése
Windows-eseménynaplók   | A Windows esemény naplózás rendszer küldött adatok
.NET-forrás    | Kód segítségével a.NET [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) osztály esemény írása
IIS naplók             | IIS-webhelyek kapcsolatos tudnivalók
Jegyzék alapú esemény-nyomkövetés   | Esemény-nyomkövetés a Windows események folyamatának által generált
Kiírása összeomlik          | Az alkalmazás összeomlik megelőzve a folyamat állapota kapcsolatos tudnivalók
Egyéni hibanaplók    | Az alkalmazás vagy szolgáltatás által létrehozott naplók
Azure diagnosztikai infrastruktúra naplók|Diagnosztikai magát kapcsolatos tudnivalók

Az Azure diagnosztika kiterjesztést is az adatok átvitele Azure tárterület-fiókkal, illetve elküldheti szolgáltatásai, például az [Alkalmazás az összefüggéseket](./application-insights/app-insights-cloudservices.md). Az adatok hibakeresési és hibaelhárítási, teljesítmény mérése, az Erőforrás kihasználtsága, forgalom elemzés és tervezés kapacitás nyomon követéséhez és naplózás is használhatja.


## <a name="versioning"></a>Verziószámozás
Lásd: [Azure diagnosztika korábbi verziói](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Következő lépések
Válassza a mely diagnosztika összegyűjtheti és a kezdéshez kövesse az alábbi cikkekben próbál szolgáltatás. Az általános Azure diagnosztika hivatkozásokkal hivatkozások az adott tevékenységhez.

## <a name="web-apps"></a>Web Apps alkalmazások
Figyelje meg, hogy Web Apps alkalmazások nem használja az Azure diagnosztika. Az azonos információk megkeresése a [Web Apps alkalmazások](./app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Azure diagnosztika segítségével cloud Services
- Ha használja a Visual Studióban, olvassa el a [Nyomon követése a Cloud Services-alkalmazások használata a Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) a kezdéshez. Minden egyéb esetben pedig lásd:
- [Azure diagnosztika segítségével Cloud services figyelése](./cloud-services/cloud-services-how-to-monitor.md)
- [Azure diagnosztika Cloud Services alkalmazásban beállítása](./cloud-services/cloud-services-dotnet-diagnostics.md)

Speciális témaköröket lásd:

- [Alkalmazás háttérismeretek Cloud Services-Azure diagnosztika használata](./application-insights/app-insights-cloudservices.md)
- [A folyamat az Azure diagnosztika Cloud Services alkalmazás nyomon követése](./cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
- [A Cloud Services diagnosztikai beállítása a PowerShell használatával](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)


## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuális gépeken futó Azure diagnosztika segítségével
- Visual Studio használata esetén lásd: [Használja a Visual Studio követéshez Azure virtuális gépeken futó](./vs-azure-tools-debug-cloud-services-virtual-machines.md) kezdéshez. Minden egyéb esetben pedig lásd:
- [Azure diagnosztika az Azure virtuális gép beállítása](./virtual-machines-dotnet-diagnostics.md)

Speciális témaköröket lásd:

- [A Azure virtuális gépeken futó diagnosztika beállítása a PowerShell használatával](./virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md)
- [A figyelés és Azure erőforrás-kezelő sablonnal diagnosztika Windows virtuális gép létrehozása](./virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md)

## <a name="service-fabric-using-azure-diagnostics"></a>Azure diagnosztika használ szolgáltatási háló
Használatának első lépései [Monitor egy szolgáltatás háló alkalmazást](./service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). További szolgáltatás háló diagnosztika sok cikkek a bal oldali navigációs fában, miután jelenik meg ez a cikk.

## <a name="general-azure-diagnostics-articles"></a>Általános Azure diagnosztika cikkek
- [Azure diagnosztika séma konfiguráció](https://msdn.microsoft.com/library/azure/mt634524.aspx) – ismerje meg, hogyan módosíthatja a sémafájl összegyűjtése és diagnosztikai adatok irányítja. Figyelje meg, hogy akkor is használhatja a Visual Studio a séma fájl módosításához.
- [Hogyan Azure diagnosztika az Azure-tárolóban lévő adatok vannak tárolva](./cloud-services/cloud-services-dotnet-diagnostics-storage.md) –, hogy a neveket a táblák és BLOB, ahol a diagnosztikai adatok íródott.
- Olvassa el a [Teljesítmény számláló az Azure diagnosztika](./cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
- Ismerje meg, hogyan [útvonal Azure diagnosztikai információ alkalmazás mélyebb](./azure-diagnostics-configure-applicationinsights.md)
- Ha problémát tapasztal a diagnosztika indítása és az adatok keresése az Azure tárolás táblázatok, olvassa el a [Azure diagnosztika – hibaelhárítás](./azure-diagnostics-troubleshooting.md)
