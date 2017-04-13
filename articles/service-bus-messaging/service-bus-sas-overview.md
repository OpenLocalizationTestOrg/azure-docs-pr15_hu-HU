<properties
    pageTitle="A megosztott Access aláírások áttekintése |} Microsoft Azure"
    description="Mik azok a megosztott Access aláírások, hogyan működnek, és hogyan használhatók csomópontot, PHP a és C#."
    services="service-bus"
    documentationCenter="na"
    authors="djrosanova"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/02/2016"
    ms.author="darosa;sethm"/>

# <a name="shared-access-signatures"></a>Megosztott Access aláírások

*A megosztott Access aláírások* (Társítások) szolgáltatás Bus, beleértve az esemény hubok brokered üzenetküldés (sorban várakozó és témakörök) szolgáló elsődleges biztonsági eljárás, és továbbítását üzenetküldés. Ez a cikk azt ismerteti, hogy megosztott Access aláírások, hogyan működnek és használatuk platform-felismerése nélkül útján.

## <a name="overview-of-sas"></a>Társítások áttekintése

Megosztott Access aláírások SHA-256 biztonságos tiltva vagy URL-címe alapján hitelesítési módszer. Társítások az összes szolgáltatás Bus szolgáltatás által használt rendkívül hatékony mechanizmusa. Tényleges használatban Társítások két összetevője: egy *Megosztott Access-házirend* és a *Megosztott Access-aláírás* (más néven egy *jogkivonat*).

A [megosztott Access-aláírás hitelesítéssel szolgáltatás Bus](service-bus-shared-access-signature-authentication.md)található Access aláírások megosztott szolgáltatás Bus részletesebb információt.

## <a name="shared-access-policy"></a>Megosztott hozzáférési házirend

Egy fontos tudnivaló a Társítások szeretne tudni az, hogy az összes kezdődik házirend. Minden a házirend használata mellett dönt három információkat vehet: **nevét**, a **hatókör**és az **engedélyeket**. A **név** szerint csak; egy egyedi nevet a hatálya alá. A hatókör elég egyszerűen: az URI a szóban forgó erőforrás. A szolgáltatás Bus névtér a hatóköre a teljes tartománynevét (FQDN), például: `https://<yournamespace>.servicebus.windows.net/`.

Az elérhető engedélyek házirendhez mindig nagymértékben értetődő:

  + Küldés
  + Meghallgatása
  + Kezelése

Miután létrehozta a házirendet, egy *Elsődleges kulcs* és egy *Másodlagos kulcsa*van rendelve. Ezek a billentyűparancsok kriptográfiailag erős. Nem vész őket, vagy előfordulhat őket – ezek mindig lesz elérhető, az [Azure-portálon][]. A létrehozott billentyűk választhat, és bármikor helyreállíthatók. Jó helyen jár Ha az elsődleges kulcs a házirend módosítása vagy újragenerálása, belőle létrehozott megosztott Access minden olyan aláírást fog érvényét.

Szolgáltatás Bus névteret létrehozásakor a teljes névteret **RootManageSharedAccessKey**nevű automatikusan létrejön egy házirendet, és ezzel a házirenddel minden engedéllyel rendelkezik. **Legfelső szintű**, így nem használja ezt a házirendet, hacsak nincs igazán jó ok nem bejelentkezni. A **beállítás** lapon a névtér a portálon további házirendeket hozhat létre. Fontos, hogy egyetlen fa szolgáltatás Bus szinttel Megjegyzés (névtér, várólista, esemény-központban stb.) csak is csatlakozik 12 házirendek.

## <a name="shared-access-signature-token"></a>Megosztott Access aláírás (jogkivonat)

A házirend magát nem az jogkivonat-szolgáltatás Bus. Célszerű az objektumra, amelyhez létre az jogkivonat - vagy az elsődleges és másodlagos billentyű használatával. A token gondosan létrehozásával a következő formátumú karakterláncként jön létre:

```
SharedAccessSignature sig=<signature-string>&se=<expiry>&skn=<keyName>&sr=<URL-encoded-resourceURI>
```

Hol `signature-string` van, a hatókör a jogkivonat (**hatókör** az előző szakaszban leírtak szerint) az SHA-256 kivonat egy fűzött CRLF és a lejárati idő (másodpercek a alapidőpont a: `00:00:00 UTC` 1 január 1970 a).

A kivonat az alábbi pszeudokód hasonlít, és 32 bájt adja eredményül.

```
SHA-256('https://<yournamespace>.servicebus.windows.net/'+'\n'+ 1438205742)
```

A nem kivonatolt értékei a **SharedAccessSignature** karakterláncban, hogy a címzett is számítja ki a kivonat ugyanolyan paraméterekkel, és ellenőrizze, hogy ugyanazt az eredményt adja. A URI, adja meg egy keresési tartományt, és a kulcs neve azonosítja a házirendet, amely a kivonat kiszámítására használható. Ez a biztonsági szempontból fontos. Ha az aláírás nem egyezik meg, amely kiszámítja a címzett (Service Bus), a hozzáférés megtagadva. Ezen a ponton biztos lehet, hogy a feladó volt a kulcs eléréséhez, és biztosítani a a tartalomvédelmi a házirendet a megadott.

## <a name="generating-a-signature-from-a-policy"></a>Aláírás létrehozása a egy házirend

Ténylegesen hogyan Ez a kód? Most tekintsünk át egy ezek közül néhány.

### <a name="nodejs"></a>NodeJS

```
function createSharedAccessToken(uri, saName, saKey) { 
    if (!uri || !saName || !saKey) { 
            throw "Missing required parameter"; 
        } 
    var encoded = encodeURIComponent(uri); 
    var now = new Date(); 
    var week = 60*60*24*7;
    var ttl = Math.round(now.getTime() / 1000) + week;
    var signature = encoded + '\n' + ttl; 
    var signatureUTF8 = utf8.encode(signature); 
    var hash = crypto.createHmac('sha256', saKey).update(signatureUTF8).digest('base64'); 
    return 'SharedAccessSignature sr=' + encoded + '&sig=' +  
        encodeURIComponent(hash) + '&se=' + ttl + '&skn=' + saName; 
}
``` 

### <a name="java"></a>Java

```
private static String GetSASToken(String resourceUri, String keyName, String key)
  {
      long epoch = System.currentTimeMillis()/1000L;
      int week = 60*60*24*7;
      String expiry = Long.toString(epoch + week);

      String sasToken = null;
      try {
          String stringToSign = URLEncoder.encode(resourceUri, "UTF-8") + "\n" + expiry;
          String signature = getHMAC256(key, stringToSign);
          sasToken = "SharedAccessSignature sr=" + URLEncoder.encode(resourceUri, "UTF-8") +"&sig=" +
                  URLEncoder.encode(signature, "UTF-8") + "&se=" + expiry + "&skn=" + keyName;
      } catch (UnsupportedEncodingException e) {

          e.printStackTrace();
      }

      return sasToken;
  }


public static String getHMAC256(String key, String input) {
    Mac sha256_HMAC = null;
    String hash = null;
    try {
        sha256_HMAC = Mac.getInstance("HmacSHA256");
        SecretKeySpec secret_key = new SecretKeySpec(key.getBytes(), "HmacSHA256");
        sha256_HMAC.init(secret_key);
        Encoder encoder = Base64.getEncoder();

        hash = new String(encoder.encode(sha256_HMAC.doFinal(input.getBytes("UTF-8"))));

    } catch (InvalidKeyException e) {
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
   } catch (IllegalStateException e) {
        e.printStackTrace();
    } catch (UnsupportedEncodingException e) {
        e.printStackTrace();
    }

    return hash;
}
```

### <a name="php"></a>PHP

```
function generateSasToken($uri, $sasKeyName, $sasKeyValue) 
{ 
$targetUri = strtolower(rawurlencode(strtolower($uri))); 
$expires = time();  
$expiresInMins = 60; 
$week = 60*60*24*7;
$expires = $expires + $week; 
$toSign = $targetUri . "\n" . $expires; 
$signature = rawurlencode(base64_encode(hash_hmac('sha256',             
 $toSign, $sasKeyValue, TRUE))); 

$token = "SharedAccessSignature sr=" . $targetUri . "&sig=" . $signature . "&se=" . $expires .      "&skn=" . $sasKeyName; 
return $token; 
}
```
 
### <a name="c35"></a>C & #35;

```
private static string createToken(string resourceUri, string keyName, string key)
{
    TimeSpan sinceEpoch = DateTime.UtcNow - new DateTime(1970, 1, 1);
    var week = 60 * 60 * 24 * 7;
    var expiry = Convert.ToString((int)sinceEpoch.TotalSeconds + week);
    string stringToSign = HttpUtility.UrlEncode(resourceUri) + "\n" + expiry;
    HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(key));
    var signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
    var sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}", HttpUtility.UrlEncode(resourceUri), HttpUtility.UrlEncode(signature), expiry, keyName);
    return sasToken;
}
```

## <a name="using-the-shared-access-signature-at-http-level"></a>A megosztott Access aláírás használatának (HTTP szinten)
 
Most, hogy tudja, hogyan hozhat létre a megosztott Access aláírások bármely entitás a szolgáltatás Bus, készen áll egy HTTP POST végrehajtása:

```
POST https://<yournamespace>.servicebus.windows.net/<yourentity>/messages
Content-Type: application/json
Authorization: SharedAccessSignature sr=https%3A%2F%2F<yournamespace>.servicebus.windows.net%2F<yourentity>&sig=<yoursignature from code above>&se=1438205742&skn=KeyName
ContentType: application/atom+xml;type=entry;charset=utf-8
``` 
    
Ne feledje, hogy az minden működik. Társítások várólista, témakör, előfizetés, esemény központi vagy továbbítási hozhat létre. Publisher identitás esemény hubok használ, ha egyszerűen fűzi `/publishers/< publisherid>`.

A feladóra vagy a biztonsági jogkivonat ügyfél ad, ha nem rendelkeznek a kulcs közvetlenül, és nem vonható vissza őket a kivonat beszerzése azt. Ilyen hozzáférése van hozzáférési lehetőségei fölé, és mennyi ideig. Ne feledje, hogy egy fontos tudnivaló, hogy az elsődleges kulcs a házirend módosításakor belőle létrehozott megosztott Access minden olyan aláírást fog érvényteleníti.

## <a name="using-the-shared-access-signature-at-amqp-level"></a>A megosztott Access aláírás használatának (AMQP szinten)

Az előző szakaszban bemutattuk HTTP POST kérésével a biztonsági jogkivonat használatáról a szolgáltatás Bus adatokat küldi. Tudja meg, mint a speciális üzenet Queuing Protocol (AMQP), amely a teljesítmény okokból sok esetben használni az előnyben részesített protokoll használatával szolgáltatás Bus érheti el. A dokumentum, amely a munka piszkozat óta a 2013-at, de jó támogatja Azure Ma [AMQP Claim-Based biztonsági 1.0-s verziója](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc) a Társítások jogkivonat használatát a AMQP leírása.

Adatok küldése a szolgáltatás Bus indításához a publisher kell küldenie a Társítások jogkivonat belül AMQP üzenetben **$cbs** nevű pontosan meghatározott AMQP csomópont (láthatóvá válik a "speciális" várólista szerezheti be, és az összes Társítások tokenek érvényesítésére a szolgáltatás által használt). A publisher meg kell adnia a **ReplyTo** mező belül a AMQP üzenet; Ez az a, amelyben a szolgáltatást a Publisher, a token érvényességi (egyszerű kérés/válasz mintát a publisher és a szolgáltatás között) eredményt tartalmazó válaszok csomópontot. A válasz csomópont létrejön "menet," a "távoli csomópont dinamikus létrehozása" Felolvasás AMQP 1.0 specifikációja által leírt módon. Annak ellenőrzése, hogy a biztonsági jogkivonat érvényes, miután a publisher előrelépés, és indítsa el a szolgáltatás adatokat küldeni.

Az alábbi lépésekkel bemutatják, hogyan küldhet a biztonsági jogkivonat AMQP protokoll használatával a [AMQP.Net Lite](https://github.com/Azure/amqpnetlite) tárat. Ez akkor hasznos, ha nem használja a hivatalos szolgáltatás Bus SDK (például a WinRT, .net Compact Framework, .net Micro keretrendszer és egyszínű) c elkészítésének\#. Természetesen a tár akkor hasznos lehet ahhoz, hogy hogyan jogcímalapú biztonsági megértésében módon működik-e a AMQP szintjén látta, hogy hogyan működik a HTTP szintjén (a HTTP-bejegyzés felkérés és az "Engedély" élőfejen belül küldött Társítások jogkivonat). Ha már nincs szükség az ilyen AMQP mély ismereteket, is használhatja a hivatalos szolgáltatás Bus SDK csomagjában talál a .net keretrendszer alkalmazások, amelyek a érhetek meg.

### <a name="c35"></a>C & #35;

```
/// <summary>
/// Send claim-based security (CBS) token
/// </summary>
/// <param name="shareAccessSignature">Shared access signature (token) to send</param>
private bool PutCbsToken(Connection connection, string sasToken)
{
    bool result = true;
    Session session = new Session(connection);

    string cbsClientAddress = "cbs-client-reply-to";
    var cbsSender = new SenderLink(session, "cbs-sender", "$cbs");
    var cbsReceiver = new ReceiverLink(session, cbsClientAddress, "$cbs");

    // construct the put-token message
    var request = new Message(sasToken);
    request.Properties = new Properties();
    request.Properties.MessageId = Guid.NewGuid().ToString();
    request.Properties.ReplyTo = cbsClientAddress;
    request.ApplicationProperties = new ApplicationProperties();
    request.ApplicationProperties["operation"] = "put-token";
    request.ApplicationProperties["type"] = "servicebus.windows.net:sastoken";
    request.ApplicationProperties["name"] = Fx.Format("amqp://{0}/{1}", sbNamespace, entity);
    cbsSender.Send(request);

    // receive the response
    var response = cbsReceiver.Receive();
    if (response == null || response.Properties == null || response.ApplicationProperties == null)
    {
        result = false;
    }
    else
    {
        int statusCode = (int)response.ApplicationProperties["status-code"];
        if (statusCode != (int)HttpStatusCode.Accepted && statusCode != (int)HttpStatusCode.OK)
        {
            result = false;
        }
    }

    // the sender/receiver may be kept open for refreshing tokens
    cbsSender.Close();
    cbsReceiver.Close();
    session.Close();

    return result;
}
```

A `PutCbsToken()` módszer a *kapcsolat* (AMQP kapcsolat osztály példány [AMQP .NET Lite tár](https://github.com/Azure/amqpnetlite)meghatározott), hogy a TCP-kapcsolatot a szolgáltatás és az *sasToken* paraméter, amely a biztonsági jogkivonat küldése a kap. 

> [AZURE.NOTE] Fontos, hogy a kapcsolat létrejött a **külső SASL hitelesítési módszer beállítása** (és a nem használt, amikor nem kell küldje el a biztonsági jogkivonat felhasználónév és jelszó az alapértelmezett egyszerű).

Ezután a publisher a biztonsági jogkivonat válna (jogkivonat érvényességi eredmény) válasz a szolgáltatásból két AMQP hivatkozást hoz létre.

A AMQP üzenet tulajdonságainak és egy egyszerű üzenet több információt tartalmaz. A biztonsági jogkivonat (segítségével saját konstruktoraikban) az üzenet törzsébe. A **"ReplyTo"** tulajdonság értéke az érvényességi eredményt, a címzett hivatkozásra (igény szerint módosíthatja a nevére, amelyet a szolgáltatás által dinamikusan létrejön, és) fogadására a csomópont nevét. Az utolsó három alkalmazás/egyéni tulajdonságok, hogy milyen típusú művelet van végrehajtani a szolgáltatást használják. CBS piszkozat specifikációja által ahogy azok a **művelet neve** ("helyezése-jogkivonat"), **Írja be a token** (ebben az esetben az "servicebus.windows.net:sastoken") és a **"név" célközönség** , amelyre a token vonatkozik (a teljes szervezet) kell lennie.

A biztonsági jogkivonat elküldése a feladó hivatkozásra, után a publisher kell olvassa el a válasz, a címzett hivatkozásra. A válasz egy egyszerű AMQP üzenet-alkalmazás tulajdonság neve **"állapotkódja"** , amely az egy HTTP állapotkód értékeket tartalmazhat. 

## <a name="next-steps"></a>Következő lépések

A [szolgáltatás Bus REST API-hivatkozás](https://msdn.microsoft.com/library/azure/hh780717.aspx) mire alkalmas a Társítások tokenek kapcsolatban további tudnivalókat talál.

A hitelesítési szolgáltatás Bus kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus hitelesítési és engedélyezési](service-bus-authentication-and-authorization.md)című témakört. 

További példákat a és C# Java Script Társítások [blogbejegyzésben](http://developers.de/blogs/damir_dobric/archive/2013/10/17/how-to-create-shared-access-signature-for-service-bus.aspx)szerepelnek.

[Azure portál]: https://portal.azure.com