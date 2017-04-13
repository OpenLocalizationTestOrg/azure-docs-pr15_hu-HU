<properties
   pageTitle="Ismerkedjen meg és egy BizTalk szabályok API-alkalmazás létrehozása a logika alkalmazásban |} Microsoft Azure"
   description="Ez a témakör bemutatja BizTalk szabályok connector-funkciók, és való használatát ismertető"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="anuragdalmia"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/20/2016"
   ms.author="andalmia"/>

#<a name="biztalk-rules"></a>BizTalk szabályok

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

Az üzleti szabályok beágyazza a házirendek és vannak, amelyek üzleti folyamatok döntéseket. Ezek a házirendek hivatalos határozható eljárás kézikönyvek, szerződéseket vagy rendelkezést, vagy előfordulhat, hogy létezik Tudásbázis vagy alkalmazottak foglalt szakértelmét. Ezek a házirendek dinamikus és fölé változhatnak idő változások miatt a vállalati verziós csomagok egyikére, előírásokat vagy más oka lehet.

Ezek a házirendek végrehajtási hagyományos programozási nyelven jelentős idő és effektusával van szükség, és nem engedélyezi a nem programozók létrehozása és fenntartása üzleti házirendek részt. BizTalk üzleti szabályok gyorsan ezekről az irányelvekről végrehajtása, valamint a többi az üzleti folyamat decouple lehetőséget nyújt. A módosítások szükséges vállalati házirendek nélkül az üzleti folyamat a többi érintő tesz lehetővé.

##<a name="why-rules"></a>Miért szabályok

Vannak 3 fő érvek BizTalk üzleti szabályok használatára üzleti folyamat:

* Üzleti logikai funkcióinak alkalmazás kódból decouple
- Lehetővé teszi, hogy az üzleti logikai kezelése szabályozhatósága üzleti elemzők
+ Üzleti logikai funkcióinak módosításai gyorsabb nyissa meg a gyártási

##<a name="rules-concepts"></a>Szabályok fogalmak

##<a name="vocabulary"></a>Szószedet

Egy _Szószedet_ a szabály feltételeket és műveleteket használt számítási objektumok rövid nevének álló definíciók gyűjteménye. Szószedet definíciók könnyebben szabályok olvasni, megértését és megosztása személyek adott üzleti.

Vocabularies készült egymással, áthidalhatják a vállalati szemantikáját és végrehajtása közötti térközt. Például-jóváhagyási állapot adatok kötést előfordulhat, hogy mutasson az egyes az adatbázisban lévő SQL-lekérdezés ábrázolva egyes sorokban lévő egyes oszlop. Összetett képviseleti a rendezés elhelyezése egy szabályt, helyett inkább létrehozhat szószedet definícióját, társított, hogy adatokat kötés, a "Állapot." egy rövid nevet Később is "Állapot" szabályok tetszőleges számú, és a szabály motor meghallgathatja a megfelelő adatokat a táblázat.

##<a name="rule"></a>Szabály

Üzleti szabályok, amelyek meghatározzák az üzleti folyamatok lefolytatása deklaráció kimutatások. Egy szabályt, ahol a feltétel és a műveleteket. Kiértékeli a feltételt, és azt értéke igaz, ha a szabály motor kezdeményez egy vagy több műveletet.
Az üzleti szabályok keretében szabályok határozzák meg a következő formátumban:

_IF_ _feltétel_ _Ezután_ _művelet_

Vegye figyelembe az alábbi példában:

*Ha értéke kisebb vagy egyenlő rendelkezésre álló forrásokhoz*  
*Kattintson a tranzakció és a nyomtatási visszaigazolás lebonyolítása*

Ez a szabály azt határozza meg, hogy tranzakció üzleti logikai funkcióinak összehasonlítása a tranzakció összegét és a rendelkezésre álló forrásokhoz formájában két pénzbeli értékekhez formájában alkalmazásával kell végrehajtani.
Az üzleti szabály létrehozása, módosítása és üzembe üzleti szabályok is használhatja. Másik lehetőségként az előző tevékenységeket programozás útján hajthatják végre.

###<a name="conditions"></a>Feltételek

A megadott feltétel igaz vagy hamis (logikai)-kifejezés, amely tartalmaz egy vagy több predikátumok.
Ebben a példában a predikátumalakzat kisebb vagy egyenlő érvényes az összeg és a rendelkezésre álló forrásokhoz. Ez a feltétel mindig értékeli igaz vagy hamis.
Predikátumok kombinálhatók és a logikai operátorokat és, vagy, nem pedig űrlap egy logikai kifejezés, amely esetleg igen nagy, de mindig értékeli igaz vagy hamis.

###<a name="actions"></a>Műveletek

Műveletek: a feltétel kiértékelésének funkcionális következményeit. Ha egy szabály feltétel teljesül, a megfelelő műveletet vagy műveleteket által kezdeményezett.
Ebben a példában "lebonyolítása tranzakció" és "visszaigazolás nyomtatása" olyan műveleteket hajtanak végre, amikor, és csak a feltétel (ebben az esetben "Ha összeg kisebb vagy egyenlő rendelkezésre álló forrásokhoz") értéke igaz.
Műveletek az üzleti szabályok keretében jelző műveleteket hajt végre beállítása a XML-dokumentumokból.

##<a name="policy"></a>Házirend

A házirend szabályok logikai csoportja. Akkor írása egy házirendet, mentse, tesztelje és, ha elégedett az eredménnyel, használhatja azt üzemi környezetben.

###<a name="policy-composition"></a>Házirend-összeállítás

A szabályok portálon házirendek is írhat. A házirend-tetszőlegesen nagy szabálykészletet is tartalmazhat, de általában írásakor házirend szabályok, amelyek egy adott vállalati tartomány fogja használni a szabály alkalmazása a környezetén belül vonatkoznak.

###<a name="policy-testing"></a>Házirend tesztelése
Hatékony végezheti el a vizsgálat a házirend előtt munkakörnyezetben használatban van. A szabályok portál lehetővé teszi, hogy adja meg a bemeneti adatokat egy házirendet, futtassa a házirendet, és megtekintheti az eredményt. A eredménye magában foglalja a naplókat, szabály végrehajtása, feltétel kiértékelésének, és az eredményül kapott kimeneti értékeket.

##<a name="sample-scenario---insurance-claims"></a>Példa - biztosítási
Lássunk egy példa készíthet, és a, ahogy azt írja be a üzleti logikai funkcióinak ugyanazon elemre.

![Helyettesítő szöveg][1]

Valójában egyszerű biztosítási helyzetben igénylő elküld biztosítási állítását (keresztül bármilyen ügyfél, például a webhely, a Telefon alkalmazás stb). A felelős kérése kap a vállalkozás állítást feldolgozása egység küldött és az adatfeldolgozás az eredménytől függően, az igény is bármelyik állapotát a jóváhagyva, elutasítva vagy küldött mentén további manuális feldolgozása.
Az esetben a felelős feldolgozása egység az adott, a rendszer üzleti logikai funkcióinak felölelő lenne. Részletesebben a egység megnyitása, azt láthatja a következőket:

![Helyettesítő szöveg][2]

Tudassa velünk most üzleti szabályok használatával a logikájának megvalósítása.


##<a name="creation-of-rules-api-app"></a>A szabályok Api-alkalmazás létrehozása


1. Jelentkezzen be az Azure Portáljára
2. Jelölje ki az új -> piactér, majd a *Szabályok BizTalk* keresése
3. Jelölje ki a BizTalk szabályok az eredménylistából. A szabályok BizTalk lap megnyitása
4. Kattintson a *Létrehozás* gombra ![helyettesítő szöveg][3]
1. A megnyíló új lap adja meg az alábbi adatokat:  
    1. Név – adja meg egy nevet a szabályok API-alkalmazáshoz
    1. Alkalmazás szolgáltatáscsomagja – jelölje ki, vagy egy új alkalmazás szolgáltatás terv létrehozása
    1. Réteg – válassza ki a kívánt alkalmazás tárolnak árak réteg árak
    1. Erőforráscsoport – válasszon vagy hozzon létre, ha az alkalmazás kell tárolnak erőforráscsoport
    2. Előfizetés - jelölje ki a használni kívánt előfizetés
    1. Hely: válassza ki a földrajzi hely, ahol meg szeretné telepíteni szeretné az alkalmazást.
4.  Jelölje ki a *létrehozása*. Néhány percen belül szeretne hozható létre az BizTalk szabályok API-alkalmazást.

##<a name="vocabulary-creation"></a>Szószedet létrehozása
Miután létrehozta a BizTalk szabályok API-at, a következő lépésben lenne vocabularies létrehozásához. Az elvárásoknak, hogy a Fejlesztőeszközök-e a gyakrabban azon az ebben a gyakorlatban kell elvégezni. Az alábbiakban beszerzése a Kész gombra:


1. A BizTalk API alkalmazás szabályok a portálon nyissa meg a Tallózás indítási -> API alkalmazások - ><Your Rules API App>. Ez jelenik meg, a szabályok API alkalmazás irányítópult alatti hasonló:

   ![Helyettesítő szöveg][4]

2. Jelölje be a "Szószedet definíciók". Ez volna jelzik, hogy a szószedet szerzői képernyő 3.a választó "Hozzáadás" az új szószedet definíciók helyezne.
Szószedet definíciók jelenleg támogatott – literál és az XML-2 típusát különböztethetjük meg.

##<a name="literal-definition"></a>Konstans meghatározása
1.  Kattintás után a "Hozzáadás", egy új "Definition hozzáadása" lap nyílik meg. Írja be az alábbi értékeket
  1.    Név – speciális karakterek nélkül várható csak alfanumerikus karaktereket. Ez is egyedinek kell lennie a meglévő szószedet definition listára.
  2.    Leírás – nem kötelező mezőben.
  3.    Definíciótípus – létezik 2 támogatott. Ebben a példában válassza ki a literál
  4.    Adattípus – így a felhasználók, jelölje be a definíció adattípusát. Jelenleg támogatott 4 adattípusok: lehet.  Karakterlánc – ezek az értékek dupla idézőjelek ("Példa karakterlánc") kell beírni  
    II. Logikai – Ez lehet igaz vagy hamis  
    III.    Szám – bármilyen decimális szám lehet  
    IV. Dátum és idő – Ez azt jelenti, hogy a definíciója dátum típusú. Adatok kell megadni ebben a formátumban – hh/nn/éééé óó AM\PM  
  5. Beviteli – az adott definíciójának értékét adja meg. Az itt megadott értékeket meg kell felelnie a választott adattípusra. Megadhat egy egyedi érték, a kulcsszó *a*tartományba vagy vesszővel elválasztott csoportja. Ha például megadhat egyedi érték 1. egy sor 1, 2, 3. vagy 1-től 5 cellatartományt. Megjegyzendő, hogy a tartomány csak a támogatott számok.
  6. Kattintson *az OK gombra*.

![Helyettesítő szöveg][5]
##<a name="xml-definition"></a>XML-definícióját
Szószedet típus XML formátumban van kiválasztva, ha a következő bemenetben kell kell megadni  
  egy.    Séma – erre kattintva megnyílik egy új lap lehetővé tevő felhasználó vagy a már feltöltött sémák vagy töltse fel egy új lehetővé teszi a listából választhatja ki.
b.    XPATH – beviteli keresése csak azt követően a séma kiválasztása az előző lépésben zárolását. A gombra kattintva jelennek meg a séma lehetőséget választotta, és lehetővé teszi, hogy a felhasználó számára, jelölje ki a csomópontot, amelynek szószedet definíciója kell létrehozni.  
  c billentyűkombinációt.    FAKT – Ez a beviteli azonosítja, mely adatobjektumok szeretne adni, hogy a szabályok feldolgozása motort. Ez egy speciális tulajdonságok és alapértelmezés szerint a kijelölt XPath-kifejezés szülőjeként értékre van állítva. FAKT különösen fontos láncolás és a webhelycsoport forgatókönyvek lesz.

![Helyettesítő szöveg][6]

### <a name="add-bulk"></a>Tömeges hozzáadása
A fenti lépéseket a rögzített tapasztalnak szószedet definíciók létrehozása. Amikor létrejött, a létrehozott szószedet definíciók listaűrlap jelennek meg. Nincsenek követelmények engedélyezni szeretné több definíciók létrehozása a fenti lépések egyetlen mindig ismétlése helyett ugyanarra a sémára. Ez a hol nagyon hasznos lesz-e tömeges hozzáadása lehetőséget.
Válassza a "Tömeges hozzáadása" kattintva megnyílik egy új lap. Az első lépésként válassza ki a sémát több definíciókat, amelynek nem hozható létre. Ha bejelöli ezt egy új engedélyezésének, vagy választhat a már feltöltött sémák listáját, vagy lehetővé tevő tölthet fel egy új lap nyílik meg.
Most már az XPath kifejezései tulajdonság nem zárolt kap. Ha bejelöli ezt a séma megjelenítő, ahol több csomópontok kiválasztott nyílik meg.
A több definíciók létrehozása a nevek alapértelmezés szerint a kijelölt csomópont nevét. Ezek mindig módosítható létrehozását követően.

![Helyettesítő szöveg][7]

##<a name="policy-creation"></a>Szabály létrehozása
A Fejlesztőeszközök szükséges vocabularies hozott létre, miután a az elvárásoknak, hogy az üzleti elemzői lenne a egy létrehozása vállalati házirendek az Azure-portálon keresztül.  
    1. a szabályok alkalmazásban létrehozott van egy gombra kattintva, amely a felhasználó a program elküldi a házirend létrehozása lappá házirend lens.  
    2. a lapon megjelenik az adott szabály alkalmazás rendelkezik házirendek listájának. Az analitikus egy új házirendet felvenni, egyszerűen írja be a szabály nevét, és kétszer szerezze meg a lapon. Több házirendek egyetlen szabályok API alkalmazásban találhatók.  
    3. a létrehozott házirend kijelölése lép, hogy a felhasználó a házirend Részletek lap hol se láthassa a szabályok, amelyek a házirend.  
    ![Helyettesítő szöveg][8]
 4.  Jelölje be a "Hozzáadás" új szabály gombra. Ezzel eljut egy új lap.

##<a name="rule-creation"></a>Szabály létrehozása
A szabály egy feltételt és műveletet kimutatások gyűjteménye. A műveletek végrehajtása, amennyiben a feltétel kiértékelésének eredménye igaz. Kattintson a szabály létrehozása a lap adjon meg egy (házirendhez) tartozó egyedi szabály nevét és leírását (nem kötelező).
A feltétel (ha) mezőben összetett feltételes utasítások létrehozásához használható. Az alábbiakban a támogatott kulcsszavak:  
1.  És – feltételes operátor  
2.  Vagy – feltételes operátor  
3.  jelent\_nem\_létezik  
4.  létezik  
5.  hamis  
6.  van\_egyenlő\_való  
7.  van\_nagyobb\_mint  
8.  van\_nagyobb\_mint\_egyenlő\_való  
9.  van\_a  
10. van\_kevesebb\_mint  
11. van\_kevesebb\_mint\_egyenlő\_való  
12. van\_nem\_a  
13. van\_nem\_egyenlő\_való  
14. maradék  
15. Igaz  

A Action(THEN) mezőben soronként, hozhat létre, amelyek a végrehajtandó műveleteket több kimutatások is tartalmazhatnak. Az alábbiakban a támogatott kulcsszavak:  
1.  egyenlő  
2.  hamis  
3.  Igaz  
4.  Megállítás  
5.  maradék  
6.  NULL  
7.  frissítés  

A feltételt és műveletet mezőben adja meg a segít szabály gyorsan Szerző az Intellisense. Szerezze meg a ctrl + szóköz, illetve csak most kezdi el beírni ezt is lehet elindítja. Kulcsszavak egyező beírt karakterek a rendszer automatikusan kiszűri lefelé és látható. Az intellisense ablak az összes kulcsszavak és szószedet definíciók jeleníti meg.
![Helyettesítő szöveg][9]

##<a name="explicit-forward-chaining"></a>Közvetlen előre láncolás
Közvetlen előre láncolás, ha a felhasználó újra a szabályok az egyes műveletek választ ki szeretné számítani BizTalk szabályok támogatja, indíthatnak ez egyes kulcsszavak használatával. A támogatott kulcsszavak a következők:  
   1.   frissítés <vocabulary definition> – a kulcsszó újból kiértékeli összes szabályok, amelyek a megadott szószedet definíció állapotát.  
   2.   Leállítja – a kulcsszó leállítja az összes szabály végrehajtások

##<a name="enabledisable-rules"></a>Enable\Disable szabályok
A házirend szabályok engedélyezhető vagy tiltható le. Alapértelmezés szerint minden szabályok engedélyezve vannak. Letiltott szabályok nem futtatható házirend végrehajtás során. Enable\Disable szabályok végezhető el a szabály lap közvetlenül – a parancsokat érhetők el a lap tetején a parancssáv, vagy a házirendet, az a helyi menü (kattintson a jobb gombbal egy szabály) enable\disable lehetősége van.

##<a name="rule-priority"></a>Szabály prioritásának
A szabályok házirend az összes sorrendben végrehajtása. Végrehajtási prioritás a sorrendben, a házirend előforduló határozza meg. A prioritás módosítható egyszerűen, húzza a szabályt.

##<a name="test-policy"></a>Házirend tesztelése
A házirendek az házirend tesztelje a lap "Tesztelése házirend" parancsával tesztelheti. Ez a lap akkor láthatja szószedet definíciókat, a házirend használt, és a felhasználói beavatkozást igénylő listáját. Felhasználók kézzel is hozzáadhatja a próba forgatókönyvet e ráfordítások értékeit. Másik megoldás, felhasználók is beállíthatja, hogy importálja XMLs tesztelése a bemeneti adatok alapján. Összes bemenetben vannak, a vizsgálat futtatását is lehetővé teszi, és minden szószedet meghatározása a kimeneti értékeket jelenít meg a kimeneti oszlopban egyszerűen összehasonlításra. Üzleti elemző rövid naplók megtekintéséhez kattintson a "Naplók megtekintése" a végrehajtás naplók megtekintéséhez. A naplók mentése a "Mentés kimeneti" lehetőség érhető el tárolásához összes ellenőrizheti, hogy a független elemzéshez kapcsolódó adatokat.

## <a name="using-rules-in-logic-apps"></a>Szabályok használata összefüggés-alkalmazásokban
Miután a házirend szerzője, és a vizsgált, már készen áll a felhasználás. Létrehozhat új logika alkalmazás logika alkalmazások választhatja ki a portál Kezdőlap lap bal szélén. Az összefüggés-alkalmazás létrehozása után indítsa el, majd válassza ki a *Eseményindítók és a műveletek*. Válassza a *nulláról* sablont. Kövesse a az BizTalk szabályok API-alkalmazás hozzáadása a logika alkalmazáshoz. Miután ez megtörtént, válassza ki, milyen szabályok API alkalmazás (művelet) célba beállítást lesz. Műveletek a házirendek az végre kell hajtani listáját tartalmazza. Válasszon egy adott házirendet, amely után a bemeneti adatok alapján a házirend-höz szükséges kell megadni. Felhasználók a kimenet után a szabályok API alkalmazásban használható további döntéshez.

## <a name="using-rules-via-apis"></a>Szabályok keresztül API-k használata
A szabályok API-alkalmazás API-khoz széles körű használatával is hivatkozhat. A felhasználók nem korlátozódik összefüggés csak alkalmazásokkal, és használhatja azáltal, hogy a többi hívások bármely alkalmazásban szabályok. Kattintson a szabályok irányítópulton a "API Definiton" lens megtekintheti a pontos REST API-k érhető el.

![Helyettesítő szöveg][10]

Példa: hogyan egyik használhatnak, ez az API C#

            // Constructing the JSON message to use in API call to Rules API App

            // xmlInstance is the XML message instance to be passed as input to our Policy
            string xmlInstance = "<ns0:Patient xmlns:ns0=\"http://tempuri.org/XMLSchema.xsd\">  <ns0:Name>Name_0</ns0:Name>  <ns0:Email>Email_0</ns0:Email>  <ns0:PatientID>PatientID_0</ns0:PatientID>  <ns0:Age>10.4</ns0:Age>  <ns0:Claim>    <ns0:ClaimDate>2012-05-31T13:20:00.000-05:00</ns0:ClaimDate>    <ns0:ClaimID>10</ns0:ClaimID>    <ns0:TreatmentID>12</ns0:TreatmentID>    <ns0:ClaimAmount>10000.0</ns0:ClaimAmount>    <ns0:ClaimStatus>ClaimStatus_0</ns0:ClaimStatus>    <ns0:ClaimStatusReason>ClaimStatusReason_0</ns0:ClaimStatusReason>  </ns0:Claim></ns0:Patient>";

            JObject input = new JObject();

            // The JSON object is to be of form {"<XMLSchemName>_<RootNodeName>":"<XML Instance String>".
            // In the below case, we are using XML Schema - "insruanceclaimsschema" and the root node is "Patient".
            // This is CASE SENSITIVE.
            input.Add("insuranceclaimschema_Patient", xmlInstance);
            string stringContent = JsonConvert.SerializeObject(input);


            // Making REST call to Rules API App
            HttpClient httpClient = new HttpClient();

            // The url is the Host URL of the Rules API App
            httpClient.BaseAddress = new Uri("https://rulesservice77492755b7b54c3f9e1df8ba0b065dc6.azurewebsites.net/");
            HttpContent httpContent = new StringContent(stringContent);
            httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse("application/json");

            // Invoking API "Execute" on policy "InsruranceClaimPolicy" and getting response JSON object. The url can be gotten from the API Definition Lens
            var postReponse = httpClient.PostAsync("api/Policies/InsuranceClaimPolicy?comp=Execute", httpContent).Result;

Ne feledje, hogy a fenti szabályok API-alkalmazás "Nyilvános Anon" értékűre biztonsági beállításait. Ez állíthat be az API-alkalmazásból használatával – minden elérhető beállítás-beállításai > -> hozzáférési szintet.

![Helyettesítő szöveg][11]

## <a name="editing-vocabulary-and-policy"></a>Szószedet és a házirend szerkesztése
Egyik legfontosabb előnye a üzleti szabályok használatára, hogy üzleti logikai funkcióinak módosításai is kell tolódik gyártási sokkal gyorsabb. Szószedet és házirendek bármilyen változás a program azonnal alkalmazza a termelési. Felhasználói egyszerűen van szüksége, akkor lépnek érvénybe, a változtatások, és keresse meg a megfelelő szószedet definition vagy házirend.

<!--Image references-->
[1]: ./media/app-service-logic-use-biztalk-rules/InsuranceScenario.PNG
[2]: ./media/app-service-logic-use-biztalk-rules/InsuranceBusinessLogic.png
[3]: ./media/app-service-logic-use-biztalk-rules/CreateRuleApiApp.png
[4]: ./media/app-service-logic-use-biztalk-rules/RulesDashboard.png
[5]: ./media/app-service-logic-use-biztalk-rules/LiteralVocab.PNG
[6]: ./media/app-service-logic-use-biztalk-rules/XMLVocab.PNG
[7]: ./media/app-service-logic-use-biztalk-rules/BulkAdd.PNG
[8]: ./media/app-service-logic-use-biztalk-rules/PolicyList.PNG
[9]: ./media/app-service-logic-use-biztalk-rules/RuleCreate.PNG
[10]: ./media/app-service-logic-use-biztalk-rules/APIDef.PNG
[11]: ./media/app-service-logic-use-biztalk-rules/PublicAnon.PNG
