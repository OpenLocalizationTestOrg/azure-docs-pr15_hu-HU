<properties
    pageTitle="HTTP-kérések készítése API alkalmazáskezelési szolgáltatás használatával"
    description="Olvassa el a kérelem és válasz házirendek kezelése lapon API segítségével hívhat fel külső szolgáltatások a API"
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


# <a name="using-external-services-from-the-azure-api-management-service"></a>Az Azure API alkalmazáskezelési szolgáltatás a külső szolgáltatások használata

A házirendek az Azure API alkalmazáskezelési szolgáltatás elérhető számos hasznos munka alapján kizárólag a beérkező kérelmet, a kimenő üzenetekhez és egyszerű konfigurációs adatok hajthat végre. Házirendek azonban éppen tud majd kommunikálni a külső szolgáltatásokat az API megnyílik számos további lehetőségek.

Azt van korábban meg benne azt és együttműködését a [naplózás, a figyelés és analytics Azure esemény központi szolgáltatást](api-management-log-to-eventhub-sample.md). Ez a cikk azt fogja bemutatják, szabályok, amelyek lehetővé teszik vezérléséhez minden olyan külső HTTP-alapú szolgáltatás. Ezek a házirendek használható kiváltó események távoli vagy az eredeti kérelem és válasz valamilyen módon kezelhetők használt adatok beolvasása.

## <a name="send-one-way-request"></a>Küldés – egy – módon-kérés
Esetleg a legegyszerűbb külső kapcsolati, amely lehetővé teszi, hogy egy külső szolgáltatásban, ha értesítést szeretne kapni a fontos esemény valamilyen kérelem fire és elfelejti stílusának. A control flow házirend ábrázolásakor `choose` észleli, hogy azt érdekli, majd a feltétel teljesül, ha eljuttatjuk kérését a [Küldés – egy – módon -](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) házirend használata külső HTTP kérelem feltételének semmilyen kitöltést. Ez lehet például Hipchat tartalékidő vagy a Posta API például SendGrid vagy MailChimp egy üzenetkezelő rendszer egy kérést, vagy a kritikus támogatási események úgy, mint ahogy PagerDuty. Összes alábbi üzenetkezelő rendszer rendelkezik, egyszerű HTTP API-hoz, amely azt egyszerűen meghívásához.

### <a name="alerting-with-slack"></a>A tartalékidő riasztási
A következő példa bemutatja, hogyan lehet üzenetet küldeni tartalékidő csevegőszoba, ha a HTTP-válasz állapotkód-nél nagyobb vagy egyenlő 500. Egy tartomány 500 hiba azt jelzi, hogy a kódmentes API az API az ügyfélnek nem találja meg saját maguk probléma. Általában a részen beavatkozás valamilyen van szükség.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Tartalékidő az állomástól bejövő webes horgok tartalmaz. Egy bejövő web hurok beállításakor a tartalékidő egy speciális URL-címet, amely lehetővé teszi az egyszerű POST kérelem ehhez és történő átadásakor egy üzenetet a tartalékidő csatorna hoz létre. A JSON-szervezet, amely hozzunk létre alapuló tartalékidő által meghatározott formátumban.

![Tartalékidő Web hurok](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Fire és elfelejti elég jó?
Vannak bizonyos kompromisszumok kérelem fire és elfelejti stílus használata esetén. Ha valamilyen oknál fogva a kérelem sikertelen, majd a hiba nem lehet jelenteni. A rendszer és a további teljesítmény költsége a kérdésre adott választ vár jelentése másodlagos hiba problémákat összetettsége adott esetben nem indokolt. Felhasználási területei, ahol fontos jelölje be a választ majd a [Küldés -](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) házirend beállítás jobb.

## <a name="send-request"></a>Küldés-kérés
A `send-request` házirend lehetővé teszi a külső szolgáltatással összetett feldolgozás feladatokat, és lépjen vissza az adatokat az API-kezelés szolgáltatás, amely további házirend feldolgozás használhatók.

### <a name="authorizing-reference-tokens"></a>Hivatkozás tokenek engedélyezése
A fő függvény API-kezelés védi az kódmentes erőforrásokat. Ha a engedélyezési kiszolgáló által az API-val használt hoz létre [JWT tokenek](http://jwt.io/) az oauth2 hitelesítési mód folyamat részeként [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , ahogy is használhatja, majd a `validate-jwt` házirendet, amely ellenőrzi a token érvényességét. Bizonyos engedély-kiszolgálók azonban létrehozni, Mit nevezünk [hivatkozás tokenek](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) anélkül, hogy a hívásokat a engedélyezési kiszolgáló nem lehet ellenőrizni.

### <a name="standardized-introspection"></a>Szabványos introspection
Az elmúlt fordult nem szabványos módon ellenőrzésére egy hivatkozás token engedély-kiszolgálóval. Azonban a legutóbb javasolt szabványos [RFC 7662](https://tools.ietf.org/html/rfc7662) lett teszi közzé a IETF, amely definiálja, hogyan ellenőrizheti egy erőforrás-kiszolgáló, jogkivonat érvényességét.

### <a name="extracting-the-token"></a>A token kibontása
Első lépésként a token kinyerése a engedélyezési fejléc. Az élőfej érték lesz formázva a `Bearer` engedélyezési séma, szóköz, majd a jóváhagyási jogkivonat [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1)szerint. Sajnos előfordulhatnak olyan esetek, ahol az engedélyezési séma argumentumot. A fiók elemzése során, azt felosztása egy üres élőfej értékét, és válassza ki az utolsó karakterlánc a visszaadott tömböt a karakterláncok. Ez a cikk a rosszul formázott engedélyezési fejlécek.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Az adatérvényesítési kérés
Van még a jóváhagyási jogkivonat, amikor az értekezlet-összehívást a token érvényesítése eljuttatjuk kérését. RFC 7662 felhívja ezt a folyamatot introspection és megköveteli, hogy Ön `POST` egy HTML-űrlap a introspection erőforrásnézethez. A HTML-űrlap legalább tartalmaznia kell egy kulcsot/érték pár kulccsal `token`. A kérés engedélyezési kiszolgálóra sikerült hitelesíteni annak érdekében, hogy a kártékony ügyfelek nem lépjen ezeken az érvényes tokenek.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>A válasz ellenőrzése
A `response-variable-name` attribútum hozzáférést, a visszaadott válasz használatos. Egy kulcsot be a nevét, az Ez a tulajdonság definiált is alkalmazható a `context.Variables` szótár eléréséhez a `IResponse` objektumot.

A válasz objektumból a szervezet meghallgathatja azt, és RFC 7622 tájékoztat arról velünk, hogy a kérdésre adott választ kell lennie a JSON-objektum, és tartalmaznia kell legalább egy tulajdonság, egy úgynevezett `active` , amely egy logikai érték. Ha `active` értéke igaz, majd a token érvényes.

### <a name="reporting-failure"></a>Jelentéskészítési hiba
Használja a `<choose>` házirend feltárása a token érvénytelen, és ha igen, lépjen vissza egy 401 választ.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Ismerteti [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) szerint hogyan `bearer` tokenek kell használni, akkor is visszaadhat egy `WWW-Authenticate` 401 fejlécet. Kérje meg egy ügyfelet, hogy miként megfelelően hivatalos kérelmének összeállításához a WWW-hitelesítést végezni alkalmas. Az alábbi módszerek az oauth2 hitelesítési mód keretrendszer lehetséges széles választéka miatt célszerű nehezen kommunikáció a szükséges adatokat. Nincsenek Szerencsére erőfeszítéseket meghívhat érdekében [ügyfelek megtudhatja, hogy miként megfelelően az erőforrás-kiszolgálón kérések engedélyezése](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Véglegesként megoldás
Az összes elemek összeillesztése, akkor jelenik meg a következő házirendet:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Ez csak az egyik sok példája hogyan a `send-request` házirend használható hasznos külső szolgáltatások integrálása a folyamat kérések és válaszok lépés a API-kezelés szolgáltatáson keresztül.

## <a name="response-composition"></a>Válasz kialakítási
A `send-request` házirend használhatók a kódmentes rendszert elsődleges kérelmének fejlesztése, bekerül az előző példában, vagy azt a kódmentes hívás a teljes csere is alkalmazható. Ez a módszer segítségével egyszerűen létrehozunk összetett erőforrások vannak összesíti több rendszerekből.

### <a name="building-a-dashboard"></a>Irányítópult létrehozása   
Előfordul, hogy szeretne tudja jelenítik meg az információkat, amelyek több kódmentes rendszerek, például létezik, és egy irányítópult meghajtóra. A KPI-k származik, minden más vissza részek, de nem szeretné őket közvetlen hozzáférést biztosít, és ez jó lenne, ha minden információ olvasható az egyetlen kérelem. Esetleg a kódmentes adatai szüksége van néhány szeletelés és kockára vágás, és végezzen fertőtlenítése először! Összetett anyagerőforráshoz, a gyorsítótárban való a segítségére lehetnek a kódmentes betöltés csökkentheti, mint felhasználójánál egy hogy beütés az F5 billentyűt a megjelenítéséhez, ha megváltozik a underperforming mértékek tudja.    

### <a name="faking-the-resource"></a>Az erőforrás faking
Az első az irányítópult erőforrás létrehozásába lépésként új művelet megadása az API kezelése a publisher-portálon. Ez egy helyőrző művelet össze a dinamikus erőforrás kialakítási szabályzatunk konfigurálásához lesz.

![Irányítópult művelet](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>A kérelem tétele
Miután a `dashboard` művelet létrehozva azt adhatja meg a házirend kifejezetten az adott művelet. 

![Irányítópult művelet](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Első lépésként lekérdezés paramétereket kinyerése a beérkező kérelmet, hogy azt is továbbítja azokat meg kódmentes. Ebben a példában az irányítópult egy idő alapján adatokat mutatja egy tehát még egy `fromDate` és `toDate` paraméter. Ábrázolásakor a `set-variable` házirendet, amely az adatok kinyerése a kérelem URL-címe.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Miután ezt az információt felkínálunk kérések eljuttatjuk kérését összes kódmentes rendszerek. Minden egyes kérelem létrehozza az új URL-CÍMÉT a paraméter adatokat, és a megfelelő kiszolgálót hívja, és a válasz egy helyi változót tárolja.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Ezek a kérelmek sorozatból, amely nem ideális hajtja végre. Az egy rövidesen azt fogja bevezetése nevű új házirendet, `wait` , amely engedélyezi az összes párhuzamosan hajtsa végre ezeket a kérelmeket.

### <a name="responding"></a>Válasz

Az összetett válasz összeállításához ábrázolásakor a [return-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) házirend. A `set-body` elem kifejezés segítségével egy új Egyenletszerkesztővel `JObject` tulajdonságként beágyazott összetevő megadott együtt.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

A teljes házirend a következőképpen néz ki:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

A helyőrző a konfigurációban azt adhatja meg az irányítópult erőforrás gyorsítótárazott legalább egy órával, mert azt megértéséhez, hogy az adatok jellegét művelet azt jelenti, hogy akkor is, ha egy óra szorul, továbbra sem lesznek eléggé hatékony értékes információáramlás a felhasználók számára.

## <a name="summary"></a>Összefoglalás
Azure API alkalmazáskezelési szolgáltatás, amely a HTTP-forgalmat szelektív alkalmazott rugalmas házirendeket biztosít, és lehetővé teszi a kialakítási kódmentes szolgáltatások. E szeretne növeli a riasztási funkciók, ellenőrzési, adatérvényesítési funkciók API átjárónak, vagy hozzon létre új összetett erőforrásokhoz több kódmentes szolgáltatások, a `send-request` és kapcsolódó házirendek, nyissa meg a lehetőségek tárházát.

## <a name="watch-a-video-overview-of-these-policies"></a>Ezek a házirendek áttekintését bemutató videó megtekintése
A [Küldés-egy – módon-kérési](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), a [Küldés-kérési](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)és a [return-válasz](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) házirendek hatálya alá tartozó, a jelen cikkben bővebben megtekintés a következő videót.

> [AZURE.VIDEO send-request-and-return-response-policies]
