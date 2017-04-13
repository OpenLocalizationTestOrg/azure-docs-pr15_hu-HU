<properties
  pageTitle="Egyéni szondákat terheléselosztó betöltése és állapot állapot ellenőrzése |} Microsoft Azure"
  description="Az Azure terheléselosztó egyéni szondákat használata a Lync-példányok terheléselosztó mögött"
  services="load-balancer"
  documentationCenter="na"
  authors="sdwheeler"
  manager="carmonm"
  editor=""
  tags="azure-resource-manager"
/>
<tags
  ms.service="load-balancer"
  ms.devlang="na"
  ms.topic="article"
  ms.tgt_pltfrm="na"
  ms.workload="infrastructure-services"
  ms.date="10/24/2016"
  ms.author="sewhee" />

# <a name="understand-load-balancer-probes"></a>Betöltési terheléselosztó szondákat ismertetése

Azure terheléselosztó lehetőséget nyújt a Lync-kiszolgálói példány állapotának szondákat használatával. Amikor egy vizsgálati nem válaszol, terheléselosztó megszakítja a küld új kapcsolatokat a sérült példány. A meglévő kapcsolatok nem érinti a probléma, és új kapcsolatokat megfelelő példányaiban küldjük.

Felhőalapú szolgáltatás szerepkörök (dolgozó és webes szerepeket) használata a vendégként való bekapcsolódáshoz ügynök vizsgálati figyelése. TCP- vagy HTTP egyéni szondákat virtuális gépeken futó mögött terheléselosztó használatakor kell beállítania.

## <a name="understand-probe-count-and-timeout"></a>Vizsgálati darab és a határidő ismertetése

Vizsgálati viselkedés függ:

- Sikeres szondákat, amelyek lehetővé teszik a kategóriafeliratok egy példány a számát, akár.
- Egy példány a kategóriafeliratok okozó sikertelen szondákat száma szerint lefelé.

Az időkorlát- és érték megadása a SuccessFailCount határozza meg, hogy egy példánya fut, vagy nem fut igazolja. Az Azure-portálon időtúllépés kétszer az a gyakoriság értékre van állítva.

Zárólap (Ez azt jelenti, hogy egy terheléselosztás készlet) minden terheléselosztás példányok vizsgálati konfigurációja azonosnak kell lennie. Ez azt jelenti, hogy nem lehet a különböző vizsgálati konfiguráció az egyes szerepkör-példány vagy a virtuális gép egy adott végpont kombináció szolgáltatott ugyanazt a szolgáltatást. Egyes példányok például az azonos helyi portokat és időtúllépései kell rendelkeznie.

>[AZURE.IMPORTANT] Egy terheléselosztó vizsgálati az IP-cím 168.63.129.16 használja. A nyilvános IP-címet az Előrehozás-a-saját-IP-Azure virtuális hálózatot belső platform erőforrásokhoz kapcsolattartást. A virtuális nyilvános IP-cím 168.63.129.16 minden régióban használja, és nem változik. Azt javasoljuk, hogy engedélyezte-e IP-cím bármely helyi tűzfal házirendek. Azt nem kell figyelembe venni biztonsági kockázatot, mert a belső Azure platform és a forrásoszlop is, hogy címről üzenetet. Ha még nem ez, például az azonos IP-címtartományokat 168.63.129.16 és kellene megkettőződik IP-címek konfigurálása az esetek különböző szokatlan viselkedést tapasztal lesz.

## <a name="learn-about-the-types-of-probes"></a>További tudnivalók: szondákat típusai

### <a name="guest-agent-probe"></a>A vendégként való bekapcsolódáshoz ügynök vizsgálati

Ez a vizsgálati csak akkor érhető el az Azure Cloud Services. Terheléselosztó használja a vendégként való bekapcsolódáshoz agent a virtuális gép belül, majd figyeli és válaszol a HTTP 200 OK választ, csak akkor, amikor a példány kész állapotban van (Ez azt jelenti, hogy nem a másik állami például elfoglalt, újrafelhasználás vagy leállítása).

További tudnivalókért lásd: [a szolgáltatás-definíciós fájl (csdef) beállítása a rendszerállapot szondákat](https://msdn.microsoft.com/library/azure/ee758710.aspx) vagy [első lépések megtételében egy internetes terheléselosztó cloud Services](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>A vendégként való bekapcsolódáshoz ügynök vizsgálati megjelölni egy példány megsérült Miből?

A vendégként való bekapcsolódáshoz agent nem válaszol a HTTP 200 az OK gombra, ha a terheléselosztó megjelöli a példány válaszol, és leállítja a forgalom küld az adott példányhoz. A terheléselosztó továbbra is a példány ping. A vendégként való bekapcsolódáshoz agent egy HTTP 200 válaszol, ha a terheléselosztó küld forgalom példányhoz újra.

A webes szerepkör használata esetén a webhely-kód általában fut w3wp.exe, amelyek nem figyel az Azure háló vagy Vendég ügynök. Ez azt jelenti, hogy hibáit w3wp.exe (például HTTP 500 válaszok) nem lehet jelenteni az Vendég ügynök, és a terheléselosztó nem veszi ki a Forgatás példányhoz.

### <a name="http-custom-probe"></a>HTTP egyéni vizsgálati

Az egyéni HTTP terheléselosztó vizsgálati felülbírálja az alapértelmezett Vendég ügynök vizsgálati, ami azt jelenti, hogy a saját egyéni logika a szerepkör-példány a állapotáról hozhat létre. A terheléselosztó minden 15 másodperc ellenőrzi a végpontot alapértelmezés szerint. A példányt el szeretné helyezni a betöltés terheléselosztó forgatás, ha a időtúllépési időszakban (alapértelmezés szerint 31 másodperc) azt válaszol egy HTTP 200 számít.

Ez akkor lehet hasznos, ha a saját logika példányok eltávolítása a betöltés terheléselosztó Forgatás végrehajtásához. Ha például sikerült eldöntheti, ha el szeretne távolítani egy példány, ha 90 %-os Processzor fölé, és nem 200 állapotot ad vissza. Ha webes szerepkörök w3wp.exe használó, ez azt is jelenti kap automatikus figyelése a webhely, mert hiba lépett fel a webhely-kód nem 200 állapotban visszatér a betöltés terheléselosztó vizsgálati.

>[AZURE.NOTE] Az egyéni HTTP-vizsgálati relatív elérési utak csak a HTTP protokollt támogató. HTTPS nem támogatott.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Miből egy egyéni HTTP vizsgálati megjelölni egy példány megsérült?

- A HTTP-alkalmazások egy HTTP-válasz kódot eltérő 200 (például 403, 404-es vagy 500) adja eredményül. Ez a pozitív visszajelzés, hogy az alkalmazás-példány azonnal szolgáltatás ki kell tenni.
- A HTTP-kiszolgáló nem válaszol minden a határidő lejárta után. Attól függően, hogy az időtúllépés értékét, és állítsa be, ez lehet jelenti, hogy több vizsgálati kéri a megválaszolatlan, mielőtt a vizsgálati kap megjelölve nem fut a go (Ez azt jelenti, mielőtt SuccessFailCount szondákat küldjük).
- A kiszolgáló megszünteti a kapcsolatot, a TCP alaphelyzetbe állítása keresztül.

### <a name="tcp-custom-probe"></a>Egyéni TCP-vizsgálati

TCP szondákat létesít kapcsolatot a hármas kézfogás a definiált porthoz hajt végre.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Miből egy példány megsérült megjelölése egyéni TCP-vizsgálati?

- A TCP-kiszolgáló nem válaszol minden a határidő lejárta után. Amikor a vizsgálati van megjelölve nem fut attól függ, hogy nem sikerült vizsgálati kérelmeket, lépjen a megválaszolatlan megjelölése nem fut, a vizsgálati előtti beállított számát.
- A vizsgálati kap egy a szerepkör-példány alaphelyzetbe állíthatja a TCP.

Egy HTTP állapot vizsgálati és a TCP-vizsgálati beállításával kapcsolatos további tudnivalókért lásd: [első lépések megtételében egy internetes terheléselosztó az erőforrás-kezelő PowerShell használatával](load-balancer-get-started-internet-arm-ps.md#create-lb-rules-nat-rules-a-probe-and-a-load-balancer).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Megfelelő hozzáadja az betöltés terheléselosztó Forgatás

TCP- és a HTTP szondákat megfelelő számítanak, és a megfelelő amennyiben az szerepkör-példány módosítható:

- A terheléselosztó kapja meg egy pozitív vizsgálati, először a virtuális betölti.
- A szám SuccessFailCount (korábbi leírás) szeretne megjelölni a szerepkör a példány megjelölése megfelelő szükséges sikeres szondákat értékét határozza meg. Egy szerepkör-példány el lett távolítva, sikeres, egymást követő szondákat kell egyenlő vagy haladja meg szeretné megjelölni az szerepkör-példány futtatásával SuccessFailCount értékét.

>[AZURE.NOTE] Ha egy szerepkör-példány állapotának hullámzó van, a terheléselosztó vár, már a szerepkör-példány helyezése a megfelelő állapotban előtt. Ez történik, a felhasználó és a infrastruktúrájának védelme házirenden keresztül.

## <a name="use-log-analytics-for-load-balancer"></a>Log analytics terheléselosztó használata

A vizsgálati állapot állapot és a vizsgálati darab ellenőrzése [napló elemzésének terheléselosztó](load-balancer-monitor-log.md) is használhatja. Naplózás kínál a Power BI vagy Azure műveleti háttérismeretek terheléselosztó állapot állapot statisztikájának megadását.
