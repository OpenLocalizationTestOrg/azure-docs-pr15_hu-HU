<properties
    pageTitle="Hálózati teljesítmény Monitor megoldás a MOBILE |} Microsoft Azure"
    description="Hálózati teljesítmény Monitor figyelheti a hálózatok a közelében real-egyszer-való ellátása segít észleli, és keresse meg a hálózati teljesítmény szűk."
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
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Hálózati teljesítmény Monitor (előzetes verzió) megoldás a MOBILE

>[AZURE.NOTE] Ez az [előnézeti megoldás](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Ez a témakör lehet beállítani, és hogy a MOBILE, amely segít a hálózatok a közelében real-egyszer címzettje teljesítményének figyelésére észleli, és keresse meg a hálózati teljesítmény Monitor megoldást hogyan hálózati teljesítmény szűk. A hálózati teljesítmény Monitor megoldásban figyelheti a veszteség és a késés hálózatok között, alhálózat vagy kiszolgálókhoz. Hálózati teljesítmény Monitor észleli, például a forgalom blackholing, útválasztási hibák és problémák, amelyek a hagyományos hálózati ellenőrzési módszer nem észleli a hálózati hibák. Hálózati teljesítmény Monitor riasztások hoz létre, és értesítést küld, amikor egy küszöbérték a hálózati kapcsolat megsértése. Ezeket a küszöbértékek is lehet tanultakhoz automatikusan a rendszer, vagy egyéni figyelmeztető szabályok konfigurálhatók. Hálózati teljesítmény Monitor hálózati teljesítmény hibák időben kimutatása biztosítja, és egy adott hálózati szakaszában vagy eszközre a problémát a forrás localizes.

Elvégezheti a problémákkal szemben a megoldást irányítópult, amelyek a legutóbbi hálózati állapot események, sérült hálózati kapcsolatok és alhálózat hivatkozásokat, hogy a csomag magas fokú és a késés szemben legyen hálózati összesített információkat jeleníti meg. Akkor lehet ásni be egy hálózati kapcsolat alhálózat hivatkozások, valamint a csomópontok hivatkozások jelenlegi állapotának megtekintéséhez. Megtekintheti a korábbi trendjének veszteség és a késlekedési idő a hálózat, a alhálózat és a csomópontok szinten is. Ideiglenes (tranziens) hálózati hibák feltárása korábbi trend diagramok csomag vész és a késés megtekintésével, és keresse meg a hálózati szűk egy topológia térképen. Az interaktív topológia graph lehetővé teszi a azonosítható a azonosítható által hálózati útvonalak ábrázolása, és meghatározzák, hogy a hiba forrását. Egyéb alkalmazások, például a napló keresési különböző analytics követelményeket segítségével hálózati teljesítmény Monitor által gyűjtött adatok alapján, egyéni jelentések létrehozása.

A megoldás egy elsődleges mechanizmusként szintetikus tranzakciók hálózati hibák feltárása használja. Igen felhasználhatja azt egy adott hálózati eszköz szállító vagy modell tekintet nélkül. A helyszíni, a felhő (IaaS) és a hibrid környezetekben működik. A megoldás automatikusan feltárja a hálózati topológiája és a hálózat különböző útvonalak.

Tipikus hálózati felügyeleti termékek kiemelése a hálózati eszköz (útválasztó, a kapcsolók stb.) rendszerállapot figyelése, de nem adja meg az összefüggéseket a hálózati teljesítmény Monitor jelent két pontok közötti hálózati kapcsolat tényleges minőségét.

### <a name="using-the-solution-standalone"></a>A megoldás önálló használatával

Ha szeretne a kritikus munkaterhelésekből közötti hálózati kapcsolatainak minőségének, hálózatok, adatközpontokkal vagy az office-webhelyek, akkor használhatja a hálózati teljesítmény Monitor megoldást önmagában Lync-kapcsolat állapota között:

- a nyilvános vagy privát hálózaton keresztül csatlakoztatott több adatközpontokkal vagy az office webhelyen
- fontos feladatok üzleti futó
- nyilvános felhő szolgáltatásokat, mint a Microsoft Azure vagy Amazon Web Services (AWS) és a helyszíni hálózatok, ha van IaaS (virtuális) elérhető, és a kommunikáció konfigurált átjárók van a helyszíni hálózatok és a felhő hálózatok között
- Azure és a helyszíni hálózatok Express útvonal használatakor

### <a name="using-the-solution-with-other-networking-tools"></a>A megoldás használata más hálózati eszközök

Ha egy szövegsor az üzleti alkalmazások figyelni kívánt, más hálózati eszközök companion megoldásként a hálózati teljesítmény Monitor megoldást is használhatja. Lassú hálózaton vezethet lassú alkalmazást, és a hálózati teljesítmény Monitor segítséget nyújtanak az alapul szolgáló hálózati problémákat okozó alkalmazás teljesítményét érintő hibák kivizsgálása. Mivel a megoldást nem kell-e bármelyik hálózati eszközök eléréséhez, az alkalmazás rendszergazdájának nem kell használja arra, hogyan gyengíti a hálózat az alkalmazások kapcsolatos információk a hálózati csoport.

Is ha Ön már más hálózati eszközök figyelése fektetni, majd a megoldást is kiegészítése ezeket az eszközöket, mert a hagyományos hálózati felügyeleti megoldások nem adja meg az összefüggéseket a végpontok közötti hálózati teljesítménymutatók például veszteség és a késés.  A hálózati teljesítmény Monitor megoldást segíthetnek töltse ki a távolság.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Való telepítéséről és konfigurálásáról a megoldáshoz ügynökök beállítása

Az alapvető folyamatokat segítségével [napló Analytics csatlakoztatása a Windows számítógépek](log-analytics-windows-agents.md) és [Operations Manager kapcsolatot a naplóban Analytics](log-analytics-om-agents.md)ügynökök telepítheti.

>[AZURE.NOTE]
Legalább 2 ügynökök telepítése annak érdekében, hogy elég Fedezze fel, és a hálózati erőforrások figyelésére adatot kell. Egyéb esetben a megoldást marad konfigurálása állapotban telepítése és beállítása további ügynökök.

### <a name="where-to-install-the-agents"></a>A ügynökök telepítési helye

Ügynökök a telepítés előtt vegye figyelembe a hálózat és a figyelni kívánt mely részei a hálózat topológiájának. Azt javasoljuk, hogy minden alhálózathoz figyelni kívánt egynél több ügynök telepítése. Ez azt jelenti minden alhálózat figyelni kívánt, válassza ki a két vagy több kiszolgálók, illetve VMs és a agent telepítése.

Ha Ön nem tudja, hogy a hálózat topológiájának, telepítse az ügynökök a kívánt helyre a hálózati teljesítmény figyelését kritikus munkaterhelésekből kiszolgálókon. Például érdemes lehet nyomon követheti a webkiszolgálóra és az SQL Server-kiszolgáló közötti hálózati kapcsolaton. Ebben a példában mindkét kiszolgáló ügynökszoftvert módon telepíthető.

Ügynökök figyelheti a hálózati kapcsolat (hivatkozások) állomások – nem magukat a hosts között. Igen Lync-egy hálózati kapcsolat, telepítenie kell ügynökök két végponton található a hivatkozás.

### <a name="configure-agents"></a>Ügynökök beállítása

Miután telepítette a ügynökök, kell ezekről a gépekről annak érdekében, hogy ügynökök kommunikálhat a tűzfalportokat megnyitása. Töltse le és futtassa a [EnableRules.ps1 PowerShell-parancsprogramot](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) paraméter nélkül a rendszergazdai jogosultságokkal rendelkező PowerShell ablakában kell

A parancsfájl a hálózati teljesítmény Monitor által igényelt beállításkulcsokat hoz létre, és a Windows tűzfal szabályok lehetővé teszi a TCP-kapcsolatok létrehozása a többi ügynökök hoz létre. A parancsprogram által létrehozott beállításkulcsokat is adja meg, hogy jelentkezzen be a hibakeresési naplókat és a naplókat fájl elérési útját. Szintén a kommunikációhoz használt ügynök portot határozza meg. Billentyűk értékeinek automatikusan beállítva a parancsfájl, így nem kell manuálisan módosítani billentyűk.

Alapértelmezés szerint a megnyitott portja 8084. Egyéni port használható, mert a paraméter `portNumber` a parancsfájl számára. Jó helyen jár ugyanazt a portot kell használni az összes számítógépen hol a parancsfájlt.

>[AZURE.NOTE] A EnableRules.ps1 parancsfájl a Windows tűzfal szabály csak azon a számítógépen, ahová a parancsfájlt konfigurálása Ha egy hálózati tűzfal, gondoskodnia kell arról, hogy engedélyezi-e a hálózati teljesítmény Monitor által használt portot adatforgalmat.


## <a name="configuring-the-solution"></a>A megoldás konfigurálása

Az alábbi információk segítségével telepítse és állítsa be a megoldást.

1. A hálózati teljesítmény Monitor megoldást szerez adatok az 1 vagy újabb verzió a Windows Server 2008 SP rendszert futtató számítógépeken vagy a Windows 7 SP1 vagy újabb, amelyek ugyanazokat a követelményeket, a Microsoft figyelése ügynök (MMA).
2. A hálózati teljesítmény Monitor megoldás hozzáadása a MOBILE munkaterület [hozzáadása napló Analytics-megoldások a megoldástárból](log-analytics-add-solutions.md)leírt eljárással.  
  ![Hálózati teljesítmény Monitor szimbólum](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. A MOBILE portálon **Hálózati teljesítmény Monitor** című az üzenettel *megoldás további konfiguráció szükséges*új csempe megjelenik. Állítsa be a megoldás hozzáadása a hálózatok alhálózatokra és a csomópontok ügynökök felismert alapján kell. Kattintson a **Hálózati teljesítmény Monitor** alapértelmezett hálózat konfigurálása indításához.  
  ![megoldás további konfiguráció szükséges.](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>A megoldás egy alapértelmezett hálózat konfigurálása

Az beállítása lapon láthatja az **alapértelmezett**nevű egyetlen hálózat. Bármely hálózatok definiálásakor még nem minden automatikus észlelését alhálózat az alapértelmezett hálózat kerülnek.

Amikor létrehoz egy hálózati, alhálózat hozzáadása, és alhálózat törlődik az alapértelmezett hálózat. Ha töröl egy hálózati, minden alhálózat automatikusan visszatér az alapértelmezett hálózat.

Más szóval az alapértelmezett hálózat olyan felhasználó által definiált hálózat nem szereplő összes alhálózat tárolója. Nem lehet szerkesztése vagy törlése az alapértelmezett hálózat. Mindig marad a rendszer. Szükség szerint annyi hálózatok is létrehozhat.

A legtöbb esetben a alhálózat a szervezet egynél több hálózat fog elhelyezni, és hozzon létre egy vagy több hálózatok logikailag csoportosíthatja a alhálózat.

### <a name="create-new-networks"></a>Hozzon létre új hálózatok

A hálózati teljesítmény Monitor hálózaton alhálózat tárolója. A hálózati hozhat létre egy tetszőleges nevet szeretne, és a alhálózat hozzáadása a hálózathoz. Például *Building1* nevű hálózat létrehozása, és vegye fel alhálózat, vagy hozzon létre egy hálózati *DMZ* nevű, és vegye fel a hálózat demilitarizált zónába tartozó összes alhálózat.

#### <a name="to-create-a-new-network"></a>Új hálózat létrehozása

1. Kattintson a **Hozzáadás a hálózat** elemre, és írja be a hálózat nevét és leírását.
2.  Jelölje ki egy vagy több alhálózat, és kattintson a **Hozzáadás**gombra.
3. A konfiguráció mentéséhez a **Mentés** gombra.  
  ![hálózat hozzáadása](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Várakozás az adatok összesítése

A konfiguráció első alkalommal mentette, miután a megoldást elindul, adatgyűjtés hálózati csomag veszteség és a késlekedési idő a csomópontok, ahol anyagok telepített között. Ez a folyamat eltarthat egy ideig, időnként 30 percig. Ez az állapot alatt a hálózati teljesítmény Monitor csempére az Áttekintés lapon egy üzenet arról az *adatok összesítése folyamat*jeleníti meg.

![folyamatban lévő adatok összesítése](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Az adatok fel van töltve, amikor megjelenik a hálózati teljesítmény Monitor frissített csempe adatok.

![Hálózati teljesítmény Monitor csempe](./media/log-analytics-network-performance-monitor/npm-tile.png)

Kattintson a hálózati teljesítmény Monitor irányítópult megtekintése a csempére.

![Hálózati teljesítmény Monitor irányítópult](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Az alhálózathoz ellenőrzési beállításainak szerkesztése

Minden alhálózat, ahol legalább egy ügynök telepítve volt az beállítása lapon **alhálózatokra** lapján sorolja fel.

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Engedélyezése vagy letiltása az adott alhálózatokra figyelése

1. Jelölje ki, vagy törölje a jelet a jelölőnégyzetéből az **alhálózat azonosító** , és majd győződjön meg arról, hogy **Figyelés használható** kijelölt bejelölve, illetve szükség szerint. Jelölje ki, vagy törölje a jelet több alhálózat. Le van tiltva, az alhálózatokra nem a ügynökök ekkor frissülnek le szeretné állítani a többi ügynökök pingelés ellenőrizni.
2. Válassza ki a kívánt figyelheti az egy adott alhálózat alhálózat kijelölésével a listából, és a szükséges csomópontok mozgatása figyelt és felügyelt csomópontok tartalmazó lista közötti csomópontok.
Vehet egy egyéni **Leírás** alhálózat, ha szeretné.
3. A konfiguráció mentéséhez a **Mentés** gombra.  
  ![alhálózat szerkesztése](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Válassza a Lync-csomópontok

A csomópontok őket telepített ügynökszoftvert rendelkező jelennek meg a **csomópontok** fülre.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Engedélyezése vagy letiltása a csomópontok figyelése

1. Jelölje be, vagy törölje a csomópontok megfigyelése és leállítása ellenőrzés kívánt.
2. Kattintson a **Figyelés használatát**, vagy törölje a jelet azoknak, szükség szerint.
3. Kattintson a **Mentés**gombra.  
  ![Engedélyezze a csomópont figyelése](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Figyelés szabályok beállítása

Hálózati teljesítmény Monitor állapot események kapcsolatos csomópontok vagy alhálózat vagy hálózati a hivatkozások küszöbértéket megsértése két közötti kapcsolatot hoz létre. Ezeket a küszöbértékek is lehet tanultakhoz automatikusan a rendszer, vagy beállíthatja őket egyéni figyelmeztető szabályok.

A rendszer által létrehozott *alapértelmezett szabály* van, és a rendszer tanultakhoz küszöbértékét létrehoz egy állapot eseményt, ha elvesztése vagy késés bármely pár hálózatok vagy alhálózat közötti kapcsolatok szabályok megsértésével. Lehetősége van az alapértelmezett szabály letiltása és egyéni ellenőrzési szabályok létrehozása

#### <a name="to-create-custom-monitoring-rules"></a>Egyéni ellenőrzési szabályok létrehozása

1. A **Monitor** csoportjában kattintson a **Szabály hozzáadása** gombra, és írja be a szabály nevét és leírását.
2. Jelölje be a Lync-a listákban a hálózati vagy alhálózat hivatkozások pár.
3. Először jelölje ki a hálózaton, amelyben az érdeklődésre számot tartó első alhálózat/s szerepel a hálózati legördülő listából, és válassza a alhálózat s a megfelelő alhálózat legördülő listából.
Jelölje be a **minden alhálózatokra** , ha a hálózati kapcsolat az összes alhálózatokra figyelni kívánt. Hasonlóképpen, jelölje ki a többi alhálózat/s érdeklődésre számot tartó. És **Adhat kivételeket** a beállításától adott alhálózat hivatkozásainak figyelése kizárása gombra kattint.
4. Ha nem szeretné a kijelölt elemek állapot események létrehozása, majd törölje a jelet **a rendszerállapot figyelése a szabály által érintett hivatkozásokra engedélyezése**.
5. Válassza ki feltételeket figyelése.
Beállíthatja, hogy egyéni küszöbértékek állapot rendezvény létrehozásánál küszöbértéket beírásával. A feltétel értéke a a kijelölt hálózati/alhálózat pár kijelölt küszöbértékét fölé kerül, amikor egy állapot jön létre.
6. A konfiguráció mentéséhez a **Mentés** gombra.  
  ![Egyéni felügyeleti szabály létrehozása](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Adatok gyűjtése részletei

Hálózati teljesítmény Monitor a gyűjthetők össze az adatvesztés TCP SYN-SYNACK-ACK-csomagokat, azokat handshake használ, és a késés információk és traceroute is használt topológia információ.

A következő táblázat mutatja az adatgyűjtési módszerek, és hogyan adatgyűjtés hálózati teljesítmény monitor más adatait.

| Platform | Közvetlen Agent | SCOM agent | Azure tárhely | Kötelező SCOM? | E-kezelés csoport elküldött SCOM ügynök adatok | a webhelycsoport gyakorisága |
|---|---|---|---|---|---|---|
| A Windows |![igen](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![igen](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![nem](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![nem](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![nem](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP kézfogások 5 másodpercenként, adatok küldött 3 percenként |

A megoldást hoz létre segítségével szintetikus tranzakciók mérje fel, hogy a hálózat állapotát jelző. Telepítve van a hálózati exchange TCP-csomagok egymással, illetve a folyamat különböző pontján MOBILE ügynökök üzenetváltási időt és a csomagot az adatvesztés ismerje meg, ha vannak ilyenek. Minden egyes ügynök rendszeres időközönként is egy nyomkövetési útvonalat más anyagokkal kell vizsgálni, hogy a hálózat minden olyan különböző útvonal kereséséhez hajt végre. Ezeket az adatokat használ, a ügynökök is tudják a hálózati késés és a csomag veszteség számok rovat. Tesztek ismétlődnek öt másodpercenként és adatokat az összesíteni három perc időszakra vonatkozóan a ügynökök által feltöltése a MOBILE előtt.

>[AZURE.NOTE] Ügynökök egymással gyakran, de azok nem készítése a hálózati forgalmának engedélyezésére sok közben a tesztek. Ügynökök csak a TCP-SYN-SYNACK-ACK handshake csomagok határozza meg a veszteség és a késés – nincsenek adatok csomagok továbbított támaszkodhat. A folyamat során ügynökök egymással csak akkor, ha szükséges, és a ügynök kapcsolati topológia hálózati forgalmának van optimalizálva.

## <a name="using-the-solution"></a>A megoldás használata

Ebből a szakaszból az összes irányítópult funkciókat és hogyan használhatók.

### <a name="solution-overview-tile"></a>Megoldás áttekintése csempe

Miután engedélyezte a hálózati teljesítmény Monitor megoldást, a megoldás csempét a MOBILE áttekintése lapon a gyors áttekintés a hálózati állapot biztosít. A megfelelő és sérült alhálózat hivatkozások számát megjelenítő perecdiagramon jeleníti meg. Ha a mozaik gombra kattint, a megoldást irányítópult nyílik meg.

![Hálózati teljesítmény Monitor csempe](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Hálózati teljesítmény Monitor megoldás irányítópult

A **Hálózati összesítő** lap összegzi a hálózatok relatív méretük együtt. A mozaikok megjelenítő hálózati kapcsolatok, a alhálózat hivatkozások és a görbék száma a rendszer (IP-címei anyagokkal két hosts és a közöttük lévő összes Ugrás áll egy elérési utat) követi.

A **Felső hálózati állapot események** lap egy listát ad az legutóbbi állapot események és riasztások a rendszer és az idő óta az esemény aktív. A rendszerállapot eseményt vagy értesítés, amikor a csomag elvesztése vagy hálózati vagy alhálózat csatolás késés eléri az jön létre.

A **Sérült hivatkozások felső a hálózati** lap sérült hálózati hivatkozásainak listája látható. Ezek a hálózati hivatkozásokat, hogy telepítve van egy vagy több káros egészségügyi esemény őket az adott pillanatban.

**Felső alhálózat hivatkozásokat tartalmaz a legtöbb adatvesztés** és **alhálózat leginkább válaszidejű** rögzítéséhez rendre megjelenítése a felső alhálózat hivatkozásokat tartalmaz a csomag veszteséggel és felső alhálózat szerint, késés. Nagy késést vagy néhány csomag veszteség összege várható bizonyos hálózati kapcsolatokról. Az ilyen hivatkozások jelennek meg a felső tíz listák, de ezeket nem jelöli meg megsérült.

Az **Általános lekérdezések** lap a kereséseket, amelyek a nyomon követési adatok közvetlenül nyers hálózati lehívása tartalmazza. Használhatja ezeket a lekérdezéseket kiindulási pontként létrehozásához a saját lekérdezések testre szabott tartalmazó jelentések készítéséhez.

![Hálózati teljesítmény Monitor irányítópult](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Ha a z-Lehatolás

A megoldás irányítópult Lehatolás mélyebb különböző hivatkozások be minden olyan elemet az érdeklődési terület kattinthat. Például ha jelzést vagy egy sérült hálózati kapcsolat jelennek meg az irányítópulton, választhatja azt vizsgálja meg a további. Fogja venni, mely tartalmazza az adott hálózati kapcsolat alhálózat hivatkozások lapra. Minden alhálózat hivatkozás veszteség, a késés és az állapot állapotának megtekintése lesz, és gyorsan megtudhatja, hogy milyen alhálózat hivatkozások okozzák a problémát. Ezután kattintson **Csatolások megtekintése csomópontra** a sérült alhálózat hivatkozás csomópont-hivatkozások. Ezután lásd: az egyes csomópontok – hivatkozások, és keresse meg a sérült csomópont hivatkozások.

**Nézet topológia** a leveleket a forrás- és céltáblák csomópontok között a azonosítható – Ugrás topológiájának megtekintése gombra. A sérült útvonalak vagy Ugrás piros színnel jelennek meg, hogy gyorsan megtalálják a problémát az egy adott részét a hálózathoz.

![Lehatolás adatok](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trend-diagramok

A minden szintű, amikor Lehatolás, megjelenik a trend veszteség és a hálózati kapcsolat késés. Trend diagramok alhálózat és csomópont hivatkozások is elérhetők. A diagram az idő vezérlő tetején látható a diagramon ábrázolni időközének módosítása

Trend-diagramok mutatja, hogy egy hálózati kapcsolat teljesítményének korábbi perspektíva. Hálózati problémák elhárításához jellegű ideiglenes (tranziens), és elfog a hálózat aktuális állapotát megtekintésével csak nehezen lenne. Ennek az oka problémák gyorsan felület, és a előtt bárki azt észleli, csak az idő ismét megjelenik egy újabb pontján eltűnnek. Az ilyen átmeneti problémák is rendszergazdái számára nehezen, mert azok problémák gyakran felület, a megtévesztő növekedése alkalmazás válaszidő, még akkor is, ha az összes alkalmazásösszetevők zökkenőmentes futtatásához jelennek meg.

Ha a probléma megjelennek egy hirtelen Nyárs hálózati késés vagy csomag vész trend diagram megjeleníti egyszerűen elvégezheti ilyen típusú problémák.

![Trend-diagram](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>A azonosítható – Ugrás topológia térkép

Hálózati teljesítmény Monitor útvonalak egy interaktív topológia térképen két csomópontok között a azonosítható – Ugrás topológiájának jeleníti meg. A csomópont hivatkozásra kattint, és kattintson a **Nézet topológia**a topológia térkép tekinthet meg. Is megtekinthetők a topológia térkép **elérési út** az irányítópulton lévő csempén gombra kattintva **Elérési út** az irányítópulton kattintáskor fognak rendelkezésére állni jelölje ki a bal oldali panelen a forrás- és céltáblák csomópontok, és kattintson az **ábra** ábrázolandó a leveleket a két csomópontok között.

A topológia térkép jeleníti meg, hány útvonalak is között a két csomópontot, és hogy milyen elérési út az adatok csomagok venni. Hálózati teljesítmény szűk a topológia térképen piros vannak megjelölve. Hibás hálózati kapcsolaton vagy egy hibás hálózati eszköz megtalálja a topológia térképen piros színű elemek megjeleníti.

Kattintáskor a csomópont vagy a hover fölé a topológia térképen, látni fogja a csomópontot, például FQDN és IP-cím tulajdonságait. Kattintson egy azonosítható megtudhatja, IP-címet. Törölje a jelet, majd kiválasztja a emelje ki a térképen útvonalak kiemelheti a adott útvonalak. Nagyíthat és a topológia térkép kicsinyítése a görgetőkerék használatával.

Figyelje meg, hogy a topológia látható a térkép layer 3 topológiája és 2 réteg eszközök és a kapcsolatokkal tartalmaz.

![a azonosítható – Ugrás topológia térkép](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Hibafa honosítási

Hálózati teljesítmény monitorra a hálózati szűk megtalálhatja a hálózati eszközök való csatlakozás nélkül. Gyűjti össze a hálózatról, és a hálózati grafikonon speciális algoritmusok alkalmazásával adatok alapján, hálózati teljesítmény Monitor lehetővé teszi a hálózati részeit, amelyek valószínűleg egy probabilisztikus becslése a hiba forrását.

Ez a módszer akkor hasznos, a hálózati szűk határozza meg, amikor Ugrás eléréséhez nem érhető el, mert nem kell hozzá bármilyen kell a hálózati eszközök, például az útválasztó vagy kapcsoló gyűjtött adatokat. Ez akkor is hasznos, ha az Ugrás két csomópontok között nem szerepelnek a rendszergazdai vezérlőelem. Az Ugrás lehet, például Internetszolgáltató útválasztó.

### <a name="log-analytics-search"></a>Jelentkezzen be a Analytics-keresés

Az összes elérhető grafikusan keresztül a hálózati teljesítmény Monitor irányítópult és Lehatolás oldalak-adatok natív módon a napló Analytics keresési is érhető el. A keresés lekérdezési nyelvet használ-e a lekérdezést, és egyéni jelentések létrehozása az adatok exportálása az Excelbe vagy a PowerBI. A **Közös lekérdezések** fel az irányítópulton tartalmaz néhány hasznos lekérdezések kiindulópontként használhatja a saját lekérdezések és jelentések készítéséhez.

![a keresési lekérdezések](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>A kiváltóok egy állapot riasztás vizsgálata

Most, hogy tekintse át a hálózati teljesítmény Monitor, nézzük meg egy egyszerű vizsgálatot a kiváltóok-állapot esemény.

1. Az Áttekintés oldalon a **Hálózati teljesítmény Monitor** csempe megfigyelésével szerezze be a hálózat állapotát jelző rövid pillanatképét. Figyelje meg, hogy ki a figyelni 80 alhálózatokra hivatkozásokat, 43 is megsérült. Ez a vizsgálat igényli. Kattintson a megoldás irányítópult megtekintése a csempére.  
  ![Hálózati teljesítmény Monitor csempe](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. Az alábbi példa képe Észre fogja venni, hogy vannak-e a 4 állapot események jelenleg és 4 hálózati hivatkozások, amelyek megsérült. Úgy dönt, hogy kivizsgáljuk a probléma, majd kattintson a **Sharepoint-webhelyen** hálózati kapcsolat megtudhatja, hogy a probléma gyökerének.  
  ![sérült hálózati kapcsolat például](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. A Lehatolás lap az összes alhálózat csatolás **Sharepoint-webhelyen** hálózati kapcsolat jeleníti meg. Észre fogja venni, hogy mindkét a alhálózat hivatkozások, a késés van Metszéspont végez a hálózati kapcsolat sérült küszöbértéket. A késés trendek, mind a alhálózat hivatkozások is láthatja. Az idő kijelölt használható vezérlő szükséges időtartomány kiemelése a diagramon. Megtekintheti a nap, amikor a késés megérkezett a csúcs időtartamát. Később is kereshet a naplókat, a következő időszakban jelölőnégyzetet, hogy kivizsgáljuk a probléma. Kattintson a Lehatolás további **Csatolások megtekintése csomópontra** .  
  ![sérült alhálózat hivatkozások példa](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Hasonló az előző oldalra, a Lehatolás lap az adott alhálózat hivatkozás felsorolja a alkotó csomópont hivatkozások lefelé. Hasonló műveletek hajthatók végre itt, mint az előző lépésben. Kattintson a **Nézet topológia** megtekintése a topológia a 2 csomópontok között.  
  ![sérült csomópont hivatkozások példa](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. A 2 kijelölt csomópontok között minden elérési út a topológia térkép vannak ábrázolja. A topológia térképen két csomópontok között útvonalak azonosítható a azonosítható által topológiájának is ábrázolása. Világos képet hány útvonalak közöttük a két csomópontot, és mit kínál fel az adatok csomagok telik elérési utak. Hálózati teljesítmény szűk piros színnel vannak megjelölve. Hibás hálózati kapcsolaton vagy egy hibás hálózati eszköz megtalálja a topológia térképen piros színű elemek megjeleníti.  
  ![Példa a sérült topológia megtekintése](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Az adatvesztés, időtartam és Ugrás az egyes elérési út számát az **Elérési út jobb** oldali ablaktáblán ellenőrizhetőek. Ebben a példában látható, hogy nincsenek 3 sérült elérési utak említett ablakban. A görgetősáv segítségével azok sérült elérési útja részleteinek megtekintését.  A jelölőnégyzetek segítségével válassza ki az egyik javaslatot, hogy csak egy elérési utat a topológia ábrázolva. A görgetőkerék használatával nagyítása és kicsinyítése a topológia térképen.

  Kattintson a kép alatt egyértelműen megjelenik a probléma területeket a hálózat adott szakaszához kiváltóok megjeleníti az elérési út és Ugrás a vörös színt. Kattintás a topológia térképen csomóponton megjelennek a csomópontot, többek között a teljesen minősített tartománynév tulajdonságainak és IP-címét. Az Ugrás IP-címe egy azonosítható a gombra kattintva jeleníti meg.  
  ![sérült topológia – elérési út részletek példa](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Következő lépések

- [Keresés naplók](log-analytics-log-searches.md) részletes hálózati teljesítmény adatok rekordjainak megtekintéséhez.
