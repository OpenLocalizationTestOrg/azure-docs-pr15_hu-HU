<properties
 pageTitle="Fejlesztőeszközök útmutató - IoT központi való hozzáférés szabályozása |} Microsoft Azure"
 description="Azure IoT központi Fejlesztőeszközök útmutató - IoT központi való hozzáférés szabályozása és biztonságának kezelési módját"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="control-access-to-iot-hub"></a>IoT központi való hozzáférés szabályozása

## <a name="overview"></a>– Áttekintés

Ez a cikk bemutatja az biztonságossá tétele a IoT-központját. IoT központi *engedélyeket* való hozzáférést minden IoT központi végpontok használja. Engedélyek az access IoT hubhoz funkciók alapján korlátozza.

Ez a cikk ismerteti:

- A különböző engedélyeket, hogy egy eszköz vagy a háttéradatbázist app hozzáférésének a IoT központi biztosíthat.
- A hitelesítési folyamat és a tokenek segítségével engedélyek ellenőrzése.
- Hogyan lehet adott erőforrásokhoz való hozzáférés korlátozása a hatókör hitelesítő adatait.
- X.509 tanúsítványok IoT központi támogatása.
- Meglévő eszköz identitás nyilvántartó vagy hitelesítési sémák hitelesítési mechanizmusok egyéni eszközt.

### <a name="when-to-use"></a>Mikor érdemes használni

A központi IoT végpontok közül eléréséhez szükséges engedélyekkel kell rendelkeznie. Például egy eszközt a hitelesítő adatokat együtt minden egyes IoT központi küld üzenetet tartalmazó jogkivonat kell tartalmaznia.

## <a name="access-control-and-permissions"></a>Hozzáférés-vezérlés és engedélyek

[Engedélyeket](#iot-hub-permissions) adhat az alábbi módon:

* A **központi szintű megosztott hozzáférési**. Megosztott hozzáférési [engedélyek](#iot-hub-permissions)tetszőleges kombinációját adhatnak. Megadhatja a házirendek az [Azure portál][lnk-management-portal], vagy a [IoT központi erőforrás szolgáltató REST API -khoz]használatával programozás útján[lnk-resource-provider-apis]. Egy újonnan létrehozott IoT hubhoz az alábbi alapértelmezett házirendeket alkalmazhatja:

    - **iothubowner**: minden engedélyekkel rendelkező házirend.
    - **szolgáltatás**: ServiceConnect engedéllyel rendelkező házirend.
    - **eszköz**: DeviceConnect engedéllyel rendelkező házirend.
    - **registryRead**: RegistryRead engedéllyel rendelkező házirend.
    - **registryReadWrite**: házirend RegistryRead és RegistryWrite engedéllyel.


* **Eszköz hitelesítő adatokat**. Minden egyes IoT központi tartalmaz egy [eszköz identitás beállításjegyzék][lnk-identity-registry]. A beállításjegyzék minden eszköznél a hitelesítő adatokat, hogy a megfelelő eszköz végpontok hatóköre **DeviceConnect** engedélyeket is beállíthatja.

Ha például a tipikus IoT megoldást:

- Az eszköz felügyeleti összetevő az *registryReadWrite* házirendet használja.
- Az esemény feldolgozó összetevő használja a *szolgáltatás* házirend.
- A futtatókörnyezet eszköz üzleti logikai összetevő a *szolgáltatás* házirendet használja.
- Egyes eszközök csatlakoztatása az IoT-központban identitás beállításjegyzékben tárolt hitelesítő adataival.

## <a name="authentication"></a>Hitelesítés

Azure IoT központi végpontok hozzáférést biztosít a megosztott access-házirendeket és eszköz identitás beállításjegyzék hitelesítő adatokat ellen token ellenőrzésével.

Hitelesítő adatokat, például a szimmetrikus kulcsok, amelyek el nem küldött a hálózaton.

> [AZURE.NOTE] Az Azure IoT központi erőforrás-szolgáltató által biztosított az Azure előfizetéssel, amelyek az [Azure erőforrás-kezelő]összes szolgáltató[lnk-azure-resource-manager].

Arról, hogy miként biztonsági tokenek használja, és a további tudnivalókért lásd: [IoT központi biztonsági tokenek][lnk-sas-tokens].

### <a name="protocol-specifics"></a>Protokoll használatát részletek

Egyes támogatott protokollok, például MQTT, AMQP és a HTTP Előtaggal alapú átvitel esetében: tokenek különböző módokon.

Amikor MQTT, a csatlakozás csomag megegyezik a deviceId ClientId, {iothubhostname} / {deviceId} a Username mező és a biztonsági jogkivonat, a jelszó mezőbe. {iothubhostname} a IoT központi (például contoso.azure-devices.net), a teljes CName kell lennie.

[AMQP]használatakor[lnk-amqp], IoT központi támogatja [SASL egyszerű] [ lnk-sasl-plain] és [AMQP Jogcímeken alapuló-biztonsági][lnk-cbs].

Ha AMQP jogcímeken alapuló-biztonsági használja, a standard meghatározza az tokenek továbbítja.

Az egyszerű SASL a **felhasználónév** lehet:

* `{policyName}@sas.root.{iothubName}`Ha központi szintű tokenek használata.
* `{deviceId}@sas.{iothubname}`Ha az eszköz – munkafüzetszintű tokenek használata.

Mindkét esetben a jelszó mezőben a jogkivonat, [IoT központi biztonsági tokenek]ismertetett módon[lnk-sas-tokens].

HTTP hitelesítési **engedélyezési** kérelem fejlécében érvényes token felvételével hajtja végre.

#### <a name="example"></a>Példa

Username (DeviceId a kis-és nagybetűket):`iothubname.azure-devices.net/DeviceId`

Jelszó (eszköz Intézővel Társítások létrehozása):`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [AZURE.NOTE] Az [Azure IoT központi SDK] [ lnk-sdks] automatikus generálása tokenek, amikor a csatlakozást a szolgáltatáshoz. Egyes esetekben az SDK nem támogatják a protokollok vagy a hitelesítési módszereket.

### <a name="special-considerations-for-sasl-plain"></a>EGYSZERŰ SASL megfontolandó különleges szempontok

SASL egyszerű AMQP való használatakor ügyfél IoT-hubhoz csatlakozik egy egyetlen token használható minden TCP-kapcsolat. Ha lejár a jogkivonat, a TCP-kapcsolatot a szolgáltatásról történő leválasztáshoz, és újracsatlakozás elindítja. A jelenség, amíg nem okoz-alkalmazás háttéradatbázist összetevő nagyon káros eszköz melletti alkalmazáshoz az alábbi okok miatt:

*  Átjárók általában csatlakozás sok szinkronizált eszköz nevében. EGYSZERŰ SASL használatakor az egyes eszközök IoT-hubhoz csatlakozik, a különböző TCP-kapcsolat létrehozása rendelkeznek. Ebben az esetben jelentősen növeli a felhasználás power és a hálózati erőforrások, és megnöveli a késés minden eszköz kapcsolat.
* Erőforrás-korlátozott eszközök negatív érinti az erőforrások újracsatlakozás után minden jogkivonat lejárati megnövelt használata.

## <a name="scope-hub-level-credentials"></a>Hatókör központi szintű hitelesítő adatok

Korlátozott hozzáférésű erőforráshoz URI tokenek létrehozásával központi szintű biztonsági házirendek is korlátozhatja. Például az eszköz a felhőbe üzenetek küldéséhez a eszközről végpontot is **/devices/ {deviceId} / üzenetek/események**. Jelentkezzen be a jogkivonat, amelynek resourceURI **/devices/ {deviceId}** **DeviceConnect** engedélyekkel egy megosztott access központi szintű házirend is használhatja. Ezt a megközelítést létrehoz egy jogkivonat, amely csak használható eszköz **deviceId**nevében üzeneteket küldhet.

Ez az eljárás hasonlít az [esemény hubok publisher házirend][lnk-event-hubs-publisher-policy], és lehetővé teszi, hogy az egyéni hitelesítési módszereket végrehajtása.

## <a name="security-tokens"></a>Biztonsági tokenek

IoT központi biztonsági tokenek használja az eszközök és szolgáltatások elkerülése érdekében a billentyűk küld a vezetékes hitelesítést végezni. Ezenkívül biztonsági tokenek érvényesség és hatókör korlátozott. [Azure IoT központi SDK] [ lnk-sdks] automatikus generálása jelölőnégyzet tokenek anélkül, hogy minden olyan speciális konfigurációs. Bizonyos esetekben azonban a felhasználók létrehozása és használata a biztonsági tokenek közvetlenül van szükség. Ezek közé tartozik a MQTT, AMQP vagy HTTP felületek közvetlen használatára, vagy a jogkivonat-szolgáltatás minta végrehajtásának [egyéni eszköz hitelesítési]leírtak[lnk-custom-auth].

IoT központi is lehetővé teszi, hogy IoT központi hitelesíteni eszközök [X.509 tanúsítványok][lnk-x509]. 

### <a name="security-token-structure"></a>Biztonsági jogkivonat szerkezete
Biztonsági tokenek segítségével adja meg az idő határolt access, az eszközök és szolgáltatások funkcióiról IoT-központban. Annak érdekében, hogy csak az engedélyezett eszközök és szolgáltatások csatlakoztatása, a biztonsági tokenek megosztott házirend hívóbetű vagy egy eszköz identitással a identitás beállításjegyzékben tárolt szimmetrikus kulcsot kell bejelentkeznie.

Jogkivonat egy megosztott access házirend kulcs a hozzáférést a megosztott hozzáférési házirend engedélyeket társított összes funkció aláírva. Kézzel egy eszköz identitás szimmetrikus kulccsal aláírt jogkivonat csak engedélyt a **DeviceConnect** a társított eszköz identitás.

A biztonsági jogkivonat formátuma a következő:

    SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}

A várt értékek a következők:

| Érték | Leírás |
| ----- | ----------- |
| {aláírás} | Az űrlap aláírás HMAC-SHA256 karakterláncra: `{URL-encoded-resourceURI} + "\n" + expiry`. **Fontos**: A kulcs a base64 dekódolva és a HMAC-SHA256 kiszámítása végrehajtásához kulcsként használt. |
| {resourceURI} | URI-előtag (által szakasz), a végpontokat is elérhető, a token kezdődő hostname (állomásnév): a IoT központi (protokoll). Ha például`myHub.azure-devices.net/devices/device1` |
| {lejárati} | UTF8 karakterláncok a alapidőpont 00:00:00 UTC 1 január 1970 a óta eltelt másodpercek számát. |
| {URL-címként kódolt-resourceURI} | Alsó az URL-kódolás a kisbetű erőforrás URI eset |
| {Házirendnév} | A megosztott hozzáférési házirendet, amely a token vonatkozik neve. Az eszköz rendszerleíró hitelesítő adatok hivatkozó tokenek esetében távol. |

**Megjegyzés: a előtag**: A URI-előtag szakasz és nem karakter számítja ki. Ha például `/a/b` van az előtag `/a/b/c` nem `/a/bc`.

A következő Node.js kódtöredékének jeleníti meg, amely kiszámítja a bemeneti adatok alapján a jogkivonat **generateSasToken** nevű függvényt `resourceUri, signingKey, policyName, expiresInMins`. A következő szakaszok részletesen a különböző felhasználandó különböző jogkivonat használati eset inicializálni hogyan.

    var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
        resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();

        // Set expiration in seconds
        var expires = (Date.now() / 1000) + expiresInMins * 60;
        expires = Math.ceil(expires);
        var toSign = resourceUri + '\n' + expires;

        // Use crypto
        var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
        hmac.update(toSign);
        var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

        // Construct autorization string
        var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
        + base64UriEncoded + "&se=" + expires;
        if (policyName) token += "&skn="+policyName;
        return token;
    };

Összehasonlítása, mint a egyenértékű Python kódot, és a biztonsági jogkivonat készítése van:

    from base64 import b64encode, b64decode
    from hashlib import sha256
    from time import time
    from urllib import quote_plus, urlencode
    from hmac import HMAC

    def generate_sas_token(uri, key, policy_name, expiry=3600):
        ttl = time() + expiry
        sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
        print sign_key
        signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

        rawtoken = {
            'sr' :  uri,
            'sig': signature,
            'se' : str(int(ttl))
        }

        if policy_name is not None:
            rawtoken['skn'] = policy_name

        return 'SharedAccessSignature ' + urlencode(rawtoken)

> [AZURE.NOTE] Mivel a token idő érvényességét IoT központi gépeken érvényesítése, fontos, hogy a gép által generált a token órájának a eltolódás is minimális.

### <a name="use-sas-tokens-in-a-device-client"></a>Az eszköz ügyfél Társítások tokenek használata

Szerezze be a biztonsági tokenek **DeviceConnect** IoT központi engedélyeket kétféleképpen: [az eszköz identitás beállításjegyzékéből szimmetrikus eszköz billentyűt](#use-a-symmetric-key-in-the-identity-registry), vagy egy [megosztott hozzáférési házirend kulcs](#use-a-shared-access-policy)az.

Ne feledje, hogy, hogy minden funkció elérhető eszközök tervezés előtaggal végponton van-e téve `/devices/{deviceId}`.

> [AZURE.IMPORTANT] Csak úgy, hogy IoT központi hitelesíti egy bizonyos eszközt használ az eszköz identitás szimmetrikus billentyűt. Azokban az esetekben egy megosztott hozzáférési házirend eszköz használható funkciók körét, eléréséhez használatakor a megoldást figyelembe kell vennie a megbízható részletes a biztonsági jogkivonat kibocsátása összetevőt.

Az eszköz elérésű végpontokat is (függetlenül a protokoll):

| Végpont | Funkciók |
| ----- | ----------- |
| `{iot hub host name}/devices/{deviceId}/messages/events` | Eszköz a felhőbe üzenetek küldése. |
| `{iot hub host name}/devices/{deviceId}/devicebound` | Felhőalapú-eszköz üzeneteket fogadni. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Az identitás beállításjegyzékben szimmetrikus termékkulccsal

Egy eszköz identitás szimmetrikus kulcs kattintva állítsa elő a házirendnév jogkivonat használatakor (`skn`) elemet a jogkivonat argumentumot.

A létrehozott összes eszköz funkciók eléréséhez jogkivonat, például a következő paraméterek kell rendelkeznie:

* erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* aláírási kulcs: minden olyan szimmetrikus kulcsa a `{device id}` identitást
* Nincs házirend neve
* bármely lejárati idő.

Példa a fenti csomópont függvény használata a következő lesz:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var deviceKey ="...";

    var token = generateSasToken(endpoint, deviceKey, null, 60);

Az összes funkciója device1 hozzáférést biztosít, eredménye a következő lesz:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697

> [AZURE.NOTE] Lehetséges a .NET eszközzel [Eszköz Explorer]biztonságos jogkivonat készítése[lnk-device-explorer].

### <a name="use-a-shared-access-policy"></a>Megosztott access-házirend használata

A megosztott hozzáférési házirend jogkivonat létrehozásakor a házirendet a név mező `skn` meg kell használni a szabály neve. Is, hogy a házirend engedélyt a **DeviceConnect** szükséges.

A megosztott hozzáférési eszköz funkciók eléréséhez két fő forgatókönyv a következők:

* [Protocol (protokoll) átjárók cloud][lnk-endpoints],
* [szolgáltatások token] [ lnk-custom-auth] egyéni hitelesítési sémák végrehajtásához használja.

Mivel a megosztott hozzáférési házirend is esetleg engedélyt szeretne csatlakozni bármelyik eszközről, érdemes biztonsági tokenek létrehozásakor, használja a megfelelő erőforrás URI. Ez különösen fontos jogkivonat szolgáltatásokhoz, amelyeket egy adott eszközt használ, az erőforrás URI a jogkivonat hatókör. Ez a pont értendő kisebb protokoll átjárók, azok már vannak közvetítő forgalom az összes eszközt.

Példaként az **eszköz** nevű előre létrehozott megosztott access-házirend használata jogkivonat-szolgáltatás jogkivonat volna létrehozását a következő paraméterek:

* erőforrás URI: `{IoT hub name}.azure-devices.net/devices/{device id}`,
* aláírási kulcs: egy, a billentyűk a `device` házirendet,
* házirend neve: `device`,
* bármely lejárati idő.

Példa a fenti csomópont függvény használata a következő lesz:

    var endpoint ="myhub.azure-devices.net/devices/device1";
    var policyName = 'device';
    var policyKey = '...';

    var token = generateSasToken(endpoint, policyKey, policyName, 60);

Az összes funkciója device1 hozzáférést biztosít, eredménye a következő lesz:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device

Protocol (protokoll) az átjárók felhasználhatók azonos az összes eszközt, egyszerűen beállítása az erőforrás URI való `myhub.azure-devices.net/devices`.

### <a name="use-security-tokens-from-service-components"></a>A szolgáltatás összetevőit biztonsági tokenek használata

Szolgáltatás-összetevők csak hozhat létre biztonsági tokenek átengedése házirendekkel megadása a megfelelő engedélyekkel, korábban című cikkben ismertetett módon.

A szolgáltatás függvényeit végpontokon elérhetővé tett a következők:

| Végpont | Funkciók |
| ----- | ----------- |
| `{iot hub host name}/devices` | Létrehozása, módosítása, beolvasásához és eszköz identitás törlése. |
| `{iot hub host name}/messages/events` | Eszköz a felhőbe üzeneteket fogadni. |
| `{iot hub host name}/servicebound/feedback` | Felhőalapú-eszköz üzenetek visszajelzéseket. |
| `{iot hub host name}/devicebound` | Felhőalapú-eszköz üzenetek küldése. |

Példaként szolgáltatás létrehozása az **registryRead** nevű előre létrehozott megosztott access-házirend használata hozna létre jogkivonat, a következő paraméterek:

* erőforrás URI: `{IoT hub name}.azure-devices.net/devices`,
* aláírási kulcs: egy, a billentyűk a `registryRead` házirendet,
* házirend neve: `registryRead`,
* bármely lejárati idő.

    var végpont = "myhub.azure-devices.net/devices";   var házirendnév = "eszköz";   var policyKey = "...";

    var jogkivonat = generateSasToken (végpontot, policyKey, házirendnév, 60);

Az eredmények szeretne hozzáférést biztosítani olvassa el az eszköz identitások, a következő lesz:

    SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead

## <a name="supported-x509-certificates"></a>Támogatott X.509-tanúsítványok

Egy eszközt hitelesíteni IoT központi bármely x.509-es tanúsítvány is használhatja. Ide tartoznak:

-   **Egy meglévő X.509-tanúsítványt**. Egy eszközt előfordulhat, hogy már van társítva X.509 tanúsítvány. Az eszköz a tanúsítvány használatával IoT központi hitelesíteni.

-   **X-509 önálló létrehozott és önaláírt tanúsítványt**. A számítógépgyártó vagy házon belüli deployer tanúsítványok készíthet, és a megfelelő titkos kulcs (és a tanúsítvány) tárolásához az eszközön. Használható eszközök, például [OpenSSL] [ lnk-openssl] és a [Windows SelfSignedCertificate] [ lnk-selfsigned] segédprogram erre a célra.

-   **Hitelesítésszolgáltató alá x.509-es tanúsítvány**. Hozza létre, és úgy hitelesítésszolgáltató (CA) jelentkezett X.509 tanúsítvány használatával azonosítása egy eszközt, és a hitelesítési IoT központi egy eszközt.

Egy eszközt vagy használhat egy X.509-tanúsítványt vagy a biztonsági jogkivonat hitelesítés, de nem mindkét.

### <a name="register-an-x509-client-certificate-for-a-device"></a>Regisztráljon az eszköz X.509 ügyfél tanúsítvány

Az [Azure IoT szolgáltatás SDK C#] [ lnk-service-sdk] (verzió 1.0.8+) támogatja a Regisztrálás egy eszközt használó ügyfél X.509 tanúsítvány hitelesítéshez. Más API-khoz, például az importálás/exportálás eszközök támogatják X.509 ügyfél tanúsítványok is.

### <a name="c-support"></a>C\# támogatás

A **RegistryManager** osztály programozott megoldást regisztrálhatja egy eszközt. Különösen az **AddDeviceAsync** és **UpdateDeviceAsync** módszerek lehetővé teszi, a felhasználó regisztrálhat, és egy központi Iot eszköz identitás beállításjegyzékbeli eszköz frissítése. E két módszer, hogy egy **eszköz** példány bemeneteként. Az **eszköz** osztály- **hitelesítés** tulajdonság, amely lehetővé teszi, hogy a felhasználó megadhatja az elsődleges és másodlagos x.509-es tanúsítvány thumbprints tartalmazza. Az ujjlenyomatot jelöl egy SHA-1 alapján a X.509-tanúsítvány (tárolja a bináris DER kódolással). Felhasználók megadásával egy elsődleges ujjlenyomat és egy másodlagos ujjlenyomat vagy lehetősége van. Az elsődleges és másodlagos thumbprints annak érdekében, hogy kezelje a tanúsítvány átváltási esetek támogatottak.

> [AZURE.NOTE] IoT központi nem szükséges, vagy a teljes X.509 ügyfél tanúsítványt, csak az ujjlenyomatot tárolni.

Íme egy példa C\# kódtöredék regisztrálhatja egy eszközt X.509-ügyfél tanúsítvány használata:

```
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "921BC9694ADEB8929D4F7FE4B9A3A6DE58B0790B"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-client-certificate-during-runtime-operations"></a>Ügyfél X.509 tanúsítvány használata futtatókörnyezet műveletek során

Az [Azure IoT eszköz a .NET rendszerhez SDK] [ lnk-client-sdk] (verzió 1.0.11+) X.509 ügyfél tanúsítványok használatát támogatja.

### <a name="c-support"></a>C\# támogatás

Az osztály **DeviceAuthenticationWithX509Certificate** X.509 ügyfél tanúsítvány használatával  **DeviceClient** példányok létrehozását támogatja.

Íme egy példa kódtöredék:

```
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-authentication"></a>Egyéni eszköz hitelesítés

Használható [eszköz identitás beállításjegyzék] IoT központi[ lnk-identity-registry] beállítása eszköz hitelesítő adatokat, és a hozzáférés-vezérlés [tokenek]használata[lnk-sas-tokens]. Azonban egy IoT megoldás már van egy jelentős befektetés rendszerben egy egyéni eszköz identitás beállításjegyzék és/vagy a hitelesítés, ha integrálhatja a meglévő infrastruktúra a IoT központi: *jogkivonat-szolgáltatás*hozzon létre. Ezzel a módszerrel más IoT funkciók a a megoldás is használhatja.

Jogkivonat-szolgáltatás egy egyéni felhőszolgáltatásba. Az IoT központi *megosztott hozzáférési házirend* azt használja **DeviceConnect** engedélyekkel rendelkező *eszköz – munkafüzetszintű* tokenek létrehozásához. Tokenek engedélyezése a IoT központi csatlakozhat egy eszközt.

  ![A lépéseket a jogkivonat-szolgáltatás minta][img-tokenservice]

Jogkivonat-szolgáltatás minta fő lépéseit a következők:

1. Hozzon létre egy megosztott IoT központi access házirendet a IoT központi **DeviceConnect** engedélyeinek. Ezzel a házirenddel az [Azure portál] hozhat létre[ lnk-management-portal] vagy programozás útján. Jogkivonat-szolgáltatás használja ezt a házirendet a létrehozott tokenek bejelentkezni.
2. Amikor egy eszközt a IoT központi eléréséhez szükséges, a jogkivonat-szolgáltatás aláírt jogkivonat kér. Az eszköz a egyéni eszköz identitás-beállításjegyzék/hitelesítési séma határozza meg az eszköz identitás a token szolgáltatás által használt hozhat létre a token hitelesítheti.
3. Jogkivonat-szolgáltatás jogkivonat adja eredményül. A token használatával hozzák létre `/devices/{deviceId}` mint `resourceURI`, a `deviceId` mint hitelesíteni kívánt eszközt. Jogkivonat-szolgáltatás a megosztott hozzáférési házirend létrehozása a token használja.
4. Az eszközön használja a token közvetlenül az IoT-központban.

> [AZURE.NOTE] A .NET osztály [SharedAccessSignatureBuilder] használható[ lnk-dotnet-sas] , illetve a Java osztály [IotHubServiceSasToken] [ lnk-java-sas] jogkivonat létrehozása a token szolgáltatásban.

Jogkivonat-szolgáltatás állíthat be a token lejárati tetszés szerint. Ha lejár a jogkivonat, a IoT központi folyamat eszközt. Ezután az eszköz egy új token kell kérniük jogkivonat-szolgáltatás. Egy rövid lejárati idő használata esetén ez növeli a terhelést a jogkivonat-szolgáltatás és az eszközhöz.

A központi csatlakozhat egy eszközhöz hozzá kell adnia továbbra is azt a IoT központi eszköz identitás beállításjegyzék – annak ellenére, hogy az eszközön használja a jogkivonat, és nem egy eszköz kulcs való csatlakozáshoz. Ezért továbbra is eszköz hozzáférési használatával engedélyezése vagy letiltása az eszköz identitások [IoT központi identitás beállításjegyzék] [ lnk-identity-registry] Ha az eszköz jogkivonat hitelesíti. Ez megszünteti a kockázatok hosszú lejárati idő a tokenek használja.

### <a name="comparison-with-a-custom-gateway"></a>Egyéni az átjárók összehasonlítása

A jogkivonat-szolgáltatás mintája egy egyéni identitás beállításjegyzék/hitelesítési mód végrehajtásához IoT központi az ajánlott módszereket. Ajánlott, mert IoT központi továbbra is a megoldást forgalmat a legtöbb kezeli. Vannak azonban olyan esetek, ahol az egyéni hitelesítési mód van így egymásba kapcsolódik, hogy szükség-e a forgalom (*egyéni átjáró*) feldolgozása szolgáltatás protokollal. Példa erre az [átviteli réteg Security (TLS) és a előre megosztott kulcsok (PSKs)][lnk-tls-psk]. További tudnivalókért lásd: az [átjáró protokoll] [ lnk-protocols] témakört.

## <a name="reference-topics"></a>Hivatkozás témakörök:

A következő hivatkozást témakörök nyújtanak további információt a IoT központi való hozzáférés szabályozása.

## <a name="iot-hub-permissions"></a>IoT központi engedélyek

Az alábbi táblázat a is használhatja a IoT központi való hozzáférés szabályozása engedélyekkel.

| Engedély            | Jegyzetek |
| --------------------- | ----- |
| **RegistryRead**      | Támogatás olvassa el a eszköz identitás beállításjegyzék elérését. További tudnivalókért lásd: az [eszköz identitás beállításjegyzék][lnk-identity-registry]. |
| **RegistryReadWrite** | Támogatás írási és olvasási hozzáférést a eszköz identitás beállításjegyzék. További tudnivalókért lásd: az [eszköz identitás beállításjegyzék][lnk-identity-registry]. |
| **ServiceConnect**    | A hozzáférési cloud szolgáltatás elérésű kommunikáció és figyelése a végpontok. Ha például azt engedélyt háttéradatbázist felhőszolgáltatások eszköz a felhőbe üzeneteket fogadni, felhőalapú-eszköz küldésére, valamint a megfelelő kézbesítési visszaigazolások. |
| **DeviceConnect**     | Eszköz elérésű kommunikációs végpontok való hozzáférést biztosít. Ha például azt engedélyt cloud-eszköz üzenetek eszköz a felhőbe üzenetek küldése és fogadása. Ezzel az engedéllyel eszközöket használja. |

## <a name="additional-reference-material"></a>További anyagminta

A Fejlesztőeszközök útmutató hivatkozás témakörök a következők:

- [IoT központi végpontok] [ lnk-endpoints] ismerteti a különböző végpontok, amely az egyes IoT központi közzététele futtatókörnyezet és kezelés műveletekhez.
- [Szabályozási és kvóták] [ lnk-quotas] a IoT központi szolgáltatás és számíthat, amikor a szolgáltatás használatához a szabályozási viselkedés vonatkozó kvóták ismerteti.
- [IoT központi eszközök és szolgáltatások SDK] [ lnk-sdks] a különböző nyelvi SDK sorolja fel, egy eszköz és a szolgáltatás-alkalmazások használata a IoT központi fejlesztésekor használja.
- [Lekérdezési nyelv twins, módszerek és feladatok] [ lnk-query] ismerteti a lekérdezésnyelv IoT központi eszköz twins, módszerek és feladatok kapcsolatos adatok kinyerése is használhatja.
- [IoT központi MQTT támogatási] [ lnk-devguide-mqtt] IoT központi támogatási további információt tartalmaz a MQTT protokollhoz.

## <a name="next-steps"></a>Következő lépések

Most már rendelkezik megtanulta hozzáférés IoT központban vezérléséhez hogyan, amelyek érdekelhetik a fejlesztői útmutató a következő témakörök:

- [Állapot és a beállítások szinkronizálása az eszköz twins használatával][lnk-devguide-device-twins]
- [Közvetlen metódus eszközön][lnk-devguide-directmethods]
- [A feladatok ütemezése több eszközön][lnk-devguide-jobs]

Ha azt szeretné, próbálja meg néhány a jelen cikkben ismertetett fogalmak, amelyek érdekelhetik az alábbi IoT központi oktatóanyag:

- [Azure IoT központi – első lépések][lnk-getstarted-tutorial]
- [Hogyan IoT központi cloud-eszköz üzenet küldése][lnk-c2d-tutorial]
- [Hogyan IoT központi eszköz felhő folyamat során][lnk-d2c-tutorial]

<!-- links and images -->

[img-tokenservice]: ./media/iot-hub-devguide-security/tokenservice.png
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-openssl]: https://www.openssl.org/
[lnk-selfsigned]: https://technet.microsoft.com/library/hh848633

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-sas-tokens]: iot-hub-devguide-security.md#security-tokens
[lnk-amqp]: https://www.amqp.org/
[lnk-azure-resource-manager]: ../azure-resource-manager/resource-group-overview.md
[lnk-cbs]: https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc
[lnk-event-hubs-publisher-policy]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab
[lnk-management-portal]: https://portal.azure.com
[lnk-sasl-plain]: http://tools.ietf.org/html/rfc4616
[lnk-identity-registry]: iot-hub-devguide-identity-registry.md
[lnk-dotnet-sas]: https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx
[lnk-java-sas]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/com/microsoft/azure/iot/service/auth/IotHubServiceSasToken.html
[lnk-tls-psk]: https://tools.ietf.org/html/rfc4279
[lnk-protocols]: iot-hub-protocol-gateway.md
[lnk-custom-auth]: iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: iot-hub-devguide-security.md#supported-x509-certificates
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-service-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/service
[lnk-client-sdk]: https://github.com/Azure/azure-iot-sdks/tree/master/csharp/device
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdks/blob/master/tools/DeviceExplorer/doc/how_to_use_device_explorer.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
