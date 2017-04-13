## <a name="about-records"></a>Tudnivalók a rekordok

Minden DNS-rekordot a név és típus tartalmaz. Rekordok különböző típusú adatokat tartalmaznak szerint vannak tagolva. A leggyakoribb típus is az "Egy" rekord, amely nevét hozzárendeli IPv4-cím. Más típusú egy "MX" rekord, amely levelezési kiszolgáló nevét.

Azure DNS támogatja az összes közös DNS-bejegyzés típusainak, beleértve A, AAAA, CNAME, MX, NS, PTR, SOA, SRV és TXT. Megjegyzés:
- SOA rekord beállítása az egyes zónák automatikusan létrehozott, hogy nem lehet létrehozni, külön-külön.
- A TXT-rekord típusa használatával SPF rekordokat kell létrehozni. További tudnivalókért lásd: [erre a lapra](http://tools.ietf.org/html/rfc7208#section-3.1).

Az Azure DNS-rekordok relatív nevük segítségével adható. Egy "teljesen minősített" tartománynevét (FQDN) a zóna nevét tartalmazza, mivel a "relatív" nevet nem. A relatív rekord nevét abban a kijelzőzónában "contoso.com" a "www" például a rekord teljesen minősített neve www.contoso.com adja vissza.

## <a name="about-record-sets"></a>Tudnivalók a rekord beállítása

Előfordul, hogy meg kell megadott név és típus egynél több DNS-rekord létrehozásához. Tegyük fel, hogy a "www.contoso.com" webhelyére a két különböző IP-címek üzemelteti. A webhely szükséges két különböző A rekordok, egyik, IP-címeket. Ez a példa egy rekordkészletben:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS kezeli a DNS-rekordjait a rekord beállítása. Egy rekordkészletben a DNS-rekordokat a zónán ugyanazt a nevet, és az azonos típusú gyűjteménye. A legtöbb rekord beállítása egyetlen rekordot tartalmazó, de ehhez hasonló, amelyben egy rekordkészletben tartalmaz egynél több rekord példák nem ritka.

SOA és a CNAME rekord beállítása a kivételek. A DNS-szabványokat nem teszi lehetővé az ilyen azonos nevű több rekordot.

Élettartam, vagy a TTL (élettartam), adja meg, mennyi ideig az egyes rekordok gyorsítótárazott ügyfelek újra lekérdezett előtt. Ebben a példában a TTL (élettartam), 3600 másodpercet vagy 1 órát. A TTL (élettartam) rekordkészletben nem minden rekordhoz tartozó van megadva, az azonos értékkel használják, hogy rekordkészletben belül minden rekordhoz.

#### <a name="wildcard-record-sets"></a>Helyettesítő rekord beállítása

Azure DNS [helyettesítő rekordok](https://en.wikipedia.org/wiki/Wildcard_DNS_record)támogatja. Ezek a visszaadott bármilyen lekérdezést egyező nevű (kivéve, ha egy nem helyettesítő rekordkészletben származó, amely közelebb van). Helyettesítő rekord beállítása az NS és SOA kivételével az összes bejegyzéstípusok támogatott.  

Hozzon létre egy helyettesítő rekordkészletben, használja az rekordkészletben nevet "\*". Kövesse a nevét a címkével ellátott "\*", például"\*.foo".

#### <a name="cname-record-sets"></a>CNAME rekord beállítása

CNAME rekord beállítása más rekord azonos nevű bíró nem használható. Például nem hozhatók létre egy CNAME rekordot a "www" relatív nevű beállítása és az A rekord "www" relatív nevű egy időben. Mivel a zóna csúcs (neve = ‘@’) mindig tartalmazni fogja az NS és SOA rekord beállítása a zóna létrehozásakor készített, nem hozható létre egy CNAME rekordot, a zóna csúcs értéken. Korlátozó az a DNS-szabványoknak felmerülő, és nem Azure DNS korlátai.
