<properties
   pageTitle="Azure virtuális hálózati (VNet) megtervezése és a tervezési útmutató |} Microsoft Azure"
   description="Megtudhatja, hogy miként tervezése és kialakítása a elkülönítési, a kapcsolatok és a hely igényeknek megfelelően alakítható Azure virtuális hálózatokat."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/08/2016"
   ms.author="jdial" />

# <a name="plan-and-design-azure-virtual-networks"></a>Azure virtuális hálózatok megtervezése

Kísérletezés a egy VNet létrehozása elég egyszerűen, de valószínűleg több VNets telepíti a gyártási igényeinek a szervezet támogatási időbeli. Az egyes tervezése és kialakítása VNets telepítése és csatlakozás a hatékonyabb szükséges erőforrások fogja. Ha nem ismeri a VNets, az alábbi helyeken ajánlott, hogy [VNets ismertetése](virtual-networks-overview.md) és [telepítéséről](virtual-networks-create-vnet-arm-pportal.md) egy a folytatás előtt. 

## <a name="plan"></a>Terv

Azure előfizetések, a régiók és a hálózati erőforrások alapos ismerete fontos a sikerhez. A lista alatti szempontok kiindulópontként is használhatja. Ha ilyen kérdései megértette, a követelmények határozhatja meg a hálózati tervezésekor.

### <a name="considerations"></a>Megfontolandó szempontok

Mielőtt a kérdések megválaszolása a tervezés alatt, vegye figyelembe a következőket:

- Azure-ban létrehozott minden tevődik össze egy vagy több erőforrásokat. Virtuális gép (virtuális) erőforrás, a hálózati adapteren (hálózati) által használt felület egy virtuális pedig egy erőforrás, a hálózati kártya által használt nyilvános IP-címét az erőforrás, a VNet a hálózati kártya csatlakozik egy erőforrás.
- Erőforrások belül az [Azure régió](https://azure.microsoft.com/regions/#services) és az előfizetés hoz létre. És erőforrások csak egy adott ugyanazt a terület és azok előfizetés megtalálható VNet kapcsolhat össze. 
- Az Azure [Virtuális Magánhálózati átjáró](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)segítségével csatlakoztathatja VNets egymással. Is csatlakozhat VNets különböző régiók és előfizetés ilyen módon.
- Azure-ban elérhető [csatlakozási beállítások](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) valamelyikével VNets csatlakoztathatja a helyszíni hálózaton. 
- Más erőforrások csoportosíthatók [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups), így könnyebben kezelheti az erőforrás egy egységként. Erőforráscsoport tartalmazhat több területéről erőforrások, mindaddig, amíg az erőforrások tartoznak, az azonos előfizetésbe.

### <a name="define-requirements"></a>Követelmények meghatározása

Az alábbi kérdésekre használja kiindulási pontként az Azure hálózattervezés.  

1. Milyen Azure helyek használ állomáshoz VNets?
2. Szükség van ezekre a helyekre Azure közötti kommunikáció számára?
3. Szükség van az Azure VNet(s) és a helyszíni datacenter(s) közötti kommunikáció számára?
4. Hány infrastruktúra szolgáltatást (IaaS) VMs, mint cloud services szerepkörök és web Apps alkalmazások történjen a megoldás van szüksége?
5. Szükség van (azaz előtér-webkiszolgálón és hátsó vége adatbázis-kiszolgálók) VMs csoportjai alapján forgalom elkülönítése?
6. Szükség van forgalom folyamat virtuális készülékek használatával szabályozhatja?
7. Felhasználók szükség különböző csoportjaihoz különböző Azure erőforrásokhoz engedélyek?

### <a name="understand-vnet-and-subnet-properties"></a>VNet és alhálózat tulajdonságok ismertetése

VNet és alhálózat forrásokkal az Azure-ban futó feladatok biztonsági oszlopazonosító jobboldali határozza meg. Egy VNet jellemzi cím szóközök, jelenti CIDR blokkok gyűjteménye. 

>[AZURE.NOTE] Hálózati rendszergazda jól ismert CIDR alakban. Ha nem ismeri a CIDR, [kaphat részletesebb tájékoztatást](http://whatismyipaddress.com/cidr).

VNets a következő tulajdonságokat tartalmazza.

|A tulajdonság|Leírás|Korlátozások|
|---|---|---|
|**név**|VNet neve|Legfeljebb 80 karakterlánc. Betűk, számok, aláhúzás, időszakok vagy kötőjeleket tartalmazhat. Kell kezdődnie egy betűt vagy számot. Kell végződnie betű-, szám, vagy aláhúzás. Felső vagy a nagybetűk is tartalmaz.|  
|**hely**|Azure helye (más néven terület).|Kell lennie az érvényes Azure helyek közül.|
|**addressSpace**|Cím prefixumokban a VNet CIDR jelöléssel alkotó gyűjteménye.|Érvényes CIDR címterületet, beleértve a nyilvános IP-címtartományok tömbjét kell lennie.|
|**alhálózat**|A VNet alkotó alhálózat gyűjteménye|Lásd a lenti alhálózat tulajdonságok-táblázatot.||
|**dhcpOptions**|Objektum, amely tartalmaz egy-egy szükséges tulajdonság neve **dnsServers**.||
|**dnsServers**|A VNet által használt DNS-kiszolgálók tömbje. Ha nincs megadva kiszolgáló, Azure belső névfeloldás használják.|Legfeljebb 10 DNS-kiszolgálók, IP-cím tömbjét kell lennie.| 

Alhálózat egy VNet gyermek erőforrást, és segít definiálása szegmensek cím szóközök belül egy CIDR szövegrészt, IP-cím prefixumokban használatával. Hálózati kártyák alhálózat hozzáadva, és kapcsolatot biztosító különböző munkaterhelésekből VMs csatlakozik.

Alhálózat a következő tulajdonságokat tartalmazza. 

|A tulajdonság|Leírás|Korlátozások|
|---|---|---|
|**név**|Alhálózat neve|Legfeljebb 80 karakterlánc. Betűk, számok, aláhúzás, időszakok vagy kötőjeleket tartalmazhat. Kell kezdődnie egy betűt vagy számot. Kell végződnie betű-, szám, vagy aláhúzás. Felső vagy a nagybetűk is tartalmaz.|
|**hely**|Azure helye (más néven terület).|Kell lennie az érvényes Azure helyek közül.|
|**addressPrefix**|A alhálózat jelölés CIDR alkotó egyetlen cím előtag|Egy egyetlen CIDR szövegrészt a VNet cím szóközöket egyik részét képező kell lennie.|
|**networkSecurityGroup**|Az alhálózathoz alkalmazott NSG|Lásd: [NSGs](resource-groups-networking.md#Network-Security-Group)|
|**routeTable**|Az alhálózathoz alkalmazott útvonal táblázat|Lásd: [UDR](resource-groups-networking.md#Route-table)|
|**ipConfigurations**|Az alhálózathoz csatlakoztatott NIC által használt IP-konfigurációs objektumok gyűjteménye|Lásd: az [IP-beállítások](../resource-groups-networking.md#IP-configurations)|

### <a name="name-resolution"></a>Névfeloldás

Alapértelmezés szerint a VNet használja [Azure által biztosított névfeloldás.](virtual-networks-name-resolution-for-vms-and-role-instances.md#Azure-provided-name-resolution) a VNet belül, és a nyilvános internetkapcsolat a névfeloldásra. Azonban a VNets csatlakozik a helyszíni adatközpontokban, ha meg kell adja meg [a saját DNS-kiszolgáló](virtual-networks-name-resolution-for-vms-and-role-instances.md#Name-resolution-using-your-own-DNS-server) névfeloldás a hálózatok között.  

### <a name="limits"></a>Korlátai

Tekintse át a hálózati korlátozások az [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) a cikkben annak érdekében, hogy a tervező nem ütközik a korlátok közül. Bizonyos korlátozások úgy, hogy megnyitja a támogatási jegyek növelhető.

### <a name="role-based-access-control-rbac"></a>Szerepköralapú hozzáférés-szerepalapú

[Azure RBAC](../active-directory/role-based-access-built-in-roles.md) szabályozhatja a különböző felhasználók lehetnek az Azure különböző erőforrásokra hozzáférési szintet is használhatja. Ezzel a módszerrel is újból a igényeik alapján csoport által végzett munkát. 

Virtuális hálózatok illeti a felhasználók a **Hálózati munkatársi** szerepkörök erőforrás-kezelő Azure virtuális hálózati erőforrások teljes hozzáféréssel rendelkezik. A **Klasszikus hálózati közreműködői** szerepkör-felhasználók hasonlóan a klasszikus virtuális hálózati erőforrások teljes hozzáféréssel rendelkeznek.

>[AZURE.NOTE] Is [létrehozhat saját szerepköröket](../active-directory/role-based-access-control-configure.md) külön felügyeleti igényeinek megfelelően.

## <a name="design"></a>Tervező

Ha már elsajátította [megtervezése](#Plan) szakaszában a kérdésekre adott válaszok, olvassa el a VNets megadása előtt a következőket.

### <a name="number-of-subscriptions-and-vnets"></a>Előfizetések és VNets száma

Vegye figyelembe az alábbi helyzetekben a több VNets létrehozása:

- **Azure különböző helyeken helyét igénylő VMs**. Azure-ban VNets területi. Ezek nem időtartomány helyek. Legalább egy VNet tehát minden kívánt host VMs az Azure hely szüksége.
- **Munkaterhelésekből, amely kell teljesen elszigetelt egymástól**. Azonos IP-cím szóközöket is használhatja az egymástól eltérő munkaterhelésekből elkülöníteni VNets hozhat létre. 

Ne feledje, hogy a lásd a fenti korlátozások egy régió száma előfizetésenként. Ez azt jelenti, hogy több előfizetéssel segítségével növelje erőforrások karbantarthatja Azure-ban. A webhely VPN vagy -készült ExpressRoute áramkör, amelyhez csatlakozni VNets különböző előfizetések használható.

### <a name="subscription-and-vnet-design-patterns"></a>Előfizetés és VNet Tervező mintázatok

Az alábbi táblázatban néhány gyakori tervezés mintázatok az előfizetések és VNets látható.

|Eset|Diagram|Szakemberek|Negatívumokat|
|---|---|---|---|
|Egyetlen előfizetését, egy alkalmazás két VNets|![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure1.png)|Csak egy előfizetés kezeléséhez.|Maximális száma VNets Azure régió. További előfizetéseket követően, hogy szüksége van. Tekintse át az [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) a cikk további információt.|
|Egy alkalmazást, az alkalmazás egy két VNets egy előfizetéssel|![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure2.png)|Csak két VNets előfizetésenként használja.|Ha túl sok alkalmazások kezeléséhez nehezebb.|
|Több előfizetés egységnyi üzleti alkalmazás egy két VNets.|![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure3.png)|Egyenleg előfizetések száma és VNets között.|Maximális száma VNets részleg (előfizetés). Tekintse át az [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) a cikk további információt.|
|Több előfizetés egységnyi üzleti alkalmazások csoportonként két VNets.|![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure4.png)|Egyenleg előfizetések száma és VNets között.|Alkalmazások kell elkülöníteni alhálózat és NSGs használatával.|


### <a name="number-of-subnets"></a>Alhálózat száma

Vegye figyelembe az alábbi helyzetekben a VNet a több alhálózat:

- **Az összes hálózati kártyákat alhálózathoz nem elég saját IP-címek**. Az alhálózathoz címterület a NICS számát az alhálózathoz nem tartalmaz elég IP-címek, ha több alhálózat létrehozásához szükséges. Ne feledje, hogy az Azure fenntartja minden alhálózathoz nem használható az 5 saját IP-címek: az első és utolsó címe (az alhálózat címét, és a csoportos) a címterület és a 3 címek belső (célokra használhatók DHCP és a DNS). 
- **Biztonsági**. Alhálózat használjon a csoportok VMs egymástól-munkaterhelésekből, amelyek több réteghez szerkezetet, és másik [hálózati biztonsági csoportok (NSGs)](virtual-networks-nsg.md#subnets) az adott alhálózat alkalmazása.
- **Hibrid kapcsolatot**. Használhatja VPN átjárók és készült ExpressRoute áramkörök [kapcsolódni](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) a VNets egymásnak, és a helyszíni adatok center(s). Virtuális Magánhálózati átjárók és készült ExpressRoute áramkörök saját létrehozandó alhálózat igényelnek
- **Virtuális készülékek**. A-Azure VNet egy virtuális készülék, például egy tűzfal, a WAN accelerator vagy a virtuális Magánhálózati átjáró is használhatja. Ha így tesz, ezek készülékek [útvonal forgalom](virtual-networks-udr-overview.md) kell, de elkülöníteni őket a saját alhálózat.

### <a name="subnet-and-nsg-design-patterns"></a>Alhálózat és NSG tervezés mintázatok

Az alábbi táblázatban látható néhány gyakori tervezés mintázatok alhálózat használatához.

|Eset|Diagram|Szakemberek|Negatívumokat|
|---|---|---|---|
|Egyetlen alhálózat NSGs minden alkalmazási réteg egy alkalmazás|![Egyetlen alhálózat](./media/virtual-network-vnet-plan-design-arm/figure5.png)|Csak egy alhálózat kezeléséhez.|Több NSGs szükséges elkülönítésére mindegyik alkalmazásra.|
|Egy alkalmazás, a használati alkalmazási réteg NSGs egy alhálózat|![Egy alkalmazás alhálózat](./media/virtual-network-vnet-plan-design-arm/figure6.png)|Kevesebb NSGs kezeléséhez.|Több alhálózat kezeléséhez.|
|Egy alhálózat minden alkalmazási réteg, egy alkalmazás NSGs.|![Alhálózat száma rétegen](./media/virtual-network-vnet-plan-design-arm/figure7.png)|Egyenleg alhálózat száma és NSGs között.|NSGs maximális száma előfizetésenként. Tekintse át az [Azure korlátozza](../azure-subscription-service-limits.md#networking-limits) a cikk további információt.|
|Minden alkalmazási réteg egy alkalmazás, a használati alhálózat NSGs egy alhálózat|![Egy réteg egy alkalmazás alhálózat](./media/virtual-network-vnet-plan-design-arm/figure8.png)|Esetleg kisebb számot NSGs.|Több alhálózat kezeléséhez.|

## <a name="sample-design"></a>Minta Tervező

Az alkalmazás Ez a cikk az adatok ábrázolására, a következő forgatókönyvvel.

Olyan céget, 2 adatközpontokban Észak-Amerikában, és két adatközpontokban Europe dolgozik. 6 különböző felhasználói szemben lévő alkalmazások különböző 2 által fenntartott azonosította, mint próbaüzem Azure áttelepítése kívánt részlegek. Az alkalmazások egyszerű architektúrája a következők:

- App1, App2, App3 és App4 olyan kiszolgálókon Linux Ubuntu futó webalkalmazások. Egyes alkalmazások csatlakozik egy külön alkalmazáskiszolgáló tároló RESTful szolgáltatások Linux kiszolgálókon. A RESTful szolgáltatások csatlakozás vissza vége MySQL-adatbázishoz.
- App5 és App6 olyan kiszolgálókon Windows fut a Windows Server 2012 R2 webalkalmazások. Egyes alkalmazások vissza vége SQL Server-adatbázishoz csatlakozik.
- Minden alkalmazás jelenleg tárolja az egyik Észak-Amerikában a vállalat adatközpontokban.
- A helyszíni adatközpontokban az 10.0.0.0/8 címterület használja.

Kell megtervezni a virtuális hálózati megoldást, amely megfelel az alábbi követelmények:

- Minden egyes részleg erőforrás-felhasználás más részlegek nem érinti.
- Meg kell a lehető legkevesebb VNets és alhálózat kezelését.
- Minden egyes részleg egyetlen próba/fejlesztési alkalmazásokhoz használt VNet kell rendelkeznie.
- Egyes alkalmazások 2 különböző Azure adatközpontokban per földrész (Észak-Amerika és Európa) helyezkedik el.
- Egyes alkalmazások teljesen elszigetelt egymástól.
- Egyes alkalmazások elérhetők, vevők az interneten HTTP használatával.
- Egyes alkalmazások érhetők el egy titkosított alagutas használatával a helyszíni adatközpontokban csatlakozó felhasználók által.
- Kapcsolat a helyszíni adatközpontokban meglévő virtuális Magánhálózati eszközök kell használni.
- A cég hálózati csoport a VNet konfiguráció teljes hozzáféréssel kell rendelkeznie.
- Láthatja, hogy az egyes részleg fejlesztők csak VMs telepítése a meglévő alhálózathoz.
- Összes alkalmazás áttelepítendőként, mint az Azure (felvonó-és-shift).
- Az adatbázisok minden olyan helyen más Azure helyekre kell bizonyos egyszer.
- Egyes alkalmazások 5 előtér-webkiszolgálón, a 2 alkalmazáskiszolgálók (ha szükséges) és az adatbázis-2 kiszolgálók kell használni.

### <a name="plan"></a>Terv

El kell indítania az alább látható módon a [meghatározás követelmények](#Define-requirements) szakaszban a kérdés megválaszolásával tervezési tervezés.

1. Milyen Azure helyek használ állomáshoz VNets?

    Észak-Amerikában 2 helyek és 2 helyekről Európában. Meg kell válassza ki azokat a meglévő helyszíni adatközpontokban fizikai helye alapján. Ezzel a módszerrel a kapcsolat a fizikai helyekről Azure lesz egy jobb késés.

2. Szükség van ezekre a helyekre Azure közötti kommunikáció számára?

    igen. Mivel az adatbázisok összes helyekre replikált kell lennie.

3. Szükség van az Azure VNet(s) és a helyszíni közötti kommunikáció a szükséges adatok center(s)?

    igen. Mivel a csatlakozó felhasználók a helyszíni adatközpontokban hozzáférhet az alkalmazások között egy titkosított alagutas kell lennie.
 
4. Hány IaaS VMs van szüksége a megoldás?

    200 IaaS VMs. App1, App2 és App3 megkövetelése minden 5 webkiszolgálón, 2 alkalmazások kiszolgálók minden és 2 adatbázis-kiszolgálók minden. Ez az alkalmazás / 9 IaaS VMs vagy 36 IaaS VMs összesen. App5 és App6 5 webkiszolgálón és az adatbázis-2 kiszolgálók minden szükség. Ez az alkalmazás / 7 IaaS VMs vagy 14 IaaS VMs összesen. 50 IaaS VMs ennélfogva minden Azure tartományban lévő összes alkalmazás szüksége. 4 régiók használatához szükséges, mivel 200 IaaS VMs lesz.

    Akkor is kell a szükséges DNS-kiszolgálók minden VNet, illetve a helyszíni adatközpontokban feloldani nevet az Azure IaaS VMs és a helyszíni hálózaton között. 

5. Szükség van (azaz előtér-webkiszolgálón és hátsó vége adatbázis-kiszolgálók) VMs csoportjai alapján forgalom elkülöníteni?

    igen. Egyes alkalmazások teljesen a egymástól elszigetelt kell lennie, és minden alkalmazási réteg is kell elkülöníteni. 

6. Szükség van forgalom folyamat virtuális készülékek használatával szabályozhatja?

    nem. Virtuális készülékek további beállítási lehetőségekre forgalom folyamat, beleértve a részletes adatok sík naplózás megadására használható. 

7. Felhasználók szükség különböző csoportjaihoz különböző Azure erőforrásokhoz engedélyek?

    igen. A hálózati csapatnak meg kell a virtuális hálózati beállítások, a teljes hozzáférés a fejlesztők csak nem lehet telepíteni a hajómegfigyelési már meglévő alhálózathoz. 

### <a name="design"></a>Tervező

A tervezés, adja meg az előfizetések, VNets, alhálózat és NSGs célszerű követnie. Fog ölelik NSGs itt, de érdemes többet szeretne tudni a [NSGs](virtual-networks-nsg.md) a tervezés befejezés előtt.

**Előfizetések és VNets száma**

Az alábbi követelmények előfizetések és VNets kapcsolatos:

- Minden egyes részleg erőforrás-felhasználás más részlegek nem érinti.
- Meg kell a lehető legkevesebb VNets és alhálózat.
- Minden egyes részleg egyetlen próba/fejlesztési alkalmazásokhoz használt VNet kell rendelkeznie.
- Egyes alkalmazások 2 különböző Azure adatközpontokban per földrész (Észak-Amerika és Európa) helyezkedik el.

Ezekről a követelményekről, meg kell előfizetés részlegek számára. Úgy, hogy egy üzleti egységből erőforrások felhasználása fog nem beleszámítanak korlátozások az egyéb részlegek. És óta szeretne VNets számának csökkentése érdekében, fontolja meg az **egységnyi üzleti alkalmazások csoportonként két VNets egy előfizetés** mintát alább látható módon.

![Egyetlen előfizetés](./media/virtual-network-vnet-plan-design-arm/figure9.png)

Meg kell a címterületet megadása minden VNet. Mivel van szüksége a helyszíni adatok közötti kapcsolatot középre igazítása az Azure régió az Azure VNets használt címterület nem ütközhetnek a helyszíni hálózaton, és az egyes VNet által használt címterület nem ütközhetnek más meglévő VNets. A cím szóközöket, az alábbi táblázatban segítségével ezeknek a követelményeknek.  

|**Előfizetés**|**VNet**|**Azure terület**|**Címterület használatára**|
|---|---|---|---|
|BU1|ProdBU1US1|Nyugati Amerikai Egyesült Államok|172.16.0.0/16|
|BU1|ProdBU1US2|Kelet-Amerikai Egyesült Államok|172.17.0.0/16|
|BU1|ProdBU1EU1|Észak-Európa|172.18.0.0/16|
|BU1|ProdBU1EU2|Nyugati Európa|172.19.0.0/16|
|BU1|TestDevBU1|Nyugati Amerikai Egyesült Államok|172.20.0.0/16|
|BU2|TestDevBU2|Nyugati Amerikai Egyesült Államok|172.21.0.0/16|
|BU2|ProdBU2US1|Nyugati Amerikai Egyesült Államok|172.22.0.0/16|
|BU2|ProdBU2US2|Kelet-Amerikai Egyesült Államok|172.23.0.0/16|
|BU2|ProdBU2EU1|Észak-Európa|172.24.0.0/16|
|BU2|ProdBU2EU2|Nyugati Európa|172.25.0.0/16|

**Alhálózat és NSGs száma**

Az alábbi követelmények alhálózat és NSGs kapcsolatos:

- Meg kell a lehető legkevesebb VNets és alhálózat.
- Egyes alkalmazások teljesen elszigetelt egymástól.
- Egyes alkalmazások elérhetők, vevők az interneten HTTP használatával.
- Egyes alkalmazások érhetők el egy titkosított alagutas használatával a helyszíni adatközpontokban csatlakozó felhasználók által.
- Kapcsolat a helyszíni adatközpontokban meglévő virtuális Magánhálózati eszközök kell használni.
- Az adatbázisok minden olyan helyen más Azure helyekre kell bizonyos egyszer.

E követelmények alapján, sikerült használja egy alhálózat minden alkalmazási réteg, és NSGs segítségével alkalmazás / szűrő forgalmat. Úgy, hogy csak akkor 3 alhálózat az egyes VNet (előtér, alkalmazási réteg és adatok réteg), és egy NSG per alhálózat egy alkalmazást. Ebben az esetben vegye figyelembe az **alkalmazási réteg, egy alkalmazás NSGs per egy alhálózat** tervezés minta használatával. Az alábbi ábrán a tervezés mintázatot, amely a **ProdBU1US1** VNet használatát.

![Egy réteg, egy alkalmazás száma rétegen egy NSG egy alhálózat](./media/virtual-network-vnet-plan-design-arm/figure11.png)

Azonban is létrehozásához szükséges egy további alhálózat a virtuális Magánhálózati kapcsolatot a VNets és a helyszíni adatközpontokban között. És meg kell adnia a címterületet minden alhálózathoz. Az alábbi ábra minta megoldást **ProdBU1US1** VNet számára. Ebben az esetben az egyes VNet volna replikáció. Minden szín jelöl egy másik alkalmazást.

![Minta VNet](./media/virtual-network-vnet-plan-design-arm/figure10.png)

**Hozzáférés-vezérlés**

Az alábbi követelmények kapcsolódnak a hozzáférés-vezérlés:

- A cég hálózati csoport a VNet konfiguráció teljes hozzáféréssel kell rendelkeznie.
- Láthatja, hogy az egyes részleg fejlesztők csak VMs telepítése a meglévő alhálózathoz.

Ezek a követelmények alapján, hozzáadhatja felhasználók hálózati csapattag a beépített **Hálózati munkatársi** szerepkörök az egyes előfizetések; és hozzon létre egy egyéni szerepkört egyes előfizetések VMs hozzáadása meglévő alhálózat jogok megadása őket az alkalmazás fejlesztők számára.

## <a name="next-steps"></a>Következő lépések

- [A virtuális hálózati Deploy](virtual-networks-create-vnet-arm-template-click.md) példa alapján.
- Dátumtáblázatok ismertetése [terheléselosztó](../load-balancer/load-balancer-overview.md) IaaS VMs és [kezelése a továbbítás több Azure régiók fölé](../traffic-manager/traffic-manager-overview.md).
- További tudnivalók [NSGs, és hogyan tervezése és kialakítása](virtual-networks-nsg.md) egy NSG megoldás.
- További tudnivalók a [határokon helyszíni és VNet csatlakozási beállításokat](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site).
