<properties 
    pageTitle="Az alkalmazás integrálása az Azure virtuális hálózaton" 
    description="Megtudhatja, hogy miként csatlakozhat egy új vagy meglévő Azure virtuális hálózati Azure alkalmazás szolgáltatás-alkalmazás" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Az alkalmazás integrálása az Azure virtuális hálózaton #

A dokumentum ismerteti az Azure alkalmazás virtuális hálózati integrációs szolgáltatást, és bemutatja, hogyan állíthatja be az alkalmazások [Azure App](http://go.microsoft.com/fwlink/?LinkId=529714)szolgáltatásban.  Ha nem ismeri Azure virtuális hálózatokkal (VNETs), az egy képesség, amely lehetővé teszi, ha el szeretne helyezni az Azure erőforrások számos való hozzáférés szabályozása a az internet routeable hálózat.  Ezek a hálózatok csatlakozhatnak az alkalmazás helyi hálózatokhoz a virtuális Magánhálózati technológiák számos.  További információ a Azure virtuális hálózatok kezdje az adatok Itt megtudhatja: [Azure virtuális hálózati – áttekintés][VNETOverview].  

Az Azure alkalmazás szolgáltatás két űrlapok tartalmaz.  

1. A több elem bérlői rendszerek, amelyek támogatják a tervek árak teljes köre
1. Az alkalmazás szolgáltatási környezetben (SKB) prémium szolgáltatás üzembe helyezése a VNET be, amelyek.  

A dokumentum kerül át VNET integráció és az alkalmazás szolgáltatási környezetben.  Ha többet szeretne tudni a ASE szolgáltatást, majd indítsa el az alábbi információkkal: [alkalmazás szolgáltatási környezetben – bevezetés][ASEintro].

VNET integráció a webes alkalmazás hozzáférést biztosít a virtuális hálózati erőforrások, de nem hozzáférést magánjellegű a web App alkalmazásban a virtuális hálózatról.  Saját hely elérési vásárolható meg, csak egy ASE egy belső betöltés terheléselosztó (ILB) a beállított.  A cikk az alábbi kezdődik egy ILB ASE használatával kapcsolatos részletekért: [létrehozása és használata egy ILB ASE][ILBASE]. 

A leggyakoribb helyzet, ahol integrációs VNET használna van segítségével, így a web app alkalmazásból az access adatbázis vagy egy Azure virtuális hálózata virtuális gépen futó webszolgáltatásokhoz.  VNET integrációval nem kell elérhetővé tenni egy nyilvános végpontot, az alkalmazások a virtuális, de a magánjellegű az internet átirányítható címeket használja helyette.  

A VNET integrációs funkciót:

- a normál vagy Premium csomagra árak igényel 
- Classic(V1) vagy az erőforrás Manager(V2) VNET működnek 
- TCP- és UDP támogatja
- az webes, a mobil és az API-alkalmazások működése
- lehetővé teszi, hogy az alkalmazás egyszerre csak 1 VNET csatlakoztatása
- lehetővé teszi, hogy egy App szolgáltatás tervben integrálódik a maximum 5 VNETs 
- lehetővé teszi, hogy az azonos VNET-alkalmazás szolgáltatás tervben több alkalmazások való használatra
- támogatja az VNET átjáró egy támaszkodik miatt egy 99,9 % SLA

Néhány dolgot, amely VNET integráció nem támogatja, többek között:

- a meghajtó csatlakoztatása
- Active Directory-integráció 
- NetBios
- személyes webhely access

### <a name="getting-started"></a>Első lépések ###
Íme néhány dolog, amit érdemes szem előtt a web App alkalmazásban a virtuális hálózatokhoz történő csatlakozás előtt:

- VNET integrációja csak akkor működik, a **normál** vagy **Premium** csomagra árak-alkalmazások használatát.  Ha szolgáltatás engedélyezése és az alkalmazás szolgáltatáscsomagja nem támogatott árak-csomagra majd méretezze az alkalmazás elvesznek a kapcsolatok a VNETs tulajdonosa milyen alkalmazásokat használ.  
- Ha a cél virtuális hálózat már létezik, pont-webhely VPN dinamikus útválasztási átjáró engedélyezve van egy adott appal csatlakoztatva előtt kell rendelkeznie.  Pont-webhely virtuális magánhálózat (VPN) nem engedélyezése, ha be van állítva az átjáró statikus útválasztási.
- A VNET ugyanabban az előfizetésben, mint az alkalmazás szolgáltatás Plan(ASP) kell lennie.  
- Az alkalmazásokat, hogy egy VNET integrálása a DNS-Rekordokat, hogy VNET megadott fogja használni.
- Alapértelmezés szerint a integrációs alkalmazásokat fogja csak irányítja a forgalmat a VNET a VNET definiált útvonalak alapján történő.  


## <a name="enabling-vnet-integration"></a>VNET integráció engedélyezése ##

A dokumentum összpontosít elsődlegesen az Azure portál használatával VNET integráció.  VNET alkalmazással való integráció engedélyezése a PowerShell használata az alkalmazással, kövesse az alábbi: [Csatlakozás az alkalmazását a PowerShell használatával virtuális hálózat][IntPowershell].

Ha a vezérlőt, amellyel az alkalmazás egy új vagy meglévő virtuális hálózathoz csatlakozik.  A integráció, majd létrehozásán csak a VNET részeként hoz létre egy új hálózaton, ha dinamikus útválasztási átjárót, az előre beállított lesz, és mutasson a webhely VPN engedélyezve lesz.  

>[AZURE.NOTE] Egy új virtuális hálózati integrációval konfigurálása néhány percet is igénybe vehet.  

Engedélyezheti VNET integrációs nyissa meg az alkalmazás beállításai, és válassza a hálózat.  A felhasználói felület fel a megnyíló három hálózati választási lehetőségeket kínál.  Ez az útmutató van csak érkeznek VNET integrációs bár hibrid kapcsolatok és alkalmazás környezetek ismertetik a dokumentumon belül.  

Ha az alkalmazás van nem a megfelelő árak terv a felhasználói felület helpfully engedélyezése lehetőség magasabb árak csomagra csomagja méretezheti.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Egy már meglévő VNET VNET integráció engedélyezése###
VNET integráció a felhasználói felület lehetővé teszi, hogy a VNETs közül választhat.  A klasszikus VNETs jelzi, hogy azok ilyen VNET neve melletti zárójelet "klasszikus" szót.  A listában, hogy az első jelennek meg az erőforrás-kezelő VNETs vannak rendezve.  Az alább látható módon képen megtekintheti, hogy csak egy VNET is kijelölhető.  Több oka, hogy egy VNET fog kell parancs szürkén jelenik meg, beleértve a:

- a VNET szerepel-e egy másik előfizetést tartalmazó a fiók eléréséhez
- a VNET nincs pont engedélyezett webhelyre
- a VNET nincs dinamikus útválasztási átjáró


![][2]

Ahhoz, hogy integrációs egyszerűen kattintson a kívánt integrálása a VNET.  Miután kiválasztotta a VNET, az alkalmazás automatikusan újraindul a módosítások érvénybelépéséhez.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Mutasson a webhelyen található egy klasszikus VNET engedélyezése #####
Ha a VNET nem rendelkezik az átjáró nem rendelkezik a pont-webhelyre, majd akkor beállítása, hogy az első.  Ehhez a klasszikus VNET, nyissa meg az [Azure-portálon] [ AzurePortal] , és jelenítse meg a virtuális Networks(classic) listáját.  A hálózaton itt kattintson a kívánt integrálása, majd kattintson a nagy párbeszédpaneljének Essentials nevű virtuális Magánhálózati kapcsolatot.  További lehetőségek a VPN Használatát a webhely és még egy átjáró létrehozása pont hozhat létre.  Átjáró létrehozása élményt webhelyre hajtsa végre a pont után körülbelül fél óráig készen áll a megfelelő lesz.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Mutasson a webhelyen található egy erőforrás-kezelő VNET engedélyezése #####

Állítson be egy erőforrás-kezelő VNET az átjárók és PowerShell-lel dokumentált itt kell webhely mutasson a [PowerShell használatá virtuális hálózat pont a webhely-kapcsolat beállítása][V2VNETP2S].  A felhasználói felület végre ezt a funkciót egyelőre nem érhető el. 

### <a name="creating-a-pre-configured-vnet"></a>Egy előre konfigurált VNET létrehozása ###
Ha szeretne hozzon létre egy új VNET be van állítva az átjárók és a webhely pont-, a felhasználói felület hálózati alkalmazás szolgáltatás a képesség azonban csak az erőforrás-kezelő VNET megtennie tartalmaz.  Ha szeretne egy klasszikus VNET létrehozni egy átjáró és webhely-pont majd meg kell a hálózat felhasználói felületen kézi tegye a. 

Szeretne létrehozni egy erőforrás-kezelő VNET keresztül VNET integráció a felhasználói felület, egyszerűen jelölje ki az **Új virtuális hálózat létrehozása** , és adja meg a:

- Virtuális hálózat neve
- Virtuális hálózati címterület
- Alhálózat neve
- A címterület alhálózat
- Átjáró címterület
- A címterület pont-webhely

Ha azt szeretné, hogy ez más a hálózati csatlakozás VNET majd kerülje hálózatokhoz átfedésben IP-címterület kiválasztása.  

>[AZURE.NOTE] Erőforrás-kezelő VNET létrehozása, az átjáró körülbelül 30 percig tart, és jelenleg nem integrálja a VNET az alkalmazást.  Miután a VNET az átjáró kell térjen vissza az alkalmazás VNET integrációs felhasználói felület, és jelölje be az új VNET jön létre.

![][3]

Azure VNETs általában jönnek létre magánhálózaton címek belül.  A VNET integrációt alapértelmezés szerint a szolgáltatás be a VNET e IP-címtartományok bármely adatforgalmat irányítja.  A magánjellegű IP-címtartományok a következők:

- – Ez ugyanaz, mint 10.0.0.0 - 10.0.0.0/8 10.255.255.255
- – Ez ugyanaz, mint 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
- – Ez ugyanaz, mint 192.168.0.0 - 192.168.0.0/16 192.168.255.255
 
A VNET címterületet kell CIDR formátumban kell megadni.  Ismeretekkel CIDR alakban, érdemes egy módszert a címterületet IP-címet és a hálózati maszk megadó egész szám megadásával. Fontolja meg egy rövid összefoglaló 10.1.0.0/24 lenne 256 címet és a 10.1.0.0/25 128 címek lenne.  Az egy /32 IPv4-cím csak 1 cím lenne.  

Ha az Itt adhatja meg a DNS-kiszolgáló adatai majd, amely állíthat a VNET.  VNET létrehozását követően módosíthatja ezeket az adatokat a VNET felhasználói élményt.

Amikor létrehoz egy klasszikus VNET VNET integráció a felhasználói felület használatával, a erőforrás csoporton belül az App egy VNET hoz létre. 

## <a name="how-the-system-works"></a>A rendszer működése ##
Ez a funkció a magában foglalja a pont-webhely VPN technológia az alkalmazás csatlakozni a VNET fölött hoz létre.   Alkalmazások Azure App szolgáltatásban a több elem bérlői rendszer architektúrája, amelyek kizárja a kiépítési közvetlenül egy VNET az alkalmazás, a virtuális gépeken futó elvégzettként van.  Pont-webhely technológia elkészítéséhez azt korlátozza a hálózati hozzáférés csak a virtuális géphez üzemeltető az alkalmazást.  A hálózat elérését tovább korlátozza az adott alkalmazás állomásokon, hogy az alkalmazás csak férnek hozzá a hálózatok konfigurálható elérheti őket.  

![][4]
 
Ha úgy állította be a DNS-kiszolgáló hálózati virtuális IP-címek szüksége lesz még nem.  IP-címek használatakor ne feledje, hogy ez a szolgáltatás fő előnyeit, hogy azt lehetővé teszi, hogy a saját címét a saját hálózaton belül.  Ha az alkalmazás használata nyilvános IP-címek a VMs egyik, majd a VNET integrációja funkció nem használ, és a kommunikáció vannak az interneten felfelé.


##<a name="managing-the-vnet-integrations"></a>A VNET integrációs eszközök kezelése##

Az azt jelenti, hogy csatlakoztatása vagy leválasztása arról szeretne egy VNET alkalmazás szinten van.  Műveletek is befolyásolja a VNET integrációt használt több alkalmazások ASP szinten vannak.  A felhasználói felületen alkalmazás szintre látható elérheti a VNET a részleteket.  Ugyanazokat az adatokat a legtöbb is megjelennek a ASP szintjén.  

![][5]

A hálózat szolgáltatás állapota lapon megtekintheti, ha az alkalmazás a VNET csatlakozik.  Ha a VNET átjáró nem működik az bármilyen okból majd ez látható nem csatlakozik az.  

Ekkor megjelenik a szintű VNET integráció felhasználói felület megegyezik a részletes információkat, letölthető az ASP alkalmazásban elérhető adatok.  Az alábbiakban ezeket az elemeket:

- VNET nevét – ez a hivatkozás megnyitása a a hálózat felhasználói felület
- Hely - hol található a VNET tükrözi.  Egy másik helyre VNET integrálása lehetőség.
- Állapot - tanúsítványok, amellyel biztonságossá a virtuális Magánhálózati kapcsolatot az alkalmazás és a VNET között vannak.  A vizsgálat érdekében azok szinkronizálja ez tükrözi.
- Átjáró állapota – az átjárók kell lefelé valamilyen okból majd az alkalmazás nem tud hozzáférni a VNET erőforrásokat.  
- Címterület VNET – Ez az IP-címterület esetében a VNET.  
- Mutasson a webhely-címterület használatára – Ez az a webhely IP-címterület a VNET pontjára.  Az alkalmazás megjelenik az IP-címei az a címterület érkező kommunikáció.  
- Webhely címterület - hely a helyi erőforrásait vagy más VNETs csatlakozhat a VNET webhelyre történő VPN adatai is használhatja.  Kell majd célokkal definiált az IP-tartományokat, hogy virtuális Magánhálózati kapcsolat jelennek meg az alábbi konfigurált.
- DNS-kiszolgálók – ha van beállítva a VNET, majd az itt felsorolt DNS-kiszolgálók.
- IP-címei a rendszer nem VNET - ott, hogy a VNET van-e a megadott útválasztás IP-címek listája.  Azon címek Itt jelennek meg.  

A csak művelet készíthet a VNET integráció az alkalmazás nézetben, az alkalmazás leválasztja az aktuálisan csatlakoztatva van a VNET.  Végezze el a egyszerűen kattintson kapcsolat bontása tetején.  Ez a művelet nem változtatja meg a VNET.  A VNET és annak beállításával és többek között az átjárók érintetlen marad.  Ha ezután törölni szeretné a VNET meg kell törölnie kell az erőforrásokat, beleértve az átjárók.  

Az alkalmazás szolgáltatás megtervezése nézetben számos további műveletek.  Azt is érhető el másképp mint a alkalmazásból.  Eléréséhez ASP hálózati a felhasználói felület nyissa meg a ASP felhasználói felület, és görgessen lefelé.  Van egy hálózati szolgáltatás állapota nevű felhasználói felület elemet.  Jó képet ad az egyes kisebb részletek körül a VNET integráció.  Ez a felhasználói felület kattintva megnyílik a hálózati szolgáltatás állapota a felhasználói felület.  A "Ide kattintva kezelheti a" elemre kattintva, felhasználói Felületét, amely felsorolja a VNET integrációs e ASP megnyitják.

![][6]

Az ASP helye jól használható, ha megjeleníti a szolgáltatással való integrálásáról VNETs helye.  Ha egy másik helyre a VNET, valószínűleg sokkal késés problémák című cikk.  

A VNETs integrálva hány VNETs egy emlékeztetőt, az alkalmazások az e ASP és hány is integrálódik.  

Az egyes VNET hozzáadott részletek megtekintéséhez parancsára az érdeklik VNET.  A részletek mellett, amely volt korábban feljegyzett is látni fogja a ASP használják, hogy VNET alkalmazások listájának.  

Műveletek részletez két elsődleges tevékenységek vannak.  Az első hozzáadása, amelyek a forgalom elhagyja az alkalmazás a VNET be meghajtó útvonalak lehetősége.  A második művelet pedig az azt jelenti, hogy a tanúsítványok és a hálózati adatok szinkronizálása.

![][7]

**Továbbítás** Korábbi amint az a VNET definiált útvonalak, mire használható az alkalmazás a VNET be irányítja a forgalmat.  Vannak bizonyos használ, hogy hol ügyfelek szeretné küldeni további kimenő forgalom az alkalmazás a VNET, és azok ezt a lehetőséget, megadva.  Mi történik a forgalmat, miután, hogy ez hogyan állítja be az ügyfél az azok VNET felfelé.  

**Tanúsítványok** A tanúsítvány állapotát által végzett az alkalmazás szolgáltatás ellenőrzéséhez, hogy a tanúsítványok, azt a virtuális Magánhálózati kapcsolatot használ, akkor továbbra is célszerű ellenőrzése tükrözi.  VNET integráció engedélyezése, majd ha ez az első integráció az adott VNET származó e ASP, az összes alkalmazás van egy szükséges exchange-tanúsítványok biztonsága érdekében a kapcsolatot.  A tanúsítványok együtt azt kaphat a DNS-konfigurációja, útvonalak és egyéb hasonló elemek, amelyek bemutatják a hálózathoz.
Ha ezeket a tanúsítványok vagy hálózati adatok megváltoztak majd meg kell kattintson a "Szinkronizálási hálózat".  **Megjegyzés**: Ha, okozhat egy rövid üzemszünetek connectivity az alkalmazás és a VNET között, majd "a szinkronizálási hálózat" gombra.  Az alkalmazás nem kell indítani közben megszakadt a kapcsolat okozhat a webhelyen a függvény nem megfelelően.  

##<a name="accessing-on-premise-resources"></a>Erőforrások helyi elérése##

A VNET integráció szolgáltatás előnyei egyik, ha a VNET webhelyre történő virtuális magánhálózattal a helyi hálózathoz csatlakoztatva van, majd az alkalmazások elérhetik a be van forrá az alkalmazás.  Ennek ellenére előfordulhat, hogy módosítania kell a helyi VPN-átjárót, a útvonalat a pont webhely IP-tartományt.  Amikor először beállítása a webhelyen a webhely-VPN használatával állítsa be a parancsfájlok beállítása kell utakat, beleértve a webhely VPN pont.  Ha felvesz a pont után webhely VPN a létrehozása a webhelyen a webhely-VPN, útvonalak frissítse manuálisan kell majd.  Olvashat, hogy egy átjáró változik, és nem itt leírt.  

>[AZURE.NOTE] Miközben a VNET integrációs szolgáltatás működik webhelyre történő virtuális Magánhálózattal kapcsolódik helyi erőforrások is elérheti jelenleg nem működik egy készült ExpressRoute a virtuális magánhálózati tegye ugyanezt a.  Az IGAZ, ha a klasszikus vagy az erőforrás-kezelő VNET integrálása.  Ha módosítani szeretné egy készült ExpressRoute VPN keresztül férhet majd is használhatja, amely a VNET futtathat egy ASE. 

##<a name="pricing-details"></a>Árakról##
Létezik néhány árak, tisztában kell lennie a VNET integrációja funkció használata esetén részleteiről.  Ez a szolgáltatás használatához a 3 kapcsolódó díjak különböztethető meg:

- ASP árak réteg vonatkozó követelmények
- Adatok átvitele költségek
- Virtuális Magánhálózati átjáró költségeket.

Az Ez a szolgáltatás segítségével az alkalmazásokat azok kell lennie a normál vagy prémium alkalmazás szolgáltatás megtervezése.  Azok a költségek itt tekintheti meg további részleteket: [Alkalmazás szolgáltatás árak][ASPricing]. 

A webhely VPN adatai úgy pont miatt vannak kezelni, bármikor van kimenő adatok díjat a VNET integráció kapcsolaton keresztül még akkor is, ha a VNET az azonos adatközpont.  Olvassa el a Mi az adott díjak vannak tekintse meg az alábbi: [Adatok átvitele árak részleteinek][DataPricing].  

Az utolsó elemet a VNET átjáró költségét.  Ha már nincs szükség az átjárók valami mást, például a webhely-webhely VPN adatai majd is fizet az átjárók támogatja a VNET integrációs funkciót.  Részletek tartalmazza az itt említett költségek: [VPN átjáró árak][VNETPricing].  

##<a name="troubleshooting"></a>Hibaelhárítás##

Miközben a lehetőség egyszerűen állíthatja be, amely nem jelenti azt, hogy a felhasználói felület lesz probléma ingyenes.  Érdemes tapasztal a kívánt végpont elérésével kapcsolatos problémákról bizonyos segédprogramok is használhatja az alkalmazás központból kapcsolat tesztelése.  Vannak olyan két konzol változat is használhatja.  Egyik a Kudu konzolról, a másik a konzolt, amely az Azure-portálon érheti el.  A Kudu konzol eléréséhez kattintson az alkalmazás-nyissa meg az eszközök > Kudu.  Ugyanaz, mint látogasson el a [sitename]. scm.azurewebsites.net.  Miután egyszerűen megnyílt, lépjen a hibakeresési konzol lapra.  Ismerkedés az Azure portál szolgáltatott konzol, majd az alkalmazás eszközök Ugrás Console ->.  


####<a name="tools"></a>Eszközök####

Az eszközök ping, az nslookup és a tracert biztonsági korlátozások miatt konzolon fognak működni.  Töltse ki a érvénytelenítése ott lett hozzáadva két külön eszközt.  Tesztelje a DNS-funkciók nameresolver.exe nevű eszköz van kibővült.  Szintaxisa a következő:

    nameresolver.exe hostname [optional: DNS Server]

Nameresolver segítségével jelölje be a állomásnevekké, az alkalmazás függ.  Ezzel a módszerrel tesztelheti ha van hatással a DNS-hibásan konfigurált vagy esetleg nem fér hozzá a DNS-kiszolgálójába.

A következő eszköz állomás és port együttes TCP kapcsolat tesztelése teszi lehetővé.  Ez az eszköz nevezik tcpping.exe és szintaxisa a következő:

    tcpping.exe hostname [optional: port]

Ez az eszköz a program tájékoztatja, ha egy adott állomás és port érhető el, de ne hajtsa végre a ugyanahhoz a tevékenységhez, igénybe vehet az ICMP alapú ping segédprogramot.  A ICMP ping eszköz program tájékoztatja, ha a szolgáltató be-e.  A tcpping tudomást szerzett egy adott portjához állomás érhető el.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Erőforrások hibakeresési VNET való hozzáférést is.####

Számos dolgot, amely megakadályozhatja, hogy az alkalmazás egy adott állomás és port alkalmazáson keresztül.  Az esetek többségében érdemes három lehetőség közül:

- **A tűzfal módon van**  Ha tűzfalat módon, majd a TCP-időtúllépése fog gombra kattint.  Ebben az esetben ez 21 másodpercre.  A tcpping eszközzel tesztelje a kapcsolatot.  TCP időtúllépései miatt tűzfalak túl sok dolgot lehet, de kezdeni.  
- **Nem érhető el a DNS**  A DNS-időtúllépése a DNS-kiszolgáló / 3 másodperces.  Ha van, 2, hogy 6 másodperc DNS-kiszolgálók.  Nameresolver használatával megtekintheti, hogy működik-e a DNS.  Ne feledje, hogy nem használhatja az nslookup, mely nem használja a VNET úgy van konfigurálva, a DNS-Rekordokat.
- **Érvénytelen P2S IP-tartomány** A webhely IP-tartomány pontjára kell elhelyezni a RFC 1918 magánjellegű IP-címtartományai (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) Ha a tartomány IP-címei használ, amely kívül, majd dolog, amit nem fog működni.  

Ha ezeket az elemeket a probléma nem fogad, a következőhöz egyszerű dolgokat az első:  

- Az átjáró jeleníti meg a portálon tekintsék?
- Megjelenítéséhez végezze el a tanúsítványok, hogy szinkronizálja?
- Bárki, változtatta a hálózati konfigurálásról anélkül, hogy a szóban forgó ASP a "szinkronizálási hálózaton" módon? 

Ha az átjáró nem működik majd jelenítse meg készítsen biztonsági másolatot.  Ha nincs szinkronban a tanúsítványok nyissa meg a VNET integrációs ASP nézetét, és találati a "Szinkronizálási hálózati".  Ha azt gyanítja, hogy az a VNET konfigurációs elvégzett módosítás volt, és azt nem lett szinkronizálási amilyent a ASP majd nyissa meg a VNET integráció a ASP nézete, majd kattintson a "Szinkronizálási hálózati" csupán emlékeztetőt, ennek hatására a VNET kapcsolat és az alkalmazások egy rövid üzemszünetek.  

Ha az összes arra, hogy nem kell aggódnia majd meg kell alaposabban egy kicsit:

- Van bármilyen egyéb alkalmazások használatával VNET integrációs erőforrásainak ugyanazt a VNET eléréséhez? 
- Lehet nyissa meg az app konzol és más erőforrások a VNET eléréséhez tcpping használni?  

Ha akár a fentiek mindegyike teljesül, akkor kattintson a VNET integráció nem kell aggódnia, és a probléma nem máshová.  Ez a hol kell több bonyolulttá, mert nincs egyszerű módon tekintheti meg, miért nem tudják elérni állomás: port kap.  A okai a következők:

- a webhely IP-tartomány mutasson az alkalmazás port elérni az állomáson használ tűzfalat.  Nyilvános hozzáférés gyakran metsző alhálózat szükséges.
- a target host nem működik
- az alkalmazás nem működik
- is voltak, a megfelelő IP-címének vagy hostname (állomásnév)
- az alkalmazás figyel, mint a várttól eltérő port.  Érdemes a tároló alakzatot, és használja a "netstat - aon" cmd parancssorából.  Ez megtudhatja, milyen azonosító figyel mi port folyamat.  
- a hálózati biztonsági csoportokat úgy van konfigurálva, a oly módon, hogy azok megakadályozása az access a alkalmazás host és port a mutasson a webhely IP-tartomány

Ne feledje, hogy nem tudja, hogy mely IP az a pont webhely IP tartománnyá, az alkalmazás által használt, így Önnek kell a teljes tartományból elérésének.  

További hibakeresési lépései a következők:

- Jelentkezzen be a VNET egy másik virtuális, és próbálja meg elérni az erőforrás állomás: port onnan.  Vannak bizonyos TCP ping segédprogramok, hogy erre a célra használható, vagy még akkor is használható telnet, ha szükséges.  Az alábbi célja csak annak megállapításához, hogy csatlakozási van a többi virtuális a. 
- az alkalmazás a konzolról, hogy host és port jelenítse meg a másik virtuális és tesztelje az access-alkalmazás  
####<a name="on-premise-resources"></a>A helyi erőforrások####
Ha a helyszíni környezetbe erőforrásait nem érheti el, akkor nézze meg a legfontosabb dolog Ha érheti el egy erőforrás az a VNET.  Ha nem működik, amely a következő lépésekkel meglehetősen egyszerű feladat.  Az egy virtuális a VNET a kell próbálja meg az alkalmazás helyi alkalmazás eléréséhez.  Telnet vagy a TCP-ping eszköz használható.  Ha a virtuális nem tudja elérni a a helyi erőforrás, majd a először győződjön meg arról, hogy a webhely-webhely virtuális Magánhálózati kapcsolat működik.  Ha működik jelölje be az azonos dolgot, és a helyi átjáró konfigurációs és állapot korábban feljegyzett.  

Ha a VNET fájlkiszolgálón most virtuális tudják elérni az alkalmazás helyi rendszer, de az alkalmazás nem tudja, majd a hiba oka valószínűleg egy, a következő:
- a útvonalak nincs beállítva a webhelyen IP-tartományokat a helyi kezdőlapja az a pont
- a hálózati biztonsági csoportokat a webhely IP-tartomány pont blokkolja a hozzáférést
- a helyi tűzfal amelyek gátolják-alapú forgalmat a pont webhely IP-tartomány
- a felhasználó által definiált Route(UDR) a VNET, amely megakadályozza, hogy a pont-webhelyet forgalom alkalmazáson keresztül a helyi hálózaton van

## <a name="hybrid-connections-and-app-service-environments"></a>Hibrid kapcsolatok és a alkalmazás szolgáltatási környezetben##
Sorolhatók VNET hosted erőforrások hozzáférés engedélyezése a 3 funkciói.  Ezek a következők:

- VNET-integráció
- Hibrid kapcsolatok
- Alkalmazás szolgáltatási környezetben

Hibrid kapcsolatok megköveteli továbbítási ügynököt egy a hibrid kapcsolat Manager(HCM) nevű a hálózaton.  A táblabeli HCM kell tud csatlakozni az Azure és az alkalmazás.  Ez a megoldás különösen kiválóan, a hálózat távoli, például a helyi hálózaton, vagy akár egy másik felhő hálózati is, mivel az internetes elérhető zárólap nincs szükség.  A táblabeli HCM csak alábbiakon futtatható Windows, és a maximum 5 példányok futtatása magas rendelkezésre állás is.  Hibrid kapcsolatok csak akkor támogatja a TCP bár és mindegyik HC végpontra kell teljesülnie egy adott host: port kombinációt.  

Az alkalmazás-szolgáltatási környezetben funkció lehetővé teszi, hogy az alkalmazás Azure szolgáltatás egy példányát a VNET futtatása.  Ezzel a lehetőséggel a VNET bármilyen további lépések nélkül alkalmazások access erőforrásait.  Néhány további előnyeiről az alkalmazás-szolgáltatási környezetben használható 8 dedikált core dolgozók 14 GB RAM.  Egy másik előnye, hogy a rendszer az igényeknek méretezheti.  Nem kedvelik-e a több elem bérlői környezetekben, ahol a ASP mérete korlátozott a egy ASE szabályozhatja, hány erőforrások meg szeretné adni, hogy a rendszer.  A dokumentum a hálózati fókusz részletez azonban a szolgáltatás, amit meg igénybe vehet egy alkatrészektől, amelyek nem VNET integrációval egyik, hogy akkor is dolgozhat egy készült ExpressRoute VPN.  

Amikor eset átfedés némelyikén, e beállítások közül egyik sem lecserélheti a többi.  Milyen funkció használatához kötődik az igényeinek, és hogyan fogja használni kívánt azt arra.  Példa:

- Ha a fejlesztők és egyszerűen szeretné futtassa a webhely Azure-ban, és azt a hozzáférést az adatbázishoz a munkaállomáson asztali csoportban a legegyszerűbb dolog használata hibrid kapcsolatok.  
- Ha Ön a nagyobb szervezetek, amely sok helyezni szeretné a nyilvános webhely tulajdonságok cloud, és kezelheti őket a saját hálózaton, majd válassza az alkalmazás-szolgáltatási környezetben szeretne.  
- Ha rendelkezik az alkalmazás szolgáltatás számos alkalmazások is, és egyszerűen hozzá szeretne férni a VNET az erőforrásokat, majd VNET integrációs módja a go.  

Használati eset vannak túl néhány egyszerű kapcsolatos szempontjait.  Ha a VNET akkor használja az VNET integráció a helyi hálózathoz csatlakoztatva van, vagy az alkalmazás-szolgáltatási környezetben ugyanígy helyi erőforrások felhasználni.  Azonban ha a VNET nem csatlakozik a a helyi hálózat beállítása a webhelyen a webhely-VPN sokkal több általános, majd a VNET varianciájában az táblabeli HCM telepítése.  

A funkcionális eltérések kívül is árak különbségek.  Az alkalmazás szolgáltatási környezetben funkció a támogatási szolgáltatás egyesíti de kínál a legtöbb hálózati konfigurációs lehetőségek kívül más nagyszerű szolgáltatásokat.  VNET integráció kínál normál vagy prémium ASP, és biztonságos módon használata más források a több elem bérlőjéről alkalmazás szolgáltatás a VNET tökéletes.  Hibrid kapcsolatok jelenleg árak, indítsa el a szabad, és kattintson a további fokozatosan drága első szintű rendelkező fiók szükséges mennyisége alapján BizTalk függ.  Amikor sok hálózatokon, bár dolgozik, nem hibrid kapcsolatok, amelyek lehetővé teszik a 100-nál jól külön hálózatok forrásokat például nincs funkció.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
