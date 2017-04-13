<properties 
    pageTitle="Azure sorban várakozó és szolgáltatás Bus várakozási sorba - összehasonlítása és a szokásos testhelyzetből |} Microsoft Azure"
    description="Elemzi a különbségeket és Azure által kínált sorok két típusa közötti hasonlóságokat."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="tbd"
    ms.date="09/23/2016"
    ms.author="sethm" />

# <a name="azure-queues-and-service-bus-queues---compared-and-contrasted"></a>Azure sorban várakozó és szolgáltatás Bus várakozási sorba - összehasonlítása és a szokásos testhelyzetből

Ez a cikk elemzi a különbségeket és a Microsoft Azure ma nyújtotta sorok két típusa közötti hasonlóságokat: Azure sorban várakozó és szolgáltatás Bus sorban várakozó. Ezek az információk segítségével is összehasonlítása és a megfelelő technológiák kontraszt és kell tenni, hogy melyik megoldást a legjobban az igényeinek döntés további tájékoztatást.

## <a name="introduction"></a>– Bevezetés

Microsoft Azure kétféle várólista mechanizmusok támogatja: **Azure sorban várakozó** és **Szolgáltatás Bus sorban várakozó**.

**Azure sorban várakozó**, az [Azure tároló](https://azure.microsoft.com/services/storage/) infrastruktúra részét képező Ez a funkció egy egyszerű többi-alapú Get/helyezése/betekintő felületet biztosító megbízható, állandó Csevegés belül és a szolgáltatások között.

**Szolgáltatás Bus sorban várakozó** szélesebb [Azure üzenetküldés](https://azure.microsoft.com/services/service-bus/) infrastruktúra, amely támogatja a közzététel/előfizetés, webes szolgáltatás távelérési és integráció mintázatok és queuing részét képezik. Szolgáltatás Bus sorban várakozó, témakörök/előfizetések és jelfogók kapcsolatos további tudnivalókért lásd: a [szolgáltatás Bus üzenetküldés áttekintése](service-bus-messaging-overview.md).

Mindkét várólista technológiák egyidejűleg létezik, miközben Azure sorban várakozó először egy dedikált várólista tároló mechanizmusként az Azure tároló szolgáltatások fölött beépített vezetett be. Szolgáltatás Bus sorban várakozó kialakításának készült alkalmazások vagy több kommunikációs protokollt, adatok szerződést, előfordulhat, hogy átterjedő alkalmazásösszetevők integráció szélesebb "üzenetküldés brokered" infrastruktúra fölött megbízható tartományok és/vagy hálózati környezetben.

Ez a cikk a két várólista technológiák által viselkedését és az egyes által biztosított szolgáltatások végrehajtásának különbségei tárgyal Azure által kínált hasonlítja össze. A cikk is nyújt útmutatást kiválaszthatja, hogy mely funkciók előfordulhat, hogy az igényeinek alkalmazás fejlesztési.

## <a name="technology-selection-considerations"></a>Technológia kijelölés kapcsolatos szempontok

Azure sorban várakozó és a szolgáltatás Bus sorban várakozó az üzenetet, amely jelenleg a Microsoft Azure service queuing példányait. Minden egyes tartalmaz egy némileg eltérő szolgáltatáskészlet, ami azt jelenti, válassza az egyiket, vagy használja az adott levelezőprogramból vagy vállalati/műszaki problémamegoldó az egyéni igényeknek megfelelően.

Annak megállapítása, hogy melyik várólista technológia célja egy adott megoldás illeszkedik, megoldás fejlesztője és a fejlesztők kell figyelembe az alábbi javaslatokat. További információ a következő szakaszban olvashat.

Megoldás architect/fejlesztői, **akkor érdemes használni az Azure sorban várakozó** esetén:

- Az alkalmazás 80 GB-nál nagyobb az üzenetek kell tárolnia egy várólista, ahol az üzenetek 7 napnál rövidebb élettartamának lennie.

- Az alkalmazás szeretne nyomon követni a sorban szereplő üzenet feldolgozásra. Ez akkor hasznos, ha az üzenet feldolgozása dolgozó összeomlik. A későbbi dolgozó használhatja ezeket az információkat a, ahol abbahagyta a előzetes dolgozó továbbra is.

- Kiszolgáló egymás naplók az összes szemben a sorok végrehajtott tranzakciók van szüksége.

Megoldás architect/fejlesztői, **vegye figyelembe használ szolgáltatási Bus sorban várakozó** esetén:

- A megoldás fogadhat üzeneteket anélkül, hogy a sor lekérdezik kell lennie. A szolgáltatás Bus, ez érhető el, a hosszú lekérdezési való fogadási műveletet, amely támogatja a szolgáltatás Bus a TCP-alapú protokollok használatával.

- A megoldás igényel egy garantált első-az első-kimenő megadására a várólista (FIFO) rendelt kézbesítési.

- Azt szeretné, hogy a szimmetrikus használhatóság Azure-ban és a Windows Server (saját felhő). További tudnivalókért lásd: a [Windows Server szolgáltatás Bus](https://msdn.microsoft.com/library/dn282144.aspx).

- A megoldás tudja támogatja az ismétlődő automatikus észlelése kell lennie.

- Azt szeretné, hogy az alkalmazás folyamat során a párhuzamos hosszabb ideig futó adatfolyamként (üzenetek társítva az üzenet a [munkamenet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) tulajdonságával adatfolyam). Ebben a modellben minden csomópont igénybe alkalmazásban hozzáférésért adatfolyamok, nem pedig az üzeneteket. Adatfolyam igénybe csomópont kapnak, amikor a csomópont tranzakciókat használ, az alkalmazás adatfolyam állam állapotának ellenőrzéséhez.

- A megoldás tranzakció alapú viselkedéséről és küldésekor vagy fogadásakor több üzenetet a sorból atomicity szükséges.

- Az alkalmazás-specifikus terhelését time-to-live (TTL) jellemző túllépheti 7 napig tartó.

- Az alkalmazás, amely 64 KB túllépheti üzenetek kezeli, de valószínűleg nem megközelítés a 256 KB korlátozza.

- Annak követelménye, a sorok és a különböző jogosultságok és engedélyek szerepköralapú hozzáférés-modell nyújt a feladónak és vevők kezelése.

- A várólista méretét a továbbiakban nem nagyobb 80 GB-nál nagyobb.

- Szeretné a AMQP 1.0 szabványos alapú üzenetben protokoll használatát. AMQP kapcsolatos további tudnivalókért olvassa el a [Szolgáltatás Bus AMQP – áttekintés](./service-bus-amqp-overview.md)című témakört.

- Az esetleges áttérés várólista alapú pontok közötti kommunikáció, amely lehetővé teszi, hogy a zökkenőmentes integráció a további vevők (előfizetők számára), a sor küldött néhány vagy összes üzenetek független példányt kap, amelyek üzenet exchange mintát is envision. A közzététel/előfizetés képesség natív módon nyújtotta szolgáltatás Bus az utóbbi hivatkozik.

- A Csevegés megoldás ahhoz tartalmat hozzáadó felhasználók támogatása a "A legtöbb – egyszeri" kézbesítési garancia meg, hogy a infrastruktúrájának bővítéseit összetevők készítése nélkül.

- Szeretné, hogy tudja közzétenni, és az üzenetek kötegekben felhasználása.

- A Windows Communication Foundation (WCF)-kapcsolati egymást fedő a .NET-keretrendszer teljes integrációja van szüksége.

## <a name="comparing-azure-queues-and-service-bus-queues"></a>Azure sorban várakozó és szolgáltatás Bus sorban várakozó összehasonlítása

A táblák, az alábbi szakaszok logikai csoportja várólista funkciók, adja meg, és mondja el, hasonlítsa össze áttekintése, az Azure sorban várakozó és szolgáltatás Bus sorban várakozó is elérhető funkciók.

## <a name="foundational-capabilities"></a>Eligazodást funkciók

Ez a szakasz hasonlítja össze az Azure sorban várakozó és szolgáltatás Bus sorban várakozó által biztosított alapvető várólista funkciók közül.

|Összehasonlító feltételek|Azure sorban várakozó|A szolgáltatás Bus sorban várakozó|
|---|---|---|
|Rendezés garancia|**nem** <br/><br>További tudnivalókért olvassa el az első megjegyzést, a "További tudnivalók" című szakaszában.</br>|**Igen – első be először ki (FIFO)**<br/><br>(való üzenetküldés munkamenetek)|
|Kézbesítési garancia|**A legkisebb-egyszer**|**A legkisebb-egyszer**<br/><br/>**A legtöbb – egyszeri**|
|A művelet atomi támogatás|**nem**|**igen**<br/><br/>|
|Működés fogadása|**Nem blokkolja**<br/><br/>(befejezése azonnal Ha nem új üzenetet nem talál.)|**Blokkolása időtúllépés/nélkül**<br/><br/>(a ajánlatok hosszú lekérdezési, vagy a ["Üstökös technika"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Nem blokkolja**<br/><br/>(.NET való felügyelt API csak)|
|Leküldéses stílusú API|**nem**|**igen**<br/><br/>[OnMessage](https://msdn.microsoft.com/library/azure/jj908682.aspx) és **OnMessage** munkamenetek .NET API-val.|
|Üzemmódban fogadhat|**Behúzás és haszonbérbe adása**|**Behúzás és zárolása**<br/><br/>**Fogadása és törlése**|
|Az access kizárólagos módban|**Bérleti-alapú**|**Zárolás-alapú**|
|Bérleti/rögzített időtartam|**30 másodperc (alapértelmezett)**<br/><br/>**7 nap (legnagyobb)** (Megújítása, vagy egy üzenet bérleti [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API segítségével kiadás.)|**60 másodperc (alapértelmezett)**<br/><br/>Üzenet lakat [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API segítségével megújíthatja.|
|Pontosság bérleti/zárolása|**Üzenet szint**<br/><br/>(minden üzenet egy másik időtúllépése érték, amely a majd frissítheti igény szerint az üzenetet, feldolgozása [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API segítségével közben is van)|**A várakozási szint**<br/><br/>(minden sor az üzeneteket alkalmazott zárolása pontosság van, de a zárolás [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) API segítségével megújíthatja.)|
|Kötegelt fogadása|**igen**<br/><br/>(kifejezetten tartalmazó üzenet száma 32 üzenetek legfeljebb az üzenetek beolvasása közben)|**igen**<br/><br/>(implicit módon engedélyezése tulajdonság előtti fájllehívási vagy explicit módon való tranzakciók)|
|Kötegelt küldése|**nem**|**igen**<br/><br/>(való tranzakciók vagy ügyféloldali kötegelés)|

### <a name="additional-information"></a>További információk

- Azure üzeneteket a szokásos első-az első – kimenő, de néha lehet őket sorrendje; Ha például egy üzenetet a láthatóság időtúllépés időtartam lejártakor (például eredményeként feldolgozása közben összeomló ügyfélalkalmazás). A láthatóság időtúllépése lejár, amikor az üzenet láthatóvá válik a ismét a feldolgozásához azt egy másik dolgozó várakozási sorban. Ezen a ponton az újonnan látható üzenet előfordulhat, hogy helyét a várakozási sorban található (a kell várólistából kivéve ismét) után egy üzenet, amely azt követően eredetileg a sorbaállítva.

- Ha már használja a Azure BLOB-tároló vagy táblákat, és használatbavétele sorban várakozó, meg vannak garantált 99,9 % elérhetősége. Ha az szolgáltatás Bus sorban várakozó BLOB vagy táblázatot használja, alsó elérhetősége lesz.

- A szolgáltatás Bus sorban várakozó garantált FIFO minta használatához az üzenet-munkamenetek. Abban az esetben, ha az alkalmazás összeomlik, egy üzenetet a **betekintő és zárolása** üzemmódban kapott feldolgozása közben, a következő alkalommal, amikor egy várólista telefonkagylót, fogadja el egy üzenetben munkamenet elindul, a sikertelen üzenettel a time-to-live (TTL) időszak lejárta után.

- Azure sorban várakozó készült támogatja a normál várólista alkalmazási eseteit, például szétválasztó alkalmazásösszetevők méretezhetőség és hibatűrést hibák, töltse be a kiegyenlítés és a munkafolyamatok.

- Szolgáltatás Bus sorban várakozó támogatja *A legkisebb – egyszeri* kézbesítési garancia. Ezeken kívül szemantikai *A legtöbb – egyszeri* nem használható a munkamenet-állapot a alkalmazás állapot tárolási és a tranzakciók atomically üzeneteket fogadni, és a munkamenet-állapot frissítése használatával.

- Azure sorban várakozó révén egységes és egységes programozási modell sorban várakozó, táblázatok és BLOB – a fejlesztők és műveletek csapatok.

- Szolgáltatás Bus sorban várakozó támogatása a egyetlen sor környezetében helyi tranzakciók.

- A szolgáltatás Bus által támogatott **fogadása, és törölje** mód lehetővé teszi a csökkentheti az üzenetben műveletek száma (és a kapcsolódó költség) leeresztett kézbesítési garancia ellenében.

- Azure sorban várakozó bérletek küldje el az azt jelenti, hogy az üzenetek bérletek bővítése. Ez a rövid bérletek üzenetekre megőrzéséhez a dolgozók lehetővé teszi. Így dolgozó összeomlik, ha az üzenetet is gyorsan feldolgoztatni újra egy másik dolgozó. Ezeken kívül dolgozó bővíthetik a bérleti egy üzeneten, ha szüksége van rá feldolgozása aktuális bérleti időnél tovább.

- Azure sorok láthatóságát időtúllépés is beállítása után a enqueueing, vagy az üzenet üzenetmozgatót kínálnak. Ezeken kívül üzenet futásidőben különböző bérleti értékek frissítése, és frissítse a különböző értékek üzenetére az ugyanazon sorban. Szolgáltatás Bus zárolása időtúllépései határozzák meg a várólista metaadatok; a zárolás megújíthatja azonban hívja fel a [RenewLock](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.renewlock.aspx) módszert.

- A maximális időkorlát esetében egy blokkolása fogadási művelet szolgáltatás Bus sorban várakozó 24 napig tart. Azonban a többi-alapú időtúllépései 55 másodperc maximális érték van.

- Ügyféloldali kötegelés szolgáltatás Bus által biztosított lehetővé teszi, hogy a köteg több üzenetet egy egyetlen küldés működésbe várólista ügyfél. Kötegelés csak akkor aszinkron küldés műveletekhez.

- Szolgáltatások, például a 200 TB plafon Azure sorban várakozó (Ha a fiókok különböző helyekre történő virtualizálása több) és a korlátlan sorban várakozó legyen egy ideális platform szoftver szolgáltatók.

- Azure sorban várakozó egy rugalmas adja meg, és performant meghatalmazott access mechanizmusa.

## <a name="advanced-capabilities"></a>Speciális lehetőségei

Ez a szakasz Azure sorban várakozó és szolgáltatás Bus sorban várakozó által biztosított speciális funkciókat hasonlítja össze.

|Összehasonlító feltételek|Azure sorban várakozó|A szolgáltatás Bus sorban várakozó|
|---|---|---|
|Ütemezett kézbesítési|**igen**|**igen**|
|Az automatikus csendes formában szerepelnek|**nem**|**igen**|
|A várakozási time-to-live érték növelése|**igen**<br/><br/>(keresztül a helyi frissítés a láthatóság időtúllépése)|**igen**<br/><br/>(feltéve keresztül egy dedikált API függvény)|
|Üzenet támogatási elhalt|**igen**|**igen**|
|A helyi frissítés|**igen**|**igen**|
|Kiszolgálóoldali tranzakciónaplója|**igen**|**nem**|
|Tárolási mértékek|**igen**<br/><br/>**A perc mértékek**: az elérhetőség, TP-k, API valós idejű mértékek hívásbeállítások száma, hibaérték száma és az egyéb, az összes értéket valós idejű (percenkénti összesíti, és mi gyártási csak történt a néhány percen belül jelenteni. További tudnivalókért olvassa el a [Tárolási Analytics mértékek kapcsolatos](https://msdn.microsoft.com/library/azure/hh343258.aspx)című témakört.|**igen**<br/><br/>(tömeges lekérdezések [GetQueues](https://msdn.microsoft.com/library/azure/hh293128.aspx)hívja fel)|
|Állapot kezelése|**nem**|**igen**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.entitystatus.aspx)|
|Üzenetek automatikus-továbbító|**nem**|**igen**|
|Véglegesen várólista függvény|**igen**|**nem**|
|Csoportok Message|**nem**|**igen**<br/><br/>(való üzenetküldés munkamenetek)|
|Üzenet csoportonként alkalmazás állapota|**nem**|**igen**|
|Ismétlődő észlelése|**nem**|**igen**<br/><br/>(a feladó oldalon konfigurálható)|
|WCF-integráció|**nem**|**igen**<br/><br/>(kínál ki-be a WCF kötések)|
|A Windows Tűzfal integrációja|**Egyéni**<br/><br/>(egyéni folyamatkövető Alaprendszer tevékenység épület igényel)|**Natív**<br/><br/>(kínál ki-be a Windows Tűzfal tevékenységek)|
|Csoportok message böngészés|**nem**|**igen**|
|Üzenet-munkamenetek beolvasása azonosító szerint|**nem**|**igen**|

### <a name="additional-information"></a>További információk

- Mindkét várólista technológiák engedélyezése egy üzenet, és később kézbesítési ütemezhető.

- Várólista automatikus továbbítási lehetővé teszi, hogy egy adott sorba, amelyből a fogadó alkalmazás fogyasztása az üzenetet az üzenetek automatikus-előre sorban várakozó ezer. Ez az eljárás használatával biztonsági elérése, a control flow, és elkülönítése tárolás minden üzenet-közzétevő között.

- Azure sorban várakozó támogatása üzenet tartalmának módosítása. Használhatja ezt a funkciót megőrzéséhez állapot adatai, és egyre növekvő tendenciát mutat végrehajtásával kapcsolatos frissítések az üzenetbe, hogy az utolsó ismert ellenőrzés, hanem nulláról feldolgozhatók. Szolgáltatás Bus sorok engedélyezheti az ugyanolyan eseteket való üzenet munkamenetek. Munkamenetek lehetővé teszi azok menteni, valamint az alkalmazás feldolgozási állapot ( [SetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.setstate.aspx) és [GetState](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagesession.getstate.aspx)) segítségével.

- [Csendes formában szerepelnek](service-bus-dead-letter-queues.md), amelyet csak támogat a szolgáltatás Bus sorban várakozó, akkor lehet hasznos elkülönítésére jelennek meg, amelyek nem lehet feldolgozni sikeresen a fogadó alkalmazás vagy üzeneteket a cél-lejárt time-to-live (TTL) tulajdonság miatt nem tudja elérni. A TTL (élettartam) értéket adja meg, hogy mennyi ideig üzenet marad a várakozási sorban található. A szolgáltatás Bus az üzenet átkerülnek $DeadLetterQueue hívja meg a TTL (élettartam) időszak lejárta egy külön sorba.

- "Poison" üzenetek kereséséhez Azure sorban várakozó, amikor az üzenet üzenetmozgatót az alkalmazás vizsgálja meg a **[DequeueCount](https://msdn.microsoft.com/library/azure/dd179474.aspx)** tulajdonsága az üzenetet. Ha **DequeueCount** egy megadott küszöbértékét fölött, az alkalmazás az üzenetet áthelyezi egy alkalmazás által definiált "" halottlevél.

- Azure sorban várakozó lehetővé teszi az összes szemben a várólista, valamint összesített mértékek végrehajtott tranzakciók részletes naplója beszerzése. Az alábbi lehetőségek is hasznosak hibakeresése során, és hogyan használja az alkalmazás a Azure sorban várakozó ismertetése. Ezek egyben hasznos, ha az alkalmazás teljesítményének javítása és a sorok használatára a költségek csökkentése.

- Amely a "üzenet munkamenetek" szolgáltatás Bus által támogatott lehetővé teszi, hogy az üzeneteket, amelyek egy adott címzett, és a hoz létre a munkamenet-szerű affinitás üzenetek és a megfelelő vevők között társítandó bizonyos logikai csoporthoz nem tartozó. Ez a szolgáltatás Bus fejlett funkcionalitást engedélyezheti egy üzeneten a [munkamenet](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.sessionid.aspx) tulajdonság beállításával. Vevők egy adott munkamenetben ID figyelnie, majd jelennek meg, amelyek a megadott munkamenet-azonosító megosztása.

- A párhuzamos észlelési funkció automatikusan szolgáltatás Bus sorban várakozó által támogatott eltávolítása ismétlődő üzeneteket várólista vagy a témakör a [MessageID](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.messageid.aspx) tulajdonság értéke alapján.

## <a name="capacity-and-quotas"></a>Kapacitás és kvóták

Ebben a szakaszban a [kapacitásának és kvóták](service-bus-quotas.md) alkalmazhatja rajtuk Azure sorban várakozó és szolgáltatás Bus sorban várakozó hasonlítja össze.

|Összehasonlító feltételek|Azure sorban várakozó|A szolgáltatás Bus sorban várakozó|
|---|---|---|
|Várólista maximális mérete|**200 TB**<br/><br/>(csak egyetlen fiók tárolókapacitású)|**1 GB 80 GB**<br/><br/>(a várakozási sora és a [szétválasztás engedélyezése](service-bus-partitioning.md) – létrehozását követően definiált lásd: a "További információ" című szakaszt)|
|Üzenetek maximális mérete|**64 KB**<br/><br/>(Ha **Base64** kódolással 48 KB)<br/><br/>Azure kombinálva sorban várakozó és BLOB – mely pontján várólistára helyezés is támogatja a nagyméretű üzenetek legfeljebb 200GB egyetlen elemet.|**256 KB** és **1 MB**<br/><br/>(beleértve az élőfej- és törzsébe élőfej maximális méret: 64 KB).<br/><br/>A [szolgáltatási réteg](service-bus-premium-messaging.md)függ.|
|Üzenetek maximális TTL (élettartam)|**7 nap**|**`TimeSpan.Max`**|
|Sorok maximális száma|**Korlátlan**|**10 000**<br/><br/>(service névtér, egy növelhető)|
|Egyidejű ügyfelek maximális száma|**Korlátlan**|**Korlátlan**<br/><br/>(100 egyidejű kapcsolat korlát csak érvényes TCP Protocol (protokoll) alapú kommunikáció)|

### <a name="additional-information"></a>További információk

- Szolgáltatás Bus várólista fájlméretet érintő korlátozásokról kényszeríti. A várólista maximális mérete a sorban a létrehozása után megadni, és beállíthatja, hogy egy 1 és 80 GB közötti értéket. Várólista méret értéke állítható be a sor létrehozása lejár, további bejövő üzenetek elutasításra kerül, és a kivételt fogadja a hívó kóddal. Szolgáltatás Bus kvótájának kapcsolatos további tudnivalókért olvassa el a [Szolgáltatás Bus kvóták](service-bus-quotas.md)című témakört.

- 1, 2, 3, 4-es vagy 5 GB-os méretben (az alapértelmezett érték 1 GB) szolgáltatás Bus sorban várakozó hozhat létre. A szétválasztás engedélyezve van (Ez az alapértelmezett), akkor a szolgáltatás Bus 16 partíciót létrehozza minden megadott GB. Ilyen hoz létre, amely 5 GB méretű várólista, ha a 16 partíciót várólista maximális mérete válik (5 * 16) = 80 GB. A maximális méretét a particionált várólista vagy a témakör lépése megjeleníti az [Azure portál][]megjelenik.

- Az Azure sorok Ha az üzenet tartalmát nem XML vannak, akkor azt kell **Base64** kódolású. Ha Ön **Base64**– az üzenet kódolását, a felhasználó-tartalom is lehet legfeljebb 48 KB 64 KB helyett.

- A szolgáltatás Bus sorok, minden üzenet várólista tárolt két részből áll: a fejléc és az egy üzenet. Az üzenet teljes mérete nem haladhatja meg az üzenetek maximális mérete a szolgáltatási réteg által támogatott.

- Ügyfelek kommunikálni szolgáltatás Bus sorban várakozó TCP protokollon keresztül, amikor egy adott szolgáltatás Bus sorba egyidejű kapcsolatainak maximális száma korlátozódik 100. Ez a szám meg van osztva küldő és között. Eléri a kvóta, további kapcsolatok későbbi kérelmek elutasításra kerül, és a kivétel fogadja a hívó kóddal. Ezt a korlátot nem írják elő ügyfelek többi-alapú API segítségével kapcsolatot.

- Ha szüksége van a szolgáltatás Bus egyetlen névteret 10 000-nél több sorban várakozó, az Azure támogatási csoport munkatársaitól kaphat, és növekedését kérése. A szolgáltatás Bus 10 000 sorban várakozó túl méretezéséhez további névtér az [Azure portálon][]is létrehozhat.

## <a name="management-and-operations"></a>Kezelési és műveleti

Ez a szakasz az Azure sorban várakozó és szolgáltatás Bus sorban várakozó által biztosított-felügyeleti funkciókat hasonlítja össze.

|Összehasonlító feltételek|Azure sorban várakozó|A szolgáltatás Bus sorban várakozó|
|---|---|---|
|Adatkezelési Protocol (protokoll)|**HTTP-/ HTTPS FÖLÉ**|**HTTPS FÖLÉ**|
|Futási idejű Protocol (protokoll)|**HTTP-/ HTTPS FÖLÉ**|**HTTPS FÖLÉ**<br/><br/>**AMQP 1.0 Standard (TLS a TCP)**|
|.NET felügyelt API|**igen**<br/><br/>(A .NET felügyelt tároló ügyfélprogram API-val)|**igen**<br/><br/>(A .NET felügyelt brokered üzenetben API-val)|
|Natív C++|**igen**|**nem**|
|Java API|**igen**|**igen**|
|PHP API|**igen**|**igen**|
|NODE.js API|**igen**|**igen**|
|Tetszőleges metaadat-támogatás|**igen**|**nem**|
|A várakozási elnevezési szabályai|**Legfeljebb 63 karakternél hosszabb**<br/><br/>(Belül várólista nevét kell kisbetűre cseréli le.)|**Legfeljebb 260 karakter hosszú**<br/><br/>(Várólista útvonalak és a nevek és nagybetűk között.)|
|Ismerkedés a várakozási hossz függvény|**igen**<br/><br/>(Közelítő értéke Ha üzenetek elévülési túl a TTL (élettartam) nélkül törléséről.)|**igen**<br/><br/>(Pontos, pont-a-idő érték.)|
|Betekintő függvény|**igen**|**igen**|

### <a name="additional-information"></a>További információk

- Azure sorban várakozó támogatása tetszőleges attribútumok, amely a várólista leírás név/érték összekapcsolásával formájában alkalmazhatók.

- Mindkét várólista technológiák ajánlja fel, az azt jelenti, hogy egy üzenet megtekintése anélkül, hogy zárolása, amely hasznos lehet várólista Explorerre/eszköz végrehajtása során.

- A szolgáltatás Bus .NET brokered üzenetben API-khoz emelés kétirányú TCP-kapcsolatokat a HTTP-többi összehasonlítva nagyobb teljesítmény, és a AMQP 1.0 szabványos protokoll támogatják.

- Az Azure várólista nevek hosszú lehet karakter a 3-63, kisbetűket, számokat és kötőjelet is tartalmazhat. További tudnivalókért lásd: a [sorok elnevezéséről és a metaadatokat](https://msdn.microsoft.com/library/azure/dd179349.aspx).

- Szolgáltatás Bus várakozási sorban található neveket is felfelé 260 karakter hosszú lehet, és kevesebb korlátozó elnevezési szabályokat. Szolgáltatás Bus várakozási sorban található neveket is tartalmazhatnak, betűket, számokat, időszakok, kötőjeleket és aláhúzás karakterek találhatók.

## <a name="authentication-and-authorization"></a>Hitelesítés és az engedélyezés

Ez a szakasz ismerteti, hogy a hitelesítési és engedélyezési által támogatott funkciók Azure sorban várakozó és szolgáltatás Bus sorban várakozó.

|Összehasonlító feltételek|Azure sorban várakozó|A szolgáltatás Bus sorban várakozó|
|---|---|---|
|Hitelesítés|**Szimmetrikus kulcs**|**Szimmetrikus kulcs**|
|Biztonsági modell|Delegált hozzáférésének Társítások tokenek.|TÁRSÍTÁSOK|
|Identitás szolgáltató összevonás|**nem**|**igen**|

### <a name="additional-information"></a>További információk

- Akár a várólista technológiák minden kérés hitelesíteni kell. A névtelen hozzáférés nyilvános sorok nem támogatottak. [Társítások](service-bus-sas-overview.md)használ, a csak olvasásra Társítások, csak olvasható Társítások vagy akár egy teljes hozzáférési Társítások közzétéve ebben az esetben is foglalkozik.

- A hitelesítési mód Azure sorban várakozó által biztosított magában foglalja a szimmetrikus termékkulccsal, amely egy kivonat-alapú üzenet hitelesítési kód (HMAC), számított SHA-256 algoritmus és **Base64** karakterláncként kódolt felhasználása. További információt a megfelelő protokollt [a Azure tároló szolgáltatást hitelesítés](https://msdn.microsoft.com/library/azure/dd179428.aspx)témakörben találhatók. Szolgáltatás Bus sorban várakozó szimmetrikus kulcsokkal hasonló modell támogatja. További tudnivalókért olvassa el a [Megosztott Access aláírás hitelesítési szolgáltatás Bus](service-bus-shared-access-signature-authentication.md)című témakört.

## <a name="cost"></a>Költség

Ebben a szakaszban a költség szemszögéből Azure sorban várakozó és szolgáltatás Bus sorban várakozó hasonlítja össze.

|Összehasonlító feltételek|Azure sorban várakozó|A szolgáltatás Bus sorban várakozó|
|---|---|---|
|A várakozási tranzakció költség|**$0.0036**<br/><br/>száma (100 000 tranzakciók)|**Egyszerű réteg**: **$0,05**<br/><br/>(per millió műveletek)|
|Számlázható műveletek|**Az összes**|**Küldési/fogadási csak**<br/><br/>(ingyenes egyéb műveletek)|
|Üresjárati tranzakciók|**Számlázható**<br/><br/>(Egy üres várólista lekérdezése számít számlázható tranzakció)|**Számlázható**<br/><br/>(Egy üres várólista ellen fogadáskor számlázható üzenet tekintendő.)|
|Tárterület költség|**$0.07**<br/><br/>(/ GB/hó)|**0,00 Ft**|
|Kimenő adatátviteli költségek|**$0.12 - $0.19**<br/><br/>(Attól függően, hogy a geography.)|**$0.12 - $0.19**<br/><br/>(Attól függően, hogy a geography.)|

### <a name="additional-information"></a>További információk

- Adatok átvitele terheli az Azure adatközpontokkal elhagyása az interneten keresztül az egy adott számlázási időszak adatok mennyiségét alapján.

- Az azonos régión belüli található Azure szolgáltatásai között adatátvitel ingyenesen nem vonatkoznak.

- Az írás, kezdve minden bejövő adatátvitel nem vonatkoznak ingyenesen.

- A támogatás megadott hosszú lekérdezési, használ szolgáltatási Bus sorban várakozó lehet esetekben, amikor alacsony-késés kézbesítési szükség-e a tényleges költség.

>[AZURE.NOTE] Az összes költségek változhatnak. Az alábbi táblázat aktuális árak tükrözi, és nem tartalmaz bármely promóciós ajánlatok, amely jelenleg lesz elérhető. Azure árak információt lásd: az [Azure árak](https://azure.microsoft.com/pricing/) lapon. Szolgáltatás Bus árak kapcsolatos további tudnivalókért olvassa el a [szolgáltatás Bus árak](https://azure.microsoft.com/pricing/details/service-bus/)című témakört.

## <a name="conclusion"></a>Elfogadásáról

Úgy is hozzáférjenek a két technológiák mélyebb ismertetése, fogja tudni, hogy melyik várólista technológia szeretne használni, további tájékoztatást döntés és mikor. Mikor érdemes használni az Azure sorban várakozó vagy szolgáltatás Bus sorban várakozó szóló egyértelműen számos tényező függ. Ezek a tényezők erősen függhet az alkalmazás és a saját architektúra igényeihez. Ha az alkalmazás már használja a Microsoft Azure core funkcióinak, előfordulhat, hogy inkább válassza az Azure sorok, különösen akkor, ha szüksége van egyszerű kommunikáció és a üzenetküldési szolgáltatások vagy méretű 80 GB-nál nagyobb lehet szükség sorok között.

Mivel a szolgáltatás Bus sorban várakozó speciális funkciókat, például a munkamenetek, tranzakciók, számos ismétlődő automatikus észlelési kézbesítetlen-formában szerepelnek és tartós közzététel/előfizetés funkciók, azokat is előnyben részesített választási lehetőséget is, ha a egy hibrid alkalmazás készítésekor, vagy ha az alkalmazás, különben a program csak ezek a funkciók.

## <a name="next-steps"></a>Következő lépések

A következő cikkekben további útmutatást és információt Azure sorban várakozó vagy szolgáltatás Bus sorban várakozó használatával.

- [Szolgáltatás Bus sorban várakozó használata](service-bus-dotnet-get-started-with-queues.md)
- [A várakozási sorban található tároló szolgáltatás használata](../storage/storage-dotnet-how-to-use-queues.md)
- [Gyakorlati tanácsok a teljesítmény javítása használ szolgáltatási Bus brokered üzenetküldés](service-bus-performance-improvements.md)
- [Sorok és Azure Service Bus témakörei bemutatása](http://www.code-magazine.com/article.aspx?quickid=1112041)
- [A Fejlesztőeszközök útmutató szolgáltatás Bus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
- [Azure várólista szolgáltatással](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)
- [Azure tároló számlázási – sávszélességgel, tranzakciók és kapacitásának ismertetése](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx)

[Azure portál]: https://portal.azure.com
 
