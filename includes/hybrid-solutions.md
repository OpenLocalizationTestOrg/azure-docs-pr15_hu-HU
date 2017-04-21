#<a name="azure-service-bus"></a>Azure Service Bus

Az adott alkalmazás vagy szolgáltatás fut a felhőben, vagy a helyszíni, nem gyakran kell-e más alkalmazások és szolgáltatások vezérléséhez. Azure ad lehetőséget szélesebb körben hasznos ehhez, ajánlatok szolgáltatás Bus. Ez a cikk azt mutatja be, Mi ez, és miért célszerű használni a technológia figyelmébe vesz igénybe.

## <a name="service-bus-fundamentals"></a>Szolgáltatás Bus alapjai
Különböző helyzetekben kommunikációs különböző stílusú kérjen. Időnként előfordulhat hogy az alkalmazások között egy egyszerű várólista üzenetek küldése és fogadása a legjobb megoldás. Egyéb esetben egy szokásos várólista nem elég; a közzététel és előfizetés mechanizmusa rendelkező várólista célszerűbb. És bizonyos esetekben igazán szükséges az összes az alkalmazások és a #151 közötti kapcsolat, a sorok nem kötelező kitölteni. Szolgáltatás Bus lehetőségeket is biztosít az összes három, az alkalmazások számos különböző módon kapcsolatba engedélyezem.

Szolgáltatás Bus egy bérlői több felhőalapú szolgáltatásba, ami azt jelenti, hogy a szolgáltatás több felhasználó van-e osztva. Minden felhasználónak, például az alkalmazás fejlesztője *névtér*hoz létre, majd határozza meg, a kommunikációs mechanizmusok kikkel belül, hogy szüksége van. [1 ábrán](#Fig1) látható, hogy miként ez.

<a name="Fig1"></a>![Azure Service Bus ábrája][svc-bus]
 
**Ábra 1: Szolgáltatás Bus szolgáltatást biztosít, a több elem bérlői történő csatlakozás az alkalmazások között a felhőben.**

Belül névteret négy különböző kommunikációs szerkezetek, amelyek a applications eltérő módon csatlakozik egy vagy több példány is használhatja. A következő lehetőségek közül:

- *Sorok*, amelyek lehetővé teszik az egyik kétirányú kommunikációt. Minden sor úgy működik, mint közvetítő (más néven egy *ügynök*), amely tárolja az elküldött üzenetek mindaddig, amíg az érkező. Minden egyes az egyetlen címzett üzenet.
- *Témakörök*, amelyek *előfizetések*– egy súgótémakör használata egy kétirányú kommunikációt is több előfizetéssel rendelkezik. Várólista, például a témakör végpontként ügynök, de egyes előfizetések tetszés szerint használható szűrő csak az adott feltételeknek megfelelő üzenetek fogadására.
- *Jelfogók*, amelyek kétirányú kommunikációt. Sorok és a témakörök eltérően a továbbítási nem tárolni repülés üzenetek-it meg nem ügynök. Ehelyett csak továbbítja azokat a célalkalmazás alkalmazásba című témakör tartalmaz.
- *Esemény hubok*, amelyek a felhőbe tömeges méreteket, a bejövő Eseménynapló és telemetriai adatok kis késés és a magas megbízhatóság.

A várakozási sorban található, a témakör, továbbítási vagy esemény központi létrehozásakor, adhat nevet. Bármely, a névtér nevű együtt, ez a név létrehoz egy egyedi azonosítót az objektum. Alkalmazások biztosítása az ilyen nevű szolgáltatás Bus, majd a várólista, témakör, továbbítási vagy esemény központi használva kommunikálhat egymással. 

Az objektumok használatához a Windows-alkalmazások Windows Communication Foundation (WCF) használhatja. A sorok, a témakörök és a esemény hubok Windows alkalmazások szolgáltatás Bus definiált üzenetben API-khoz is használhatja. Az objektumok használatának megkönnyítése a Windows-alkalmazásokból, hogy a Microsoft Java, Node.js és más nyelvek SDK biztosít. Sorok, a témakörök és a REST API-k használata a HTTP-esemény hubok is elérheti. 

Fontos, ha meg szeretné érteni, hogy annak ellenére, hogy a szolgáltatás Bus magát futtatja a felhőben (Ez azt jelenti, hogy a Microsoft Azure adatközpont esetén), így azt futtatását is lehetővé teszi tetszőleges helyre. Csatlakozás Azure, például futó alkalmazások és a saját adatközponthoz belül fut szolgáltatás Bus is használhatja. Csatlakozás Azure vagy egy másik futó alkalmazás is használható a helyszíni alkalmazással vagy a telefonok és táblagépek felhő platform. Háztartási, érzékelőket és egyéb eszközök csatlakozni egy központi alkalmazás vagy egy másikat is lehetőség. Szolgáltatás Bus egy általános kommunikációt meglehetősen sok bárhonnan elérhető a felhőben. Hogyan használja függ az alkalmazások kell tennie.


## <a name="queues"></a>Sorok

Tegyük fel, hogy úgy dönt, hogy a két alkalmazás használ szolgáltatási Bus várólista csatlakozás. [Ábra 2](#Fig2) ezt a helyzetet mutatja be.

<a name="Fig2"></a>![A szolgáltatás Bus sorok diagram][queues]
 
**Ábra 2: A szolgáltatás Bus sorban várakozó szükséges egyirányú aszinkron queuing.**

A folyamat nem egyszerű: feladó szolgáltatás Bus várólista elküld egy üzenetet, és néhány későbbi időpontban ezt az üzenetet felveszi a címzett. Több alkalmazást az egy sorból olvashatja, vagy várólista is csak egyetlen címzett, [2 ábrán](#Fig2) látható módon. Ez utóbbi esetben minden üzenet egyetlen címzett értelmezése – egy több öntött szolgáltatás kell használnia a témakör helyette.

Minden üzenet két részből áll: a tulajdonságaik, minden kulcs/érték két, és a bináris üzenet törzsébe. Hogyan által használt attól függ, hogy mi az alkalmazások partnere kapcsolatba próbál tegye. Például az alkalmazás legutóbbi értékesítés kapcsolatban az üzenet elküldése az tulajdonságokat tartalmazhatnak olyan *Eladó "Ava" =* és *összeg = 10000*. Az üzenet törzsébe az értékesítés aláírt szerződés beolvasott képen tartalmazhat, vagy, ha nem, csak maradnak üres.

Egy vevő erről egy üzenetet a szolgáltatás Bus sorból kétféle módon. Az első lehetőséget, és nevű *ReceiveAndDelete*, üzenet eltávolítása a várakozási sorban található, és azonnal törli azt. Ez az egyszerű, de ha a címzett összeomlik, mielőtt fejeződik be az üzenet, az üzenet el fog veszni. Van eltávolítottuk a várólista, mert nincs más vevő érhető el. 

A második lehetőség *PeekLock*, ami segítheti a probléma megoldásához jelenti. ReceiveAndDelete, például az PeekLock olvasási üzenet eltávolítása a várakozási sorban található. Az üzenetet, de nem törli. Ehelyett zárolja az üzenetet, ami láthatatlan egyéb a címzett, majd megvárja, amíg egy három események:

- Ha a címzett sikeresen feldolgozásával az üzenetet, *kész*meghívja, és a várólista törli az üzenetet. 
- Ha a címzett úgy dönt, hogy azt nem lehet feldolgozni azokat az üzenet sikeresen, hívások *elhagyja*. A sorban a zárolás távolít el az üzenetet, majd a többi címzett számára elérhetővé teszi.
- Ha a címzett felhívja sem, az alábbiak konfigurálható időszakon belül (Ez alapértelmezés szerint 60 másodperc), a sor feltételezi, hogy a címzett sikertelen volt. Ebben az esetben azt úgy viselkednek, mint, ha a címzett volna nevű eseményértesítéshez, az üzenet elérhetővé tétele a többi címzett.

Figyelje meg, mi itt akkor fordulhat elő: ugyanezt a hibaüzenetet előfordulhat, hogy, kézbesítése kétszer, talán két különböző vevők. A szolgáltatás Bus sorban várakozó használó alkalmazásai kell készíteni. Az üzenet könnyebb ismétlődő észlelése, minden egyes üzenetnek egy egyedi MessageID tulajdonság, amely alapértelmezés szerint munkamennyiségének függetlenül attól, hogy hány alkalommal a sorból olvasható. 

Sorok igazán néhány esetekben hasznosak. Még ha mindkét nem fut egy időben, valamit, amely különösen hasznos a köteget, és a mobilalkalmazások kommunikáció alkalmazások segítségükkel. A több vevők várólista is tartalmaz, az automatikus terheléselosztás, mivel az elküldött üzenetek ilyen vevők között helyezkednek.


## <a name="topics"></a>Témakörök

Hasznos, mint a "sorok nem mindig" a megfelelő megoldást. Szolgáltatás Bus témakörök sokszor jobb. [A 3-as szám](#Fig3) a arról mutatja be.

<a name="Fig3"></a>![Szolgáltatás Bus témakörök és előfizetések ábrája][topics-subs]
 
**3. ábra: Egy előfizetés alkalmazás Itt adhatja meg a szűrő alapján, megkaphatja a némelyikét vagy mindegyikét a szolgáltatás Bus témakör küldött üzeneteket.**

A tematikus hasonlít várólista számos módon. Feladók küldés üzeneteket, hogy elküldése várólista üzeneteket, és azokat az üzeneteket ugyanúgy témakör ugyanaz, mint a sorok keresése. A nagy különbség, hogy témakörök engedélyezése minden fogadó alkalmazás hozzon létre saját előfizetés *szűrő*megadásával. Előfizető csak azokat az üzeneteket, amelyek megegyeznek a szűrő majd jelenik meg. [Ábra 3](#Fig3) például feladó és a tematikus három előfizetők, az egyes saját szűrővel jeleníti meg:

- 1 előfizetői fogadása csak a tulajdonság tartalmazó üzenetek *Eladó "Ava" =*.
- 2 előfizetői kap a tulajdonság tartalmazó üzenetek *Eladó "Fonetikus" =* , illetve az *összeg* tulajdonságot, amelynek értéke nagyobb, mint 100 000 tartalmaz. Fonetikus esetleg az értékesítési vezető, és ezért szeretne saját értékesítés és a teljes nagy értékesítés, függetlenül attól, ki készíti őket.
- 3 előfizetői nem állított be a szűrő *Igaz*, ami azt jelenti, hogy összes üzenetet kap. Például lehet, hogy az alkalmazás könyvvizsgálati fenntartása felelős, és ezért szükséges összes üzenet megjelenítéséhez.

Sorok, az előfizetői témakörben olvashatja ReceiveAndDelete vagy a PeekLock az üzenetek. Sorok, eltérően azonban témakörben egy üzenethez képes fogadni több előfizetők számára. Ezt a megközelítést gyakran nevű *Közzététel és előfizetés*, akkor lehet hasznos, amikor több alkalmazások ugyanazokat az üzeneteket az lehet, hogy érdekelheti. A jobb oldali szűrő megadásával minden előfizetői, válassza a be, hogy szüksége van az üzenet-adatfolyam részét.


## <a name="relays"></a>Jelfogók

Sorok és a témakörök nyújtanak egyirányú aszinkron kommunikációs ügynök keresztül. Egyetlen irányba forgalmat, és nincs közvetlen kapcsolat küldő és között. De mi a teendő, ha nem szeretné, hogy ez? Tegyük fel, hogy az alkalmazások kell egyaránt üzenetek küldése és fogadása, vagy esetleg közöttük közvetlen hivatkozást szeretne, és nem szükséges ügynök üzenetek tárolásához. A cím forgatókönyvek, például szolgáltatás Bus biztosít jelfogók, [4 ábrán](#Fig4) látható módon.

<a name="Fig4"></a>![Szolgáltatás Bus továbbítási ábrája][relay]
 
**Ábra 4: A szolgáltatás Bus továbbítási alkalmazás közötti szinkron, kétirányú kommunikációt biztosít.**

A egyértelmű kérdés jelfogók, és kérdezze meg: Miért előnyös az használata egy? Akkor is, ha a sorok nem szeretném, miért győződjön meg az alkalmazások keresztül egy felhőalapú szolgáltatásba, hanem csak közvetlenül kapcsolatba kommunikáció? A választ ki kell, hogy mit írt közvetlenül lehet nehezebb, mint gondolná.

Tegyük fel, hogy két a helyszíni környezetbe applications csatlakozni szeretne, mindkét verziót vállalati adatközpontokkal belül. Ezeket az alkalmazásokat, mindegyik tűzfal mögött helyezkedik el, és minden egyes adatközponthoz valószínűleg használja a hálózati címfordítást (hálózati Címfordítást). A tűzfal akadályozza néhány kivételével az összes portokra bejövő adatokat, és a hálózati címfordító Eszközig, az azt jelenti, hogy az egyes alkalmazás fut a számítógépen nincs-e egy rögzített érheti el közvetlenül a kívül az Adatközpont IP-cím. Anélkül, hogy néhány további segítségre van szüksége ezeket az alkalmazásokat az nyilvános interneten keresztül kapcsolódó okoz.

A szolgáltatás Bus továbbítási Ez segítséget nyújt. A továbbítási keresztüli kommunikáció a két irányban, mindegyik alkalmazásra a szolgáltatás Bus egy kimenő TCP-kapcsolatot létesít, majd továbbra is a megnyitott. A két alkalmazás között az összes kommunikációs haladási ezeket a kapcsolaton keresztül. Mivel minden kapcsolat létrehozása az az Adatközpont belül, a tűzfal lehetővé egyes alkalmazások bejövő forgalom új portok megnyitása nélkül. Ez a módszer is megkapja a hálózati Címfordítást a problémát, mivel minden alkalmazásnak meg egy következetes végpontot egész a kommunikáció a felhőben. Által a továbbítási keresztül adatcsere, az alkalmazások elkerülhető, egyébként tenné kommunikációs nehéz problémákat. 

Alkalmazások szolgáltatás Bus jelfogók használatához használja arra, a Windows Communication Foundation (WCF). Szolgáltatás Bus biztosít, amelyek a Windows-alkalmazások keresztül jelfogók interaktív egyértelmű legyen WCF kötések. WCF használó alkalmazások segítségével általában csak adja meg, hogy ezek a kötések egyikét, majd beszélhet a továbbítási keresztül. Sorok és a témakörök eltérően azonban-Windows alkalmazásokból, lehetséges, miközben jelfogók használatához néhány programozási munkamennyiség; nincs szabványos tárak találhatók.

Sorok és a témakörök eltérően alkalmazások nem kifejezetten hoz létre jelfogók. Ehelyett ha olyan alkalmazás, amely kívánja jelennek meg a szolgáltatás Bus TCP kapcsolatot létesít, a továbbítási automatikusan létrejön. A kapcsolat megszakad, amikor a program törli a továbbítási. Ahhoz, hogy egy alkalmazást, keresse meg a továbbítási egy adott figyelő által létrehozott, szolgáltatás Bus biztosít a beállításjegyzék, amely lehetővé teszi, hogy alkalmazásokat úgy találhatja meg egy adott továbbítási név szerint.

Jelfogók a megfelelő megoldást esetén szüksége közvetlen alkalmazás közötti kommunikációt. Például érdemes megvizsgálni egy légitársaságok foglalás rendszer futtatása egy helyszíni adatközpontban, amely a beadás helyek, a mobileszközök és a más számítógépek érhető el. E rendszerek futó alkalmazások bárhol az esetleg futó támaszkodhat szolgáltatás Bus jelfogók közli, hogy a felhőben.

## <a name="event-hubs"></a>Esemény hubok

Esemény hubok rendszer erősen méretezhető bevitel, amely is milliónyi másodpercenként események feldolgozása, a segítségével, így az alkalmazás folyamat és a nagy mennyiségű adatot a csatlakoztatott eszközök és alkalmazások által gyártott. Például egy esemény hubhoz segítségével élő motor teljesítménnyel kapcsolatos adatokat gyűjt a egy flotta autós. Esemény hubra gyűjtött, miután átalakítás pontra, és tárolja az adatokat bármely valós idejű analytics-szolgáltatója vagy a tárhely fürt segítségével. Esemény hubok kapcsolatos további tudnivalókért olvassa el a az [esemény hubok – áttekintés][]című témakört.

## <a name="summary"></a>Összefoglalás

Csatlakozás alkalmazások mindig volt teljes megoldások fejlesztésére részét, és a cellatartományt, hogy az alkalmazások és szolgáltatások egymással esetek növelje a további alkalmazások van állítva, és eszköz csatlakozik az internethez. Mert a felhőalapú technológiák eléréséhez a sorok, témák, jelfogók és esemény hubok keresztül, a szolgáltatás Bus célja, hogy a lényeges függvény, egyszerűbb és szélesebb körben elérhető.

[svc-bus]: ./media/hybrid-solutions/SvcBus_01_architecture.png
[queues]: ./media/hybrid-solutions/SvcBus_02_queues.png
[topics-subs]: ./media/hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[relay]: ./media/hybrid-solutions/SvcBus_04_relay.png
[Esemény hubok – áttekintés]: https://msdn.microsoft.com/library/azure/dn836025.aspx
