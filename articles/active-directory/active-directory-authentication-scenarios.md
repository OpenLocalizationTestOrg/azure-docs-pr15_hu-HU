
<properties
   pageTitle="Az Azure Active Directory Authentication esetek |} Microsoft Azure"
   description="Az öt leggyakoribb hitelesítési jelenik meg az Azure Active Directory (AAD) áttekintése"
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="authentication-scenarios-for-azure-ad"></a>Az Azure Active Directory Authentication esetek

Azure Active Directory (Azure Active Directory) egyszerűbbé teszi a hitelesítési fejlesztők számára megadásával identitás szolgáltatáshoz, szabványos protokollok, például OAuth 2.0-s és csatlakozás OpenID támogatása, valamint megnyitott adatforrásként tárak más platformokon segítséget gyorsan coding megkezdéséhez. A dokumentum segít megérteni a különböző esetek Azure AD-támogatja, és láthatja, hogy miként kezdheti. Az alábbi szakaszok van osztva:

- [Az Azure Active Directory Authentication alapjai](#basics-of-authentication-in-azure-ad)

- [Az Azure Active Directory biztonsági tokenek követelések](#claims-in-azure-ad-security-tokens)

- [Alkalmazás rögzítése az Azure Active Directory alapjai](#basics-of-registering-an-application-in-azure-ad)

- [Alkalmazás-típusok és -esetek](#application-types-and-scenarios)

  - [Webalkalmazás webböngészőben](#web-browser-to-web-application)

  - [Egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés)](#single-page-application-spa)

  - [A webes API tartozó alkalmazásban](#native-application-to-web-api)

  - [Webes API webalkalmazás](#web-application-to-web-api)

  - [Démon vagy a webes API alkalmazáskiszolgáló](#daemon-or-server-application-to-web-api)



## <a name="basics-of-authentication-in-azure-ad"></a>Az Azure Active Directory Authentication alapjai

Ha nem ismeri a alapfogalmak: Azure AD-hitelesítés, olvassa el. Egyéb esetben érdemes le az [alkalmazás-típusok és -esetek](#application-types-and-scenarios)ugorja át.

Nézzünk meg az adott identitás szükség-e a legalapvetőbb eset: webböngészőben felhasználó kell webalkalmazás hitelesítést végezni. Részletesebben a [Webböngészőben webalkalmazás](#web-browser-to-web-application) szakaszban ismertetett ebben az esetben, de hasznos kiindulási pont elemzéssel hatékonyan szemléltethető az Azure Active Directory lehetőségeit és az alkalmazási példát működése conceptualize. Vegye figyelembe az alábbi ábra az ebben az esetben:

![Bejelentkezés a webes alkalmazás áttekintése](./media/active-directory-authentication-scenarios/basics_of_auth_in_aad.png)

A diagram felett tartva, az alábbiakban mit kell tudni a különféle összetevőket:

- Azure AD a identitásszolgáltató, a felhasználók és az alkalmazások, a szervezet címtárára megtalálható a személyazonosság ellenőrzésének, és végül kiadása után azok a felhasználók és alkalmazások sikeres hitelesítése biztonsági tokenek felelős.


- Olyan alkalmazás, amely az Azure Active Directory-hitelesítés kihelyező csevegni regisztrálni az Azure Active Directory, amely regisztrálja, és az alkalmazás a címtárban egyedileg kell azonosítania kell.


- A fejlesztők hitelesítési egyszerűen teheti a protokoll részleteinek kezelésével meg a Megnyitás Azure Active Directory authentication tárak használhatja. [Azure Active Directory Authentication tárak](active-directory-authentication-libraries.md) talál további információt.


•, Amikor a felhasználó hitelesített, az alkalmazás ellenőriznie kell a felhasználó biztonsági jogkivonat annak érdekében, hogy a hitelesítés sikeresen befejeződött a tervezett felek. A fejlesztők a megadott hitelesítési tárak segítségével az Azure Active Directory, beleértve a JSON webes tokenek (JWT) vagy a SAML 2.0 bármely jogkivonat érvényességi kezelni. Ha szeretne manuálisan végezze el az érvényesítési, akkor olvassa el a [JWT jogkivonat kezelő](https://msdn.microsoft.com/library/dn205065.aspx) dokumentációt.


> [AZURE.IMPORTANT] Azure Active Directory nyilvános kulcs titkosítás használatával jelentkezzen be a token, és győződjön meg róla, hogy azok érvényes. Lásd: a [Fontos információkat kapcsolatos aláírási kulcs átváltási az Azure Active Directory](active-directory-signing-key-rollover.md) további információt a szükséges logika kell rendelkeznie ahhoz, hogy az alkalmazás frissítése mindig a legújabb billentyűkkel.


• Az kérések és válaszok a hitelesítési folyamat határozzák meg a hitelesítési protokollnak használt, például OAuth 2.0-s OpenID csatlakozni, Webszolgáltatás-Összevonás, illetve a SAML 2.0-s verziója. Ezek a protokollok részletesebben az [Azure Active Directory Authentication protokollok](active-directory-authentication-protocols.md) témakör és az alábbi szakaszok ismertetik.

> [AZURE.NOTE] Azure Active Directory támogatja a OAuth 2.0-s, és mértékben OpenID csatlakozás szabványokat bearer tokenek, többek között a JWTs ábrázolva bearer tokenek használja. Egy bearer jogkivonathoz a könnyű biztonsági jogkivonat, amely a "bearer" hozzáférést biztosít a védett erőforrás. Ebben az értelemben "bearer" bármely fél, amely a token bemutathatja. Bár kell fél Azure AD az bearer jogkivonathoz fogadásához először hitelesíteni, ha a szükséges lépéseket nem vesznek biztonságossá tétele a token továbbítása, tárterület, hozzá, és egy várt fél használják. Néhány biztonsági tokenek van a beépített kisalkalmazások megakadályozza, hogy a jogosulatlan felek használja őket, miközben a bearer tokenek Ez az eljárás nem rendelkezik, és egy biztonságos csatornát, például átviteli réteg biztonsági (HTTPS) kell lennie szállított. Egy bearer jogkivonathoz törlése küldött a rendszer, egy férfi a a középső név kezdőbetűjét homonimaszerű webcímmel használható rosszindulatú fél szerezheti be a token és használni szeretné az illetéktelen hozzáférést védett erőforrás. Azonos biztonsági elvek alkalmazása tárolásáról vagy gyorsítótárazásának bearer tokenek későbbi felhasználás céljából. Mindig győződjön meg arról, hogy az alkalmazás továbbítja, és biztonságos módon bearer tokenek tárolja. További biztonsági megfontolások a bearer tokenek [RFC 6750 szakasz 5](http://tools.ietf.org/html/rfc6750)című témakör tartalmaz.


Most, hogy alapvető áttekintése, olvassa el a kiépítési működésének megértése az Azure Active Directory az alábbi szakaszok, és a gyakori alkalmazási területek Azure Active Directory támogatja.


## <a name="claims-in-azure-ad-security-tokens"></a>Az Azure Active Directory biztonsági tokenek követelések

Azure Active Directory által kibocsátott biztonsági tokenek követelések és előfeltételek az alany, amelyek hitelesítése információkat tartalmaz. Ezek követelések különböző tevékenységek használható az alkalmazás. Ha például azok használható a token érvényesítése, a tárgy címtár bérlői azonosítása, felhasználói adatok, megállapítása a tárgy, és így tovább. A bemutató minden megadott biztonsági jogkivonat jogcímalapú jogkivonat, a hitelesítő adatok hitelesíti a felhasználó, és az alkalmazás-konfiguráció típusú típusától függnek. Azure Active Directory által kibocsátott állítást hibatípusonként egy rövid leírást az alábbi táblázatban megadva. További tudnivalókért olvassa el az [jogkivonat támogatott és állítást típusok](active-directory-token-and-claims.md).


| Igénylése | Leírás |
|-------|-------------|
| Azonosítója | Az alkalmazás, amely használja a token azonosítja.
| A célközönség | A címzett erőforrás a token számára íródott azonosítja. |
| Alkalmazás hitelesítési helyi osztály hivatkozás | Azt jelzi, hogy az ügyfél hitelesített (nyilvános ügyfél bizalmas ügyfél összehasonlítása). |
| Azonnali hitelesítés | Rekordok a dátumot és időpontot, amikor a hitelesítés történt. |
| Hitelesítési módszer | Azt jelzi, hogy hogyan hitelesített a token tárgya (jelszót, tanúsítvány stb.). |
| Utónév | A megadott névvel, annak a felhasználónak itt beállított az Azure Active Directory. |
| Csoportok | A felhasználó tagja azonosítók az Azure Active Directory objektumcsoportok tartalmazza. |
| Identitásszolgáltató | Rekordok az identitásszolgáltató, és amelyek hitelesítése a token tárgyát. |
| A kiadott | Az idő, amelynél a token bocsátotta, gyakran használt jogkivonat frissessége rekordok. |
| Kibocsátó | A STS, amely a jogkivonat, valamint a Azure AD-bérlő által kibocsátott azonosítja. |
| Vezetéknév | Itt annak a felhasználónak a Vezetéknév szerint az Azure Active Directory. |
| név | Itt emberi olvasható érték, amely azonosítja a token tárgyát. |
| Objektum azonosítója | Azure AD a tárgy megváltoztatható, egyedi azonosítót tartalmaz. |
| Szerepkörök | A felhasználó számára engedélyezett Azure Active Directory alkalmazás szerepkörök rövid nevét tartalmazza. |
| Hatókör | Azt jelzi, hogy az ügyfélalkalmazás az engedélyeket. |
| Tárgy | Azt jelzi, hogy a tőketörlesztés mértékét arról, hogy melyik a token asserts információkat. |
| Bérlői azonosító | A címtár bérlő által kiadott a token megváltoztatható, egyedi azonosítót tartalmaz. |
| Élettartam | Megadja az időszakot, amelyeken belül a token érvényes. |
| Egyszerű felhasználónév | A tárgy egyszerű felhasználónév tartalmazza. |
| Verzió | A token verziószámának tartalmazza. |


## <a name="basics-of-registering-an-application-in-azure-ad"></a>Alkalmazás rögzítése az Azure Active Directory alapjai

Bármely alkalmazásban, amely az Azure Active Directory-hitelesítés outsources regisztrálni kell könyvtárában található. Ezt a lépést foglal magában Azure Active Directory jelzi az alkalmazásról, például az URL-cím azt még helyét, az URL-CÍMÉT a válaszüzenetek küldése után a hitelesítés, a URI azonosítja az alkalmazást, és így tovább. Szükség néhány fontosabb okok miatt:

- Azure Active Directory van szüksége a koordináták kommunikálni az alkalmazás, jelentkezzen be- és cserélő tokenek kezelésekor. Ezek a következők:

  - Alkalmazás azonosítója URI: Az alkalmazás azonosítója. Ezt az értéket, hogy melyik alkalmazást a hívó szeretne egy token Azure AD-hitelesítés során a rendszer elküldi. Ezenkívül ez az érték szerepel a jogkivonat, hogy az alkalmazás tudja célja volt.


  - Válasz az URL-CÍMEK és átirányítás URI: webes API vagy webalkalmazás, a válasz URL-CÍMÉT a hely, amelyre Azure Active Directory küld a hitelesítési választ, beleértve a jogkivonat, ha sikeres volt-hitelesítés. Natív alkalmazást, amíg az átirányítás URI egy egyedi azonosítót, amelyhez Azure Active Directory úgy irányítja át a felhasználó-agent OAuth 2.0-összehívásban.


  - Ügyfél-azonosító: Az alkalmazás, amely Azure AD, amikor az alkalmazás van regisztrálva által generált azonosítója. Egy engedélyezési kód vagy token kérésekor Azure AD-hitelesítés során elküldi az ügyfél-azonosító és a kulcsot.


  - Kulcs: A kulcs küldött együtt ügyfél-azonosító hitelesítés során Azure Active Directory hívja fel a webes API-val.


- Azure Active Directory van szüksége, az alkalmazás rendelkezzen a címtár-adatok, a szervezet más alkalmazások és egyéb műveleteket is végezhet a szükséges engedélyekkel

Kiépítési válik világosabb is, ha tisztában fejlett, és Azure Active Directory integrált alkalmazások két kategóriába sorolhatók:

- Egy bérlői alkalmazás: egy egyetlen bérlői alkalmazás lett tervezve egy szervezet. Ezek a szokásos sor üzleti üzletági alkalmazások egy vállalati fejlesztő által megírt. Egy egyetlen bérlői alkalmazást csak kell egy adott könyvtár használhatók, és emiatt csak szüksége van egy adott könyvtár kell építenie. Ezeket az alkalmazásokat általában a szervezet által regisztrált.


- Több elem bérlői alkalmazás: A több elem bérlői alkalmazások használatra szánt számos szervezetnél nem csak egy szervezet. Ezek a szokásos szoftver-mint-a-(szoftver) szolgáltatásalkalmazások egy független szoftverének a szállítójával (külső) által írt. Több elem bérlői alkalmazások kell minden könyvtárban hol lesz, felhasználó vagy a rendszergazda jóváhagyását adja őket regisztrálni igénylő kell építenie. A jóváhagyási folyamat indul el, ha az alkalmazás a címtárban van regisztrálva, és a diagram API vagy akár egy másik webes API kap hozzáférést. Ha egy felhasználó vagy egy másik szervezettől rendszergazda jelentkezik be az alkalmazás használatát, mutatják be azokat egy párbeszédpanel, amely megjeleníti az engedélyeket az alkalmazás segítségével. A felhasználó vagy a rendszergazda is majd beleegyezés az alkalmazás, amely az alkalmazás hozzáférést biztosít a megadott adatok, és végül regisztrál az alkalmazás a címtár. További információ [a beleegyezés keretrendszer áttekintése](active-directory-integrating-applications.md#overview-of-the-consent-framework)című témakörben találhat.

További megfontolandó merülnek fel, a több elem bérlői alkalmazások helyett egy egyetlen bérlői alkalmazás fejlesztésekor. Például ha meg van az alkalmazás elérhetővé tétele a felhasználók több könyvtárban, szeretné határozza meg, mely bérlői is legyenek. Egy egyetlen bérlői csak alkalmazásnak nézheti meg saját könyvtár egy felhasználóhoz, miközben a több elem bérlői alkalmazásnak azonosítása egy adott felhasználó összes könyvtárak az Azure Active Directory. A feladat végrehajtásához az Azure Active Directory egy közös hitelesítési végpontot, ahol bármely több bérlői alkalmazás bejelentkezési kérést, helyett egy bérlői-specifikus végpontot irányítsa biztosít. A végpont az összes könyvtárak https://login.microsoftonline.com/common Azure Active Directory, mivel lehet, hogy egy bérlői-specifikus végpontot https://login.microsoftonline.com/contoso.onmicrosoft.com. A közös végpont különösen fontos az alkalmazás fejlesztésekor, mert szüksége lesz a szükséges logika több bérlők kezeléséhez bejelentkezési, kijelentkezéshez és jogkivonat ellenőrzés során figyelembe.

Ha jelenleg fejlesztéséhez egy egyetlen bérlői alkalmazás, de sok szervezetek számára is elérhetővé szeretné tenni kívánt, könnyen módosíthatja, hogy több elem bérlői legyen az alkalmazás és az Azure Active Directory konfigurációját alkalmas. Ezeken kívül Azure Active Directory használja ugyanazt az aláírási kulcsot összes tokenek található összes, hogy meg van adva a hitelesítési egyetlen bérlői vagy több elem bérlői alkalmazás.

Minden esetben a dokumentumban szerepel egy szakasz a kiépítési követelmények betartását leíró tartalmazza. További információt [Alkalmazások integrálása az Azure Active Directory címtárral](active-directory-integrating-applications.md) talál részletesebb információt a kiépítési az Azure Active Directory-alkalmazást, és egy vagy több elem bérlői alkalmazások közötti különbségeket. Továbbra is az alkalmazás gyakori alkalmazási területek megértéséhez az Azure Active Directory olvasási.

## <a name="application-types-and-scenarios"></a>Alkalmazás-típusok és -esetek

Minden egyes az jelenik meg a dokumentum ismertetett lehet kialakítani a különféle nyelvek és platformokon. Azok az összes készül érhetők el az [Útmutató mintakódok](active-directory-code-samples.md), vagy közvetlenül a megfelelő [Github minta tárházakban](https://github.com/Azure-Samples?utf8=%E2%9C%93&query=active-directory)teljes mintakódok szerint. Ezenkívül ha az alkalmazás egy adott darab vagy egy végpontok közötti forgatókönyv szakaszában, a legtöbb esetben funkció felvehetők egymástól függetlenül. Például ha egy natív alkalmazás, amely felhívja a webes API-val, egyszerűen hozzáadhatja egy webalkalmazás, amely szükségessé teszi a webes API-val. Az alábbi ábra mutatja be, ezeket a forgatókönyveket és alkalmazás típusú, és hogyan különféle összetevőket manuálisan is hozzáadhatók:

![Alkalmazás-típusok és -esetek](./media/active-directory-authentication-scenarios/application_types_and_scenarios.png)

Ezek az Azure Active Directory által támogatott öt elsődleges helyzeteket:

- [Webböngésző webalkalmazás](#web-browser-to-web-application): egy felhasználónak kell jelentkezzen be az Azure Active Directory által biztosított webalkalmazás.

- [Egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés)](#single-page-application-spa): egy felhasználónak kell jelentkezzen be az Azure Active Directory által biztosított egyoldalas alkalmazásba.

- [Webes API-nak tartozó alkalmazásban](#native-application-to-web-api): A natív telefonon, táblagépen vagy PC-n futó alkalmazásnak a felhasználók számára beolvasása a webes API Azure Active Directory által biztosított erőforrások hitelesítést végezni.

- [Webalkalmazás webes API-nak](#web-application-to-web-api): webalkalmazás kell erőforrások beolvasása a webes API védi az Azure Active Directory.

- [Démon és webes API-kiszolgáló alkalmazások](#daemon-or-server-application-to-web-api): egy démonalkalmazásaihoz nem webes felhasználói felülettel kiszolgálói alkalmazások által erőforrások beolvasása a webes API védi az Azure Active Directory.

### <a name="web-browser-to-web-application"></a>Webböngésző webalkalmazáshoz

Ez a szakasz olyan alkalmazás, amely egy webböngészőben webalkalmazás felhasználói hitelesíti ismerteti. Ebben az esetben a webes alkalmazás a felhasználó böngészőt, hogy jelentkezzen be őket az Azure Active Directory irányítja. Azure Active Directory és a felhasználó böngészőben, amely tartalmazza a felhasználó a biztonsági jogkivonat kapcsolatos igények bejelentkezési választ ad eredményül. Ebben az esetben támogatja a bejelentkezés a Webszolgáltatás-Összevonás, a SAML 2.0-s és a csatlakozás OpenID protokollok használatával.


#### <a name="diagram"></a>Diagram
![Hitelesítési folyamat webalkalmazást a böngészőben](./media/active-directory-authentication-scenarios/web_browser_to_web_api.png)


#### <a name="description-of-protocol-flow"></a>Protocol (protokoll) folyamat leírása


1. A felhasználóról, az alkalmazás és a bejelentkezés az igényeinek, amikor azok irányítja a hitelesítési végpont bejelentkezési kérelem keresztül az Azure Active Directory.


2. A felhasználó bejelentkezik a bejelentkezési lapon.


3. Sikeres hitelesítés esetén Azure AD-hitelesítési jogkivonat hoz létre, és az alkalmazás válasz URL-címre az adatkezelési portálon Azure konfigurált bejelentkezési választ ad. Egy gyártási alkalmazáshoz válasz URL-címe HTTPS kell lennie. A visszaadott jogkivonat magában foglalja a felhasználóról és Azure Active Directory követelések, az alkalmazás a token ellenőrzéséhez szükséges.


4. Az alkalmazás használatával egy nyilvános aláírási kulcs kibocsátó elérhető és információk az összevonási metaadat-dokumentum, az Azure Active Directory azt ellenőrzi a token. Miután az alkalmazás ellenőrzi a jogkivonat, Azure AD a felhasználó új munkamenet kezdődik. Ebben a munkamenetben lehetővé teszi, hogy a felhasználó hozzáférhet az alkalmazást, amíg lejárna.


#### <a name="code-samples"></a>Mintakódok


A mintakódok talál webböngészőben webalkalmazás esetek. És ellenőrizze újra gyakran – a mindig új minták hozzáadunk. [Webalkalmazás webböngészőben](active-directory-code-samples.md#web-browser-to-web-application).


#### <a name="registering"></a>Regisztráció


- Egyetlen bérlői webhely esetén: Ha az alkalmazás csak a szervezet számára hoz létre, akkor regisztrálni kell a vállalati címtárban az Azure adatkezelési portál használatával.


- Több elem bérlői webhely esetén: Hoz létre olyan alkalmazás, amely a szervezeten kívüli felhasználókkal használható, ha az a vállalati címtárban regisztrálva kell lenniük, hanem is regisztrálni kell az egyes szervezeti könyvtár, hogy az alkalmazás használata. Elérhetővé szeretné tenni az alkalmazást a címtárban, a regisztrációs folyamat is elhelyezhet, amely lehetővé teszi őket, hogy az alkalmazás az ügyfeleknek. Ha az azok regisztrálni szeretne az alkalmazáshoz, azok egy párbeszédpanel, amely mutatja az alkalmazáshoz szükséges engedélyeket, és a vezérlőt, amellyel beleegyezés formában. Attól függően, hogy a megfelelő engedélyekkel adjon a felhasználó hozzájárul ahhoz a szervezeten belül a rendszergazda is szükség. Amikor a felhasználó vagy a rendszergazda hozzájárul, az alkalmazás regisztrálva van a címtárban. További tudnivalókért lásd: az [Alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Jogkivonat lejárati

A felhasználói munkamenet lejár az Azure Active Directory által kibocsátott jogkivonathoz élettartamának lejártakor. Az alkalmazás a következő időszakban jelölőnégyzetet, ha azt szeretné, például a felhasználók inaktivitás ponttal alapján kijelentkezés is rövidítése. A munkamenet lejár, amikor a felhasználó újra bejelentkezni kérni fogja.





### <a name="single-page-application-spa"></a>Egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés)

Ez a szakasz ismerteti a hitelesítés egyetlen lap alkalmazás, amely felhasználások Azure Active Directory és az OAuth 2.0-s implicit engedély megadása a webes API vissza befejezése biztonságos. Egyetlen lap alkalmazások JavaScript bemutató réteg (előtér), amelyek a böngészőben és a webes API vissza célból, hogy a kiszolgálón fut, és az alkalmazás logikájának megvalósítása fut, a szokásos szerkezete. Ha többet szeretne megtudni az implicit engedély megadása, és úgy dönt, hogy az alkalmazás forgatókönyvet jobb súgó [ismertetése](active-directory-dev-understanding-oauth2-implicit-grant.md)című cikk nyújt az Azure Active Directory, a oauth2 hitelesítési mód implicit támogatási folyamat.

Ebben az esetben amikor a felhasználó bejelentkezik a, a JavaScript Előrehozás vége használja az Active Directory Authentication Library [a JavaScript (ADAL. JS)](https://github.com/AzureAD/azure-activedirectory-library-for-js/tree/dev) és Azure Active Directory-azonosító (id_token) jogkivonat szerzi implicit engedély megadása. A token gyorsítótárazott, és az ügyfél csatolja a kérést, az bearer jogkivonathoz hívása esetén a webes API befejezési amelynek használatával a OWIN köztes védett biztonsági másolatot. 
#### <a name="diagram"></a>Diagram

![Egyetlen lap alkalmazás diagram](./media/active-directory-authentication-scenarios/single_page_app.png)

#### <a name="description-of-protocol-flow"></a>Protocol (protokoll) folyamat leírása

1. A felhasználó megnyitja a webalkalmazást.


2. Az alkalmazás a JavaScript előtér (bemutató réteg) a böngészőben adja vissza.


3. A felhasználónak bejelentkezés, például elindítja a bejelentkezés hivatkozásra kattintva. A böngésző GET küld az Azure Active Directory engedélyezési végpontot-azonosító token kérhet. A kérés az ügyfél azonosítója és válasz URL-cím szerepel a lekérdezés paraméterei.


4. Azure Active Directory ellenőrizte ellen regisztrált válasz URL-CÍMÉT az adatkezelési portálon Azure konfigurált válasz URL-CÍMÉT.


5. A felhasználó bejelentkezik a bejelentkezési lapon.


6. Sikeres hitelesítés esetén Azure AD-azonosító token hoz létre, és adja vissza azt, egy URL-cím fragment (#) az alkalmazás válasz URL-címre. A gyártás-alkalmazást a válasz URL-címe HTTPS kell lennie. A visszaadott jogkivonat magában foglalja a felhasználóról és Azure Active Directory követelések, az alkalmazás a token ellenőrzéséhez szükséges.


7. A böngészőben futó JavaScript-ügyfél kódot a token olvas használata a hívások átirányítása az alkalmazás webes API vissza befejezése biztonságossá tétele a kérdésre adott választ.


8. A böngésző hívja fel az alkalmazás webes API vissza az jogkivonat végződjön a engedélyezési fejlécében.

#### <a name="code-samples"></a>Mintakódok


Lásd: a mintakódok egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés) felhasználási területei. Ne felejtse el gyakran ellenőrizni--mindig új minták hozzáadunk. [Egyetlen lap alkalmazás (biztonságos jelszó-hitelesítés)](active-directory-code-samples.md#single-page-application-spa).


#### <a name="registering"></a>Regisztráció


- Egyetlen bérlői webhely esetén: Ha az alkalmazás csak a szervezet számára hoz létre, akkor regisztrálni kell a vállalati címtárban az Azure adatkezelési portál használatával.


- Több elem bérlői webhely esetén: Hoz létre olyan alkalmazás, amely a szervezeten kívüli felhasználókkal használható, ha az a vállalati címtárban regisztrálva kell lenniük, hanem is regisztrálni kell az egyes szervezeti könyvtár, hogy az alkalmazás használata. Elérhetővé szeretné tenni az alkalmazást a címtárban, a regisztrációs folyamat is elhelyezhet, amely lehetővé teszi őket, hogy az alkalmazás az ügyfeleknek. Ha az azok regisztrálni szeretne az alkalmazáshoz, azok egy párbeszédpanel, amely mutatja az alkalmazáshoz szükséges engedélyeket, és a vezérlőt, amellyel beleegyezés formában. Attól függően, hogy a megfelelő engedélyekkel adjon a felhasználó hozzájárul ahhoz a szervezeten belül a rendszergazda is szükség. Amikor a felhasználó vagy a rendszergazda hozzájárul, az alkalmazás regisztrálva van a címtárban. További tudnivalókért lásd: az [Alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).

Az alkalmazás a regisztrálás után úgy kell beállítania OAuth 2.0-s Implicit támogatás protokoll használatát. A Protocol (protokoll) alkalmazások alapértelmezés szerint le van tiltva. Ahhoz, hogy az alkalmazás oauth2 hitelesítési mód Implicit támogatás protokollt, töltse le az alkalmazás jegyzék az Azure adatkezelési portálról, a "oauth2AllowImplicitFlow" értéket Igaz értékre állítás, és töltse be a nyilvánvalóan vissza a portálra. Részletes útmutatásért lásd: [Segítségével, így OAuth 2.0-s Implicit támogatás egyetlen lap alkalmazásait](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Jogkivonat lejárati

Kezelheti az Azure Active Directory authentication ADAL.js használatakor élvezhetik megkönnyítése frissítése egy lejárt jogkivonat, valamint a tokenek első további webes API-erőforrások, amely szerint az alkalmazás neve lehet többféle funkcióval. Amikor a felhasználó sikeresen hitelesíti az Azure Active Directory, a cookie-k által biztosított munkamenet a felhasználó a böngészős és az Azure Active Directory. Fontos tudni, hogy a munkamenet létezik a felhasználó és Azure Active Directory és nem a felhasználó és a webes alkalmazás, a kiszolgálón futó közötti. Jogkivonat lejártakor ADAL.js csendes szerezze be a másik jogkivonat ebben a munkamenetben használja. Ezt nem küldéséhez és fogadásához a kérést, az OAuth Implicit támogatás protokollon keresztül a rejtett iFrame használatával. ADAL.js csendes hozzáférési jogkivonat szerzi Azure Active Directory más webes API-erőforrások, hogy az alkalmazás hívások mindaddig, amíg az ezek az erőforrások támogatja az idegen származási erőforrás megosztása (CORS), regisztrálva van a felhasználó a címtárban, és minden szükséges kifejezett engedélyét adta-e meg a felhasználó által bejelentkezés során ugyanezzel a módszerrel is használhatja.


### <a name="native-application-to-web-api"></a>A webes API tartozó alkalmazásban


Ez a szakasz ismerteti a natív alkalmazás, amely a webes API felhasználó nevében hívja. Ebben az esetben az OAuth 2.0-s engedélyezési kód megadása típusát ahhoz, hogy egy nyilvános ügyféllel épül az [OAuth 2.0-s specifikációja](http://tools.ietf.org/html/rfc6749)4.1 szakaszban leírt módon. Az eredeti alkalmazást-hozzáférési jogkivonat, a felhasználó megkapja a OAuth 2.0-s protokoll használatával. Ez a jogkivonat majd küldi az értekezlet-összehívást a webes API-val, amely engedélyezi a felhasználó és a kívánt erőforrás adja eredményül.

#### <a name="diagram"></a>Diagram

![Natív alkalmazás webes API-Diagram](./media/active-directory-authentication-scenarios/native_app_to_web_api.png)

#### <a name="authentication-flow-for-native-application-to-api"></a>Hitelesítési folyamat tartozó alkalmazásban API-hoz

#### <a name="description-of-protocol-flow"></a>Protocol (protokoll) folyamat leírása


Ha használja az Active Directory Authentication tárak, a protokoll adatai leírása alább a legtöbb kezelése, például a böngésző előugró ablak, jogkivonat gyorsítótár és frissítés tokenek kezelését.

1. Az eredeti alkalmazást az engedélyezés végpontot kérelmének módosít az Azure Active Directory előugró böngészőt használ. A kérés az ügyfél-azonosító és a az átirányítást az eredeti alkalmazást URI-tartalmazza az adatkezelési portál és az alkalmazás azonosítója URI a webes API látható módon. Ha a felhasználó nem már be van jelentkezve, jelentkezzen be újra szeretné kérni


2. Azure Active Directory hitelesíti a felhasználót. Ha a több elem bérlői-alkalmazások és a felhasználó hozzájárul ahhoz az alkalmazás használatához szükséges, a felhasználónak kötelező beleegyezés, ha az azok még nem tette meg. Engedély megadása után, és a sikeres hitelesítés esetén az Azure AD vissza az ügyfélalkalmazás átirányítási URI a kód engedélyezési választ hibák.


3. Azure AD vissza az átirányítási URI a kód engedélyezési választ problémák, ha az ügyfélalkalmazás leállítja a böngésző kapcsolati, és a engedélyezési kód olvas a kérdésre adott választ. Engedély kód használata esetén az ügyfélalkalmazás egy kérést küld Azure Active Directory jogkivonat végpontot, amely magában foglalja a engedélyezési kód, a részleteket az ügyfélalkalmazás (ügyfél-azonosító és átirányítási URI), és a kívánt erőforrás (alkalmazás azonosítója URI a webes API-val).


4. A engedélyezési kód és az ügyfél alkalmazás és a webes API-val kapcsolatos információk érvényesíteni Azure AD. Frissítéskor sikeres érvényesítés, Azure Active Directory adja vissza két tokenek: egy JWT jogkivonat, és egy JWT frissítés token. Ezeken kívül Azure AD a felhasználó számára, például az megjelenítendő nevét és a bérlői azonosítójuk. egyszerű adatokat ad eredményül.


5. HTTPS az ügyfélalkalmazás a visszaadott JWT jogkivonat használja a kérelem engedélyezési fejlécében a JWT karakterlánc, egy "Bearer" megnevezéssel hozzáadása a webes API-val. A webes API majd ellenőrzi a JWT jogkivonat, és ha érvényesítése sikeres, ad eredményül a kívánt erőforrás.


6. Amikor lejár az jogkivonat, a ügyfélalkalmazás hibaüzenetet kap, amely jelzi, hogy a felhasználónak kell újra hitelesítést végezni. Ha az alkalmazás egy érvényes frissítési jogkivonat, szerezheti be egy új jogkivonat, a felhasználót, hogy jelentkezzen be újra a rákérdezés nélkül használható. Ha a frissítés jogkivonat lejár, az alkalmazás kell interaktív csatlakozó felhasználók hitelesítését még egyszer.


> [AZURE.NOTE] A frissítés jogkivonat Azure Active Directory által kibocsátott több forrásokat is használható. Például, hogy hívja fel a két webes API-k hozzáférési jogosultsággal rendelkező ügyfélalkalmazás esetén a frissítés jogkivonat az egy férhet jogkivonat más webes API-val is használható.


#### <a name="code-samples"></a>Mintakódok


Lásd: a mintakódok a webes API esetek tartozó alkalmazásban. És ellenőrizze újra gyakran – a mindig új minták hozzáadunk. [A webes API tartozó alkalmazásban](active-directory-code-samples.md#native-application-to-web-api).


#### <a name="registering"></a>Regisztráció


- Egyetlen bérlői webhely esetén: A két a natív alkalmazás és a webes API regisztrálni kell ugyanabban a mappában Azure AD. A webes API beállítható úgy, hogy az engedélyek korlátozása az erőforrásokhoz tartozó alkalmazásban csatlakozni használt csoportja jelenítik meg. Az ügyfélalkalmazás majd a kívánt engedélyeket a "Engedélyeket szeretne egyéb alkalmazások" legördülő menüben az adatkezelési portálon Azure kijelölése


- Több elem bérlői webhely esetén: Először az eredeti alkalmazást csak minden eddiginél regisztrálni a fejlesztői vagy a publisher-címtárból. Második az eredeti alkalmazást van konfigurálva, hogy a funkcióira szükséges engedélyekkel. A szükséges engedélyekkel listája látható egy párbeszédpanel, amikor a felhasználó vagy a rendszergazdának a cél címtárban hozzájárulása biztosít az alkalmazás, amely a szervezet számára elérhetővé teszi azt. Egyes alkalmazások csak a felhasználói szintű engedélyek, amelyek a szervezet bármely felhasználója van szükség. Egyéb alkalmazások rendszergazdai jogosultsággal, amelyek a szervezet egy felhasználója nem lehet szükség. Csak a címtár rendszergazdája adhat hozzájárulása Ez az engedélyszint az engedélyeket igénylő alkalmazásokhoz. Amikor a felhasználó vagy a rendszergazda hozzájárul, csak a webes API van regisztrálva a címtárban. További tudnivalókért lásd: az [Alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).


#### <a name="token-expiration"></a>Jogkivonat lejárati


Ha az eredeti alkalmazást az engedélyezési kód segítségével hozzáférhet a JWT jogkivonat, is kap egy JWT frissítés token. Amikor lejár az jogkivonat, a frissítés jogkivonat újra csatlakozó felhasználók hitelesítését anélkül, jelentkezzen be újra a használható. A frissítés majd jogkivonat hitelesíteni a felhasználót, amelynek eredménye egy új jogkivonat és frissítés jogkivonat.





### <a name="web-application-to-web-api"></a>Webalkalmazás webes API


Ez a szakasz ismerteti a források beolvasása a webes API kell, hogy webalkalmazás. Ebben az esetben, amely a webes alkalmazás segítségével hitelesítést végezni, és hívja fel a webes API két identitás-típusba sorolhatók: az alkalmazás azonosítója, illetve a meghatalmazott felhasználói azonosítót.

*Alkalmazás azonosítója:* Ebben a példában OAuth 2.0 ügyfél hitelesítő adatok támogatás hitelesítést végezni, mint az alkalmazást, és a webes API eléréséhez. Az alkalmazás azonosítója, a webes API csak azt észleli, hogy a webalkalmazás, hívásához használata esetén, a webes API nem kapja meg a felhasználó minden információt. Ha az alkalmazás a felhasználó adatai kap, az alkalmazás protokollon keresztül küld, és nem aláíró Azure AD. A megbízik a webes API-val, hogy a webalkalmazás hitelesített felhasználó. Emiatt a minta egy megbízható alrendszer neve.

*Felhasználóazonosító meghatalmazott:* Ebben az esetben kétféle módon lehet elvégezni: OpenID csatlakozhat és OAuth 2.0-s engedélyezési kód megadása bizalmas ügyféllel. A webes alkalmazás szerez egy jogkivonat, a felhasználó kiderül, hogy a webes API-val, amely a webalkalmazás sikeresen hitelesített felhasználó és a webes alkalmazás nem kapnak, hívja fel a webes API delegált felhasználói azonosítót. Ez a jogkivonat a webes API-val, amely engedélyezi a felhasználó és a kívánt erőforrás adja vissza az értekezlet-összehívást küldi.

#### <a name="diagram"></a>Diagram

![Webalkalmazás webes API-diagramhoz](./media/active-directory-authentication-scenarios/web_app_to_web_api.png)



#### <a name="description-of-protocol-flow"></a>Protocol (protokoll) folyamat leírása

Az alkalmazás azonosítója és a meghatalmazott felhasználói azonosító típusú a folyamat az alábbi tárgyalja. A fő különbségét, hogy a meghatalmazott felhasználóazonosító először kell szerezni egy engedélyezési kód, mielőtt a felhasználó bejelentkezési és a webes API eléréséhez szükséges.

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Alkalmazás azonosítója OAuth 2.0 ügyfél hitelesítő adatok megadása

1. A felhasználó be van jelentkezve a webalkalmazás az Azure Active Directory (lásd: a [Webböngészőben webalkalmazás](#web-browser-to-web-application) fenti).


2. A webes alkalmazás kell szerezheti be egy hozzáférési jogkivonat, így hitelesíteni a webes API-val és a kívánt erőforrás beolvasásához. Azure Active Directory jogkivonat végpontra a hitelesítő adatokat, az ügyfél-azonosító és a webes API-alkalmazás azonosítója URI van egy kérelmet.


3. Azure AD az alkalmazás hitelesíti, és egy JWT hozzáférési jogkivonat, hívja fel a webes API-val használt adja eredményül.


4. HTTPS a webalkalmazás az JWT visszaküldött jogkivonat használja a kérelem engedélyezési fejlécében a JWT karakterlánc, egy "Bearer" megnevezéssel hozzáadása a webes API. A webes API majd ellenőrzi a JWT jogkivonat, és ha érvényesítése sikeres, ad eredményül a kívánt erőforrás.

##### <a name="delegated-user-identity-with-openid-connect"></a>Csatlakozás a OpenID delegált felhasználóazonosító

1. A felhasználó be van jelentkezve egy webalkalmazás használatával az Azure Active Directory (lásd a fenti [Webböngészőben webalkalmazás](#web-browser-to-web-application) szakaszt). A webes alkalmazás a felhasználó még nem járult hozzá lehetővé teszi a webalkalmazás, hívja fel a webes API nevében, ha a felhasználónak kell beleegyezés. Az alkalmazás megjeleníti az ehhez szükséges engedélyek, és ha ezek között rendszergazdai engedélyekkel, normál felhasználó a címtárban nem tudják beleegyezés. A jóváhagyási folyamat csak érvényes több bérlői alkalmazások, nem egyetlen bérlői alkalmazások, az alkalmazás rendelkezik a szükséges engedélyekkel. A felhasználó bejelentkezve, a webalkalmazás kapott egy azonosító jogkivonat, a felhasználót, valamint egy engedélyezési kód vonatkozó információkkal.


2. Az Azure Active Directory által kibocsátott engedélyezési kód használata esetén a webalkalmazás egy kérést küld Azure Active Directory jogkivonat végpontot, amely magában foglalja a engedélyezési kód, a részleteket az ügyfélalkalmazás (ügyfél-azonosító és átirányítási URI), és a kívánt erőforrás (alkalmazás azonosítója URI a webes API-val).


3. A engedélyezési kód és a webes alkalmazáshoz, és a webes API-val kapcsolatos információk érvényesíteni Azure AD. Frissítéskor sikeres érvényesítés, Azure Active Directory adja vissza két tokenek: egy JWT jogkivonat, és egy JWT frissítés token.


4. HTTPS a webes alkalmazás segítségével a visszaadott JWT jogkivonat a kérelem engedélyezési fejlécében a JWT karakterlánc, egy "Bearer" megnevezéssel hozzáadása a webes API-val. A webes API majd ellenőrzi a JWT jogkivonat, és ha érvényesítése sikeres, ad eredményül a kívánt erőforrás.

##### <a name="delegated-user-identity-with-oauth-20-authorization-code-grant"></a>Delegált felhasználóazonosító az OAuth 2.0-s engedélyezési kód megadása

1. A felhasználó van már be van jelentkezve egy webalkalmazás, amelynek hitelesítési módszer független Azure AD.


2. A webes alkalmazás egy engedélyezési kód szerezheti be egy hozzáférési jogkivonat, így a böngészőn keresztül kérelmének általa Azure Active Directory engedélyezési végpontra, mert az ügyfél-azonosító és URI átirányítása a webalkalmazás után sikeres hitelesítés szükséges. A felhasználó bejelentkezik Azure Active Directory.


3. A webes alkalmazás a felhasználó még nem járult hozzá lehetővé teszi a webalkalmazás hívja fel a webes API nevében, ha a felhasználónak kell beleegyezés. Az alkalmazás megjeleníti az ehhez szükséges engedélyek, és ha ezek között rendszergazdai engedélyekkel, normál felhasználó a címtárban nem tudják beleegyezés. A jóváhagyási folyamat csak érvényes több bérlői alkalmazások, nem egyetlen bérlői alkalmazások, az alkalmazás rendelkezik a szükséges engedélyekkel.


4. A felhasználó hozzájárult, miután a webalkalmazás szüksége van egy hozzáférési jogkivonat szerezheti be engedélyt kódot kapja meg.


5. Az Azure Active Directory által kibocsátott engedélyezési kód használata esetén a webes alkalmazás egy kérést küld Azure Active Directory jogkivonat végpontot, amely magában foglalja a engedélyezési kód, a részleteket az ügyfélalkalmazás (ügyfél-azonosító és átirányítási URI), és a kívánt erőforrás (alkalmazás azonosítója URI a webes API-val).


6. A engedélyezési kód és a webes alkalmazáshoz, és a webes API-val kapcsolatos információk érvényesíteni Azure AD. Frissítéskor sikeres érvényesítés, Azure Active Directory adja vissza két tokenek: egy JWT jogkivonat, és egy JWT frissítés token.


7. HTTPS a webalkalmazás az JWT visszaküldött jogkivonat használja a kérelem engedélyezési fejlécében a JWT karakterlánc, egy "Bearer" megnevezéssel hozzáadása a webes API. A webes API majd ellenőrzi a JWT jogkivonat, és ha érvényesítése sikeres, ad eredményül a kívánt erőforrás.

#### <a name="code-samples"></a>Mintakódok

Lásd: a mintakódok webes API esetek webalkalmazást. És ellenőrizze újra gyakran – a mindig új minták hozzáadunk. Webes [webes API alkalmazást](active-directory-code-samples.md#web-application-to-web-api).


#### <a name="registering"></a>Regisztráció

- Egyetlen bérlői webhely esetén: Az alkalmazás azonosítója mind a meghatalmazott felhasználói azonosító esetek a webes alkalmazás és a webes API regisztrálni kell ugyanabban a mappában az Azure Active Directory. A webes API beállítható úgy, hogy az jelenítik meg az engedélyek, korlátozhatja a hozzáférést a webalkalmazás az erőforrások használt csoportja. A meghatalmazott felhasználói azonosító típusú használatakor az webalkalmazás kell az adatkezelési portálon Azure "Engedélyeket szeretne egyéb alkalmazások" legördülő menüből válassza a kívánt engedélyeket. Ha az alkalmazás azonosító típusa van használatban, ez a lépés nem szükséges.


- Több elem bérlői: Először a webalkalmazás van konfigurálva, hogy a funkcióira szükséges engedélyekkel. A szükséges engedélyekkel listája látható egy párbeszédpanel, amikor a felhasználó vagy a rendszergazdának a cél címtárban hozzájárulása biztosít az alkalmazás, amely a szervezet számára elérhetővé teszi azt. Egyes alkalmazások csak a felhasználói szintű engedélyek, amelyek a szervezet bármely felhasználója van szükség. Egyéb alkalmazások rendszergazdai jogosultsággal, amelyek a szervezet egy felhasználója nem lehet szükség. Csak a címtár rendszergazdája adhat hozzájárulása Ez az engedélyszint az engedélyeket igénylő alkalmazásokhoz. Amikor a felhasználó vagy a rendszergazda hozzájárul, a webes alkalmazás és a webes API mindkét regisztrált a címtárban.

#### <a name="token-expiration"></a>Jogkivonat lejárati

Ha a webalkalmazás az engedélyezési kód segítségével hozzáférhet a JWT jogkivonat, is kap egy JWT frissítés token. Amikor lejár az jogkivonat, a frissítés jogkivonat újra csatlakozó felhasználók hitelesítését anélkül, jelentkezzen be újra a használható. A frissítés majd jogkivonat hitelesíteni a felhasználót, amelynek eredménye egy új jogkivonat és frissítés jogkivonat.


### <a name="daemon-or-server-application-to-web-api"></a>Démon vagy a webes API alkalmazáskiszolgáló


Ez a szakasz ismerteti a démon vagy a kiszolgáló-alkalmazások erőforrások beolvasása a webes API kell, hogy a. Forgatókönyv két alszint ebben a szakaszban alkalmazása: hívja fel a webes API-val, OAuth 2.0-s ügyféltípus hitelesítő adatok megadása; épül kell, hogy a démon és kiszolgálói alkalmazásnak (például a webes API-val) kell hívnia webes API-val, OAuth 2.0-s On-Behalf-Of piszkozat specifikációja épül.

Az alkalmazási példát, amikor egy démonalkalmazásaihoz van szüksége, hívja fel a webes API, fontos lehet áttekinteni néhány dolog, amit. Első lépésként felhasználói beavatkozás esetén nem lehet a démonalkalmazásaihoz, amely előírja, hogy a saját magát az alkalmazás. Példa egy démonalkalmazásaihoz egy köteg feladatot, vagy a háttérben futó operációs rendszer szolgáltatásainak. Alkalmazás a következő típusú egy hozzáférési jogkivonat kéri az alkalmazás identitása használatával, és az ügyfél bemutatót tart, Azonosítót, a hitelesítő adatok (a jelszó vagy a tanúsítvány) és az alkalmazás azonosítója URI Azure ad. A hitelesítés sikeres után a démon kap egy hozzáférési jogkivonat Azure Active Directory, majd hívja fel a webes API-val használt.

Eset kiszolgálói alkalmazás kell hívja fel a webes API, amikor célszerű használni egy példát. Tegyük fel, hogy egy tartozó alkalmazásban a hitelesített felhasználó, és a natív alkalmazást kell hívja fel a webes API. Azure AD egy JWT jogkivonat, hívja fel a webes API hibák. Ha egy másik későbbi webes API hívja fel a webes API-t, azt segítségével a nevében – az áramlás meghatalmazási a felhasználói azonosító, és a második szintű webes API hitelesíteni.

#### <a name="diagram"></a>Diagram

![Démon vagy kiszolgálói alkalmazás webes API-diagramhoz](./media/active-directory-authentication-scenarios/daemon_server_app_to_web_api.png)

#### <a name="description-of-protocol-flow"></a>Protocol (protokoll) folyamat leírása

##### <a name="application-identity-with-oauth-20-client-credentials-grant"></a>Alkalmazás azonosítója OAuth 2.0 ügyfél hitelesítő adatok megadása

1. Első lépésként a kiszolgáló alkalmazást kell hitelesítést végezni, például egy interaktív bejelentkezési párbeszédpanel az emberi beavatkozás nélkül, magát, az Azure AD. Azure Active Directory jogkivonat végpontra kezeléséről a hitelesítő adatait, az ügyfél-azonosító és az alkalmazás azonosítója URI van egy kérelmet.


2. Azure AD az alkalmazás hitelesíti, és egy JWT hozzáférési jogkivonat, hívja fel a webes API-val használt adja eredményül.


3. HTTPS a webalkalmazás az JWT visszaküldött jogkivonat használja a kérelem engedélyezési fejlécében a JWT karakterlánc, egy "Bearer" megnevezéssel hozzáadása a webes API. A webes API majd ellenőrzi a JWT jogkivonat, és ha érvényesítése sikeres, ad eredményül a kívánt erőforrás.


##### <a name="delegated-user-identity-with-oauth-20-on-behalf-of-draft-specification"></a>Az OAuth 2.0-s verziója a nevében – az piszkozat specifikációja delegált felhasználóazonosító

A folyamat itt tárgyalt feltételezi, hogy egy másik alkalmazás (például a saját alkalmazások) a felhasználó hitelesítése megtörtént, és a felhasználó személyazonosságot szerezheti be az első szintű webes API-jogkivonat használták.

1. Az eredeti alkalmazást az jogkivonat küld az első szintű webes API-val.


2. Az első szintű webes API küld egy jogkivonat Azure AD-végpontot, az ügyfél kezeléséről az azonosító és a hitelesítő adatait, valamint a felhasználói jogkivonat. Ezenkívül a kérelem küld-e egy on_behalf_of paraméter, amely jelzi a webes API lefelé irányuló webes API-t az eredeti felhasználó nevében hívja fel az új tokenek kér.


3. Azure Active Directory ellenőrzi, hogy az első szintű webes API rendelkezik engedélyekkel a második szintű webes API eléréséhez, és ellenőrzi a kérelmet, visszaadása egy JWT jogkivonat, és egy JWT frissítése az első szintű webes API jogkivonat.


4. HTTPS az első szintű webes API majd felhívja a második szintű webes API által hozzáfűzése a token karakterláncot a engedélyezési fejlécben a kérést. Az első szintű webes API mindaddig, amíg a hozzáférési jogkivonat, és a frissítési tokenek érvényesek, hívja fel a második szintű webes API-val is.

#### <a name="code-samples"></a>Mintakódok

Tekintse meg a mintakódok démon vagy a webes API esetek alkalmazáskiszolgáló. És ellenőrizze újra gyakran – a mindig új minták hozzáadunk. [Kiszolgáló vagy a webes API démonalkalmazásaihoz](active-directory-code-samples.md#server-or-daemon-application-to-web-api)

#### <a name="registering"></a>Regisztráció

- Egyetlen bérlői webhely esetén: Az alkalmazás azonosítója mind a meghatalmazott felhasználói azonosító esetek a démon vagy server alkalmazás regisztrálni kell ugyanabban a mappában az Azure Active Directory. A webes API beállítható úgy, hogy az engedélyek korlátozása a démon vagy az erőforrások kiszolgáló elérése használt csoportja nézetéhez. A meghatalmazott felhasználói azonosító típusú használatakor a kiszolgáló alkalmazás kell az adatkezelési portálon Azure "Engedélyeket szeretne egyéb alkalmazások" legördülő menüből válassza a kívánt engedélyeket. Ha az alkalmazás azonosító típusa van használatban, ez a lépés nem szükséges.


- Több elem bérlői: Először a démon vagy server alkalmazás van konfigurálva, hogy a funkcióira szükséges engedélyekkel. A szükséges engedélyekkel listája látható egy párbeszédpanel, amikor a felhasználó vagy a rendszergazdának a cél címtárban hozzájárulása biztosít az alkalmazás, amely a szervezet számára elérhetővé teszi azt. Egyes alkalmazások csak a felhasználói szintű engedélyek, amelyek a szervezet bármely felhasználója van szükség. Egyéb alkalmazások rendszergazdai jogosultsággal, amelyek a szervezet egy felhasználója nem lehet szükség. Csak a címtár rendszergazdája adhat hozzájárulása Ez az engedélyszint az engedélyeket igénylő alkalmazásokhoz. Amikor a felhasználó vagy a rendszergazda hozzájárul, mind a webes API-khoz regisztrált a címtárban.

#### <a name="token-expiration"></a>Jogkivonat lejárati

Ha az első alkalmazást az engedélyezési kód segítségével hozzáférhet a JWT jogkivonat, is kap egy JWT frissítés token. Amikor lejár az jogkivonat, a frissítés jogkivonat újból hitelesíteni a felhasználó hitelesítő adatait rákérdezés nélkül használható. A frissítés majd jogkivonat hitelesíteni a felhasználót, amelynek eredménye egy új jogkivonat és frissítés jogkivonat.

## <a name="see-also"></a>Lásd még:

[Azure Active Directory fejlesztői útmutató](active-directory-developers-guide.md)

[Azure Active Directory mintakódok](active-directory-code-samples.md)

[Fontos információkat a fő átváltási az Azure Active Directory-PAL kapcsolatban](active-directory-signing-key-rollover.md)

[OAuth 2.0-s az Azure Active Directory](https://msdn.microsoft.com/library/azure/dn645545.aspx)
