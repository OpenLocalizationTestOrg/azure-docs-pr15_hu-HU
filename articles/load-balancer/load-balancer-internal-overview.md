
<properties
   pageTitle="Belső terheléselosztó áttekintése |} Microsoft Azure"
   description="A belső terheléselosztó és a szolgáltatások áttekintése. Hogyan működik a egy terheléselosztó Azure és lehetséges belső végpontok konfigurálása felhasználási területei"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />


# <a name="internal-load-balancer-overview"></a>Belső betöltés terheléselosztó – áttekintés

Nem kedvelik-e majd Internet szemben lévő terheléselosztó a belső terheléselosztó (ILB) irányítja a forgalmat csak az erőforrások a felhőbeli szolgáltatástól, vagy használja a virtuális Magánhálózati az Azure infrastruktúra eléréséhez. Az infrastruktúrára hozzáférésének korlátozására terheléselosztása virtuális IP-címei (VIP) egy felhőalapú szolgáltatásba vagy egy virtuális hálózaton, hogy azok program soha nem közvetlenül kitenni Internet zárólap. Ezzel a belső sor vállalati verzió (üzleti) alkalmazások futtatásához Azure-ban, és a felhő belül, illetve a helyszíni erőforrások érhetők el.

## <a name="why-you-may-need-an-internal-load-balancer"></a>Miért lehet szükség egy belső terheléselosztó

Azure belső betöltése terheléselosztás (ILB) között, amely egy felhőalapú szolgáltatásba vagy egy virtuális területi hatókör-hálózaton belül lakik virtuális gépeken futó terheléselosztási biztosít. Használata és konfigurálása a regionális hatókörrel virtuális hálózatok kapcsolatos további tudnivalókért lásd [Területi virtuális hálózatok](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) Azure a blogban. Meglévő virtuális hálózatok egy affinitás csoport konfigurált ILB nem használható.

ILB lehetővé teszi, hogy a következő típusú terheléselosztás:

- Egy felhőalapú szolgáltatásba, a virtuális gépeken futó használatával egy virtuális gépeken futó megjelenítő ugyanazt a felhőalapú szolgáltatást belül (lásd az ábrát 1).
- A virtuális hálózaton belül a virtuális gépeken futó a virtuális hálózaton használatával egy virtuális gépeken futó, amely a virtuális ugyanazt a felhőalapú szolgáltatást belül találhatók a hálózati (lásd az ábrát 2).
- Idegen helyszíni virtuális hálózati használatával egy virtuális gépeken futó felhőalapú ugyanezt a virtuális megjelenítő helyszíni számítógépekről (lásd az ábrát 3) hálózati.
- Internetes, a több szálon alkalmazások, amelyben a háttéradatbázist rétegek nem internetes, de terheléselosztási megkövetelése a internetes réteg érkező forgalmat.
- További terhelést terheléselosztó hardveres és szoftveres anélkül Azure-ban tárolt üzleti alkalmazások terheléselosztási. A helyszíni kiszolgálók többek között a számítógépek, amelyek a forgalom betöltés megadása meghatározni.

## <a name="internet-facing-multi-tier-applications"></a>Internetes több szálon futó alkalmazások


A webes réteg Internet szemben lévő végpontok internetes ügyfelek és részét terheléselosztás. A terheléselosztó elosztja a webkiszolgálóra webes ügyfelek, a porthoz TCP 443-as (HTTPS) érkező forgalmat.

Az adatbázis-kiszolgálók tároló kiszolgálót használó ILB zárólap mögött van. A adatbázis szolgáltatás terheléselosztás végpontot, mely forgalom betöltés ILB megadása az adatbázis-kiszolgálók között.

Az alábbi képen látható az interneten szemben lévő a több szálon alkalmazás belül ugyanazt a felhőalapú szolgáltatást.

Ábra: 1 – internetes több szálon alkalmazás

![Belső terheléselosztási egy felhőalapú szolgáltatásba](./media/load-balancer-internal-overview/IC736321.png)

Egy másik lehetséges a több szálon alkalmazás használata során a ILB rendszerbe, mint a szolgáltatást a ILB használata más különböző felhőszolgáltatásokba.

Cloud services használatáról virtuális hálózatához hozzáférést kap a ILB végpontot.

Ábra 2 kiszolgálók vannak az adatbázis háttéradatbázist és használata ugyanazon a virtuális hálózaton belül a ILB végpont valamelyik másik felhőszolgáltatásában előtér webes jeleníti meg.

![Belső terheléselosztási cloud services között](./media/load-balancer-internal-overview/IC744147.png)

Szám 2 - valamelyik másik felhőszolgáltatásában előtér-kiszolgálók

## <a name="intranet-line-of-business-applications"></a>Az üzleti alkalmazások intranetes sor

A helyszíni hálózaton ügyfelektől érkező forgalmat első terheléselosztás végig a hálózati Azure virtuális Magánhálózati kapcsolatot használó üzleti-kiszolgálók készlete.

Az ügyfélgép pont segítségével virtuális Magánhálózati webhely Azure virtuális Magánhálózati szolgáltatásból access kell az IP-cím. Lehetővé teszi a használja az üzleti alkalmazás, a ILB végpont mögött is.

![Belső terheléselosztás pont VPN webhely használata](./media/load-balancer-internal-overview/IC744148.png)

Ábra 3 – az LB végpontot mögött üzemeltetett üzleti alkalmazások

Egy másik forgatókönyv az üzleti, hogy a webhely virtuális Magánhálózattal kapcsolódik a virtuális hálózat, ahol a ILB végpont be van állítva. Ez lehetővé teszi, hogy a hálózati forgalmat a helyszíni lehet továbbítani az ILB végpontot.

![Belső terheléselosztás VPN webhely-webhely használata](./media/load-balancer-internal-overview/IC744150.png)

Ábra 4 – a helyszíni hálózati forgalmának engedélyezésére, a rendszer a ILB végpont


## <a name="next-steps"></a>Következő lépések

[Azure terheléselosztó Azure erőforrás-kezelő támogatása](load-balancer-arm.md)

[Első lépések az internetes terheléselosztó konfigurálása](load-balancer-get-started-internet-arm-ps.md)

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

