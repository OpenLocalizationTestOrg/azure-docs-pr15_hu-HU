<properties 
    pageTitle="API-kezelés alapfogalmak" 
    description="Tudjon meg többet az API-hoz, termékek, szerepkörök, csoportok és más API-kezelés alapfogalmak." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Mi az API-szolgáltatás?

API-kezelés segítségével a szervezetek külső API-k, partnerek és belső fejlesztők zárolásának feloldása a lehetséges adatok és a szolgáltatások közzététele. Vállalkozások mindenhol szeretné tevékenységeik kiterjesztése megegyezik egy digitális platform, új csatornák létrehozásáról, új ügyfelek keresése és ösztönzése mélyebb részvételét a meglévőket. API-kezelés itt az alapvető szakértelem sikeres API program átengedése a fejlesztői tetszés szerint elmélyedhet, üzleti hírcsatornájában, analytics, biztonság és védelem biztosítása érdekében.

Azure API-szolgáltatások áttekintése a következő videót, és megtudhatja, hogy miként API-kezelés használatával számos szolgáltatások hozzáadása a API, beleértve a hozzáférés-vezérlés, ráta korlátozása, figyelése, eseménynaplózás és válaszok gyorsítótárazása hajtana minimális munkahelyi.

> [AZURE.VIDEO azure-api-management-overview]

API-kezelés használatához a rendszergazdák API-hoz létre. Minden egyes API áll egy vagy több műveletet, és egy vagy több termék egyes API manuálisan is hozzáadhatók. Egy API-t használ, fejlesztők, amely tartalmazza az adott API termék előfizetés, és kattintson az API művelet fizetnie talált használatát házirendeket, hogy az érvényben felhívhatja őket.

Ez a témakör áttekintést API-kezelés alapfogalmak.

>[AZURE.NOTE] További tudnivalókért lásd: a [API-kezelés felhőalapú: az API-k Power kifejlesztése](http://j.mp/ms-apim-whitepaper) PDF-fájlt szeretne. A bevezető szeretne API Management CITO kutatás által foglalja magában: 
>
> - Közös API követelmények és kihívásokkal kapcsolatban
>     - API-khoz szétválasztás és homlokzatoknál bemutatása
>     - A fejlesztők használatba és gyors
>     - Hozzáférés biztosítása érdekében
>     - Analitikai és mérőszámok
> - Elveszti a hozzáférés és a betekintést és az API-kezelés platform
> - Felhőalapú viewben helyszíni megoldások használata
> - Azure API-kezelés

## <a name="apis"> </a>API-k és műveletek

API-khoz is a foundation API Management szolgáltatás példány. Minden egyes API fejlesztők számára elérhető műveletek csoportját képviseli. Minden egyes API a háttéradatbázist szolgáltatás, amely az API hivatkozást, és a háttéradatbázist szolgáltatás végrehajtott műveletekhez a műveleteket térkép tartalmazza. API-kezelés műveletek erősen konfigurálható, és szabályozni kell URL-cím megfeleltetés, lekérdezés és elérési útjának paramétereit, kérés és válasz tartalom és a művelet válasz gyorsítótárazás. Ráta korlát, kvóták és IP-korlátozás házirendek is is szerepelni fog az API-val, vagy az egyéni művelet szintjén.

További tudnivalókért olvassa el a [API-k létrehozása][] , és [hogy miként vehet fel egy API-nak műveletek][]című témakört.


## <a name="products"></a> Termékek

A termékeket hogyan fejlesztők az API-khoz hogy megjelentek. Termékek API-kezelés egy vagy több API-khoz van, és egy név, leírás és használati feltételei vannak beállítva. **Megnyitás** és a **védett**, a termékek is lehet. Védett termékek elő kell fizetnie az felhasználhatók, miközben Megnyitás előtt termékek előfizetés nélkül is használható. Ha egy termék fejlesztők használatra kész tehető közzé. Miután közzétette, akkor megjeleníthető (és védett termékek fizetett elő,) a fejlesztők. Előfizetés jóváhagyási termék szintre van beállítva, és rendszergazdai jóváhagyás megkövetelése, vagy automatikus jóvá kell.

Csoportok kezelése a termékekkel, és a fejlesztők olvashatóságát szolgálnak. Termékek Láthatóság megadása csoportok, és a fejlesztők megtekintheti és látható a csoportokba tartoznak, ahol a termékek előfizetés. 

További tudnivalókért lásd: [létrehozása és közzététele a termék][] - és az alábbi videóban.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Csoportok

Csoportok kezelése a termékekkel, és a fejlesztők olvashatóságát szolgálnak. API-kezelés az alábbi megváltoztatható rendszer csoportok tartalmaz.

-   **A rendszergazdák** - Azure előfizetés rendszergazdák tartoznak ennek a csoportnak. A rendszergazdák kezelése API-kezelési-példányok, az API-hoz, műveletek és a fejlesztők felhasznált termékek létrehozása.
-   A **fejlesztők** - hitelesített developer Portal segítségével a felhasználók sorolhatók ennek a csoportnak. A fejlesztők készíthet alkalmazásokat az API-k használata a vevők. A fejlesztők a developer Portal segítségével hozzáférést kapnak, és hozza létre a hívás a műveletek az API-alkalmazások.
-   **Vendégek** - felhasználók nem hitelesített Fejlesztőeszközök portál, például megtalálhatók az API-kezelési-példányok developer Portal segítségével leendő ügyfelek sorolhatók ennek a csoportnak. Hogy adható bizonyos csak olvasási hozzáférést, például az azt jelenti, hogy megtekinthetők API-hoz, de nem hívja őket.

Ezek a rendszer csoportok mellett a rendszergazdák hozhatnak létre egyéni csoportok vagy [kihasználhatja az Azure Active Directory-bérlők társított külső csoportok](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Egyéni és a külső csoport mellett rendszer csoportok biztosítva a láthatóság fejlesztők és az access API-termékek használható. Például sikerült hozzon létre egy egyéni csoportot egy adott partner szervezeti viszonyban fejlesztők számára, és teszi lehetővé az access az API csak a releváns API tartalmazó termékének. A felhasználó egynél több csoport tagja lehet.

További információért megtudhatja, [hogy miként csoportok létrehozása és használata][].

## <a name="developers"></a> Fejlesztők számára

A fejlesztők jelenítik meg a felhasználói fiókok API Management szolgáltatás példány. A fejlesztők kell létrehozni vagy a rendszergazdák által meghívja, vagy azok feliratkozhat a [Developer Portal segítségével][]. Mindegyik fejlesztő egy vagy több csoport tagja, és ezekhez a csoportokhoz láthatóságának biztosítása termékeket előfizetés lehet.

Ha egy termék fizessen elő a fejlesztők kapnak a terméket az elsődleges és másodlagos kulcsa. A kulcs híváskezdeményezési be a termék API-hoz használatos.

További tudnivalókért olvassa el a [hogyan hozhatja létre, és a fejlesztők meghívása][] , és [hogy miként társítható fejlesztők-csoportok][]című témakört.

## <a name="policies"></a> Házirendek

Házirend-beállítások a hatékony, amelyek lehetővé teszik az API keresztül konfigurációs viselkedésének módosítása a publisher API-kezelés lehetőséget. Házirendek olyan gyűjteménye, kimutatások, egymás után a kérésre végrehajtott vagy a válasz egy API. Népszerű kimutatások olyan konvertálás XML-ből JSON és a hívás ráta korlátozása egy fejlesztő bejövő hívásait mennyiségét korlátozni, és sok más házirendek érhetők el.

Házirend-kifejezések is alkalmazható attribútum vagy szöveges értékek közül az API-kezelési házirendek kivéve, ha a házirend mást ad meg. Néhány irányelvet, például a [Control flow](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) és [-változó beállítása](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) házirendek házirend kifejezések alapulnak. További tudnivalókért lásd: a [Speciális házirendek](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [házirend kifejezések](https://msdn.microsoft.com/library/azure/dn910913.aspx), és nézze meg az alábbi videót.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

API adatkezelési házirendek listáját olvassa el a [Csoportházirend-útmutatója][]című témakört. Használatáról és beállításáról házirendek a további tudnivalókért olvassa el a [API adatkezelési házirendek][]című témakört. A termék létrehozása sebesség korlát és kvóta házirendek oktatóanyag olvassa el a [hogyan létrehozása és speciális termék beállításainak konfigurálása][]című témakört. A bemutató az alábbi videóban talál.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Developer Portal segítségével

A developer Portal segítségével pedig a fejlesztők is megismerheti az API-hoz, megtekintése és a műveleteket, fizessen elő termékekben. Lehetséges ügyfelek keressék fel a developer Portal segítségével API-hoz és a műveletek megtekintése, és jelentkezzen. A Fejlesztőeszközök portál URL-CÍMÉT az irányítópulton a API Management szolgáltatás példány az Azure klasszikus portálon található.

Testre szabhatja a megjelenését és működését a developer Portal segítségével egyéni tartalom felvétele, stílusainak testreszabása és a arculati elemek hozzáadása.

## <a name="api-management-and-the-api-economy"></a>API-kezelési és az API-fogyasztási

Ha többet szeretne megtudni az API-kezelés, nézze meg a Microsoft Ignite 2015 konferencia a következő bemutatót.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer Portal segítségével]: #developer-portal

[API-k létrehozása]: api-management-howto-create-apis.md
[Műveletek hozzáadása egy API]: api-management-howto-add-operations.md
[Hogyan hozhat létre, és egy termék közzététele]: api-management-howto-add-products.md
[Hogyan csoportok létrehozása és használata]: api-management-howto-create-groups.md
[Hogyan csoportok társítása fejlesztők számára]: api-management-howto-create-groups.md#associate-group-developer
[Hogyan létrehozása és speciális termék beállításainak konfigurálása]: api-management-howto-product-with-rules.md
[Hogyan hozhatja létre, és a fejlesztők meghívása]: api-management-howto-create-or-invite-developers.md
[Házirend-hivatkozás]: api-management-policy-reference.md
[API adatkezelési házirendek]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
