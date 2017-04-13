<properties 
   pageTitle="Hibrid kapcsolat 2 szintű alkalmazás |} Microsoft Azure"
   description="Megtudhatja, hogy miként virtuális készülékek és a több szálon alkalmazás környezet létrehozása az Azure UDR terjesztése"
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
   ms.date="05/05/2016"
   ms.author="jdial" />

# <a name="virtual-appliance-scenario"></a>Virtuális készülék eset

A leggyakoribb helyzet nagyobb Azure ügyfél között szükség egy kétszintű alkalmazás érhető el az interneten, miközben egy helyszíni adatközponthoz a hátsó réteg hozzáférést biztosít. A dokumentum végigvezeti felhasználó által definiált útvonalak (UDR), a virtuális Magánhálózati az átjárók és a hálózati virtuális készülékek használatának üzembe egy kétszintű környezetben, amely megfelel az alábbi példa:

- Webalkalmazás elérhető nyilvános internetkapcsolat a kell lennie.
- Az alkalmazás webkiszolgáló elérheti a kódmentes alkalmazáskiszolgáló kell lennie.
- A webalkalmazás az internetről származó minden forgalom haladjon végig a tűzfal virtuális készülék. Ez a virtuális készülék használandó csak az internetes forgalmat.
- Nyissa meg az application server minden forgalom haladjon végig a tűzfal virtuális készülék. Ez a virtuális készülék hozzáférése a kódmentes befejezése és az access érkező a helyszíni hálózaton keresztül egy virtuális Magánhálózati átjárót használható.
- A rendszergazdák kell lennie a tűzfal virtuális készülékek kezelő a számítógépére a helyszíni, használatával a harmadik kizárólag management célra használt virtuális készülék tűzfal.

Ez a példa egy DMZ és a védett hálózati szabványos DMZ. Az ilyen esetben NSGs, a tűzfal virtuális készülékek vagy mindkettőt használatával is összeállítás Azure-ban. Az alábbi táblázatban látható a szakemberek és a negatívumokat között NSGs tűzfal virtuális készülékek részét.

||Szakemberek|Negatívumokat|
|---|---|---|
|NSG|Költség nélkül. <br/>Azure RBAC integrálva. <br/>ARM-sablonok szabályok hozhat létre.|Összetettsége eltérőek lehetnek nagyobb környezetben. |
|Tűzfal|Teljes hozzáférés sík adatot felett. <br/>A központi felügyeleti konzolon tűzfalon keresztül.|Tűzfal készülék költségét. <br/>Azure RBAC nem integrálva.|

A megoldást, az alábbi DMZ/védett hálózati példa végrehajtásához használja a tűzfal virtuális készülékek.

## <a name="considerations"></a>Megfontolandó szempontok

A környezet használatával a rendelkezésre álló szolgáltatások ma, a következőképpen Azure-ban fentiekben telepítheti.

- **Virtuális hálózati (VNet)**. Az Azure VNet egy helyszíni hálózaton hasonló módon működik, és be egy vagy több alhálózat forgalom elkülönítési és szétválasztása aggályokat megadására szegmentált is lehet.
- **Virtuális készülék**. Több partnerek adja meg a Microsoft Azure piactéren a fenti három tűzfalak használható virtuális készülékek. 
- A **felhasználó által definiált útvonalak (UDR)**. Útvonal táblák UDRs belül egy VNet csomagok szabályozását Azure hálózat segítségével is tartalmazhat. Az alábbi útvonal táblázatok alhálózathoz alkalmazhatók. Azure-ban legújabb szolgáltatásainak egyik útvonal táblázat alkalmazása a GatewaySubnet, az azt jelenti, hogy minden forgalom az Azure VNet be lesz a hibrid kapcsolatot a egy virtuális készülék továbbítása nyújtó lehetősége.
- **IP-továbbítási**. Alapértelmezés szerint az Azure hálózati motor virtuális hálózati kártyákkal (előfordulhat) csomagok továbbítása, csak akkor, ha a csomag cél IP-címe felel meg a hálózati kártya IP-címet. Ha egy UDR határozza meg, hogy a csomag egy adott virtuális készülék kell elküldeni, ezért az Azure hálózati motor legördülő volna csomagban. Annak érdekében, hogy a csomag érkeznek egy virtuális (ebben az esetben egy virtuális készülék), amely nem a tényleges cél a csomag, kell IP-továbbítási engedélyezése a virtuális készülék.
- **Hálózati biztonsági csoportok (NSGs)**. Az alábbi példában nem előhívható NSGs, de, ha a megoldás az alhálózathoz vagy a NIC alkalmazott NSGs segítségével további szűrheti és kijelentkezés azok alhálózat és NIC a forgalmat.


![Létesített IPv6-kapcsolat](./media/virtual-network-scenario-udr-gw-nva/figure01.png)

Ebben a példában az előfizetés, amely tartalmazza az alábbiakat:

- 2 erőforrás csoportokat, nem jelenik meg a diagramot. 
    - **ONPREMRG**. A helyszíni hálózaton hasonlóan szükséges összes erőforrás tartalmazza.
    - **AZURERG**. Az Azure virtuális hálózati környezet szükséges összes erőforrás tartalmazza. 
- Egy VNet nevű **onpremvnet** egy helyszíni adatközponthoz, az alább felsorolt szegmentált imitálása használt.
    - **onpremsn1**. Az alhálózathoz tartalmazó Ubuntu imitálása egy helyszíni kiszolgálón futó virtuális gép (virtuális).
    - **onpremsn2**. A virtuális Ubuntu imitálása-rendszergazda által használt helyszíni számítógépen futó tartalmazó alhálózat.
- Van egy tűzfal virtuális készülék **onpremvnet** **azurevnet**szeretne egy alagutas karbantarthatja a **OPFW** nevű.
- A névvel ellátott **azurevnet** VNet szegmentált, az alább felsorolt.
    - **azsn1**. A külső tűzfalat alhálózat kizárólag a külső tűzfalat használ. Az alhálózathoz keresztül összes internetes forgalmat fog érkezni. Az alhálózathoz csak tartalmazza a külső tűzfalon keresztül kapcsolódik egy hálózati.
    - **azsn2**. Előtér-alhálózat szolgáltatója egy virtuális fut, mint az internetről megnyitó webkiszolgálóra.
    - **azsn3**. Egy virtuális egy kódmentes application server, amely az előtér-webkiszolgálón elérhető futása szolgáltatója Kódmentes alhálózat.
    - **azsn4**. Adatkezelési alhálózat használt kizárólag az összes tűzfal virtuális készülékek management hozzáférést biztosít. Az alhálózathoz csak az egyes tűzfal virtuális készülék, használja a megoldás egy hálózati tartalmaz.
    - **GatewaySubnet**. Azure hibrid kapcsolat alhálózat szükséges készült ExpressRoute és a virtuális Magánhálózati átjáró Azure VNets és a más hálózatokon közötti kapcsolatot. 
- Vannak olyan 3 tűzfal virtuális készülékek a **azurevnet** hálózaton. 
    - **AZF1**. Nyilvános internetkapcsolat Azure-ban egy nyilvános IP-cím erőforrás használatával elérhetővé tett külső tűzfalon keresztül. Hogy biztosan sablon a piactérről, vagy közvetlenül a készülék szállítójával, hogy rendelkezések a 3-hálózati virtuális készülék szüksége.
    - **AZF2**. Segítségével megadható **azsn2** és **azsn3**közötti forgalom belső tűzfalon keresztül. Ez akkor is a 3-hálózati virtuális készülék.
    - **AZF3**. Adatkezelési tűzfal a helyszíni adatközponthoz rendszergazdák számára elérhető, és az összes tűzfal készülékek kezelésére szolgáló felügyeleti alhálózat csatlakoztatva van. A piactér 2-hálózati virtuális készülék sablonok keresése, vagy közvetlenül a készülék szállítótól kérhet.

## <a name="user-defined-routing-udr"></a>Felhasználó által definiált útválasztási (UDR)

Határozza meg, hogyan forgalom kezdeményezett annak, hogy alhálózat továbbítás UDR táblázat minden egyes alhálózat Azure-ban csatolható. Ha nincs UDRs meg vannak adva, Azure alapértelmezett útvonalak használatával teszi lehetővé a forgalmat a egy alhálózat között. Jobban megértheti UDRs, látogasson el a [felhasználó által definiált útvonalak és IP-továbbítási](./virtual-networks-udr-overview.md#ip-forwarding).

Kapcsolati befejeződött, a fenti utolsó kötelező alapján jobb tűzfalat a készülék keresztül biztosítása érdekében a következő útvonal táblázat UDRs tartalmazó **azurevnet**létrehozásához szükséges.

### <a name="azgwudr"></a>azgwudr

Ebben az esetben a csak érkező forgalmat a helyszíni Azure halad kezelése a tűzfalak való csatlakozással **AZF3**szolgálnak, és ezt a forgalmat a belső tűzfalon keresztül, **AZF2**haladjon. Ezért csak egy útvonal szükség az alább látható módon a **GatewaySubnet** .

|Cél|Következő ugrás|MAGYARÁZAT|
|---|---|---|
|10.0.4.0/24|10.0.3.11|Lehetővé teszi, hogy a helyszíni forgalom management tűzfal **AZF3** eléréséhez|

### <a name="azsn2udr"></a>azsn2udr

|Cél|Következő ugrás|MAGYARÁZAT|
|---|---|---|
|10.0.3.0/24|10.0.2.11|Lehetővé teszi az application server **AZF2** keresztül szolgáltatója kódmentes az alhálózathoz forgalom|
|0.0.0.0/0|10.0.2.10|Lehetővé teszi az összes többi forgalom **AZF1** irányítása|

### <a name="azsn3udr"></a>azsn3udr

|Cél|Következő ugrás|MAGYARÁZAT|
|---|---|---|
|10.0.2.0/24|10.0.3.10|Lehetővé teszi, hogy a forgalmat **azsn2** , a webkiszolgáló **AZF2** keresztül az alkalmazás server elérését|

Is kell a alhálózat útvonal táblák létrehozása az **onpremvnet** imitálása a helyszíni adatközponthoz.

### <a name="onpremsn1udr"></a>onpremsn1udr

|Cél|Következő ugrás|MAGYARÁZAT|
|---|---|---|
|192.168.2.0/24|192.168.1.4|Lehetővé teszi, hogy a forgalmat **onpremsn2** **OPFW** keresztül|

### <a name="onpremsn2udr"></a>onpremsn2udr

|Cél|Következő ugrás|MAGYARÁZAT|
|---|---|---|
|10.0.3.0/24|192.168.2.4|Lehetővé teszi, hogy a forgalmat a biztonsági alhálózathoz **OPFW** keresztül Azure-ban|
|192.168.1.0/24|192.168.2.4|Lehetővé teszi, hogy a forgalmat **onpremsn1** **OPFW** keresztül|

## <a name="ip-forwarding"></a>IP-továbbítás 

UDR és IP-továbbítási, amelyek segítségével használhatja funkciók engedélyezése szabályozhatja az Azure VNet forgalom áramlás használandó virtuális készülékek együtt.  A virtuális készülék semmi több, mint egy virtuális, az valamilyen módon, például a tűzfalat vagy hálózati Címfordítást eszköz hálózati forgalmának kezeléséhez használható alkalmazást futtató.

Ez a virtuális készülék virtuális érkeznek bejövő forgalom magát nem címzett kell lennie. Egy virtuális más helyekre címezve forgalom fogadására engedélyezéséhez engedélyeznie kell a IP-továbbítási a virtuális. Ez a-beállítás, nem módosítása a vendégként való bekapcsolódáshoz operációs rendszer Azure. A virtuális készülék továbbra is kezelheti a bejövő forgalmat és irányítja a megfelelő alkalmazás bizonyos típusú futtatásához szükséges.

Többet szeretne tudni az IP-továbbítási, látogasson el a [felhasználó által definiált útvonalak és IP-továbbítási](./virtual-networks-udr-overview.md#ip-forwarding).

Példaként tegyük fel, az alábbi beállítások az Azure vnet is van:

- Alhálózat **onpremsn1** egy virtuális **onpremvm1**nevű tartalmazza.
- Alhálózat **onpremsn2** egy virtuális **onpremvm2**nevű tartalmazza.
- A virtuális készülék **OPFW** nevű **onpremsn1** és **onpremsn2**csatlakozik.
- A felhasználó által definiált útvonalat **onpremsn1** csatolva Itt adhatja meg, hogy minden forgalom **onpremsn2** **OPFW**kell elküldeni.

Ezen a ponton **onpremvm1** **onpremvm2**kapcsolatot próbál létrehozni, ha a UDR szolgálnak, és **OPFW** , mint a következő ugrás küld forgalmat. Ne feledje, hogy a tényleges csomagot cél nem módosul, hogy továbbra is szerint **onpremvm2** a cél. 

IP-továbbítási **OPFW**engedélyezve, nélkül az Azure virtuális hálózati logika fog legördülő csomagokat, mert csak egy virtuális küldeni, ha a virtuális IP-cím a csomag a cél csomag lehetővé teszi.

Az IP-továbbítási az Azure virtuális hálózati logika továbbítja a csomagok OPFW, az eredeti célcím módosítása nélkül. **OPFW** kell kezelni a csomagok és határozza meg, mi a teendő őket.

A fenti eset dolgozik, engedélyeznie kell a IP-a NIC a továbbítási **OPFW**, **AZF1**, **AZF2**és **AZF3** útválasztás használt (a lehetőségekből kezelése az alhálózathoz kivételével az összes NIC). 

## <a name="firewall-rules"></a>Tűzfalszabályokat

Fentebb ismertetett köszönhetően IP-továbbítási csak csomagok elküldi a virtuális berendezések. A készülék még elvégzendő döntse el, hogy mi a teendő, ha azokat a csomagokat. A fenti esetben szüksége lesz a következő szabályok létrehozása a készülékek:

### <a name="opfw"></a>OPFW

OPFW egy helyszíni eszközön, az alábbi szabályokat tartalmazó jelenti:

- **Útvonal**: 10.0.0.0/16 (**azurevnet**) minden forgalom alagutas **ONPREMAZURE**keresztül kell elküldeni.
- **Házirend**: **port2** és **ONPREMAZURE**közötti összes kétirányú forgalmának engedélyezésére.
 
### <a name="azf1"></a>AZF1

AZF1 az alábbi szabályokat tartalmazó Azure virtuális készülék jelenti:

- **Házirend**: **port1** és **port2**közötti összes kétirányú forgalmának engedélyezésére.

### <a name="azf2"></a>AZF2

AZF2 az alábbi szabályokat tartalmazó Azure virtuális készülék jelenti:

- **Útvonal**: 10.0.0.0/16 (**onpremvnet**) minden forgalom az Azure gateway IP-címet (azaz 10.0.0.1) **port1**keresztül kell küldeni.
- **Házirend**: **port1** és **port2**közötti összes kétirányú forgalmának engedélyezésére.

## <a name="network-security-groups-nsgs"></a>Hálózati biztonsági csoportok (NSGs)

Ebben az esetben NSGs nem használják. Azonban minden bejövő és kimenő forgalmának korlátozása alhálózathoz NSGs is alkalmazhat. Például a következő NSG szabályok sikerült alkalmaz a külső Átirányító alhálózat.

**Bejövő levelek**

- Minden TCP-forgalmat engedélyezése az internetről származó minden virtuális az alhálózathoz a 80-as portjához.
- Az összes többi forgalom elutasítása az internetről.

**Kimenő**
- Az összes forgalom elutasítása az internethez.

## <a name="high-level-steps"></a>Magas szintű lépések

Ebben az esetben üzembe helyezéséhez kövesse a magas szintű az alábbi.

1.  Jelentkezzen be az Azure-előfizetésébe.
2.  Ha szeretne egy VNet a helyszíni hálózaton imitálása üzembe, kiépítése az erőforrások **ONPREMRG**részét képező.
3.  Az erőforrások **AZURERG**részét képező kiépítése.
4.  A alagutas a **onpremvnet** való **azurevnet**rendelkezni.
5.  Miután az összes erőforrás kiépített környezetet, jelentkezzen be **onpremvm2** , és a ping 10.0.3.101 **onpremsn2** és **azsn3**közötti kapcsolat tesztelése.
