<properties
    pageTitle="A hálózati infrastruktúrára vonatkozó vészhelyreállítás tervezése |} Microsoft Azure"
    description="Ez a cikk azt ismerteti, hogy a hálózati tervezési szempontjait az Azure webhely helyreállítás"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>A hálózati infrastruktúrára vonatkozó vészhelyreállítás tervezése

Ez a cikk az informatikai szakemberek számára, akik architecting, felelős végrehajtási, és üzemkészségének és katasztrófa helyreállítási (BCDR) infrastruktúra és ki szeretné kihasználhatja a Microsoft Azure webhely-Helyreállítási való támogatása támogatja, és javíthatja minőségét a BCDR szolgáltatások irányítja. Ez a papír System Center virtuális gép Manager server deployment, a szakemberek és a negatívumokat összehasonlítása alhálózat feladatátvevő elnyújtva alhálózat gyakorlati kérdései, és hogyan strukturálhatja vészhelyreállítás a Microsoft Azure virtuális webhelyek ismerteti.

## <a name="overview"></a>– Áttekintés

[Azure webhely-Helyreállítási](https://azure.microsoft.com/services/site-recovery/) Microsoft Azure szolgáltatás, amely a protection és az üzleti folytonosságot (BCDR) vészhelyreállításra virtualizált alkalmazás helyreállítási orchestrates. A dokumentum célja Útmutató azon a folyamaton, tervezése a hálózatok, az olvasót architecting az IP-tartományokat és a katasztrófa helyreállítási webhelyen alhálózat virtuális gépeken futó (VMs) webhely helyreállítás segítségével verzióazonosítójának összpontosul.

Ezenkívül ez a cikk bemutatja, hogyan webhely helyreállítási lehetővé teszi, hogy architecting, és egy többhelyszínű BCDR szolgáltatásokat támogatja a próba-vagy katasztrófa egyszerre virtuális adatközponthoz végrehajtásához.

A világ, ahol mindenki vár 24/7 kapcsolat érdemes fontosnak mint minden eddiginél infrastruktúra- és alkalmazások, és folytonos. Az üzleti folytonosságot és katasztrófa helyreállítási (BCDR) célja sikertelen összetevők visszaállítására, hogy a szervezet gyorsan folytathatja a szokásos műveletek. Nagyon nehéz Vészhelyreállítási stratégiák meghatározása kell foglalkozni az valószínű, pusztító események elkészítésének. Ez a miatt a tervek, előrejelzésére belső nehézsége, különösen, mint valószínűtlenek eseményeket, és adja meg a megfelelő mértékek kiterjedt katasztrófa elleni védelem magas költség vonatkozik. 

Tervezési BCDR döntő, helyreállítási idő cél (RTO) és helyreállítási pont cél (Készletben) kell definiálhatók egy Vészhelyreállítási terv elkészítése részeként. Online a replikált virtuális gépeken futó található legkisebb adatvesztés (alacsony Készletben), a másodlagos adatközpont vagy a Microsoft Azure hozása esetén katasztrófa éri az ügyfél adatközpont Azure-webhely helyreállítás, ügyfelek lehet segítségével gyorsan (alacsony RTO). 

Feladatátvevő automatikus rendszer-Helyreállítás kezdetben a mezőnek kijelölt virtuális gépeken futó az elsődleges adatközpont a másodlagos adatközpont vagy Azure (attól függően, hogy az alkalmazási példát), és rendszeres időközönként frissítse a replikák lehetséges történik. Infrastruktúra megtervezésekor, hálózattervezés potenciális szűk, amelyek megakadályozzák a RTO értekezlet vállalattól, és a Készletben célok lehet tekinteni.  

Ha a rendszergazdák katasztrófa helyreállítási megoldást üzembe tervezi, a saját készítésű kérdések egyik hogyan a virtuális gép a következő lesz elérhető a feladatátvételi befejeződése után. Automatikus rendszer-Helyreállítás lehetővé teszi, hogy a rendszergazda számára, válassza ki a hálózatot, amelyhez egy virtuális gép volna csatlakoznia feladatátvétel után. Ha az elsődleges webhely VMM kiszolgáló kezeli majd ez érhető el hálózati leképezés használata. További részleteket lásd: a [hálózati hozzárendelést előkészítés](site-recovery-network-mapping.md) .

A hálózat a helyreállítási webhely tervezésekor a rendszergazda két lehetőség közül választhat:

- A hálózat helyreállítási helyen egy másik IP-címtartományokat használja. Ebben az esetben a virtuális gép feladatátvétel után jelenik meg egy új IP-címet, és a rendszergazda kellene végezze el a DNS-frissítés. További információ arról, hogy miként végezze el a DNS frissítése [Itt](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- A hálózat helyreállítási webhelyén azonos IP-címtartományokat használja. Bizonyos esetekben rendszergazdák szeretné megőrizni az IP-címek, amely a elsődleges webhelyen, a feladatátvétel után is rendelkeznek. Normál helyzetben rendszergazda szeretné, hogy a leveleket, hogy az új helyre a IP-címek frissítése. De az esetben, ha telepítve van egy elnyújtva virtuális az elsődleges és a helyreállítási webhelyek között, de megtartja az IP-címek a virtuális gépeken futó vonzó lehetőség lesz. Hagyja az azonos IP címek egyszerűbbé teszi a helyreállítási folyamat nem vagyok a gépnél olyan hálózat vételével kapcsolódó bejegyzés átváltó lépéseket.


Ha a rendszergazdák katasztrófa helyreállítási megoldást üzembe tervezi, azok szem előtt a kérdések egyik hogyan az alkalmazások lesz elérhető a feladatátvételi befejeződése után. Alkalmazásokat modern a hálózat bizonyos fokig, így fizikailag áthelyezése egyik webhelyről a másikra szolgáltatás jelöli hálózati bonyolulttá majdnem mindig függnek. Kétféleképpen fő, amely a probléma megoldásához a katasztrófa helyreállítási megoldások foglalkozik. Az első megközelítésre, hogy a rögzített IP-címek kezelése. A services, áthelyezés és a különböző fizikai helyek üzemeltető kiszolgálókon, ellentétben alkalmazások készítése az IP-cím beállítása a velük az új helyre. A második megközelítés teljesen módosítása az IP-címet az áttérés a helyreállított webhelyen során. Minden egyes megközelítés alatti soroljuk több végrehajtása változat tartalmaz.

A hálózat a helyreállítási webhely tervezésekor a rendszergazda két lehetőség közül választhat:

## <a name="option-1-retain-ip-addresses"></a>1 beállítást: Megőrzi az IP-címek 

A katasztrófa helyreállítási folyamat szemszögéből rögzített IP használatával címek úgy tűnik, hogy a legegyszerűbb módszer végrehajtásához, de a lehetséges problémáit, amelyek a gyakorlatban legyen a legkevésbé népszerű megközelítés számos rendelkezik. Azure webhely helyreállítási lehetővé teszi az IP-címek, minden esetben megtartani. Egy úgy dönt, amely megőrzi az IP, mielőtt megfelelő gondolkodási kell venni a kényszereket azt ró feladatátvevő lehetőségeit. Tudassa velünk tekintse meg a tényezőkkel, amelyek segítséget nyújtanak a döntés, amely megőrzi az IP-címet, vagy nem. Ez lehet elérni kétféle módon elnyújtva alhálózat használatával, vagy egy teljes alhálózat feladatátvevő módon.

### <a name="stretched-subnet"></a>Kiterjesztett alhálózat

Itt a alhálózat lett egyidejű elsődleges és DR helyen is elérhető. Egyszerűbben fogalmazva ez azt jelenti, és helyezheti át a kiszolgáló IP (Layer 3) konfigurációját a másik pontra, és a hálózat irányítja a forgalmat az új helyre automatikusan. Ez az a kiszolgáló szemszögéből kezelendő trivial, de számos kihívásokkal kapcsolatban van:

- A réteg 2 (adatok hivatkozás réteg) szemszögéből hálózati eszközöket, amelyek kezelhetik a elnyújtva virtuális kell hozzá, de ez lett kisebb, a probléma, széles körben elérhető. A második és nehezebben probléma, amely a virtuális mindkét webhelyen, a potenciális hibafa tartomány kiterjed lényegében valamit hiba egyetlen pont széthúzásával. Ez az valószínűleg nem esemény, miközben történt, hogy egy közvetítési vihar elindult, de nem sikerült elszigetelt. Vegyes véleményét erről a problémáról utolsó megtalálta azt, és jól "azt soha nem hajtja végre az alábbi e technológia" látható sok sikeres megvalósítás.
- Kiterjesztett alhálózat esetén nem lehet, ha a DR helyként használja a Microsoft Azure.


### <a name="subnet-failover"></a>Alhálózat feladatátvevő

A nyújtást használhatja az alhálózathoz több webhelyen nélkül fentebb ismertetett elnyújtva alhálózat megoldás előnyeinek alhálózat áttérni végrehajtásához lehetőség. Itt minden megadott alhálózat akkor jelenik meg a webhely-1 vagy a webhely 2, de nem mindkét webhelyen egyidejű. Annak érdekében, hogy az IP-címterület feladatátvételnél megtartásához programozás útján elrendezheti a útválasztó infrastruktúrájának a alhálózat áthelyezése egyik webhelyről a másikra. Az alhálózathoz szeretné áthelyezni, a társított feladatátvevő helyzetben VMs védett. A fő ezt a megközelítést hátránya megelőzve a hiba akkor kell áthelyeznie, a teljes alhálózat, amelyek lehetnek az OK gombra, de ez hatással lehet a feladatátvevő Granularitás megfontolások. 

Érdemes megvizsgálni, hogyan tudja a VMs bizonyos közben a hibás helyreállítási helyre a teljes alhálózat keresztül-e egy kontraktor nevű kitalált vállalati. Fog először megnézi az Mi a Contoso kezelő azok alhálózat, mialatt a VMs replikálása két között a helyszíni helyekre, majd fog ölelik hogyan alhálózat feladatátvevő működik, ha az [Azure szolgál a katasztrófa helyreállítási webhelyet](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Másodlagos való áttérés a helyszíni webhely

Tudassa velünk tekintse meg példa hol szeretnénk őrizni a VMs mindegyikének az IP és veszi át a teljes alhálózat együtt. Az elsődleges webhely alhálózat 192.168.1.0/24 futtatott alkalmazást tartalmaz. A feladatátvételi történik, ha minden a virtuális gépeken futó az alhálózathoz részét képező sikertelen lesz fölé a helyreállítási webhelyet, és megőrzi az IP-címeket. Útvonalak kell megfelelően módosíthatók megfelelően arra, hogy az összes a virtuális gépeken futó alhálózat 192.168.1.0/24 tartozó most át lett helyezve a helyreállítási webhelyet. 

Az alábbi ábrán látható elsődleges hely és helyreállítási webhely harmadik és webhelyek engedélyei és elsődleges webhelyen, és harmadik webhely helyreállítási webhely között a útvonalak kell megfelelően módosíthatók. 

Az alábbi képeken látható, a alhálózat a átadása előtt. Alhálózat 192.168.0.1/24 aktív a átadása előtt elsődleges webhelyen, és a feladatátvétel után a helyreállítási webhely aktívvá válik 

![Átadása előtt](./media/site-recovery-network-design/network-design2.png)

Átadása előtt


Az alábbi képen hálózatok és alhálózat feladatátvétel után.
    
![Feladatátvétel után](./media/site-recovery-network-design/network-design3.png)

Feladatátvétel után

A másodlagos webhelyen van a helyszíni és VMM kiszolgáló segítségével kezeli azt, majd egy adott virtuális gép védelmének engedélyezésekor automatikus rendszer-Helyreállítás fog erőforrásokat hálózati szerint az alábbi munkafolyamatot:

- Automatikus rendszer-Helyreállítás IP-címet a virtuális gépen minden hálózati kapcsolat lefoglalja a készletből statikus IP címet az egyes System Center VMM példány a megfelelő hálózaton definiált.
- Ha a rendszergazda a hálózati azonos IP-cím készletben közben lefoglalhat ugyanazt a címet, amely a virtuális elsődleges gép szerint szeretné a replika virtuális gép automatikus rendszer-Helyreállítás az IP-címek hozzárendelése a helyreállítási webhelyen, mint amit a elsődleges webhelyen, a hálózati IP-cím készlete határozza meg.  A IP VMM lefoglalt, de nincs beállítva mint feladatátvevő IP. Feladatátvevő IP csak az a átadása előtt van beállítva.

![IP-cím megőrzése](./media/site-recovery-network-design/network-design4.png)
    
Ábra 5

Ábra 5 látható a replika virtuális gép feladatátvevő TCP/IP beállításainak (a Hyper-V konzol). Ezeket a beállításokat szeretné töltik, csak, mielőtt a virtuális gép indítják feladatátvevő után

Az azonos IP nem érhető el, ha az automatikus rendszer-Helyreállítás néhány elérhető IP-cím lenne a definiált IP-cím készletből lefoglalhat. 

Miután a virtuális engedélyezve van a védelem használhatja mintaparancsfájl követően ellenőrizze a felosztott a virtuális géphez IP. Az azonos IP volna értéke lehet feladatátvevő IP, és a virtuális rendelt feladatátvevő idején:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] Az alkalmazási példát, ahol a virtuális gépeken futó DHCP használja, az IP-címek kezelését teljesen kívülre esik automatikus rendszer-Helyreállítás irányítását. A rendszergazda annak érdekében, hogy a szerepkör a helyreállítási webhelyen az IP-címek útja szolgálhatnak ugyanazt a tartományt az, hogy az elsődleges webhely.

#### <a name="failover-to-azure"></a>Azure áttérni

Azure webhely-Helyreállítási lehetővé teszi, hogy a Microsoft Azure katasztrófa helyreállítási helyként a virtuális gépeken futó használandó. Ebben az esetben meg kell foglalkozni egy további kényszer. 

Érdemes megvizsgálni forgatókönyvön, ahol Woodgrove Bank megnevezett kitalált vállalat rendelkezik a helyszíni infrastruktúra szolgáltatója a vonal üzleti alkalmazások, és azok üzemeltet a mobil Azure-alkalmazásokkal. Woodgrove Bank VMs az Azure és a helyszíni kiszolgálók közötti kapcsolatot által egy webhely (S2S) virtuális magánhálózat (VPN) áll rendelkezésre. S2S VPN lehetővé teszi, hogy Woodgrove Bank virtuális hálózati Woodgrove Bank helyszíni hálózaton meghosszabbítása láthatja az Azure-ban. A kapcsolati S2S VPN Woodgrove Bank széle és a Azure virtuális hálózati közötti engedélyezve van. Most már Woodgrove használni szeretné automatikus rendszer-Helyreállítás való replikáció annak az az Adatközpont Azure operációs rendszert futtató munkaterhelésekből. Ez a beállítás igényeknek Woodgrove, amely gazdaságos DR beállítást kíván, és tudja, hogy tárolja az adatokat nyilvános felhőalapú környezetben. Woodgrove kell foglalkozni az alkalmazások és a konfigurációk csomagolásukkor IP-címek attól függenek, amely tartalmaz, ezért rendelkeznek a követelmény, hogy miután az Azure megőrzi az IP-címek a saját alkalmazások.

Woodgrove úgy döntött, IP-címek hozzárendelése az IP-címtartományokat (172.16.1.0/24, 172.16.2.0/24) az Azure-ban futó erőforrásait.

Engedélyezni szeretné a virtuális gépeken futó bizonyos az Azure az IP-címek megtartásával Woodgrove egy Azure virtuális hálózat meg kell adni a létrehozandó. A helyszíni hálózaton kiterjesztése kell tenni, hogy az alkalmazások Azure áttérni a helyszíni webhely zökkenőmentes is. Azure-webhely, valamint a pont-webhely virtuális Magánhálózati kapcsolat hozzáadása a készült Azure virtuális hálózatok teszi lehetővé. A webhely kapcsolat beállításakor Azure hálózati teszi lehetővé a forgalmat a helyszíni helyre (Azure hívja meg helyi hálózati) csak akkor, ha az IP-címtartományokat eltér a helyi IP-címtartományokat, mert Azure Nyújtás alhálózathoz nem támogatja.  Ez azt jelenti, hogy ha van egy alhálózat 192.168.1.0/24 a helyszíni, nem a helyi hálózati 192.168.1.0/24 hozzáadása az Azure hálózaton. Ez várható, mivel az Azure nem tudja, hogy nincsenek-e nem aktív VMs az alhálózathoz, és csak DR célokra létrehozott az alhálózathoz. Engedélyezni az Azure hálózat a hálózat és a helyi hálózat az alhálózathoz nem lehetnek azonosak ki helyesen hálózati forgalmat. 

![Alhálózat átadása előtt](./media/site-recovery-network-design/network-design7.png)

Átadása előtt

Woodgrove azok üzleti követelmények teljesítése érdekében az alábbi munkafolyamatokat megvalósítása szükség:

- Hozzon létre egy további hálózat, tudassa velünk hívja meg helyreállítási hálózati, ahol ezzel a sikertelen fölé virtuális gépeken futó hozható létre.
- Győződjön meg arról, hogy a egy virtuális IP után feladatátvevő tárolja, lépjen a beállítás lapon a virtuális tulajdonságai csoportban adja meg, hogy a virtuális van-e a helyszíni az azonos IP, és kattintson a Mentés gombra. Amikor a virtuális keresztül nem sikerült, Azure webhely helyreállítási rendel a megadott IP a virtuális gépen. 

![Hálózati tulajdonságai](./media/site-recovery-network-design/network-design8.png)

Miután a feladatátvételi dátumra, és a helyreállítási a kívánt IP-hálózaton a virtuális gépeken futó jönnek létre, a hálózati kapcsolat hozható létre egy [Vnet Vnet kapcsolatot](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)használ. Ha szükséges, ez a művelet parancsfájl alapú is lehet.  Az előző szakaszban tárgyalt azt kapcsolatos alhálózat feladatátvételt, akár az Azure-áttérni esetében útvonalak kellene megfelelően, hogy 192.168.1.0/24 most átkerül Azure megfelelően módosíthatók. 

![Alhálózat feladatátvétel után](./media/site-recovery-network-design/network-design9.png)

Feladatátvétel után

Ha nem rendelkezik egy Azure hálózaton, a fenti képen látható módon. A "Elsődleges hely" és "Helyreállítási hálózat" közötti webhelyre történő virtuális magánhálózati kapcsolat a feladatátvétel után hozhat létre.  


## <a name="option-2-changing-ip-addresses"></a>2 beállítás: Az IP-címek módosítása

Ezt a megközelítést úgy tűnik, hogy a legelterjedtebb mi azt egyszer látott alapján. Az űrlap minden érintett a című témakörében virtuális IP-címének módosítása vesz igénybe. Az eljárás visszatérítési minderről "", hogy az alkalmazás, amely a IPx ettől kezdve a IPy a bejövő hálózat szükséges. Akkor is, ha IPx és IPy logikai nevek, általában módosítható vagy a hálózaton, egész kiürített szükséges DNS-bejegyzések és hálózati táblázatokban gyorsítótárazott bejegyzések kell frissítése vagy a kiürített, ezért a legrövidebb leállás sikerült láthatja attól függően, hogy a DNS-infrastruktúrát van beállítva. Ezeket a problémákat is szüntethető alacsony TTL (élettartam) értékek esetén intranetes alkalmazások használatával, és [Azure forgalom kezelő automatikus rendszer-Helyreállítás az](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) internet alapú alkalmazáshoz

### <a name="changing-the-ip-addresses---illustration"></a>Az IP-címek - ábra módosítása

Tudassa velünk tekintse meg az alkalmazási példát hol szeretné használata különböző IP-címei az elsődleges és a helyreállítási webhelyek. A következő példa azt is van egy harmadik webhelyen, a hol az alkalmazások is elsődleges vagy helyreállítási webhely is elérhető.

![Különböző IP - átadása előtt](./media/site-recovery-network-design/network-design10.png)

Ábra 11

A 11 néhány alhálózat 192.168.1.0/24 alhálózat az elsődleges webhelyen lévő alkalmazások vannak, és helyreállítási webhelyén alhálózat 172.16.1.0/24 után feladatátvevő hamarosan beállítva. Virtuális Magánhálózati kapcsolatot/hálózati útvonalak konfigurálva megfelelően, hogy az összes három webhely elérhetik egymást.
 
12 jeleníti meg, miután egy vagy több alkalmazások ábrának őket a program visszaállítja a helyreállítási alhálózat a. Ebben az esetben azt nem korlátozza a teljes alhálózat átadni egy időben. Nincs módosítás konfigurálnia VPN- vagy hálózati útvonalak van szükség. Feladatátvevő és néhány DNS-frissítéseket fog fontos, hogy alkalmazások által elérhető. Ha be van állítva a DNS dinamikus frissítések engedélyezése a virtuális gépeken futó volna regisztráljon maguk az új IP használatával, miután feladatátvevő után induljanak el. 

![Különböző IP - feladatátvétel után](./media/site-recovery-network-design/network-design11.png)

Ábra 12

Hibás fölé után a replika virtuális gép esetleg IP-címet, amely nem ugyanaz, mint az elsődleges virtuális gép IP-címét. Virtuális gépeken futó frissíti a DNS-kiszolgáló, amely használnak után induljanak el. DNS-bejegyzések jellemzően a módosítani vagy hálózatán kiürített rendelkezik, és hálózati táblázatokban gyorsítótárban tárolt bejegyzések kell frissítése vagy a kiürített, így nem kell projektvezetők legrövidebb leállás közben állapota a módosítások helyébe ritkán. Ez a probléma is szüntethető meg:

- Intranetes alkalmazások alacsony TTL (élettartam) értéket használja.
- Azure forgalom kezelő használata automatikus rendszer-Helyreállítás az internet alapú alkalmazások.
- Frissítse a DNS-kiszolgáló ahhoz, hogy egy időben frissítés (a parancsfájl nincs szükség esetén a dinamikus DNS-regisztrációhoz konfigurációja) a helyreállítási terv belül a következő parancsfájl használatával

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>Az IP-címek – az Azure DR módosítása

A [Microsoft Azure katasztrófa helyreállítási helyként infrastruktúra-beállítása hálózati](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) blogbejegyzés beállítása a szükséges Azure hálózati infrastruktúrát, amikor annak követelménye IP-címek megőrzése nem ismerteti. Azt az alkalmazást ismertető kezdődik, és keresse meg a hálózati helyszíni beállítása, valamint Azure, és hogyan próba feladatátvevő és a tervezett feladatátvevő majd megkötése.



## <a name="next-steps"></a>Következő lépések

[Ismerje meg, hogy](site-recovery-network-mapping.md) hogyan webhely helyreállítási térképek forrás- és célwebhelyek hálózatok amikor VMM kiszolgáló van használatban az elsődleges hely kezelését. 
