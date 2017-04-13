<properties
    pageTitle="404-es állapot visszaadó Azure CDN-végpontok hibaelhárítása |} Microsoft Azure"
    description="Azure CDN-végpontok 404-es válasz kódok hibák elhárítása"
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Hibaelhárítás 404-es állapotok visszatérés CDN-végpontok

Ez a cikk segítséget nyújt a problémák elhárítása [CDN-végpontok](cdn-create-new-endpoint.md) 404 hibát visszaadó.

Ha ez a cikk bármely pontján további segítségre van szüksége, kapcsolatba léphet [az MSDN Azure](https://azure.microsoft.com/support/forums/)és a túlcsordulás Papírhalom fórumok az Azure szakértőktől. Másik lehetőségként az Azure támogatási kérelmeiket is küldhet. Nyissa meg a [Azure támogatási webhelyén](https://azure.microsoft.com/support/options/) , és a **Get-támogatást**gombra.

## <a name="symptom"></a>A jelenség

CDN-profil és a zárólap létrehozta, de a tartalom nem tűnnek a CDN elérhető.  A felhasználók, akik próbálnak hozzáférni a tartalomhoz a CDN URL-CÍMEN keresztül HTTP 404-es állapot kódok kap. 

## <a name="cause"></a>OK

Nincs több oka lehet, például:

- A fájl származási nem látható a CDN-re
- Az endpoint nincs megfelelően beállítva, a CDN-t a megfelelő helyen meg okoz
- A host a host fejléc, az a CDN van elutasítása
- A végpont nem volt idő a CDN egész terjesztése

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

> [AZURE.IMPORTANT] Miután létrehozott egy CDN-végpontot, nem azonnal lesz használható, mint a szülőtől keresztül a CDN regisztráció időt vesz igénybe.  <b>Azure CDN Akamai a</b> profilok propagálása általában egy percen belül egészíti ki.  <b>Azure CDN Verizon a</b> profilok propagálása 90 percen belül általában befejeződik, de bizonyos esetekben több időt vesz igénybe.  Ha, hajtsa végre a dokumentumban, és továbbra is 404-es válasz érkezik, fontolja meg, ellenőrizze ismét megnyitása előtt végezze el a támogatási jegyek néhány órát várakozás.

### <a name="check-the-origin-file"></a>Jelölje be az origin fájl

Először is azt kell ellenőrizze az, hogy a szeretnénk gyorsítótárazott fájl érhető el az origin és nyilvánosan elérhető.  Ennek leggyorsabb módja, nyissa meg a böngészőben a saját vagy Incognito munkamenetet, és keresse meg közvetlenül a fájlt.  Csak írja be vagy illessze be az URL-címet a cím mezőbe, és látható, ha, amely a fájlban a várt eredményeket.  Ebben a példában fogom használni egy fájlt, hogy van-e Azure tároló fiókkal, a könnyen kezelhető `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Amint látható, a vizsgálat sikeresen továbbítja.

![A siker!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Noha a leggyorsabban és legegyszerűbben úgy bizonyosodjon meg arról fájlját nyilvánosan elérhető, a szervezet bizonyos hálózati konfigurációk sikerült ad a finomabb, hogy a fájl esetén nyilvánosan elérhető kerül, sőt, csak látható a felhasználók számára a hálózat (még akkor is, ha azt üzemelteti Azure-ban).  Egy külső böngészőt, amelyből tesztelheti, például a mobileszközön használja, amely nem csatlakozik a szervezeti hálózatba, vagy egy virtuális gép az Azure-van, akkor legjobb.

### <a name="check-the-origin-settings"></a>Az origin beállításainak ellenőrzése

Most, hogy a nyilvánosan elérhető az interneten lévő fájl végzett, akkor győződjön meg arról az origin beállításait.  Az [Azure-portálon](https://portal.azure.com)keresse meg a CDN-profilját, és a végpontot, használja a Hibaelhárítás gombra.  Az eredményül kapott **végpont** lap kattintson az origin.  

![Végpont lap a kiemelt origin](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Az **Origin** lap jelenik meg. 

![Origin lap](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Origin típusát, és a hostname (állomásnév)

Ellenőrizze, hogy az **Origin típus** helyességét, valamint az **Origin hostname (állomásnév)**.  A példában `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, az URL-címet a hostname (állomásnév) része `cdndocdemo.blob.core.windows.net`.  A képernyőképen látható, mint ez a helyes-e.  Azure tárolására, a Web App és a Felhőbeli szolgáltatástól forrásokból a **származási hostname (állomásnév)** mező a legördülő menü, így azt nem kell aggódnia amiatt, hogy helyesen a helyesírás.  Azonban egy egyéni origin használ, érdemes *teljesen kritikus* helyesen írta be az állomásnév!

#### <a name="http-and-https-ports"></a>HTTP- és HTTPS-portok

A más dolog Itt a **HTTP** - és **HTTPS-portok**.  A legtöbb esetben 80 és 443-as is helyes, és a módosítások nem lesz szüksége.  Azonban a forráskiszolgálóval a különböző port figyel, ha, amely kell itt képviseli.  Ha nem biztos benne, keresse a origin fájl URL-címen.  A http- és HTTPS jellemzői 80-as és 443-as adja meg az alapértelmezett beállításokat. Az URL-cím `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, olyan portot nincs megadva, az alapértelmezett 443-as tekinti, és a beállítások helyességéről.  

Azonban mondja ki az URL-címet, a származási fájl korábbi tesztelt, nem `http://www.contoso.com:8080/file.txt`.  Megjegyzés: a `:8080` a hostname (állomásnév) szakasz végén.  Ezzel azt jelzi, hogy portot használja a böngésző `8080` való csatlakozáshoz a webes kiszolgálón `www.contoso.com`, így a **HTTP-port** mezőbe írja be a 8080 kell.  Fontos tudni, hogy ezek portbeállításokat csak hatással a végpont használja az adatok kinyerése a origin port.

> [AZURE.NOTE] **Azure CDN Akamai a** végpontokat nem engedélyezik a teljes TCP porttartományt forrásokból.  Az origin portok nem áll rendelkezésre, című témakör [Akamai engedélyezett Origin portokat az Azure CDN-t](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Az endpoint beállításainak ellenőrzése

Vissza a lap a **végpontot** , a **Konfigurálás** gombra.

![A lap végpont beállítása gombja](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

A végpont **beállítása** lap jelenik meg.

![A lap konfigurálása](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokollok

**Protokollok**ellenőrizze, hogy az ügyfelek által használt protokollt, van-e jelölve.  Az ügyfél által használt ugyanazt a protokollt azzal az origin, ezért fontos, hogy az origin portokat megfelelően beállítva az előző szakaszban eléréséhez használt lesz.  Az alapértelmezett HTTP és HTTPS portokat (80 és 443-as), függetlenül az origin portokat csak figyel az végpontot.

Vegyük térjen vissza az kitalált példa `http://www.contoso.com:8080/file.txt`.  A emlékezni fog Contoso meghatározott `8080` , a HTTP port, de tegyük is fel őket a megadott `44300` , a HTTPS-port.  Ha az azok nevű zárólap `contoso`, azok CDN végpont hostname (állomásnév) lenne `contoso.azureedge.net`.  Egy kérelemre `http://contoso.azureedge.net/file.txt` HTTP felkérés van, így a végpont használna HTTP port 8080 a eredetű lekérdezni.  Egy HTTPS, biztonságos kérelmet `https://contoso.azureedge.net/file.txt`, a HTTPS használhatja a port 44300 végpont járhat amikor retriving az origin át a fájlt.

#### <a name="origin-host-header"></a>Origin host élőfej

Az **Origin host élőfej** értéke host fejléc minden kérésével kezdőpontjának küldött.  A legtöbb esetben meg kell ugyanaz, mint azt a korábbi ellenőrzött **Origin hostname (állomásnév)** .  Ebben a mezőben helytelen értéket általában nem okozó 404-es állapotok, de más 4xx állapotok, attól függően, hogy az origin vár eredményezi.

#### <a name="origin-path"></a>Origin elérési út

Végezetül ellenőriznie kell azt az **Origin elérési útját**.  Alapértelmezés szerint az üres.  Ez a mező csak ha azt szeretné, hogy a kívánt elérhetővé szeretné tenni a CDN origin által üzemeltetett erőforrásokra hatókörének szűkítésére kell használni.  

Például a végpontot, az e szeretett volna összes erőforrás tároló fiókom elérhetővé szeretné tenni, így e üresen **Origin elérési útját** .  Ez azt jelenti, hogy kérelmének `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` eredménye egy kapcsolatot a végpontot, `cdndocdemo.core.windows.net` kéri, hogy `/publicblob/lorem.txt`.  Hasonlóképpen, ha kér `https://cdndocdemo.azureedge.net/donotcache/status.png` eredménye az endpoint kér `/donotcache/status.png` el.

De mi a teendő, ha nem szeretném megjeleníteni a CDN használható minden elérési út az origin?  I. fel, hogy csak szeretett volna kattintva jelenítse meg a `publicblob` elérési útját.  Ha */publicblob* meg saját **származási elérési útja** mezőben, amelyek hatására a program a végpont beszúrása */publicblob* előtt minden kérelem az origin történik.  Ez azt jelenti, hogy a vonatkozó `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` most ténylegesen tart az URL-cím kérelem részét `/publicblob/lorem.txt`, és a Hozzáfűzés `/publicblob` elejéig. Ennek eredménye egy kérelemre `/publicblob/publicblob/lorem.txt` el.  Ha az elérési út nem oldja meg a tényleges fájlba, a kezdőpontjának 404-es állapotban ad vissza.  Ebben a példában lorem.txt beolvasásához helyes URL-címet ténylegesen lenne `https://cdndocdemo.azureedge.net/lorem.txt`.  Jegyzet, hogy azt nem */publicblob* elérési útjának feltüntetése, mert az URL-címet a kérelem része `/lorem.txt` , és hozzáadja a végpont `/publicblob`, az eredményül kapott `/publicblob/lorem.txt` a kérelem átadott az origin.
