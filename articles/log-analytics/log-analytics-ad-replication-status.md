<properties
    pageTitle="Active Directory replikációs állapotát megoldás napló Analytics |} Microsoft Azure"
    description="Az Active Directory replikációs állapotát megoldás csomag rendszeresen figyeli a környezet Active Directory replikációs hibákat, és az eredmények jelentéseket a MOBILE irányítópulton."
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

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory replikációs állapotát megoldás napló Analytics

Az Active Directory-vállalati környezetben fő része. Magas rendelkezésre állásának és a nagy teljesítményű érdekében minden tartomány vezérlőnek az Active Directory adatbázis saját példányát. Tartomány vezérlők bizonyos egymással annak érdekében, hogy a módosítások átvitele a cégen belül. A replikációs folyamatot a hibák okozhatnak problémák számos a cégen belül.

Az Active Directory-replikáció állapotát megoldás csomag rendszeresen figyeli a környezet Active Directory replikációs hibákat, és az eredmények jelentéseket a MOBILE irányítópulton.

## <a name="installing-and-configuring-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldás
Az alábbi információk segítségével telepítse és állítsa be a megoldást.

- A tartomány vezérlők, amelyek a tartomány tagjai ki kell értékelni, vagy beállította az Active Directory-replikáció adatok küldése a MOBILE tag kiszolgálókon telepíteni kell az ügynökök. Windows rendszerű számítógépeken keresztüli MOBILE, című cikkben talál részletes [napló Analytics csatlakoztatása a Windows számítógépek](log-analytics-windows-agents.md). Ha a tartományvezérlőnek része már meglévő System Center Operations Manager környezetben, MOBILE csatlakozni szeretne, akkor a [Operations Manager kapcsolatot a naplóban Analytics](log-analytics-om-agents.md).
- Az Active Directory replikációs állapotát megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  Nem szükséges, nincs további beállításokat.


## <a name="ad-replication-status-data-collection-details"></a>Active Directory replikációs állapotát Adatrészletek gyűjtemény

A következő táblázat mutatja az adatgyűjtési módszerek és más adatait, hogy hogyan adatgyűjtés az Active Directory-replikáció állapotát.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
|A Windows|![igen](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![igen](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![nem](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![nem](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![igen](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| 5 naponként|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Tetszés szerint az Active Directory adatok küldése a MOBILE egy tartományon kívüli vezérlő engedélyezése
Ha nem szeretné a tartomány vezérlők bármelyikét közvetlenül csatlakozik MOBILE, is használhatja bármely más MOBILE csatlakoztatott számítógép az Ön tartományában lévő adatgyűjtés az Active Directory-replikáció állapotát megoldás Pack és küldje el az adatok.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Active Directory adatok küldése a MOBILE egy tartományon kívüli vezérlő engedélyezése
1.  Győződjön meg arról, hogy a számítógépen-e a tartományt, használja az Active Directory-replikáció állapotát megoldás figyelni kívánt tag.
2.  [A Windows MOBILE számítógép csatlakoztatása](log-analytics-windows-agents.md) vagy [a meglévő Operations Manager-környezetből MOBILE használatával csatlakozni](log-analytics-om-agents.md), ha nem csatlakozik.
3.  Az adott számítógépen állítsa a következő beállításkulcsot:
    - Kulcs: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management csoportok\<ManagementGroupName > \Solutions\ADReplication**
    - Érték: **IsTarge**
    - Érték: **Igaz**

    >[AZURE.NOTE]A módosítások csak akkor lépnek érvénybe a indítsa újra a Microsoft figyelése Agent szolgáltatás (HealthService.exe).

## <a name="understanding-replication-errors"></a>Replikációs hibákat ismertetése
Ha befejezte az Active Directory replikációs állapot adatok MOBILE küldött, látni fogja a mozaik, az alábbihoz hasonló az jelenleg hány replikációs hibát jelző MOBILE irányítópulton.  
![Active Directory-replikáció állapotát csempe](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritikus replikációs hibák** azok a vagy a [törlésre élettartam](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) az Active Directory-erdő 75 %-nál.

Ha a mozaik gombra kattint, látni fogja a hibák további információt.
![Active Directory-replikáció állapotát irányítópult](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Cél kiszolgáló és forrás kiszolgáló állapota
Ezeket a pengéit cél és replikációs hibákat tapasztal, adatforrás-kiszolgálók állapotának megjelenítése. A számot, minden egyes tartományhoz vezérlő neve után azt jelzi, hogy a tartomány replikációs hibák számát.

A hibák cél kiszolgálóihoz és adatforrás-kiszolgálók mert problémák a cél kiszolgáló perspektívából hibák elhárítása a forrás kiszolgáló perspektíva és mások könnyebben jelennek meg.

Ebben a példában látható, hogy sok cél kiszolgálón van nagyjából ugyanannyi hibákat, de egy forráskiszolgáló (ADDC35), amelynek számos további hibákat, mint a többi. Valószínű, hogy van problémák megoldására, hogy nem küld adatokat való replikáció partnerei ADDC35. A problémák megoldása a ADDC35, az a hibák, amelyek akkor jelennek meg a célként megadott kiszolgáló lap számos valószínűleg feloldja.

### <a name="replication-error-types"></a>Replikációs hiba típusai
Ez a lap a hibák, a szervezetben típusú információt nyújt. Az egyes hibák egyedi numerikus kódját, és egy üzenet, amely segítséget nyújtanak a hiba okát a legfelső szintű tartalmaz.

A gyűrű tetején adja vissza, mely jelennek meg, további és ritkábban a arról.

Ezzel megjelenítheti, amikor több tartományt vezérlők tapasztalható a ugyanaz a replikáció hiba jelentkezik. Ebben az esetben, akkor fel egy tartományvezérlőnek a megoldásán, akkor ismételje meg a többi ugyanaz a hiba jelentkezik által érintett tartomány vezérlők a.

### <a name="tombstone-lifetime"></a>Élettartam törlésre
A törlésre élettartam határozza meg, hogy mennyi ideig törölt objektumok, a törlésre néven, az Active Directory-adatbázisban megőrződnek. Törölt objektumok átadja a törlésre élettartam, amikor a szemétgyűjtő folyamatának automatikusan eltávolítja az Active Directory-adatbázisból.

Az alapértelmezett törlésre élettartama legújabb verziója a Windows, 180 napig tart, de korábbi verzióiban 60 napig volt, és az Active Directory-rendszergazda által explicit módon módosítható.

Fontos tudnivalók, ha megközelíti vagy a törlésre élettartam lejárt replikációs hibákat tapasztal. Két tartomány vezérlő, amely korábbi a törlésre élettartam továbbra is fennáll a replikáció hiba fordul elő, ha a replikáció letiltjuk e két tartomány vezérlők között, ha az alapul szolgáló replikációs hiba megoldódott.

Azonosítsa a helyek ennek az az oka veszélyt hol található a törlésre élettartam lap segítségével. A hibák kijavítására az **100 %-nál TSL** kategória sávot jelöl még nem replikált a forrás- és kiszolgáló közötti legalább a törlésre élettartam erdőhöz.

Ebben az esetben a egyszerűen a a replikáció hiba kijavítása nem elég. Legalább kell manuálisan vizsgálja meg a azonosítása, és objektumokat, mielőtt adattisztítási hozhat létre is indítsa újra a replikáció. Még szüksége lehet a tartományvezérlőnek le.

Nemcsak a múltbeli a törlésre élettartam van állandó replikációs hibák azonosítása, fog is szeretne fizetni az **50-75 % -os TSL** vagy **75-100 % -os TSL** kategóriákba tartozó hibák figyelmet.

Ezek a hibák, amelyek egyértelműen megoldani, nem tranziens, így valószínűleg a beavatkozás úgy kell. A jó hír az, hogy azok nem ért a törlésre élettartam. Ha azonnal a problémák kijavításához és *előtt* a törlésre élettartam érik, replikációs is indítsa újra a minimális kézi beavatkozás.

Amint korábbi, az Active Directory-replikáció állapotát megoldás irányítópult csempéje számát jeleníti meg a *kritikus* replikációs hibákat a környezetben, amely definíciója hibák, amelyek a törlésre leírási idő (beleértve a hibák, amelyek több mint TSL 100 %-os) 75 %-nál. Ez a szám 0 őrizni törekedjen.

>[AZURE.NOTE] Az összes a törlésre élettartam százalékos számításokat, melyekben megbízhat, hogy ezek százalékos pontosak, akkor is, ha az Ön egyéni törlésre élettartam értékek vannak beállítva, a tényleges törlésre élettartam, az Active Directory-erdő alapulnak.

### <a name="ad-replication-status-details"></a>Active Directory-replikáció állapotadatok
Amikor a listák közül bármelyik elemére kattint, látni fogja a napló keresés használatával kapcsolatban további részleteket. Az eredmények vannak szűrve csak az adott elem kapcsolatos hibákat. Ha például a **Cél kiszolgáló állapota (ADDC02)**beállításnál első tartományvezérlőnek kattint, ha látni fogja szűrve a keresési eredmények megjelenítése hibákat, hogy a tartományvezérlőnek a célként megadott kiszolgáló állapotúként:

![Active Directory replikációs állapot hibákat a keresési eredmények között](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Innen kiindulva további szűrése, a kereső lekérdezés módosítása, és így tovább. A napló keresés használatával kapcsolatos további tudnivalókért lásd: a [napló keresések](log-analytics-log-searches.md).

A **HelpLink** mező jeleníti meg, hogy egy adott hibakóddal kapcsolatban további részleteket TechNet lap URL-CÍMÉT. Másolja a vágólapra, és illessze be a böngésző ablakának szeretne tájékozódni a hibaelhárítás és a hiba kijavítása erre a hivatkozásra.

Az eredmények exportálása az Excelbe az **Exportálás** is kattinthat. Ez lehetővé teszi, hogy semmilyen formában szeretné a replikáció hiba adatok ábrázolása.

![exportált AD replikációs állapot hibákat, az Excel programban](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Active Directory-replikáció állapotát – gyakori kérdések
**Kérdés: milyen gyakran az Active Directory replikációs állapot adatok frissítése?**
A: az adatok minden 5 nap frissülnek.

**Kérdés: van-e lehetőség a adatok frissítési gyakoriságának beállítása?**
Megoldás: a problémának jelenleg nincs.

**Kérdés: van szükségem hozzáadni a tartomány vezérlők a MOBILE munkaterületre való replikáció állapot látható?**
V: nem, csak egy tartományvezérlőnek hozzá kell adnia. A MOBILE munkaterületen Ha több tartományt vezérlők, mindegyiket adatainak MOBILE küldi.

**Probléma: nem szeretnék bármelyik tartomány vezérlők hozzáadása a MOBILE munkaterülethez. Van-e még lehetőség az Active Directory-replikáció állapotát megoldás?**
V: Igen. Beállíthatja, hogy ahhoz, hogy ez egy beállításkulcs értékének. Lásd: [ahhoz, hogy egy tartományon kívüli vezérlő MOBILE Active Directory adatok küldése](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**Kérdés: Mi az a folyamat adatgyűjtési fejlesztő neve?**
A: AdvisorAssessment.exe

**Kérdés: hogyan ideig tart a gyűjtendő adatok?**
V: az adatok gyűjtése idő attól függ, az Active Directory-környezet méretét, de általában a kisebb, mint a 15 percet vesz igénybe.

**Kérdés: milyen típusú adatokat kell felvenni?**
A: a replikáció adatgyűjtés LDAP keresztül.

**Kérdés: van-e lehetőség beállítása során gyűjtött adatok?**
Megoldás: a problémának jelenleg nincs.

**K: milyen engedélyekkel kell adatgyűjtés?**
A: normál felhasználói engedélyek Active Directoryhoz általában megfelelőek.

## <a name="troubleshoot-data-collection-problems"></a>Adatok gyűjtése kapcsolatos hibák elhárítása
Annak érdekében, hogy adatgyűjtés, az Active Directory-replikáció állapotát megoldás csomag szükséges legalább egy tartományvezérlőnek a MOBILE munkaterület csatlakoztatni. Ezt megtette, amíg meg nem jelenik meg üzenet jelzi, hogy **továbbra is gyűjtött adatok**.

Csatlakozás a tartomány vezérlőket segítségre van szüksége, ha a dokumentáció [napló Analytics való csatlakozás Windows](log-analytics-windows-agents.md)mögött tekinthet meg. Másik lehetőségként a tartományvezérlőnek már csatlakozik egy meglévő System Center Operations Manager-környezet, dokumentáció a a [Csatlakozás rendszer központ Operations Manager a napló Analytics](log-analytics-om-agents.md)tekinthet meg.

Ha nem szeretne összekapcsolni, a tartomány vezérlők bármelyikét közvetlenül MOBILE vagy SCOM, lásd: [ahhoz, hogy egy tartományon kívüli vezérlő MOBILE Active Directory adatok küldése](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Következő lépések

- [Log Analytics napló keresések](log-analytics-log-searches.md) segítségével részletes Active Directory-replikáció állapot adatainak megtekintése.
