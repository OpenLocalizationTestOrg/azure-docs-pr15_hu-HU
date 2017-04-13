<properties
   pageTitle="Azure Active Directory Fejlesztőeszközök szószedet |} Microsoft Azure"
   description="Feltételek a gyakran használt Azure Active Directory Fejlesztőeszközök fogalmak és szolgáltatások listája."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/31/2016"
   ms.author="bryanla"/>

# <a name="azure-active-directory-developer-glossary"></a>Azure Active Directory Fejlesztőeszközök szószedet
Ez a cikk néhány alapvető Azure Active Directory (AD) fejlesztői fogalmak, amelyek hasznos, amikor az Azure Active Directory alkalmazások fejlesztése információk tartalmazza.

## <a name="access-token"></a>hozzáférési jogkivonat
A [biztonsági jogkivonat](#security-token) típusú [engedélyezési kiszolgáló](#authorization-server)által kibocsátott használt, és egy [ügyfélalkalmazás](#client-application) [védett erőforrás-kiszolgáló](#resource-server)elérése érdekében. A szokásos egy [JSON webes jogkivonat (JWT)]formájában[JWT], a jogkivonat, úgy, az ügyfél számára az [erőforrás-tulajdonos](#resource-owner), a kért szintű hozzáférés engedélyt. A token minden esetben [követelések](#claim) tárgyú, az ügyfélalkalmazás használata a hitelesítő adatok űrlapként, amikor egy adott erőforráshoz engedélyezése tartalmazza. Ez is szükségtelenné kattintva jelenítse meg a hitelesítő adatait, és az ügyfél erőforrás tulajdonosa.

Hozzáférési jogkivonat is nevezik "Felhasználó + alkalmazás" vagy "App csak", attól függően, hogy a hitelesítő adatok éppen jelöli. Például amikor egy ügyfél alkalmazást használja a:

- Az erőforrás tulajdonosaként, az erőforrás eléréséhez az ügyfélnek engedélyezési delegálása először ["Engedélyezési kód" engedélyt adni](#authorization-grant), hitelesíti a végfelhasználó. Az ügyfél hitelesíti testreszabásokat beszerzése a hozzáférési jogkivonat. A token is lehet néven pontosabban a "Felhasználó + alkalmazás" jogkivonat, mint ez azt jelenti, hogy mindkét, hogy az ügyfélalkalmazás és az alkalmazás a hitelesített felhasználó.
- ["Ügyfél hitelesítő adatait" engedélyt adni](#authorization-grant), az ügyfél biztosít a kizárólagos hitelesítés működését anélkül, hogy az erőforrás-tulajdonos hitelesítési/engedélyezési, így a token is előfordul, hogy a továbbiakban: egy "Csak alkalmazás" jogkivonat.

[Azure Active Directory jogkivonat – segédlet] című témakörben[ AAD-Tokens-Claims] további információt.

## <a name="application-manifest"></a>alkalmazás jegyzék  
Az [Azure klasszikus portal]által nyújtott szolgáltatás[AZURE-classic-portal], az Identitáskezelés alkalmazást, a hozzá társított [alkalmazás] frissítése egy mechanizmusként használt JSON ábrázolása állít elő[ AAD-Graph-App-Entity] és [ServicePrincipal] [ AAD-Graph-Sp-Entity] szervezetek. [Az Azure Active Directory-alkalmazás jegyzék ismertetése] című cikk nyújt[ AAD-App-Manifest] további információt.

## <a name="application-object"></a>objektum  
Amikor, külső.FÜGGV és frissítés az alkalmazások az [Azure klasszikus portál][AZURE-classic-portal], a portálon hoz létre/frissítések alkalmazás objektum és a megfelelő [szolgáltatás fő objektumot](#service-principal-object) is, hogy a bérlői webhelyen. Az alkalmazás objektum *határozza meg* az alkalmazást egy sablont, ahonnan a megfelelő szolgáltatás fő objektum(ok) *származtatott* helyileg futásidőben (az adott bérlői webhelyre) használatát biztosító identitás konfigurációs globálisan (ahol hozzáférése van az összes bérlők), át.

Lásd: az [alkalmazás és a szolgáltatás egyszerű objektumok] [ AAD-App-SP-Objects] további információt.

## <a name="application-registration"></a>alkalmazás regisztráció  
Ahhoz, hogy az alkalmazás integrálása és Azure Active Directory identitás és a hozzáférés kezelése a függvények delegálása, hogy regisztrálva kell egy Azure AD- [bérlő](#tenant). Amikor regisztrál az alkalmazás az Azure Active Directory, meg van adva az identitás-konfiguráció az alkalmazás lehetővé teszi, hogy integrálása az Azure Active Directory és funkciók használhatók, például:

- Az egyszeri bejelentkezés az Azure Active Directory-Identitáskezelés és [Csatlakozás OpenID] hatékony kezelési[ OpenIDConnect] protokoll végrehajtása
- [Erőforrások védett](#resource-server) [ügyfélalkalmazásokban](#client-application)keresztül Azure Active Directory OAuth 2.0-s [engedélyezési](#authorization-server) kiszolgálókkal brokered eléréséhez
- Az ügyfélhozzáférés védett erőforrásokhoz, erőforrás-tulajdonosi engedély alapján kezelésére szolgáló [framework beleegyezés](#consent) .

Lásd: [az Azure Active Directory integrálása alkalmazások] [ AAD-Integrating-Apps] további információt.

## <a name="authentication"></a>hitelesítés
Az act, egy fél jogos hitelesítő adatokat, az alap kezeléséről az identitás- és a hozzáférés-vezérlés használandó rendszerbiztonsági létrehozásának ellen. Során az [oauth2 hitelesítési mód engedélyt adhatnak](#authorization-grant) például a hitelesítő fél tölti ki a szerepkört, [erőforrás-tulajdonos](#resource-owner) vagy a [ügyfélalkalmazás](#client-application)attól függően, hogy a támogatáshoz használható.

## <a name="authorization"></a>engedély
Az act, egy hitelesített biztonsági fő engedélyt szeretne elhelyezni. Az Azure Active Directory programozási modell van két fő használati eset:

- Az [Adja meg az oauth2 hitelesítési mód engedélyezési](#authorization-grant) folyamat során: az [erőforrás-tulajdonosi](#resource-owner) engedély [ügyfélalkalmazás](#client-application)ad, amikor lehetővé teszi az ügyfél számára, hogy az erőforrás tulajdonosa forrásokat.
- Során az ügyfél által erőforrás-elérés: az [erőforrás-kiszolgáló](#resource-server)megvalósítása, használatával [formál](#claim) értékek ábrázolása a [hozzáférési jogkivonat](#access-token) access vezérlő alapján döntéseket őket.

## <a name="authorization-code"></a>engedélyezési kód
Rövid életben volt "jogkivonat" [ügyfélalkalmazás](#client-application) által biztosított [engedélyezési végpontot](#authorization-endpoint), a "engedélyezési kód" folyamat részeként közül a négy oauth2 hitelesítési mód [engedélyezése lehetőséget biztosít](#authorization-grant). A kódot a rendszer az ügyfélalkalmazás egy [erőforrás tulajdonosa](#resource-owner), jelezve, hogy az erőforrás tulajdonosa a kért erőforrások hozzáférés engedélyezése rendelkezik-e meghatalmazott hitelesítési válaszul adja vissza. A folyamat részeként a kód később beváltott egy [hozzáférési jogkivonat](#access-token).

## <a name="authorization-endpoint"></a>engedély végpont
A végpontok [engedélyezési server](#authorization-server), használt vezérléséhez az [erőforrás-tulajdonos](#resource-owner) során az oauth2 hitelesítési mód engedély- [engedély megadása](#authorization-grant) a biztosítása érdekében végrehajtania egyik adja meg az adatfolyam. Attól függően, hogy a engedélyezési támogatási folyamat használja a megadott tényleges támogatás változhat, beleértve az [engedélyezési kód](#authorization-code) vagy a [biztonsági jogkivonat](#security-token).

Lásd: az oauth2 hitelesítési mód beállítások [engedélyt adhatnak típusok] [ OAuth2-AuthZ-Grant-Types] és [engedélyezési végpont] [ OAuth2-AuthZ-Endpoint] szakaszok és a [OpenIDConnect specifikációja] [ OpenIDConnect-AuthZ-Endpoint] további információt.

## <a name="authorization-grant"></a>engedély megadása
A hitelesítő adatait, amely az [erőforrás tulajdonosi](#resource-owner) [engedély](#authorization) védett erőforrásait [ügyfélalkalmazás](#client-application)nyújtott eléréséhez. Ügyfélalkalmazás használhatja az [oauth2 hitelesítési mód engedélyezési keretében által meghatározott négyféle támogatás] [ OAuth2-AuthZ-Grant-Types] beszerzése támogatás, attól függően, hogy az ügyfél típusú/követelmények: "engedély kód megadása", "ügyfél hitelesítő adatok megadása", "implicit támogatás" és "erőforrás tulajdonosa jelszó hitelesítő adatok megadása". A hitelesítő adatok adja vissza az ügyfél- [hozzáférési jogkivonat](#access-token), vagy egy [engedélyezési kód](#authorization-code) (cserélni később az egy hozzáférési jogkivonat), a használt engedély megadása típusától függően.

## <a name="authorization-server"></a>engedély kiszolgáló
Az [Oauth2 hitelesítési mód engedélyezési keretrendszer]által meghatározott[OAuth2-Role-Def], az access kiadására kiszolgáló jogkivonatok az [ügyfél](#client-application) sikeresen hitelesítése az [erőforrás-tulajdonos](#resource-owner) és engedélyét beszerzése után. A engedélyezési kiszolgálón keresztül az [Engedélyezés](#authorization-endpoint) futásidőben kommunikáljon egy [ügyfélalkalmazás](#client-application) és [jogkivonat](#token-endpoint) végpontok, az oauth2 hitelesítési mód megfelelően definiált [engedélyt ad](#authorization-grant).

Azure AD-alkalmazás integráció, amíg Azure Active Directory alkalmazza az engedélyezés kiszolgálói szerepkör az Azure Active Directory-alkalmazások és a Microsoft-szolgáltatás API-hoz, például [Microsoft Graph API -khoz][Microsoft-Graph].

## <a name="claim"></a>igénylése
A [biztonsági jogkivonat](#security-token) követelések, amelyek Előfeltételek egy személy (például [ügyfélalkalmazás](#client-application) vagy [az erőforrás-tulajdonos](#resource-owner)) tartalmaz, egy másik személyhez (például az [erőforrás-kiszolgáló](#resource-server)). Követelések név/érték összekapcsolásával, amely az alapvető tudnivalók a token tárgyát (például a rendszerbiztonsági tag, amely a [engedélyezési kiszolgáló](#authorization-server)által hitelesített) közvetítése. A bemutató egy adott jogkivonat jogcímalapú többváltozós, például a típus jogkivonat, a tárgyat, az alkalmazás-konfiguráció stb hitelesíti hitelesítő adatok típusától függnek.

Lásd: [Azure AD-jogkivonat hivatkozás] [ AAD-Tokens-Claims] további információt.

## <a name="client-application"></a>ügyfélalkalmazás  
Az [Oauth2 hitelesítési mód engedélyezési keretrendszer]által meghatározott[OAuth2-Role-Def], olyan alkalmazás, amely lehetővé teszi a védett erőforrás összehívásokat, az [erőforrás-tulajdonos](#resource-owner)nevében. A "ügyfél" kifejezés nem jelenti azt bármely adott hardver végrehajtása jellemzők (például, hogy az alkalmazás végrehajtja a kiszolgálón, asztali vagy más eszközökre).  

Ügyfélalkalmazás [engedélyt](#authorization) kér a erőforrás tulajdonosával részt egy [Adja meg az oauth2 hitelesítési mód engedélyezési](#authorization-grant) folyamat, és az API-khoz/adatok férhetnek hozzá az erőforrás tulajdonosa nevében. [Határozza meg, kétféle ügyfelek]oauth2 hitelesítési mód engedélyezési keretrendszer[OAuth2-Client-Types], "bizalmas" és "nyilvános", az ügyfél azt jelenti, hogy a bizalmas saját hitelesítő adatok karbantartása alapján. Alkalmazások segítségével miként állíthat futó webkiszolgálóra, [natív ügyfele (nyilvános)](#native-client) telepítve van egy eszközt, vagy egy [ügyfél felhasználói ügynökök-alapú (nyilvános)](#user-agent-based-client) , amely egy eszközt böngészőben futtatja a [webes ügyfelek (a bizalmas)](#web-client) .

## <a name="consent"></a>beleegyezés
A folyamat [erőforrás tulajdonosi](#resource-owner) engedély egy [ügyfélalkalmazás](#client-application), egyedi [engedélyek](#permissions) védett erőforrásokat, az erőforrás tulajdonosa nevében eléréséhez. Attól függően, hogy az ügyfél által kért az engedélyek rendszergazda vagy felhasználó megkéri a szervezet/egyéni adataikat elérésének engedélyezése a kurzor beleegyezés. Vegye figyelembe a [több elem bérlői](#multi-tenant-application) forgatókönyvben, az alkalmazás [fő szolgáltatás](#service-principal-object) is rögzítve van a consenting felhasználó bérlőhöz.

## <a name="id-token"></a>Azonosító token
Egy [OpenID csatlakozás] [ OpenIDConnect-ID-Token] [biztonsági jogkivonat](#security-token) egy [engedélyezési kiszolgáló](#authorization-server) [engedélyezési végpontot](#authorization-endpoint), amely tartalmazza a végfelhasználói [erőforrás tulajdonosa](#resource-owner)a hitelesítés kérelmekre vonatkozó [követelések](#claim) . Egy jogkivonat, például azonosító tokenek is jelennek meg a digitálisan aláírt [JSON webes jogkivonat (JWT)]szerint[JWT]. Egy hozzáférési jogkivonat eltérően azonban egy azonosító jogkivonat követelések kapcsolódó erőforrás access célokra nem használják és kifejezetten hozzáférés-vezérlés.

Lásd: [Azure AD-jogkivonat hivatkozás] [ AAD-Tokens-Claims] további információt.

## <a name="multi-tenant-application"></a>több elem bérlői alkalmazás
Osztály [ügyfélalkalmazás](#client-application) , amely lehetővé teszi, hogy jelentkezzen be, és a felhasználók által [beleegyezés](#consent) kiépítve bármely Azure Active Directory [bérlői](#tenant), beleértve a bérlők kívül az adott, hogy az ügyfél hol van regisztrálva. Ezzel ellentétben egyetlen-bérlői, azonosítóként regisztrált alkalmazás csak lehetővé válik bejelentkezési bővítmények, a kiépítéstől ugyanahhoz a bérlőhöz, mint a felhasználói fiókok hol van regisztrálva a az alkalmazást. [Natív](#native-client) ügyfélalkalmazásokban alapértelmezés szerint a több elem bérlői is, míg a [webes](#web-client) ügyfélalkalmazásokban van, az azt jelenti, hogy egyetlen és a több elem bérlői választhat.

Megtudhatja, [hogy miként jelentkezzen be a több elem bérlői alkalmazás minta használatával Azure Active Directory bárki] [ AAD-Multi-Tenant-Overview] további információt.

## <a name="native-client"></a>natív ügyfele
Natív módon telepítve van egy eszközön [ügyfélalkalmazás](#client-application) típusát. Mivel az összes kód végrehajtása egy eszközön, célszerű a hitelesítő adatok magánjellegű/bizalmasan tárolására, mert az "nyilvános" ügyfél. Lásd: az [oauth2 hitelesítési mód lekérdezésiügyfél-típusok és profilok] [ OAuth2-Client-Types] további információt.

## <a name="permissions"></a>engedélyek
Egy [ügyfélalkalmazás](#client-application) fér hozzá egy [erőforrás-kiszolgáló](#resource-server) által jogosultsági kéréseket deklarálási. Két típusa állnak rendelkezésre:

- "Delegált" engedélyek, a [hatókör-alapú](#scopes) hozzáférés igénylése meghatalmazott engedélyt azoktól az [erőforrás-tulajdonos](#resource-owner)jelentkezett be, jelennek meg az erőforráshoz futásidőben az ügyfél- [hozzáférési jogkivonat](#access-token) ["scp" igények](#claim) szerint.
- [Szerepköralapú](#roles) hozzáférés - az ügyfélalkalmazás hitelesítő adatok/identitással kér, "Alkalmazás" engedélyek jelennek meg az erőforrás futásidőben, az ügyfél-hozzáférési jogkivonat- ["szerepkörök" követelések](#claim) .

Azok is felszínre hozatala a [beleegyezés](#consent) folyamat során, biztosítva a rendszergazda vagy az erőforrás-tulajdonosi engedélyek biztosítása vagy elutasítsa a ügyfél erőforrásokat a bérlői webhelyemen lehetőséget.

Jogosultsági összehívásokat úgy van konfigurálva, a "alkalmazások" / "Konfigurálása" lap az [Azure klasszikus portál][AZURE-classic-portal], az "Engedélyek más alkalmazások", amelyet parancsot választva a kívánt meghatalmazott engedélyeit"" és "Alkalmazásengedélyek" (az utóbbi tagjának kell lennie a globális rendszergazdai szerep). [Nyilvános ügyfél](#client-application) nem tudja őrizni a hitelesítő adatok, mert azt csak akkor kérheti a meghatalmazott engedélyeit, míg a [bizalmas ügyfél](#client-application) kérelem mindkét meghatalmazott és Alkalmazásengedélyek. Az ügyfél [alkalmazás objektum](#application-object) feltüntetett engedélyek tárolja a [requiredResourceAccess tulajdonság][AAD-Graph-App-Entity].

## <a name="resource-owner"></a>erőforrás-tulajdonos
Az [Oauth2 hitelesítési mód engedélyezési keretrendszer]által meghatározott[OAuth2-Role-Def], képes a védett erőforrás-hozzáférés engedélyezése entitás. Ha az erőforrás tulajdonosa egy személyt, akkor nevezik végfelhasználó. Például ha egy [ügyfélalkalmazás](#client-application) szeretne férni a felhasználó postafiók keresztül a [Microsoft Graph API][Microsoft-Graph], a postaláda az erőforrás-tulajdonosi engedély igényel.

## <a name="resource-server"></a>erőforrás-kiszolgáló
Az [Oauth2 hitelesítési mód engedélyezési keretrendszer]által meghatározott[OAuth2-Role-Def], hogy a hosts védett erőforrások képes elfogadása és válaszol a kiszolgáló erőforrás kérések védett [ügyfélalkalmazásokban](#client-application) előidéző egy [jogkivonat eléréséhez](#access-token). Más néven egy védett erőforrás-kiszolgáló, vagy az erőforrás-alkalmazást.

Erőforrás-kiszolgáló API-khoz közzététele és kényszeríti a védett erőforrásokhoz [hatókörök](#scopes) és a [szerepkörök](#roles), az OAuth 2.0-s engedélyezési keretrendszer használatával való hozzáférést. Többek között a Azure Active Directory Graph API, amely biztosítja az Azure AD-bérlő adatokhoz való hozzáférés, és az Office 365 API, amely a szükséges adatokat, például a levelezés és a naptár elérését. A [Microsoft Graph API]keresztül is hozzáférhetők mindkét[Microsoft-Graph].  

Ügyfélalkalmazás, hasonlóan a erőforrás identitás alkalmazást egy Azure AD-bérlői webhelyen, az alkalmazás és a szolgáltatás fő objektum nyújtó létrejön [regisztrációs](#application-registration) keresztül. Néhány Microsoft által biztosított API-hoz, például az Azure Active Directory Graph API előre regisztrálta szolgáltatás rendszerbiztonsági kiépítési során az összes bérlők hozzáférhetővé.

## <a name="roles"></a>szerepkörök
[Keresési tartományok](#scopes), például szerepkörök kínál egy [erőforrás-kiszolgáló](#resource-server) szabályozására védett erőforrásait a hozzáférést. Két típusa van: a "felhasználó" szerepkör közben azonos hozzáférést igénylő [ügyfélalkalmazásokban](#client-application) "alkalmazás" szerepet hajtja végre az erőforráshoz való hozzáférést igénylő felhasználók vagy csoportok szerepköralapú hozzáférés-vezérlés hajtja végre.

Szerepkörök olyan erőforrás által definiált karakterláncok (például "költség véleményező", "Csak olvasható", "Directory.ReadWrite.All"), az [Azure klasszikus portál] felügyelt[ AZURE-classic-portal] keresztül az erőforrás [cikkét alkalmazást](#application-manifest), és az erőforrás [appRoles tulajdonság]tárolt[AAD-Graph-Sp-Entity]. Az Azure klasszikus portálon is felhasználók rendeljen "felhasználó" szerepkörök, és az ügyfelek [szolgáltatásalkalmazás jogosultságainak](#permissions) elérésére "alkalmazás" szerepet szolgál.

[Diagram API jogosultsági hatókörök]témakörben talál részletes vitafórum az Azure Active Directory API-grafikon által elérhetővé tett alkalmazás szerepkörök,[AAD-Graph-Perm-Scopes]. A lépésenkénti végrehajtása példa című témakörben [szerepköralapú hozzáférés-vezérlés felhő alkalmazások használata az Azure Active Directory][Duyshant-Role-Blog].

## <a name="scopes"></a>keresési tartományok
[Szerepkörök](#roles), például tartományok kínál egy [erőforrás-kiszolgáló](#resource-server) szabályozására védett erőforrásait a hozzáférést. Keresési tartományok [hatókör-alapú] végrehajtásához használt[ OAuth2-Access-Token-Scopes] hozzáférés-vezérlés [ügyfélalkalmazás](#client-application) , amely már megkapta az access delegált az erőforráshoz a tulajdonosa.

Hatókörök állnak erőforrás által megadott karakterláncot (például "Mail.Read", "Directory.ReadWrite.All"), az [Azure klasszikus portál] felügyelt[ AZURE-classic-portal] keresztül az erőforrás [cikkét alkalmazást](#application-manifest), és az erőforrás [oauth2Permissions tulajdonság]tárolt[AAD-Graph-Sp-Entity]. Az Azure klasszikus portálon is használatával állítsa be az ügyfél alkalmazás [meghatalmazott engedélyeit](#permissions) a hatókör eléréséhez.

A legjobb gyakorlathoz elnevezési konvenció környezetbe "resource.operation.constraint" formátumot. [Diagram API jogosultsági hatókörök]témakörben talál részletes vitafórum az Azure Active Directory API-grafikon által elérhetővé tett hatókörök,[AAD-Graph-Perm-Scopes]. Az Office 365-szolgáltatásokkal, által elérhetővé tett hatókörök című témakör tartalmaz [az Office 365 API engedélyek hivatkozás][O365-Perm-Ref].

## <a name="security-token"></a>biztonsági jogkivonat
Aláírt dokumentum követelések, például az oauth2 hitelesítési mód jogkivonat vagy a SAML 2.0-s állítás tartalmazó. Az oauth2 hitelesítési mód [engedélyt adhatnak](#authorization-grant), a [hozzáférési jogkivonat](#access-token) (oauth2 hitelesítési mód) és a [Azonosító jogkivonat](OpenID Connect) , biztonsági tokenek típusú, mindkettő időrendi megvalósítása egy [JSON webes jogkivonat (JWT)][JWT].

## <a name="service-principal-object"></a>szolgáltatás fő objektum
Amikor, külső.FÜGGV és frissítés az alkalmazások az [Azure klasszikus portál][AZURE-classic-portal], a portálon hoz létre/frissítések [alkalmazás objektum](#application-object) és a megfelelő szolgáltatás fő objektumot is, hogy a bérlői webhelyen. Az alkalmazás objektum *határozza meg* az alkalmazást identitás globálisan (ahol a társított alkalmazás hozzáférést kapott valamennyi bérlők), át, és a sablont, ahonnan a megfelelő szolgáltatás fő objektum(ok) *származtatott* helyileg futásidőben (az adott bérlői webhelyre) használata.

Lásd: az [alkalmazás és a szolgáltatás egyszerű objektumok] [ AAD-App-SP-Objects] további információt.

## <a name="sign-in"></a>bejelentkezés
A folyamat [ügyfélalkalmazás](#client-application) kezdeményezése végfelhasználói hitelesítési és kapcsolódó állapotát, a [biztonsági jogkivonat](#security-token) beszerzése és hatókör meghatározása az adott állam alkalmazás munkamenet rögzítése. Adja meg is eltéréseket, például a felhasználói profil adatait, és a token jogcímeken származó információkat.

Az alkalmazás bejelentkezési függvény rendszerint használatos egyetlen – bejelentkezés (SSO) végrehajtásához. Azt is előz "regisztráció" függvény, a bejegyzés helyként egy végfelhasználó hozzáférni az alkalmazás (frissítéskor első bejelentkezés). Az előfizetési függvény összegyűjtése és a probléma továbbra is fennáll a felhasználó számára különleges további állapot szolgál, és szükség lehet a [felhasználó beleegyezés](#consent).

## <a name="sign-out"></a>kijelentkezési
Nem hitelesítő folyamata az [ügyfélalkalmazás](#client-application) munkamenet közben [bejelentkezési](#sign-in) tartozó végfelhasználó, a felhasználói állapot leválasztása

## <a name="tenant"></a>Bérlői webhelyen
Egy példánya az Azure Active directory-Azure AD-bérlő nevezik. Sokféle tartalomelem, többek között biztosít:

- integrált alkalmazások a beállításjegyzék szolgáltatás
- felhasználói fiókok és regisztrált alkalmazások hitelesítés
- Szükséges: különböző protokollok, például oauth2 hitelesítési mód és a SAML, beleértve az [Engedélyezés végpontot](#authorization-endpoint), [jogkivonat végpont](#token-endpoint) és a "közös" végpont [több bérlői alkalmazások](#multi-tenant-application)által használt többi végpontok.

Bérlői webhelyre is társítva az Azure Active Directory vagy az Office 365-előfizetés az előfizetés kezeléséről az előfizetés identitás és a hozzáférés-kezelés szolgáltatás kiépítési során. Lásd: [az Azure Active Directory-bérlői beszerzése] [ AAD-How-To-Tenant] különféle módjait kapcsolatos részletekért hozzáférhet bérlői webhelyre. Lásd: az [Azure Active Directory hogyan Azure előfizetések társulnak] [ AAD-How-Subscriptions-Assoc] előfizetések és az Azure AD-bérlő közötti kapcsolatra részletesen.

## <a name="token-endpoint"></a>jogkivonat végpont
A támogatási oauth2 hitelesítési mód [engedélyezése lehetőséget biztosít](#authorization-grant)a [engedélyezési kiszolgáló](#authorization-server) által végrehajtott végpontjait közül. Attól függően, hogy az engedélyek biztosítása használhatná szerezheti be a [hozzáférési jogkivonat](#access-token) (és kapcsolódó "frissítés" token) [ügyfél](#client-application)vagy [azonosító token](#ID-token) [OpenID csatlakozás] együtt használva[ OpenIDConnect] Protocol (protokoll).

## <a name="user-agent-based-client"></a>Felhasználói ügynökök-alapú ügyfél
[Ügyfélalkalmazás](#client-application) , amely a webkiszolgálóra kód letölti, és végrehajtja a belül egy felhasználói ügynököt (például egy webböngészőben), például egy lap alkalmazás (egészében) típusú. Mivel az összes kód végrehajtása egy eszközön, célszerű a hitelesítő adatok magánjellegű/bizalmasan tárolására, mert az "nyilvános" ügyfél. Lásd: az [oauth2 hitelesítési mód lekérdezésiügyfél-típusok és profilok] [ OAuth2-Client-Types] további információt.

## <a name="user-principal"></a>felhasználók egyszerű
Hasonlóan egy egyszerű-objektum egy alkalmazás példány jelző, egy felhasználói fő célja egy másik típusú rendszerbiztonsági, amely a felhasználó képviseli. Az Azure Active Directory Graph [felhasználói entitás] [ AAD-Graph-User-Entity] határozza meg a séma felhasználói-objektum, például a felhasználókkal kapcsolatos tulajdonságait, például a vezeték- és utóneve, egyszerű felhasználónév, címtár szerepkör tagságát és stb. Ezzel a megoldással a felhasználói azonosító beállítások létrehozására, hogy a felhasználók egyszerű futásidőben Azure AD. A felhasználó egyszerű jelző egy hitelesített felhasználó számára Single Sign-On, meghatalmazás [beleegyezés](#consent) rögzítése, így access vezérlő döntéseket stb.

## <a name="web-client"></a>webes ügyfél
[Ügyfélalkalmazás](#client-application) , amely a webkiszolgálóra, és tudja végrehajtja a összes kód biztonságos tárolása a hitelesítő adatok a kiszolgáló által egy "bizalmas" ügyfél működhet típusát. Lásd: az [oauth2 hitelesítési mód lekérdezésiügyfél-típusok és profilok] [ OAuth2-Client-Types] további információt.

## <a name="next-steps"></a>Következő lépések
Az [Azure Active Directory fejlesztői útmutató] [ AAD-Dev-Guide] használható minden Azure AD-fejlesztés a portálon található kapcsolódó témakörök, ezen belül a [alkalmazás integrálását] [ AAD-How-To-Integrate] és [Azure Active Directory authentication és által támogatott hitelesítéstípusok esetek]alapjait[AAD-Auth-Scenarios].

A következő Disqus Megjegyzések szakasz használatával visszajelzést, és segítse a szolgáltatás szűkítése és alakíthat ki a tartalmat.

<!--Image references-->

<!--Reference style links -->
[AAD-App-Manifest]: ./active-directory-application-manifest.md
[AAD-App-SP-Objects]: ./active-directory-application-objects.md
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Graph-Perm-Scopes]: https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AAD-Graph-User-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#user-entity
[AAD-How-Subscriptions-Assoc]: ./active-directory-how-subscriptions-associated-directory.md
[AAD-How-To-Integrate]: ./active-directory-how-to-integrate.md
[AAD-How-To-Tenant]: active-directory-howto-tenant.md
[AAD-Integrating-Apps]: ./active-directory-integrating-applications.md
[AAD-Multi-Tenant-Overview]: active-directory-devhowto-multi-tenant-overview.md
[AAD-Security-Token-Claims]: ./active-directory-authentication-scenarios/#claims-in-azure-ad-security-tokens
[AAD-Tokens-Claims]: ./active-directory-token-and-claims.md
[AZURE-classic-portal]: https://manage.windowsazure.com
[Duyshant-Role-Blog]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/
[JWT]: https://tools.ietf.org/html/draft-ietf-oauth-json-web-token-32
[Microsoft-Graph]: https://graph.microsoft.io
[O365-Perm-Ref]: https://msdn.microsoft.com/en-us/office/office365/howto/application-manifest
[OAuth2-Access-Token-Scopes]: https://tools.ietf.org/html/rfc6749#section-3.3
[OAuth2-AuthZ-Endpoint]: https://tools.ietf.org/html/rfc6749#section-3.1
[OAuth2-AuthZ-Grant-Types]: https://tools.ietf.org/html/rfc6749#section-1.3
[OAuth2-Client-Types]: https://tools.ietf.org/html/rfc6749#section-2.1
[OAuth2-Role-Def]: https://tools.ietf.org/html/rfc6749#page-6
[OpenIDConnect]: http://openid.net/specs/openid-connect-core-1_0.html
[OpenIDConnect-AuthZ-Endpoint]: http://openid.net/specs/openid-connect-core-1_0.html#AuthorizationEndpoint
[OpenIDConnect-ID-Token]: http://openid.net/specs/openid-connect-core-1_0.html#IDToken
