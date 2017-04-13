<properties
    pageTitle="Egyéni gyorsítótárazás Azure API kezelése"
    description="Megtudhatja, hogy miként gyorsítótár-elemek billentyű Azure API kezelése"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>Egyéni gyorsítótárazás Azure API kezelése
Azure API alkalmazáskezelési szolgáltatás rendelkezik beépített támogatással- [gyorsítótár a HTTP-válasz](api-management-howto-cache.md) az erőforrás URL-cím használata a kulcsként. A kulcs kérelem fejlécek használatával módosíthatja a `vary-by` tulajdonságait. Ez akkor hasznos, gyorsítótárazás teljes HTTP válaszok (más néven ábrázolása), de néha célszerű csak gyorsítótárat a megadott egy részét. Az új [gyorsítótár keresési_érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) és a [gyorsítótár-tároló – érték](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) házirendeket biztosítson tárolni, valamint tetszőleges csatolt fájllal a házirend-definíciók belül lehetőséget. Ez a lehetőség is felveszi az value a korábban bevezetett [Küldés-kérési](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend mivel most gyorsítótárba helyezni külső szolgáltatások válaszainak.

## <a name="architecture"></a>Architektúra  
API alkalmazáskezelési szolgáltatás megosztott per-bérlői adatgyorsítótár használ, így, mint a maximum, méretezheti továbbra is jelenik meg ugyanezt való hozzáférést több egységek gyorsítótárazott adatokat. Több területre telepítéshez való munkavégzés során vannak azonban olyan független gyorsítótárát belül a régióban. Miatt, fontos, hogy a gyorsítótár nem tekinti a adatok mentése, amennyiben néhány adatot csak forrását. Ha jelent, és később úgy döntött, hogy a régió több példányban előnyeit, majd utazási felhasználókkal ügyfelek elveszíthetik a hozzáférésüket a gyorsítótárban tárolt adatokat.

## <a name="fragment-caching"></a>Fragment gyorsítótárazás
Vannak bizonyos esetekben, ha a eredményül adott válaszok költséges határozza meg, és lehetővé teszi a meghatározott ideig még friss marad adatok bizonyos része tartalmaz. Szerepel példaként fontolja meg egy beépített nézetbeli zárolások, nézetbeli állapot stb vonatkozó információkat tartalmazza légitársaság szolgáltatás. A légitársaságok pontok program tagja a felhasználó esetén azok szeretné is, hogy az aktuális állapotát és a halmozott fogyasztási vonatkozó információkat. Felhasználói kapcsolatos információk egy másik rendszer lehetséges tárolási helyei, de célszerű bele válaszok nézetbeli állapot és zárolások adja vissza. Ezt megteheti fragment gyorsítótárazás nevű folyamat. Az elsődleges ábrázolása a forráskiszolgálóval használata bizonyos típusú jogkivonat jelezze, ahol a felhasználó kapcsolatos információk szúrhatók be a adhatók vissza. 

Fontolja meg egy kódmentes API következő JSON válaszát.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

És a másodlagos erőforrás `/userprofile/{userid}` , hogy hasonlít,

     { "username" : "Bob Smith", "Status" : "Gold" }

Annak megállapításához, a megfelelő felhasználói adatok, amelyet fel szeretne venni, határozni, hogy kik a végfelhasználó van szükség. Ez az eljárás végrehajtása függő. Példaként használok a `Subject` formál egy `JWT` jogkivonat. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Ez tároljuk `enduserid` érték egy helyi változót későbbi felhasználás céljából. A következő lépésként határozza meg, ha egy korábbi kérelmet már beolvasni a felhasználói adatok és a gyorsítótárban tárolt. Ez azt használja a `cache-lookup-value` házirend.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Ha nem szerepel bejegyzés, amely megfelel a kulcs értékét, majd a nincs gyorsítótár `userprofile` helyi változót jön létre. Rendszer ellenőrzi, hogy a keresési használatának egyik a `choose` control flow házirend.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Ha a `userprofile` helyi változót nem létezik, akkor kell, hogy megtalálja a HTTP-kérésben fogjuk.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Használja a `enduserid` összeállításához a felhasználói profil erőforrás URL-CÍMÉT. Amikor azt rendelkezik, a válasz, hogy a kérdésre adott választ ki a szövegtörzs lekérés, és tárolja azt az egy helyi változót.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Kapcsolatfelvételi, hogy újra, ellenőrizze a HTTP-kérés, amikor az adott felhasználó egy másik kérelmet elkerülése érdekében azt a felhasználói profil tárolhatja a gyorsítótár.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Az érték kulccsal pontosan, hogy eredetileg próbált megtalálja a gyorsítótár tároljuk. Az időtartam, hogy választva érték tárolása hogyan alapján kell a változtatások alkalmazása és a felhasználók hogyan alternatív során gyakran való elavult információkat. 

Fontos tudni, hogy a gyorsítótárból beolvasása továbbra is a folyamaton, hálózati kérés és esetleg továbbra is hozzáadhatja ezredmásodperces tízesre a kérést. Előnyökkel jár a felhasználóiprofil-adatok telik, mint az adatbázis-lekérdezések vagy több vissza részek összesített adatok erre szolgáló miatt meghatározásakor származnak.

A végleges a folyamat lépésként a felhasználóiprofil-adatok frissítése a visszaadott választ.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Kiválasztott szeretné szerepeltetni az idézőjelek között a jogkivonat, hogy akkor is, ha a csere nem jelentkezik, a válasz lett-e még érvényes JSON. Ez elsősorban, hogy könnyebben hibakeresési volt.

Miután közös kombinálása ezeket a lépéseket, a befejezési eredménye egy házirendet, amely az alábbihoz hasonlóan néz ki.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Gyorsítótárazási eljárás elsősorban a webhelyek hol HTML áll a kiszolgálóoldalon, hogy egyetlen lapként megjelenítését. Azonban is hasznos lehet a API-k, ahol az ügyfelek nem ügyfél, oldal HTTP gyorsítótárazás vagy célszerű nem felelősséget elhelyezése az ügyfélnek.

A részlet gyorsítótárazás azonos típusú is elvégezhető vgx.dll gyorsítótár-kiszolgáló használata kódmentes webkiszolgálón, azonban a munka elvégzéséhez az API-kezelés szolgáltatással akkor hasznos, ha a gyorsítótárban tárolt töredékek származó különböző vissza leállítja a elsődleges válaszokat mint.

## <a name="transparent-versioning"></a>Áttetsző verziószámozás
Általános eljárás a egy API-t egy időpontban véve több különböző végrehajtási verziójának. Ez talán támogatása a különböző környezetekben, például a fejlesztők, vizsgálatot, gyártási stb, vagy lehet, támogatja az újabb verziók áttelepítése API fogyasztói idő biztosítása API régebbi verzióit. 

Módosíthatja az URL-címeit, az ügyfél fejlesztők igénylő helyett kezelését Ez egy megközelítés `/v1/customers` való `/v2/customers` tárolni a fogyasztói profiladatok azok jelenleg szeretne használni, és hívja fel a megfelelő kódmentes URL-CÍMÉT az API melyik verziója. Annak megállapításához, ha fel szeretne hívni egy adott ügyfél számára megfelelő kódmentes URL-CÍMÉT, a lekérdezés bizonyos konfigurációs adatok szükség. Az konfigurációs adatok gyorsítótárba által azt méretűre módon ezt a keresést, a teljesítményét.

Első lépésként határozza meg az azonosító használatával állítsa be a kívánt verziót. Ebben a példában a kiválasztott társíthatja az előfizetéshez tartozó termékkulccsal verzió. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Azt végezze el a gyorsítótárban keresés tekintheti meg, ha azt már rendelkezik olvassa be a kívánt ügyfél verziója.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Ezután azt ellenőrzi, ha azt nem találta meg a gyorsítótárban lévő.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Ha azt nem kellett volna, akkor azt lépjen, és megtalálja.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Bontsa ki a szövegtörzs kívánt válasz a kérdésre adott választ.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Vissza a későbbi felhasználásra a gyorsítótár tárolja azt.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

És végül frissítheti a háttéradatbázist URL-címet, jelölje ki a kívánt az ügyfél által szolgáltatás verzióját.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

A teljesen akkor házirendet, az alábbi képlettel történik.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Elegáns megoldást, amely sok API-a verziószámozás aggályokat API fogyasztói átlátszó szabályozhatja a kódmentes verziószámának használatban van ügyfelek anélkül, hogy frissítsen és telepítsen újra ügyfelek.

## <a name="tenant-isolation"></a>Bérlői elkülönítési

A nagyobb, több bérlői környezetekben egyes vállalatok hozzon létre külön csoportokat a bérlők a különböző környezetekben kódmentes eszközök kapacitása. Ez a kis méretűre állítása hatással van a hardver probléma a kódmentes a felhasználók számát. Azt is lehetővé teszi, hogy új szoftver verziók szakaszban változatwebhelyeken. Ideális a kódmentes architektúra átlátszó API-felhasználóknak lehetnek. Ez lehet elérni átlátszó verziószámozás hasonló módon mivel ugyanezzel a technikával konfigurációs állam / API-ja kulcs használatával kódmentes URL-cím kezelésére alapul.  

Helyett adatszolgáltató az előnyben részesített minden előfizetés billentyű az API-verziót, akkor adja vissza, hogy egy bérlői vonatkozik, a hozzárendelt hardver csoport azonosítót. Adott azonosító használható Egyenletszerkesztővel megfelelő kódmentes URL-CÍMÉT.

## <a name="summary"></a>Összefoglalás
A szabadságot az Azure API management gyorsítótár bármilyen típusú adatok tárolására szolgáló lehetővé teszi, hogy a hatékony hozzáférés érdekében konfigurációs adatok, amely hatással lehet a bejövő felkérés feldolgozása. Is használható, amelyek is kiegészítheti válaszokat, a kódmentes API által visszaadott adatok töredékek tárolásához.

## <a name="next-steps"></a>Következő lépések
Adjon visszajelzést a Disqus szálhoz tartozó üzenet Ez a témakör a Ha forgatókönyv más ezekről az irányelvekről engedélyezve van, az Ön számára, vagy ha vannak olyan esetek szeretné, hogy elérése, de nem úgy érzi jelenleg lehetséges.
