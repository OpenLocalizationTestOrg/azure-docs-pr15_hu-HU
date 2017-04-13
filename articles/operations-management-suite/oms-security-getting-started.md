<properties
   pageTitle="Tevékenységek kezelése csomagja biztonsági és naplózási megoldás első lépések |} Microsoft Azure"
   description="A dokumentum első lépések a tevékenységek kezelése csomagja biztonsági és naplózási megoldáskészítő lehetőségek a Lync-a hibrid felhő nyújt segítséget."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="get-started-article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="yurid"/>
 
# <a name="getting-started-with-operations-management-suite-security-and-audit-solution"></a>Tevékenységek kezelése csomagja biztonsági és naplózási megoldás első lépések
Első lépések gyors műveletek Management csomagja (MOBILE) és a naplózási megoldáskészítő lehetőségek létrehozásában az egyes beállítások ehhez a dokumentumhoz segítségével.

## <a name="what-is-oms"></a>Mi az MOBILE?
Microsoft műveletek Management csomagja (MOBILE) használata a Microsoft felhőalapú IT kezelési megoldás, amelyek segítségével kezelése és védelme a helyszíni és felhőbeli infrastruktúra. MOBILE kapcsolatos további tudnivalókért olvassa el a cikk [Műveletek Management csomagja](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="oms-security-and-audit-dashboard"></a>MOBILE és a naplózási irányítópult

A MOBILE és a naplózási megoldást be a szervezet átfogó képet ad az informatikai biztonsági testtartását a főbb problémákat, a figyelmet igénylő beépített keresési lekérdezések. A **Biztonság és naplózási** irányítópult a kezdőképernyőn mindenre kapcsolatos biztonsági a MOBILE. A számítógépek biztonsági állapotát magas szintű betekintést nyújt. Az elmúlt 24 óra napján, az összes eseményének vagy bármely más egyéni időkereten megtekintése is tartalmaz. A **Biztonság és naplózási** irányítópult elérésére, kövesse az alábbi lépéseket:

1. Kattintson a **Microsoft műveletek Management Suite** fő irányítópult **beállításai** lapon, a bal oldalon található.
2. A **Beállítások** lap, a **megoldások** csoportban **biztonsági és naplózási** lehetőséget.
3. A **Biztonság és naplózási** irányítópult jelennek meg:

    ![MOBILE és a naplózási irányítópult](./media/oms-security-getting-started/oms-getting-started-fig1-ga.png)

Ha először az irányítópult érik el, és nincs eszközök MOBILE felügyeli, a csempék nem tölti fel a agent kapott adatok. A ügynököt eltarthat egy kis időt feltölteni, ezért megjelenő kezdetben hiányozhatnak bizonyos adatok, továbbra is feltöltése a felhőben.  Ebben az esetben célszerű normál néhány csempék tárgyi eszközök adatai nélkül. Olvassa el a [közvetlenül a számítógépek csatlakoztatása a Windows MOBILE](https://technet.microsoft.com/library/mt484108.aspx) MOBILE ügynök telepítése a Windows rendszert és [Csatlakozás Linux számítógépek MOBILE](https://technet.microsoft.com/library/mt622052.aspx) további információt olvashat a feladat végrehajtásához Linux rendszerben További információt.

> [AZURE.NOTE] A agent összegyűjti az adatokat, az aktuális eseményeket, hogy engedélyezve vannak, például a számítógép neve, IP-cím és a felhasználó neve alapján. Azonban nem dokumentumfájlok /, az adatbázis neve vagy a személyes adatok összegyűjtése.   

Megoldások egy szabálygyűjteményt logika, a képi megjelenítések és az adatok beszerzése a címet a kulcsfontosságú felhasználói kihívásokkal kapcsolatban. Biztonság és naplózási egyik megoldás, mások külön kell hozzáadnia. Olvassa el a cikk [hozzáadása megoldások](https://technet.microsoft.com/library/mt674635.aspx) , hogy miként vehet fel egy új megoldás további információt.

A MOBILE és a naplózási irányítópult négy fő kategóriába rendezett:

- **Biztonsági tartományok**: ezen a területen fogja tudni további feltárása biztonsági rekordok adott idő alatt, kártevőt értékelési elérni, értékelése és a hálózati biztonsági, a identitás- és az access adatait, a számítógépen biztonsági események frissítése és gyorsan hozzáférhet az Azure biztonsági központ irányítópult.
- **Főbb problémákat**: Ez a beállítás lehetővé teszi a gyors azonosítását az aktív problémák számát, és ezeket a problémákat súlyosságát.
- **Az észlelési (előzetes verzió)**: lehetővé teszi, hogy a biztonsági figyelmeztetések megjelenítése, ahogy azok helyébe szemben az erőforrások által homonimaszerű webcímmel azonosításához.
- **Veszély intelligencia**: lehetővé teszi, hogy homonimaszerű webcímmel megjelenítése a kimenő rosszindulatú IP-forgalmat, a kártékony veszély, és egy térképen, amely mutatja az alábbi IP-címei származnak amennyiben kiszolgálók száma szerint azonosításához. 
- **Közös biztonsági lekérdezések**: ezt a lehetőséget biztosít a leggyakoribb biztonsági lekérdezések figyelheti a környezet használó listáját. Ha egy adott lekérdezések gombra kattint, a **Keresés** lap társítás meg, hogy a lekérdezés eredményét.

> [AZURE.NOTE] Hogyan MOBILE továbbra is az adatok biztonságos kapcsolatos további tudnivalókért olvassa el a hogyan MOBILE védelemmel látja el az adatok.

## <a name="security-domains"></a>Biztonsági tartományok

Figyelése erőforrásokat, amikor célszerű lehet gyorsan hozzáférhet a környezet aktuális állapotát. Azonban fontos is engedélyezni szeretné, hogy a Mi történik az egyes szakaszokban környezetben időpont jobb megértése vezethet múltbeli vissza események nyomon követéséhez. 

> [AZURE.NOTE] Adatmegőrzés alapján történik a MOBILE csomagot árak. A [Microsoft műveletek Management Suite](https://www.microsoft.com/server-cloud/operations-management-suite/pricing.aspx) árak, oldal tudhat meg többet.

Az esemény válasz és forensics vizsgálatot felhasználási területei közvetlenül előnyei a rendelkezésre álló **Biztonsági rekordok időbeli** csempéjén közül.

![Biztonsági rekordok adott idő alatt](./media/oms-security-getting-started/oms-getting-started-fig2.JPG)

Ez a csempe kattint, a **Keresés** lap megnyílik, rajta a lekérdezés eredményét **Biztonsági eseményekre** vonatkozóan: (típus = SecurityEvents) az adatok alapján az alább látható módon az utóbbi hét napban:

![Biztonsági rekordok adott idő alatt](./media/oms-security-getting-started/oms-getting-started-fig3.JPG)

A találatok között a következő két ablaktáblát oszlik: a bal oldali ablaktáblában lépve részletezzük, hogy a található, a számítógépek, amelyben az események talált, ezek a számítógépek felismert fiókok számát és a tevékenységtípusok biztonsági események száma. A jobb oldali itt az összes eredményt és a biztonsági eseményeket a számítógép neve és esemény tevékenységeket használó időrendi nézetének. Választhatja **A több megjelenítése** többet szeretne tudni az eseményt, például az esemény adatai, az esemény azonosító és a forrás megtekintése.

> [AZURE.NOTE] MOBILE keresési lekérdezés kapcsolatos további tudnivalókért olvassa el a [MOBILE keresése hivatkozást](https://technet.microsoft.com/library/mt450427.aspx).

### <a name="antimalware-assessment"></a>Antimalware értékelése

Ez a beállítás lehetővé teszi, hogy gyors azonosítását számítógépek nem megfelelő védelmet és a számítógépen, amely szerint kártevőt valamilyen hordoznak. Kártevőt értékelési állapot és a megfigyelt kiszolgálókon észlelt veszélyek olvasni, és kattintson az adatokat küldi a MOBILE szolgáltatás feldolgozásra a felhőben. Kiszolgálók előforduló érhető el a **Antimalware értékelési** mozaik gombra kattintás után kártevőt értékelési irányítópult látható veszélyek és kiszolgálók és a nem megfelelő védelmet. 

![kártevőt értékelése](./media/oms-security-getting-started/oms-getting-started-fig4-ga.png)

Mint bármely más élő csempe MOBILE irányítópult érhető el akkor kattintson rá, a **Keresés** lap megnyílik lekérdezés eredményét. Ezt a lehetőséget választja Ha gombra kattint a **Védelem állapota**szakaszban **Nem jelentése** lehetőséget választania kell a lekérdezés eredménye, amely mutatja Ez egy bejegyzés, amely tartalmazza a számítógép neve és a rangját, az alább látható módon:

![keresési eredmény](./media/oms-security-getting-started/oms-getting-started-fig5.png)

> [AZURE.NOTE] *rangsor* egy értékelést megfeleljen a védelmet állapotának biztosítva (a, kikapcsolásához frissíteni, stb) és veszélyek található. Összesítések egy szám biztosíthatja, hogy, amely.

Ha a számítógép neve gombra kattint, akkor ezen a számítógépen a védelem állapota időrendi nézete. Ez nagyon hasznos, ha olyan esetek, amelyek megérthető, ha a antimalware egyszer telepítve volt szükség, és bizonyos pontján el lett távolítva.   

### <a name="update-assessment"></a>Felmérés módosítása 

Ez a beállítás lehetővé teszi, hogy gyorsan alapján megállapíthatja, hogy a teljes megvilágítás potenciális biztonsági problémákat, és kell-e, illetve hogyan fontos frissítések környezetben. MOBILE és a naplózási megoldás csak a frissítések telepítése a megjelenítés, a [Rendszer frissítések megoldások](https://technet.microsoft.com/library/mt484096.aspx), amely egy másik modulra MOBILE alkalmazásból származik, a tényleges adatok. Az alábbi példa a frissítések:

![rendszer-frissítések](./media/oms-security-getting-started/oms-getting-started-fig6.png)

> [AZURE.NOTE] Frissítések megoldás kapcsolatos további tudnivalókért olvassa el [a rendszer frissítéseit megoldásban kiszolgálók frissítése](https://technet.microsoft.com/library/mt484096.aspx).

### <a name="identity-and-access"></a>Identitás- és hozzáférés

Identitás kell lennie a vezérlő sík a nagyvállalati, az identitás védelme kell lennie a felső prioritását. Miközben múltbeli nincsenek kialakítását szervezetek köré, és ezeket kialakítását azok közül az elsődleges idejű határai, ma több adatot és a további alkalmazások áthelyezés a felhőbe azonosítója lesz új pereme. 

> [AZURE.NOTE] jelenleg az adatok alapján csak biztonsági események bejelentkezési adatok (esemény azonosító 4624) a jövőbeli Office 365-ben bejelentkezések, és Azure Active Directory-adatok is szerepelnek.

A személyes tevékenységek megfigyelésével fogja tudni megelőző műveletekhez esemény helyre vagy leállítása homonimaszerű webcímmel kísérlet reaktív műveletek végrehajtása előtt. Az **identitás- és az Access** irányítópult áttekintése, az Identitáskezelés állapotát, köztük a sikertelen kísérletek jelentkezzen be a, ezeket kísérletek, a, hogy azok zárolt fiókok, a fiókok során használt a felhasználó fiókjának módosultak, vagy alaphelyzetbe jelszavát, és jelenleg, hogy vannak-e jelentkezve fiókok számát. 

Az **identitás- és az Access** csempére kattintva az alábbi irányítópult jelennek meg:

![identitás- és hozzáférés](./media/oms-security-getting-started/oms-getting-started-fig7-ga.png)

A rendelkezésre álló az irányítópulton szereplő információk azonnal segítséget azonosítása egy potenciális gyanús tevékenységet. Például vannak követve jelentkezzen be **rendszergazdaként** , és a következő kísérletek 100 %-os 338 kísérletek sikertelen volt. Ez a fiók támadások ellen okozhatja. Ha ez a fiók parancsára kattint, további információt, amely segíthet alapján megállapíthatja, hogy a célhely erőforrás a potenciális támadásokat fog kapja:

![keresési eredmények](./media/oms-security-getting-started/oms-getting-started-fig8.JPG)

A részletes jelentés ebben az esetben fontos információkat tartalmaz együtt: a Céllista számítógépet, a bejelentkezési (a Ez esetben hálózati bejelentkezési), a tevékenység (Ez esetben az esemény 4625) és az egyes kísérletek átfogó ütemterv a típusát. 

### <a name="computers"></a>Számítógépek

Ez a csempe biztonsági események aktívan rendelkező összes számítógépre történő elérésére használható. Ha ez a csempe parancsra jelennek meg a számítógépen biztonsági események és események száma listáját összes olyan számítógépen:

![Számítógépek](./media/oms-security-getting-started/oms-getting-started-fig9.JPG)

Továbbra is a vizsgálat kattintva az összes olyan számítógépen, és tekintse át a biztonsági eseményeket lettek megjelölve.

### <a name="azure-security-center"></a>Azure biztonság otthon

Ez a csempe tulajdonképpen egy helyi Azure biztonsági központ irányítópult eléréséhez. Olvassa el az [Ismerkedés az Azure biztonság otthon](../security-center/security-center-get-started.md) további információt a megoldást.

## <a name="notable-issues"></a>Főbb kapcsolatos problémák

A beállítások csoport a fő célnak, hogy a problémákat, amelyek szerint kategorizálásához őket a kritikus, figyelmeztetés és tájékoztatások van a környezetben, gyors áttekintést nyújt. Az aktív probléma típus csempére adatábrázolás ezeket a problémákat, de még nem engedélyezi a velük kapcsolatos részletekért feltárása az, hogy szeretné-e használni, amelynek a probléma (név) nevére, hány objektumok volna ez fordulhat elő, (száma), és hogyan kritikus (frissítés) Ez a csempe alsó részén.

![Főbb kapcsolatos problémák](./media/oms-security-getting-started/oms-getting-started-fig10.JPG)

Láthatja, hogy a megerősíti ebben a nézetben szándékának **Tartományok biztonsági** csoport különböző területein volt már alá ezeket a problémákat: ábrázolása a legfontosabb problémákat egy helyről a környezetben.

## <a name="detections-preview"></a>Az észlelési (előzetes verzió)

Ezzel a lehetőséggel a fő célnak engedélyezni az IT-keresztül környezetükben és az e potenciális veszélyek gyors azonosítását.

![Intel veszélyt](./media/oms-security-getting-started/oms-getting-started-fig12.png)

Ezt a beállítást is használható az esemény válasz vizsgálat alatt végezze el a felmérés és a homonimaszerű webcímmel bővebb tájékoztatást kaphat.

> [AZURE.NOTE] További információt a MOBILE használatát az esemény választ nézze meg [hogyan használhatja az Azure biztonság otthon és a Microsoft műveletek Management Suite az esemény választ](https://channel9.msdn.com/Blogs/Taste-of-Premier/ToP1703).

## <a name="threat-intelligence"></a>Veszélyt intelligenciával kapcsolatos funkciók

Az új veszélyt intelligencia szakaszt a biztonság és naplózási megoldás megjeleníti a lehetséges támadások mintázatok többféleképpen: az összes kimenő rosszindulatú IP-forgalmat, a kártékony veszélyt típusát, és egy térképen, amely mutatja az alábbi IP-címei származnak amennyiben kiszolgálók számát. A térkép másokkal, és kattintson a további információt az IP-címei.

A térképen sárga rajzszögek azt jelzik, hogy rosszindulatú IP-címei érkező forgalmat. Nem, hogy a program csak akkor érhető el az interneten keresztül lásd: a bejövő rosszindulatú forgalom kiszolgálók ritka, de azt ajánljuk, hogy az egyik sem sikerült a következő kísérletek áttekintése. Hibajelzők IIS naplók, WireData alapulnak, és a Windows tűzfal naplózza.  

![Intel veszélyt](./media/oms-security-getting-started/oms-getting-started-fig11-ga.png)

## <a name="common-security-queries"></a>Közös biztonsági lekérdezések

A rendelkezésre álló közös biztonsági lekérdezések listáját akkor lehet hasznos gyors elérése az erőforrás adatainak és testre szabhatja a környezet igényei alapján. Ezek a közös lekérdezések a következők:

- Az összes biztonsági tevékenységek
- Biztonsági tevékenységek azon a számítógépen "computer01.contoso.com" (a saját számítógép neve a csere)
- A számítógépen "computer01.contoso.com", "Rendszergazda" (a saját számítógépen és a fiók nevét a csere) fiók biztonsági tevékenységek
- Jelentkezzen be a számítógép tevékenység
- Fiókok, akik Microsoft antimalware bármely olyan számítógépen megszakítása
- Ha a Microsoft antimalware folyamat meg lett szakítva számítógépeken
- Számítógépek, ahol a "hash.exe" volt végrehajtása (cseréje másik folyamat nevű)
- Az összes folyamat neveket, amelyeket végrehajtva
- Jelentkezzen be a fiók tevékenység
- A számítógépen "computer01.contoso.com" (a saját számítógép neve a csere) távolról bejelentkezett fiókok

## <a name="see-also"></a>Lásd még:

A dokumentum MOBILE és a naplózási megoldássá hozták be. MOBILE biztonsággal kapcsolatos további információért olvassa el a következő cikkeket:

- [Tevékenységek kezelése csomagja (MOBILE) – áttekintés](operations-management-suite-overview.md)
- [Figyelés és a biztonsági figyelmeztetések válaszol a műveletek Management csomagja biztonsági és naplózási megoldás](oms-security-responding-alerts.md)
- [Tevékenységek kezelése csomagja biztonsági és a naplózási megoldás erőforrások ellenőrzése](oms-security-monitoring-resources.md)
