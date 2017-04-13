<properties
   pageTitle="Megbízható szolgáltatási architektúrája |} Microsoft Azure"
   description="A megbízható szolgáltatási architektúrája, az állapot-nyilvántartó és állapot nélküli szolgáltatások áttekintése"
   services="service-fabric"
   documentationCenter=".net"
   authors="AlanWarwick"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/30/2016"
   ms.author="alanwar"/>

# <a name="architecture-for-stateful-and-stateless-reliable-services"></a>Az állapot-nyilvántartó és állapot nélküli megbízható szolgáltatásokhoz architektúra

Az Azure szolgáltatás megbízható háló állapot-nyilvántartó vagy állapot nélküli lehetnek. Egy adott architektúra belül minden szolgáltatás típusa fut. Ez a cikk a architektúrákban témakörben olvashat.
A [megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md) további információt az állapot-nyilvántartó és állapot nélküli szolgáltatások közötti különbségek című témakörben találhat.

## <a name="stateful-reliable-services"></a>Állapot-nyilvántartó megbízható szolgáltatások

### <a name="architecture-of-a-stateful-service"></a>Az állapot-nyilvántartó szolgáltatás architektúrája
![Állapot-nyilvántartó szolgáltatás architektúra ábrája](./media/service-fabric-reliable-services-platform-architecture/reliable-stateful-service-architecture.png)

### <a name="stateful-reliable-service"></a>Állapot-nyilvántartó megbízható szolgáltatás

Állapot-nyilvántartó megbízható szolgáltatás StatefulService vagy StatefulServiceBase osztály is származhat. A következő alap osztályok szolgáltatás háló által nyújtott. Különböző szintű támogatási és az állapot-nyilvántartó szolgáltatás szolgáltatás háló – kommunikáljanak, és részt vehetnek a szolgáltatás háló fürt belül szolgáltatásként hardverabsztrakciós kínálnak.

StatefulService StatefulServiceBase származik. StatefulServiceBase szolgáltatásokat nyújtja rugalmasabb, de a szolgáltatás háló belső további megértéséhez szükséges.
Lásd: a [megbízható szolgáltatás – áttekintés](service-fabric-reliable-services-introduction.md) és a [megbízható szolgáltatás használatát speciális](service-fabric-reliable-services-advanced-usage.md) részletei határozzák meg a szolgáltatások leírás a StatefulService és StatefulServiceBase osztályok használatával további információt.

Mindkét alap osztályok élettartam és szolgáltatás végrehajtása szerepe kezelheti. A szolgáltatás végrehajtása felülbírálhatják vagy alap osztály virtuális módszerek, ha szolgáltatás végrehajtása a szolgáltatás végrehajtása életciklusa – ezek időpontokban elvégzendő munka, vagy ha azt szeretné, hogy hozzon létre egy kapcsolati figyelő objektumot. Figyelje meg, hogy bár a szolgáltatás megvalósítás saját kommunikációs figyelő objektum ki ICommunicationListener, a diagram a fenti hajthat végre a kapcsolati figyelő valósítható szolgáltatás háló – szolgáltatás végrehajtása használ szolgáltatási háló által végrehajtott kommunikációs figyelő.

Állapot-nyilvántartó megbízható szolgáltatás a megbízható állapot-kezelő használ, kihasználhatja megbízható gyűjtemények. Megbízható gyűjtemények erősen érhetők el a szolgáltatás – vagyis helyi adatstruktúrák, hogy mindig elérhetők, függetlenül attól, szolgáltatás feladatátadás. Különböző típusú megbízható webhelycsoport egy megbízható állam szolgáltató által történik.
Megbízható gyűjtemények kapcsolatos további tudnivalókért olvassa el a a [megbízható gyűjtemények – áttekintés](service-fabric-reliable-services-reliable-collections.md)című témakört.

### <a name="reliable-state-manager-and-state-providers"></a>Megbízható állapot-kezelő és állapot szolgáltatók

A megbízható állam felügyelője állam megbízható adatszolgáltatók kezelő az objektumot. A használható funkciók körét, létrehozása, törlése, számba veheti a webhely és biztosítása érdekében, hogy az állapot megbízható adatszolgáltatók állandó és könnyen hozzáférhető rendelkezik. Egy megbízható állam szolgáltató példány állandó és erősen elérhető adatstruktúra, például egy szótár vagy várólista egy példányának jelöli.

Minden megbízható állam szolgáltató közzététele a megbízható állam szolgáltató vezérléséhez állapot-nyilvántartó szolgáltatás által használt felületet. Például IReliableDictionary használják a megbízható szótárhoz, miközben IReliableQueue használatos felület felület és a megbízható várakozási sorban található. Minden megbízható állam szolgáltatók a IReliableState felület hajtja végre.

A megbízható állapot-kezelő IReliableStateManager, amely lehetővé teszi, hogy az access, az állapot-nyilvántartó szolgáltatás nevű felületet tartalmaz. A visszaadott állapot megbízható adatszolgáltatók felületek IReliableStateManager keresztül.

A megbízható állapot-kezelő a beépülő modul architektúra használ, úgy, hogy új típusú megbízható gyűjtemények is csatlakoztatható dinamikusan.

A megbízható szótár és megbízható várólista nagy teljesítményű, verziószámmal megkülönböztető áruház végrehajtásakor készültek.

### <a name="transactional-replicator"></a>Tranzakció alapú replikációs

A tranzakció alapú replikációs összetevője biztosítva, hogy egy (Ez azt jelenti, hogy a megbízható állam manager és a megbízható gyűjtemények belül állapot) szolgáltatás állapota egységes végig a szolgáltatást futtató összes másolatnál felelős. Is biztosítható, hogy az állapot a naplóban megőrződnek. A megbízható állam manager kapcsolódási pontok a tranzakció alapú replikációs egy privát eljárás keresztül.

A tranzakció alapú replikációs kommunikáció a többi szolgáltatás példány állapotában, hogy az összes másolatnál van naprakész-e állapotadatok hálózati protokollt használ.

A tranzakció alapú replikációs naplózási használja a probléma továbbra is fennáll állapot adatai, hogy az állapot adatai survives folyamat vagy összeomlik csomópontot. A napló a felületet egy privát eljárás található.

### <a name="log"></a>Napló

A napló összetevő biztosítja a nagy teljesítményű állandó tároló írásra optimalizált pörgő vagy félvezető lemezt.  A napló felépítésének nem kell az állapot-nyilvántartó szolgáltatást futtató csomópontok helyi az állandó tárolás (tehát merevlemezeken). Ez az alacsony késések és magas átviteli, mint távoli állandó tárolására, amely nem a csomópontra helyi lehetővé teszi.

A napló összetevő több naplófájlok használja. Egy egész csomópont megosztott naplófájl összes másolatnál használó, ezáltal a legalacsonyabb időtartama és a legmagasabb átviteli állam adatok tárolására van. Alapértelmezés szerint a megosztott napló bekerül a szolgáltatás háló csomópont munkakönyvtár, de azt is konfigurálhatók egy másik helyén, ideális esetben csak a megosztott napló fenntartott lemezen helyét. A szolgáltatás mindegyik kópiában van egy dedikált naplófájl is, és a dedikált naplófájl felépítése a szolgáltatás munka címtáron belül. Állítsa be a saját napló más helyen helyét semmilyen módszerrel nem.

A megosztott napló kiszolgáló a replika állapot adatai, egy átmeneti terület, míg a dedikált naplófájljának a rendeltetési, ahol állandó. Ebben a modellben az állapotinformációkat először a megosztott naplófájl írt, és majd lazily átkerül a dedikált naplófájl a háttérben. Ezzel a módszerrel a megosztott naplóban az írás szeretné, hogy a legalacsonyabb időtartama és a legmagasabb átviteli, amely lehetővé teszi, hogy gyorsabban halad, a szolgáltatás.

Beolvassa, és a megosztott napló ír végzett a lemezen a megosztott naplófájl előzetesen lefoglalt területre közvetlen IO keresztül. Ahhoz, hogy az optimális használata lemezterületet a dedikált naplók meghajtón, a dedikált naplófájl jön létre NTFS ritka fájlként. Ne feledje, hogy ez lehetővé teszi a lemezterület overprovisioning az operációs rendszer használata sokkal több szabad lemezterület ténylegesen használják, mint a dedikált naplófájlok jelennek meg.

A napló kívül a minimális felhasználói módú felület a napló, mint kernelmódú illesztőprogram rögzítésének. A napló kernelmódú illesztőprogram fut, biztosíthat a legmagasabb teljesítmény szolgáltatások használatához.

A napló beállításával kapcsolatos további tudnivalókért olvassa el a [Állapot-nyilvántartó megbízható szolgáltatások konfigurálása](service-fabric-reliable-services-configuration.md)című témakört.

## <a name="stateless-reliable-service"></a>Állapot nélküli megbízható szolgáltatás

### <a name="architecture-of-a-stateless-service"></a>Az állapot nélküli szolgáltatás architektúrája
![Állapot nélküli szolgáltatás architektúra ábrája](./media/service-fabric-reliable-services-platform-architecture/reliable-stateless-service-architecture.png)

### <a name="stateless-reliable-service"></a>Állapot nélküli megbízható szolgáltatás

Szolgáltatási állapot nélküli megvalósítás származtatása a StatelessService vagy StatelessServiceBase osztály. A StatelessServiceBase osztály lehetővé teszi a StatelessService osztály-nál nagyobb rugalmasság érdekében.
Mindkét alap osztályok leírási idő és a szolgáltatások szerepkör kezelheti.

Szolgáltatás végrehajtása felülbírálhatják vagy alap osztály virtuális módszerek, ha a szolgáltatás a szolgáltatás életciklusa – ezek időpontokban elvégzendő munka, vagy ha azt szeretné, hogy hozzon létre egy kapcsolati figyelő objektumot. Figyelje meg, hogy bár a szolgáltatást a saját kommunikációs figyelő objektum ki ICommunicationListener, a diagram a fenti hajthat végre a kapcsolati figyelő valósítható szolgáltatás háló, hogy szolgáltatás végrehajtásához használ szolgáltatási háló által végrehajtott kommunikációs figyelő.

Olvassa el a [megbízható szolgáltatás – áttekintés](service-fabric-reliable-services-introduction.md) és a [megbízható szolgáltatás speciális használatát](service-fabric-reliable-services-advanced-usage.md) , szolgáltatások használata az StatelessService és StatelessServiceBase osztályoknak írás részletei határozzák meg további információt.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Következő lépések

Szolgáltatás háló kapcsolatos további tudnivalókért lásd:

[Megbízható szolgáltatás áttekintése](service-fabric-reliable-services-introduction.md)

[Rövid útmutató](service-fabric-reliable-services-quick-start.md)

[Megbízható gyűjtemények – áttekintés](service-fabric-reliable-services-reliable-collections.md)

[Megbízható szolgáltatás használatát speciális](service-fabric-reliable-services-advanced-usage.md)

[Megbízható szolgáltatás konfigurálása](service-fabric-reliable-services-configuration.md)  
