<properties
   pageTitle="Több VMs futó |} Architektúra hivatkozás |} Microsoft Azure"
   description="Hogyan lehet több virtuális példánya futtatható a Azure méretezhetőség, tűrőképessége, kezelhetőség és biztonsági."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Több VMs futó Azure méretezhetőség és elérhetősége 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk méretezhetőség, elérhetőségét, kezelhetőség és biztonsági javítható több virtuális gépeken futó (virtuális) példánya fut igazolt tanácsok halmazának ismertet.   

Ez a architektúrában a terhelést a elosztva a virtuális példányok. Egy egyetlen nyilvános IP-címet, és internetes forgalmat a VMs egy terheléselosztó használatával történő van meghatározva. Ez a felépítés egyrétegű-alkalmazásokban, például egy állapot nélküli web app vagy a tárhely fürt használható. Érdemes emellett N szintű alkalmazások építőelem. 

Ez a cikk hoz létre [Egy egyetlen Azure virtuális gép]futtatásával[single vm]. Ebben a cikkben a javaslatok e architektúra is alkalmazhat.

## <a name="architecture-diagram"></a>Architektúra diagramja

Azure-ban VMs szükség támogatása a hálózati és tárolási erőforrásokat.

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "számítási - több virtuális fülre." 

![[0]][0]

Az alábbi összetevőket architektúrája foglalja magában: 

- **Elérhetőség beállítása.** Az [elérhetőség beállítása] [ availability set] a VMs tartalmaz, és szükség az [elérhetőség az Azure VMs SLA]támogatására szolgáló[vm-sla].

- **VNet**. Minden az Azure virtuális hálózatba egy virtuális (VNet), amely további oszlik **alhálózat**telepíti.

- **Azure terheléselosztó.** [Terheléselosztó] elosztja a bejövő internetes kérelmek a virtuális példányaiban egy elérhetőségének beállítása. A terheléselosztó bizonyos kapcsolódó erőforrásokat tartalmazza:

  - **Nyilvános IP-címet.** Egy nyilvános IP-címet az internetes forgalmat fogadni terheléselosztó szükséges.

  - **Előtér-konfiguráció.** A nyilvános IP-címmel társít a terheléselosztó.

  - **Készlet háttéradatbázist címet.** A VMs, amely kapja a bejövő forgalmat a hálózati kapcsolatok (NIC) tartalmaz.

- **Betöltése terheléselosztó szabályokat.** Hálózati forgalmának engedélyezésére között a háttéradatbázist cím készletben összes VMs terjesztésére használhatók. 

- **Hálózati Címfordítást szabályokat.** Egy adott virtuális forgalom átirányítása szolgál. Például ahhoz, hogy a távoli asztali jegyzőkönyv (RDP) a VMs, külön hálózati cím címfordító szabály létrehozása minden virtuális. 

- **Hálózati kapcsolatok (NIC)**. Minden egyes virtuális tartalmaz egy hálózati kártya, és csatlakozzon a hálózathoz.

- **Tárhely.** Tárterület-fiókok tartsa lenyomva az ujját a virtuális képek és egyéb fájllal kapcsolatos erőforrások, például az Azure által rögzített virtuális diagnosztikai adatok.

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A hivatkozás architektúra, amely követi az alábbiakban ismertetett ajánlások telepítése egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik egy adott követelmény, amely nem támogatja a ajánlást célszerű követnie az alábbi javaslatokat. 

### <a name="availability-set-recommendations"></a>Elérhetőség javaslatok beállítása

Létre kell hoznia legalább két VMs támogatja az [elérhetőség az Azure VMs SLA]elérhetősége[vm-sla]. Figyelje meg, hogy az Azure terheléselosztó is kell lennie, hogy terheléselosztás VMs elérhetősége mindazon tartozik.

### <a name="network-recommendations"></a>Hálózati javaslatok

A VMs mögött a terheléselosztó szeretné az összes elhelyezni ugyanahhoz az alhálózathoz belül. Nem jelenítik meg a VMs közvetlenül az interneten, de inkább egy privát IP-címet az egyes virtuális ad. Az ügyfélgépek hogyan kapcsolódnak a terheléselosztó a nyilvános IP-cím használatával.

### <a name="load-balancer-recommendations"></a>Terheléselosztó javaslatok betöltése

Az összes VMs hozzáadása beállítása a háttéradatbázist cím kvótáját a terheléselosztó elérhetőségét.

Közvetlen hálózati forgalmat a VMs betöltés terheléselosztó szabályok határozza meg. Például ahhoz, hogy a HTTP-forgalmat, hozzon létre egy szabályt, amely a port 80 a előtér-beállítások a háttéradatbázist cím készlet a 80-as port rendeli. Amikor a terheléselosztó 80-as port a nyilvános IP-címének a kérelem érkezik, azt a kérelem szeretné a háttéradatbázis cím készlet a hálózati kártyákat egyik 80-as port irányítja.

Egy adott virtuális forgalom átirányítása hálózati Címfordítást szabályok határozza meg. Például ahhoz, hogy a VMs való RDP külön hálózati Címfordítást szabály létrehozása minden virtuális. Szabályok kell egy egyedi portszámot megfeleltetése port 3389, az alapértelmezett portja RDP. (Például portot 50001 használja a "VM1", "VM2," 50002 port és stb.) A hálózati Címfordítást szabályok hozzárendelése a VMs hálózati kártyája. 

### <a name="storage-account-recommendations"></a>Tárterület-fiók beállítására vonatkozó javaslatok

Minden egyes virtuális tartása a virtuális merevlemez (VHD), hogy a jövőben elkerülhesse szerezze meg a bemeneti és kimeneti műveletek második [(IOPS) korlátai] külön Azure tárterület-fiókok létrehozása[ vm-disk-limits] tároló fiókok. 

A diagnosztikai naplók egy tárterület-fiók létrehozása. Ehhez a fiókhoz tároló összes VMs is osztozhat.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Kétféleképpen lehet a méretezés kifelé VMs Azure-ban: 

- Egy terheléselosztó segítségével VMs halmazának elosztása hálózati forgalmának engedélyezésére. Méretezése, további VMs kiépítése, és helyezze át őket a terheléselosztó mögött. 

- [Virtuális gép skála készletek]használata[vmss]. Egy skála megadása a mögött egy terheléselosztó azonos VMs Transaction számot tartalmaz. Virtuális skála állítja be a támogatási autoscaling teljesítménymutatók alapján. Ahogy a VMs terhelését nő, további VMs automatikusan megjelennek a terheléselosztó. 

A következő szakaszokkal hasonlítsa össze az alábbi két lehetőség.

### <a name="load-balancer-without-vm-scale-sets"></a>Terheléselosztó virtuális skála beállítása nélkül

Egy terheléselosztó bejövő hálózati kéréseket fogad, és elosztja a hálózati kártyákat a háttéradatbázist cím készlet őket. Vízszintesen méretezéséhez úgy adhat hozzá további virtuális példányok az elérhetőség (vagy ha át kívánja méretezni VMs felszabadítása). 

Tegyük fel, hogy webkiszolgálóra futtat. Vegyen fel 80-as port és a 443-as port betöltés terheléselosztó szabály (az SSL-). Amikor egy ügyfél HTTP-kérelem küld, a terheléselosztó választja ki a háttéradatbázist IP-cím használata a [Kivonatolás algoritmus] [ load balancer hashing] , amely tartalmazza a forrás IP-címét. Úgy, hogy az ügyfél kérések összes VMs vannak elosztva. 

> [AZURE.TIP] Ha egy új virtuális rendel egy elérhetőségének beállítása, ügyeljen arra, hogy a virtuális hozzon létre egy hálózati kártya és a hálózati kártya hozzáadása a háttéradatbázist cím gyűjtő a terheléselosztó. Egyéb esetben internetforgalom nem lehet továbbítani az új virtuális.

Minden egyes Azure előfizetés munkalapjai alapértelmezett helyen, például VMs / régió maximális száma. A korlát növelése egy támogatási kérelmet tárolásához. További tudnivalókért lásd: a [Azure előfizetést és a szolgáltatás korlátozások, a kvótákat, és a kényszerek][subscription-limits].  

### <a name="vm-scale-sets"></a>Virtuális skála beállítása 

Virtuális skála készletek segítséget üzembe helyezéséhez és kezeléséhez azonos VMs csoportja. Az összes VMs konfigurált azonos, virtuális skála beállítása támogatja az IGAZ Automatikus méretezéssel, előre kiépítési VMs, a könnyebb olvashatóság érdekében nagy számítási, nagy adatokkal és indexelése munkaterhelésekből kiválasztásával nagyméretű szolgáltatások összeállítása nélkül. 

További információt a virtuális skála készletek [Áttekintése]című témakörben találhat virtuális gép skála állítja be[vmss].

Virtuális skála készletek használatával kapcsolatos szempontok

- Ha szeretne gyorsan VMs méretezése, vagy az Automatikus méretezéssel kell, akkor érdemes skála beállítása. 

- Méretezés beállítása jelenleg nem támogatja a adatok lemezt. A beállítások, adatok tárolására szolgáló Azure tárhely, a OS meghajtóra, a Temp meghajtó vagy egy külső áruházból, például Azure tárhely. 

- Elérhetőség ugyanazokat hibafa-tartományok 5 és 5 frissítés tartományok belül skálával automatikusan beállítva, példányait virtuális tartozik.

- Alapértelmezés szerint skála készletek használata "overprovisioning", ami azt jelenti, skála megadása kezdetben rendelkezések VMs több, mint ki, majd törli a felesleges VMs. Ez a teljes sikeres ráta javítja a kiépítésekor a VMs. 

- Több nem javasoljuk, majd mint 20 VMs tárterület-használati engedélyezett vagy legfeljebb 40 VMs overprovisioning overprovisioning rendelkező fiók tiltható le.  

- Erőforrás-kezelő sablonok megtalálhatja a központi telepítés skála állít be az [Azure quickstart útmutató sablonok][vmss-quickstart].

- Kétféleképpen egyszerű telepítését a skála megadása az VMs konfigurálása: hozzon létre egy egyéni képe, vagy használhatja a bővítmények konfigurálása a virtuális, miután azt már kiépítve.

    - Egy egyéni kép épül skála megadása összes OS lemez VHD belül egy tárterület-fiókot kell létrehoznia. 

    - Egyéni képekkel kell naprakészen tartása a képet.

    - A bővítmények tarthat egy újonnan kiépített virtuális a léptetőnyíl vezérlőelem be.

További szempontok című témakörben talál [Tervezése virtuális skála beállítja a méretezés][vmss-design].

> [AZURE.TIP]  Amikor bármely automatikus méretezése megoldást, tesztelje a termelési szintű munka terhelések jól előre. 

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Az elérhetőség beállítása ellenőrzi az alkalmazás rugalmasabb az tervezett és a tervezett karbantartás eseményeket.

- _Tervezett karbantartás_ fordul elő, ha a Microsoft frissíti az alapul szolgáló platform néha okozó VMs újra kell indítani. Azure teszi, hogy a VMs egy elérhetőségének beállítása nem összes egyszerre indítani, legalább egy legyen fut, amikor mások újraindításkor.

- _Tervezett karbantartás_ történik, ha a hardver hiba. Azure meggyőződik arról, hogy egy elérhetőségének beállítása VMs kiépített környezetet egynél több kiszolgáló állvány keresztül. Ezzel az elemzéssel csökkentheti a hardver hibák hatásait, hálózati kimaradások, kiküszöbölése power és így tovább.

További tudnivalókért lásd: a [virtuális gépeken futó elérhetőségének kezelése][availability set]. Az alábbi videóban is rendelkezik elérhetőségének beállítása a áttekintése: [Hogyan a állítható be a méretezés VMs egy elérhetőségének beállítása][availability set ch9]. 

> [AZURE.WARNING]  Győződjön meg róla, hogy be, amikor Ön a virtuális kiépítése elérhetőségének beállítása. Jelenleg nem tudja, hogy egy erőforrás-kezelő virtuális hozzáadása egy után a virtuális már kiépítve elérhetőségét.

A terheléselosztó használja a [állapot ellenőrzi-e a] Lync-példányok virtuális az elérhető. Ha egy vizsgálati nem éri el egy példány időtúllépés időszakon belül, a terheléselosztó megszakítja a forgalom küldene, hogy virtuális. Azonban továbbra is a terheléselosztó probe, és a virtuális ismét elérhetővé válik, ha a terheléselosztó folytatja a forgalmat, hogy virtuális küldő.

Az alábbiakban néhány javaslatot a betöltés terheléselosztó állapot szondákat:

- Szondákat tesztelheti HTTP vagy a TCP. Ha a VMs Futtatás HTTP-kiszolgáló (IIS, nginx, Node.js alkalmazást, és így tovább), hozzon létre egy HTTP vizsgálati. Egyéb esetben hozzon létre egy TCP-vizsgálati.

- Egy HTTP vizsgálati az elérési útját a HTTP-végpont megadása A vizsgálati ellenőrzi, hogy az elérési út HTTP 200 választ. Ez a legfelső szintű elérési utat ("/") és a rendszerállapot figyelése végpontot, amely az egyes egyéni logika az alkalmazás állapotának ellenőrzése is lehet. Az endpoint engedélyeznie kell a névtelen HTTP-kérelmeket.

- A vizsgálati küldünk [ismert] [ health-probe-ip] 168.63.129.16 IP-cím. Ellenőrizze, hogy a forgalmat, illetve az IP-házirendek tűzfal vagy a hálózati biztonsági csoport (NSG) szabályok a nem letiltja.

- [Állapot vizsgálati naplók] használata[ health probe log] a rendszerállapot szondákat állapotának megtekintéséhez. Az egyes terheléselosztó az Azure-portálon naplózás engedélyezése. Naplók Azure Blob-tárolóhoz kerülnek. A naplók hány VMs megjelenítése a háttér nem kapnak miatt sikertelen vizsgálati válaszokat a hálózati forgalmának engedélyezésére.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

A több VMs fontos, hogy a folyamatok, automatizálása, így azok megbízható és megismételhető lesz. [Azure automatizálási] használható[ azure-automation] telepítési OS javítása és más feladatok automatizálásához. Azure automatizálási az automatizálási szolgáltatást az Azure fut, és a Windows PowerShell alapul. Példa automatizálási parancsfájlokat TechNet [Runbook gyűjtemény] érhetők el.

## <a name="security-considerations"></a>Biztonsági megfontolások

Virtuális hálózatok forgalom elkülönítési oszlopazonosító jobboldali Azure-ban. Az egyik VNet közvetlenül a egy másik VNet VMs az nem lehet kommunikálni VMs. Azonos belül VMs VNet tudnak kommunikálni, amíg nem hoz létre a [network biztonsági csoportok] [ nsg] forgalmának korlátozása (NSGs). További tudnivalókért olvassa el [a Microsoft-felhőszolgáltatások és]hálózati biztonsági[network-security].

A bejövő internetes forgalmat a betöltés terheléselosztó szabályok határozzák meg, mely forgalom érhető el a háttér. Azonban nem támogatják betöltés terheléselosztó szabályok IP whitelisting, ezért ha a hozzáadandó whitelist bizonyos nyilvános IP-címei, egy NSG alhálózathoz.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A hivatkozás-architektúra, amely a fenti ajánlást egy telepítésének GitHub érhető el. A hivatkozás architektúra egy virtuális hálózati (VNet), a hálózati biztonsági csoport (NSG), a terheléselosztó és a két virtuális gépeken futó (VMs) tartalmazza.

A hivatkozás architektúra telepítheti Windows vagy Linux VMs az alábbi útmutatást követve: 

1. Kattintson a jobb gombbal az alábbi gombra, és válassza ki a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Miután a hivatkozásra az Azure-portálon megnyílt, értéket kell megadnia az egyes beállításokat: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza a **Meglévő használatát** , és adja meg `ra-multi-vm-rg` a szövegmezőbe.
    - A **hely** legördülő listából válassza ki a régió.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - Jelölje ki az **Operációs rendszer típusa** a legördülő lista, **a windows** vagy **Linux rendszerhez**. 
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

4. A paraméter fájlokat olyan csomagolásukkor rendszergazda felhasználónevet és jelszót, és a javasolt, hogy azonnal módosítsa is. Kattintson a virtuális nevű `ra-multi-vm1` az Azure-portálon. Kattintson a **jelszó alaphelyzetbe állítása** az a **támogatási + hibaelhárítási** lap. Jelölje be a **jelszó alaphelyzetbe állítása** **üzemmód** legördülő mezőben, majd jelölje be az új **felhasználó nevét** és **jelszavát**. A **frissítés** gombra kattintva mentse az új felhasználó nevét és jelszavát. Ismételje meg a virtuális nevű `ra-multi-vm2`.

További módszerek, a hivatkozás architektúra üzembe a további tudnivalókért lásd a [útmutatást-egyetlen-virtuális] fontos fájl[ github-folder] GitHub mappát. 

## <a name="next-steps"></a>Következő lépések

Építőelem létrehozása a több szálon architektúrákban úgy, hogy a mögött egy terheléselosztó több VMs. További tudnivalókért lásd: [Windows VMs fut az Azure-N szintű architektúrája] [ n-tier-windows] és [Linux VMs fut az Azure-N szintű architektúrája][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[állapot szondákat]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[terheléselosztó]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Runbook gyűjtemény]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Az Azure-két VMs és egy terheléselosztó az elérhetőség alkotó a többszörös-virtuális megoldás architektúra"
