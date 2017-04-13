<properties
    pageTitle="Jelentkezzen Analytics API HTTP adatgyűjtő |} Microsoft Azure"
    description="A napló Analytics HTTP adatok adatgyűjtő API segítségével bejegyzés JSON adatok hozzáadása a napló Analytics-tár, amely felhívhatja a REST API-t bármely ügyfélprogramból. Ez a cikk azt ismerteti, hogyan használhatja az API-t, és az adatok közzététele másik programnyelv használatával példákat."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="bwren"/>


# <a name="log-analytics-http-data-collector-api"></a>Log Analytics HTTP adatgyűjtő API

Azure napló Analytics HTTP-adatgyűjtő API használatakor vehet bejegyzés JavaScript objektum jelölés (JSON) adatok a napló Analytics-tár, amely felhívhatja a REST API-t bármely ügyfélprogramból. Ez a módszer segítségével is küldhet adatok harmadik fél által fejlesztett alkalmazások vagy parancsfájlok, például az Azure automatizálás egy runbook.  

## <a name="create-a-request"></a>Kérelem létrehozása

A következő két táblázatokban minden kérés a napló Analytics HTTP adatok adatgyűjtő API-hoz szükséges attribútumokat. Azt ismertetik, hogy minden attribútum a cikk későbbi részében részletesebben.

### <a name="request-uri"></a>Kérés URI

| Attribútum | A tulajdonság |
|:--|:--|
| A módszer | BEJEGYZÉS |
| URI | https://\<CustomerId\>.ods.opinsights.azure.com/api/logs?api-version=2016-04-01 |
| Webhelytartalom-típus | alkalmazás/json |

### <a name="request-uri-parameters"></a>A kérelem URI paraméterei
| Paraméter | Leírás |
|:--|:--|
| Vevőkód  | A Microsoft műveletek Management Suite munkaterület egyedi azonosítója. |
| Erőforrás    | Az API-erőforrás neve: / api/naplók. |
| API-verzió | A kérés használata az API verziója. Ez jelenleg 2016-04-01. |

### <a name="request-headers"></a>Fejlécek kérése
| Élőfej | Leírás |
|:--|:--|
| Engedély | Engedély aláírását. A cikk erről arról, hogy miként hozhat létre egy HMAC-SHA256 fejléc. |
| Log-típus | Adja meg az adatokat, hogy Ön küldi a rekord típusát. A naplófájl típusa jelenleg csak alfa karaktereket. Nem támogatja a numerics betűket és különleges karaktereket. |
| x – az ms-dátum | A kérelem lett feldolgozott RFC 1123 formátumú dátum. |
| idő generált mező | Az adatok, az időbélyegző, az elem tartalmazó mező neve. Ha megad egy mező tartalma használható parancsmagokról **TimeGenerated**. Ha nem adja meg ezt a mezőt, **TimeGenerated** alapértelmezés szerint az üzenetet keresztül van a szervezetbe időpontját. Az üzenet mező tartalmát követnie kell az ISO 8601 formátumban YYYY-MM-DDThh:mm:ssZ. |


## <a name="authorization"></a>Engedély

A napló Analytics HTTP adatok adatgyűjtő API kérésének tartalmaznia kell egy engedélyezési fejléc. Hitelesíteni a kérést, be kell jelentkeznie a kérelmet, az elsődleges vagy a másodlagos kulcsa a munkaterületen, amely lehetővé teszi a kérést. Ezután adják át az aláírást a kérelem részeként.   

Az alábbiakban a formátumot a engedélyezési fejléc:

```
Authorization: SharedKey <WorkspaceID>:<Signature>
```

*WorkspaceID* a tevékenységek kezelése csomagja munkaterület egyedi azonosító érték. *Aláírás* , amely a kért összeállítás, és kattintson a [SHA256 algoritmus](https://msdn.microsoft.com/library/system.security.cryptography.sha256.aspx)használatával számított [Kivonat-alapú üzenet hitelesítési kód (HMAC)](https://msdn.microsoft.com/library/system.security.cryptography.hmacsha256.aspx) . Ezután, kódolását azt Base64 kódolással.

Ez a formátum segítségével kódolása a **SharedKey** aláírás karakterláncot:

```
StringToSign = VERB + "\n" +
               Content-Length + "\n" +
               Content-Type + "\n" +
               x-ms-date + "\n" +
               "/api/logs";
```

Íme egy példa aláírás karakterlánc:

```
POST\n1024\napplication/json\nx-ms-date:Mon, 04 Apr 2016 08:00:00 GMT\n/api/logs
```

Ha az aláírás karakterlánc, kódolását azt a UTF-8-címként kódolt karakterláncot a HMAC-SHA256 algoritmus alkalmazással, és ezután kódolását Base64 az eredményt. Ez a formátum használata:

```
Signature=Base64(HMAC-SHA256(UTF8(StringToSign)))
```

A következő szakaszokban a minták példakódot segít hozzon létre egy engedélyezési fejléc van.

## <a name="request-body"></a>Szervezet kérése

Az üzenet törzsében JSON kell lennie. A tulajdonság neve és értékpárjai működnek az egy vagy több rekord tartalmaznia kell a következő formátumban:

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

A köteg együtt az egyetlen kérelem több rekordot a következő formátumban. Összes rekord azonos típusú bejegyzéseket kell lennie.

```
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
},
{
"property1": "value1",
" property 2": "value2"
" property 3": "value3",
" property 4": "value4"
}
```

## <a name="record-type-and-properties"></a>Bejegyzés típusa és tulajdonságai

Egy egyéni bejegyzéstípus meg kell határoznia, a naplófájl Analytics HTTP adatok adatgyűjtő API adatoknak elküldésekor. Jelenleg adatok nem tud írni meglévő rekordtípusokat készített más adattípusok és megoldásokat. Log analitikai a bejövő adatokat olvas be, és ezután hoz létre, amelyek megfelelnek a megadott értékek adattípusainak tulajdonságait.

A napló Analytics API minden kérés rekordtípus nevű **Napló típusú** élőfej kell tartalmaznia. A utótag **_CL** automatikusan hozzáfűzi a név megadása azok megkülönböztetése más napló típusú egyéni naplózási szerint. Ha például beírta a nevét, **MyNewRecordType**, ha napló Analytics rekordot hoz létre **MyNewRecordType_CL**típusú. Ezzel biztosíthatja, hogy nincsenek-e a felhasználó által létrehozott címzettek nevét, és az aktuális vagy jövőbeli Microsoft-megoldások teljesült közötti ütközések.

Azonosítása egy tulajdonság adattípusát, az napló Analytics utótag hozzáadása a tulajdonság neve. Ha egy tulajdonság, egy null értéket tartalmaz, a tulajdonság nem szerepel, a rekordot. Az alábbi táblázat felsorolja a tulajdonság adattípusát és a megfelelő utótag:

| A tulajdonság adattípusát. | Utótag |
|:--|:--|
| Karakterlánc    | z |
| Logikai érték   | _B |
| Dupla    | _D |
| Dátum/idő | _Szo |
| GLOBÁLISAN EGYEDI AZONOSÍTÓJA      | _G |


Log Analytics által az egyes tulajdonság adattípusát attól függ, hogy már létezik az új rekord rekordtípus.

- Rekordtípus nem létezik, ha a napló Analytics hoz létre egy újat. Log Analytics a JSON-típus megállapítás alapján határozza meg az új rekord egyes tulajdonság adattípusát.
- A rekord típusa létezik, a naplófájl Analytics megkísérli hozzon létre egy új rekordot, a meglévő tulajdonságok alapján. Ha írja be az adatokat az új rekord tulajdonság nem egyezik meg, és nem alakítható át a meglévő típusa, vagy a rekord tartalmaz egy nem létező tulajdonság, napló Analytics létrehoz egy új tulajdonságot, amely rendelkezik a megfelelő utótag.

Például ez a bejegyzés Beküldési volna hozzon létre egy rekordot három tulajdonságait, **number_d**, **boolean_b**és **string_s**:

![Minta rekord 1](media/log-analytics-data-collector-api/record-01.png)

A következő bejegyzést, majd formátumú karakterláncként az összes érték elküldése, ha nem változna a Tulajdonságok parancsot. Ezeket az értékeket a meglévő adattípusok alakíthatók:

![Minta rekord 2](media/log-analytics-data-collector-api/record-02.png)

De ekkor a következő küldéskor történik, ha a napló Analytics az új tulajdonságok **boolean_d** és **string_d**hozna létre. Ezeket az értékeket nem alakítható:

![3 minta rekord](media/log-analytics-data-collector-api/record-03.png)

Ha a következő bejegyzést, majd a record type létrehozása előtt küldött, napló Analytics szeretné hozzon létre egy rekordot három tulajdonságait, **a sikeresek**, **boolean_s**és **string_s**. A bejegyzést a kezdőértéket mindegyike van formázva karakterláncként:

![Minta rekord 4](media/log-analytics-data-collector-api/record-04.png)


## <a name="data-limits"></a>Adatok korlátozása
Vannak bizonyos korlátozások a napló Analytics adatgyűjtési API közzétett adatok körül.

- Legfeljebb 30 MB bejegyzés napló Analytics adatainak adatgyűjtő API-hoz. Ez a egy méretkorlátok vonatkoznak azokra az egyszeri bejegyzésbe. Ha egyetlen adatainak közzétenni, amely meghaladja 30 MB-ot, illetve, az adatok felfelé kisebb méretű szövegadattömb küldhet neki egy időben. 
- Legfeljebb 32 KB korlátját mező értékeit. Ha a mező értéke 32 KB-nál nagyobb, az adatok csonkolja. 
- Egy adott típusú mezők ajánlott maximális száma 50. Ez a használhatóság és a keresési élmény perspektíva gyakorlati korlátozott.  


## <a name="return-codes"></a>Visszatérési kódok

A HTTP-állapotkód 202 azt jelenti, hogy elfogadta feldolgozás volna a felkérését, de a feldolgozás még nem fejeződött be. Ez azt jelzi, hogy a művelet sikeresen befejeződött.

Ez a táblázat, amelyek a feltétlenül járnak a szolgáltatás állapota kódok teljes készlete tartalmazza:

| Kód | Állapot | Kódszámú hiba jelenik meg | Leírás |
|:--|:--|:--|:--|
| 202 | Elfogadott |  | A kérés tud elfogadva. |
| 400 | Hibás kérés | InactiveCustomer | Megszűnt a munkaterület. |
| 400 | Hibás kérés | InvalidApiVersion | A szolgáltatás nem ismerhető fel a megadott API-verziót. |
| 400 | Hibás kérés | InvalidCustomerId | A megadott munkaterület azonosító érvénytelen. |
| 400 | Hibás kérés | InvalidDataFormat | Érvénytelen JSON elindították. A válasz szervezet tartalmazhat a hiba részletes tájékoztatást. |
| 400 | Hibás kérés | InvalidLogType | A naplófájl típusa zárt különleges karaktereket vagy numerics megadva. |
| 400 | Hibás kérés | MissingApiVersion | Az API-verzió nem lett megadva. |
| 400 | Hibás kérés | MissingContentType | A webhelytartalom-típus nem lett megadva. |
| 400 | Hibás kérés | MissingLogType | A szükséges értéket naplófájl típusa nem lett megadva. |
| 400 | Hibás kérés | UnsupportedContentType | A webhelytartalom-típus nem lett beállítva ****alkalmazás/JSON-ba. |
| 403 | Tiltott | InvalidAuthorization | A szolgáltatás nem sikerült hitelesíteni a kérést. Győződjön meg róla, hogy a munkaterület azonosító és a kapcsolat kulcs érvényes. |
| 500 | Belső kiszolgálóhiba | UnspecifiedError | A szolgáltatás belső hiba történt. Ismételje meg a kérést. |
| 503 | A szolgáltatás nem érhető el | ServiceUnavailable | A szolgáltatás jelenleg nem érhetők el a kérelem érkezik. Ismételje meg a kérést. |

## <a name="query-data"></a>Adatok lekérdezése

A napló Analytics HTTP adatok adatgyűjtő API által küldött adatok a lekérdezés, kereshet **típusa** , amely megegyezik az **LogType** értékkel rendelkező rekordokat, hogy adott, a hozzáfűzött **_CL**. Például ha **MyCustomLog**használt, majd azt adja vissza az összes rekordot **típus = MyCustomLog_CL**.


## <a name="sample-requests"></a>Minta kérések

A következő szakaszokban a minták, küldje el az adatokat a napló Analytics HTTP adatok adatgyűjtő API másik programnyelv használatával hogyan találhatók.

Hajtsa végre ezeket a lépéseket a változók beállítása a engedélyezési fejléc minden egyes minta:

1. A tevékenységek kezelése csomagja portálon válassza a **Beállítások** csempét, és válassza ki a **Kapcsolódó források** fülre.
2. **Munkaterület-azonosító**jobbra kattintson a Másolás ikonra, és illessze be a program a **Vevőkód** változó azonosítója.
3. Az **Elsődleges kulcs**jobbra kattintson a Másolás ikonra, és illessze be a program a **Megosztott kulcs** változó azonosítója.

Azt is megteheti a naplófájl típusa és a JSON adatainak változók módosíthatja.

### <a name="powershell-sample"></a>A PowerShell-minta

```
# Replace with your Workspace ID
$CustomerId = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"  

# Replace with your Primary Key
$SharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# Specify the name of the record type that you'll be creating
$LogType = "MyRecordType"

# Specify a field with the created time for the records
$TimeStampField = "DateValue"


# Create two records with the same set of properties to create
$json = @"
[{  "StringValue": "MyString1",
    "NumberValue": 42,
    "BooleanValue": true,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "9909ED01-A74C-4874-8ABF-D2678E3AE23D"
},
{   "StringValue": "MyString2",
    "NumberValue": 43,
    "BooleanValue": false,
    "DateValue": "2016-05-12T20:00:00.625Z",
    "GUIDValue": "8809ED01-A74C-4874-8ABF-D2678E3AE23D"
}]
"@

# Create the function to create the authorization signature
Function Build-Signature ($customerId, $sharedKey, $date, $contentLength, $method, $contentType, $resource)
{
    $xHeaders = "x-ms-date:" + $date
    $stringToHash = $method + "`n" + $contentLength + "`n" + $contentType + "`n" + $xHeaders + "`n" + $resource

    $bytesToHash = [Text.Encoding]::UTF8.GetBytes($stringToHash)
    $keyBytes = [Convert]::FromBase64String($sharedKey)

    $sha256 = New-Object System.Security.Cryptography.HMACSHA256
    $sha256.Key = $keyBytes
    $calculatedHash = $sha256.ComputeHash($bytesToHash)
    $encodedHash = [Convert]::ToBase64String($calculatedHash)
    $authorization = 'SharedKey {0}:{1}' -f $customerId,$encodedHash
    return $authorization
}


# Create the function to create and post the request
Function Post-OMSData($customerId, $sharedKey, $body, $logType)
{
    $method = "POST"
    $contentType = "application/json"
    $resource = "/api/logs"
    $rfc1123date = [DateTime]::UtcNow.ToString("r")
    $contentLength = $body.Length
    $signature = Build-Signature `
        -customerId $customerId `
        -sharedKey $sharedKey `
        -date $rfc1123date `
        -contentLength $contentLength `
        -fileName $fileName `
        -method $method `
        -contentType $contentType `
        -resource $resource
    $uri = "https://" + $customerId + ".ods.opinsights.azure.com" + $resource + "?api-version=2016-04-01"

    $headers = @{
        "Authorization" = $signature;
        "Log-Type" = $logType;
        "x-ms-date" = $rfc1123date;
        "time-generated-field" = $TimeStampField;
    }

    $response = Invoke-WebRequest -Uri $uri -Method $method -ContentType $contentType -Headers $headers -Body $body -UseBasicParsing
    return $response.StatusCode

}

# Submit the data to the API endpoint
Post-OMSData -customerId $customerId -sharedKey $sharedKey -body ([System.Text.Encoding]::UTF8.GetBytes($json)) -logType $logType  
```

### <a name="c-sample"></a>C# minta

```
using System;
using System.Net;
using System.Security.Cryptography;

namespace OIAPIExample
{
    class ApiExample
    {
// An example JSON object, with key/value pairs
        static string json = @"[{""DemoField1"":""DemoValue1"",""DemoField2"":""DemoValue2""},{""DemoField1"":""DemoValue3"",""DemoField2"":""DemoValue4""}]";

// Update customerId to your Operations Management Suite workspace ID
        static string customerId = "xxxxxxxx-xxx-xxx-xxx-xxxxxxxxxxxx";

// For sharedKey, use either the primary or the secondary Connected Sources client authentication key   
        static string sharedKey = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx";

// LogName is name of the event type that is being submitted to Log Analytics
        static string LogName = "DemoExample";

// You can use an optional field to specify the timestamp from the data. If the time field is not specified, Log Analytics assumes the time is the message ingestion time
        static string TimeStampField = "";

        static void Main()
        {
// Create a hash for the API signature
            var datestring = DateTime.UtcNow.ToString("r");
            string stringToHash = "POST\n" + json.Length + "\napplication/json\n" + "x-ms-date:" + datestring + "\n/api/logs";
            string hashedString = BuildSignature(stringToHash, sharedKey);
            string signature = "SharedKey " + customerId + ":" + hashedString;

            PostData(signature, datestring, json);
        }

// Build the API signature
        public static string BuildSignature(string message, string secret)
        {
            var encoding = new System.Text.ASCIIEncoding();
            byte[] keyByte = Convert.FromBase64String(secret);
            byte[] messageBytes = encoding.GetBytes(message);
            using (var hmacsha256 = new HMACSHA256(keyByte))
            {
                byte[] hash = hmacsha256.ComputeHash(messageBytes);
                return Convert.ToBase64String(hash);
            }
        }

// Send a request to the POST API endpoint
        public static void PostData(string signature, string date, string json)
        {
            string url = "https://"+ customerId +".ods.opinsights.azure.com/api/logs?api-version=2016-04-01";
            using (var client = new WebClient())
            {
                client.Headers.Add(HttpRequestHeader.ContentType, "application/json");
                client.Headers.Add("Log-Type", LogName);
                client.Headers.Add("Authorization", signature);
                client.Headers.Add("x-ms-date", date);
                client.Headers.Add("time-generated-field", TimeStampField);
                client.UploadString(new Uri(url), "POST", json);
            }
        }
    }
}
```

### <a name="python-sample"></a>Python minta

```
import json
import requests
import datetime
import hashlib
import hmac
import base64

# Update the customer ID to your Operations Management Suite workspace ID
customer_id = 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

# For the shared key, use either the primary or the secondary Connected Sources client authentication key   
shared_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# The log type is the name of the event that is being submitted
log_type = 'WebMonitorTest'

# An example JSON web monitor object
json_data = [{
   "slot_ID": 12345,
    "ID": "5cdad72f-c848-4df0-8aaa-ffe033e75d57",
    "availability_Value": 100,
    "performance_Value": 6.954,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "true"
},
{   
    "slot_ID": 67890,
    "ID": "b6bee458-fb65-492e-996d-61c4d7fbb942",
    "availability_Value": 100,
    "performance_Value": 3.379,
    "measurement_Name": "last_one_hour",
    "duration": 3600,
    "warning_Threshold": 0,
    "critical_Threshold": 0,
    "IsActive": "false"
}]
body = json.dumps(json_data)

#####################
######Functions######  
#####################

# Build the API signature
def build_signature(customer_id, shared_key, date, content_length, method, content_type, resource):
    x_headers = 'x-ms-date:' + date
    string_to_hash = method + "\n" + str(content_length) + "\n" + content_type + "\n" + x_headers + "\n" + resource
    bytes_to_hash = bytes(string_to_hash).encode('utf-8')  
    decoded_key = base64.b64decode(shared_key)
    encoded_hash = base64.b64encode(hmac.new(decoded_key, bytes_to_hash, digestmod=hashlib.sha256).digest())
    authorization = "SharedKey {}:{}".format(customer_id,encoded_hash)
    return authorization

# Build and send a request to the POST API
def post_data(customer_id, shared_key, body, log_type):
    method = 'POST'
    content_type = 'application/json'
    resource = '/api/logs'
    rfc1123date = datetime.datetime.utcnow().strftime('%a, %d %b %Y %H:%M:%S GMT')
    content_length = len(body)
    signature = build_signature(customer_id, shared_key, rfc1123date, content_length, method, content_type, resource)
    uri = 'https://' + customer_id + '.ods.opinsights.azure.com' + resource + '?api-version=2016-04-01'

    headers = {
        'content-type': content_type,
        'Authorization': signature,
        'Log-Type': log_type,
        'x-ms-date': rfc1123date
    }

    response = requests.post(uri,data=body, headers=headers)
    if (response.status_code == 202):
        print 'Accepted'
    else:
        print "Response code: {}".format(response.status_code)

post_data(customer_id, shared_key, body, log_type)
```

## <a name="next-steps"></a>Következő lépések

- Adatok elküldése az egyéni nézeteket [Nézet Tervező](log-analytics-view-designer.md) használatával.
