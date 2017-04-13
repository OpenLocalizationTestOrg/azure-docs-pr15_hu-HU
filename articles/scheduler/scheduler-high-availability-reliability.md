<properties
 pageTitle="A Feladatütemező magas rendelkezésre állás és megbízhatóság"
 description="A Feladatütemező magas rendelkezésre állás és megbízhatóság"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>A Feladatütemező magas rendelkezésre állás és megbízhatóság

## <a name="azure-scheduler-high-availability"></a>Azure ütemező magas rendelkezésre állás

Alapvető Azure platform szolgáltatás Azure ütemező könnyen hozzáférhető és geo felesleges szolgáltatás telepítése és a feladat geo-területi replikációs szolgáltatásait.

### <a name="geo-redundant-service-deployment"></a>GEO felesleges szolgáltatás telepítése

Azure ütemező a felhasználói felület Azure-ban van még ma szinte minden geo régióban keresztül érhető el. Azure ütemező érhető el a régiójában [az itt felsorolt](https://azure.microsoft.com/regions/#services). Egy olyan szolgáltatott régióban adatközpont leképezésének módját nem érhető el, a Azure ütemező feladatátvevő funkcióinak is, hogy a szolgáltatás érhető el a másik adatközpont.

### <a name="geo-regional-job-replication"></a>A területi GEO feladat replikációs

Nemcsak az előtér-kezelés kérések, de a saját feladatok érhető el akkor is geo replikált az Azure ütemezőt. Ha egy tartományban lévő egy üzemszünetek, Azure ütemező átvétele, és biztosíthatja, hogy a feladat futtatása a páronkénti földrajzi területhez tartozik egy másik adatok központból.

Például ha létrehozott egy feladatot a déli központi US, Azure ütemező automatikusan replikálja Észak központi Velünk a feladat. A Dél központi US hibát tartalmaz, a Azure ütemező biztosítja, hogy a feladat futtatása a Észak központi US. 

![][1]

Emiatt Azure ütemező biztosítja, hogy az adatok lépje túl a ugyanabban a szélesebb földrajzi régióban Azure hiba esetén. Eredményt adja meg kell nem ismétlődő csak magas elérhetősége hozzáadni a feladatok – Azure ütemező automatikusan biztosít a feladatok magas rendelkezésre állás funkciókhoz.

## <a name="azure-scheduler-reliability"></a>Megbízhatósága Azure ütemező

Azure ütemező saját magas elérhetősége garantálja, és egy másik megközelítés felhasználó által létrehozott feladatok időt vesz igénybe. Például a feladatok alkalmazhatja a HTTP-zárólap, amely nem érhető el. Azure ütemező mindazonáltal próbálja meg végrehajtani a feladat sikeresen, azzal, hogy meg kell foglalkozni az hiba alternatív beállítások. Azure ütemező végzi két módon:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigurálható újra házirend "retryPolicy" keresztül

Azure ütemező lehetővé teszi, hogy ismét házirend konfigurálása. Alapértelmezés szerint egy feladat sikertelen lesz, ha a Feladatütemező próbálja a feladat újra további négyszer 30 másodperces időközönként. Előfordulhat, hogy újra állítsa be újra a házirend csak akkor kell több olyan szigorú (például tíz időpontok 30 másodperces időközönként) vagy lazább (például kétszer, napi időközönként.)

Szerepel példaként, amikor ez segíthet a hozhatja létre, hetente egyszer elindul, és elindítja a HTTP zárólap feladatot. Ha a HTTP-végpont le néhány órát, ha a feladat futtatása, előfordulhat, hogy nem szeretné Várjon egy további hét futtassa újra, mivel a még az alapértelmezett házirend kísérletek sikertelenek lesznek a feladathoz. Ezekben az esetekben előfordulhat, hogy átkonfigurálása helyett 30 másodpercenként a szabványos újrapróbálkozási házirendet, az újbóli próbálkozáshoz három óránként (például).

Megtudhatja, hogy miként újrapróbálkozási házirend beállításával, olvassa el a [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternatív végpont konfigurálhatósági "errorAction" keresztül

Az Azure ütemező projektre vonatkozóan a cél végpont marad nem érhető el, ha Azure ütemező visszavált az alternatív hibakezelő végpontot újrapróbálkozási házirendje leírt lépések végrehajtását követően. Ha másik hibakezelő zárólap be van állítva, az Azure ütemező elindítja. Az alternatív zárólap a saját feladatok érhetők el erősen hiba szemben.

Szerepel példaként, az alábbi ábrán az Azure ütemező követi a New York webszolgáltatás találati újrapróbálkozási házirend. Nem sikerül a próbálkozások, miután ellenőrzi, van-e egy másik. Ezután Ugrás a következő, és elindítja az alternatív azonos újrapróbálkozási házirend kérések végez.

![][2]

Figyelje meg, hogy az azonos újrapróbálkozási házirend vonatkozik-e az eredeti művelet és a művelet az alternatív hiba esetén. Az is lehetséges, hogy a fő művelet művelettípus eltérő lehet a másodlagos hiba művelet művelettípus. Például a fő művelet előfordulhat, hogy HTTP zárólap meghívása kell, amíg a hiba művelet inkább lehet tároló várólista, szolgáltatás bus várólista, vagy hiba naplózás fejlesztő bus témakör művelet.

Alternatív zárólap beállításáról információért olvassa el a [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Lásd még:

 [Mi az a Feladatütemező?](scheduler-intro.md)

 [Azure ütemező fogalmak, kifejezések és entitás hierarchia](scheduler-concepts-terms.md)

 [Az Azure-portálon a Feladatütemező használatának első lépései](scheduler-get-started-portal.md)

 [Tervek és a számlázásra az Azure ütemező](scheduler-plans-billing.md)

 [Összetett ütemterveket és a Azure ütemezővel speciális ismétlődés létrehozásának](scheduler-advanced-complexity.md)

 [Azure ütemező REST API-hivatkozás](https://msdn.microsoft.com/library/mt629143)

 [Azure ütemező PowerShell-parancsmagok hivatkozás](scheduler-powershell-reference.md)

 [Azure ütemező korlátozások, alapértelmezett és hibakódok esetén](scheduler-limits-defaults-errors.md)

 [Azure ütemező kimenő hitelesítést](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
