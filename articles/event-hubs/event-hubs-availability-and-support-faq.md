<properties 
    pageTitle="Esemény hubok gyakran ismételt kérdések |} Microsoft Azure"
    description="Esemény hubok – gyakori kérdések."
    services="event-hubs"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="09/01/2016"
    ms.author="sethm" />

# <a name="event-hubs-faq"></a>Esemény hubok – gyakori kérdések

Esemény hubok nagyméretű bevitelt adatmegőrzési és nagy átviteli adatforrásból származó adatok események feldolgozása, illetve eszközök milliónyi biztosít. Párosítva szolgáltatás Bus sorban várakozó és témaköröket, esemény hubok lehetővé teszi, hogy állandó parancs és a vezérlőelem-alapú telepítés esetek [Dolog Internet (IoT)](https://azure.microsoft.com/services/iot-hub/) .

Ez a cikk azt ismerteti, hogy a árak információkat, és esemény hubok kapcsolatos gyakori kérdésekre ad választ:

## <a name="pricing-information"></a>Árinformációkat

Esemény hubok árak teljes információt az [esemény hubok árak részletek](https://azure.microsoft.com/pricing/details/event-hubs/)megtekintése

## <a name="how-are-event-hubs-ingress-events-calculated"></a>Hogyan számítsa ki esemény hubok bejövő adatok események?

Egyes események esemény-hubon keresztül csatlakozott küldött üzenet számlázható számít. Egy *bejövő adatok esemény* definíciója időegység-adatok legfeljebb 64 KB. Bármelyik eseményre, amely kisebb vagy egyenlő méretű 64 KB számít egy számlázható eseményt. Ha az esemény 64 KB-nál nagyobb, számlázható események száma szerint az esemény mérete 64 KB Többszörösök számítja ki. Például az esemény-központban küldött 8 KB esemény terheli egy eseményt, de az esemény-központban küldött üzenet 96 KB két esemény terheli.

Egy esemény központból felhasznált események csakúgy, mint az adatkezelési műveletek és pontjainak például hívások kezelése, számlázható bejövő adatok események nem számít, de felfelé az átviteli egység engedmény felmerülés.

## <a name="what-are-event-hubs-throughput-units"></a>Mik azok a esemény hubok átviteli egység?

Választania esemény hubok átviteli mennyiség, az Azure-portálra vagy esemény hubok erőforrás manager sablonokat. Átviteli egységek alkalmazása egy esemény hubok névtér az összes esemény hubra, és minden egyes átviteli egység feljogosítja a névtér kattintva az alábbi funkciókat:

- Felfelé / szekundum bejövő adatok események (esemény-elosztóhoz küldött események), de nem 1000-nél több bejövő adatok események, adatkezelési műveletek vagy vezérlőelem 1 MB API hívások másodpercenként.

- Legfeljebb 2 MB másodpercenként kilépési események (események egy esemény központból elfogyasztott mennyiség).

- Esemény tárolási (az alapértelmezett 24 órás az adatmegőrzési időszak elegendő) 84 GB.

Esemény hubok átviteli egységek vannak számlát kapni óránként, a megadott órában kijelölt egységek maximális száma alapján.

## <a name="how-are-event-hubs-throughput-unit-limits-enforced"></a>Hogyan vannak esemény hubok átviteli egység korlátozva?

Az összes bejövő adatok átviteli vagy az összes bejövő adatok esemény ráta keresztül névtér az összes esemény hubra mérete meghaladja az összesítő átviteli egység juttatások, feladók fog kell szabályozott, illetve fogadni, hogy a bejövő adatok túllépte jelző hibák.

A teljes kilépési átviteli vagy a teljes esemény kilépési ráta keresztül névtér az összes esemény hubra mérete meghaladja az összesítő átviteli egység juttatások, vevők szabályozott vannak, illetve fogadni, hogy a kilépési túllépte jelző hibák. Be- és kilépési kvóták akkor lép életbe külön-külön, hogy a feladó okozhatják esemény felhasználás csökkentheti, sem pedig a címzett megakadályozhatja, hogy események esemény-elosztóhoz küldött.

Ne feledje, hogy a átviteli egység kijelölés független esemény hubok partíciók száma. Miközben az összes partíciót a maximális sebesség 1 MB méretű második bejövő adatok (a legfeljebb 1000 események másodpercenként) és a második kilépési felhasználónként 2 MB kínál, nem a saját maguk partíciók rögzített ingyenesek. A költség nem egy esemény hubok névtér az összes esemény hubra az összesített átviteli egységet. Ezt a minta elég partíciót a várható maximális betöltés támogatása a rendszerek, mindaddig, amíg a rendszer az esemény terheltsége om, pedig nagyobb átviteli számok igényel, és szerkezetének és a rendszer architektúráját módosítása, a rendszer a betöltés nélkül növeli az átviteli egység díjak nélkül hozhat létre.

## <a name="is-there-a-limit-on-the-number-of-throughput-units-that-can-be-selected"></a>Van-e a kiválasztott átviteli egységek száma korlátozott?

Van egy alapértelmezett kvótáját 20 átviteli mennyiség / névtér. Kérheti egy átviteli egységek nagyobb kvóta által támogatási jegy tárolásához. A 20 átviteli egység korlát kötegeket kívül 20 és 100 átviteli egységek érhető el. Figyelje meg, hogy több, mint 20 átviteli egységek használatával eltávolítja az azt jelenti, hogy átviteli mennyiségek módosítása nélkül támogatási jegy tárolásához.

## <a name="is-there-a-charge-for-retaining-event-hubs-events-for-more-than-24-hours"></a>Van-e esemény hubok események megőrzése a 24 órán felüli időpontokat díjat?

Az esemény hubok szabványos réteg engedélyezése üzenet adatmegőrzési legfeljebb 30 nappal 24 óránál hosszabb Ha tárolt események száma mérete meghaladja a tárhely támogatás kijelölt átviteli mennyiségek (átviteli egységnyi 84 GB), a mérete meghaladja a támogatás alapdíjával díjazott a közzétett Azure Blob tárhelyet. A tárhely támogatás minden átviteli egységben összes tárolási költségek bemutatja az adatmegőrzési időszak 24 óra (alapértelmezett) akkor is, ha az átviteli egység maximális bejövő adatok támogatásra ki nem ürül.

## <a name="what-is-the-maximum-retention-period"></a>Mi az a maximális az adatmegőrzési időszak?

Esemény hubok szabványos réteg jelenleg támogatott maximális az adatmegőrzési időszak napján. Megjegyzés: az esemény hubok nem szánt egy állandó adatokat tároló. Az adatmegőrzési időszak nagyobb, mint 24 óra szolgálnak, amelyben célszerű ismétlődő lejátszásra van állítva egy esemény adatfolyam be az azonos rendszerek; felhasználási területei Ha például képzése, és ellenőrizze a meglévő adatok új gépi tanulási modell.

## <a name="how-is-the-event-hubs-storage-size-calculated-and-charged"></a>Hogyan esemény hubok tárhely méretét számított és fel?

Egész nap mérhető összes tárolt eseményeket, például a bármely belső terhelést esemény fejléce és struktúrákat a lemezen tároló összes esemény agyváltók, a teljes méretét. A napot végén tároló maximális mérete számítja ki. A napi tároló támogatás számítása során a (minden egyes átviteli egység biztosít 84 GB-támogatás) nap kiválasztásának átviteli egységek minimális száma alapján történik. Ha a teljes mérete meghaladja a számított napi tároló támogatás, a fölösleges tároló számlázva Azure Blob-tároló díjak használja ( **Helyben felesleges tároló** kamatlábbal).

## <a name="can-i-use-a-single-amqp-connection-to-send-and-receive-from-event-hubs-and-service-bus-queuestopics"></a>Használhatom-e egyetlen AMQP kapcsolat küldéséhez és fogadásához az esemény hubok és szolgáltatás Bus sorban várakozó/témaköröket?

Igen, feltéve, hogy ugyanazt a névteret az esemény hubok, sorok és témakörök van. Alkalmazhat a kétirányú brokered kapcsolatot sok eszközök, subsecond késések, például költséghatékony és erősen méretezhető módon.

## <a name="do-brokered-connection-charges-apply-to-event-hubs"></a>Végezze el brokered adatforgalmi költségek alkalmazása esemény hubok?

A feladók a kapcsolat tarifa szerint csak a AMQP protokoll használatakor. Vannak olyan nincs adatforgalmi költségek küldésének események HTTP, függetlenül attól, rendszerek és eszközök küldése számát használják. Ha AMQP (például elérése a hatékonyabb esemény adatfolyam- vagy kétirányú kommunikációt IoT parancs és a vezérlő helyzetekben engedélyezése) használni kívánja, olvassa el a [szolgáltatás Bus árinformációkat](https://azure.microsoft.com/pricing/details/service-bus/) lap mit jelent a brokered kapcsolatot, és hogyan forgalmi információt.

## <a name="what-is-the-difference-between-event-hubs-basic-and-standard-tiers"></a>Esemény hubok egyszerű és a szokásos rétegek közötti különbség

Esemény hubok szabványos réteg érhető el, az esemény hubok egyszerű és az egyes versenytársak rendszerek mértéket szolgáltatásokat nyújt. Ezek a funkciók a 24 órán felüli időpontokat és az azt jelenti, hogy egyetlen AMQP kapcsolat használata nagyszámú subsecond késések eszközökhöz parancsokat küldhet, valamint telemetriai küldhet e eszközökről esemény hubra adatmegőrzési időszak tartalmazza. Szolgáltatások listáját az [esemény hubok árak részletek](https://azure.microsoft.com/pricing/details/event-hubs/)megtekintése

## <a name="geographic-availability"></a>Földrajzi elérhetősége

Az alábbi régiókban esemény hubok érhető el:

|GEO|Területek|
|---|---|
|Egyesült Államok|Központi Amerikai Egyesült Államok, kelet-Amerikai Egyesült Államok, kelet-amerikai 2, központi Dél-Amerikai Egyesült Államok, nyugati Amerikai Egyesült Államok|
|Európa|Észak-Európa nyugati Európa|
|Ázsia Csendes-óceáni|Kelet-ázsiai, Délkelet-ázsiai|
|Japán|Kelet nyugati Japánban japán|
|Brazília|Brazília Dél|
|Ausztrália|Ausztrália-keleti Ausztrália Könyvesbolt is.|

## <a name="support-and-sla"></a>Támogatási és SLA

A [közösségi fórumok](https://social.msdn.microsoft.com/forums/azure/home)– az esemény hubok technikai támogatás érhető el. Számlázási és előfizetési Dokumentumkezelés támogatásának áttérhet megadva.

Többet szeretne tudni a SLA, lásd: a [Szolgáltatás szint rendelkezést](https://azure.microsoft.com/support/legal/sla/) lapon.

## <a name="next-steps"></a>Következő lépések

Esemény hubok olvashat az alábbi cikkekben talál:

- [Esemény hubok áttekintése][].
- Egy teljes [minta alkalmazást használó esemény hubok][].

[Esemény hubok – áttekintés]: event-hubs-overview.md
[Esemény hubok használó minta alkalmazás]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
