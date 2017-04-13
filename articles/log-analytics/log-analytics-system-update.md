<properties
    pageTitle="Rendszer frissítés értékelési megoldás napló Analytics |} Microsoft Azure"
    description="Használhatja a frissítéseket a rendszer megoldást napló Analytics segít a hiányzó frissítések telepítése a infrastruktúra kiszolgálókon."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="system-update-assessment-solution-in-log-analytics"></a>Rendszer frissítés értékelési megoldás napló Analytics

Használhatja a frissítéseket a rendszer megoldást napló Analytics segít a hiányzó frissítések telepítése a infrastruktúra kiszolgálókon. Miután telepítette a megoldást, megtekintheti, hogy a **Rendszer frissítés értékelési** csempére az **Áttekintés** lapon, a MOBILE használatával a megfigyelt kiszolgálókról hiányoznak a frissítések.

Ha hiányzó frissítések találhatók, a részletek a **frissítések** irányítópulton jelennek meg. A **frissítések** irányítópult használata hiányzik a frissítések és alkalmazni azokat a kiszolgálókat, szüksége van rájuk terv kidolgozása is használhatja.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- A rendszer frissítés értékelési megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.

## <a name="system-update-data-collection-details"></a>Rendszer frissítés Adatrészletek gyűjtemény

Rendszer frissítés értékelési gyűjti össze a metaadatok és állapot adatok használata az ügynökök, hogy engedélyezve van.

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés rendszer frissítés felméréshez más adatait.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![igen](./media/log-analytics-system-update/oms-bullet-green.png)|![igen](./media/log-analytics-system-update/oms-bullet-green.png)|![nem](./media/log-analytics-system-update/oms-bullet-red.png)|            ![nem](./media/log-analytics-system-update/oms-bullet-red.png)|![igen](./media/log-analytics-system-update/oms-bullet-green.png)| Legalább 2 és naponta és 15 percet frissítés telepítése után|

A következő táblázat mutatja a rendszer Update szolgáltatás által gyűjtött adattípusok példákat:

|**Adattípus**|**Mezők**|
|---|---|
|Metaadatok|BaseManagedEntityId, ObjectStatus, szervezeti_egység, ActiveDirectoryObjectSid, PhysicalProcessors, hálózatnév, IP-cím, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-címet, NetbiosDomainName, LogicalProcessors, Dns_név, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime|
|Állam|StateChangeEventId, és a StateId, NewHealthState, OldHealthState, környezetben, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, módosítás dátuma, LastGreenAlertGenerated, DatabaseTimeModified|


### <a name="to-work-with-updates"></a>Dolgozhat a frissítések

1. Az **Áttekintés** oldalon kattintson a **Rendszer frissítés értékelési** csempére.  
    ![Rendszer frissítés értékelési csempe](./media/log-analytics-system-update/sys-update-tile.png)
2. A **frissítések** irányítópult a frissítést kategóriák megjelenítése.  
    ![Frissítések irányítópult](./media/log-analytics-system-update/sys-updates02.png)
3. Görgetés jobbra látható a lapon a **Kritikus/biztonsági frissítések a Windows** -lap megtekintése és **besorolását**, válassza a **Frissítések**.  
    ![Biztonsági frissítések](./media/log-analytics-system-update/sys-updates03.png)
4. A napló lapon a infrastruktúra talált hiányzó kiszolgálókról biztonsági frissítéseiről adatokat számos látható. Kattintson a frissítések részletes információinak megtekintése a **listában** .  
    ![Keresési eredmények - lista](./media/log-analytics-system-update/sys-updates04.png)
5. A napló lapon információt az egyes frissítések jelenik meg. A KBID szám mellett kattintson a **Nézet** megtekintéséhez megfelelő a cikk a Microsoft támogatási webhelyen.  
    ![Jelentkezzen be a keresési eredmények - nézet KB](./media/log-analytics-system-update/sys-updates05.png)
6. A böngésző frissítés a Microsoft támogatási weblap megnyílik egy új lapon. A frissítés, amely nem található kapcsolatos információk megtekintése.  
    ![A Microsoft Support az weblapon](./media/log-analytics-system-update/sys-updates06.png)
7. Információk felhasználásával használatával megtalálta, létrehozhat egy tervet manuális frissítést a hiányzó vagy továbbra is a frissítést az alkalmazás automatikusan alkalmazza a hátralévő lépéseket követően.
8. Szeretne automatikusan alkalmazza a hiányzó frissítés, térjen vissza az **frissítések** irányítópult, és a **Update Lefut**, válassza a **kattintson ide az ütemezett frissítés futtatása**.  
    ![Update Lefut - ütemezett lap](./media/log-analytics-system-update/sys-updates07.png)
9. **Update Lefut** lapon az **ütemezett** lapon kattintson a **Hozzáadás** hozhat létre egy új frissítés futtatása lehetőséget.  
    ![Ütemezett tab - hozzáadása](./media/log-analytics-system-update/sys-updates08.png)
10. **Új frissítés futtatása** lapon írja be egy nevet a frissítés futtatása, egyes számítógépeken vagy a számítógép csoportok hozzáadása, ütemezéséről, és kattintson a **Mentés**parancsra.  
    ![Új frissítés futtatása](./media/log-analytics-system-update/sys-updates09.png)
11. Az **Update Lefut** lapon látható az új frissítést, hogy futtatni **ütemezett** lapján, van ütemezve.  
    ![Ütemezett lap](./media/log-analytics-system-update/sys-updates10.png)
12. A frissítés futtatása indításakor, látni fogja azt az információt az **operációs rendszert futtató** lapon.  
    ![Aktív lap](./media/log-analytics-system-update/sys-updates11.png)
13. A frissítés futtatása után a **kész** lap állapota.
14. Ha frissítések a frissítés futtatása a **Kritikus/biztonsági frissítések a Windows** lap a volt alkalmazott látni fogja, hogy a frissítések száma csökken.  
    ![A kritikus/biztonsági frissítések a Windows lap - csökkentett frissítés száma](./media/log-analytics-system-update/sys-updates12.png)



## <a name="next-steps"></a>Következő lépések

- [Keresés naplók](log-analytics-log-searches.md) megtekintéséhez a részletes adatok frissítése.
