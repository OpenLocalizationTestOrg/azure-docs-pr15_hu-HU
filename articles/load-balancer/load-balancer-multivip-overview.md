<properties
   pageTitle="Az Azure több VIP terheléselosztó |} Microsoft Azure"
   description="A Azure terheléselosztó több VIP áttekintése"
   services="load-balancer"
   documentationCenter="na"
   authors="chkuhtz"
   manager="narayan"
   editor=""
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="chkuhtz"
/>

# <a name="multiple-vips-for-azure-load-balancer"></a>Az Azure több VIP terheléselosztó

Azure betöltése terheléselosztó több portok vagy több IP-címek egyenleg szolgáltatások teszi lehetővé. Nyilvános és belső betöltés terheléselosztó definíciók egyenleg flow betölteni egy sor olyan VMs keresztül is használhatja.

Ez a cikk ismerteti a alapjait, ez azt jelenti, lényeges koncepciót is bemutat és kényszerek. Ha csak szeretné elérhetővé tenni a szolgáltatások egy IP-címet, található egyszerűsített útmutató [nyilvános](load-balancer-get-started-internet-portal.md) és [belső](load-balancer-get-started-ilb-arm-portal.md) betöltése terheléselosztó konfigurációk. Több VIP felvétele az egyetlen virtuális konfigurációja egyre növekvő tendenciát mutat. Ez a cikk a fogalmak használ, a bármikor egyszerűsített konfiguráció elemre.

Az Azure terheléselosztó határozza meg, amikor a időtúllépést és a kódmentes konfiguráció kapcsolódnak szabályokat. Az állapot vizsgálati a szabály által hivatkozott azt határozza meg, hogy új flow küldjük kódmentes készletben csomópontra. A frontend által egy virtuális IP (virtuális), amely verzióban lévő IP-cím (nyilvános vagy belső), a protokoll (UDP vagy TCP) és a portszámot 3-rekordhoz van meghatározva. Egy DIP az Azure virtuális hálózati csatolt kódmentes készletben egy virtuális IP-címet.

Az alábbi táblázat néhány példa frontend konfiguráció tartalmazza:

| VIRTUÁLIS | IP-cím | Protocol (protokoll) | port |
|-----|------------|----------|------|
|1|65.52.0.1|TCP|80|
|2|65.52.0.1|TCP|_8080_|
|3|65.52.0.1|_UDP_|80|
|4|_65.52.0.2_|TCP|80|

A táblázat négy különböző frontends. Frontends #1 #2 és #3 a több szabállyal rendelkező egyetlen virtuális. Az azonos IP-címet használja, de a port vagy protokoll eltér az egyes frontend. Frontends #1 és #4 több VIP, ahol ugyanazon frontend protokoll és port vannak ismételten több VIP példát.

Azure betöltése terheléselosztó terheléselosztási szabályok meghatározásakor rugalmasságot biztosít. A szabályok deklarálása hogyan egy címet és a frontend portjához van hozzárendelve a címzett címét és a kódmentes portjához. Engedélyezi vagy tiltja a kódmentes portokat használja fel újra keresztül szabályok attól függ, hogy milyen típusú a szabályt. Különböző típusú szabály is befolyásolja a host konfigurálása és vizsgálati tervezés adott követelményeket tartalmaz. Szabályok két típusa van:

1. Az alapértelmezett szabály nincs kódmentes port újrafelhasználása
2. A lebegő IP szabályt, ahol kódmentes portok újrafelhasználható vannak

Azure betöltése terheléselosztó lehetővé teszi, hogy ugyanazt a betöltés terheléselosztó konfigurációt szabály mindkettőt keverjen ki. A terheléselosztó lehet a segítségükkel egyszerre egy adott virtuális vagy tetszőleges, feltéve, a kényszerek, a szabály betartják. Kiválaszthatja, hogy milyen szabály attól függ, a követelményeknek, az alkalmazás és a konfigurációs támogató komplexitását. Az igényektől szabályhoz milyen legalkalmasabbak kell kiértékelésének eredménye.

További forgatókönyvekben feltárása az alapértelmezett működés kezdve.

## <a name="rule-type-1-no-backend-port-reuse"></a>Szabály #1 típus: nincs kódmentes port újrafelhasználása

![MultiVIP ábra](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

Ebben az esetben a frontend VIP az alábbiak szerint vannak beállítva:

| VIRTUÁLIS | IP-cím | Protocol (protokoll) | port |
|-----|------------|----------|------|
|![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

A DIP célja a bejövő folyamat. A kódmentes készletben minden virtuális közzététele egy egyedi portjához egy DIP a kívánt szolgáltatást. Ez a szolgáltatás társítva a frontend egy szabály definition keresztül.

Azt adja meg a két szabályok:

| Szabály | Térkép frontend | Kódmentes készlet |
|------|--------------|-----------------|
| 1 | ![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![kódmentes](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![kódmentes](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 | ![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![kódmentes](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![kódmentes](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

A teljes leképezés az Azure terheléselosztó már az alábbi képlettel történik:

| Szabály | Virtuális IP-cím | Protocol (protokoll) | port | Cél | port |
|------|----------------|----------|------|-----|------|
|![szabály](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|DIP IP-cím|80|
|![szabály](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|DIP IP-cím|81|

Szabályok cél IP-címe és célport egyedi kombinációjával adatfolyam kell dolgoznia. A folyamat a célport megváltoztatásával több szabállyal tartana flow a azonos DIP más portokra.

Állapot szondákat mindig irányítja át a DIP egy virtuális gép. Győződjön meg arról, hogy a vizsgálati tükrözze a virtuális állapotának.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Szabály #2 típus: kódmentes port újrafelhasználása lebegő IP használatával

Azure terheléselosztó rugalmasságot újra felhasználhatja a frontend port használt szabály függetlenül több VIP keresztül. Ezenkívül néhány alkalmazás forgatókönyvet inkább, vagy csak az ugyanazt a portot egy egyetlen virtuális kódmentes készletben a több szolgáltatásalkalmazás-példányok való használatra. A port újrafelhasználása hétköznapi példát szeretne felvenni a magas elérhetősége fürtözőszoftverét, hálózati virtuális készülékek, és ki több TLS végpontok újbóli titkosítás nélkül.

Ha, hogy szeretné használni a kódmentes port több szabállyal végig, engedélyeznie kell lebegő IP a szabály definícióban.

Lebegő IP egy részét terület szerint közvetlen kiszolgáló vissza (DSR). DSR két részből áll: egy folyamat topológiája és a egy IP-cím leképezési séma. A platform szinten Azure terheléselosztó mindig működik DSR folyamat topológiában függetlenül lebegő IP van engedélyezve van-e vagy sem. Ez azt jelenti, hogy a kimenő részét egy folyamat megfelelően mindig újraírásra kattintva visszatérhet az origin közvetlenül flow.

Az alapértelmezett szabálytípus az Azure közzététele hagyományos terheléselosztási IP cím hozzárendelése az Office-téma az egyszerű használat érdekében. Az IP cím hozzárendelése az Office-téma további rugalmasság érdekében alatti című cikkben ismertetett módon lebegő IP-engedélyezése változik.

Az alábbi ábra bemutatja az ebben a konfigurációban:

![MultiVIP ábra](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

Az ebben az esetben a kódmentes készletben minden virtuális három hálózati kapcsolatok foglalja magában:

* DIP: a virtuális hálózati kártya társított a virtuális (Azure cég hálózati erőforrás)
* VIP1: visszacsatolási felhasználói felülete Vendég IP-címet a VIP1 konfigurált OS belül
* VIP2: visszacsatolási felhasználói felülete Vendég IP-címet a VIP2 konfigurált OS belül

>[AZURE.IMPORTANT] A logikai felületek konfigurációja belül a vendégként való bekapcsolódáshoz OS történik. Ez a beállítás nem elvégzett és Azure kezeli. Ebben a konfigurációban nélkül nem működik a szabályokat. Állapot vizsgálati definíciók a DIP a logikai virtuális helyett a virtuális használja. Ezért a szolgáltatás biztosítania kell vizsgálati válaszok DIP port, amely a logikai virtuális teendőkre kínált szolgáltatás állapotát.

Tegyük fel például az előző forgatókönyvben ugyanazt a frontend konfigurációt:

| VIRTUÁLIS | IP-cím | Protocol (protokoll) | port |
|-----|------------|----------|------|
|![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|
|![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|*65.52.0.2*|TCP|80|

Azt adja meg a két szabályok:

| Szabály | Térkép frontend | Kódmentes készlet |
|------|--------------|-----------------|
| 1 | ![szabály](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 | ![kódmentes](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (a VM1 és VM2) |
| 2 | ![szabály](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 | ![kódmentes](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (a VM1 és VM2) |

A következő táblázat mutatja a teljes leképezés a terheléselosztó:

| Szabály | Virtuális IP-cím | Protocol (protokoll) | port | Cél | port |
|------|----------------|----------|------|-------------|------|
|![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1|65.52.0.1|TCP|80|ugyanaz, mint a virtuális (65.52.0.1)|ugyanaz, mint a virtuális (80)|
|![VIRTUÁLIS](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2|65.52.0.2|TCP|80|ugyanaz, mint a virtuális (65.52.0.2)|ugyanaz, mint a virtuális (80)|

A bejövő folyamat célja a virtuális a visszacsatolási felületén a virtuális címet. Szabályok cél IP-címe és célport egyedi kombinációjával adatfolyam kell dolgoznia. Cél IP-címét a folyamat megváltoztatásával port újrafelhasználása a azonos virtuális a lehetőség. A szolgáltatás által kötése a virtuális IP-cím és a megfelelő visszacsatolási felület port van kitéve a terheléselosztó.

Figyelje meg, hogy ez a példa a célport nem változik. Akkor is, ha ez a példa az IP lebegő, Azure terheléselosztó is lehetővé teszi a kódmentes célport átírásának és, hogy a frontend célport eltér szabályt.

A lebegő IP szabálytípus az alapja a konfigurációs minták több betöltése terheléselosztó. Egy példa jelenleg elérhető [SQL AlwaysOn több hallgatók tartalmazó](../virtual-machines/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) beállítás. Az idő múlásával azt fogja a dokumentum több forgatókönyvekben.

## <a name="limitations"></a>Korlátozások

* Több virtuális konfiguráció csak használhatók a IaaS VMs.
* A lebegő IP-szabályt az alkalmazás kell használni a DIP kimenő folyamatok. Ha az alkalmazás az operációs rendszer vendégként visszacsatolási felületén virtuális címmel kötődik, majd SNAT nem érhető el a kimenő folyamat írniuk, és a folyamat nem sikerült.
* Nyilvános IP-címek számlázási hatással. További tudnivalókért lásd: a [IP-cím árak](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Előfizetés a korlátok vonatkoznak. További információ című cikkben találhat [szolgáltatás](../azure-subscription-service-limits.md#networking-limits) további információt.
