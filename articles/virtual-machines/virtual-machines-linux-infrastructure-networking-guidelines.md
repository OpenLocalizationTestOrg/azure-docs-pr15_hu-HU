<properties
    pageTitle="Hálózati infrastruktúrát irányelvek |} Microsoft Azure"
    description="Tudjon meg többet a fontos tervezéséhez és kivitelezéséhez alapelve üzembe helyezése a Azure infrastruktúrájának szolgáltatásai virtuális hálózat."
    documentationCenter=""
    services="virtual-machines-linux"
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2016"
    ms.author="iainfou"/>

# <a name="networking-infrastructure-guidelines"></a>Hálózati infrastruktúrát útmutató

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)] 

Ez a cikk koncentrál ismertetése a szükséges tervezési lépéseket belül Azure virtuális hálózati és a csatlakozási meglévő helyszíni környezetekkel között.


## <a name="implementation-guidelines-for-virtual-networks"></a>Virtuális hálózatok végrehajtása szabályai

Határozatok:

- Milyen típusú virtuális hálózathoz van szüksége az informatikai terhelést vagy infrastruktúra szeretné üzemeltetni (felhőben vagy határokon helyszíni)?
- Idegen helyszíni virtuális hálózatok mennyi címterület használatára van szüksége az alhálózathoz és VMs tárolni, most és a jövőben lehetővé teszi kiterjesztett?
- Lesz a központi virtuális hálózatok vagy az egyes erőforráscsoport egyes virtuális hálózatok létre?

Feladatok:

- Adja meg a létrehozandó virtuális hálózatok címterület.
- A definiálhatja alhálózat és a címterületet minden egyes.
- Idegen helyszíni virtuális hálózatok a definiálhatja a VMs a virtuális hálózaton olyasvalakivel kell a helyszíni helynek helyi hálózaton cím szóközt.
- Munka a helyszíni annak érdekében, hogy a megfelelő útválasztás hálózati csoport van beállítva, amikor létrehozása határokon helyszíni virtuális hálózatok.
- A virtuális hálózatot az elnevezésük is egységes hozhat létre.


## <a name="virtual-networks"></a>Virtuális hálózatok

Virtuális hálózatok szükség a támogatási virtuális gépeken futó (VMs) közötti kommunikáció. Alhálózat, az egyéni IP-cím, a DNS-beállítások, a biztonsági szűrés határozza meg, és terheléselosztás, fizikai hálózatok együtt. [Virtuális Magánhálózati átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md) vagy [Express útvonal áramkör](../expressroute/expressroute-introduction.md)használatával csatlakozhat Azure virtuális hálózatok a helyszíni hálózatok. Erről további tudnivalók a [virtuális hálózatok és összetevőik](../virtual-network/virtual-networks-overview.md).

Erőforrás-csoportok használatával, ha a virtuális hálózati összetevők felépítésétől rugalmasság. VMs csatlakoztathatja virtuális hálózatok kívül saját erőforráscsoport. Közös tervezés megközelítést lenne a alapvető hálózati infrastruktúrájába közös csapatnak kezelhető tartalmazó központi erőforrás csoportokat hozhat létre. VMs és az alkalmazásaikat külön erőforráscsoport rendszerbe állított. Ezt a megközelítést lehetővé teszi, hogy az alkalmazás a tulajdonosok hozzáférést az erőforráscsoport, amely tartalmazza a hajómegfigyelési virtuális szélesebb hálózati erőforrásokat konfigurációja elérésének megnyitása nélkül.

## <a name="site-connectivity"></a>Webhely-adatkapcsolat

### <a name="cloud-only-virtual-networks"></a>Felhőben virtuális hálózatok
Ha helyszíni felhasználóit és a számítógépen nincs szükség a folyamatban lévő kiszolgálóhoz való csatlakozás VMs az Azure virtuális hálózat, a virtuális hálózattervezés átirányítása közvetlenül van:

![Egyszerű felhőben virtuális hálózatdiagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet01.png)

Ezt a megközelítést a szokásos internetes munkaterhelésekből, például az Internet alapú webkiszolgáló nem. Ezek a SSH vagy a webhely-pont virtuális Magánhálózati kapcsolat VMs kezelheti.

Mivel azok ne csatlakozzon a helyszíni hálózaton, csak Azure virtuális hálózatok használhatja az IP-magánjellegű címterület bármely részét. A címterület lehet a azonos magánjellegű szóköz használata a helyszíni lévő.


### <a name="cross-premises-virtual-networks"></a>Idegen helyszíni virtuális hálózatok
Ha helyszíni felhasználóit és számítógépek kér a folyamatban lévő kiszolgálóhoz való csatlakozás VMs az Azure virtuális hálózat, hozzon létre virtuális határokon helyszíni hálózaton. Csatlakozás a virtuális hálózat készült ExpressRoute vagy a webhely virtuális Magánhálózati kapcsolat a helyszíni hálózaton.

![Idegen helyszíni virtuális hálózatdiagram](./media/virtual-machines-common-infrastructure-service-guidelines/vnet02.png)

Ebben a konfigurációban a Azure virtuális hálózat akkor lényegében egy felhőalapú kiterjesztése a helyszíni hálózaton.

A helyszíni hálózaton csatlakoznak, mert határokon helyszíni virtuális hálózatok kell használnia az egyedi a szervezet által használt címterület egy részét. Különböző vállalati helyek a meghatározott IP-alhálózat hozzárendelt ugyanúgy Azure szerint meg a hálózat kibővíti lesz egy másik helyre.

Ahhoz, hogy a helyszíni hálózaton felkeresniük a kereszt helyszíni virtuális hálózatról csomagok, meg kell adnia a megfelelő helyszíni cím prefixumokban készlete a helyi hálózaton meghatározása a virtuális hálózat részeként. Virtuális hálózat a címterület és a helyszíni megfelelő helyekre, beállításától függően is, hogy sok cím prefixumokban a helyi hálózaton.

Felhőben virtuális hálózati átalakítása határokon helyszíni virtuális hálózatot, de nagy valószínűséggel van szüksége, az IP-re a virtuális hálózati Címterület és Azure erőforrások. Ezért körültekintően fontolja meg egy virtuális hálózati kell a helyszíni hálózaton csatlakozik, ha egy IP-alhálózat rendeli.

## <a name="subnets"></a>Alhálózat
Alhálózat lehetővé teszi információforrások, amelyek kapcsolódnak, rendezheti vagy logikailag (például egy alhálózat az ugyanazt a kérelmet társított VMs), vagy fizikailag (például az erőforrás csoportonként egy alhálózat). Akkor is alkalmazhat, a nagyobb biztonság alhálózat elkülönítési technikákat alkalmaz.

Idegen helyszíni virtuális hálózatok tervezheti az azonos szabályokat, a helyszíni erőforrások használó alhálózat kell. **Azure mindig használja a címterületet minden alhálózathoz első három IP-címét**. Annak megállapításához, a alhálózat szükséges címek számát, először megszámlálása a VMs most már van szükség. A jövőbeli növekedés becslése, és az alábbi táblázat segítségével határozza meg az alhálózathoz méretét.

A szükséges VMs száma | A host eltolással szükséges | Az alhálózathoz mérete
--- | --- | ---
1 – 3 | 3 | / 29
4 – 11     | 4 | / 28
12 – 27 | 5 | / 27
28 – 59 | 6 | / 26
60 – 123 | 7 | / 25

> [AZURE.NOTE] Normál helyszíni alhálózat n host bittel alhálózat host címek maximális száma 2<sup>n</sup> – 2. Az Azure alhálózat maximális száma n host bittel alhálózat host címeinek 2<sup>n</sup> – 5 (2 és 3 Azure használó minden alhálózathoz címek).

Ha úgy dönt, hogy egy alhálózat mérete, amely túl kicsi, kell az IP-re, és telepítsen újra a VMs az alhálózathoz a.


## <a name="network-security-groups"></a>Hálózati biztonsági csoportok
Alkalmazhat szűrési szabályok folyik át a forgalmat a virtuális hálózatok között hálózati biztonsági csoportok használatával. Készíthet a virtuális hálózati környezet biztonságos részletes szűrési szabályokat vezérlése a bejövő és kimenő forgalmának engedélyezésére, a forrás és a cél IP-tartományokat, engedélyezett portokat stb. Hálózati biztonsági csoportokat is alkalmazható, alhálózathoz virtuális hálózaton belül vagy közvetlenül egy adott virtuális hálózati kapcsolat. Ajánlott bizonyos hálózati biztonsági csoport forgalmat a virtuális hálózatokon szűrő szinttel rendelkezik. Erről további tudnivalók a [Hálózati biztonsági csoportok](../virtual-network/virtual-networks-nsg.md).


## <a name="additional-network-components"></a>További hálózati összetevők
Egy helyszíni fizikai hálózati infrastruktúrát, az Azure virtuális hálózat nem egyszerűen csak alhálózat és IP-címek tartalmazhat. Az alkalmazás infrastruktúra tervezésekor, érdemes, amelyekkel beépíthetők a következő további összetevőit részét:

- [Átjárók VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) - Azure virtuális hálózatok csatlakoztatása más Azure virtuális hálózatok, vagy a helyszíni hálózatokhoz VPN webhely kapcsolaton keresztül. Express útvonal kapcsolatok dedikált, biztonságos kapcsolatok végrehajtása. Felhasználók közvetlen hozzáférést biztosít a pont-webhely VPN kapcsolatokkal is.
- Tetszés szerint belső és külső forgalmához forgalom terheléselosztás [terheléselosztó](../load-balancer/load-balancer-overview.md) - biztosít.
- [Alkalmazás átjáró](../application-gateway/application-gateway-introduction.md) – az alkalmazás rétegben terheléselosztási, néhány további előnyökkel jár kezeléséről a webalkalmazások telepítése helyett az Azure HTTP terheléselosztó.
- [Adatforgalom Manager](../traffic-manager/traffic-manager-overview.md) - DNS-alapú forgalmat terjesztési irányítsa át a legközelebbi rendelkezésre álló végpont végfelhasználóknak lehetővé teszi az alkalmazás különböző régiókban Azure adatközpontokkal kívül tárolni.


## <a name="next-steps"></a>Következő lépések

[AZURE.INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)] 