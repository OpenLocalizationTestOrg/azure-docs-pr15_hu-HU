<properties
   pageTitle="A többi használata a Power BI beágyazott |} Microsoft Azure"
   description="Megtudhatja, hogy miként használata a Power BI beágyazott többi "
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="how-to-use-power-bi-embedded-with-rest"></a>A többi Power BI beágyazott használata


## <a name="power-bi-embedded-what-it-is-and-what-its-for"></a>A Power BI beágyazott: Mi ez, és mit
A Power BI beágyazott áttekintése a [Power BI beágyazott webhely](https://azure.microsoft.com/services/power-bi-embedded/)-hivatalos leírása, de most tekintsünk át egy rövid előtt azt foglalkozni használata a többi részleteit.

Hogy valójában meglehetősen egyszerű. Egy külső gyakran szeretne használni a saját alkalmazások dinamikus adatok [A Power BI](https://powerbi.microsoft.com) megjelenítések felhasználói felület building blocks parancsot.

De tudja, a Power BI szolgáltatásban vagy a csempék beillesztése az weblapon már nélkül is lehetséges a Power BI beágyazott Azure Service szolgáltatást, a **Power BI API**segítségével. Ha meg szeretné megosztani a jelentéseket a ugyanarra szervezetében, a jelentések ágyazhatja be az Azure Active Directory authentication. A felhasználó, aki tekint meg a jelentések kell saját Azure AD-fiók használatával jelentkezzen be. Ha szeretné megosztani a reports (külső felhasználók is beleértve) minden felhasználó számára, egyszerűen beágyazhatja névtelen hozzáféréssel.

Jelenik meg, ez az egyszerű megoldás beágyazása, de nem igazán igényeinek egy külső alkalmazást.
A legtöbb Szoftverfejlesztői alkalmazások kell az adatokat ad a saját ügyfeleknek, nem feltétlenül saját a szervezet felhasználói. Például esetén tartja meg a vállalat és a B vállalat néhány szolgáltatás, a vállalat felhasználói kell csak olvassa el az adatok saját céget válaszok parancsra. Ez azt jelenti, hogy a több elem bérleti kézbesítve van szükség.

A külső alkalmazás is előfordulhat, hogy kínáló saját hitelesítési módszereket, például űrlapalapú hitelesítés, alapszintű hitelesítés stb... Ezután a beágyazási megoldást kell együttműködhet másokkal a meglévő hitelesítési módszereket biztonságosan. Is szükség a felhasználók számára használhatja a felesleges vásárlása nélkül külső alkalmazásokon vagy licencelési a Power BI-előfizetésben.

 **A Power BI beágyazott** készült külső esetek pontosan az ilyen típusú. Így most, hogy az adott rövid útmutatást talál az útból felkínálunk pedig hozzuk működésbe be néhány részletei

.NET használható \(C#) vagy Node.js SDK csomagjában talál, az alkalmazás, a Power BI beágyazott egyszerűen létrehozásához. De ez a cikk azt ismertetik a HTTP-adatfolyam \(AuthN együtt) a Power BI SDK nélkül. A folyamat ismertetése, az alkalmazás **bármely programozási nyelven**készíthet, és meg érthető mélyen Power BI beágyazott lényege.

## <a name="create-power-bi-workspace-collection-and-get-access-key-provisioning"></a>Munkaterület-webhelycsoport létrehozása a Power BI és a get-hívóbetű \(létesítése)
A Power BI beágyazott az egyik az Azure szolgáltatások. Csak az Azure-portálon használó külső terheli használatát díjak \(óránként felhasználói munkamenet /), és a jelentés megjeleníti a felhasználó nem fel, vagy akár az Azure-előfizetésre van szükség.
Mielőtt hozzákezdene a saját alkalmazások fejlesztése, azt létre kell hoznia a **Power BI-munkaterület webhelycsoport** Azure portál használatával.

A Power BI beágyazott egyes munkaterületeken a munkaterület ügyfelek (bérlő esetében), és azt sok munkaterületek minden munkaterület webhelycsoport adhat hozzá. Az azonos hívóbetű minden munkaterület webhelycsoport használják. Az effektus, a munkaterület gyűjteménye a biztonsági oszlopazonosító, a Power BI beágyazott.

![](media\power-bi-embedded-iframe\create-workspace.png)

Amikor a munkaterületet gyűjtemény szerkesztésének befejezéséhez azt másolja a hívóbetű Azure-portálon.

![](media\power-bi-embedded-iframe\copy-access-key.png)

> [AZURE.NOTE] Azt is a munkaterület-gyűjtemény kiépítése és első hívóbetű keresztül REST API-t. További tudnivalókért olvassa el a [Power BI erőforrás szolgáltató API -khoz](https://msdn.microsoft.com/library/azure/mt712306.aspx)című témakört.

## <a name="create-pbix-file-with-power-bi-desktop"></a>A Power BI Desktop .pbix fájl létrehozása
Ezután azt létre kell hoznia az adatbázisból származó adatok és jelentéseit lesz beágyazva.
Ehhez a feladathoz nem tartozik programozás vagy kódot. Csak a Power BI Desktop használjuk.
Ez a cikk azt nem hajtsa végre a Power BI Desktop használatáról olvashat. Ha segítségre van itt, olvassa el a [Power BI Desktop – első lépések](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/)című témakört. Példa a [Kiskereskedelmi Analysis minta](https://powerbi.microsoft.com/documentation/powerbi-sample-datasets/)csak használjuk.

![](media\power-bi-embedded-iframe\power-bi-desktop-1.png)

## <a name="create-a-power-bi-workspace"></a>A Power BI-munkaterület létrehozása

Most, hogy a kiépítési összes befejeződött, lássunk hozzá egy ügyfél-munkaterület létrehozása a munkaterület gyűjteményben REST API-khoz keresztül. A következő HTTP bejegyzés kérése (REST) az új munkaterület létrehozása a meglévő munkaterület a webhelycsoportban. Ebben a példában a munkaterület webhelycsoport neve **mypbiapp**.
Azt csak beállíthatja azt előzőleg másolt, hívóbetű **AppKey**. Rendkívül egyszerű hitelesítés!

**HTTP-kérés**

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-válasz**

```
HTTP/1.1 201 Created
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces
RequestId: 4220d385-2fb3-406b-8901-4ebe11a5f6da

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/$metadata#workspaces/$entity",
  "workspaceId": "32960a09-6366-4208-a8bb-9e0678cdbb9d",
  "workspaceCollectionName": "mypbiapp"
}
```

A visszaadott **workspaceId** későbbi API-hívások szolgál. Az alkalmazás ezt az értéket kell tartania.

## <a name="import-pbix-file-into-the-workspace"></a>A munkaterület .pbix fájl importálása
Egyes munkaterületeken üzemeltetheti egyetlen a Power BI Desktop fájl az adatkészletet \(többek között az adatforrás-beállítások), és jelentéseket. A .pbix fájl a munkaterületre, ahogy az alábbi kód importálhatja azt. Amint látható, azt is feltölthet a bináris .pbix fájl MIME multipart http használatával.

A uri fragment **32960a09-6366-4208-a8bb-9e0678cdbb9d** a workspaceId, lekérdezési paraméter **datasetDisplayName** pedig a az adatkészlet nevét. A létrehozott adatkészlet rendelkezik, az összes adat kapcsolódó .pbix az eltérések a fájl olyan importált adatokat, az egérmutatót az adatforrás stb...

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports?datasetDisplayName=mydataset01
Authorization: AppKey MpaUgrTv5e...
Content-Type: multipart/form-data; boundary="A300testx"

--A300testx
Content-Disposition: form-data

{the content (binary) of .pbix file}
--A300testx--
```

Importálás a feladat egy ideig működni. Amikor elkészült, az alkalmazás felteheti a tevékenység állapota importálása azonosítójával. Ebben a példában az importálás azonosító **4eec64dd-533b-47c3-a72c-6508ad854659**.

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659?tenantId=myorg
RequestId: 658bd6b4-b68d-4ec3-8818-2a94266dc220

{"id":"4eec64dd-533b-47c3-a72c-6508ad854659"}
```

A következő is kérdezi állapotát az importálás azonosító:

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/imports/4eec64dd-533b-47c3-a72c-6508ad854659
Authorization: AppKey MpaUgrTv5e...
```

Ha a tevékenység nem teljes, a HTTP-válasz lehet jelennek meg:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: 614a13a5-4de7-43e8-83c9-9cd225535136

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Publishing",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "name": "mydataset01"
}
```

Ha a tevékenység befejeződött, a HTTP-válasz lehet további jelennek meg:

```
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
RequestId: eb2c5a85-4d7d-4cc2-b0aa-0bafee4b1606

{
  "id": "4eec64dd-533b-47c3-a72c-6508ad854659",
  "importState": "Succeeded",
  "createdDateTime": "2016-07-19T07:36:06.227",
  "updatedDateTime": "2016-07-19T07:36:06.227",
  "reports": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://app.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c"
    }
  ],
  "datasets": [
    {
      "id": "458e0451-7215-4029-80b3-9627bf3417b0",
      "name": "mydataset01",
      "tables": [
      ],
      "webUrl": "https://app.powerbi.com/datasets/458e0451-7215-4029-80b3-9627bf3417b0"
    }
  ],
  "name": "mydataset01"
}
```

## <a name="data-source-connectivity-and-multi-tenancy-of-data"></a>Adatforrás-kapcsolat \(és az adatok több bérleti)
Szinte minden .pbix fájlban az eltérések a program importálja a munkaterületre, miközben az adatforrások hitelesítő adatok nincsenek. Emiatt **DirectQuery módban**használja, a beágyazott jelentés nem megfelelően jelennek. De **importálása**móddal azt tekinthetik meg a jelentést, a meglévő importált adatok felhasználásával. Ebben az esetben azt kell állítani a hitelesítő adatokat, az alábbi lépésekkel többi hívások keresztül.

Az átjáró adatforrás először be kell szereznie azt. Azt tudható, hogy az adatkészlet **azonosító** az előzőleg visszaadott azonosítóját.

**HTTP-kérés**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.GetBoundGatewayDatasources
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-válasz**

```
GET HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: 574b0b18-a6fa-46a6-826c-e65840cf6e15

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#gatewayDatasources",
  "value": [
    {
      "id": "5f7ee2e7-4851-44a1-8b75-3eb01309d0ea",
      "gatewayId": "ca17e77f-1b51-429b-b059-6b3e3e9685d1",
      "datasourceType": "Sql",
      "connectionDetails": "{\"server\":\"testserver.database.windows.net\",\"database\":\"testdb01\"}"
    }
  ]
}
```

A visszaadott átjáróhoz és adatforrás-azonosító használatával \(olvassa el a korábbi **gatewayId** és a visszaadott eredmény **azonosító** ), azt módosíthatja az alábbi képlettel történik, az adatforrás hitelesítő adatok:

**HTTP-kérés**

```
PATCH https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/gateways/ca17e77f-1b51-429b-b059-6b3e3e9685d1/datasources/5f7ee2e7-4851-44a1-8b75-3eb01309d0ea
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "credentialType": "Basic",
  "basicCredentials": {
    "username": "demouser",
    "password": "P@ssw0rd"
  }
}
```

**HTTP-válasz**

```
HTTP/1.1 200 OK
Content-Type: application/octet-stream
RequestId: 0e533c13-266a-4a9d-8718-fdad90391099
```

A gyártási azt is beállíthatja a különböző kapcsolati karakterláncot az egyes munkaterületeken REST API-t használja. \(azaz, azt is külön az adatbázis minden egyes ügyfelek számára.)

A következő megváltoztatása a kapcsolati karakterláncot az adatforrás a többi keresztül.

```
POST https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/datasets/458e0451-7215-4029-80b3-9627bf3417b0/Default.SetAllConnections
Authorization: AppKey MpaUgrTv5e...
Content-Type: application/json; charset=utf-8

{
  "connectionString": "data source=testserver02.database.windows.net;initial catalog=testdb02;persist security info=True;encrypt=True;trustservercertificate=False"
}
```

Vagy a Power BI beágyazott ábrázolásakor a sor biztonsági szint, és azt is elválaszthatja egymástól az adatokat egy jelentésben szereplő minden felhasználónál. As a Result, hogy minden azonos .pbix ügyfél jelentés kiépítése \(felhasználói felülete stb.) és különböző forrásokból.

> [AZURE.NOTE] **Importálási mód** helyett **DirectQuery módban**használja, ha nincs modellek API-e frissíteni mód. És a helyszíni adatforrások a Power BI átjárón keresztül még nem támogatott a Power BI beágyazott. Azonban valójában szeretné figyelemmel az újdonságokat a [Power BI-blog](https://powerbi.microsoft.com/blog/) , és milyen újdonságokat jövőbeli elengedi.

## <a name="authentication-and-hosting-embedding-reports-in-our-web-page"></a>Hitelesítés és (beágyazási)-jelentéseket tartalmazó az weblapon

Az előző REST API-t azt a engedélyezési fejléc a következő hívóbetű **AppKey** magát használhatók. Ezeket a hívásokat is kezelhető kódmentes kiszolgálóoldalon, mert az megbízható.

De ha a jelentés azt beágyazására a weblapon, ilyen típusú biztonsági információk volna kezelhető használatáról JavaScript \(frontend). Kattintson a fejléc értékét is védeni szeretné. Ha saját hívóbetű felhasználói rosszindulatú vagy kártékony kódok által van, azok felhívhatja a billentyű használata műveletek.

A jelentés azt beágyazására a weblapon, ha a kiszámított jogkivonat kell használjuk hívóbetű **AppKey**helyett. Az alkalmazás létre kell hoznia az OAuth Json webes jogkivonat \(JWT) amely követelések és a kiszámított digitális aláírás áll. Alább látható módon a OAuth JWT a pont tagolt kódolt karakterláncot tokenek.

![](media\power-bi-embedded-iframe\oauth-jwt.png)

Először is azt kell készítenie a bemeneti érték, amely később jelentkezett-e. Ezt az értéket a base64 URL-címként kódolandó (rfc4648) a következő json karakterláncot, és ezek a pont határolja \(.) karakter. Később megismerheti a jelentés azonosító beszerzése.

> [AZURE.NOTE] Sor szintű biztonsági (RLS) használata a Power BI beágyazott szeretnénk, ha azt kell adnia **felhasználónevét** és a **szerepkör** a követelések.

```
{
  "typ":"JWT",
  "alg":"HS256"
}
```

```
{
  "wid":"{workspace id}",
  "rid":"{report id}",
  "wcn":"{workspace collection name}",
  "iss":"PowerBISDK",
  "ver":"0.2.0",
  "aud":"https://analysis.windows.net/powerbi/api",
  "nbf":{start time of token expiration},
  "exp":{end time of token expiration}
}
```

Ezután azt létre kell hoznia a base64-címként kódolt karakterláncot HMAC \(az aláírás) SHA256 algoritmus. Az aláírt beviteli értéke az előző karakterlánc.

Az utolsó, azt kell alakítania a bemeneti érték és aláírás karakterlánc időszak használatával \(.) karakter. Az elkészült karakterlánca az alkalmazás jogkivonathoz beágyazásának jelentés. Akkor is, ha az alkalmazás jogkivonathoz rosszindulatú felhasználó van talált, nem tudnak elérni az eredeti hívóbetű. Ez az alkalmazás jogkivonat gyorsan lejár.

Íme egy PHP példa az alábbi lépéseket:

```
<?php
// 1. power bi access key
$accesskey = "MpaUgrTv5e...";

// 2. construct input value
$token1 = "{" .
  "\"typ\":\"JWT\"," .
  "\"alg\":\"HS256\"" .
  "}";
$token2 = "{" .
  "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
  "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
  "\"wcn\":\"mypbiapp\"," . // workspace collection name
  "\"iss\":\"PowerBISDK\"," .
  "\"ver\":\"0.2.0\"," .
  "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
  "\"nbf\":" . date("U") . "," .
  "\"exp\":" . date("U" , strtotime("+1 hour")) .
  "}";
$inputval = rfc4648_base64_encode($token1) .
  "." .
  rfc4648_base64_encode($token2);

// 3. get encoded signature
$hash = hash_hmac("sha256",
    $inputval,
    $accesskey,
    true);
$sig = rfc4648_base64_encode($hash);

// 4. show result (which is the apptoken)
$apptoken = $inputval . "." . $sig;
echo($apptoken);

// helper functions
function rfc4648_base64_encode($arg) {
  $res = $arg;
  $res = base64_encode($res);
  $res = str_replace("/", "_", $res);
  $res = str_replace("+", "-", $res);
  $res = rtrim($res, "=");
  return $res;
}
?>
```

## <a name="finally-embed-the-report-into-the-web-page"></a>Végül a jelentés beágyazása a az weblapon

A jelentés beágyazásának azt kell első beágyazás URL-címét, és jelentse a **azonosítója** , a következő REST API.

**HTTP-kérés**

```
GET https://api.powerbi.com/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/reports
Authorization: AppKey MpaUgrTv5e...
```

**HTTP-válasz**

```
HTTP/1.1 200 OK
Content-Type: application/json; odata.metadata=minimal; odata.streaming=true
RequestId: d4099022-405b-49d3-b3b7-3c60cf675958

{
  "@odata.context": "http://wabi-us-east2-redirect.analysis.windows.net/v1.0/collections/mypbiapp/workspaces/32960a09-6366-4208-a8bb-9e0678cdbb9d/$metadata#reports",
  "value": [
    {
      "id": "2027efc6-a308-4632-a775-b9a9186f087c",
      "name": "mydataset01",
      "webUrl": "https://app.powerbi.com/reports/2027efc6-a308-4632-a775-b9a9186f087c",
      "embedUrl": "https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c",
      "isFromPbix": false
    }
  ]
}
```

A jelentés beágyazása azt a saját webalkalmazás használatával az előző alkalmazás token.
Ha megnézi az a következő példa kód, a korábbi része ugyanaz, mint az előző példában. Későbbi szakaszában, ez a példa bemutatja a **embedUrl** \(az előző eredményt) az IFRAME-et, és a bejegyzés írása a alkalmazás jogkivonat az iframe be.

> [AZURE.NOTE] Módosítsa a jelentés azonosító értékét az egyik saját kell. Emellett a tartalmak kezeléséhez rendszerünk egy hiba miatt az iframe címke, a kód minta olvasható betűhíven. Ha másolja és illessze be ezt a minta kódot a címke eltávolítása a határértéket meghaladó szöveget.

```
    <?php
    // 1. power bi access key
    $accesskey = "MpaUgrTv5e...";

    // 2. construct input value
    $token1 = "{" .
      "\"typ\":\"JWT\"," .
      "\"alg\":\"HS256\"" .
      "}";
    $token2 = "{" .
      "\"wid\":\"32960a09-6366-4208-a8bb-9e0678cdbb9d\"," . // workspace id
      "\"rid\":\"2027efc6-a308-4632-a775-b9a9186f087c\"," . // report id
      "\"wcn\":\"mypbiapp\"," . // workspace collection name
      "\"iss\":\"PowerBISDK\"," .
      "\"ver\":\"0.2.0\"," .
      "\"aud\":\"https://analysis.windows.net/powerbi/api\"," .
      "\"nbf\":" . date("U") . "," .
      "\"exp\":" . date("U" , strtotime("+1 hour")) .
      "}";
    $inputval = rfc4648_base64_encode($token1) .
      "." .
      rfc4648_base64_encode($token2);

    // 3. get encoded signature value
    $hash = hash_hmac("sha256",
        $inputval,
        $accesskey,
        true);
    $sig = rfc4648_base64_encode($hash);

    // 4. get apptoken
    $apptoken = $inputval . "." . $sig;

    // helper functions
    function rfc4648_base64_encode($arg) {
      $res = $arg;
      $res = base64_encode($res);
      $res = str_replace("/", "_", $res);
      $res = str_replace("+", "-", $res);
      $res = rtrim($res, "=");
      return $res;
    }
    ?>
    <!DOCTYPE html>
    <html>
    <head>
      <meta charset="utf-8" />
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
      <title>Test page</title>
      <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
      <button id="btnView">View Report !</button>
      <div id="divView">
        <**REMOVE THIS CAPPED TEXT IF COPIED** iframe id="ifrTile" width="100%" height="400"></iframe>
      </div>
      <script>
        (function () {
          document.getElementById('btnView').onclick = function() {
            var iframe = document.getElementById('ifrTile');
            iframe.src = 'https://embedded.powerbi.com/appTokenReportEmbed?reportId=2027efc6-a308-4632-a775-b9a9186f087c';
            iframe.onload = function() {
              var msgJson = {
                action: "loadReport",
                accessToken: "<?=$apptoken?>",
                height: 500,
                width: 722
              };
              var msgTxt = JSON.stringify(msgJson);
              iframe.contentWindow.postMessage(msgTxt, "*");
            };
          };
        }());
      </script>
    </body>
```

És így az eredmény:

![](media\power-bi-embedded-iframe\view-report.png)

Ekkor a Power BI beágyazott csak jeleníti meg a jelentést az iframe. De a [Power BI-Blog]()figyelemmel. Jövőbeli fejlesztések új ügyféloldali API-k fog tudassa velünk az iframe az információkat, valamint információk használhatja. Izgalmas elemek!


## <a name="see-also"></a>Lásd még:
- [Hitelesítése és a Power BI beágyazott engedélyezése](power-bi-embedded-app-token-flow.md)
