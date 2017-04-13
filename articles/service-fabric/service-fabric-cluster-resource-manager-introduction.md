<properties
   pageTitle="A szolgáltatás háló fürt erőforrás-kezelő bemutatása |} Microsoft Azure"
   description="Bevezetés a a szolgáltatás háló fürt erőforrás-kezelő."
   services="service-fabric"
   documentationCenter=".net"
   authors="masnider"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/19/2016"
   ms.author="masnider"/>

# <a name="introducing-the-service-fabric-cluster-resource-manager"></a>A szolgáltatás háló fürt erőforrás-kezelő bemutatása
Hagyományos kezelése az IT-rendszerek vagy szolgáltatások első néhány fizikai vagy egyes szolgáltatások vagy rendszerek dedikált virtuális gépeken futó érteni. Sok fő szolgáltatás volt a "webhely" réteg és bontásban egy "adatok" vagy "tároló" réteg esetleg néhány más speciális elemekkel, például a gyorsítótár. Más típusú alkalmazások szeretné, hogy hol érkezett kérelmek beadását és kivételét, egy üzenetben réteg csatlakozik egy munka réteg bármely analysis vagy átalakítási szükséges az üzenetkezelés részeként. Különböző típusú terhelést kitűzött célja, hogy egy adott gépek használ: az adatbázis gépe van hozzárendelve, a webkiszolgáló kevés a néhány gépek. Ha egy bizonyos típusú terhelést a gép okozott futtatásához túl nagy a volt, majd a további gépeken futó hozzáadja az adott típusú rajta futtatására beállított terhelést, vagy nagyobb gépek gépek néhány cseréli. Egyszerű. Nem sikerült egy számítógépre, az alkalmazás általános azt a részét alsó kapacitással futtatta mindaddig, amíg a gép lehetett visszaállítani. Továbbra is meglehetősen egyszerű (Ha nem feltétlenül szórakoztató).

Most azonban érdemes mondjuk megtalálta méretezése és a tárolók és/vagy microservice kapcsolatos beszerzéseké, s léptek kell. Váratlanul megtalálhatja saját maga tízesre, több száz vagy akár több ezer gépek, különböző szolgáltatások, esetleg különböző példányok használják ezeket a szolgáltatásokat, minden egyes példányok vagy a nagy elérhetőség (HA) kópiák több száz tucatnyi.

Váratlanul kezelése a környezettől nem így egyszerűen munkaterhelésekből egyetlen típusú hozzárendelve néhány gépek kezelése. A kiszolgáló virtuális, és nem kell többé nevek (, *van* a [Hobbiállatok szarvasmarha való](http://www.slideshare.net/randybias/architectures-for-open-and-scalable-clouds/20) mindsets után átváltott). A beállítás kisebb, a gép és további tudnivalók a szolgáltatások magukat. Hardveres már a múlté, és szolgáltatások kis elosztott rendszerek, több kisebb hardvereszközöket örvend hardver tartó váltak.

Következtében a korábban egységes, többszintű alkalmazás megszakítása örvend hardver futó szolgáltatások külön be, ekkor számos további kombinációk foglalkozik. Kik úgy dönt, hogy milyen típusú munkaterhelésekből futtatását is lehetővé teszi a hardver, illetve hány? Mely feladatok is jól működik az azonos hardver, és amelyek ütköznek? Ha egy számítógépre megszakad... Mi is futtatott van? Ki van felelős gondoskodhat arról, hogy az adott terhelést elindítja az ismételt futtatása? Várakozás az illető visszatér (ezek olyan virtuális?) gép vagy végezze el a munkaterhelésekből automatikusan átadni más gépek és megőrzése fut? Az emberi beavatkozás szükséges? Mit kell tudni az ilyen jellegű környezetben frissítések?

A fejlesztők és a rendezés a világ élő operátorok összetettség kezelése segítségre megyünk, és az értelemben, hogy a humánerőforrás binge és összetettségétől fölé személyekkel papír próbál nem jelenik meg a megfelelő választ kaphat.

Mi a teendő?

## <a name="introducing-orchestrators"></a>Orchestrators bemutatása
"Orchestrator" általános kifejezés, amely segítséget nyújt a rendszergazdáknak környezetek az ilyen típusú kezelése szoftver e-. Orchestrators a összetevői, például "Szeretném ezt a szolgáltatást futtató saját környezetben 5 példányainak" kérések viselő, igaz, így azok majd (próbálja) maradjon is úgy hogy.

(Nem az emberek) orchestrators, mi éppen működésbe, amikor egy számítógépre nem sikerül vagy terhelési megszakítja a váratlan valamiért. A legtöbb Orchestrators nem egyszerűen csak foglalkozik hiba, például segítséget nyújt az új telepítések, frissítések kezelése és erőforrás-felhasználás kezelése, de az összes alapvetően kapcsolatos néhány kívánt a környezet konfigurációja állapotának fenntartása. Egy Orchestrator állapítható meg, hogy mit szeretné, és végezze el a nehéz emelő már engedélyezni kívánt. Chronos Marathon Mesos, flotta, méhrajt, Kubernetes és szolgáltatás háló fölött Orchestrators összes ábrázolnak (vagy beépített azokat). További hoznak létre a különböző környezetekben valós telepítések kezelése a bonyodalmainak egyidejűleg és feltételek a nagyobb és módosítása.

## <a name="orchestration-as-a-service"></a>Szolgáltatásként üzletifolyamat-tervező
A feladat szolgáltatás háló fürt belül a Orchestrator elsősorban a fürt erőforrás-kezelő kezeli. A szolgáltatás háló fürt erőforrás-kezelő egyike, a rendszer Services szolgáltatás háló belül, és belül minden fürt automatikusan elindul.  Általánosságban elmondható az erőforrás felülete feladat bontani három részből áll:

1. Érvényesítési szabályok
2. A környezet optimalizálása
3. Másik folyamat során

### <a name="what-it-isnt"></a>Ez nem
A hagyományos N réteg web apps történt mindig egy "terheléselosztó", általában egy hálózati betöltés terheléselosztó Terheléselosztási vagy nevezik-alkalmazás betöltés terheléselosztó (ALB) attól függően, hogy hol kép a hálózati egymást fedő néhány állomástól. Néhány terheléselosztókkal jelennek meg az F5 BigIP felületek alapú hardver, mások alapján, például a Microsoft szoftveres Terheléselosztási. Más környezetekben a szerepkör a megjelenhet HAProxy hasonló. Az alábbi architektúrákban a feladat terheléselosztás bizonyosodhat meg arról, hogy összes a különböző állapot nélküli előtér gépek vagy a különböző számítógépeken a fürt (nagyjából) munka ugyanannyi. Stratégiák a változatos, minden más hívást egy másik kiszolgálóra küldjenek munkamenet rögzítése/ragadósság, a tényleges becslés és a hívás terhelés várható költség és a jelenlegi gépi betöltés alapján.

Megjegyzendő, hogy ez a legjobb volt a mechanizmusa annak biztosítására, hogy a webes réteg nagyjából meghatározni. Stratégiák a adatok réteg terheléselosztás voltak teljesen különböző és a függő adatokat tároló mechanizmusa, általában középre igazítása körüli adatok sharding, gyorsítótárazás, felügyelt adatbázis nézetek és tárolt eljárások, stb.

Ezek stratégiák érdekes, miközben a szolgáltatás háló fürt erőforrás-kezelő nincs bármi például egy hálózati terheléselosztó vagy a gyorsítótár. A hálózati betöltése terheléselosztó biztosítja készíteni-e a rendszer, hogy a első végét áthelyezése a forgalmat a szolgáltatásokat futtató, hogy a, miközben a szolgáltatás háló fürt erőforrás-kezelő alapvetően megnyitja az teljesen más stratégia –, a szolgáltatás háló *szolgáltatások* lép Ha már stimmelnek a legtöbb (és vár a forgalmat, vagy hajtsa végre a betöltés). Ez lehet például csomópontokat, amelyeknek vannak jelenleg hideg, mert a szolgáltatások, amelyek van még nem teljesítenek a munka sok pillanatban, illetve amelyek törölni vagy máshol áthelyezve. Másik példa a fürt erőforrás-kezelő is sikerült az szolgáltatásai, amelyek voltak futó csatlakozik egy számítógépre, amely készül, hogy frissíteni fogják, vagy amelyek következtében a Nyárs a felhasználás túl van terhelve szolgáltatás. Mivel az erőforrás-kezelő fürt felelős a szolgáltatások (nem kézbesítése hálózati forgalmának engedélyezésére attól függ, ahol szolgáltatások vannak-e már) körül áthelyezése, amelyben az lényegesen eltér a funkció a egy hálózati terheléselosztó volna felfedése, és annak biztosítására, hogy a fürt hardver-erőforrások is használhatók alapvetően különböző stratégiák alkalmaz.

## <a name="next-steps"></a>Következő lépések
- A architektúrája és információk folyamat belül az erőforrás-kezelő tudnivalókért olvassa el [Ez a cikk](service-fabric-cluster-resource-manager-architecture.md)
- Az erőforrás-kezelő fürt túl sok a fürt leíró lehetőséget. Megtudhatja, hogy több róluk nézze meg a [szolgáltatás háló fürt ismertető](service-fabric-cluster-resource-manager-cluster-description.md) cikkben
- Az elérhető lehetőségekről további információt a témakör nézze át az erőforrás-kezelő fürt konfigurációk Services elérhető [szolgáltatások beállításával kapcsolatos további tudnivalók](service-fabric-cluster-resource-manager-configure-services.md)
- Mértékek, hogyan kezeli az szolgáltatás háló fürt erőforrás-kezelő felhasználás és a fürt kapacitása. Ha tudni szeretné őket, és hogyan konfigurálhatók többet tanulmányozza [Ez a cikk](service-fabric-cluster-resource-manager-metrics.md)
- Az erőforrás-kezelő fürt működik-e a szolgáltatás háló kezelési lehetőségeit. Ha meg szeretne tudni arról, hogy integráció, olvassa el a [Ez a cikk](service-fabric-cluster-resource-manager-management-integration.md)
- Megtudhatja, hogyan fürt az erőforrás-kezelő kezeli, és a fürt betöltés egyenlege, olvassa el a cikk a [terheléselosztás betöltése](service-fabric-cluster-resource-manager-balancing.md)
