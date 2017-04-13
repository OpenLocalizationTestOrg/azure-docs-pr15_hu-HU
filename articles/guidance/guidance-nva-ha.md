<properties
   pageTitle="Virtuális készülékek magas elérhetősége üzembe helyezése |} Microsoft Azure"
   description="Telepítéséről hálózati virtuális készülékek magas állnak rendelkezésre."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Virtuális készülékek magas elérhetősége üzembe helyezése

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk a hálózati virtuális készülékek (NVAs) üzembe helyezése a könnyen hozzáférhető módon tanácsok halmazának ismertet. A folytatás előtt győződjön meg arról, hogy megismeri hogyan [felhasználói útvonalak (UDR)] [ udr-overview] és [terheléselosztó] [ lb-overview] munkahelyi Azure-ban.

Az Azure piactéren elérhető különböző NVAs segítségével készülékek használata a helyszíni adatközponthoz ugyanúgy Azure bővítése. Az alábbi ábrán egy minta telepítésének egy [egyetlen NVA] [ nva-scenario] , a tűzfal készülék. 

![[0]][0]

Bár az előző környezetben dolgozik, egyetlen hiba pont vezet be. Ha nem sikerül a virtuális készülék, nincs forgalmat. A probléma megoldásához kell több NVAs használja. Azonban is más beállítások és igénylő erőforrások használható attól függően, hogy az igényeknek megfelelően alakíthatja.

Egy könnyen hozzáférhető NVA környezetben üzembe használhatja egy, az alábbi tippekkel.

|Megoldás|Legfontosabb előny|Megfontolandó szempontok|
|---|---|---|
|A réteg 7 virtuális készülékek bejövő adatok|Az összes csomópontok aktívak.|Egy NVA, amely megszünteti a kapcsolatok és SNAT használható igényel<br/>Külön NVAs igényel az internetről, és az Azure érkező forgalmat<br/>Csak akkor használható Azure kívülről származó forgalmának beállítása|
|Bejövő adatok-kilépési a réteg 7 virtuális készülékek|Az összes csomópontok aktívak.<br/>Képes kezelni a forgalmat Azure-ban |Egy NVA, amely megszünteti a kapcsolatok és SNAT használható igényel<br/>Külön NVAs igényel az internetről, és az Azure érkező forgalmat|
|Váltás a MEZŐPONT-UDR|Minden forgalom NVAs egyetlen csoportja<br/>Képes kezelni az összes forgalom (nincs korlátozás port szabályokról)|Aktív-passzív<br/>Egy feladatátvételi folyamat igényel|

## <a name="ingress-with-layer-7-virtual-appliances"></a>A réteg 7 virtuális készülékek bejövő adatok
NVAs az Azure terheléselosztó mögött készlete segítségével csatlakozhasson Azure munkaterhelésekből kiszolgálóoldali portokat (például a HTTP- és HTTPS) kis készlete mögött. Az alábbi ábrán kiemeli hogyan magas rendelkezésre állás ebben az esetben a NVA szintjén.

![[1]][1]

Ebben az esetben a hálózati virtuális készülék használt kell megszünteti a minden kapcsolat lehetőséget, és adja meg azokat a terhelést alhálózathoz. A terhelési virtuális gépeken futó (VMs) megválaszolása a NVA kérése és forgalom problémamentes kaptak. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Bejövő adatok-kilépési a réteg 7 virtuális készülékek
Kiterjesztése az előző architektúrája, és egy másik NVAs származó Azure NVAs, kezeli az alábbi ábrán látható módon forgalom kezelésére-készletek hozzáadása:

![[2]][2]

Ebben az esetben az Azure származó minden forgalom küldött egy belső terheléselosztó elosztja a betöltés NVAs más beállítási között. Ezek a NVAs közvetlen forgalom az internethez, azok egyéni nyilvános IP-cím használatával. 

## <a name="pip-udr-switch"></a>Váltás a MEZŐPONT-UDR
Elkerülheti, hogy több NVA készleteket létrehozása két NVAs a aktív-passzív módban. Ebben az esetben válthat a nyilvános IP-cím (MEZŐPONT) és a felhasználó által definiált útvonalak (UDRs) Ha megszünteti a aktív csomópontot.  

![[3]][3]

Ebben az esetben hasonlít az egyetlen NVA alkalmazási példát. Az egyetlen különbség, hogy a MEZŐPONT és UDRs módosítani kell a NVAs forgalmát válthat. Ezek a változások manuálisan kell elvégezni, illetve is automatizálni őket. Automatizálható, mindkét NVAs az alkalmazás, amely ellenőrzi, hogy az aktív csomópont állapotának telepítheti. Az aktív csomópont nem működik, ha az alkalmazás MEZŐPONT és a passzív csomópontra, amelyre a hivatkozás UDRs módosíthatja.

A megoldás egy lehetséges alkalmazásának környezetbe a [ZooKeeper] [ zookeeper] vezető election (el, hogy melyik csomópont aktív csomópontjának) kezelése a NVAs démont. Miután kijelölt egy vezető, hívja az Azure REST API-hoz a MEZŐPONT eltávolítása a sikertelen csomópontot, és csatolja a vezetőnek. Mutasson az új vezető saját IP-cím UDRs is azt kell módosíthatja.

## <a name="next-steps"></a>Következő lépések

- Ismerje meg, [a DMZ Azure és a helyszíni adatközponthoz között] megvalósításáról[ dmz-on-prem] réteg-7 NVAs használatával.
- Ismerje meg, [a DMZ Azure és az Internet között] megvalósításáról[ dmz-internet] réteg-7 NVAs használatával.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Egyetlen NVA architektúra"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Réteg 7 bejövő adatok"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Réteg 7 be- és kilépési"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktív-passzív fürthöz"