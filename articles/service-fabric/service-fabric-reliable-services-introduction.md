<properties
   pageTitle="A szolgáltatás háló megbízható programozási modell áttekintése |} Microsoft Azure"
   description="Tudnivalók a szolgáltatás háló megbízható szolgáltatás programozási modell, és kezdheti el a saját szolgáltatások írása."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor="vturecek; mani-ramaswamy"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/25/2016"
   ms.author="masnider;vturecek"/>

# <a name="reliable-services-overview"></a>Megbízható szolgáltatások – áttekintés
Azure Service háló leegyszerűsíti írása és állapot nélküli és állapot-nyilvántartó megbízható szolgáltatások kezelése. Ezt a dokumentumot az előadás:

- A megbízható szolgáltatások programozási modell állapot nélküli és az állapot-nyilvántartó szolgáltatáshoz.
- A választási lehetőségek, hogy egy megbízható Service írásakor van.
- Egyes esetek és mikor érdemes megbízható szolgáltatásokat, és hogyan írt példákat.

Megbízható szolgáltatások egyike, a szolgáltatás háló programozási modellek. További információt a megbízható szereplők programozási modell című témakörben [szolgáltatás háló megbízható szereplők](service-fabric-reliable-actors-introduction.md).

Szolgáltatás háló szolgáltatás konfigurálása alkalmazás kódot, tevődik össze, és adja meg (nem kötelező).

Szolgáltatás háló a szolgáltatást, élettartam kezeli a kiépítési és telepítési keresztül frissítési és törlési [alkalmazáskezelési szolgáltatás háló](service-fabric-deploy-remove-applications.md)keresztül.

## <a name="what-are-reliable-services"></a>Mik a megbízható szolgáltatásokat?
Megbízható szolgáltatásokat biztosít egy egyszerű, sokoldalú, legfelső szintű programozási modell segít express, hogy mi fontos, hogy az alkalmazás. A megbízható szolgáltatások programozási modell kap:

- Az állapot-nyilvántartó szolgáltatások a megbízható szolgáltatások programozási modell lehetővé teszi, hogy egységes, és biztos, hogy az állapot jobbra belül a szolgáltatásban tárolhatja megbízható gyűjtemény használatával. Ez egy olyan egyszerű könnyen hozzáférhető webhelycsoport osztályok, amely már jól ismert bárki megtekintheti, aki használta C# gyűjtemények lesz. Régebben szolgáltatások megbízható állam management szükséges külső rendszerek. Megbízható gyűjtemények, a tárolhatja a állapotát a számítási melletti azonos magas elérhetőségének és a nagyon elérhető külső áruház jár számíthat, megbízhatóságának és az további késés fejlesztésekkel közös megkeresése a számítási és az állapot megadása.

- Egy egyszerű modell, amely a saját kód futtatásához néz ki vannak használt modellek programozási. A kód egy jól meghatározott belépési pontjához és könnyen felügyelt életciklus tartalmaz.

- Beépíthető kommunikációs modell. A választási lehetőségek, például a [Webes API](service-fabric-reliable-services-communication-webapi.md), WebSockets, egyéni TCP-protokollok, például HTTP szállítása használja. Megbízható szolgáltatásokat nyújtanak néhány nagyszerű használhatja ki-be a beállítások, illetve megadhatja saját.

## <a name="what-makes-reliable-services-different"></a>Miből megbízható szolgáltatások különböző?
A szolgáltatás háló megbízható Services különbözik előtt előfordulhat, hogy írt szolgáltatások. Szolgáltatás háló megbízhatóság, elérhetőségét, egységesebb és méretezhetőség biztosít.  

- **Megbízhatósága**– a szolgáltatás még akkor is, ha a gép nem vagy hálózati hibák találati mellékletei nem megbízható környezetekben lesz látható.

- **Elérhetőség**– a szolgáltatás lesz elérhető, és válaszol. (Ez nem jelenti azt, hogy nem rendelkezik, amely nem található, és az elérhető szolgáltatások kívüli.)

- **Méretezhetőség**– szolgáltatások vannak leválasztott az adott hardver, és a nagyobb vagy kisebb keresztül hozzáadása vagy eltávolítása a hardver vagy a virtuális erőforrásokat szükség szerint. Szolgáltatások vannak egyszerűen formázva (különösen az állapot-nyilvántartó eset) annak érdekében, hogy a szolgáltatás független részeire méretezni, és válaszoljon rá hibák egymástól függetlenül. Végül a szolgáltatás háló szolgáltatások könnyű lehetővé tevő szolgáltatásokat egyetlen folyamat belül kell építenie ezer, hanem igénylő vagy teljes OS példányok hogy egy adott terhelést egyetlen példányára való javasolja.

- **Egységesebb**– Ez a szolgáltatás tárolt információkat készítheti el biztosítani szeretné konzisztens (Ez a cikk csak az állapot-nyilvántartó szolgáltatások – további információ újabb)

## <a name="service-lifecycle"></a>Szolgáltatás életciklus
Hogy a szolgáltatás, a rendszer állapot-nyilvántartó vagy állapot nélküli megbízható biztosítanak egy egyszerű életciklus, amellyel gyorsan dugja be a kódot, és vágjon bele.  Módon csak egy vagy két felfelé és fut a szolgáltatás megszerezni végrehajtásához szükséges.

- **CreateServiceReplicaListeners/CreateServiceInstanceListeners** – Ez az határozza meg, ahol a szolgáltatást a kapcsolati Papírhalom, amely használni szeretné. A kapcsolati Papírhalom, például a [Webes API -val](service-fabric-reliable-services-communication-webapi.md), akkor a figyelő végpont vagy a szolgáltatás (hogyan ügyfelek eléri akkor) végpontok határozza meg. Azt is határozza meg, hogyan be a szolgáltatáskód a többi személlyel vége megjelenő üzeneteket.

- **RunAsync** – Ez a, ahol a szolgáltatás fut, az üzleti logikai funkcióinak. A lemondás jogkivonat biztosított egy jel mikor kell szüntetni a használható. Például ha folyamatosan húzza ki egy megbízható várólista üzeneteket, és feldolgozni azokat kell, hogy a szolgáltatás, az adott használható volna fordulhat elő.

### <a name="service-startup"></a>A szolgáltatás indítása

A fő eseményeinek egy megbízható szolgáltatás a életciklusáról a következők:

1. A szolgáltatás objektum (a származik, az állapot nélküli vagy állapot-nyilvántartó szolgáltatást dolog) összeállítás.

2. A `CreateServiceReplicaListeners` / `CreateServiceInstanceListeners` módszer neve, biztosítva a szolgáltatást a névjegyadatok saját maga által kiválasztott egy vagy több kommunikációs hallgatók vissza.
  - Figyelje meg, hogy ez nem kötelező, bár a legtöbb szolgáltatás közvetlenül megmutatják néhány végpontot.

3. A kapcsolati hallgatók létrehozása után a van megnyitva.
  - Kapcsolati hallgatók nevű módszer van `OpenAsync()`, ezen a ponton nevezik, és a szolgáltatás listening címét adja meg, amelyek. A megbízható szolgáltatásban a beépített ICommunicationListeners egyikét használja, ha ez az Ön kezeli.

4. Miután a kapcsolati figyelő meg nyitva, a `RunAsync()` módszer a fő szolgáltatás neve.
  - Megjegyzendő, hogy `RunAsync()` nem kötelező. Ha a szolgáltatás nem az összes munkáját közvetlenül felhasználói válaszul a hívások csak, nincs szükség az, végrehajtásához `RunAsync()`.

### <a name="service-shutdown"></a>Szolgáltatás leállítása

Ha a szolgáltatás leállítása folyamatban van (a törlendő, korszerűsített vagy áthelyezett) a hívás sorrendben tükörképként jelenik meg: először a lemondás jogkivonat birtokában `RunAsync()` megszakad; Ezután `CloseAsync()` a kapcsolati hallgatók a nevezik.

Létezik néhány fontos megjegyzés kapcsolatban az állapot-nyilvántartó szolgáltatások leállítása:

- Szolgáltatás háló nem elősegíti az elsődleges állapot addig, amíg a szolgáltatás replikává `CloseAsync` és `RunAsync` visszaküldött. Ha egy beépített kommunikációs figyelő használja a `CloseAsync` módszer az Ön kezeli.

- Noha nincs időkorlát meg ezeket a módszereket visszaadása, azonnal elveszíti a az azt jelenti, hogy megbízható gyűjtemények írni, és ezért nem sikerül elvégezni bármely tényleges munka. Vissza a lemondás kérelem fogadásakor lehető leggyorsabban ajánlott.

## <a name="example-services"></a>Példa szolgáltatások
A programozási modell ismeretében nézzük egy rövid két különböző szolgáltatások tekintheti meg, hogyan darabot illeszkedjenek egymáshoz.

### <a name="stateless-reliable-services"></a>Állapot nélküli megbízható szolgáltatások
Állapot nélküli szolgáltatás egy where betűhíven nem nincs karbantartása belül a szolgáltatás állapota, vagy az államot, a jelen van teljes mértékben rendelkezésre álló, és használatához nincs szükség a szinkronizálást, replikációs, adatmegőrzési vagy magas elérhetősége.

Például érdemes megvizsgálni a Számológép nincs memória és kifejezések és a műveletek elvégzéséhez egyszerre kap.

Ebben az esetben a szolgáltatás a RunAsync() üres is lehet, mert nincs háttér tevékenység-feldolgozási elvégzendő végezze el a szolgáltatás nem. A Számológép szolgáltatás létrehozásakor egy megnyílik egy bizonyos port listening végpontot felfelé ICommunicationListener (például a [Webes API -val](service-fabric-reliable-services-communication-webapi.md)) ad vissza. A listening végpont fog bekötése különböző módszerek (például: "Hozzáadása (n1, n2)"), amely meghatározása a Számológép nyilvános API.

Amikor egy ügyfél irányuló hívás, a megfelelő módszert meghívott, és a Számológép szolgáltatás a megadott adatok műveleteket hajt végre, és az eredményt adja vissza. Hogy nem tárolja bármely állam.

Minden belső állapot nem tárolja teszi rendkívül egyszerű Ez a példa Számológép. De a legtöbb szolgáltatás nem igazán állapot nélküli. Ehelyett néhány egyéb áruház állapotukban externalize őket. (Például minden olyan biztonsági áruházban vagy a gyorsítótár munkamenet-állapot megőrzési támaszkodik web App alkalmazásban nem teljesen állapot nélküli.)

A közös példa hogyan állapot nélküli szolgáltatásokat használja a szolgáltatás háló megegyezik egy előtér, amely a nyilvános elérésű API webalkalmazáshoz közzététele. Az előtér-szolgáltatás majd fejezze be a felhasználó kérésére állapot-nyilvántartó szolgáltatások folytatott beszélgetést. Ebben az esetben ügyfelektől érkező hívások ismert port, például a 80, amennyiben az állapot nélküli szolgáltatás figyel irányítja. Ez a állapot nélküli szolgáltatás fogadja a hívást, és meghatározza, hogy a hívás megbízható ügyfélből, valamint melyik szolgáltatás azt szánt.  Ezután az állapot nélküli szolgáltatás továbbítja a hívást a állapot-nyilvántartó szolgáltatás helyességét, és választ vár. Az állapot nélküli szolgáltatás választ kap, amikor válaszol vissza az eredeti ügyfél számára.

### <a name="stateful-reliable-services"></a>Állapot-nyilvántartó megbízható szolgáltatások
Állapot-nyilvántartó szolgáltatás egy bizonyos része következetes és annak érdekében, hogy a szolgáltatás jelenlegi tartani függvény állam kell rendelkeznie. Fontolja meg egy szolgáltatás kiszámító folyamatosan valamilyen érték alapján a frissítések kap egy közbeni átlaga. Ehhez ezt a folyamatot, valamint az aktuális átlagos szükséges, bejövő felkérést aktuális készlete kell rendelkeznie. Bármely szolgáltatás, amely beolvassa, folyamatok és tárolja az adatokat (például egy Azure blob- vagy táblázat áruház ma) külső tárban állapot-nyilvántartó. A külső állam áruházban állapotába csak követi.

Legtöbb szolgáltatásához állapotukban külső felekkel, ma tárolni, mivel a külső tároló mi nyújt a megbízhatóságot, elérhetőségét, méretezhetőség és konzisztencia állam. A szolgáltatás háló állapot-nyilvántartó szolgáltatások nem tárolásához szükséges állapotukban külső felekkel; Szolgáltatás háló gondoskodik ezekről a követelményekről a szolgáltatáskód és a szolgáltatás állapotát.

Tegyük fel, azt szeretné, hogy mi kell végrehajtani a képet, és a kép alakítható kell, hogy a dokumentumkonvertálás válaszolhatók kérések szolgáltatás írása.  Ehhez a szolgáltatáshoz meghaladná kommunikációs figyelő (tegyük fel, hogy webes API-val) be a kapcsolati portot megnyílik, és lehetővé teszi, hogy a beküldött elemek keresztül egy API-t, mint `ConvertImage(Image i, IList<Conversion> conversions)`. Az Ez az API a szolgáltatás képes a információkat venni és tárolni a kérést egy megbízható várakozási sorban található, és néhány jogkivonat majd térjen vissza az ügyfél, azt is nyomon követheti a a kérelem (mivel a kérések eltarthat egy kis időt).

Ez a szolgáltatás összetettebb lehet RunAsync. A szolgáltatás lehet belül a RunAsync gyűjti össze a IReliableQueue-összehívások, a felsorolt Dokumentumkonvertálás hajt végre, és tárolja az eredmények IReliableDictionary, hurkot, hogy amikor az ügyfél vissza, a konvertált kép elérheti. Annak biztosítására, hogy akkor is, ha a kép nem sikerült, valamit, amit nem vesznek el, a megbízható szolgáltatás szeretné húzza ki a sorban, végezze el a dokumentumkonvertálás, az eredmény tárolását tranzakció. Ebben az esetben az üzenetet csak a sorból ténylegesen törlődik, és az eredményeket az eredmény szótár tárolja a dokumentumkonvertálás teljes esetén. Ha valamit, amit nem sikerül a középső (például a számítógép, a kód példány be van kapcsolva), a kérelem marad, a sorban várakozó dolgozható fel újra.

Ezzel a szolgáltatással kapcsolatos megjegyzés egyvalamihez, például a szokásos .NET szolgáltatás hangok. Az egyetlen különbség, hogy az aktív formázás adatszerkezetek (IReliableQueue és IReliableDictionary) szolgáltatás háló által nyújtott, és így történik megbízható, érhető el, és egységes.

## <a name="when-to-use-reliable-services-apis"></a>Mikor érdemes használni a megbízható szolgáltatások API-hoz
Ha az alábbiak bármelyikét szabadságfokának az alkalmazás szolgáltatásokra van szüksége, majd vegye fontolóra megbízható szolgáltatások API-hoz:

- Meg kell adnia az alkalmazás működésének több egységek (például megrendelések és elemek rendezése sor) szerinti keresztül.

- Az alkalmazás állapota természetesen lehet modellezni megbízható szótárak és a sorok.

- Az állapot kell lennie a kis késés hozzáféréssel nagyon érhető el.

- Az alkalmazás kell szabályozhatja a párhuzamos vagy Granularitás feldolgozott műveletek át egy vagy több megbízható lekérdezésére.

- A kommunikáció kezelése vagy szabályozhatja a particionálási sémát a szolgáltatásbeli kívánt.

- A kód egy ingyenes összefűzött futtatókörnyezet szükséges.

- Az alkalmazás dinamikusan létrehozása vagy megbízható szótárak vagy sorban várakozó destroy futásidőben kell.

- Kell programozás útján szabályozhatja szolgáltatás háló által biztosított biztonsági mentése és visszaállítása a szolgáltatás állapota * funkcióinak.

- A alkalmazást kell a módosítási előzmények karbantartása az állapot * jegyeit.

- Kidolgozása vagy harmadik fél fejlesztett, egyéni állapotban szolgáltatók * felhasználni kívánt.

> [AZURE.NOTE] * A SDK általános elérhetősége elérhető szolgáltatások.


## <a name="next-steps"></a>Következő lépések
+ [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
+ [Speciális használatát megbízható szolgáltatások](service-fabric-reliable-services-advanced-usage.md)
+ [A megbízható szereplők programozási modell](service-fabric-reliable-actors-introduction.md)
