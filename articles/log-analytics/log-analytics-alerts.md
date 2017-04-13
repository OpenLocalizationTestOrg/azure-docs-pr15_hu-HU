<properties 
   pageTitle="Log Analytics riasztásai |} Microsoft Azure"
   description="Log Analytics riasztásai fontos információkat a MOBILE adattárban azonosítása és is ezzel kapcsolatban beérkező értesítést szeretne problémák vagy meghívása műveletek, próbálja meg javítani őket.  Ez a cikk ismerteti, hogy miként hozhat létre egy szabályt, és a részletek a különböző műveleteket azokat is."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/22/2016"
   ms.author="bwren" />

# <a name="alerts-in-log-analytics"></a>Log Analytics riasztásai

Log Analytics riasztásai azonosítsa a MOBILE adattárban fontos információkat.  Riasztási szabályok automatikusan napló keresések ütemezés szerint futtatni, és hozzon létre egy figyelmeztető rekordot, ha az eredményeket az adott feltételeknek megfelelő.  A szabály majd futtatható automatikusan ezzel kapcsolatban beérkező értesítést szeretne az értesítésre, vagy egy másik folyamat meghívása egy vagy több műveletet.   

![Jelentkezzen be a Analytics-értesítések](media/log-analytics-alerts/overview.png)

## <a name="creating-an-alert-rule"></a>Egy szabályt létrehozása
Hozzon létre egy szabályt, hogy esetén létrehozásával kell kezdenie napló kereshet az Office.com rekordokat kell elindítani az értesítésre.  Az **értesítés** gomb majd érhetők el, hozzon létre, és állítsa be a szabályt.

1.  A MOBILE áttekintése lapon kattintson a **Log keresés**gombra.
2.  Hozzon létre egy új napló keresési lekérdezést, vagy jelölje ki a mentett naplót keresés. 
3.  Kattintson a **figyelmeztető** elemre a lap tetején az **Értesítési szabály hozzáadása** képernyő megnyitásához.
4. Olvassa el az alábbi táblázatokban további információt az értesítés beállítása az beállításai.
5. Az időkeret a figyelmeztetést ad, amikor a keresési feltételeknek megfelelő idő ablak meglévő rekordok számát jelennek meg.  Ez segíthetnek annak eldöntésében, hogy a gyakoriság, amely a képet ad a várt eredmények számát.
4.  Kattintson a **Mentés** elvégzéséhez a szabályt.  Futtatása azonnal elindul.


![Riasztási szabály hozzáadása](media/log-analytics-alerts/add-alert-rule.png)

| A tulajdonság | Leírás |
|:--|:--|
| **Értesítés adatainak** | |
| név |  Egyedi nevet, amely azonosítja a szabályt. |
| Súlyosságát | Az értesítés, ez a szabály által létrehozott súlyosságát. |
| Keresési lekérdezés | Jelölje be az **aktuális keresési lekérdezés** az aktuális lekérdezés használni, vagy válassza ki a korábban mentett keresési a listából.  A lekérdezés szintaxisát kapja a szövegmezőbe, amelyekkel módosítható, ha szükséges.  |
| Időkeret | Adja meg a lekérdezés időtartomány.  A lekérdezés csak a pontos idő ebben a tartományban létrehozott rekordokat adja vissza.  Ez lehet az 5 perc és a 24 óra közötti bármely értéket.  Nagyobb vagy egyenlő az értesítés ismétlődési kell.  <br><br> Ha például a időkeret értéke 60 perc, és a lekérdezés futtatása a 1:15 du, ha csak azok a rekordok között a 12:15 du és 1:15 du létrehozott visszatér. |
| **Ütemterv** |     
| Küszöbérték | Mikor érdemes létrehozni egy figyelmeztetés feltételek.  Értesítés hoz létre, ha a lekérdezés által visszaadott rekordok száma megegyezik a megadott feltételnek. |
| Értesítések gyakorisága | Itt adhatja meg, hogy milyen gyakran kell futtatni a lekérdezést.  Bármilyen érték 5 percig tart, és a 24 óra közötti lehet.  Egyenlő vagy kisebb, mint a időkeret kell lennie. |
| Értesítések mellőzése | Ha bekapcsolja a riasztási szabály Tűzelfojtó, a műveletek a szabály egy meghatározott ideig új értesítés létrehozása után le vannak tiltva.  A szabály továbbra is működik, és riasztási rekordot hoz létre, ha teljesül a feltétel.  Ez a probléma megoldására ismétlődő tevékenységek futtatása nélkül időzítése engedélyezése. |
| **Műveletek** | |
| Értesítő e-mailt | Adja meg a **Igen** , ha azt szeretné, hogy egy e-mailt küldeni, amikor a figyelmeztetés jelenik meg. |
| Tárgy    | Tárgy az e-mailben.  Az üzenet törzsében nem módosíthatók. |
| A címzettek | Az összes e-mail címzettek címét.  Ha egynél több címet ad meg, majd a címeket egymástól pontosvesszővel (;). |
| Webhook | Ha fel szeretne hívni egy webhook, amikor a figyelmeztetés jelenik meg, válassza az **Igen** . |
| Webhook URL-címe | A webhook URL-CÍMÉT. |
| Egyéni JSON-tartalom felvétele | Akkor válassza ezt a lehetőséget, ha azt szeretné, hogy az alapértelmezett tartalom lecserélése egy egyéni tartalom. |
| Írja be az egyéni JSON-tartalom | A webhook az egyéni tartalom.  Részletek előző szakaszban olvashat. |
| Runbook | Ha szeretne kezdeni az Azure automatizálási runbook, amikor a figyelmeztetés jelenik meg, válassza az **Igen gombra** . |
| Jelölje ki a runbook | Jelölje be az automatizálási konfigurálva a automatizálási megoldás a fiók runbooks kiinduláshoz a runbook. |
| Futtatása | Jelölje ki az **Azure** futtatásához a runbook Azure a felhőben.  Jelölje ki a **Hibrid dolgozó** futtatni szeretne a runbook a [Hibrid Runbook dolgozó](..\automation\automation-hybrid-runbook-worker.md) a helyi környezetben. |


## <a name="manage-alert-rules"></a>A figyelmeztetési szabályok kezelése
Log Analytics **Beállítások** **riasztások** menüjében elérheti összes riasztási szabályok listáját.  

![Értesítések kezelése](./media/log-analytics-alerts/configure.png)

1. A MOBILE konzolban válassza a **Beállítások** csempére.
2. Válassza a **riasztások**lehetőséget.

Több műveleteket hajthatja végre a nézetből.

- Tiltsa le a szabály **melletti ki** .
- Mellette lévő ceruza ikonra kattintva szerkesztheti egy szabályt.
- Távolítsa el egy szabályt melletti **X** ikonra. 


## <a name="setting-time-windows"></a>A windows idő beállítása 

### <a name="event-alerts"></a>Esemény értesítések

Események olyan adatforrásokhoz, például a Windows eseménynaplók Syslog, és egyéni jelentkezik.  Érdemes értesítést kap egy adott hiba esemény létrehozásakor, vagy ha létrehozott egy adott időpont ablakon belül több hibák események hozhat létre.

Egyetlen olyan esemény értesítést, hogy állítsa megjelenő találatok számának nagyobb, mint a 0 és a gyakoriság és az 5 perc időkeret.  Hogy futtassa a lekérdezést 5 percenként, és keressen egy egyszeri esemény, a lekérdezés futtatásának utolsó óta létrehozott.  Egy hosszabb gyakoriság elhalaszthatja az esemény gyűjtenek és a figyelmeztetés eredményezne között eltelt idő.

Egyes alkalmazások feltétlenül kerülni a Webhelyfiókok előléptetése jelzést alkalmi hibát jelentkezhetnek be.  Az alkalmazás például újra a folyamatot a hibák esemény létrehozott és a következő alkalommal majd sikeres.  Ebben az esetben nem érdemes hozni az értesítést, kivéve, ha több események létrehozott egy adott időpont ablakon belül.  

Egyes esetekben előfordulhat, hogy kívánt értesítés létrehozása hiányában az esemény.  Ha például egy folyamat előfordulhat, hogy naplózása normál jelzi, hogy megfelelően működik-e.  Ha egy ilyen eseményt, egy adott időpont ablakon belül nem jelentkezik, egy figyelmeztetés kell létrehozni.  Ebben az esetben akkor értékre kell állítania a küszöbértékét *kisebb, mint 1*.

### <a name="performance-alerts"></a>Teljesítmény értesítések

[A teljesítményadatok](log-analytics-data-sources-performance-counters.md) események hasonló MOBILE tárban tárolnak rekordként tárolja.  Az egyes rekordok értéke az átlag felett az előző 30 percben mérhető.  Ha szeretne értesítést, amikor a teljesítmény számláló eléri az adott, majd küszöbértéket szerepelnie kell a lekérdezésben.

Ha például a processzor fut, amikor a felhasználó megy végbe 30 percig 90 %-nál használja például a lekérdezés *típus = Perf objektumnév processzor CounterName = = "processzor %" ellenértéknek > 90* és a szabályt, a *0-nál nagyobb*küszöbértékét.  

 Mivel a [Teljesítmény rekordok](log-analytics-data-sources-performance-counters.md) 30 percenként, függetlenül attól, hogy minden számláló gyűjtse össze a gyakoriság vannak összesíti, kisebb, mint 30 perc időkeret esetleg egy rekord sem.  Az időkeret beállításának 30 percig biztosítása egyetlen olyan rekordot beszerzése minden csatlakoztatott forrást, amely az átlag, hogy az idő múlásával.

## <a name="alert-actions"></a>Riasztási műveletek

Az értesítés rekord létrehozásán beállíthatja az egy vagy több műveletet automatikusan futtatásához a szabályt.  Műveletek ezzel kapcsolatban beérkező értesítést szeretne az értesítésre, vagy meghívása néhány folyamat, próbálja meg, hogy az észlelt probléma megoldására.  Az alábbi szakaszok ismertetik a jelenleg elérhető műveletek.

### <a name="email-actions"></a>E-mailek műveletek
E-mail műveletek a figyelmeztetés az adataival e-mail küldése egy vagy több címzettnek.  Megadhatja, hogy az e-mail tárgya, de most tartalmat egy szabványos formátum állít elő napló Analytics.  Az értesítés mellett legfeljebb 10 rekordot a napló keresés által visszaadott részleteit, például a nevet az összegzett adatokat tartalmazza.  A napló Analytics, amely a teljes rekordhalmaz ad vissza, amelyek lekérdezik az napló a keresés hivatkozást is tartalmaz.   A levelek, a feladó *Microsoft műveletek Management csomagja csapata &lt; noreply@oms.microsoft.com *. 


### <a name="webhook-actions"></a>Webhook műveletek

Webhook műveletek meghívása egy külső folyamat egyetlen HTTP POST kérelem keresztül teszi lehetővé.  A szolgáltatás a hívott kell webhooks támogatja, és meghatározza, hogy hogyan használ minden olyan tartalom kap.  Hívhatja a REST API-t nem támogató kifejezetten webhooks mindaddig, amíg a kérést, amelyet az API értelmez formátumban is.  Értesítés válaszként egy webhook példák az értesítés, illetve egy szolgáltatásba, például [PagerDuty](http://pagerduty.com/)létrehozásáról az esemény részleteit tartalmazó üzenet küldése egy szolgáltatásba, például [tartalékidő](http://slack.com) használják.  

A teljes áttekintése a következő egy webhook hívja fel a minta szolgáltatás egy figyelmeztető szabály létrehozása a [Webhooks a napló Analytics-értesítések](log-analytics-alerts-webhooks.md)címen érhető el.

Webhooks közé tartozik, az URL-CÍMEK és a formázott, amely a külső szolgáltatásnak küldött adatok JSON hasznos.  Alapértelmezés szerint a tartalom az alábbi táblázat tartalmazza a értékeket fogja.  Megadhatja, hogy a tartalom lecserélése saját egyéni egy.  Ebben az esetben használhatja a változók a táblázat minden egyes paraméterek értékük szerepeltetni az egyéni tartalom.


| Paraméter | Változó | Leírás |
|:--|:--|:--|
| AlertRuleName | #alertrulename | A riasztási szabály nevét. |
| AlertThresholdOperator | #thresholdoperator | A riasztási szabály operátor küszöbértéknél.  *Nagyobb* vagy *kisebb, mint*. |
| AlertThresholdValue | #thresholdvalue | Küszöbértéke a szabályt. |
| LinkToSearchResults | #linktosearchresults | Hivatkozás napló Analytics napló keresési, amely a lekérdezést, amely az értesítés létrehozása a rekordok vissza. |
| ResultCount  | #searchresultcount | A keresési eredmények között a rekordok számát. |
| SearchIntervalEndtimeUtc  | #searchintervalendtimeutc | A lekérdezés UTC formátumban befejezési idő. |
| SearchIntervalInSeconds | #searchinterval | A figyelmeztetési szabály időkeret. |
| SearchIntervalStartTimeUtc  | #searchintervalstarttimeutc | Indítsa el a lekérdezés idő UTC formátumban. |
| SearchQuery | #searchquery | Log kifejezést használja a szabályt. |
| SearchResults | Lásd az alábbi | A rekordok JSON formátumban a lekérdezés által visszaadott.  Az első 5000 rekordok korlátozva. |
| WorkspaceID | #workspaceid | A MOBILE munkaterület azonosítója. |


Például előfordulhat, hogy adja meg az alábbi egyéni tartalom, amely tartalmazza a *szöveg*egyetlen paramétert.  A szolgáltatás, amely a webhook hív meg ezt a paramétert szeretne kell meglepő.

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }

Ez a példa tartalom volna oldja fel a következő, amikor elküldi a webhook hasonló.

    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }

Szeretne felvenni a keresési eredmények egyéni hasznos, a következő sor hozzáadása a json-tartalom a legfelső szintű tulajdonság  

    "IncludeSearchResults":true

Ha például szeretne létrehozni, amely magában foglalja a felhasználó nevét, és a keresési eredmények egyéni hasznos, használhatja is a következő. 

    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }


Akkor is végigvezetik teljes példája egy figyelmeztető szabály létrehozása egy webhook egy külső szolgáltatás, a [naplófájl Analytics riasztást webhook minta](log-analytics-alerts-webhooks.md)indításához.

### <a name="runbook-actions"></a>Runbook műveletek

Runbook műveletek az Azure automatizálás indítsa el a runbook.  A művelet a következő típusú használatához, telepítette és beállította a MOBILE munkaterületen az [automatizálási megoldás](log-analytics-add-solutions.md) kell rendelkeznie.  Ha nincs telepítve van, amikor létrehoz egy új szabályt, a telepítés mutató hivatkozás jelenik meg.  Az automatizálási megoldásban konfigurált automatizálási fiók runbooks közül választhat.

Runbook műveletek indítsa el a runbook egy [webhook](../automation/automation-webhooks.md)használatával.  A szabályt létrehozásakor automatikusan hoz létre egy új webhook a runbook az **MOBILE értesítés Remediation** követ GUID nevű.  

Nem közvetlenül elhelyezte a runbook a paramétereket, de a [$WebhookData paraméter](../automation/automation-webhooks.md) tartalmazni fogja a riasztást, beleértve az azt létrehozó napló keresési eredmények részleteit.  A runbook kell ahhoz, hogy a figyelmeztető tulajdonságait paraméter **$WebhookData** meghatározásával.  A riasztási adatok egyetlen tulajdonság neve **SearchResults** **$WebhookData** **RequestBody** tulajdonságlapján a json formátumban érhető el.  Ez lesz, az a tulajdonságokat az alábbi táblázatban.


| Csomópont | Leírás |
|:--|:--|
| azonosító         | Elérési utat és a keresési globálisan egyedi azonosítója. |
| __metadata | Információ arról, hogy az értesítésre, beleértve a rekordok számát és a keresési eredmények állapotát. |
|  érték     |  A keresési eredmények között a rekordokat külön bejegyzés.  A tulajdonságok és a rekord értékek oszlopnevek meg fognak egyezni a bejegyzés részleteit.   |

Például a következő runbook szeretné bontsa ki a rekordok, a naplófájl keresés által visszaadott és hozzárendelése az egyes rekordok függően különböző tulajdonságokat.  Látható, hogy a runbook **RequestBody** konvertálása json, így akkor is kell időadatok használata a PowerShellben objektumként indítja el.

    param ( 
        [object]$WebhookData
    )

    $RequestBody = ConvertFrom-JSON -InputObject $WebhookData.RequestBody
    $Records     = $RequestBody.SearchResults.value
    
    foreach ($Record in $Records)
    {
        $Computer = $Record.Computer
        
        if ($Record.Type -eq 'Event')
        {
            $EventNo    = $Record.EventID
            $EventLevel = $Record.EventLevelName
            $EventData  = $Record.EventData
        }
        
        if ($Record.Type -eq 'Perf')
        {
            $Object    = $Record.ObjectName
            $Counter   = $Record.CounterName
            $Instance  = $Record.InstanceName
            $Value     = $Record.CounterValue
        }
    }


## <a name="alert-records"></a>Riasztási rekordok

Log Analytics riasztási szabályok által létrehozott riasztási rekordok **értesítés** **típusát** , és egy **SourceSystem** a **MOBILE**van.  Az tulajdonságokat az alábbi táblázat rendelkeznek.

| A tulajdonság | Leírás |
|:--|:--|
| Típus          | *A felhasználó* |
| SourceSystem  | *MOBILE* |
| AlertSeverity | A figyelmeztetés szintje súlyosságát. |
| AlertName     | A riasztás nevét. |
| Lekérdezés         | Futtatott lekérdezési szöveg.  |
| QueryExecutionEndTime   | A lekérdezés időtartomány végére. |
| QueryExecutionStartTime | A lekérdezés időtartomány annak kezdete.  |
| TimeGenerated | Dátum és idő a figyelmeztető hozták létre. |

Vannak egyéb típusú riasztási bejegyzéseket és [a Power BI exportálja](log-analytics-powerbi.md)a [riasztás-kezelési megoldás](log-analytics-solution-alert-management.md) által létrehozott.  Az ilyen összes lehet **típus** **riasztási** , de azok **SourceSystem**alapján különböztethető.




## <a name="next-steps"></a>Következő lépések

- A [riasztási projektmenedzsment megoldás](log-analytics-solution-alert-management.md) elemzése a létrehozott naplófájl Analytics együtt a System Center műveletek Manager (SCOM) gyűjtött értesítések riasztások telepítése.
- További információ a [napló keresések](log-analytics-log-searches.md) riasztások hozhat létre.
- [Egy webook beállítása](log-analytics-alerts-webhooks.md) egy szabályt az útmutató elvégzéséhez.  
- Megtudhatja, hogy miként írja be [az Azure automatizálás runbooks](https://azure.microsoft.com/documentation/services/automation) való ismételt riasztások jelölt problémákat.