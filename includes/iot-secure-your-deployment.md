# <a name="securing-your-iot-deployment"></a>A IoT-telepítés biztonságossá tétele

Ebben a cikkben a következő szint az Azure IoT-alapú Internet a dolog (IoT) infrastruktúra biztosítása érdekében. Szintű részleteket minden egyes összetevő telepítése és konfigurálása csatol. Összehasonlítás és a különböző versengő módszerek közötti választási lehetőséget is biztosít.

Az Azure IoT-telepítés biztonságossá tétele a következő három biztonsági területeket lehet osztani:

- **Biztonsági eszköz**: IoT eszköz biztosítása, miközben a vadon élő telepítik.
- **Kapcsolat biztonsága**: IoT eszköz és IoT Hub közötti adatátvitelt biztosító rendszer bizalmas és hamisíthatatlan.
- **Felhő biztonsági**: adatok védelmében, miközben halad át, és tárolja a felhő eszközöket biztosítva.

![Három biztonsági területek][img-overview]

## <a name="secure-device-provisioning-and-authentication"></a>Biztonságos eszköz kiépítése és a hitelesítés

Az Azure IoT Suite IoT eszközök titkosítja az alábbi két módszer szerint:

- Azáltal, hogy egy egyedi azonosító kulcsot (biztonsági tokenek) az egyes eszközök, amelyek segítségével az eszköz által a IoT Hub kommunikálni.

- A-eszköz segítségével [X.509 tanúsítvány] [ lnk-x509] és a személyes kulcs hitelesíteni az eszközt a IoT hub eszközeként. Ez a hitelesítési módszer biztosítja, hogy az eszköz a személyes kulcs nem ismert az eszköz kívül bármikor magasabb szintű biztonságot nyújt.

A biztonsági jogkivonat módszer az eszköz által végrehajtott IoT hubhoz rendeli a szimmetrikus kulcsot minden hívás minden hívás hitelesítést biztosít. X.509-alapú hitelesítés lehetővé teszi, hogy egy IoT eszköz hitelesítési TLS kapcsolat létrehozásának részeként a fizikai réteg szintjén. A biztonsági jogkivonat-alapú módszer használható az X.509 hitelesítés, amely kevésbé biztonságos minta nélkül. A két módszer közötti választást elsősorban határozza meg hogyan biztonságos eszköz hitelesítési kell lennie, és biztonságos tárolását (biztonságosan tárolhatja a személyes kulcs) az eszközön rendelkezésre állásáról.

## <a name="iot-hub-security-tokens"></a>Biztonsági jogkivonatokat IoT Hub

IoT Hub eszközöket és szolgáltatásokat lehet, ne küldjön kulcsokat a hálózat hitelesítésére biztonsági jogkivonatokat használ. Ezenkívül biztonsági jogkivonatokat érvényességi idejét és hatályát korlátozott. Borzas IoT Hub SDK anélkül, hogy semmilyen speciális beállítást automatikusan hoz létre jogkivonatokat. Bizonyos esetekben azonban a felhasználó létrehozása és használata a biztonsági jogkivonatokat közvetlenül szükséges. Ezek közé tartozik a MQTT, AMQP vagy HTTP-felületek közvetlen felhasználásra, vagy a biztonságijogkivonat-szolgáltatás minta végrehajtásának.

További részleteket a biztonsági jogkivonat és annak használati szerkezetének a következő cikkekben található:

-   [Biztonsági token szerkezete][lnk-security-tokens]
-   [Eszközként SAS tokenek használata][lnk-sas-tokens]

Minden IoT van egy [Eszköz azonosító rendszerleíró] [ lnk-identity-registry] , amely a a szolgáltatásban olyan várólista, amely tartalmazza a repülés felhő-eszköz üzenetek, például eszköz erőforrások létrehozása és az eszköz néző végpont eléréséhez használható. A IoT Hub azonosító rendszerleíró azonosító eszköz és megoldás a biztonsági kulcsok biztonságos tárolását biztosítja. Személy vagy eszköz identitások csoportokhoz lehet hozzáadni az Engedélyezett partnereim listán, vagy a tiltólista lehetővé tevő eszköz hozzáférés vezérlését. A következő cikkek további részleteket ad az eszköz azonosító rendszerleíró adatbázis szerkezete és a támogatott műveletek.

[IoT Hub MQTT, AMQP és a HTTP protokollt támogat][lnk-protocols]. Biztonsági jogkivonatokat az IoT eszközről IoT hubhoz eltérően egyes ezeket a protokollokat használja:

- AMQP: SASL egyszerű és AMQP jogcímalapú biztonsági ({policyName}@sas.root.{iothubName} esetében a központi szintű tokenek; {deviceId} tokenek hatókörű eszköz esetén).

- MQTT: Csatlakozás csomagok használ {deviceId} {ClientId} {IoThubhostname} / {deviceId} a **felhasználónév** és a **jelszó** mezőbe egy SAS token.

- HTTP: Érvénytelen token az engedélyezési kérelem fejlécében.

IoT Hub eszközt azonosító rendszerleíró használható eszköz biztonsági hitelesítő adatok beállítása és hozzáférés-vezérlés. Azonban ha egy IoT megoldás már jelentős beruházásokat [egyéni eszköz azonosító rendszerleíró adatbázist és a hitelesítési séma]-[lnk-custom-auth], azt is be kell építeni IoT hubbal meglévő infrastruktúrába biztonságijogkivonat-szolgáltatás létrehozásával.

### <a name="x509-certificate-based-device-authentication"></a>X.509 tanúsítvány alapú eszköz hitelesítése

[Eszköz alapú X.509 tanúsítvány] használata[ lnk-use-x509] és annak társított nyilvános és személyes kulcspár segítségével további hitelesítés a fizikai réteg szintjén. A személyes kulcs az eszköz biztonságosan tárolja, és nem kívül az eszköz felderíthető. Az X.509 tanúsítvány, például az Eszközazonosítót, az eszköz és egyéb szervezeti adatait tartalmazza. A tanúsítvány aláírása a személyes kulcs segítségével jön létre.

A kiépítési folyamat magas szintű eszköz:

- Tartozó fizikai eszköz – eszköz azonosságát és/vagy az eszköz gyártási vagy üzembe helyezés során az eszközhöz tartozó X.509 tanúsítvány azonosító társításához.

- Hozzon létre egy megfelelő azonosító IoT Hub-eszköz azonosságát és társított eszköz információt a rendszerleíró adatbázisban IoT Hub eszközt.

- X.509 tanúsítvány ujjlenyomata biztonságosan IoT Hub-eszköz rendszerleíró adatbázis tárolja.

### <a name="root-certificate-on-device"></a>Főtanúsítvány eszközön

Kapcsolatlétesítés közben biztonságos TLS IoT hubbal, a IoT eszköz IoT Hub az eszköz SDK csomag részét képező legfelső szintű tanúsítvány használatával hitelesíti. A tanúsítvány a C ügyfél SDK mappája alatt található "\\c\\tanúsítványok" alatt a repó gyökere. Ellenére, hogy ezek főtanúsítványok hosszú élettartamú, azok továbbra is lejárnak, vagy vissza kell vonni. Ha nem tudja a tanúsítványt az eszköz frissítése, az eszköz nem lehet csatlakozni később a IoT Hub (vagy bármely más felhő szolgáltatás). A főtanúsítvány frissítése után a IoT eszköz telepítve van egy azt jelenti hatékonyan csökkentheti ennek kockázatát.

## <a name="securing-the-connection"></a>A kapcsolat biztonságossá tétele

Az eszköz IoT és IoT Hub közötti internetkapcsolat a Transport Layer Security (TLS) szabvány védett. Borzas IoT támogatja a [TLS 1.2-es][lnk-tls12], TLS 1.1 és a TLS 1.0, ebben a sorrendben. A TLS 1.0 támogatja-e a csak a visszamenőleges kompatibilitás érdekében. Ajánlott a TLS 1.2 használni, mert ez nyújtja a legnagyobb biztonságot.

Borzas IoT Suite ebben a sorrendben következő titkosító csomag támogatja.

| Rejtjelező csomag | Hossza |
|--------------|--------|
| TLS\_ECDHE\_az RSA\_WITH\_AES\_256\_CBC\_SHA384 ECDH secp384r1 (0xc028) (eq. FS 7680 bit RSA) | 256    |
| TLS\_ECDHE\_az RSA\_WITH\_AES\_128\_CBC\_SHA256 (0xc027) ECDH secp256r1 (eq. FS 3072 bites RSA) | 128    |
| TLS\_ECDHE\_az RSA\_WITH\_AES\_256\_CBC\_SHA (0xc014) ECDH secp384r1 (eq. FS 7680 bit RSA)           | 256    |
| TLS\_ECDHE\_az RSA\_WITH\_AES\_128\_CBC\_SHA (0xc013) ECDH secp256r1 (eq. FS 3072 bites RSA)           | 128    |
| TLS\_az RSA\_WITH\_AES\_256\_GCM\_SHA384 (0x9d) | 256    |
| TLS\_az RSA\_WITH\_AES\_128\_GCM\_SHA256 (0x9c) | 128    |
| TLS\_az RSA\_WITH\_AES\_256\_CBC\_SHA256 (0x3d) | 256    |
| TLS\_az RSA\_WITH\_AES\_128\_CBC\_SHA256 (0x3c) | 128    |
| TLS\_az RSA\_WITH\_AES\_256\_CBC\_SHA (0x35 hiba)    | 256    |
| TLS\_az RSA\_WITH\_AES\_128\_CBC\_SHA (0x2f)    | 128    |
| TLS\_az RSA\_WITH\_3DES\_EDE\_CBC\_SHA (0xa)    | 112    |

## <a name="securing-the-cloud"></a>A felhő biztonságossá tétele

Borzas IoT Hub [hozzáférési szabályok] meghatározása lehetővé teszi, hogy[ lnk-protocols] az összes biztonsági kulcshoz. A következő engedélyek csoportja hozzáférést minden IoT Hub végpontokat használ. Engedélyekkel korlátozza a hozzáférést funkciók alapján IoT hubhoz.

- **RegistryRead**. Az eszköz azonosító rendszerleíró olvasási hozzáférést biztosít. További tudnivalókért lásd az [Eszköz azonosító rendszerleíró][lnk-identity-registry].

- **RegistryReadWrite**. A támogatások írási és olvasási hozzáférési eszköz rendszerleíró azonosító. További tudnivalókért lásd az [Eszköz azonosító rendszerleíró][lnk-identity-registry].

- **ServiceConnect**. Hozzáférési szolgáltatás néző kommunikációs és megfigyelési végpontokon a felhő. Például hogy engedélyt háttér-felhő szolgáltatások eszköz felhő üzenetek fogadására, felhő-eszközre küldéshez és beolvasni a megfelelő kézbesítési visszaigazolások.

- **DeviceConnect**. Eszköz néző kommunikációs végpont engedélyezi a hozzáférést. Például hogy engedélyt felhő-eszköz üzenetek eszköz felhő üzenetek küldésére és fogadására. Ez az engedély eszközöket használják.

A [biztonsági jogkivonatok]IoT hubbal **DeviceConnect** engedélyek beszerzése kétféleképpen[lnk-sas-tokens]: eszköz azonosító kulcs vagy egy megosztott hozzáférési házirend kulcs használatával. Emellett fontos megjegyezni, hogy az összes funkció elérhető eszközök szándékosan végponton előtaggal van téve `/devices/{deviceId}`.

[Szolgáltatás összetevőit csak hoz létre biztonsági jogkivonatokat] [ lnk-service-tokens] a megfelelő engedélyek megadásának házirendjei megosztott használata.

Borzas IoT Hub és egyéb szolgáltatások, amelyeket a megoldás részét képezheti az Azure az Active Directory segítségével a felhasználók kezelésének engedélyezése.

Adatok szerint Azure IoT Hub lenyelve Azure adatfolyam Analytics és Azure blob-tároló szolgáltatások széles skáláját által igénybe vehető. Ezek a szolgáltatások lehetővé teszik a hozzáférés. Tudjon meg többet a e szolgáltatások és a rendelkezésre álló beállítások az alábbi:

- [Borzas DocumentDB][lnk-docdb]: olyan méretezhető, teljes mértékben indexelt adatbázis-szolgáltatást, amely kezeli a metaadatok az eszközök félig strukturált adatok, kiépítése, attribútumok, konfigurációs és biztonsági tulajdonságait. DocumentDB nagy teljesítményű és nagy átviteli feldolgozás, a séma-felismerése nélkül indexelési adatok és a gazdag SQL lekérdezési felületet kínál.

- [Borzas adatfolyam Analytics][lnk-asa]: a felhőben, amely lehetővé teszi, hogy gyorsan fejleszthetnek és vezethetnek be egy alacsony költségű analytics megoldás felfedezhető eszközök, érzékelők, infrastruktúra és alkalmazások valós idejű elképzelések feldolgozása valós idejű adatfolyam. Ez a szolgáltatás teljes mértékben kezelt adatait kötet továbbra is a nagy áteresztőképességű, kis késésű és rugalmasságát elérése mellett képes alkalmazkodni.

- [Azure App szolgáltatások][lnk-appservices]: A felhő platform készíthet hatékony webes és mobil alkalmazások olyan adatok bárhol; a felhő vagy helyszíni. Build iOS, Android és Windows mobile apps végzése. A szoftver mint szolgáltatás (szoftver) és a felhő alapú szolgáltatások több tucat out-of-the-box kapcsolatot a vállalati alkalmazások és a vállalati alkalmazások integrálása. Kód a kedvenc nyelv és IDE (.NET, NodeJS, PHP, Python vagy Java) web Apps alkalmazások és API-k készítéséhez gyorsabb, mint valaha.

- [Logika Apps][lnk-logicapps]: A logika Apps szolgáltatás Azure App szolgáltatás segítségével integrálhatja a IoT megoldást a meglévő sor üzleti rendszerek és munkafolyamatok automatizálása. Logika Apps lehetőséget nyújt a fejlesztőknek eseményindítóból indítása és lépések sorozata hajthat végre tervezési munkafolyamatok – szabályok és intézkedések, amelyeket az üzleti folyamatok integrálása hatékony összekötők segítségével. Logika Apps szoftver, felhő alapú egy hatalmas ökológiai out-of-the-box kapcsolatot biztosít és helyszíni alkalmazások.

- [Azure blob-tároló][lnk-blob]: megbízható, gazdaságos felhő tárolási az adatok küldése az eszközöket a felhő.

## <a name="conclusion"></a>Következtetés

Ez a cikk áttekintést végrehajtása tervezéséhez és üzembe helyezéséhez olyan IoT infrastruktúra használatával Azure IoT szintű részleteket. Beállítása az egyes alkatrészek biztonságos, mint a teljes IoT infrastruktúra biztosítása a kulcs. Azure IoT Tervező választható nyújtanak bizonyos fokú rugalmasságot és választási lehetőség; Minden választási lehetőséget is vannak biztonsági vonatkozásai. Ajánlott, hogy minden ilyen választás lehet értékelni a kockázatok/költségek becslésére.

[img-overview]: media/iot-secure-your-deployment/overview.png

[lnk-security-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#security-token-structure
[lnk-sas-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#use-sas-tokens-as-a-device
[lnk-identity-registry]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-protocols]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-custom-auth]: ../articles/iot-hub/iot-hub-devguide-security.md#custom-device-authentication
[lnk-x509]: http://www.itu.int/rec/T-REC-X.509-201210-I/en
[lnk-use-x509]: ../articles/iot-hub/iot-hub-devguide-security.md
[lnk-tls12]: https://tools.ietf.org/html/rfc5246
[lnk-service-tokens]: ../articles/iot-hub/iot-hub-devguide-security.md#using-security-tokens-from-service-components
[lnk-docdb]: https://azure.microsoft.com/services/documentdb/
[lnk-asa]: https://azure.microsoft.com/services/stream-analytics/
[lnk-appservices]: https://azure.microsoft.com/services/app-service/
[lnk-logicapps]: https://azure.microsoft.com/services/app-service/logic/
[lnk-blob]: https://azure.microsoft.com/services/storage/
