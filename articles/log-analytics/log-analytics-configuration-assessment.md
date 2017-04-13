<properties
    pageTitle="Konfigurációs értékelési megoldás napló Analytics |} Microsoft Azure"
    description="A konfiguráció értékelési megoldást napló Analytics biztosít a System Center Operations Manager server infrastruktúra aktuális állapotával kapcsolatos részletes információt Operations Manager ügynökök vagy Operations Manager kezelése csoport használata esetén."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="configuration-assessment-solution-in-log-analytics"></a>Konfigurációs értékelési megoldás napló Analytics

A konfiguráció értékelési megoldást napló Analytics segít értesítések és a Tudásbázis javaslatok potenciális kiszolgáló konfigurációs problémák keresése a.

Ez a megoldás System Center Operations Manager szükséges. Konfigurációs értékelési nem érhető el, ha csak közvetlenül csatlakozó ügynökök.

A böngészőben a Silverlight bővítményt információk megtekintése a konfigurációs értékelési megoldás van szükség.

>[AZURE.NOTE] Július 5, 2016-ban, a konfigurációs értékelési megoldást már nem bővíthető napló Analytics-munkaterületek vagy a megoldás többé nem érhető el a meglévő felhasználóknak 2016 augusztus 1 után. Ezzel a megoldással SQL Server vagy az Active Directory-ügyfeleknek azt javasoljuk az [SQL Server értékelési](log-analytics-sql-assessment.md), [Az Active Directory felmérése](log-analytics-ad-assessment.md) és [Az Active Directory replikációs állapotát](log-analytics-ad-replication-status.md) megoldások használja. Az ügyfeleknek konfigurációs értékelési használatával a Windows a Hyper-V, és a System Center virtuális gép Manager, azt javasoljuk, hogy az esemény gyűjtemény használható, és a követési funkciókat megpróbálhatnak kapcsolatos problémák megoldásához belül a környezetek holisztikus nézetének módosítása.

![Konfigurációs értékelési csempe](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Konfigurációs adatok felügyelt kiszolgálók gyűjtött, és elküldi a MOBILE szolgáltatás feldolgozásra a felhőben. Logika a kapott adatokon alkalmazott, és a felhőbeli szolgáltatástól rekordok az adatokat. Az alábbi kérdésekre kiszolgálók feldolgozott adatok jelennek meg:

- **Értesítések:** A felügyelt kiszolgálók felvetett beállításaival kapcsolatos, a megelőző értesítések megjelenítése Ezek darab termék készült Microsoft Customer és támogatási szervezeten (CSS) ajánlott eljárások a mező által létrehozott szabályokat.
- **Tudásbázis javaslatok:** A Microsoft Tudásbázis projektvezetési cikkeket talál, amelyek szerepelnek a infrastruktúrát; munkaterhelésekből javasoltak mutatja Ezek a program automatikusan ajánl Ön konfigurációjának-használata a gépi tanulási.
- **Kiszolgálók és elemzett Munkaterhelésekből:** Kiszolgálók és munkaterhelésekből, amelyek felügyeli MOBILE jeleníti meg.

![Konfigurációs értékelési irányítópult](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### <a name="technologies-you-can-analyze-with-configuration-assessment"></a>Konfigurációs értékelési elemezheti technológiák

MOBILE konfigurációs értékelési elemzi a következő feladatok:

- A Windows Server 2012 és a Microsoft a Hyper-V Server 2012
- Windows Server 2008 és Windows Server 2008 R2, például:
    - Az Active Directory
    - A Hyper-V host
    - Általános operációs rendszer
- Az SQL Server 2008 vagy újabb verzió
    - Az SQL Server adatbázismotort
- Microsoft SharePoint 2010-ben
- Microsoft Exchange Server 2010 és a Microsoft Exchange Server 2013-ban
- A Microsoft Lync Server 2013 és a Lync Server 2010-ben
- System Center 2012 SP1 – virtuális gép Manager

Az SQL Server analysis 32 bites és 64 bites kiadásában támogatott:

- Az SQL Server 2016 – kiadásainak
- Az SQL Server - 2014-es kiadásainak
- Az SQL Server 2008, 2008 R2 - és kiadásainak

Az SQL Server adatbázismotor elemzése összes támogatott verzióján van. Ezenkívül a 32 bites verzió az SQL Server esetén támogatott operációs rendszert futtató WOW64 végrehajtása.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- Operations Manager szükség a konfigurációs értékelési megoldás.
- Minden olyan számítógépen, amelyre konfigurációját felmérése Operations Manager ügynökszoftvert kell rendelkeznie.
- A konfiguráció értékelési megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.

## <a name="configuration-assessment-data-collection-details"></a>Konfigurációs értékelési adatokat webhelycsoport részletei

Konfigurációs értékelési gyűjti össze a konfigurációs adatok, metaadatok és állapot adatok használata az ügynökök, hogy engedélyezve van.

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés konfigurációs felméréshez más adatait.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![nem](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![igen](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![nem](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|            ![igen](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![igen](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| kétszer naponként|

A következő táblázat mutatja az adattípusok konfigurációs szolgáltatás által gyűjtött példákat:

|**Adattípus**|**Mezők**|
|---|---|
|Konfiguráció|CustomerID, AgentID, entityid beállítást, ManagedTypeID, ManagedTypePropertyID, a CurrentValue, ChangeDate|
|Metaadatok|BaseManagedEntityId, ObjectStatus, szervezeti_egység, ActiveDirectoryObjectSid, PhysicalProcessors, hálózatnév, IP-cím, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-címet, NetbiosDomainName, LogicalProcessors, Dns_név, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Állam|StateChangeEventId, és a StateId, NewHealthState, OldHealthState, környezetben, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, módosítás dátuma, LastGreenAlertGenerated, DatabaseTimeModified|

## <a name="configuration-assessment-alerts"></a>Konfigurációs értékelési értesítések
Megtekintheti és értesítések kezelése a konfigurációs értékelése az értesítések lappal. Értesítések mondani, hogy az észlelt a problémát, a probléma okát és a probléma megoldása. Is információkkal szolgálnak a környezetben, amelyek teljesítménnyel kapcsolatos problémákat okozhatnak beállításokat.

![értesítések megjelenítése](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] A konfigurációs értékelési figyelmeztetések a napló Analytics más riasztást eltérnek. A böngészőben megtekintése, értesítések igényel egy Silverlight bővítményt.

Amikor kijelöl egy elem vagy az értesítések oldalon lévő elemek kategória, látni fogja a kiszolgálókon vagy az egyes elemekre vonatkozó értesítések munkaterhelésekből listáját.

![riasztási lista](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Minden riasztás tartalmaz egy hivatkozást a cikk a Microsoft Tudásbázis. Ezek a cikkek az értesítésre kapcsolatos további információkat.

>[AZURE.TIP] A megjelenő riasztások maximális száma alapértelmezés szerint ki 2000. További riasztások megtekintéséhez kattintson az értesítési sávon a riasztások listája felett.

Bármely elemre a listában a Tudásbázis-cikk, amely segíthet a probléma okát, az értesítést generáló a probléma megoldásához megtekintéséhez.

![Tudásbáziscikk](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Riasztási szabályokat figyelmen kívül hagyja a szabályok vagy egy osztályok szabályok kezelése

![a figyelmeztetési szabályok kezelése](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## <a name="knowledge-recommendations"></a>Tudásbázis javaslatok
Amikor Tudásbázis javaslatok, Microsoft KB cikkek ajánlott a munkaterhelésének és számítógépek, a figyelmeztető kapcsolatos további információkat a bejegyzésére napló találatok esetén jelenik meg.

![Tudásbázis javaslatok keresésére kapott találatok](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## <a name="servers-and-workloads-analyzed"></a>Kiszolgálók és elemzett munkaterhelésekből
Amikor Tudásbázis javaslatok, bejegyzésére a kiszolgálókat és munkaterhelésekből, amely az Operations Manager MOBILE ismert napló találatok esetén jelenik meg.

![Kiszolgálók és feladatok](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## <a name="next-steps"></a>Következő lépések

- Használja a [napló keresések napló Analytics](log-analytics-log-searches.md) részletes konfigurációs értékelési adatokat szeretne megtekinteni.
