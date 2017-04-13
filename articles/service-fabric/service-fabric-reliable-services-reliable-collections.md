<properties
   pageTitle="Megbízható gyűjtemények |} Microsoft Azure"
   description="Állapot-nyilvántartó services szolgáltatás háló adja meg, amelyek lehetővé teszik, hogy nagymértékben érhető el, méretezhető és alacsony-késés felhő alkalmazásokat ír megbízható gyűjtemények."
   services="service-fabric"
   documentationCenter=".net"
   authors="mcoskun"
   manager="timlt"
   editor="masnider,vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="10/18/2016"
   ms.author="mcoskun"/>

# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Az állapot-nyilvántartó services Azure Service háló megbízható webhelycsoportok – bevezetés

Megbízható gyűjtemények lehetővé teszi erősen érhető el, méretezhető és alacsony-késés felhő alkalmazásokat ír, mintha egyetlen számítógépre alkalmazások, azok írása. A **Microsoft.ServiceFabric.Data.Collections** névtér az osztályok, amely automatikusan elérhetővé teszi a állapotát erősen ki-be a webhelycsoportok tartalmaznak. A fejlesztők a program csak a megbízható webhelycsoport API-k és, és mondja el megbízható webhelycsoportok kezelése a replikált és a helyi állam kell.

Fő megbízható webhelycsoportok és más magas rendelkezésre állás technológiák (például vgx.dll, Azure tábla és Azure várólista szolgáltatás) közötti különbség, hogy az állapot legyen helyileg a szolgáltatás példány közben is elérhetővé erősen. Ez azt jelenti, hogy:

- Minden olvasás helyi, amelynek eredménye kis késés és nagy átviteli felolvassa.
- Minden írás minimális számú hálózati IOs, amelynek kis késés eredménye merülnek fel, és írja a nagy átviteli.

![Webhelycsoportok alakulása képe.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Megbízható gyűjtemények a tekinthető **System.Collections** osztályok természetes alakulása: egy olyan új webhelycsoportok, amely a felhőben, és több számítógépen alkalmazások nélkül összetettsége növelésével a fejlesztők számára készült csoportja. Megbízható gyűjtemények ilyenként, a következők:

- Replikált: Állapotának megváltozásakor replikált magas elérhetőség.
- Állandó: Adatok állandó lemezre tartóssági nagyméretű kimaradások (például adatközponthoz áramszünet) szemben a.
- Aszinkron: API-khoz is aszinkron annak érdekében, hogy szálak nem blokkolt IO hozni.
- Tranzakció alapú: API-khoz csatlakozást a tranzakciók ivóvízkivételre hogy szolgáltatás belül több megbízható gyűjtemények egyszerűen kezelhetők.

A szükséges megbízható gyűjtemények erős konzisztencia garantálja beépített alkalmazás állapotával kapcsolatos érvelés egyszerűbbé tétele érdekében.
Erős konzisztencia véglegesítése Befejezés csak azt követően a teljes tranzakció bejelentkezve kópiák, beleértve az elsődleges többsége határozatképességének tranzakció biztosításával érhető el.
Gyengébb konzisztencia eléréséhez alkalmazások visszatér az ügyfél igénylő is nyugtázza előtt a aszinkron véglegesítés adja eredményül.

A megbízható gyűjtemények API-khoz is egy fejlesztés egyidejű gyűjtemények API-hoz (a **System.Collections.Concurrent** névtér található):

- Aszinkron: Ad vissza egy tevékenység óta eltérően egyidejű gyűjtemények, a műveletek replikált és állandó.
- Nem meg a Paraméterek: használja a `ConditionalValue<T>` való visszatéréshez egy logikai és kimenő paraméterek helyett egy értéket. `ConditionalValue<T>`olyan, mintha `Nullable<T>` , de nincs szükség legyen a struktúra T.
- Tranzakciók: Ahhoz, hogy a felhasználó a műveletek csoport több megbízható gyűjtemények tranzakció tranzakció objektumot használ.

Ma a **Microsoft.ServiceFabric.Data.Collections** két gyűjtemények tartalmazza:

- [Megbízható szótár](https://msdn.microsoft.com/library/azure/dn971511.aspx): kulcs/érték párokká replikált tranzakció alapú és aszinkron gyűjteményét jelképezi. **ConcurrentDictionary**hasonló, a kulcs és az érték is lehet bármilyen típusú.
- [Megbízható várólista](https://msdn.microsoft.com/library/azure/dn971527.aspx): replikált tranzakció alapú és aszinkron szigorú első a kiküldési várólista jelöli. **ConcurrentQueue**hasonlóan értéke lehet bármilyen típusú.

## <a name="isolation-levels"></a>Elkülönítési szintek
Elkülönítési fokot határozza meg, hogy milyen mértékig, amelyhez a tranzakció legyen elkülönítve egyéb tranzakciók által végzett módosításokat.
Vannak elkülönítési kétszintű megbízható tartozó támogatott:

- **Megismételhető olvasás**: Itt adhatja meg, hogy a kimutatások nem tudja olvasni, módosítás, de még nem véglegesített egyéb tranzakció adatok és, hogy nincs más tranzakciók módosíthatja, hogy elolvasta az aktuális tranzakció, amíg befejeződik az aktuális tranzakció adatokat. További információra kíváncsi olvassa el a [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)című témakört.
- **Pillanatkép**: azt adja meg, hogy bármely kimutatás tranzakció által beolvasott adatok megjelenik az elején található a tranzakció volt adatokat tranzakción keresztül egységes verzióját.
A tranzakció felismeri csak a kezdete a tranzakció volt lekötött adatok módosításokat.
Adatok módosítások egyéb tranzakció az aktuális tranzakció kezdetét követő nem láthatók az aktuális tranzakció menjen végbe kimutatásokhoz.
Az effektus, mintha a kimutatások tranzakció beszerzése a lekötött adatai pillanatképét adott tranzakció elején verzióját.
Egy adott időpontban érvényes konzisztensek megbízható gyűjtemények keresztül.
További információra kíváncsi olvassa el a [https://msdn.microsoft.com/library/ms173763.aspx](https://msdn.microsoft.com/library/ms173763.aspx)című témakört.

Megbízható gyűjtemények automatikusan válassza a elkülönítési fokot használható egy adott olvasási művelet attól függően, hogy a művelet, és a szerepkör a másolat tranzakció létrehozási idején.
Az alábbiakban a táblázat, amely a elkülönítési szintű alapértelmezett megbízható szótár és várólista műveletekhez ábrázolja.

| A művelet \ szerepkör      | Elsődleges          | Másodlagos        |
| --------------------- | :--------------- | :--------------- |
| Egyetlen entitás olvasása    | Megismételhető olvasása  | Pillanatkép         |
| Felsorolás \ száma   | Pillanatfelvétel         | Pillanatkép         |

>[AZURE.NOTE] Gyakori egyetlen entitás műveletekhez példák `IReliableDictionary.TryGetValueAsync`, `IReliableQueue.TryPeekAsync`.

A megbízható szótár és a megbízható várólista is támogatja a olvasható a ír.
Más szóval bármely írási tranzakción belül is látható lesz a következő olvasási, amelyhez az ugyanazon tranzakció tartozik.

## <a name="locking"></a>Zárolása
Megbízható tartozó, az összes tranzakcióinak két elosztva: tranzakció nem zárolások a szerezte meg, amíg egy megszakítás vagy a jóváhagyás megszakítja a tranzakciók.

Megbízható szótár sor szint összes egyetlen entitás műveletekhez zárolása használja.
Megbízható várólista óceánokon szigorú tranzakció alapú FIFO tulajdonság feldolgozási ki.
Megbízható várólista használ egy tranzakció, amely lehetővé teszi a művelet szintű zárolások `TryPeekAsync` és/vagy `TryDequeueAsync` és egy tranzakció, amelynek `EnqueueAsync` egyszerre.
Ne feledje, megőrzéséhez FIFO, ha egy `TryPeekAsync` vagy `TryDequeueAsync` minden eddiginél azt látja, hogy a megbízható várólista argumentum üres, azok is zárolása `EnqueueAsync`.

Írja be a műveletek mindig kizárólagos zárolások.
Az olvasási műveletek a zárolás függ, hogy néhány tényezők.
Bármely olvasási művelet kész pillanatkép elkülönítési használatával ingyenes zárolása.
Alapértelmezés szerint minden megismételhető olvasási művelet megosztott zárolások vesz igénybe.
Azonban olvasási műveletek, amely támogatja az megismételhető olvasható, a felhasználó is kérdezze meg a megosztott zárolás helyett frissítési zárolás.
Frissítési zárolás egy aszimmetrikus zárolása, hogy a közös megjeleníthető kölcsönös kizárás fordul elő, ha több tranzakciók zárolása erőforrások potenciális frissítések, várjon egy kis időt, a használt.

A zárolás kompatibilitási mátrix alatt találhatók:

| Kérés \ nyújtott | Nincs lehetőség         | A megosztott       | Frissítés      | Kizárólagos    |
| ----------------- | :----------- | :----------- | :---------- | :----------- |
| A megosztott            | Ha nem ütközik  | Ha nem ütközik  | Ütközés    | Ütközés     |
| Frissítés            | Ha nem ütközik  | Ha nem ütközik  | Ütközés    | Ütközés     |
| Kizárólagos         | Ha nem ütközik  | Ütközés     | Ütközés    | Ütközés     |

Figyelje meg, hogy a megbízható gyűjtemények API-khoz időtúllépési argumentumnak kölcsönös kizárás észlelése szolgál.
Például két tranzakciók (T1 és T2) olvasási és K1 frissíteni próbálja.
Lehetőség, hogy kölcsönös kizárás, mert mindkettő, amilyennek a megosztott zárolás problémákat.
Ebben az esetben az egyik vagy mindkét műveletek az időtúllépést okoz.

Ne feledje, hogy a fenti kölcsönös kizárás esetben kiválóan példája hogyan frissítési zárolás fellépő holtpont elkerülése.

## <a name="persistence-model"></a>Adatmegőrzési modell
A megbízható állam Manager és a megbízható gyűjtemények hajtsa végre a meghívott naplókban és ellenőrzés adatmegőrzési modell.
Ez a, ahol minden állapot módosítása bejelentkezett a lemez és csak a memóriában alkalmazott modell.
A teljes állam magát megőrződnek alkalmanként (más néven Ellenőrzés).
Az az előnye, hogy a delta szekvenciális csak hozzáfűzhető írások lemezen a jobb teljesítmény elérése érdekében be vannak kapcsolva.

A naplókban és ellenőrzés modell nhttp://OnlineHelp.microsoft.com/hu-hu/Bing/dn261810.aspx, először nézzük meg, a végtelen lemez forgatókönyvet.
A megbízható állapot-kezelő naplózza a minden művelet előtt, akkor replikált.
Ezzel a megbízható gyűjtemény alkalmazása csak a memóriában művelet.
Naplók állandó vannak, akkor is, ha a sikertelen, és újra kell indítani, mivel a megbízható állapot-kezelő elegendő információt tartalmaz a naplók ismétlődő lejátszásra van állítva a replika vész el az összes művelet.
Amennyiben a lemez végtelen, naplóbejegyzések soha nem kell, hogy távolítsa el, és a megbízható gyűjtemény kell kezelése csak a memóriában állapotát.

Most már tekintse meg a függvényt lemez forgatókönyvet.
Naplóbejegyzések gyűjteniük, mint a megbízható állapot-kezelő elegendő szabad lemezterület fog futni.
Ebben az esetben, mielőtt a megbízható állapot-kezelő kell csonkítja a naplót újabb rekordok növeléséhez.
Az ellenőrzés megbízható gyűjtemények lemezre a memóriában állapotukban kér.
Feladata, a megbízható gyűjtemények felfelé ponthoz állapotában is.
A megbízható gyűjtemények azok pontjainak elvégezni, ha a megbízható állapot-kezelő az lemezterület felszabadítása a napló is csonkolja.
Módja, ha a kópia újra kell indítani, a megbízható gyűjtemények fognak helyreállítása alkulcsaihoz állapotukban, és a megbízható állapot-kezelő program helyreállítása, majd játssza le az állapotra vonatkozó módosításokat az ellenőrzés óta történt.

>[AZURE.NOTE] A ellenőrzésipont hozzáadása egy másik értékre, hogy javítja a helyreállítási teljesítményt, a gyakori eset bemutatása olvasható.
Ennek az oka pontjainak csak a legújabb verziókat tartalmazó.

## <a name="recommendations"></a>Javaslatok

- Ne módosítsa az olvasási műveletek által visszaadott egyéni típusú objektum (például `TryPeekAsync` vagy `TryGetValueAsync`). Megbízható gyűjtemények, hasonlóan a egyidejű gyűjtemények, objektumok és a lemásolni tilos hivatkozását adja eredményül.
- Lemásolni mély egy egyéni típusú visszaadott objektum módosítás előtt. Mivel a struktúrák és a beépített típusú érték szerint fázis, nem szükséges mély lemásolni őket.
- Ne használjon `TimeSpan.MaxValue` az adatokon való műveletvégzés. Adatokon való műveletvégzés használandó kölcsönös kizárások feltárása.
- Ne használja a tranzakciók után azt lekötött, megszakadt, vagy értékesített.
- Ne használjon egy számbavétel alkalmazásban készült tranzakciók hatókörének kívül.
- Ne hozzon létre egy másik tranzakción belül tranzakció `using` utasítás mivel kölcsönös kizárások okozhat.
- Győződjön meg arról, hogy a `IComparable<TKey>` végrehajtása helyességét. A rendszer a függőség pontjainak űrlapegyesítéshez veszi.
- Frissítési zárolás szándéknak az elem olvasásakor frissítéséhez kövesse megakadályozhatja, hogy egy bizonyos osztály kölcsönös kizárások.
- Fontolja meg a biztonsági másolat használata, és visszaállíthatja az, hogy az vészhelyreállítás funkcionalitását.
- Kerülje az egyetlen entitás műveletek és a több személy műveletek (pl. `GetCountAsync`, `CreateEnumerableAsync`) miatt a különböző elkülönítési szintek azonos műveletben.

Íme néhány dolog, amit érdemes észben tartania:

- Az alapértelmezett érték a 4 másodpercig megbízható webhelycsoport API. A legtöbb felhasználó nem felül ez.
- Az alapértelmezett lemondási token `CancellationToken.None` az összes megbízható gyűjtemények API-khoz.
- Az írja be a paraméter (*TKey*) egy megbízható szótárhoz helyesen kell megvalósítása `GetHashCode()` és `Equals()`. Billentyűk megváltoztatható kell lennie.
- A megbízható gyűjtemények magas elérhetősége eléréséhez minden szolgáltatás legalább egy cél és a méretét, a 3 legkisebb kópiakészlet kell rendelkeznie.
- A másodlagos műveleteket olvasási előfordulhat, hogy olvassa el a verziók, amelyek nem véglegesített kvórum.
Ez azt jelenti, hogy egyetlen másodlagos beolvasott adatok verziójának előfordulhat, hogy kell hamis előrehaladtával.
Állandóan stabil természetesen elsődleges Olvasás: is soha nem kell hamis előrehaladtával.

## <a name="next-steps"></a>Következő lépések

- [Megbízható szolgáltatások – első lépések](service-fabric-reliable-services-quick-start.md)
- [Megbízható gyűjtemények használata](service-fabric-work-with-reliable-collections.md)
- [Megbízható szolgáltatások értesítések](service-fabric-reliable-services-notifications.md)
- [Megbízható szolgáltatások biztonsági mentése és visszaállítása (Vészhelyreállítás)](service-fabric-reliable-services-backup-restore.md)
- [Megbízható állam Manager konfigurálása](service-fabric-reliable-services-configuration.md)
- [Szolgáltatás háló webes API-szolgáltatások – első lépések](service-fabric-reliable-services-communication-webapi.md)
- [Speciális programozási modell megbízható szolgáltatások használatát](service-fabric-reliable-services-advanced-usage.md)
- [Fejlesztői segédlet megbízható gyűjtemény](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
