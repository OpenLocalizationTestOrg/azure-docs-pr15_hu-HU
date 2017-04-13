Identitás kezelése fontos csak a nyilvános a felhőben, mert a helyszíni. Segíti az adatokkal, a Azure számos különböző felhő identitás technológiát támogatja. Ezek tartalmazzák:

- A felhőben használatáról virtuális gépeken futó készült Azure virtuális gépeken futó futtatását is lehetővé teszi a Windows Server Active Directory (általában csak az Active Directory a neve). Ez a módszer van ilyesmire lehetőség, a helyszíni adatközponthoz kiterjeszti az a felhő Azure használata esetén.


- Azure Active Directory segítségével a felhasználók egyszeri bejelentkezés [(szoftver) szolgáltatásként](https://azure.microsoft.com/overview/what-is-saas/) alkalmazások adni. A Microsoft Office 365-ben ez technológiát használ, például és Azure vagy más felhő platformon futó alkalmazások is használhatja.


- A helyszíni vagy felhőalapú futó alkalmazások Azure Active Directory hozzáférés-vezérlés segítségével küldő felhasználók log identitások a Facebookról, a Google, a Microsoft vagy más identitásszolgáltatók használata.


Ez a cikk ismerteti az alábbi lehetőségek mindhárom.

## <a name="table-of-contents"></a>A tartalomjegyzék

- [Windows Server Active Directory virtuális gépeken futó operációs rendszert futtató](#adinvm)

- [Azure Active Directory használata](#ad)

- [Azure Active Directory hozzáférés-vezérlés használata](#ac)


## <a name="adinvm"></a>Windows Server Active Directory virtuális gépeken futó operációs rendszert futtató

A Windows Server AD Azure virtuális gépeken futó operációs rendszert futtató nagyon hasonlít a helyszíni futtatásával. [Ábra 1](#fig1) egy tipikus példa látható, hogyan ezzel jeleníti meg.

![Azure Active Directory virtuális gépen](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Ábra 1: Windows Server Active Directory futtatását is lehetővé teszi az Azure virtuális gépeken futó egy szervezet helyszíni adatközponthoz Azure virtuális hálózaton keresztül csatlakozik.

Az alábbi példával, a Windows Server AD készült Azure virtuális gépeken futó, a platform IaaS technológia VMs fut. Ezek a VMs és néhány mások egy helyszíni adatközponthoz Azure virtuális hálózaton keresztül csatlakozik egy virtuális hálózati vannak csoportosítva. A virtuális hálózat kivágja, együttműködhet a helyszíni hálózaton, virtuális magánhálózaton (VPN) kapcsolaton keresztül, a felhőben virtuális gépeken futó csoportja. Lehetővé módon ezek Azure virtuális gépeken futó megjelenésével megegyező kialakítással szeretné a helyszíni adatközponthoz csak egy másik alhálózat. Az ábrán az látható, mint két e VMs futtatja a Windows Server Active Directory tartományi vezérlők. Előfordulhat, hogy a többi virtuális gépeken futó a virtuális hálózaton futnia alkalmazásokhoz, például a SharePoint, vagy használja-e más módon, például fejlesztés és tesztelésére. A helyszíni adatközponthoz is működik, és két Windows Server Active Directory tartományi vezérlő.

Létezik a tartományhoz vezérlők a felhőben összekapcsolása helyszíni konstrukciókban több lehetőség közül választhat:

- Ellenőrizze az összeset egyetlen Active Directory-tartománya része.

- Hozzon létre külön AD tartományok helyszíni és felhőbeli ugyanabban a tartományban részét képező.

- A felhőben, és a helyszíni külön AD erdők létrehozása, majd az idegen erdőszintűek vagy Windows Server Active Directory összevonási szolgáltatások (AD FS), amely a Azure virtuális gépeken futó is futtathatók forests csatlakozni.

Lett bármilyen választási lehetőségek, a rendszergazda győződjön meg arról, hogy a helyszíni felhasználó hitelesítési kérések nyissa meg a felhő tartomány vezérlők csak akkor, ha szükséges, mivel a hivatkozást a felhőbe valószínűleg a helyszíni hálózatok lassabb. Egy másik tényezőt kell figyelembe venni a felhőbe csatlakozó és a helyszíni tartomány vezérlők a replikáció által generált forgalmat. A felhőben tartomány vezérlők jellemzően a saját Active Directory, a webhelyet, amely lehetővé teszi, hogy milyen gyakran ütemezése rendszergazda replikációs befejeződött. Azure díjai ki az Azure adatközponthoz adatforgalom, bár nem bájtok küldött, amely hatással lehet a rendszergazda replikációs választási lehetőségeket. Érdemes is mutató, hogy Azure támogatja a saját tartománynév (DNS), amíg ez a szolgáltatás hiányzik a szolgáltatások (például dinamikus DNS és SRV rekordok támogatásának) az Active Directory számára szükséges. Emiatt a saját DNS-kiszolgálók a felhőben való beállításának futó Azure Active Directory Windows Server szükséges.

A Windows Server AD az Azure VMs operációs rendszert futtató is értelme számos különböző helyzetben. Íme néhány példa:

- Ha Azure virtuális gépeken futó saját adatközponthoz meghosszabbítása használata esetén a felhőben van szükség helyi tartomány vezérlők dolgot, például a Windows-hitelesítés kérések vagy LDAP-lekérdezések kezelése alkalmazások előfordulhat, hogy futtatnia. A SharePoint, például kommunikáljon gyakran az Active Directory, és úgy is lehet egy SharePoint-farm futtathatnak Azure egy helyszíni könyvtár használatával, miközben beállítása a tartomány vezérlők a felhőben jelentősen növeli a teljesítményt. (Fontos tudni, hogy ez nem feltétlenül szükséges, azonban; alkalmazások rengeteg sikeresen futtathatók a felhőben, csak a helyszíni tartomány vezérlők használata.)

- Tegyük fel, hogy egy közösségihez irodában nem rendelkezik a szükséges források a saját tartomány-vezérlők futtatása. Ma a felhasználók kell hitelesíteni a világ másik oldalon tartomány vezérlők - bejelentkezések lassú. Azure Active Directory futó egy közelebb Microsoft adatközpontban felgyorsítható ez anélkül, hogy az irodában további kiszolgálókhoz.

- Azure vészhelyreállítás használó előfordulhat, hogy kezelése a felhőben, beleértve a tartományvezérlőnek aktív VMs kis készlete. Tudja majd készüljön fel bontsa ki a helyet, máshol hibák átveheti szükség szerint.

Egyéb lehetőségek is vannak. Ha például jelenleg nem szükséges, a Windows Server AD a felhőben csatlakozhat egy helyszíni adatközponthoz. Egy adott meg felszolgált SharePoint-farm futtatásához megy végbe, ha például, akik volna jelentkezzen be kizárólag felhőalapú felhasználókkal, előfordulhat, hogy hozzon létre egy különálló erdő Azure a. Ez a technológia használatát attól függ, hogy Mik azok a célok. (A részletes útmutatást AD a Windows Server használata Azure, [itt talál](http://msdn.microsoft.com/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Azure Active Directory használata

Amint szoftver alkalmazások válnak, több közös, azok előléptetése egy egyértelmű kérdés: milyen típusú címtár-szinkronizálás eszközének kell ezeket az alkalmazásokat felhőalapú használni? A kérdésre adott válasz a Microsoft Azure Active Directory.

Kétféleképpen lehet fő a felhőben a címtár-szinkronizálás eszközének használata:

- Személyek és a szervezeteknek szól, amelyek csak a szoftver-alkalmazások használatát számíthat Azure Active Directory, a kizárólagos címtár-szinkronizálás eszközének.

- Azoknak a szervezeteknek, futtassa a Windows Server Active Directory a helyszíni címtár csatlakoztatása az Azure Active Directory, majd adjon meg a felhasználók egyszeri bejelentkezés a szoftver alkalmazások használatával.


[Ábra 2](#fig2) azt szemlélteti, hogy az első két beállításokat, ahol Azure Active Directory az összes szükséges.

![Azure Active Directory virtuális gépen](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Ábra 2: Azure Active Directory biztosít szervezet felhasználók egyszeri bejelentkezés a szoftver alkalmazásokkal, beleértve az Office 365-ben.

Az ábrán az látható, mint az Azure Active Directory olyan több bérlői szolgáltatás. Ez azt jelenti, hogy egyszerre sok más szervezetek, mindegyiknél felhasználókról címtár adatok tárolásának támogatja. Ebben a példában a szervezet A felhasználó megpróbál elérhetik a szoftver alkalmazást. Ez az alkalmazás tartozhat az Office 365-ben, például a SharePoint Online vagy mást lehet – Microsoft által fejlesztett alkalmazások e technológia is használhatja. Azure AD a SAML 2.0-s protokollt támogató, az azt jelenti, hogy kapcsolatba, használja a szabványos alkalmazásokból szükséges az összes található. (Valójában Azure Active Directory használó alkalmazások futtatását is lehetővé teszi olyan adatközpontban, nem csak Azure adatközponthoz.)

A folyamat elkezdődik, amikor a felhasználó hozzáfér a szoftver-alkalmazások (lépés: 1). Ez az alkalmazás használatához a felhasználónak kell bemutatnia Azure Active Directory által kibocsátott token.

Ez a token információkat tartalmaz, a felhasználói azonosító, és Azure Active Directory digitális aláírással. Ha a jogkivonat, a felhasználó hitelesíti saját maga Azure AD, mert a felhasználónév és jelszó (lépés: 2). Azure Active Directory majd eredménye a token ő 3 szükséges (lépés).

Ez a token kattintson a rendszer elküldi a szoftver alkalmazás (lépés: 4), amely ellenőrzi a token aláírást, és használja (5 lépés) tartalma. Az alkalmazás általában az a token azt mutatja, hogy milyen információkat, a felhasználó számára engedélyezett az access és esetleg más módokon azonosító információkat fogja használni.

Ha az alkalmazás további információt a felhasználó mi található a jogkivonat-nél, akkor kérhet Ez közvetlenül az Azure Active Directory, a Azure Active Directory Graph API (6 lépés). Az első verziójának Azure Active Directory, a címtár-séma használata rendkívül egyszerű: csak a felhasználók és csoportok és kapcsolatok közöttük tartalmaz. Alkalmazások ezek az információk segítségével megismerheti a felhasználók közötti kapcsolatok. Tegyük fel, hogy egy alkalmazást kell, hogy kivel döntse el, hogy ő még hozzáférhetnek néhány adattömb a felhasználó felettese. Azt lekérdezésével keresztül a Graph API Azure Active Directory talál.

A diagram API-hétköznapi RESTful protokollt használ, melyen különböző információkat egyszerű használata a legtöbb ügyfelektől, beleértve a mobileszközök. Az API támogatja a bővítményeket, OData, dolog, amit egy lekérdezési nyelv például, hogy az ügyfelek access-adatok még hasznosabbá módokon hozzáadása által meghatározott is. (További OData szóló című témakörben talál [OData bemutatása](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) A diagram API is használható, ha többet szeretne tudni a felhasználók közötti kapcsolatokat, mivel a közösségi diagram, amely egy adott szervezet számára (amely oka annak, hogy a diagram API hívják) az Azure Active Directory sémában beágyazott megértéséhez alkalmazások lehetővé teszi. És és hitelesítse magát Graph API kérések Azure AD, az alkalmazás OAuth 2.0-s verzióját használja.

Ha a szervezet nem használja a Windows Server Active Directory - nem rendelkezik a helyszíni kiszolgálók vagy tartományok –, és a kizárólag a felhőben alkalmazások által használt Azure Active Directory támaszkodik, csak a felhőben directory használata adna a cég felhasználók egyszeri bejelentkezés a az összes őket. Még ebben az esetben gyakrabban kap minden nap, miközben a legtöbb szervezet továbbra is használja a helyszíni tartományok létrehozása a Windows Server Active Directory. Azure Active Directory tartalmaz egy hasznos szerepkör lejátszásához itt is látható, [3 ábrán](#fig3) látható módon.

![Azure Active Directory virtuális gépen](./media/identity/identity_03_AD.png)
<a id="fig3"></a>ábra 3: szervezet is összevonása a Windows Server Active Directory Azure Active Directory adhat a felhasználók egyszeri bejelentkezés a szoftver alkalmazásokat.

Ebben az esetben a felhasználók a szervezet B kívánja elérni a szoftver-alkalmazások. Ő végzi, mielőtt a szervezet címtárban kell létrehozni a szövetséges kapcsolat használatával Active Directory összevonási szolgáltatások, mint az ábrán látható Azure AD. Ezeket a rendszergazdák konfigurálnia kell a szervezet helyszíni Windows Server Active Directory és Azure Active Directory adatok szinkronizálása. Ezzel automatikusan másolja felhasználói és csoportadatok a helyszíni címtárból Azure AD. Figyelje meg, ezzel engedélyezi: gyakorlatilag azt mondja, a szervezet van kiterjeszti a helyszíni címtár a felhőben. A szervezet kombinálása a Windows Server Active Directory és Azure Active Directory ilyen ad a címtár-szinkronizálás eszközének segítségével kezelhető egységként, miközben továbbra is fennáll a helyigénye mindkét a helyszíni és felhőbeli.

Azure Active Directory használatához a felhasználónak először jelentkezik be a helyszíni Active Directory tartományi a szokásos módon (lépés: 1). Amikor megpróbálja elérhetik a szoftver alkalmazást (lépés: 2), Noémi az összevonási folyamat eredményez Azure AD-kiállító, amelyen a jogkivonat, ehhez az alkalmazáshoz (3 lépés). (További összevonási működése a [identitás Jogcímeken alapuló Windows: technológiák és -esetek](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Mielőtt a token információkat tartalmaz, amely azonosítja a felhasználó, és Azure Active Directory digitális aláírással. Ez a token kattintson a rendszer elküldi a szoftver alkalmazás (lépés: 4), amely ellenőrzi a token aláírást, és használja (5 lépés) tartalma. Az előző példában az alkalmazás a Graph API segítségével többet szeretne tudni a felhasználóhoz, ha a szoftver, és szükség esetén (6 lépés).

Ma Azure Active Directory nem a teljes helyszíni Windows Server AD. Már említettük, mint a felhőben könyvtár még sokkal egyszerűbb séma, és például csoportházirend, az azt jelenti, hogy a gép adatainak tárolására, és a támogatási LDAP dolgot is hiányzik. (Valójában egy Windows számítógépre nem állítható be, hogy a felhasználó, jelentkezzen be az Azure Active Directory de semmi sem használatával – Ez a nem támogatott.) Ehelyett a kezdeti céloknak való Azure Active Directory olyan vállalati felhasználóknak access-alkalmazások a felhőben engedélyezem fenntartása egy külön bejelentkezés nélkül és felszabadítása a helyszíni a helyszíni címtár manuális szinkronizálása minden szoftver alkalmazást a szervezete használ a címtárban. Az idő azonban a felhőben címtár-szinkronizálás eszközének forgatókönyvek szélesebb köre megoldására várnak.

## <a name="ac"></a>Azure Active Directory hozzáférés-vezérlés használata

Identitás felhőalapú technológiák használható sokféle probléma megoldására. Azure Active Directory adhat a szervezeti felhasználók egyszeri bejelentkezés a több szoftver-alkalmazásokhoz, például. De a felhőben identitás technológiák más módokon is használható.

Tegyük fel például, hogy az alkalmazás kívánja-e a felhasználóknak, hogy jelentkezzen be a token több *Identitásszolgáltatók (IdPs)*által kibocsátott. Számos különböző Identitásszolgáltatók Facebook, a Google, a Microsoft és mások számára, például a ma, létezik, és alkalmazások gyakran engedélyezése a felhasználók számára, jelentkezzen be az alábbi identitások egyik. Miért célszerű az alkalmazások ne saját listája a felhasználók és jelszavak kezelése, ha inkább számíthat identitások, már meg? Meglévő identitások elfogadása teszi leírási_idő egyszerűbb mind a felhasználók számára, akik egy felhasználónevet és jelszót, ne feledje, hogy kevesebb és a létrehozó személyekkel az alkalmazás, akik már nem kell kezelniük a saját listák, a felhasználóneveket és jelszavakat.

De amíg minden identitásszolgáltató problémák valamilyen jogkivonat, ezeket a tokenek nem szabványos - egyes IdP saját formátuma. Ezenkívül az adott tokenek adatokat is nem szabványos. Olyan alkalmazás, amely szerint által kibocsátott tokenek, a Facebook, a Google és a Microsoft fogadja el kívánja a beavatkozás igazolására szolgáló eljárás egyedi kódírás, minden más formátumokról kezelésére van projektvezetők.

De miért van erre? Miért nem inkább létre, amely miatt egy közös azonosító információkat megjelenítő egyetlen jogkivonat formátumot közvetítő? Ezt a megközelítést volna leírási_idő egyszerűbbé teszik a fejlesztőknek alkalmazások, létrehozása, mivel most már van szükségük kezelésére csak egy jellegű jogkivonat. Azure Active Directory hozzáférés-vezérlés végzi pontosan, a felhőben közvetítő kezeléséről a különböző tokenek. [Ábra 4](#fig4) jeleníti meg, hogy hogyan működik

![Azure Active Directory virtuális gépen](./media/identity/identity_04_IdentityProviders.png)
<a id="fig4"></a>ábra 4: Azure Active Directory hozzáférés-vezérlés megkönnyíti az alkalmazások fogadja el a különböző identitásszolgáltatók által kibocsátott identitás tokenek.

A folyamat megkezdi a alkalommal, amikor egy felhasználó az alkalmazás elérése a böngészőből. Az alkalmazás irányítja át, hogy szeretne egy saját megválasztott IdP (és, hogy megbízik-e még az alkalmazás). Noémi hitelesíti saját magát a IdP, például felhasználónevet és jelszót (lépés: 1) beírásával, és a IdP saját (lépés: 2) adatait tartalmazó jogkivonat adja eredményül.

Mint az ábrán az látható, a hozzáférés-vezérlés különböző felhőalapú IdPs, beleértve a Google, Yahoo, Facebook, a Microsoft-(korábbi nevén Windows Live ID azonosító) és bármely OpenID szolgáltató által létrehozott egy cellatartomány támogatja. Is támogat identitások készült Azure Active Directory és az AD FS, Windows Server Active Directory összevonási keresztül. A cél, hogy pontosan illeszkedjen ma, a leggyakrabban használt identitások, hogy azok a esetén a felhőben, vagy a helyszíni IdPs által kibocsátott.

Miután a felhasználó böngészőjében egy IdP jogkivonat a választott IdP rendelkezik, elküldi a jogkivonat hozzáférés-vezérlés (3 lépés). Hozzáférés-vezérlés ellenőrzi a jogkivonat, gondoskodhat arról, hogy, hogy azt valójában a IdP bocsátotta, majd létrehoz egy új jogkivonat, az ehhez az alkalmazáshoz megadott szabályoknak megfelelő. Azure Active Directory, például a hozzáférés-vezérlés több bérlői szolgáltatás, de a bérlők ügyfél szervezetek, hanem alkalmazásokat. Egyes alkalmazások is használható lesz a saját névtér, mint az ábrán látható, és különböző szabályokat a hitelesítés és egyebeket adhat meg.

Szabályok engedélyezése a minden alkalmazás rendszergazda határozza meg, hogyan token származhat különféle IdPs be egy hozzáférési jogkivonat transzformált kell-e. Például ha másik IdPs különféle kifejezésére felhasználónevét, hozzáférés-vezérlés szabályok alakíthatják át ezeket a felosztott közös felhasználónév típus összes. Hozzáférés-vezérlés majd elküldi e új token a böngészőben (lépés: 4), amely elküldi az alkalmazás (5 lépés). Rendelkezik a hozzáférési jogkivonat, amikor az alkalmazás ellenőrzi, hogy a token valójában hozzáférés-vezérlés bocsátotta, majd használja a benne lévő információkat meg (6 lépés).

Ez a folyamat kissé bonyolultnak tűnhet, miközben ténylegesen van leírási_idő jelentősen egyszerűbb a létrehozó az alkalmazás. Hanem a különböző információkat tartalmazó különböző tokenek kezelheti, az alkalmazás elfogadhatja továbbra is fogadása csak a jól ismert adatokkal egyetlen jogkivonat közben több identitásszolgáltatók által kibocsátott identitások. Is, hanem csak a minden alkalmazás megbízhatónak különböző IdPs konfigurálni, ezek a bizalmi kapcsolatok inkább karbantartott által hozzáférés-vezérlés – van szüksége az alkalmazás csak megbízható.

Érdemes mutató, hogy semmi, a hozzáférés-vezérlés területhez tartozik a Windows - is felhasználhatók, amely csak a Google és a Facebook identitások elfogadott Linux alkalmazásával. És annak ellenére, hogy a hozzáférés-vezérlés az Azure Active Directory család része, érdemes elképzelnie, a Mi az előző szakaszban ismertetett teljesen különálló szolgáltatásként. Mindkét technológiák identitás dolgozni, miközben azok igazán más problémák megoldása (bár Microsoft van said, hogy a két bizonyos pontján integrálása vár).

Identitás használata fontos szinte minden alkalmazásban. A hozzáférés-vezérlés célja, így azok könnyebben fejlesztők számára fogadja el a különböző Identitásszolgáltatók identitás-alkalmazások létrehozásához. Ha elhelyez ezt a szolgáltatást a felhőben, Microsoft tette bármely bármely platformon futó alkalmazás érhető el.

##<a name="about-the-author"></a>A szerzőről

Belinszki Chappell fő a Chappell és ismerőseivel [www.davidchappell.com](http://www.davidchappell.com) San Francisco, Kalifornia.
