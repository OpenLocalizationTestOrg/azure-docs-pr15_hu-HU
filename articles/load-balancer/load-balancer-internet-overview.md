
<properties
   pageTitle="Internetes betöltés terheléselosztó áttekintése |} Microsoft Azure "
   description="Az internetes terheléselosztó és szolgáltatásai áttekintése. Egy terheléselosztó működése az Azure virtuális gépeken futó és a felhőbeli szolgáltatások használatával."
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


# <a name="internet-facing-load-balancer-overview"></a>Internetes szemben lévő betöltés terheléselosztó – áttekintés

Azure terheléselosztó magánjellegű IP-címet és a portszámot számát a virtuális gép, és fordítva a válasz forgalmat a nyilvános IP-cím és port száma bejövő forgalmat a virtuális számítógépről rendeli. Betöltési terheléselosztó szabályok forgalom virtuális gépeken futó vagy a szolgáltatások között adott típusú terjesztése teszi lehetővé. Több webkiszolgálón vagy web szerepkörök például is húzza szét a betöltés webes kérelem forgalmat.

A szolgáltatás definition (.csdef) fájl egy felhőalapú szolgáltatásba, amely tartalmazza a webes szerepkörök vagy dolgozó szerepkörök előfordulását, megadhatja az egy nyilvános végpontot.

A _servicedefinition.csdef_ fájl tartalmaz a végpont konfigurációját, és ha webes vagy dolgozó szerepkör telepítés több szerepkör-példányok, a terheléselosztó, a telepítő. A módszer hozzáadja az a felhő üzembe a szolgáltatás konfigurációs fájl (.csfg) példány számának megváltoztatása.

Az alábbi ábrán egy három virtuális gépeken futó portszámként nyilvános és titkos TCP 443 közötti megosztott titkosított webes forgalmához terheléselosztás végpontot. Következő három virtuális gépeken futó szerepelnek, a terheléselosztás beállítása.

![nyilvános betöltés terheléselosztó példa](./media/load-balancer-internet-overview/IC727496.png))

Szám 1 - terheléselosztás végpontot titkosított internetes forgalmat a

Amikor internetes ügyfelek a weblap kérést küld a felhőalapú szolgáltatást a TCP 443-as port nyilvános IP-címét, a az Azure betöltése terheléselosztó elosztja a kérések között a három virtuális gépeken futó terheléselosztás megadása. Betöltési terheléselosztó algoritmusok kapcsolatos további tudnivalókért olvassa el a [terheléselosztó Áttekintés lap töltse be](load-balancer-overview.md#load-balancer-features)című témakört.

Alapértelmezés szerint az Azure terheléselosztó elosztja a hálózati forgalmának engedélyezésére egyaránt több virtuális gép példányok között. Munkamenet affinitás, további információért lásd: [Betöltés terheléselosztó terjesztési mód](load-balancer-distribution-mode.md)is beállítható.

## <a name="next-steps"></a>Következő lépések

Tudnivalók a [belső terheléselosztó](load-balancer-internal-overview.md) megértéséhez, mely terheléselosztó jobban megfelelnek a felhőben példányban.

Is [első lépések megtételében egy internetes terheléselosztó](load-balancer-get-started-internet-arm-ps.md) és [terjesztési mód](load-balancer-distribution-mode.md) egy adott betöltése terheléselosztó hálózati forgalmának engedélyezésére elemeire vonatkozó típusának beállítása.

Ha az alkalmazás hagyja kapcsolat életben mögött egy terheléselosztó kiszolgálók, akkor is többet szeretne tudni [a terheléselosztó üresjárati TCP időtúllépés beállításainak](load-balancer-tcp-idle-timeout.md). Tudnivalók az Azure terheléselosztó használatakor üresjárati kapcsolati viselkedés segítik.
