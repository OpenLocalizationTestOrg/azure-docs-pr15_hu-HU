## <a name="virtual-network-basics"></a>Virtuális hálózati alapjai

### <a name="what-is-an-azure-virtual-network-vnet"></a>Mi az az Azure virtuális hálózati (VNet)?

VNets segítségével rendelkezés és kezelheti virtuális magánhálózat (VPN) Azure-ban és, tetszés szerint hivatkozás a VNets az egyéb VNets Azure-ban vagy a helyszíni az informatikai infrastruktúrát hibrid vagy határokon helyszíni megoldások létrehozása. Minden VNet hoz létre saját CIDR blokk van, és mindaddig, amíg a CIDR blokkok nem ütközik más VNets és a helyszíni hálózatokon csatolható. Akkor is, a DNS-kiszolgáló beállításait VNets vezérlők, és az alhálózathoz a VNet a szegmens.

A VNets használja:

- Hozzon létre egy dedikált felhőben virtuális magánhálózaton

    Előfordul, hogy a megoldás a határokon helyszíni konfiguráció nincs szükség. Amikor létrehoz egy VNet, a szolgáltatások és a VNet belül VMs kommunikálni közvetlenül és biztonságos módon egymással a felhőben. Biztonságos belül a VNet forgalmat továbbra is, de továbbra is lehetővé teszi a VMs és a megoldás részeként internetes kommunikációs igénylő szolgáltatások végpont kapcsolatainak konfigurálása.

- Az Adatközpont biztonságosan bővítése

    A VNets, hagyományos webhely (S2S) VPN biztonságosan méretezni a adatközponthoz kapacitás hozhat létre. S2S VPN adatai IPSEC segítségével adja meg a vállalati virtuális Magánhálózati átjáró és Azure közötti biztonságos kapcsolatot.

- Hibrid felhő esetek engedélyezése

    VNets támogatja a hibrid felhő esetek adattartomány rugalmasan ad. A helyszíni rendszer, például nagyszámítógépeket és Unix rendszerek bármilyen típusú alkalmazásokat felhőalapú biztonságos módon lehet csatlakozni.

### <a name="how-do-i-know-if-i-need-a-virtual-network"></a>Honnan tudhatom, hogy szükségem van-e virtuális hálózatot?

Látogasson el a [Virtuális hálózat áttekintése](../articles/virtual-network/virtual-networks-overview.md) , amelynek segítségével eldöntheti, hogy a legjobb hálózati tervezés beállítás, döntési táblázat megjelenítéséhez.

### <a name="how-do-i-get-started"></a>Hogyan kezdjek hozzá?

Látogasson el [a virtuális rendszergazdához](https://azure.microsoft.com/documentation/services/virtual-network/) a kezdéshez. Erre a lapra a gyakori konfigurálási lépéseket, valamint információkat, amely segít megérteni az dolgot, amely a virtuális hálózat tervezésekor figyelembe kell mutató hivatkozásokat tartalmaz.

### <a name="what-services-can-i-use-with-vnets"></a>Milyen szolgáltatásokra van lehetőség a VNets?

Számos különböző Azure szolgáltatásai, például Cloud Services (PaaS), a virtuális gépeken futó és Web Apps alkalmazások VNets használható. Vannak azonban olyan néhány olyan szolgáltatásokat, a VNet nem támogatott. Ellenőrizze az adott szolgáltatás szeretne használni, és ellenőrizze, hogy azt kompatibilis.

### <a name="can-i-use-vnets-without-cross-premises-connectivity"></a>Használhatom-e VNets keresztté helyszíni kapcsolat nélkül?

igen. Egy VNet használata a webhelyen a webhely-kapcsolat nélkül is használhatja. Az különösen akkor hasznos, ha a használni kívánt tartományt vezérlők és a SharePoint-farmokon Azure-ban.

## <a name="virtual-network-configuration"></a>Virtuális hálózat konfigurálása

### <a name="what-tools-do-i-use-to-create-a-vnet"></a>Milyen eszközök segítségével hozzon létre egy VNet?

A következő eszközök segítségével létrehozása vagy egy virtuális hálózat konfigurálása:

- Azure Portal (klasszikus- és erőforrás-kezelő VNets).

- A hálózati konfigurációs fájl (netcfg – csak a klasszikus VNets). [A hálózati konfigurációs fájl segítségével virtuális hálózat konfigurálása](../articles/virtual-network/virtual-networks-using-network-configuration-file.md)című témakört.

- A PowerShell (klasszikus- és erőforrás-kezelő VNets).

- Azure CLI (klasszikus- és erőforrás-kezelő VNets).

### <a name="what-address-ranges-can-i-use-in-my-vnets"></a>Milyen címtartományok használhatom a saját VNets?

Nyilvános IP-címtartományok és a bármely IP-címtartományokat [RFC 1918](http://tools.ietf.org/html/rfc1918)meghatározott is használhatja.

### <a name="can-i-have-public-ip-addresses-in-my-vnets"></a>Lehet-e nyilvános IP-címek a VNets az?

igen. Nyilvános IP-címtartományok kapcsolatos további tudnivalókért olvassa el a [nyilvános IP-címterület a virtuális hálózatban (VNet)](../articles/virtual-network/virtual-networks-public-ip-within-vnet.md)című témakört. Ne feledje, hogy a nyilvános IP-címei nem lesz az internetről közvetlenül elérhető.

### <a name="is-there-a-limit-to-the-number-of-subnets-in-my-virtual-network"></a>Van-e a virtuális hálózatomban alhálózat száma korlátozott?

Nincs a korlát belül egy VNet használt alhálózat nem. Minden alhálózat teljesen tartalmaznia kell a virtuális hálózati címterületet és kell nem áll átfedésben összekapcsolódjon.

### <a name="are-there-any-restrictions-on-using-ip-addresses-within-these-subnets"></a>Van valamilyen korlátozás, ezek alhálózat belüli IP-címek használatáról?

Azure fenntartja néhány IP-címek minden alhálózathoz belül. Az első és utolsó IP-címek a alhálózat protokoll megfelelőség együtt 3 több címet, használja az Azure-szolgáltatások számára vannak fenntartva.

### <a name="how-small-and-how-large-can-vnets-and-subnets-be"></a>Hogyan lehet a kis- és milyen nagy VNets és alhálózat kell?

A legkisebb alhálózat támogatjuk egy /29 pedig a legnagyobb olyan /8 (CIDR alhálózat definíciók használata).

### <a name="can-i-bring-my-vlans-to-azure-using-vnets"></a>Hozhatom át a VLAN az Azure VNets használatával?

nem. VNets réteg-3-as átfedi. Azure minden réteg-2 szemantikáját nem támogatja.

### <a name="can-i-specify-custom-routing-policies-on-my-vnets-and-subnets"></a>Is egyéni útválasztási házirendek is megadhatja saját VNets és alhálózat?

igen. Felhasználó által definiált Routing (UDR) is használhatja. UDR kapcsolatos további tudnivalókért látogassa meg a [felhasználó által definiált útvonalak és IP-továbbítási](../articles/virtual-network/virtual-networks-udr-overview.md).

### <a name="do-vnets-support-multicast-or-broadcast"></a>Csoportos vagy közvetítési támogatják VNets?

nem. Nem támogatott diagramtípusról csoportos vagy szórás.

### <a name="what-protocols-can-i-use-within-vnets"></a>Milyen protokollok van lehetőség VNets belül?

Szabványos IP-alapú protokollok VNets belül is használhatja. Azonban csoportos közvetítési, IP-a-IP-beágyazott csomagok általános útválasztás beágyazást (es) csomagok blokkolt és VNets belül. Normál munka tartalmazó protokollok:

- TCP
- UDP
- ICMP

### <a name="can-i-ping-my-default-routers-within-a-vnet"></a>Az alapértelmezett útválasztó belül egy VNet is ping?

nem.

### <a name="can-i-use-tracert-to-diagnose-connectivity"></a>Tracert segítségével kapcsolatot diagnosztizálása?

nem.

### <a name="can-i-add-subnets-after-the-vnet-is-created"></a>Vehetek fel alhálózat a VNet létrehozása után?

igen. Alhálózat bővíthető VNets bármikor mindaddig, amíg az alhálózathoz cím nem része a VNet a másik alhálózat.

### <a name="can-i-modify-the-size-of-my-subnet-after-i-create-it"></a>Is lehet módosítani a alhálózat méretének után lehet létrehozni a dokumentumot?

Hozzáadása, eltávolítása, bontsa ki a vagy kisebb alhálózat, ha nincsenek benne PowerShell-parancsmagok vagy a NETCFG fájl használatával telepített szolgáltatások vagy VMs. Is hozzáadása, eltávolítása, kibonthatja és bármely prefixumokban a Lekicsinyítve, mindaddig, amíg a alhálózat VMs vagy szolgáltatásokat tartalmazó módosítása nem érinti.

### <a name="can-i-modify-subnets-after-i-created-them"></a>Is lehet módosítani a alhálózat lehet őket létrehozása után?

igen. Hozzáadása, eltávolítása és módosítása a VNet által használt CIDR blokkok.

### <a name="can-i-connect-to-the-internet-if-i-am-running-my-services-in-a-vnet"></a>Tudok csatlakozni az interneten, ha egy VNet a saját szolgáltatások használom?

igen. Egy VNet belül telepített szolgáltatások csatlakozhat az internethez. Minden felhőszolgáltatásba telepítését az Azure van rendelve egy nyilvánosan címmel rendelkező virtuális. Be kell beviteli végpontokat PaaS szerepkörök és virtuális gépeken futó engedélyezése az internetről kapcsolatok fogadja el az alábbi szolgáltatások végpontok megadása.

### <a name="do-vnets-support-ipv6"></a>Végezze el VNets támogatja az IPv6-ot?

nem. Az IPv6 VNets egyelőre nem használható.

### <a name="can-a-vnet-span-regions"></a>Egy VNet régiók is kiterjedhet?

nem. Egy VNet korlátozódik egyetlen területére.

### <a name="can-i-connect-a-vnet-to-another-vnet-in-azure"></a>Tudok csatlakozni a VNet egy másik VNet Azure-ban?

igen. VNet kommunikációs VNet REST API-hoz vagy a Windows PowerShell használatával hozhat létre. VNets VNet Peering keresztül is csatlakozhat. Többet szeretne tudni a peering [itt.](../articles/virtual-network/virtual-network-peering-overview.md)

## <a name="name-resolution-dns"></a>A névfeloldás (DNS)

### <a name="what-are-my-dns-options-for-vnets"></a>Mik azok a DNS-beállítások az VNets?

A [Névfeloldás VMs és szerepkör-példányok](../articles/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) lapon a döntési táblázat segítségével végig az elérhető összes DNS-beállításai.

### <a name="can-i-specify-dns-servers-for-a-vnet"></a>Lehet-e meg a DNS-kiszolgálók egy VNet?

igen. Megadhatja a DNS-kiszolgáló IP-címek a VNet beállításait. Ez a VNet az összes VMs a alapértelmezett DNS-kiszolgálói lesz érvényes.

### <a name="how-many-dns-servers-can-i-specify"></a>Hány DNS-kiszolgálók adhat meg?

Megadhatja, hogy 12 DNS-kiszolgálók.

### <a name="can-i-modify-my-dns-servers-after-i-have-created-the-network"></a>Is lehet módosítani a DNS-kiszolgálók lehet a hálózat létrehozása után?

igen. A DNS-kiszolgálók listáját bármikor módosíthatja a VNet. Ha módosítja a DNS-kiszolgálók listáját, szüksége lesz a VMs mindegyikére indítsa újra a VNet jegyzetcímkéket válassza az új DNS-kiszolgáló.


### <a name="what-is-azure-provided-dns-and-does-it-work-with-vnets"></a>Mi az Azure által biztosított DNS és működik VNets?

Azure által biztosított DNS a Microsoft által nyújtott szolgáltatás több bérlői DNS. Azure Ez a szolgáltatás VMs és szerepkör-példányok regisztrálása. Ez a szolgáltatás nyújt névfeloldás hostname (állomásnév) VMs és szerepkör-példányok tartalmazott a egy felhőalapú szolgáltatásba, és a FQDN VMs és a példányok szerepkör az azonos VNet.

> [AZURE.NOTE] Idegen-bérlői névfeloldás Azure által biztosított DNS használata az első 100 felhőszolgáltatásokhoz a virtuális hálózaton most korlátozása van. Ha a saját DNS-kiszolgálót használ, a korlátozás nem vonatkozik.

### <a name="can-i-override-my-dns-settings-on-a-per-vm--service-basis"></a>A használati-virtuális a saját DNS-beállítások felülbírálása / alap szolgáltatás?

igen. Beállíthatja, hogy a DNS-kiszolgálók alapon egy felhőalapú szolgáltatás bírálja felül az alapértelmezett hálózati beállítások. Azonban azt javasoljuk, hogy használja-e a lehető hálózati szintű DNS.

### <a name="can-i-bring-my-own-dns-suffix"></a>A saját DNS-utótag is átvihet?

nem. A VNets egyéni DNS-utótag adható meg.

## <a name="vnets-and-vms"></a>VNets és VMs

### <a name="can-i-deploy-vms-to-a-vnet"></a>Telepíthetem-e VMs egy VNet?

igen.

### <a name="can-i-deploy-linux-vms-to-a-vnet"></a>Telepíthetem-e Linux VMs egy VNet?

igen. A Linux Azure által támogatott bármely distro telepítheti.

### <a name="what-is-the-difference-between-a-public-vip-and-an-internal-ip-address"></a>Mi a különbség egy nyilvános virtuális és belső IP-címének között?

- Belső IP-címének DHCP van rendelve egy VNet belül minden virtuális IP-címet. Még nem nyilvános elérhető. Ha létrehozott egy VNet, a belső IP-címének van-e hozzárendelve a tartományban, amely a VNet alhálózat beállításaiban megadott. Ha nem rendelkezik egy VNet, belső IP-címének továbbra is kiosztani. A belső IP-cím marad a virtuális a az élettartam, kivéve, ha, amely a virtuális felszabadítása.

- Egy nyilvános virtuális a nyilvános IP-címet, amely a felhőalapú szolgáltatás vagy betöltése terheléselosztó hozzá van rendelve. Nem hozzárendelték közvetlenül a virtuális adaptert A virtuális mutatni a felhőbeli szolgáltatástól hozzá van rendelve addig, amíg az összes VMs abban a felhőbeli szolgáltatástól felszabadítása vagy törölt. Ezen a ponton megjelent.

### <a name="what-ip-address-will-my-vm-receive"></a>Milyen IP-címet fog kapni a virtuális?

- **Belső IP-cím –** Ha egy virtuális egy VNet rendszerbe, a virtuális belső IP-címek megadott készletből kapja meg belső IP-címet. VMs bonyolítsanak belül a VNets belső IP-címet. Bár az Azure rendel egy dinamikus belső IP-címet, a virtuális statikus cím kérhet. Többet szeretne tudni a belső IP-címek, látogasson el a [hogyan kell beállítani a statikus belső IP-cím](../articles/virtual-network/virtual-networks-reserved-private-ip.md).

- **Virtuális-** A virtuális is társítva egy virtuális, bár a virtuális soha nem tartozik a virtuális közvetlenül. A virtuális egy nyilvános IP-címet a felhőalapú szolgáltatás rendelt. Ha szükséges, foglalhat egy virtuális a felhőalapú szolgáltatáshoz.

- **ILPIP-** Egy példány szintű nyilvános IP-cím (ILPIP) is beállítható. ILPIPs vannak közvetlenül a felhőbeli szolgáltatástól, hanem a virtuális társított. Többet szeretne tudni a ILPIPs, látogasson el a [Példány szintű nyilvános IP-áttekintése](../articles/virtual-network/virtual-networks-instance-level-public-ip.md).

### <a name="can-i-reserve-an-internal-ip-address-for-a-vm-that-i-will-create-at-a-later-time"></a>Lehet-e foglalni belső IP-címet a virtuális, amely lehet hozza létre, várjon egy kis időt?

nem. Belső IP-cím nem lehet foglalni. Ha érhető el egy belső IP-cím azt rendeli hozzá egy virtuális vagy szerepkör-példány a útja szerint. A virtuális is, illetve nem lehet a hozzárendelni kívánt belső IP-címét, amelyet egy. Azonban módosíthatja a belső egy korábban létrehozott virtuális IP-címe minden elérhető belső IP-címére.

### <a name="do-internal-ip-addresses-change-for-vms-in-a-vnet"></a>Végezze el az egy VNet VMs belső IP-címek módosulása?

igen. Belső IP-címek még a virtuális a élettartama kivéve, ha a virtuális felszabadítása. Felszabadítása egy virtuális, amikor a belső IP-címének fejlesztésének van, kivéve, ha megadott egy belső statikus IP-címet a virtuális. Ha a virtuális egyszerűen leállítja (és nem helyezi el az állapot **Leállítva (Deallocated)**) az IP-címet a virtuális hozzárendelt marad.

### <a name="can-i-manually-assign-ip-addresses-to-nics-in-vms"></a>E manuálisan rendelhet IP-címek VMs hálózati kártyákat?

nem. Minden kapcsolat tulajdonságai VMs nem módosítható. Módosítások vezethet esetleg elvesztését a virtuális kapcsolat.

### <a name="what-happens-to-my-ip-addresses-if-i-shut-down-a-vm"></a>Mi történik az IP-címek, ha egy virtuális leállítása lehet?

Semmi sem. Az IP-címek (nyilvános virtuális és belső IP-címét) virtuális vagy a felhőbeli szolgáltatástól lesz látható.

> [AZURE.NOTE] Ha azt szeretné, egyszerűen leállítja a virtuális, ezt nem használja az adatkezelési portálon. Jelenleg a Leállítás gombra a virtuális gép fog felszabadítani.

### <a name="can-i-move-vms-from-one-subnet-to-another-subnet-in-a-vnet-without-re-deploying"></a>Áthelyezhetők-e VMs egy alhálózat egy VNet a másik alhálózathoz újra telepítése nélkül?

igen. További információt találhat [az alábbi](../articles/virtual-network/virtual-networks-move-vm-role-to-subnet.md).

### <a name="can-i-configure-a-static-mac-address-for-my-vm"></a>Is állítható be a statikus MAC-címet a virtuális?

nem. A MAC-cím statikus nem állítható be.

### <a name="will-the-mac-address-remain-the-same-for-my-vm-once-it-has-been-created"></a>A MAC-cím változatlan marad a virtuális létrehozása után?

Igen, a MAC-cím változatlan marad a virtuális annak ellenére, hogy a virtuális leállítva (deallocated) és az s.

### <a name="can-i-connect-to-the-internet-from-a-vm-in-a-vnet"></a>Tudok csatlakozni az internetről származó egy virtuális egy VNet az?

igen. Egy VNet belül telepített szolgáltatások csatlakozhat az internethez. Ezenkívül minden felhőszolgáltatásba telepítését az Azure van rendelve egy nyilvánosan címmel rendelkező virtuális. Meg kell adnia a bemeneti végpontokat PaaS szerepkörök és engedélyezése az internetről kapcsolatok fogadja el az alábbi szolgáltatások VMs végpontok.

## <a name="vnets-and-services"></a>VNets és szolgáltatások

### <a name="what-services-can-i-use-with-vnets"></a>Milyen szolgáltatásokra van lehetőség a VNets?

Számítási szolgáltatások VNets belül csak használható. Számítási szolgáltatások korlátozódik Cloud Services (webes és dolgozó szerepkörök) és VMs.

### <a name="can-i-use-web-apps-with-virtual-network"></a>Használhatom-e Web Apps alkalmazások virtuális hálózatot?

igen. Telepítheti a Web Apps alkalmazások belül egy VNet ASE (alkalmazás szolgáltatási környezetben) segítségével. Vesz fel, amelyek, Web Apps alkalmazások is biztonságosan csatlakozás és a hozzáférési az Azure VNet erőforrásainak pont-webhely be van állítva a VNet esetén. További tudnivalókért lásd: a következőket:


- [Az alkalmazás-szolgáltatási környezetben Web Apps alkalmazások létrehozása](../articles/app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)

- [Web Apps alkalmazások virtuális hálózati integrációval](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)

- [A VNet integráció és a hibrid kapcsolatok Web Apps alkalmazások használata](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)

- [Egy webalkalmazás integrálása az Azure virtuális hálózaton](../articles/app-service-web/web-sites-integrate-with-vnet.md)

### <a name="can-i-deploy-cloud-services-with-web-and-worker-roles-paas-in-a-vnet"></a>Telepítheti a felhőalapú szolgáltatások webes és dolgozó szerepkörök (PaaS) egy VNet?

igen. PaaS szolgáltatások VNets belül telepítheti.

### <a name="how-do-i-deploy-paas-roles-to-a-vnet"></a>Hogyan telepíthető PaaS szerepkörök szeretne egy VNet?

Ez a szolgáltatás konfigurálása a hálózat konfigurálása csoportban a VNet nevét és a /subnet szerepkör-hozzárendelések megadásával végezheti el. Nem kell frissíteni a bináris közül.

### <a name="can-i-move-my-services-in-and-out-of-vnets"></a>Áthelyezhetők-e saját szolgáltatások és kijelentkezés VNets?

nem. Nem tudja áthelyezni és kijelentkezés VNets szolgáltatásokat. Meg kell törlése, majd újból telepíteni az áthelyezheti egy másik VNet a szolgáltatás.

## <a name="vnets-and-security"></a>VNets és biztonság

### <a name="what-is-the-security-model-for-vnets"></a>Mi az a biztonsági modell VNets?

VNets elkülönülnek teljesen egymásnak, és az egyéb szolgáltatásokat az Azure infrastruktúrára is. Egy VNet adatvédelmi oszlopazonosító jobboldali.

### <a name="can-i-define-acls-or-nsgs-on-my-vnets"></a>Is lehet definiálása hozzáférés-vezérlési listák vagy NSGs a saját VNets?

nem. Hozzáférés-vezérlési listák vagy a VNets NSGs nem lehet rendelni. Azonban a hozzáférés-vezérlési listák adható meg a bemeneti végpontok egy VNets telepített VMs elemre, és NSGs alhálózat vagy NIC társítható.

### <a name="is-there-a-vnet-security-whitepaper"></a>Van-e egy VNet biztonsági szeretne?

igen. Letöltheti azt [az alábbi](http://go.microsoft.com/fwlink/?LinkId=386611).

## <a name="apis-schemas-and-tools"></a>API-khoz sémák és eszközök

### <a name="can-i-manage-vnets-from-code"></a>Lehet kezelni VNets kódból?

igen. VNets és idegen helyszíni kapcsolatszolgáltatások kezeléséhez REST API-khoz is használhatja. További információt talál [az alábbi](http://go.microsoft.com/fwlink/?LinkId=296833).

### <a name="is-there-tooling-support-for-vnets"></a>Van-e VNets szerszámok támogatása?

igen. Platformokon sokféle PowerShell-és parancsot is használhatja. További információt talál [az alábbi](http://go.microsoft.com/fwlink/?LinkId=317721).
