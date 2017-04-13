<properties 
    pageTitle="Gyakori kérdések közvetítése |} Microsoft Azure"
    description="Azure továbbítási kapcsolatos néhány gyakori kérdésekre ad választ."
    services="service-bus"
    documentationCenter="na"
    authors="jtaubensee"
    manager=""
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub" />

# <a name="relay-faq"></a>Továbbítás – gyakori kérdések

Ez a cikk a Microsoft Azure továbbítási kapcsolatos gyakori kérdésekre ad választ. Látogasson el a az [Azure támogatja a gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure árak és támogatási információkat is. A következő témakörökben találhatók:

- [Általános kérdések a Azure-továbbító](#general-questions)
- [Árak](#pricing)
- [Kvóták](#quotas)
- [Előfizetés és névtér kezelése](#subscription-and-namespace-management)
- [Hibaelhárítás](#troubleshooting)

## <a name="general-questions"></a>Általános kérdések

### <a name="what-is-azure-relay"></a>Mi az Azure továbbítási?

A [továbbítási szolgáltatáshoz](relay-what-is-it.md) lehetővé teszi a átlátszó szolgáltató és a WCF-szolgáltatások és bárhonnan elérheti. Más szóval lehetővé teszi az Azure adatközponthoz és a helyszíni vállalati környezetben futtatott hibrid alkalmazásokat.

### <a name="what-is-a-relay-namespace"></a>Mi az továbbítási névteret?

[Névtér](Relay-create-namespace-portal.md) tartalmazó tároló biztosít megcímezheti továbbítási erőforrások az alkalmazáson belül. Létre tudja hozni használatához szükséges, és az első lépések az első lépések közül.

## <a name="pricing"></a>Árak

Ebben a szakaszban a struktúra árak továbbítási kapcsolatos gyakori kérdésekre ad választ. Látogasson el a Microsoft Azure általános árak információkat [Azure támogatási – gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083) is. A továbbítási árak, témakörben talál [szolgáltatás Bus árak részleteket](https://azure.microsoft.com/pricing/details/service-bus/).

### <a name="how-do-you-charge-for-relay"></a>Hogyan tegye meg díjat számít fel továbbítási?

Továbbítás árak teljes információkért olvassa el [szolgáltatás Bus árak részletek][Pricing overview]. Nemcsak a rögzített árakat az előfizetést terhelő társított adatátvitel az az adatközpont, amelyben az alkalmazás már kiépítve kívüli kilépési.

### <a name="what-usage-of-relay-is-subject-to-data-transfer"></a>Milyen továbbítási használatát az adatátvitel fizetnie?

Továbbítás 5 GB / hó előfizetésenként adatok bejövő-adatok tartalmazza. További Azure bejövő adatok/kilépési ingyenesek továbbítási által használt adatokat nem.

Az adatok díja: továbbítási az csak feladóktól érkező bejövő adatok, mint továbbítási hallgatók nem járnak adatok díjat. Ha például 1 GB küldi el, ha csak számlázott 1 GB, a annak ellenére, hogy egy figyelő is 1 GB kapott, és Azure-féle adatközpontokkal kívüli lehet.

### <a name="how-are-relay-hours-calculated"></a>Hogyan számítsa ki továbbítási óra?

Továbbítás óra is számlát kapni, összesített adott idő "alatt, amely minden továbbítási nyitva" egy adott számlázási időszak alatt. A továbbítás implicit módon megjelenítésre és a megadott névteret nyílt meg, amikor a továbbítási figyelő először csatlakozik a címére. A továbbítás be van zárva, csak akkor, ha az utolsó figyelő címének történő leválasztáshoz. Ezért számít, hogy a továbbítási "Megnyitás", az idő a számlázási célokra az első továbbítási figyelő csatlakozik, az idő, a legutóbbi továbbítási figyelő bontja a névtér. Ez azt jelenti számít, hogy a továbbítási nyissa meg, amikor egy vagy több továbbítási hallgatók szolgáltatás Bus címének kapcsolódik.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-given-relay"></a>Mi történik, ha egynél több figyelő egy adott továbbítási csatlakozik-e?

Egyes esetekben egy egyetlen továbbítási előfordulhat, hogy több csatlakoztatott hallgatók. A továbbítás tekintendő "nyissa meg a" Ha legalább egy továbbítási figyelő kapcsolódik hozzá. További hallgatók hozzáadása nyitott továbbító további továbbítási óra eredményezi. Továbbítás feladók (ügyfelek meghívása vagy üzeneteket küldeni jelfogók) számát csatlakozik a továbbítási is nem befolyásolja a számítás továbbító óra.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Hogyan számítható ki az üzenetek kijelző a WCF jelfogók?

**Ez csak a WCF jelfogók alkalmazandó pedig nem egy hibrid kapcsolatok költségeit**

Az általános számlázható üzenetek ugyanazzal a módszerrel brokered entitás (sorban várakozó, témák és előfizetések) a fentebb ismertetett jelfogók a kiszámított. Vannak azonban több főbb különbségek:

A továbbítás figyelő a kézbesítési követő küld egy szolgáltatás Bus továbbítási üzenetet kezelnek, a "teljes keresztül" elküldheti a továbbítási figyelő, amely a Küldés a szolgáltatás Bus továbbítási helyett az üzenetet kapja. Emiatt a kérés / válasz stílus szolgáltatás meghíváshoz (legfeljebb 64 KB) elleni továbbítási figyelő két számlázható üzenetek eredményezi: a kérelem és a válasz egy számlázható üzenet számlázható üzenetek (feltételezve, hogy a kérdésre adott választ egyben \<64 KB =). Ez különbözik ügyfél és a szolgáltatás közötti résidőkiosztással várólista használatával. Az utóbbi esetben az azonos kérés / válasz mintát egy kérelem küldése a sorba, követi dequeue/kézbesítési a sorból a szolgáltatás, a válasz küldése egy másik sorba, majd és dequeue/kézbesítési adott sorból az ügyfélnek kell rendelkeznie. Azonos (\<= 64 KB) egész feltételezések méretét, a által várólista mintázat így eredményezne négy számlázható az üzenetek kétszer az a szám, a továbbítási használata ugyanazon minta végrehajtásához számlát kapni. Természetesen előnyökkel sorban várakozó használatával a mintát, például tartóssági eléréséhez, és töltse be a simítás. Alábbi előnyökkel jár előfordulhat, hogy a további költség sorkizárása

A netTCPRelay WCF kötés segítségével megnyitott jelfogók kezeli az üzenetek nem egyedi üzenetekként, de a lépés, a rendszer keresztül adatok adatfolyamként. Más szóval csak a feladó és a figyelő van betekintést kap abba, hogy a kép az egyes témájának szegélyezésén üzenetek küldött és fogadott kötés használatával. Így jelfogók használata a netTCPRelay kötés, az összes adatot kell kezelni számlázható üzenetek kiszámításához adatfolyamként. Ebben az esetben szolgáltatás Bus kiszámítása adatok mennyiségét küldött vagy minden egyes továbbítási 5 perces alapon keresztül érkezett és a nullával való osztás, hogy a teljes 64 KB annak érdekében, hogy az adott időszakon belül a szóban forgó továbbítási megállapításához számlázható üzenetek számát.

## <a name="quotas"></a>Kvóták

|Kvóta neve|Hatókör|Típus|Vezérlő viselkedése, amikor túllépve|Érték|
|---|---|---|---|---|
|Kattintson a továbbítás egyidejű hallgatók|Személy|Statikus|Ezután valaki további kapcsolatok elutasításra kerül, és kivételt fogadja a hívó kóddal.|25|
|Egyidejű továbbítási hallgatók|Rendszerbeli|Statikus|Ezután valaki további kapcsolatok elutasításra kerül, és kivételt fogadja a hívó kóddal.|2000|
|Egy szolgáltatás névtér az összes továbbítási végponton egyidejű kapcsolatok|Rendszerbeli|Statikus|-|5000|
|Egy szolgáltatás névtere továbbítási végpontok|Rendszerbeli|Statikus|-|10 000|
|Üzenet mérete [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) és [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) továbbítja.|Rendszerbeli|Statikus|Bejövő üzeneteket, amelyekre az alábbi kvóták elutasításra kerül, és kivételt fogadja a hívó kóddal.|64KB
|Üzenet mérete [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) és [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) továbbítja.|Rendszerbeli|Statikus|-|Korlátlan|

### <a name="does-relay-have-any-usage-quotas"></a>Rendelkezik a továbbítás a bármilyen használati kvótájának?

Alapértelmezés szerint minden felhő szolgáltatás Microsoft állítja be, hogy mindig összes ügyfél előfizetések egy összesített havi használati kvótája. Mivel azt megértéséhez, hogy szüksége lehet több, mint ezek a korlátok, kérjük, forduljon az ügyfélszolgálat bármikor, hogy azt igényeinek megfelelően megértéséhez, és ezek a korlátok megfelelően módosítsa. A szolgáltatás Bus a összesítő használati kvótájának a következők:

- 5 milliárd üzenetek
- 2 millió továbbítási óra

Azt lefoglalása letiltása egy adott hónapban a használati kvótájának túllépte ügyfélfiók jobbra, amíg azt fogja adja meg az e-mailben értesítést, és végezze el az ügyfél bármely megtétele előtt több próbálkozás. Ezek a kvóták meghaladó ügyfelek továbbra is díjakat, amelyekre a kvótákat felelős.

#### <a name="naming-restrictions"></a>Elnevezési korlátozások

A továbbítás névtér neve között 6-50 karakter hosszúságú állhat.

## <a name="subscription-and-namespace-management"></a>Előfizetés és névtér kezelése

### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Hogyan tudom áttelepíteni a névtér egy másik Azure-előfizetését?

PowerShell-parancsok használatához (a cikkben található [Itt](../service-bus-messaging/service-bus-powershell-how-to-provision.md#migrate-a-namespace-to-another-azure-subscription)) szeretne váltani névteret egy Azure előfizetés. A művelet végrehajtása, hogy a névtér már aktívnak kell lennie. A felhasználó a parancsok végrehajtása is a forrás- és célwebhelyek előfizetések rendszergazdának kell lennie.

## <a name="troubleshooting"></a>Hibaelhárítás

### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-their-suggested-actions"></a>Melyek a Azure továbbítási API-hoz és a javasolt műveleteket által létrehozott kivételekkel?

A [Továbbítás kivételek] [ Relay exceptions] a cikk ismerteti a javasolt műveleteket kivétel.

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Mi az, hogy megosztott Access aláírás és nyelveket támogatja az aláírás létrehozása?

Megosztott Access aláírások SHA – 256 biztonságos tiltva vagy URL-címe alapján hitelesítési módszer. A saját aláírás az csomópontot, PHP, Java és C szeretne információt\#, olvassa el az [Access aláírások megosztott][] .

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Megosztott Access aláírások]: service-bus-sas-overview.md

## <a name="next-steps"></a>A következő lépéseket:

- [Névtér létrehozása](relay-create-namespace-portal.md)
- [Első lépések a .NET rendszerhez](relay-hybrid-connections-dotnet-get-started.md)
- [Csomópont – első lépések](relay-hybrid-connections-node-get-started.md)