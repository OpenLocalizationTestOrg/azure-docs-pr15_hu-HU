<properties
   pageTitle="Forgalom az Azure Active Directory ismertetése implicit oauth2 hitelesítési mód megadása |} Microsoft Azure"
   description="További tudnivalók a további információ az implicit oauth2 hitelesítési mód Azure Active Directory végrehajtása adja meg az adatfolyam, és hogy az alkalmazás megfelelő."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="vibronet"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/17/2016"
   ms.author="vittorib;bryanla"/>

# <a name="understanding-the-oauth2-implicit-grant-flow-in-azure-active-directory-ad"></a>A oauth2 hitelesítési mód implicit támogatási folyamat az Azure Active Directory (AD) ismertetése

Az oauth2 hitelesítési mód implicit támogatás notorious az alatt a támogatás az oauth2 hitelesítési mód beállítások biztonsági aggályokat leghosszabb listáját. És még, ez az ADAL JS, és a biztonságos jelszó-hitelesítés alkalmazások írásakor javasoljuk megvalósítani megközelítés. Milyen ad? Célszerű az összes frissítését és a kompromisszumok: és azzal kikapcsolja, mint az implicit támogatás a legjobb megközelítés is végezze el az alkalmazást, amely egy webes API JavaScript-e felhasználni a böngészőben.

## <a name="what-is-the-oauth2-implicit-grant"></a>Mi az a oauth2 hitelesítési mód implicit támogatás?

[Adja meg az oauth2 hitelesítési mód engedélyezési kód](https://tools.ietf.org/html/rfc6749#section-1.3.1) quintessential két külön végpontok használó engedély megadása. Az engedély végpontot eredménye egy engedélyezési kód felhasználói beavatkozás fázis szolgál. A token végpont majd a cseréje a kód egy hozzáférési jogkivonat, és gyakran frissítés jogkivonat, valamint az ügyfél által használható. Webalkalmazások szükségesek a saját alkalmazás hitelesítő adatait, és a token végpont bemutatása, hogy a engedélyezési kiszolgáló hitelesíthet az ügyfélnek.

[Adja meg az oauth2 hitelesítési mód implicit](https://tools.ietf.org/html/rfc6749#section-1.3.2) egy ügyfelet, hogy hozzáférési jogkivonat (és id_token [OpenId csatlakozás](http://openid.net/specs/openid-connect-core-1_0.html)esetén) lehetővé tevő variant közvetlenül az engedélyezés végpontot, anélkül, hogy kapcsolatba az jogkivonat végpontot, sem a ügyfélalkalmazás hitelesítő. Ez a változat kifejezetten készült az JavaScript-alapú böngészőben futó alkalmazások: az eredeti oauth2 hitelesítési mód beállítások a tokenek egy URI fragment választ. Amely a token bittel számára elérhetővé teszi a JavaScript-kód az ügyfélprogramban, de azok nem fog szerepelni a kiszolgáló felé átirányítások biztosítja. Közvetlenül az engedélyezés végpontot átirányítja tokenek Visszatérés a böngészőben. Azt is tartományok origin hívásokat, amelynek van szükség, ha a JavaScript-alkalmazás szükséges lépjen kapcsolatba a token végpont vonatkozó követelmények kiküszöbölésével előnyeit.

Egy implicit oauth2 hitelesítési mód megadása fontos jellemzője arra, hogy ilyen átfolyása soha nem visszatérési frissítés tokenek az ügyfél számára. Azt a program a következő szakaszban látható, mint, amely nem igazán szükséges és valójában egy biztonsági probléma.

## <a name="suitable-scenarios-for-the-oauth2-implicit-grant"></a>Az oauth2 hitelesítési mód implicit támogatás alkalmas esetek

Az oauth2 hitelesítési mód beállítások magát deklarálása, mint az implicit támogatás dolgozott lett ahhoz, hogy felhasználói ügynök alkalmazások – azaz, végrehajtása egy böngészőből a JavaScript-alkalmazásokat. A definiáló ilyen alkalmazások jellemzője szolgál, hogy JavaScript-kód eléréséhez server erőforrásaira (általában a webes API-val) és az alkalmazás UX megfelelően frissítéséhez. Az alkalmazások, például Gmail- vagy az Outlook Web Access gondolkodási: Amikor kijelöl egy üzenetet a Beérkezett üzenetek mappából, az üzenet képi megjelenítés panelen az egész lapon változatlan marad, amíg az új kijelölés megjelenítendő változik. Ez a – a hagyományos átirányítás-alapú Web Apps alkalmazások, ahol minden felhasználói beavatkozásra eredménye egy teljes oldal postback és az új kiszolgáló válasz teljes lapos megjelenésének.

Egyetlen lap alkalmazások vagy gyógyfürdőhelyek alkalmazásokat, amelyek a JavaScript-alapú megközelítés a rendkívüli az úgynevezett: a arról, hogy az alkalmazások csak szolgálnak, egy kezdeti HTML-lapot, és a kapcsolódó, JavaScript, minden ezt követő tevékenységek kritikus előfeltételei: webes API-hívások végrehajtani a JavaScript keresztül. Azonban hibrid módszer, ha az alkalmazás főleg postback-alapú, de alkalmi JS hívások hajt végre, amelyek nem ritka – implicit folyamat használatáról a vitafórum jelentősége azok számára, valamint.

Átirányítás-alapú alkalmazások jellemzően biztonságos azok megközelítés nem működik, valamint a JavaScript-alkalmazásokhoz kérések cookie-kat, azonban keresztül. Cookie-k használata csak a tartomány, azok elkészülte, miközben JavaScript-hívások előfordulhat, hogy irányítja, a nulla felé tartományához ellen működik. Erre valójában, amely gyakran lesz az eset: Microsoft Graph API, Office API-val Azure API – a tartományt, amikor az alkalmazás felszolgált-e a kívül az összes tartózkodó meghívása alkalmazások gondolkodási. JavaScript-alkalmazásokhoz egyre növekvő tendenciát, hogy egyáltalán nem kódmentes használna 100 %-os 3 fél üzleti funkciójuk végrehajtásához webes API-khoz.

Jelenleg a hívások átirányítása egy webes API védelme elsődleges módszer, hogy az oauth2 hitelesítési mód bearer jogkivonat módszert használja, ahol minden hívás egy oauth2 hitelesítési mód hozzáférési jogkivonat fűzni. A webes API megvizsgálja a bejövő hozzáférési jogkivonat, és ha úgy találja, rajta a szükséges hatókörök azt a kívánt művelet hozzáférést biztosít. A implicit folyamat lehetővé teszi a kényelmes JavaScript-alkalmazások beszerzése a webes API hozzáférési jogkivonat kínáló cookie-k szempontból számos előnye:

- Tokenek megbízható kapott nélkül tartományok origin hívások – az átirányítás, amelyhez tokenek visszatérési URI biztosítja, hogy a program nem kiszorított tokenek kötelező nyilvántartása
- JavaScript-alkalmazások szerezhet be annyi hozzáférési jogkivonat, mint a számukra szükséges jogosultságokkal annyi webes API-khoz Megcélozhat – a tartományok korlátozás nélkül
- HTML5-ös szolgáltatásokat, mint munkamenetből vagy helyi tároló teljes hozzáférés fölé jogkivonat gyorsítótár-élettartam kezelése, mivel a cookie-k kezelése átlátszatlanná válik az alkalmazásba
- Az Access tokenek nem téve a webhelyközi kérelem hamisítása (CSRF) támadásokkal

A implicit támogatási folyamat a frissítés tokenek, főleg biztonsági okokból nem ad meg. Frissítés jogkivonat szűken, a hozzáférési jogkivonat, így anyagi sokkal több kárt, abban az esetben, ha azt van kiszivárogtatott höz power megadása, nem változik. Az implicit folyamat tokenek érkeznek, az URL-cím, így a hozzáférés kockázata nagyobb, mint az engedélyezés kód megadása.

Jó helyen jár láthatja, hogy van-e a JavaScript-alkalmazások más mechanizmusa for megújításáról hozzáférési jogkivonat a felhasználó hitelesítő adatok többszöri rákérdezés nélkül rendelkezésére. Az alkalmazás új jogkivonat kérések ellen Azure Active Directory engedélyezési végpontjának is elvégezheti a rejtett iframe: mindaddig, amíg a böngésző továbbra is tartalmaz, az aktív munkamenet (további: rendelkezik a munkamenet cookie-k) az Azure Active Directory tartományi ellen a hitelesítési kérés tud fordulhat elő, felhasználói beavatkozás nélkül. 

Ez a modell hozzárendeli a JavaScript-alkalmazás független megújítása hozzáférési jogkivonat, és még szerezheti be egy új API újakat (feltéve, hogy a felhasználó korábban átadni kívánt hozzájárult e azokat. Ezzel elkerülhető a hozzáadott terhet beszerzésekor, ez esetben megtartják és védelme a magas érték eltérés, például a frissítés jogkivonat. Az eltérés beavatkozás nélküli megújítása lehetővé teszi, amely a Azure AD-munkamenet cookie az alkalmazás-Ön kívül kezeli. Egy másik eljárás előnye felhasználó is jelentkezzen ki az Azure Active Directory, az alkalmazások jelentkezett be az Azure Active Directory, a böngésző lapjai futó használatával. Ez az Azure Active Directory-munkamenet cookie-k törlése eredményez, és a JavaScript-alkalmazás automatikusan elvesznek az azt jelenti, hogy az aláírt ki felhasználó tokenek megújítása.

## <a name="is-the-implicit-grant-suitable-for-my-app"></a>Az alkalmazásom alkalmas az implicit támogatás?

Implicit megadásáról további kockázatok, mint a többi támogatás jeleníti meg. A figyelmet kell területek vannak jól dokumentált (lásd például [Visszaélés a hozzáférési jogkivonat megszemélyesítés erőforrás tulajdonosának az Implicit Flow] [ OAuth2-Spec-Implicit-Misuse] és [OAuth 2.0-s veszélyt modell és a biztonsági megfontolások][OAuth2-Threat-Model-And-Security-Implications]). A kockázat magasabb profil viszont nagymértékben oka az oka, hogy azt jelenti, hogy az aktív programkód, melyet a böngészőben távoli erőforrás-alkalmazások engedélyezése. Ha azt tervezi, egy biztonságos jelszó-hitelesítés architektúrájának nincs kódmentes komponenseknek vagy meghívása szándékos JavaScript-e a webes API, a implicit folyamat jogkivonat WIA használata ajánlott.

Ha az alkalmazás a natív ügyfele, az a implicit folyamat nem egy nagy igazítás. Az Azure Active Directory munkamenet cookie-k egy natív ügyfele környezetében hiányában megfosztja fenntartása hosszú élettartamú munkamenet az azt jelenti az alkalmazást. Ami azt jelenti, hogy az alkalmazás többször kéri a a felhasználót, amikor új erőforrások hozzáférési jogkivonat beszerzése.

Ha egy webalkalmazás, amelyek tartalmazzák még a háttér-kiszolgálói és használata más-annak kódmentes kódból API, a implicit folyamat akkor sem közelítését. Más támogatás höz power ad. Például oauth2 hitelesítési mód ügyfél hitelesítő adatok megadását teszi lehetővé beszerzése által jogkivonat, magát, az alkalmazás nem pedig felhasználói küldöttségek hozzárendelt engedélyeknek megfelelően. Ez azt jelenti, hogy az ügyfél rendelkezik-e a lehetőséget, ha meg szeretné őrizni programozási erőforrások elérését még a felhasználó nem aktívan folytat munkamenetet, és így tovább. Nem, csak az adott, de az ilyen magasabb security garancia ad. Ha például soha nem továbbítási hozzáférési jogkivonat, a felhasználó böngészőn keresztül, azok nem menti a program a böngészési előzmények kockázat, és így tovább. Az ügyfélalkalmazás is végrehajthat erős hitelesítés jogkivonat kérésekor.

## <a name="next-steps"></a>Következő lépések

- Fejlesztőeszközök erőforrások teljes listáját beleértve a hivatkozás adatokat a protokollok és az oauth2 hitelesítési mód engedélyezése adja meg az Azure Active Directory flow támogatásával, kérje meg a [Azure Active Directory fejlesztői útmutató][AAD-Developers-Guide]
- Itt elolvashatja, [hogyan integrálása az Azure Active Directory-alkalmazás]  [ ACOM-How-To-Integrate] további mélység integration alkalmazás folyamatáról.

<!--Image references-->

<!--Reference style links in use-->
[AAD-Developers-Guide]: active-directory-developers-guide.md
[ACOM-How-And-Why-Apps-Added-To-AAD]: active-directory-how-applications-are-added.md
[ACOM-How-To-Integrate]: active-directory-how-to-integrate.md
[OAuth2-Spec-Implicit-Misuse]: https://tools.ietf.org/html/rfc6749#section-10.16 
[OAuth2-Threat-Model-And-Security-Implications]: https://tools.ietf.org/html/rfc6819

