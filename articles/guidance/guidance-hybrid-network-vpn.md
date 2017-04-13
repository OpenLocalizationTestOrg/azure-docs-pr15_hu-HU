<properties
   pageTitle="A hibrid hálózat architektúrája Azure és a helyszíni virtuális magánhálózati végrehajtási |} Microsoft Azure"
   description="Hogyan lehet egy biztonságos webhely hálózat architektúrája, az Azure virtuális és egy helyszíni hálózat segítségével virtuális Magánhálózattal kapcsolódik nyúló végrehajtásához."
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>A hibrid hálózat architektúrája Azure és a helyszíni VPN végrehajtása

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti, eljárások használata a webhelyen a webhely-virtuális magánhálózat (VPN) Azure alakzatot egy helyszíni hálózaton bővítése csoportja. A forgalmat a helyszíni hálózaton és az Azure virtuális hálózatba (VNet) egy IPSec VPN alagutas között. Ez a felépítés alkalmas a következő jellemzőkkel hibrid alkalmazások:

- Az alkalmazás részei futtassa a helyszíni, míg mások hajtja végre Azure-ban.

- Között a forgalmat a helyszíni hardver és a felhő valószínűleg egyszerűsített verziója, vagy célszerű kereskedelmi rugalmasság és a felhő teljesítményének kissé bővített késés.

- A bővített hálózati zárt rendszer jelent. Nincs közvetlen elérési útjának az internetről az Azure VNet nem.

- Felhasználók csatlakozhatnak az Azure-ban tárolt szolgáltatások használatához a helyszíni hálózaton. A helyszíni hálózaton és Azure-ban futó szolgáltatások között a híd felhasználó számára nem látható.

A profil illeszkedő esetek példája:

- A vállalati verziós alkalmazások használt a szervezeten belül, ahol része a funkciókat tartalmaz áttelepítése megtörtént a felhőbe.

- Fejlesztési/próba/labor munkaterhelésekből.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Azure erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A tervrajz használja, erőforrás-kezelő Microsoft új telepítési javasolja.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli az e-architektúrában összetevőket:

> A Visio-dokumentum, amely tartalmazza a architektúra diagram letölthető a [Microsoft letöltőközpontból][visio-download]. Ez az ábra be van kapcsolva a "hibrid hálózat - VPN" lapot.

![[0]][0]

- **A helyszíni hálózaton.** Ez a hálózat és eszközfüggetlen módon, a szervezeten belül futó helyi magánhálózaton keresztül csatlakozik.

- **Virtuális Magánhálózati készülék.** Ez az eszköz vagy szolgáltatás, ahol a külső kapcsolat a helyszíni hálózaton. A virtuális Magánhálózati készülék lehet egy hardvereszközt, vagy a Útválasztás és távoli hozzáférés szolgáltatás () a Windows Server 2012 például szoftveres megoldást.

> [AZURE.NOTE] Támogatott VPN készülékek és konfigurálásáról kijelölt VPN készülékek csatlakozhat az Azure virtuális Magánhálózati átjáró listáját, olvassa el a megfelelő eszköz a [virtuális Magánhálózati eszközök támogatják az Azure]útmutatót[vpn-appliance].

- **N szintű felhő alkalmazást.** Ez a az Azure-ban tárolt alkalmazást. Többszintű, az Azure terheléselosztókkal keresztül csatlakoztatott több alhálózat tartalmazhat. A forgalmat a minden alhálózathoz tartozhatnak [Azure hálózati biztonsági csoportok (NSGs)]használatával definiált szabályok[azure-network-security-group]. További tudnivalókért lásd: [Microsoft Azure biztonsági – első lépések][getting-started-with-azure-security].

> [AZURE.NOTE] Ez a cikk ismerteti a felhőben alkalmazás egy egységként. Lásd: [Azure-N szintű architektúrája Futtatás] [ implementing-a-multi-tier-architecture-on-Azure] bővebb információt.

- **[Virtuális hálózati (VNet)][azure-virtual-network].** Az azonos VNet a felhőben alkalmazás és a összetevők az Azure virtuális Magánhálózati átjáró tárolnak.

- **[Azure virtuális Magánhálózati átjáró][azure-vpn-gateway].** A virtuális Magánhálózati átjáró szolgáltatás lehetővé teszi, hogy csatlakozzon a VNet a helyszíni hálózaton keresztül egy virtuális Magánhálózati készülék. További tudnivalókért olvassa el a [Csatlakozás Microsoft Azure virtuális hálózathoz egy helyszíni hálózaton][connect-to-an-Azure-vnet]. A virtuális Magánhálózati átjáró a következő elemekből áll:

  - **Virtuális hálózati átjáró.** Ez az erőforrás, amely egy virtuális VPN készülék biztosít a VNet. Érdemes a felelős útválasztási forgalmat a helyszíni hálózaton a VNet.

  - **Helyi hálózaton átjáró.** Ez azoknak a helyszíni VPN készülék absztrakciója. Hálózati forgalmat a helyszíni hálózaton a felhőben alkalmazásból továbbít ezt az átjárót.

  - **Kapcsolat.** A kapcsolat tulajdonságai, adja meg a kapcsolat típusát (IPSec) és a megosztott a helyszíni VPN készülék kulcs forgalom titkosítása tartalmaz.

  - **Átjáró alhálózat.** A virtuális hálózati átjáró tartja a saját alhálózat, amely különböző követelményeknek, amelyekre a javaslatok szakaszban leírt módon.

- **Belső terheléselosztó.** Hálózati forgalmat a virtuális Magánhálózati átjáró továbbítás a felhőben alkalmazás egy belső terheléselosztó keresztül. Az alkalmazás az előtér-alhálózat a terheléselosztó található.

## <a name="recommendations"></a>Javaslatok

Azure felajánlja a különféle számos különböző erőforrásokat és erőforrástípus, így a hivatkozás architektúra lehet kiépítve számos különböző módon. A fenti ajánlást követő hivatkozás architektúra telepíteni egy Azure erőforrás-kezelő sablon nyújtott. Ha úgy dönt, hogy hozzon létre saját hivatkozás architektúrája, kivéve, ha rendelkezik az adott követelmény, hogy nem férnek el a ajánlást célszerű követnie az alábbi javaslatokat.

### <a name="vnet-and-gateway-subnet"></a>VNet és az átjáró alhálózat

Hozzon létre egy Azure VNet összetevők tartsa a felhőben. A címterület használatára vonatkozó az Azure VNet elég nagy a címeket VMs és a VNet az alhálózathoz által használt kell lennie. Gondoskodjon arról, hogy a VNet címterületet elegendő hely a növekedés szükség lehet a jövőben várhatóan további VMs esetén. A címterület használatára, a VNet nem kell a helyszíni hálózaton átfedést. A fenti diagramot például használja a hely címe 10.20.0.0/16 a VNet.

Hozzon létre egy cím tartományát /27 _GatewaySubnet_, nevű alhálózat. Az alhálózathoz van szüksége a virtuális hálózati átjáró, és megakadályozhatja, hogy a jövőben futó lehetséges átjáró méretkorlátja az alábbi 32-címek az alhálózathoz lefoglalása segítik. Ne helyezzen az alhálózathoz a címterületet közepén. Tanácsos új célközönségszabályt felvenni a címterületet az átjáró alhálózat beállítása a felső szélén a VNet címterületet. Az ábrán látható példa 10.20.255.224/27.  A gyors műveletsorral számítja ki a CIDR van az alábbi képlettel történik:

1. A címterület a VNet 1, a bittel, hogy az átjáró alhálózat által használt, majd a többi bit adja meg a 0 felfelé az a változó bittel beállítása

2. Az eredményül kapott bittel konvertálása decimális és express, mint az átjáró alhálózat méretét a előtag hossza értéke-címterület használatára.

Ha például egy IP-címtartományokat 10.20.0.0/16, a VNet, alkalmazása a fenti # 1 lesz 10.20.0b11111111.0b11100000.  Amely konvertálása decimális és kifejező, mint egy címterület együttesen 10.20.255.224/27

> [AZURE.WARNING] Telepítsen más virtuális gépeken futó vagy szerepkör-példányok az átjáró alhálózathoz. Is nem hozzárendelése egy NSG az alhálózathoz, mint le szeretné állítani, hogy működik-e az átjáró okoz.

### <a name="virtual-network-gateway"></a>Virtuális hálózati átjáró

Lefoglalhat egy nyilvános IP-címet a virtuális hálózati átjáró.

A virtuális hálózati átjáró létrehozása az átjáró alhálózat, és rendelje hozzá az újonnan kiosztott nyilvános IP-címet. Az átjáró típus az igényeknek leginkább megfelelő használata és amely a virtuális Magánhálózati készülék engedélyezve van:

[Házirend-alapú átjáró] létrehozása[ policy-based-routing] Ha szorosan szabályozhatja a kérések továbbításáról kell. például a cím prefixumokban házirend-feltételek alapján. Házirend-alapú átjárók statikus átirányításra, és csak olyan hely közötti kapcsolatok dolgozhat.

Hozzon létre egy [átjáró útvonal-alapú] [ route-based-routing] Ha a helyszíni hálózaton RRAS használatával szeretne csatlakozni, támogatja a több elem webhely vagy határokon-régió kapcsolatokat vagy megvalósítása VNet-VNet kapcsolatok (utakat, amely több VNets is beleértve). Átjárók útvonal-alapú használata a dinamikus útválasztás közvetlen forgalom hálózatok között. Azok elviseli jobb statikus útvonalak, mint a hálózati elérési utat a hibák, mivel az alternatív útvonalak is Displayed. Átjárók útvonal-alapú is csökkentheti a kezelés átfedés, mert útvonalak előfordulhat, hogy nem kell manuálisan frissülnek, amikor hálózati címek módosítása.

Támogatott VPN készülékek listáját, lásd: [A virtuális Magánhálózati eszközök webhely virtuális Magánhálózati átjáró kapcsolatok][vpn-appliances].

> [AZURE.NOTE] Az átjáró létrehozása után törlése, és hozza létre újra az átjáró nélkül átjáró típusok között nem módosítható.

Jelölje ki az Azure virtuális Magánhálózati átjáró Termékváltozat átviteli igényeinek leginkább megfelelő. Azure virtuális Magánhálózati átjáró három termékváltozatban, a következő táblázatban érhető el. Minden egyes Termékváltozat tartalmaz különböző átviteli [díj van érvényben] [ azure-gateway-charges] alapján, hogy az átjáró már kiépítve idő mennyiségét és érhető el.

| RAKTÁRI SZÁM | Virtuális Magánhálózati átviteli | Max IPSec alagutak|
|-----|----------------|------------------|
| Egyszerű | 100 MB | 10 |
| Normál | 100 MB | 10 |
| Nagy teljesítmény | 200 MB | 30 |

> [AZURE.NOTE] Az egyszerű Termékváltozat fájl nem kompatibilis a készült Azure ExpressRoute. [A Termékváltozat] módosítható[ changing-SKUs] az átjáró létrehozása után.

A bejövő alkalmazás közvetlen forgalom az az átjáró alhálózat útválasztási szabályok létrehozása az átjáró a belső terheléselosztó, hanem közvetlenül az, hogy az alkalmazás megvalósítása VMs át kérések engedélyezése.

### <a name="on-premises-network-connection"></a>A helyszíni hálózati kapcsolat

A helyi hálózati átjáró létrehozása Adja meg a nyilvános IP-címe a helyszíni VPN készülék és a címterület használatára, a helyszíni hálózaton. Ne feledje, hogy a helyszíni VPN készülék kell egy nyilvános IP-címet, a helyi hálózaton átjáró Azure virtuális Magánhálózati átjáró által is elérhető. A virtuális Magánhálózati eszköz nem található egy hálózati Címfordítást eszköz mögött.

A virtuális hálózati átjáró és a helyi hálózati átjáró hely közötti kapcsolat létrehozása. Jelölje ki a webhely (IPSec) kapcsolat típusát, és adja meg a megosztott kulcsot. Webhely titkosítás: Azure virtuális Magánhálózati átjáró IPSec protokollt, előre megosztott kulcsok használatának hitelesítési alapul. Adja meg a kulcsot az Azure virtuális Magánhálózati átjáró létrehozásakor. Meg kell adnia a virtuális Magánhálózati készülék futó helyszíni ugyanazzal a kulccsal. További hitelesítési mechanizmusok jelenleg nem támogatott.

Győződjön meg arról, hogy a helyszíni útválasztási infrastruktúra úgy van konfigurálva, hogy az Azure VNet a virtuális Magánhálózati eszközre címek szánt kérések továbbítása.

Nyissa meg bármelyik portokat követel meg a felhőben alkalmazás a helyszíni hálózaton.

Annak ellenőrzéséhez, hogy a kapcsolat tesztelése:

- A helyszíni VPN készülék megfelelően a forgalom átirányítása a felhőben alkalmazás az Azure virtuális Magánhálózati átjáró keresztül.

- A VNet megfelelően a forgalom visszatérhet a helyszíni hálózaton.

- Mindkét irányba tiltott forgalmának megfelelően le van tiltva.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Korlátozott függőleges méretezhetőség a nagy teljesítmény VPN Termékváltozat a Basic vagy szabványos VPN átjáró termékváltozatok áthelyezésével érhet el.

Amely a virtuális Magánhálózati forgalmat nagy mennyiségű várt VNets érdemes megfontolni a különböző feladatok be külön kisebb VNets terjesztése, és egy virtuális Magánhálózati átjárót konfigurálása az egyes őket.

A VNet szintén szétválaszthatók, vízszintesen vagy függőlegesen. Partition vízszintesen, helyezze virtuális esetenként minden réteg alhálózat, az új VNet. Az eredmény azért, hogy minden VNet van-e a struktúra és a használható funkciók körét. Partition függőlegesen, hogy minden réteg funkcióit (például rendelések kezelése, Számlázás, felhasználói fiókok kezelése, és így tovább) különböző logikai területekre osztáshoz újratervezéséhez. Minden egyes funkcionális terület saját VNet majd kerül.

Egy helyszíni Active Directory tartományvezérlőnek a VNet replikálása, és a DNS végrehajtása a VNet, csökkentheti a biztonsággal kapcsolatos és felügyeleti forgalmat a helyszíni lépés, a felhőbe részét segítséget.

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Annak érdekében, hogy a helyszíni hálózaton használható formában megmaradnak az Azure virtuális Magánhálózati átjáró van szüksége, ha a helyszíni VPN átjáró hajtja végre a Feladatátvevőfürt.

Ha szervezete több a helyszíni webhely, [több webhelyen kapcsolat] létesítése[ vpn-gateway-multi-site] szeretne egy vagy több Azure VNets. Ez a módszer igényli dinamikus (útvonal-alapú) útválasztás, így biztosítva, hogy a helyszíni VPN-átjáró támogatja ezt a szolgáltatást.

Lásd: a [virtuális Magánhálózati átjáró SLA] [ sla-for-vpn-gateway] a virtuális Magánhálózati átjáró SLA olvashat.

## <a name="manageability-considerations"></a>Kezelhetőség kapcsolatos szempontok

A helyszíni VPN készülékek diagnosztikai adatok figyelése Ez az eljárás attól függ, hogy a virtuális Magánhálózati készülék szolgáltatását. Például ha használ a Útválasztás és a távoli szolgáltatás Windows Server 2012, engedélyeznie kell a [naplózás RRAS][rras-logging].

Használja a [Azure virtuális Magánhálózati átjáró diagnosztika] [ gateway-diagnostic-logs] rögzítéséhez csatlakozási problémákat tapasztal információt. Ezek a naplók használható nyomon követheti az információkat, például a forrás- és rendeltetési helye kapcsolat kérelmeket, használt protokollt, és hogyan a kapcsolat létrehozása (vagy a miért nem sikerült).

A műveleti naplók az Azure virtuális Magánhálózati átjáró érhető el az Azure-portálon naplókat figyelése A helyi hálózati átjáró, az Azure hálózati átjáró és a kapcsolat külön naplók érhetők el. Ezt az információt az átjáró végzett módosítások nyomon követésére használható, és akkor lehet hasznos, ha korábban már működik az átjáró nem működik a valamilyen okból.

![[2]][2]

Lync-kapcsolat, és csatlakozási hiba események nyomon követéséhez. Használhatja például [Nagios] felügyeleti csomag[ nagios] rögzítheti és jelentés ezt az információt.

## <a name="security-considerations"></a>Biztonsági megfontolások

Minden VPN átjáró egy másik megosztott kulcs generálása. Használatával a megosztott kulcs erős próbálkozásos támadások ellenálljon.

> [AZURE.NOTE] Jelenleg nem használható Azure kulcs tárolóra előre megosztott kulcsok Azure virtuális Magánhálózati átjáró.

Biztosítani, hogy a helyszíni VPN készüléket használ, [az Azure virtuális Magánhálózati átjáró szolgáltatással kompatibilis]titkosítási metódus[vpn-appliance-ipsec]. Házirend-alapú útválasztás az Azure virtuális Magánhálózati átjáró támogatja a AES256 AES128 és 3DES titkosítási algoritmust. Az átjárók útvonal-alapú AES256 és 3DES támogatja.

Ha a helyszíni VPN készülék be van kapcsolva, hogy van-e tűzfal a peremhálózat és az Internet között peremhálózat, lehet, ha [További tűzfalszabályokat] konfigurálni[ additional-firewall-rules] engedélyezése a webhelyen a webhely-virtuális Magánhálózati kapcsolatot.

Ha az alkalmazás a VNet adatokat küld az internethez, fontolja meg [a kényszerített tunneling] [ forced-tunneling] összes internetes kötött forgalmat a helyszíni hálózaton keresztül. Ezt a megközelítést lehetővé teszi, hogy az alkalmazás a helyszíni infrastruktúra kimenő kérések naplózási.

> [AZURE.NOTE] Kényszerített tunneling hatással lehet a csatlakozási Azure szolgáltatások (a tárhely szolgáltatás, például) és a Windows-licenc kezelő.

## <a name="troubleshooting-considerations"></a>Hibaelhárítási kapcsolatos szempontok

> [AZURE.NOTE] Látogasson el a Útválasztás és távoli hozzáférés Blog általános információt a [Gyakori VPN kapcsolatos hibák elhárítása][troubleshooting-vpn-errors].

Ha forgalom nem tudja a virtuális Magánhálózati kapcsolat bejárásához, ellenőrizze az alábbiakat:

### <a name="on-premises-vpn-appliance"></a>A helyszíni VPN készülék:

**A helyszíni VPN készülék megfelelően működik-e?**

A VPN Használatát a készülék hibák és hibák által létrehozott naplófájlok jelölőnégyzetet. Hol található ez az információ a készülék megfelelően változik. Például ha RRAS használja a Windows Server 2012, is használhatja az alábbi PowerShell-parancsot a RRAS szolgáltatás hiba esemény információkat jeleníthet meg:

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

Az _üzenet_ tulajdonsága az egyes bejegyzések a hiba leírását tartalmazza. Néhány hétköznapi példát a következők:

- Meggátoló szeretne csatlakozni, esetleg nem a megfelelő IP-címet az Azure virtuális Magánhálózati átjáró a RRAS VPN a hálózati kapcsolat konfigurációban megadott miatt:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- A megfelelő megosztott billentyűt, a RRAS VPN a hálózati kapcsolat konfigurációban megadott:

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

Szerezze be a eseménynaplójában talál információt kísérel meg csatlakozni a RRAS szolgáltatáson keresztül az alábbi PowerShell-parancs használatával is:

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

Kapcsolódás a hiba esetén a napló hibáról, az alábbihoz hasonló fogja tartalmazni:

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**A virtuális Magánhálózati készülék megfelelően adatforgalom az Azure virtuális Magánhálózati átjáró keresztül?**

Például a [PsPing] eszközzel[ psping] kapcsolódási és útválasztás keresztül a virtuális Magánhálózati átjáró megerősítéséhez. Ha például a VNet található webkiszolgálóra történő egy helyi gépről kapcsolat tesztelése, futtassa a következő parancsot (cseréje `<<web-server-address>>` a címet, a webkiszolgáló):

```
PsPing -t <<web-server-address>>:80
```

Ha a helyi számítógép is irányítja a forgalmat az érintett webkiszolgálóra, meg kell jelennie eredménye az alábbihoz hasonló:

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

Ha a helyi számítógép nem tud kommunikálni a megadott cél, látni fogja a üzenetek jelennek meg:

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**A tűzfal engedélyezi a virtuális Magánhálózati irányítja át? Megnyitott portok?**

Ellenőrizze, hogy a helyszíni VPN készüléket használ, [az Azure virtuális Magánhálózati átjáró szolgáltatással kompatibilis]titkosítási metódus[vpn-appliance].

Házirend-alapú útválasztás az Azure virtuális Magánhálózati átjáró támogatja a AES256 AES128 és 3DES titkosítási algoritmust. Az átjárók útvonal-alapú AES256 és 3DES támogatja.

### <a name="azure-vnet-gateway"></a>Azure VNet átjáró:

Vizsgálja meg [a diagnosztikai naplók Azure virtuális Magánhálózati átjáró] [ gateway-diagnostic-logs] a potenciális problémákról.

**És vannak az Azure virtuális Magánhálózati átjáró ugyanazzal a megosztott hitelesítési kulccsal konfigurált helyszíni VPN készülék?**

A megosztott billentyűt az Azure virtuális Magánhálózati átjáró tárolja a következő Azure CLI paranccsal tekintheti meg:

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

A paranccsal a helyszíni VPN készülék megfelelő kattintva jelenítse meg a készülék be van állítva a megosztott billentyűt.

Ellenőrizze, hogy a _GatewaySubnet_ alhálózat tartó az Azure virtuális Magánhálózati átjáró nem kapcsolódik egy NSG.

Az alhálózathoz adatait a következő Azure CLI paranccsal tekintheti meg:

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

Győződjön meg róla van _hálózati biztonsági csoport azonosító_neve nincs adatmezőt. Az alábbi példa eredménye például egy tevékenységhez rendelt NSG (_VPN-átjáró-csoport_) tartalmazó _GatewaySubnet_ látható. Ez a okozhat megakadályozhatja, hogy az átjáró a megfelelően működik-e NSG megadott szabályoknak esetén:

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**A virtuális gépeken futó az Azure VNet a bíró forgalmat a érkező kívül a VNet? Jelölje be az alábbi virtuális gépeken futó tartalmazó alhálózat társított NSG szabályokat.**

Az összes NSG szabályok a következő Azure CLI paranccsal tekintheti meg:

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**Megszakadt az Azure virtuális Magánhálózati átjáró?**

Az alábbi Azure PowerShell parancs használatával az Azure virtuális Magánhálózati kapcsolat aktuális állapotának ellenőrzése. A `<<connection-name>>` paraméter az Azure virtuális Magánhálózati kapcsolat, amely a virtuális hálózati átjáró és a helyi átjáró neve.

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

A következő kódrészletek jelölje ki a kimenet jön létre, ha az átjáró csatlakoztatva van (az első példát), és be megszakad a kapcsolata (a második példában):

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>Host virtuális konfigurációs, a hálózati sávszélesség-kihasználtsági és az alkalmazás teljesítményének:

Ellenőrizze, hogy a a tűzfalat a vendégként való bekapcsolódáshoz operációs rendszerben futó az Azure VMs az alhálózathoz a helyesen vannak-e beállítva a helyi IP-címtartományai engedélyezett forgalmának engedélyezésére.

**Érhető el a forgalom mennyiségű sávszélesség határértékén közelébe az Azure virtuális Magánhálózati átjáró?**

Szerszámok attól függ, hogy a rendelkezésére a VPN Használatát a készülék futó helyszíni eszközökkel. Például ha RRAS használja a Windows Server 2012, használhatja teljesítmény Monitor nyomon követéséhez a kapott és a virtuális Magánhálózati kapcsolat; átvitt adatok mennyisége _RAS összes_ objektumot használva, jelölje be a _Bájt kapott/Sec_ és a _Továbbított/Sec bájt_ számláló:

![[3]][3]

Az eredmények (a Basic és a szokásos termékváltozatok 100 MB és a nagy teljesítmény Termékváltozat az 200 MB) a virtuális Magánhálózati átjáró sávszélességgel kell összehasonlítása:

![[4]][4]

**Van bármilyen a virtuális gépeken futó az Azure VNet a lassan működik? Azok a túlterhelt vannak, elég el őket a betöltés kezelése vannak az összes-terheléselosztókkal megfelelően konfigurálva vannak?**

Ehhez a [rögzítésével és a diagnosztikai adatok elemzése][azure-vm-diagnostics]. Ellenőrizheti, hogy az eredmények az Azure portálon, de számos külső eszközök is elérhető nyújt részletes az összefüggéseket a teljesítményadatok be.

**Az felhő erőforrások használni az alkalmazást, hogy hatékony?**

Eszköz alkalmazás kódja futó minden virtuális határozza meg, hogy alkalmazásokat az erőforrások felhasználási készít. Használható eszközök, például az [Alkalmazás az összefüggéseket] [ application-insights] érdekében ezeket a műveleteket.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

Ha egy meglévő helyszíni infrastruktúra már be van állítva egy virtuális Magánhálózati készülék, ezeket a lépéseket követve telepítheti a hivatkozás architektúra:

1. Kattintson a jobb gombbal az alábbi gombra, és válassza a bármelyik "hivatkozás megnyitása új lapon" vagy "A hivatkozás megnyitása új ablakban":  
[![Azure telepítése](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. Megvárja, amíg az Azure-portálon nyissa meg a hivatkozásra, majd tegye a következőket: 
    - Az **erőforráscsoport** neve már van megadva a paraméter fájlt, így válassza az **Új létrehozása** , és adja meg `ra-hybrid-vpn-rg` a szövegmezőbe.
    - A **hely** legördülő listából válassza ki a régió.
    - Ne módosítsa a **Sablon legfelső szintű Uri** vagy a **Paraméter legfelső szintű Uri** mezőbe.
    - Tekintse át a szerződési feltételeket, majd jelölje be az **elfogadom a fenti feltételek** jelölőnégyzetet.
    - Kattintson a **vásárlás** gombra.

3. Várja meg, a telepítés befejezéséhez.

## <a name="next-steps"></a>Következő lépések

- [Az Azure VNet VMs használata a helyszíni Active Directory tartomány esetén további tartományt vezérlők telepítése][installing-ad].

- [A Windows Server Active Directory Azure VMs telepítési útmutatója][deploying-ad].

- [A DNS-kiszolgáló létrehozása egy VNet][creating-dns].

- [Beállításáról és kezeléséről a VNet DNS][configuring-dns].

- [A helyszíni Stormshield hálózati biztonsági készülékek használata egy virtuális Magánhálózaton keresztül][stormshield].

- [A hibrid hálózat architektúrája készült Azure ExpressRoute a végrehajtási][expressroute].

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "Hibrid szerkezetének hálózati tartó a helyszíni, és a felhő infrastruktúra"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "Szétválasztás egy VNet méretezhetőség javítása érdekében"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "Naplók az Azure-portálon"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "Virtuális Magánhálózati hálózati forgalmának engedélyezésére figyelemmel kísérésére teljesítmény számláló"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "Példa VPN hálózati teljesítmény graph"