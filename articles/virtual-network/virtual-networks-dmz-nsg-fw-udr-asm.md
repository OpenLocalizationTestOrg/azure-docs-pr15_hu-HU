<properties
   pageTitle="Példa DMZ – összeállítása a DMZ védelme a tűzfalat, UDR és NSG hálózatok |} Microsoft Azure"
   description="A DMZ tűzfallal összeállítása, felhasználó által definiált útválasztási (UDR), és a hálózati biztonsági csoportok (NSG)"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/01/2016"
   ms.author="jonor;sivae"/>

# <a name="example-3--build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg"></a>Például: 3 – a tűzfal, UDR és NSG hálózatok védelme a DMZ összeállítása

[Lépjen vissza a biztonsági oszlopazonosító ajánlott eljárások a lapra][HOME]

Ebben a példában a DMZ hoz létre, a tűzfalat, négy windows-kiszolgálók, felhasználó által definiált útválasztás, IP-továbbítási és hálózati biztonsági csoportokat. Minden egyes lépés egy mélyebb megértése megadására vonatkozó parancsok keresztül is azt ismerteti. Is van egy mélyreható megadására forgalom forgatókönyv szakasz részletes hogyan a forgalmat a DMZ a védelmet a rétegek végighalad. Végül a hivatkozás szakaszban az teljes kódot és útmutatás a környezet ellenőrzéséhez és kísérletezhet a különböző forgatókönyvekben össze. 

![Kétirányú DMZ NVA, NSG és UDR][1]

## <a name="environment-setup"></a>Környezet beállítása
Ebben a példában az előfizetés, amely tartalmazza az alábbiakat:

- Három cloud services: "SecSvc001", "FrontEnd001" és "BackEnd001"
- A virtuális hálózati "CorpNetwork", a három alhálózat: "SecNet", "FrontEnd" és "Kódmentes"
- Ebben a példában a tűzfalat, a SecNet alhálózathoz csatlakoztatott egy hálózati virtuális készülék
- A Windows Server, amely az alkalmazás webkiszolgáló ("IIS01")
- Vissza az alkalmazás jelképező két windows-kiszolgálók befejezése servers ("AppVM01", "AppVM02")
- A Windows server, amely a DNS-kiszolgáló ("DNS01")

A hivatkozások szakaszban alatti van egy PowerShell-parancsprogramot, amely a fentebb ismertetett környezet a legtöbb gyűjt. A VMs és virtuális hálózatok épület ugyan a példában parancsfájl által végzett nem témakörben olvashat részletesen a dokumentumban.

A környezet létrehozásához:

  1.    A hivatkozások szakaszban szereplő hálózati beállítások XML-fájl mentése (frissíti neve, hely és IP-címek a megfelelő az adott esetben)
  2.    A környezet a parancsfájl van (az előfizetések, szolgáltatásnevek stb.) futtassanak megfelelően a parancsfájl felhasználói változók frissítése
  3.    A parancsprogram végrehajtása a PowerShell

**Megjegyzés**: A régió esetében a PowerShell-parancsprogramot, meg kell egyeznie a hálózati konfigurációja XML-fájlban szereplő.

Miután sikeresen futtatható parancsfájl a következőképpen utáni parancsfájl lehet venni:

1.  A tűzfal szabályok beállítása, ez foglalt lejjebb című szakaszt: tűzfal szabály leírása.
2.  Másik lehetőségként a hivatkozások szakaszban vannak két parancsfájl, a webkiszolgáló és engedélyezése ebben a konfigurációban DMZ tesztelése egyszerű webalkalmazás server alkalmazás beállítása.

Miután a parancsfájl sikeresen fut a tűzfal szabályokat kell elvégezni, ez foglalt című szakaszt: tűzfalszabályokat.

## <a name="user-defined-routing-udr"></a>Felhasználó által definiált útválasztási (UDR)
Alapértelmezés szerint az alábbi rendszer útvonalak meghatározásuk szerint:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

A VNETLocal értéke mindig a VNet adott hálózat (ie azt változik VNet VNet attól függően, hogy hogyan minden adott VNet van megadva), a megadott cím prefix(es). A hátralévő rendszer útvonalak statikusak, valamint a fenti alapértelmezett.

Prioritás, mint útvonalak feldolgozása keresztül a leghosszabb előtag hol.van (LPM) módszer, így a legjobban megfelelő útvonalon a táblázat egy adott címzett címét szeretné alkalmazni.

Emiatt a helyi hálózat (10.0.0.0/16) szánt (például hogy DNS01 server 10.0.2.4) adatforgalom végig a VNet miatt az 10.0.0.0/16 útvonalat a címzetthez lesz átirányítva. Más szóval 10.0.2.4, az a 10.0.0.0/16 út a legjobban megfelelő útvonal annak ellenére, hogy a 10.0.0.0/8 és 0.0.0.0/0 is lehet alkalmazni, de mivel kisebb adott nincsenek hatással a forgalmat. Így 10.0.2.4 forgalmat szeretne egy a helyi VNet a következő ugrás van, és egyszerűen átirányítása az a cél.

Ha volt szánt adatforgalom 10.1.1.1 például 10.0.0.0/16 útvonal nem alkalmazásához, de a 10.0.0.0/8 lenne a legjobban megfelelő, és a forgalom lenne kihagyott ("fekete furatos"), mivel a következő ugrás értéke Null. 

Ha a cél nem vonatkozik a Null prefixumokban vagy a VNETLocal prefixumokban közül, majd adott legalább volna kövesse irányítása, 0.0.0.0/0 és továbbíthatók az internethez, a következő ugrás és így Azure meg az internet él meg.

Ha az útvonal táblázat két azonos prefixumokban, az alábbiakban az útvonal "forrás" attribútum alapján elsőbbségi sorrendjének:

1.  "VirtualAppliance" = a táblázatot manuálisan hozzá felhasználói definiált útvonal
2.  "VPNGateway" = A dinamikus útvonalat (hibrid hálózatokkal való használatakor BGP), dinamikus hálózati protokoll által hozzáadott, ezek az útvonalak idő után megváltoznak, a dinamikus protokoll automatikusan tükrözi, peered hálózati változásai
3.  "Alapértelmezett" = a rendszer útvonalak, a helyi VNet, valamint a statikus bejegyzéseket, a fenti útvonal táblázatban látható módon.

>[AZURE.NOTE] Most már használhatja felhasználó által definiált Routing (UDR) készült ExpressRoute és a virtuális Magánhálózati átjárók kényszerítse a bejövő és kimenő e-mailként hálózati virtuális-készülék (NVA) való határokon helyszíni-forgalmat.

#### <a name="creating-the-local-routes"></a>A helyi útvonalak létrehozása

Ebben a példában a két útválasztási táblák van szükség, egy minden Frontend és Kódmentes alhálózat lenne. Az adott alhálózat szükséges statikus útvonalakkal betöltött minden táblát. Céljából ebben a példában mindegyik táblázatban szerepel három útvonalak:

1. A tűzfal megkerülje helyi alhálózat forgalmának engedélyezésére definiált helyi alhálózat forgalmat a következő ugrás
2. Virtuális a következő ugrási jelenti tűzfal a hálózati forgalmat, ez az alapértelmezés szerinti szabályt, amely lehetővé teszi a helyi VNet forgalom átirányítása közvetlenül felülbírál
3. A következő ugrás a tűzfal jelenti az összes hátralévő forgalom (0 és 0)

Az útválasztási táblázatok létrehozása után azok a alhálózathoz kötött. A Frontend alhálózat útválasztási tábla, miután létrehozott, illetve az alhálózathoz kötött így néz ki:

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active


Ebben a példában a következő parancsok végrehajtása az útvonal táblázat összeállítása, a felhasználó által definiált út hozzáadása és majd kötést létrehozni az útvonal táblázat alhálózat használatosak (Megjegyzés; egy dollárjelet kezdődő alatti elemek (például: $BESubnet) a dokumentum hivatkozás szakaszában a forgatókönyv a felhasználó által definiált változók):

1.  Az alap útválasztási táblázat először létre kell. Ez a kódtöredék létrehozása a táblázat a Kódmentes alhálózat jeleníti meg. A parancsfájlt, a megfelelő létrejön egy táblázat is a Frontend alhálózat.

        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"

2.  Az útvonal táblázat létrehozása után az adott felhasználó által definiált útvonalak felvehetők. Az e snipped minden forgalom (0.0.0.0/0) keresztül a virtuális készülék (változó [0], $VMIP használt átadni a virtuális készülék a parancsfájl a korábban létrehozott IP-cím) történik. A parancsfájlt, a megfelelő szabály is létrehozott a Frontend táblázatban.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

3. A fenti útvonal bejegyzés felülírja az alapértelmezett "0.0.0.0/0" útvonal, de még mindig meglévő alapértelmezett 10.0.0.0/16 szabály amely lehetővé teszi a belül a VNet, és közvetlenül a cél nem pedig a hálózati virtuális készülék irányítja a forgalmat. Helyes-e ez a probléma a követés szabály hozzá kell adni.

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]

4. Ezen a ponton választási lehetőség van kell tenni. A fenti két útvonalat minden forgalom irányítja, a tűzfal felméréshez, még akkor is forgalom egyetlen alhálózat. Ez lehet feltétlenül szükség, azonban és irányítja a helyi meghajtóra nélkül a tűzfalat a részvétel a harmadik alhálózat forgalmának engedélyezésére, nagyon konkrét szabály manuálisan is hozzáadhatók. Ez az útvonal bármely cím destine esetében a helyi alhálózat is csak állapotok közvetlenül irányítása van (NextHopType = VNETLocal).

        Get-AzureRouteTable $BERouteTableName | `
            Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal

5.  Végül a útválasztási táblázatot létrehozni, és a felhasználó által definiált útvonalak feltöltve, a táblázat most kell kötni alhálózat. A parancsfájl az előtér-útválasztási tábla is kötött Frontend az alhálózathoz. Az alábbiakban a kötelező parancsfájl vissza vége alhálózat.

        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
           -SubnetName $BESubnet `
           -RouteTableName $BERouteTableName

## <a name="ip-forwarding"></a>IP-továbbítás
A kereső UDR, hogy a funkció IP-továbbítási. Ez a beállítás egy virtuális készülék, amely lehetővé teszi, hogy kifejezetten nem címezve a készülék adatforgalmat és mailjeinek átirányítása a végső címzetthez forgalmat a.

Példaként forgalmat a AppVM01 megkeresést DNS01-kiszolgálóhoz, ha UDR volna irányítja a ezt a tűzfalon keresztül. IP-továbbítási engedélyezve a forgalmat a DNS01 cél (10.0.2.4) fog kell fogadja el a készülék (10.0.0.4), és kattintson a végső címzetthez (10.0.2.4) továbbítsák. IP-továbbítási engedélyezve van a tűzfalon, nélkül forgalom volna nem fogadja el a készülék annak ellenére, hogy az útvonal táblának a tűzfalat, mint a következő ugrás. 

>[AZURE.IMPORTANT] Rendkívül fontos kell tartania, hogy IP-továbbítási engedélyezése a felhasználó által definiált útválasztás együtt.

IP-továbbítási beállítása egyetlen parancs és virtuális létrehozási idejének legyen végezhető. Az ebben a példában a folyamat a kódrészletet a parancsfájl vége felé, és a UDR parancsok csoportosítva:

1.  Hívja fel a virtuális példányt, amely a virtuális készülék, a tűzfal ebben az esetben, és engedélyezze a IP-továbbítási (Megjegyzés; a piros kezdődő egy dollárjelet bármelyikére (pl.: $VMName[0]) a felhasználó által definiált változó a forgatókönyv a dokumentum hivatkozás szakaszában. Zárójelek [0], a nulla jelképezi a VMs módosítás nélkül szoktam példa parancsfájl tömbje az első virtuális, az első virtuális (virtuális 0) kell lennie a tűzfal):

        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] | `
           Set-AzureIPForwarding -Enable

## <a name="network-security-groups-nsg"></a>Hálózati biztonsági csoportok (NSG)
Ebben a példában NSG csoport összeállítása és majd betöltött egyetlen szabályok. Ebben a csoportban majd kötött csak a Frontend és Kódmentes alhálózat (nem a SecNet). A következő szabályt deklaratív éppen beépített:

1.  Minden forgalom (az összes portok) az internetről az egész VNet (az összes alhálózat) van megtagadja

Bár NSGs ebben a példában használnak, annak fő célja, a másodlagos réteg helytelen a manuális konfiguráció ellen védelmet. Szeretnénk blokkolja a minden bejövő forgalmat az internetről vagy a Frontend Kódmentes alhálózat, forgalom kell csak adatfolyam az SecNet alhálózat a tűzfalon keresztül (és megjelenítheti a Frontend vagy Kódmentes megfelelő majd alhálózat). Plusz helyen UDR szabályokkal minden forgalom webhelyé fejeződött be a Frontend vagy Kódmentes alhálózat volna meg a irányítja a tűzfal (köszönetnyilvánító UDR). A tűzfal módon jelenik meg, egy aszimmetrikus adatfolyam, és szeretné húzhatja a kimenő forgalmának engedélyezésére. Így az alábbi három réteg biztonsági Frontend és Kódmentes alhálózat; védelme 1) nyissa meg a FrontEnd001 és BackEnd001 végpontok felhőszolgáltatásokba, 2) NSGs forgalom elutasítja az internetről 3) a húzással történő áthelyezéséről aszimmetrikus tűzfal forgalmat.

Egy érdekes ponttal kapcsolatos ebben a példában a hálózati biztonsági csoport, hogy a tartalmazza csak egy szabályt, alább, amely olyan, a teljes virtuális hálózat, amely tartalmazza a biztonsági alhálózat internetes forgalmat letiltja. 

    Get-AzureNetworkSecurityGroup -Name $NSGName | `
        Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet `
        from the Internet" `
        -Type Inbound -Priority 100 -Action Deny `
        -SourceAddressPrefix INTERNET -SourcePortRange '*' `
        -DestinationAddressPrefix VIRTUAL_NETWORK `
        -DestinationPortRange '*' `
        -Protocol *

Azonban csak a NSG kötött a Frontend és Kódmentes alhálózat, mivel a szabály nem feldolgozásának forgalmat a bejövő biztonsági az alhálózathoz. Eredményt adja akkor is, ha a NSG szabály bármely cím nem internetes forgalmat arról tájékoztat a VNet, mert a NSG soha nem kötve a biztonsági alhálózat forgalom fog adatfolyam biztonsági az alhálózathoz.

    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $FESubnet -VirtualNetworkName $VNetName
    
    Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName `
        -SubnetName $BESubnet -VirtualNetworkName $VNetName

## <a name="firewall-rules"></a>Tűzfalszabályokat
A tűzfalat, a továbbítási szabályokban kell létrehozni. Mivel a tűzfal blokkolja a vagy a hívásátirányítás minden bejövő, kimenő és a belüli-VNet forgalom sok tűzfalszabályokat van szükség. Az összes bejövő forgalmat is, találati a biztonsági szolgáltatása nyilvános IP-címét (a különböző portok), a tűzfal feldolgoztatni. A legjobb a logikai flow, mielőtt beállítaná a alhálózat diagram és tűzfalszabályokat elkerülése érdekében átdolgozási később. Az alábbi ábrán az ebben a példában a tűzfalszabályokat logikai megjelenítése:
 
![A tűzfal szabályok logikai megtekintése][2]

>[AZURE.NOTE] A hálózati virtuális készülék használt alapján, a kezelés portokat változhatnak. Ebben a példában a Barracuda NextGen tűzfal hivatkozott használó 22, 801 és 807. A dokumentációjában készülék szállító kezelése a használt eszköz használható pontos portok kereséséhez.

### <a name="logical-rule-description"></a>Logikai szabály leírása
A fenti logikai diagram a biztonsági alhálózathoz nem jelenik meg, a tűzfal legyen az egyetlen erőforrás adott alhálózat, és ez az ábra, jelenik meg, a tűzfalszabályokat és hogyan logikailag engedélyezése vagy tiltása a forgalom és a tényleges útválasztásos elérési. Is, a külső portokat a RDP-forgalmat, nagyobb kijelölt volt portokat (8014 – 8026), és a helyi IP-címek könnyebb olvashatóság érdekében az utolsó két bájt némileg igazítani kiválasztásának (például helyi kiszolgáló címét 10.0.1.4 társítva külső port 8014), azonban valamelyik újabb nem ütköző port felhasználható.

Ebben a példában 7 típusú szabályok szükséges, ezek szabály típusai leírt a következők:

- Külső szabályok (a bejövő forgalom):
  1.    Tűzfal Management szabály: Alkalmazás átirányítási szabály lehetővé teszi a forgalmat a hálózati virtuális készülék az adatkezelési portokat át.
  2.    RDP szabályairól (egyes windows server esetén): E négy szabályok (egy adott kiszolgáló) lehetővé teszi az egyes kiszolgálók keresztül RDP kezelésének. Ez lehet is lehet egyszerre kezelni be egy szabályt, attól függően, hogy a funkciók a hálózat virtuális készülék használatban.
  3.    Alkalmazás forgalom szabályokat: Két alkalmazás forgalom szabályok vonatkoznak, az előtér-webes forgalom az első és hátsó vége forgalmához (pl. webkiszolgálót adatok réteg) a második. A konfiguráció szabályok fog függenek, hogy a hálózat architektúrája (ahol a kiszolgálók kerülnek) adatforgalom és flow (irányát a forgalom, és amelyek portok használt).
      - Az első szabály lehetővé teszi a tényleges alkalmazás forgalom az application server eléréséhez. Miközben a többi szabályokat a biztonsági, management stb engedélyezéséhez alkalmazás szabályokat kell, mi engedélyezése külső felhasználóknak vagy a szolgáltatások elérése az alkalmazásokat. Ebben a példában a 80-as port egyetlen webkiszolgálóra van, így egyetlen tűzfal alkalmazás szabály átirányítja bejövő webes kiszolgálók belső IP-címét a külső IP. A forgalmat munkamenet volna hálózati Címfordítást lenni a belső kiszolgáló.
      - A második alkalmazás forgalom szabály a vissza vége szabály lehetővé teszi az érintett webkiszolgálóra, ha társalogni szeretne AppVM01 server (de nem AppVM02) minden olyan portot keresztül.
- Belső szabályairól (belüli-VNet forgalom)
  4.    Internetes szabály kimenő: Ez a szabály lehetővé teszi olyan hálózat, a kijelölt hálózatok átadni érkező forgalmat. Ez a szabály az általában egy alapértelmezett szabályt a már a tűzfalat, de letiltott állapotba kerül. Ez a példa engedélyezni kell a szabályt.
  5.    DNS-szabály: Ez a szabály lehetővé teszi, hogy csak DNS (port 53) forgalom számára a DNS-kiszolgáló. A környezet a Kódmentes a Frontend a legtöbb forgalmat zárolva van ez a szabály kifejezetten lehetővé teszi, hogy DNS bármely helyi alhálózat.
  6.    Alhálózat az alhálózathoz szabály: Ez a szabály azt javasoljuk, hogy bármely kiszolgáló engedélyezése az vissza vége alhálózat az előtér-alhálózat (de nem a Fordított sorrend) bármely kiszolgálóhoz való csatlakozáshoz.
- Biztonságos szabály (forgalmához, amely nem felel meg a fenti bármelyikét):
  7.    Minden forgalom szabály tiltása: Ez mindig kell lennie, a végleges szabály (prioritás) számát tekintve, és ilyen a forgalmat Ha nem sikerül a megelőző, ez a szabály a program eltávolítja szabályok megfelelően. Ez az alapértelmezett szabály, és általában aktiválva, módosítás nélkül általában szükségesek.

>[AZURE.TIP] A második alkalmazás forgalom szabályt minden olyan portot engedélyezve van az ebben a példában valós használatával egyszerűen a legjobban megfelelő port, és ez a szabály homonimaszerű webcímmel felületének csökkentése címtartományok kell használni.

<br />

>[AZURE.IMPORTANT] A fenti szabályok létrehozása után fontos, hogy minden forgalom fog kell engedélyezni vagy tiltani tetszés szerint biztosításához szabály prioritásának áttekintése. Ebben a példában a szabályok elsőbbségi sorrendben vannak. Egyszerűen ki a tűzfalat hibásan rendezett szabályok következtében zárolva. Legalább győződjön meg róla magát a tűzfalat a kezelésének mindig a legnagyobb prioritású abszolút szabályt.

### <a name="rule-prerequisites"></a>A szabály vonatkozó követelmények
A virtuális gépen fut, a tűzfal egy előfeltétel nyilvános végpontokat is. A tűzfal feldolgozása forgalmat a megfelelő nyilvános végpontok meg kell nyitni. Ebben a példában; forgalom három típusba sorolhatók 1) management forgalom vezérlőt a tűzfal és tűzfalszabályokat, 2) RDP-forgalmat a windows-kiszolgálók, és a 3) alkalmazás forgalmat. Ezek a felső forgalom típusú három oszlopot a fenti tűzfalszabályokat logikai nézetének felén.

>[AZURE.IMPORTANT] A fő takeway, hogy ne feledje, hogy **minden** forgalom származnak a tűzfalon keresztül. Így távoli asztal IIS01-kiszolgálóhoz, akkor is, ha az első vége felhőalapú szolgáltatásba, és az előtér-alhálózat a kiszolgáló elérésére azt fogja kell a tűzfalat a port 8014 RDP, és engedje a tűzfalat, és a IIS01 RDP Port irányítja a belső RDP kérésére. Az Azure portál "Csatlakozás" gomb nem fognak működni, mert nincs közvetlen RDP elérési út IIS01 (amennyire a portálon látható). Ez azt jelenti, hogy az internetről származó minden kapcsolat ki lesz a biztonsági szolgáltatás, és a Port, például secscv001.cloudapp.net:xxxx (hivatkozás a fenti diagramot a külső és belső IP és portot megfeleltetése).

Zárólap megnyitható vagy a virtuális létrehozáskor, vagy írjon az összeállítás, végezhető el a mintaparancsfájl és a kódtöredék az alább látható módon (Megjegyzés; minden olyan elem kezdődő egy dollárjelet (pl.: $VMName[$i]) a felhasználó által definiált változó a forgatókönyv a dokumentum hivatkozás szakaszában. Szögletes zárójelek között, "$i" [$i], amely a VMs tömbje az egy adott virtuális tömb számát):

    Add-AzureEndpoint -Name "HTTP" -Protocol tcp -PublicPort 80 -LocalPort 80 `
        -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | `
        Update-AzureVM

Bár nem jól látható, de a végpontok változók használata miatt az alábbiakban **csak** nyílik meg a biztonsági felhőalapú szolgáltatást. Annak érdekében, hogy az összes bejövő forgalmat kezelése (továbbítás, hálózati címfordító Eszközig, csökkent) által a tűzfalon keresztül.

A felügyeleti ügyfél kell telepítve van a PC-n kezelheti a tűzfalat, és hozzon létre a szükséges beállításokat. Nézze meg a dokumentáció a tűzfal (vagy más NVA) szállító eszközök kezeléséhez. A következő szakaszban, a tűzfal szabályok létrehozását, és ez a szakasz hátralévő konfigurálását fog magát, a tűzfalat a szállítók management ügyfél (tehát nem a Azure portál vagy PowerShell) keresztül.

Itt leírt lépésekkel ügyfél letöltési és csatlakoztatása az ebben a példában a Barracuda megtalálható itt: [Barracuda NG rendszergazda](https://techlib.barracuda.com/NG61/NGAdmin)

Miután bejelentkezett, a tűzfal alakzatot, de tűzfalszabályokat létrehozása előtt, vannak-e fel, hogy könnyebben; a szabályokat hoz létre két kapcsolatban előzetesen szükséges objektum osztályok Hálózati és szolgáltatás objektumok.

Ebben a példában a három elnevezett hálózati objektumok definiált (egy a Frontend alhálózat és a Kódmentes alhálózat, a DNS-kiszolgáló IP-címét is egy hálózati objektum) kell lennie. Névvel ellátott hálózaton; létrehozása kezdve az ügyfél Barracuda NG rendszergazdai irányítópult, nyissa meg azt a beállítás lapon, a működési konfigurálása szakaszban kattintson a szabálykészletben, majd a tűzfal objektumok menüben válassza a "Hálózatok", majd hálózatok Szerkesztés menüjében kattintson az új gombra. A hálózati objektum most hozható létre, a név és az előtag hozzáadásával:

![Hálózati FrontEnd-objektum létrehozása][3]
 
Ezzel hoz létre elnevezett hálózat beállítása FrontEnd alhálózat, egy hasonló objektumot, valamint a Kódmentes alhálózat kell létrehozni. Most már az alhálózathoz könnyebben hivatkozhat a tűzfalszabályokat név szerint.

A DNS-kiszolgáló objektum:

![A DNS Server-objektum létrehozása][4]

A DNS-szabályokban a dokumentumon belül lesz használatban a egyetlen IP-cím hivatkozást.

A második kapcsolatban előzetesen szükséges objektumok Services objektumaihoz. Ezek a RDP kapcsolat portokat minden kiszolgáló jelöli. A meglévő RDP-objektum RDP alapértelmezett portja a kötött, mivel 3389, új szolgáltatások hozhat létre engedélyezése a külső portokat (8014-8026) érkező forgalmat. Az új portokat is hozzáadni a meglévő RDP-szolgáltatás, de a bemutató megkönnyítése érdekében az egyes szabályok minden kiszolgáló hozhatók létre. A kiszolgáló; új RDP szabály létrehozása kezdve az ügyfél Barracuda NG rendszergazdai irányítópult, nyissa meg azt a beállítás lapon, a működési konfigurálása szakaszban kattintson a szabálykészletben, a tűzfal objektumok menüben kattintson a "Szolgáltatások" elemre, görgessen lefelé a listában, a szolgáltatások és elemet, majd a "RDP" szolgáltatás. Kattintson a jobb gombbal, és válassza a másolás, majd kattintson a jobb gombbal, és válassza a beillesztés. Most már van egy RDP-Copy1 szolgáltatás objektum szerkeszthető. Kattintson a jobb gombbal a RDP-Copy1, és válassza a Szerkesztés menü ablak akár itt ismertetett módon jelenik szerkesztése szolgáltatás az objektum:

![Alapértelmezett RDP szabály másolata][5]

Az értékek majd szerkeszthető egy adott kiszolgáló RDP szolgáltatáshoz ábrázolásához. A AppVM01 a fenti alapértelmezett RDP szabályt módosítani kell egy új szolgáltatás neve, leírás és a ábra 8 diagramon használt külső RDP Port megfelelően (Megjegyzés: a portokat az adott kiszolgáló használt külső port a 3389 RDP alapértelmezettől módosítja, az AppVM01 esetében a külső portja 8025) alább látható a módosított szolgáltatás :

![AppVM01 szabály][6]
 
Ez a folyamat meg kell ismételni hozhat létre a hátralévő kiszolgálók; RDP-szolgáltatások AppVM02 DNS01 és IIS01. Az alábbi szolgáltatások kibocsátása láthatóvá válik a szabály létrehozása egyszerűbbé és további egyértelmű a következő szakaszban.

>[AZURE.NOTE] A tűzfal RDP szolgáltatásainak nem szükséges két okból; a tűzfal virtuális a alapú Linux képként, így SSH volna virtuális management RDP és 2) port 22 helyett használható port 22, és két management port engedélyezettek az adatkezelési kapcsolódási lehetővé teszi az itt leírt első management szabály 1) első.

### <a name="firewall-rules-creation"></a>Tűzfal szabályok létrehozása
Ebben a példában használt tűzfalszabályokat háromféle, az összes rendelkeznek különböző ikonok:

Az alkalmazás átirányítási szabály: ![Átirányítás Alkalmazásikon][7]

A cél hálózati Címfordítást szabály: ![Cél hálózati Címfordítást ikon][8]

A fázis szabály: ![Fázis ikon][9]

További információt a következő szabályokat alkalmazza a Barracuda webhelyen találhatók.

A következő szabályok létrehozása (vagy a meglévő alapértelmezett szabályok ellenőrzése), kezdve az ügyfél Barracuda NG rendszergazdai irányítópult lapon keresse meg a beállítás lapon, az üzemeltetési beállítás szakaszban kattintson a szabálykészletben. A rács nevű, "Fő szabályok" jelennek meg a meglévő aktív és inaktív szabályok e tűzfal. Ez a rács jobb felső sarkában lévő kis zöld van "+" gomb, ide kattintva hozzon létre egy új szabályt (Megjegyzés: a tűzfal "zárolva" módosítások, ha egy megjelölt "Zárolása" gomb látható, és nem tudja létrehozása vagy szerkesztése a szabályok, "zárolásának feloldásához" a szabálykészlete és szerkesztésük engedélyezése erre a gombra). Ha szeretne egy meglévő szabály szerkesztése, jelölje ki, hogy a szabályt, kattintson a jobb gombbal, és jelölje be a szabály szerkesztése.

Miután szabályokat is létrehozott, illetve módosítani kell tolni a tűzfal és kell majd aktiválva, ha ez nem történik meg a szabály módosítások életbe. A leküldéses és aktiválási folyamat leírása alább a részletek szabály leírása.

Részletei határozzák meg ez a példa befejezéséhez szükséges szabályok, az alábbiak szerint:

- **Adatkezelési szabály**: Ez alkalmazás átirányítás szabály lehetővé teszi, hogy a forgalmat a hálózati virtuális készülék, ebben a példában az adatkezelési portokat Barracuda NextGen tűzfal átadni. Az adatkezelési 801, a 807 és tetszőlegesen 22 legyen. A belső és külső portokat megegyeznek (tehát nem port fordítási). A szabályok kezelése-hozzáférési beállítása, egy alapértelmezett szabály, és alapértelmezés szerint engedélyezve (a Barracuda NextGen tűzfal 6.1-es verzió).

    ![Adatkezelési szabály][10]

>[AZURE.TIP] Az adatforrás címterület a szabály érték a bármelyik, a kezelés IP-címtartományok ismertek, ha a hatókör csökkentése volna is csökkentheti az adatkezelési portokat homonimaszerű webcímmel felület.

- **RDP szabályok**: A cél hálózati Címfordítást szabályok lehetővé teszi az egyes kiszolgálók keresztül RDP kezelésének.
Vannak Ez a szabály létrehozásához szükséges négy kritikus mezők:
  1.    Forrás – bármilyen RDP bárhonnan, a hivatkozás "" a forrás mezőben szerepel.
  2.    Szolgáltatás – használja a korábban létrehozott, ebben az esetben "AppVM01 RDP" megfelelő szolgáltatás-objektumra, és a külső portokat átirányítás kiszolgálók helyi IP-címét, és a porttal 3386 (az alapértelmezett RDP port). Ez a szabály van AppVM01 RDP eléréséhez.
  3.    Statikus IP-címei használata cél – "a *tűzfal helyi portjához*, DCHP 1 helyi IP-" vagy eth0 kell lennie. Lehet, hogy más, ha a hálózati készülék több helyi felületek a sorszámok (eth0, eth1 stb.). A tűzfal küldi ki a portja (is lehet ugyanaz, mint a fogadó port), a tényleges útválasztásos célja a Céllista mezőben.
  4.    Átirányítás – Ez a szakasz alapján a virtuális készülék találja végül irányítja a forgalmat. A legegyszerűbb átirányítása az, hogy az IP- és a Port (nem kötelező) a Céllista mezőben. Ha nincs portot használja a célport a bejövő kérésre lesz (ie nincs fordítási), használja a abban az esetben, ha olyan portot van kijelölve a port hálózati Címfordítást volna együtt a IP cím is lesznek.

    ![Tűzfal RDP szabály][11]

    Négy RDP szabályok összesen kell hozható létre: 

  	|   A szabály neve    | Kiszolgáló  |   Szolgáltatás   |  Céllista  |
  	|----------------|---------|-------------|---------------|
  	| RDP-IIS01   | IIS01   | IIS01 RDP   | 10.0.1.4:3389 |
  	| RDP-DNS01   | DNS01   | DNS01 RDP   | 10.0.2.4:3389 |
  	| RDP-AppVM01 | AppVM01 | AppVM01 RDP | 10.0.2.5:3389 |
  	| RDP-AppVM02 | AppVM02 | AppVm02 RDP | 10.0.2.6:3389 |
  
>[AZURE.TIP] A forrás- és mezők hatókörének szűkíteni csökkenti a támadások felület. A legtöbb korlátozni hatókör, amely lehetővé teszi, hogy a használható funkciók körét kell használni.

- **Alkalmazás forgalom szabályokat**: vannak a két alkalmazás forgalom szabályokat, az előtér-webes forgalom az első és hátsó vége forgalmához (pl. webkiszolgálót adatok réteg) a második. Szabályok fog függenek, hogy a hálózat architektúrája (ahol a kiszolgálók kerülnek) adatforgalom és flow (irányát a forgalom, és amelyek portok használt).

    Először ismertetjük, az internetes forgalmat a előtér szabály:

    ![Webes szabály][12]
 
    A cél hálózati Címfordítást szabály lehetővé teszi, hogy a tényleges alkalmazás forgalom az application server eléréséhez. Miközben a többi szabályokat a biztonsági, management stb engedélyezéséhez alkalmazás szabályokat kell, mi engedélyezése külső felhasználóknak vagy a szolgáltatások elérése az alkalmazásokat. Ebben a példában a 80-as port egyetlen webkiszolgálóra van, így az egyetlen tűzfal alkalmazás szabály átirányítja bejövő webes kiszolgálók belső IP-címét a külső IP.

    **Megjegyzés**:, hogy nincs a Céllista mezőben rendelt port, így a bejövő port 80 (vagy a kijelölt szolgáltatás 443-as) fog szerepelni az érintett webkiszolgálóra átirányítását. Ha egy másik port figyel az érintett webkiszolgálóra, például port 8080, akkor a céllistában mező sikerült frissíteni szeretné engedélyezni, valamint a Port átirányításhoz 10.0.1.4:8080.

    A következő alkalmazás forgalom szabály a hátsó vége szabály lehetővé teszi az érintett webkiszolgálóra, ha társalogni szeretne AppVM01 server (de nem AppVM02) bármely szolgáltatáson keresztül:

    ![Tűzfal AppVM01 szabály][13]

    A fázis szabály lehetővé teszi, hogy a bármely IIS-kiszolgálót az Frontend alhálózat elérje a AppVM01 (IP-cím 10.0.2.5) minden olyan portot, protokollal bármely szükség szerint a webalkalmazás az access-adatokhoz.

    A képernyőkép az egy "\<explicit cél\>" használják a célmező célként 10.0.2.5 időtartamon. Ez lehet vagy közvetlen látható módon, illetve nem nevű hálózati objektum (mint a DNS-kiszolgáló előfeltételei végzett). Ez a felfelé a rendszergazda, a tűzfal arról, hogy melyik módszert szolgálnak. Egy Explict Desitnation 10.0.2.5 létrehozni, kattintson duplán az első üres sorban a \<explicit cél\> és az előugró ablakban írja be a címet.

    A fázis szabály, és nincs hálózati Címfordítást van szükség, mivel ez belső forgalom, így a csatlakozási mód beállítható, hogy a "Nincs SNAT".

    **Megjegyzés**: Ez a szabály az adatforrás hálózat bármely erőforrás az FrontEnd alhálózat csak lesz egy vagy ismert adott számú webkiszolgálón, ha hálózati objektum erőforrás lehetett létrehozni e pontos IP-címek helyett a teljes Frontend alhálózat részletesebb.

>[AZURE.TIP] Ez a szabály használja a szolgáltatás "Minden" könnyebb a minta alkalmazás beállítása és használata, ezzel is lehetővé teszi ICMPv4 (ping) az egyetlen szabályt. Azonban ez nem ajánlott. A használható portok és protokollok ("szolgáltatások") kell maradt minimálisra, amely lehetővé teszi az alkalmazás a művelet a homonimaszerű webcímmel felület csökkentheti a oszlopazonosító között.

<br />

>[AZURE.TIP] Bár ez a szabály azt mutatja, hogy egy explicit cél hivatkozást használják, a tűzfal beállításainak egész egy következetes megközelítés kell használni. Javasoljuk, hogy az elnevezett hálózati objektum felhasználható a könnyebb olvashatóság érdekében és a támogatási lehetőségek. A közvetlen cél itt csak szolgál egy másik hivatkozás módszer megjelenítése, és általában nem ajánlott (különösen az összetett konfigurációk).

- **Internetes szabály kimenő**: A fázis szabály lehetővé teszi a forgalmat a minden forrás hálózat átadni a kijelölt cél hálózatokhoz. Ez a szabály egy alapértelmezett szabály már általában a Barracuda NextGen tűzfal, de a kikapcsolt állapotban van. Kattintson a jobb gombbal a szabályt a érheti el a aktiválása szabály parancsot. Az itt ismertetett szabály hozzáadása hivatkozásként kapcsolatban előzetesen szükséges részében ezt a dokumentumot a két helyi alhálózat készített Ez a szabály forrás attribútumának módosították.

    ![Kimenő szabály][14]

- **DNS-szabály**: ezt a sikeres szabály lehetővé teszi, hogy csak DNS (port 53) forgalom számára a DNS-kiszolgáló. A környezet a Kódmentes a FrontEnd a legtöbb forgalmat zárolva van ez a szabály kifejezetten lehetővé teszi, hogy DNS.

    ![Tűzfal DNS szabály][15]

    **Megjegyzés**: Ez a képernyő tartalma a csatlakozási mód szerepel. Mivel ez a szabály belső IP-cím forgalmat a belső IP-hez, a nincs NATing szükség, ez a csatlakozási mód értéke "Nincs SNAT" fázis szabály.

- **Alhálózat az alhálózathoz szabály**: A fázis szabály egy alapértelmezett szabály aktiválta és módosítani kell, hogy bármely kiszolgáló az vissza vége alhálózat az előtér-alhálózat bármely kiszolgálóhoz való csatlakozáshoz. Ez a szabály is a belső minden forgalom, ezért a csatlakozási mód nem SNAT beállíthatók.

    ![Tűzfal belüli-VNet szabály][16]

    **Megjegyzés**: A kétirányú jelölőnégyzet nincs bejelölve (és nem találta a legtöbb szabályok beadás), a fontos szabály annak, hogy a "több irányított" szabály azt ellenőrzi, kapcsolatot a hátsó vége alhálózat olyan indítható az előtér-hálózatot, de nem a Fordított sorrend. Ha a fiókbeállításokban bejelölte a ezt a jelölőnégyzetet, ez a szabály lehetővé tenné kétirányú a forgalmat, aminek a logikai diagramból nemkívánatosnak tartott adatközpontokban.

- **Minden forgalom szabály tiltása**: a végleges szabály (prioritás) számát tekintve mindig meg kell lennie, és ilyen a forgalmat nem felelnek meg, ha az előző szabályok a szabály által a program eltávolítja. Ez az alapértelmezett szabály, és általában aktiválva, módosítás nélkül általában szükségesek. 

    ![Tűzfal szabály elutasítása][17]

>[AZURE.IMPORTANT] A fenti szabályok létrehozása után fontos, hogy minden forgalom fog kell engedélyezni vagy tiltani tetszés szerint biztosításához szabály prioritásának áttekintése. Ebben a példában az szabályokat kell abban a sorrendben kell jelenniük a fő rács a továbbítási szabályokban Barracuda Management ügyfélprogramban.

## <a name="rule-activation"></a>A szabály aktiválása
A szabálykészletet módosíthatók úgy, hogy a rajz meghatározását, az a szabálykészletet legyen töltenek fel a tűzfalat, és majd aktiválva van.

![Tűzfal szabály aktiválása][18]
 
Az adatkezelési ügyfél jobb felső sarkában lévő gombok fürt vannak. "Küldés módosítások" gombra kattintva küldje el a módosított szabályokat a tűzfalat, majd kattintson a "Aktiválás" gombra.
 
Az aktiválás, a tűzfal szabálykészletet Ez például környezet összeállítás befejeződött.

## <a name="traffic-scenarios"></a>Adatforgalom felhasználási területei
>[AZURE.IMPORTANT] A fő takeway, hogy ne feledje, hogy **minden** forgalom származnak a tűzfalon keresztül. Így távoli asztal IIS01-kiszolgálóhoz, akkor is, ha az első vége felhőalapú szolgáltatásba, és az előtér-alhálózat a kiszolgáló elérésére azt fogja kell a tűzfalat a port 8014 RDP, és engedje a tűzfalat, és a IIS01 RDP Port irányítja a belső RDP kérésére. Az Azure portál "Csatlakozás" gomb nem fognak működni, mert nincs közvetlen RDP elérési út IIS01 (amennyire a portálon látható). Ez azt jelenti, hogy az internetről származó minden kapcsolat a biztonsági szolgáltatás, és olyan portot, például secscv001.cloudapp.net:xxxx lesz.

Ez a helyzet a következő tűzfalszabályokat helyen kell:

1.  Tűzfal kezelése
2.  A IIS01 RDP
3.  A DNS01 RDP
4.  A AppVM01 RDP
5.  A AppVM02 RDP
6.  A Web App forgalom
7.  Alkalmazás-alapú forgalmat AppVM01
8.  Kimenő az internethez
9.  A DNS01 Frontend
10. Belüli-alhálózat forgalom (csak az első végére vissza vége)
11. Az összes elutasítása

A tényleges tűzfal szabálykészletet valószínűleg lesz ezek sok egyéb szabályokat, tetszőleges adott tűzfalat szabályok is különböző prioritási számok, mint az itt szereplő felhasználókat. A lista- és kapcsolódó számok és adja meg a relevancia e 11 szabályok és az egyik ablaktábláról a másikra őket a relatív prioritás között. Más szóval; a tényleges tűzfalon a "RDP való IIS01" Előfordulhat, hogy 5-ös szám szabályt, de mindaddig, amíg a "Tűzfal Management" szabály alatt és fölött a "RDP DNS01" szabály azzal a lista lenne igazítása. A lista is fog megkönnyítése a alatti esetek, amely lehetővé teszi a fentebb; például "Átirányító szabály 9 (DNS)". Is fentebb, a négy RDP szabályokat együttesen neve lesz, "RDP szabályok" RDP független a forgalom forgatókönyv esetén.

Is visszahívása, hogy a hálózati biztonsági csoportok helyi legyenek a bejövő internetes forgalmat a Frontend és Kódmentes alhálózat.

#### <a name="allowed-internet-to-web-server"></a>(Megengedett) Internetes webkiszolgálón
1.  Internetes felhasználói kérések HTTP lap a SecSvc001.CloudApp.Net (internetes szemben lévő felhőalapú szolgáltatás)
2.  Felhőalapú szolgáltatás fázisai forgalom az portjához 80 tűzfal felület 10.0.0.4:80 megnyitott végponton
3.  Nincs NSG biztonsági alhálózat rendelve, így rendszer NSG szabályok – a tűzfal forgalmának engedélyezésére
4.  Adatforgalom találatok belső IP-címét, a tűzfal (10.0.1.4)
5.  Tűzfal megkezdi a szabályok feldolgozása:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabályok 2 – 5 (RDP szabályok) nem vonatkoznak, Ugrás a következő szabályok
  3.    Átirányító szabály 6 (alkalmazás: webes) alkalmazásához forgalom engedélyezve, tűzfal címfordító, hogy 10.0.1.4 (IIS01)
6.  A Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem érvényes (ezt a forgalmat a hálózati Címfordítást volt a tűzfal használna, így a forrás címét ettől kezdve a tűzfalat, amely a biztonsági alhálózat és a Frontend alhálózat NSG látják "helyi" forgalom lesz, és így engedélyezett), Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
7.  IIS01 figyel az internetes forgalmat, megkapja a kérelmet, és elindítja a kérelem feldolgozása
8.  IIS01 próbál kezdeményez egy FTP foglalkozáson, ahol AppVM01 Kódmentes alhálózat
9.  Az alhálózathoz Frontend UDR útvonalon lehetővé teszi a tűzfalat a következő ugrás
10. Nincs kimenő szabályok Frontend alhálózat forgalom engedélyezve
11. Tűzfal megkezdi a szabályok feldolgozása:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabály 2 – 5 (RDP szabályok) nem alkalmazható, Ugrás a következő szabályok
  3.    Átirányító szabály 6 (alkalmazás: webes) nem alkalmazható, Ugrás a következő szabályok
  4.    Átirányító szabály 7 (alkalmazás: Kódmentes) alkalmazásához forgalom engedélyezett, a tűzfal továbbítja forgalmat 10.0.2.5 (AppVM01)
12. A Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem vonatkoznak, Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
13.  AppVM01 megkapja a kérelmet, és a munkamenet indítása és reagál
14. Az alhálózathoz Kódmentes UDR útvonalon lehetővé teszi a tűzfalat a következő ugrás
15. Mivel nincsenek olyan engedélyezve van-e a Kódmentes a kérdésre adott választ alhálózat nincs kimenő NSG szabályok
16. Mivel ez ad eredményül forgalom egy elfogadott munkamenetben a tűzfal továbbítja a választ az érintett webkiszolgálóra (IIS01)
17. Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem vonatkoznak, Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
18. Az IIS-kiszolgáló kap, a válasz, a tranzakció AppVM01, és végrehajtja a felépíteni a HTTP-válasz, a HTTP-válasz a rendszer elküldi a lekérdező
19. Mivel nincsenek kimenő NSG szabályok az engedélyezett, a válasz Frontend alhálózat
20. A HTTP-válasz találatok a tűzfalat, és mivel ezt a választ egy elfogadott hálózati Címfordítást munkamenet elfogadta a tűzfalon keresztül
21. A tűzfal majd irányítja át a választ az internetes felhasználó
22. Mivel nincsenek olyan nem kimenő NSG szabályok vagy UDR Ugrás az Frontend alhálózat, a válasz engedélyezve van, és az internetes felhasználó megkapja az weblapon kért.

#### <a name="allowed-internet-rdp-to-backend"></a>(Megengedett) Az Internet a Kódmentes RDP
1.  Internetes kiszolgáló rendszergazdája kér RDP-munkamenetet AppVM01 SecSvc001.CloudApp.Net:8025, hol 8025-e a felhasználóhoz rendelt portszámot, a "RDP AppVM01" tűzfal szabály keresztül
2.  A felhőbeli szolgáltatástól továbbítja a megnyitott végpont áthaladó port 8025 10.0.0.4:8025 tűzfal felület
3.  Nincs NSG biztonsági alhálózat rendelve, így rendszer NSG szabályok – a tűzfal forgalmának engedélyezésére
4.  Tűzfal megkezdi a szabályok feldolgozása:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabály 2 (RDP IIS) nem vonatkoznak, Ugrás a következő szabályok
  3.    Átirányító szabály 3 (RDP DNS01) nem vonatkoznak, Ugrás a következő szabályok
  4.    Átirányító szabály 4 (RDP AppVM01) alkalmazása, a forgalom engedélyezett, tűzfal címfordító, hogy 10.0.2.5:3386 (RDP-portjához AppVM01)
5.  A Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem érvényes (ezt a forgalmat a hálózati Címfordítást volt a tűzfal használna, így a forrás címét ettől kezdve a tűzfalat, amely a biztonsági alhálózat és a Kódmentes alhálózat NSG látják "helyi" forgalom lesz, és így engedélyezett), Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
6.  AppVM01 RDP-forgalmat a figyeli és válaszol
7.  Nincs kimenő NSG szabályok alapértelmezett szabályok lépnek érvénybe, és visszatérő forgalom engedélyezett
8.  UDR a forgalom kimenő a tűzfal, a következő ugrás
9.  Mivel ez ad eredményül forgalom egy elfogadott munkamenetben a tűzfal továbbítja a választ az internetes felhasználó
10. RDP-munkamenet engedélyezése
11. AppVM01 kér, felhasználónév jelszó

#### <a name="allowed-web-server-dns-lookup-on-dns-server"></a>(Megengedett) Webes Server DNS keresési DNS-kiszolgálón
1.  Webes kiszolgálón, IIS01, egy adatcsatorna www.data.gov az igényeinek, de a cím feloldásához igényeinek.
2.  A hálózati konfigurálásról a VNet listák DNS01 (az Kódmentes alhálózat 10.0.2.4), az elsődleges DNS-kiszolgálót, IIS01 küld a DNS-kérés DNS01
3.  UDR a forgalom kimenő a tűzfal, a következő ugrás
4.  Nincs kimenő NSG szabályok Frontend az alhálózathoz kötött, forgalom engedélyezve
5.  Tűzfal megkezdi a szabályok feldolgozása:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabály 2 – 5 (RDP szabályok) nem alkalmazható, Ugrás a következő szabályok
  3.    Átirányító szabályok 6 és 7 (alkalmazás szabályok) nem vonatkoznak, Ugrás a következő szabályok
  4.    Átirányító szabály 8 (az Internet) nem vonatkoznak, Ugrás a következő szabályok
  5.    Átirányító szabály 9 (DNS) alkalmazása, a forgalom engedélyezve van, a tűzfal továbbítja forgalmat 10.0.2.4 (DNS01)
6.  A Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem vonatkoznak, Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
7.  DNS-kiszolgáló megkapja a kérelmet
8.  DNS-kiszolgáló nem tartozik a gyorsítótárazott címet és a program rákérdez a legfelső szintű DNS-kiszolgálót az interneten
9.  UDR a forgalom kimenő a tűzfal, a következő ugrás
10. Nincs kimenő NSG szabályok Kódmentes alhálózat, forgalom engedélyezve
11. Tűzfal megkezdi a szabályok feldolgozása:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabály 2 – 5 (RDP szabályok) nem alkalmazható, Ugrás a következő szabályok
  3.    Átirányító szabályok 6 és 7 (alkalmazás szabályok) nem vonatkoznak, Ugrás a következő szabályok
  4.     Átirányító szabály 8 (az interneten) alkalmazása, a forgalom engedélyezve van, a munkamenet SNAT ki a legfelső szintű DNS-kiszolgálóhoz, az interneten
12. Az Internet a DNS-kiszolgáló válaszol, mivel ebben a munkamenetben a tűzfalat a kezdeményezett, a válasz elfogadta a tűzfalon keresztül
13. Mivel ez egy elfogadott munkamenet, a tűzfal továbbítja a válasz, a kezdeményező kiszolgálóra DNS01
14. A Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem vonatkoznak, Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
15. A DNS-kiszolgáló kap és a válasz gyorsítótárát, és kattintson az eredeti értekezlet-összehívást vissza IIS01 válaszol
16. Az alhálózathoz kódmentes UDR útvonalon lehetővé teszi a tűzfalat a következő ugrás
17. Nincs kimenő NSG szabályok az Kódmentes alhálózat létezik, forgalom engedélyezve
18. Ez egy elfogadott munkamenet a tűzfalon, a válasz továbbítja a tűzfal az IIS-kiszolgálón
19. Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    Nincs bejövő alkalmazott NSG szabály Frontend az alhálózathoz, így a NSG egyik szabály alkalmazása a Kódmentes alhálózat érkező forgalmat
  2.    Az alapértelmezett rendszer szabály lehetővé teszi a forgalom alhálózat közötti lehetővé teszi a forgalom, hogy engedélyezi-e a forgalmat
20. IIS01 a kérdésre adott választ kap a DNS01

#### <a name="allowed-backend-server-to-frontend-server"></a>(Megengedett) Kódmentes-kiszolgálóról Frontend kiszolgálóra
1.  A rendszergazda AppVM02 RDP keresztül bejelentkezett kér fájlt közvetlenül a kiszolgálóról IIS01 windows Fájlkezelőben keresztül
2.  Az alhálózathoz Kódmentes UDR útvonalon lehetővé teszi a tűzfalat a következő ugrás
3.  Mivel nincsenek olyan engedélyezve van-e a Kódmentes a kérdésre adott választ alhálózat nincs kimenő NSG szabályok
4.  Tűzfal megkezdi a szabályok feldolgozása:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabály 2 – 5 (RDP szabályok) nem alkalmazható, Ugrás a következő szabályok
  3.    Átirányító szabályok 6 és 7 (alkalmazás szabályok) nem vonatkoznak, Ugrás a következő szabályok
  4.    Átirányító szabály 8 (az Internet) nem vonatkoznak, Ugrás a következő szabályok
  5.    Átirányító szabály 9 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  6.    Átirányító szabály 10 (belüli-alhálózat) alkalmazása, a forgalom engedélyezve van, a tűzfal átadja a forgalom 10.0.1.4 (IIS01)
5.  Frontend alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem vonatkoznak, Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
6.  Feltételezve, hogy a megfelelő hitelesítési és engedélyezési, IIS01 elfogadja a kérést, és megválaszolja
7.  Az alhálózathoz Frontend UDR útvonalon lehetővé teszi a tűzfalat a következő ugrás
8.  Mivel nincsenek kimenő NSG szabályok az engedélyezett, a válasz Frontend alhálózat
9.  Mivel ez egy meglévő munkamenethez a tűzfalon engedélyezett, a válasz, és a tűzfalat AppVM02 a választ adja eredményül.
10. Kódmentes alhálózat megkezdi a bejövő szabályok feldolgozása:
  1.    NSG szabály 1 (Internet blokk) nem vonatkoznak, Ugrás a következő szabályok
  2.    Alapértelmezett NSG szabályok alhálózat az alhálózathoz forgalmának engedélyezésére, forgalom engedélyezett, NSG szabály feldolgozás leállítása
11. AppVM02 kap, a válasz

#### <a name="denied-internet-direct-to-web-server"></a>(Megtagadva) Az Internet közvetlen webkiszolgálón
1.  Internetes felhasználó megkísérel IIS01, a webkiszolgáló hozzáférni a FrontEnd001.CloudApp.Net szolgáltatáson keresztül
2.  Mivel nincs olyan nyissa meg a HTTP-forgalmat, ez nem kellene haladnia a felhőalapú szolgáltatást, és nem jutnak el a kiszolgálóra
3.  Ha valamilyen okból a végpontok megnyitott, a NSG (Internet blokk) az Frontend alhálózat megakadályozza a forgalom
4.  Végül a Frontend alhálózat UDR útvonal volna küldhessenek az minden kimenő forgalmának IIS01, a következő ugrás a tűzfal, és a tűzfalat, akinek aszimmetrikus adatforgalom jelenik meg és húzza a kimenő választ, így az alábbi legalább három független réteg védekezés az internetes és IIS01 között a felhőbeli szolgáltatástól letiltom a nem engedélyezett és a nem megfelelő hozzáférést keresztül.

#### <a name="denied-internet-to-backend-server"></a>(Megtagadva) Internetes Kódmentes-kiszolgálóra
1.  Internetes felhasználó megpróbál egy fájlt a AppVM01 hozzáférni a BackEnd001.CloudApp.Net szolgáltatáson keresztül
2.  Vannak nyitva fájlmegosztási végpontok, mivel ez nem kellene át a felhőalapú szolgáltatást, és nem jutnak el a kiszolgálóra
3.  Ha valamilyen okból a végpontok megnyitott, a NSG (Internet blokk) megakadályozza a forgalom
4.  Végül a UDR útvonal volna küldhessenek az minden kimenő forgalmának AppVM01, a következő ugrás a tűzfal, és a tűzfalat, akinek aszimmetrikus adatforgalom jelenik meg és húzza a kimenő választ, így az alábbi legalább három független réteg védekezés az internetes és AppVM01 között a felhőbeli szolgáltatástól letiltom a nem engedélyezett és a nem megfelelő hozzáférést keresztül.

#### <a name="denied-frontend-server-to-backend-server"></a>(Megtagadva) Frontend-kiszolgálóról Kódmentes kiszolgálóra
1.  Tegyük fel, IIS01 jutott, és próbálja szeretne képet beolvasni a Kódmentes alhálózat kiszolgálók kártékony kódok fut.
2.  A Frontend alhálózat UDR útvonal volna küldhessenek az minden kimenő forgalmának IIS01 a tűzfal, a következő ugrás. Ez a nem, amit a biztonságos virtuális is lehet megváltoztatni.
3.  A tűzfal volna feldolgozását a forgalmat, ha a kérés AppVM01, vagy a DNS-kiszolgálóhoz, a DNS-kereséseket, amelyek a forgalom esetleg meghatároz a tűzfal (miatt Átirányító szabályok 7 és 9). Minden más forgalom volna blokkolja Átirányító szabály 11 (az összes elutasítása).
4.  Ha speciális veszélyt észlelési engedélyezték a tűzfal (amelyek nem szerepelnek a dokumentumban, olvassa el az adott hálózati készülék speciális veszélyt lehetőségek szállító dokumentációját) szeretné tenni a dokumentum tárgyalt egyszerű továbbítási szabályok páros forgalmakról kell miatt nem, ha a forgalom ismert aláírások vagy mintákat, amelyek egy speciális veszélyt szabály jelző.

#### <a name="denied-internet-dns-lookup-on-dns-server"></a>(Megtagadva) Az Internet a DNS-keresés DNS-kiszolgálón
1.  Az Internet-felhasználó megpróbálja megkeresni egy belső DNS-rekordot a DNS01 BackEnd001.CloudApp.Net szolgáltatáson keresztül 
2.  Mivel nincs olyan nyissa meg a DNS-forgalmat, ez nem kellene haladnia a felhőalapú szolgáltatást, és nem jutnak el a kiszolgálóra
3.  Ha valamilyen okból a végpontok megnyitása, az Frontend alhálózat (Internet blokk) NSG szabály megakadályozza a forgalmat
4.  Végül a Kódmentes alhálózat UDR útvonal volna küldhessenek az minden kimenő forgalmának DNS01, a következő ugrás a tűzfal, és a tűzfalat, akinek aszimmetrikus adatforgalom jelenik meg és húzza a kimenő választ, így az alábbi legalább három független réteg védekezés az internetes és DNS01 között a felhőbeli szolgáltatástól letiltom a nem engedélyezett és a nem megfelelő hozzáférést keresztül.

#### <a name="denied-internet-to-sql-access-through-firewall"></a>(Megtagadva) Internetes SQL Access tűzfalon keresztül
1.  Internetes felhasználó SQL-adatokat kér SecSvc001.CloudApp.Net (internetes szemben lévő felhőalapú szolgáltatás)
2.  Mivel nincsenek végpontok olyan nyissa meg az SQL, ez nem kellene át a felhőalapú szolgáltatást, és nem éri a tűzfalon keresztül
3.  Ha valamilyen okból SQL végpontok nyitva, a tűzfal a szabályok feldolgozása szeretné megkezdése:
  1.    Átirányító szabály 1 (Átirányító kezelése) nem vonatkoznak, Ugrás a következő szabályok
  2.    Átirányító szabályok 2 – 5 (RDP szabályok) nem vonatkoznak, Ugrás a következő szabályok
  3.    Átirányító szabály 6 és 7 (alkalmazás szabályok) nem vonatkoznak, Ugrás a következő szabályok
  4.    Átirányító szabály 8 (az Internet) nem vonatkoznak, Ugrás a következő szabályok
  5.    Átirányító szabály 9 (DNS) nem vonatkoznak, Ugrás a következő szabályok
  6.    Átirányító szabály 10 (belüli-alhálózat) nem vonatkoznak, Ugrás a következő szabályok
  7.    Átirányító szabály 11 (az összes elutasítása) alkalmazhat, forgalom letiltott, leállítása a szabály feldolgozása


## <a name="references"></a>Hivatkozások
### <a name="main-script-and-network-config"></a>Fő parancsfájl, és a hálózati beállítások
Mentse a teljes parancsfájl PowerShell parancsprogramot fájlban. Mentse a hálózati Config "NetworkConf2.xml" nevű fájlt.
Szükség szerint módosítsa a felhasználó által definiált változók. Futtassa a, majd hajtsa végre a tűzfal szabály beállítási útmutatás a fenti.

#### <a name="full-script"></a>Teljes parancsfájl
Ez a parancsfájl lesz, a felhasználó által definiált változók alapján:

1.  Csatlakozás az Azure előfizetéssel
2.  Tárterület új fiók létrehozása
3.  Hozzon létre egy új VNet és a hálózati konfigurációs fájl meghatározott három alhálózathoz
4.  Öt virtuális gépeken futó; összeállítása 1 tűzfal és a 4 windows server VMs
5.  Állítsa be a UDR együtt:
  1.    Két új útvonal tábla létrehozása
  2.    A táblák útvonalak hozzáadása
  3.    Kötést létrehozni a táblázatok a megfelelő alhálózat
6.  Kattintson a NVA IP-továbbítás engedélyezése
7.  Állítsa be a NSG együtt:
  1.    Egy NSG létrehozása
  2.    Szabály hozzáadása
  3.    A megfelelő alhálózat a NSG kötése

A PowerShell-parancsprogramot kell futtatni a helyi meghajtóra internetes PC-re vagy a kiszolgáló csatlakozik.

>[AZURE.IMPORTANT] Ha ezt a parancsfájlt, lehetnek figyelmeztetések vagy más PowerShell videogaléria tájékoztató üzenetek. Piros csak hibaüzenetek aggodalomra.

    <# 
     .SYNOPSIS
      Example of DMZ and User Defined Routing in an isolated network (Azure only, no hybrid connections)
    
     .DESCRIPTION
      This script will build out a sample DMZ setup containing:
       - A default storage account for VM disks
       - Three new cloud services
       - Three Subnets (SecNet, FrontEnd, and BackEnd subnets)
       - A Network Virtual Appliance (NVA), in this case a Barracuda NextGen Firewall
       - One server on the FrontEnd Subnet
       - Three Servers on the BackEnd Subnet
       - IP Forwading from the FireWall out to the internet
       - User Defined Routing FrontEnd and BackEnd Subnets to the NVA
    
      Before running script, ensure the network configuration file is created in
      the directory referenced by $NetworkConfigFile variable (or update the
      variable to reflect the path and file name of the config file being used).
    
     .Notes
      Everyone's security requirements are different and can be addressed in a myriad of ways.
      Please be sure that any sensitive data or applications are behind the appropriate
      layer(s) of protection. This script serves as an example of some of the techniques
      that can be used, but should not be used for all scenarios. You are responsible to
      assess your security needs and the appropriate protections needed, and then effectively
      implement those protections.
    
      Security Service (SecNet subnet 10.0.0.0/24)
       myFirewall - 10.0.0.4
     
      FrontEnd Service (FrontEnd subnet 10.0.1.0/24)
       IIS01      - 10.0.1.4
     
      BackEnd Service (BackEnd subnet 10.0.2.0/24)
       DNS01      - 10.0.2.4
       AppVM01    - 10.0.2.5
       AppVM02    - 10.0.2.6
    
    #>
    
    # Fixed Variables
        $LocalAdminPwd = Read-Host -Prompt "Enter Local Admin Password to be used for all VMs"
        $VMName = @()
        $ServiceName = @()
        $VMFamily = @()
        $img = @()
        $size = @()
        $SubnetName = @()
        $VMIP = @()
    
    # User Defined Global Variables
      # These should be changes to reflect your subscription and services
      # Invalid options will fail in the validation section
    
      # Subscription Access Details
        $subID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    
      # VM Account, Location, and Storage Details
        $LocalAdmin = "theAdmin"
        $DeploymentLocation = "Central US"
        $StorageAccountName = "vmstore02"
    
      # Service Details
        $SecureService = "SecSvc001"
        $FrontEndService = "FrontEnd001"
        $BackEndService = "BackEnd001"
    
      # Network Details
        $VNetName = "CorpNetwork"
        $VNetPrefix = "10.0.0.0/16"
        $SecNet = "SecNet"
        $FESubnet = "FrontEnd"
        $FEPrefix = "10.0.1.0/24"
        $BESubnet = "BackEnd"
        $BEPrefix = "10.0.2.0/24"
        $NetworkConfigFile = "C:\Scripts\NetworkConf3.xml"
    
      # VM Base Disk Image Details
        $SrvImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Windows Server 2012 R2 Datacenter'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
        $FWImg = Get-AzureVMImage | Where {$_.ImageFamily -match 'Barracuda NextGen Firewall'} | sort PublishedDate -Descending | Select ImageName -First 1 | ForEach {$_.ImageName}
    
      # UDR Details
        $FERouteTableName = "FrontEndSubnetRouteTable"
        $BERouteTableName = "BackEndSubnetRouteTable"
    
      # NSG Details
        $NSGName = "MyVNetSG"
    
    # User Defined VM Specific Config
        # Note: To ensure UDR and IP forwarding is setup
        # properly this script requires VM 0 be the NVA.
    
        # VM 0 - The Network Virtual Appliance (NVA)
          $VMName += "myFirewall"
          $ServiceName += $SecureService
          $VMFamily += "Firewall"
          $img += $FWImg
          $size += "Small"
          $SubnetName += $SecNet
          $VMIP += "10.0.0.4"
    
        # VM 1 - The Web Server
          $VMName += "IIS01"
          $ServiceName += $FrontEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $FESubnet
          $VMIP += "10.0.1.4"
    
        # VM 2 - The First Appliaction Server
          $VMName += "AppVM01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.5"
    
        # VM 3 - The Second Appliaction Server
          $VMName += "AppVM02"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.6"
    
        # VM 4 - The DNS Server
          $VMName += "DNS01"
          $ServiceName += $BackEndService
          $VMFamily += "Windows"
          $img += $SrvImg
          $size += "Standard_D3"
          $SubnetName += $BESubnet
          $VMIP += "10.0.2.4"
    
    # ----------------------------- #
    # No User Defined Varibles or   #
    # Configuration past this point #
    # ----------------------------- #
    
      # Get your Azure accounts
        Add-AzureAccount
        Set-AzureSubscription –SubscriptionId $subID -ErrorAction Stop
        Select-AzureSubscription -SubscriptionId $subID -Current -ErrorAction Stop
    
      # Create Storage Account
        If (Test-AzureName -Storage -Name $StorageAccountName) { 
            Write-Host "Fatal Error: This storage account name is already in use, please pick a diffrent name." -ForegroundColor Red
            Return}
        Else {Write-Host "Creating Storage Account" -ForegroundColor Cyan 
              New-AzureStorageAccount -Location $DeploymentLocation -StorageAccountName $StorageAccountName}
    
      # Update Subscription Pointer to New Storage Account
        Write-Host "Updating Subscription Pointer to New Storage Account" -ForegroundColor Cyan 
        Set-AzureSubscription –SubscriptionId $subID -CurrentStorageAccountName $StorageAccountName -ErrorAction Stop
    
    # Validation
    $FatalError = $false
    
    If (-Not (Get-AzureLocation | Where {$_.DisplayName -eq $DeploymentLocation})) {
         Write-Host "This Azure Location was not found or available for use" -ForegroundColor Yellow
         $FatalError = $true}
    
    If (Test-AzureName -Service -Name $SecureService) { 
        Write-Host "The SecureService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use." -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $FrontEndService) { 
        Write-Host "The FrontEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The FrontEndService service name is valid for use" -ForegroundColor Green}
    
    If (Test-AzureName -Service -Name $BackEndService) { 
        Write-Host "The BackEndService service name is already in use, please pick a different service name." -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The BackEndService service name is valid for use." -ForegroundColor Green}
    
    If (-Not (Test-Path $NetworkConfigFile)) { 
        Write-Host 'The network config file was not found, please update the $NetworkConfigFile variable to point to the network config xml file.' -ForegroundColor Yellow
        $FatalError = $true}
    Else { Write-Host "The network config file was found" -ForegroundColor Green
            If (-Not (Select-String -Pattern $DeploymentLocation -Path $NetworkConfigFile)) {
                Write-Host 'The deployment location was not found in the network config file, please check the network config file to ensure the $DeploymentLocation varible is correct and the netowrk config file matches.' -ForegroundColor Yellow
                $FatalError = $true}
            Else { Write-Host "The deployment location was found in the network config file." -ForegroundColor Green}}
    
    If ($FatalError) {
        Write-Host "A fatal error has occured, please see the above messages for more information." -ForegroundColor Red
        Return}
    Else { Write-Host "Validation passed, now building the environment." -ForegroundColor Green}
    
    # Create VNET
        Write-Host "Creating VNET" -ForegroundColor Cyan 
        Set-AzureVNetConfig -ConfigurationPath $NetworkConfigFile -ErrorAction Stop
    
    # Create Services
        Write-Host "Creating Services" -ForegroundColor Cyan
        New-AzureService -Location $DeploymentLocation -ServiceName $SecureService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $FrontEndService -ErrorAction Stop
        New-AzureService -Location $DeploymentLocation -ServiceName $BackEndService -ErrorAction Stop
    
    # Build VMs
        $i=0
        $VMName | Foreach {
            Write-Host "Building $($VMName[$i])" -ForegroundColor Cyan
            If ($VMFamily[$i] -eq "Firewall") 
                { 
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Linux -LinuxUser $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                # Set up all the EndPoints we'll need once we're up and running
                # Note: All traffic goes through the firewall, so we'll need to set up all ports here.
                #       Also, the firewall will be redirecting traffic to a new IP and Port in a forwarding
                #       rule, so all of these endpoint have the same public and local port and the firewall
                #       will do the mapping, NATing, and/or redirection as declared in the firewall rules.
                Add-AzureEndpoint -Name "MgmtPort1" -Protocol tcp -PublicPort 801  -LocalPort 801  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "MgmtPort2" -Protocol tcp -PublicPort 807  -LocalPort 807  -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "HTTP"      -Protocol tcp -PublicPort 80   -LocalPort 80   -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPWeb"    -Protocol tcp -PublicPort 8014 -LocalPort 8014 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp1"   -Protocol tcp -PublicPort 8025 -LocalPort 8025 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPApp2"   -Protocol tcp -PublicPort 8026 -LocalPort 8026 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                Add-AzureEndpoint -Name "RDPDNS01"  -Protocol tcp -PublicPort 8024 -LocalPort 8024 -VM (Get-AzureVM -ServiceName $ServiceName[$i] -Name $VMName[$i]) | Update-AzureVM
                # Note: A SSH endpoint is automatically created on port 22 when the appliance is created.
                }
            Else
                {
                New-AzureVMConfig -Name $VMName[$i] -ImageName $img[$i] –InstanceSize $size[$i] | `
                    Add-AzureProvisioningConfig -Windows -AdminUsername $LocalAdmin -Password $LocalAdminPwd  | `
                    Set-AzureSubnet  –SubnetNames $SubnetName[$i] | `
                    Set-AzureStaticVNetIP -IPAddress $VMIP[$i] | `
                    Set-AzureVMMicrosoftAntimalwareExtension -AntimalwareConfiguration '{"AntimalwareEnabled" : true}' | `
                    Remove-AzureEndpoint -Name "RemoteDesktop" | `
                    Remove-AzureEndpoint -Name "PowerShell" | `
                    New-AzureVM –ServiceName $ServiceName[$i] -VNetName $VNetName -Location $DeploymentLocation
                }
            $i++
        }
    
    # Configure UDR and IP Forwarding
        Write-Host "Configuring UDR" -ForegroundColor Cyan
    
      # Create the Route Tables
        Write-Host "Creating the Route Tables" -ForegroundColor Cyan 
        New-AzureRouteTable -Name $BERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $BESubnet subnet"
        New-AzureRouteTable -Name $FERouteTableName `
            -Location $DeploymentLocation `
            -Label "Route table for $FESubnet subnet"
    
      # Add Routes to Route Tables
        Write-Host "Adding Routes to the Route Tables" -ForegroundColor Cyan 
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $BERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $BEPrefix `
            -NextHopType VNETLocal
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "All traffic to FW" -AddressPrefix 0.0.0.0/0 `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Internal traffic to FW" -AddressPrefix $VNetPrefix `
            -NextHopType VirtualAppliance `
            -NextHopIpAddress $VMIP[0]
        Get-AzureRouteTable $FERouteTableName `
            |Set-AzureRoute -RouteName "Allow Intra-Subnet Traffic" -AddressPrefix $FEPrefix `
            -NextHopType VNETLocal
    
      # Assoicate the Route Tables with the Subnets
        Write-Host "Binding Route Tables to the Subnets" -ForegroundColor Cyan 
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $BESubnet `
            -RouteTableName $BERouteTableName
        Set-AzureSubnetRouteTable -VirtualNetworkName $VNetName `
            -SubnetName $FESubnet `
            -RouteTableName $FERouteTableName
    
     # Enable IP Forwarding on the Virtual Appliance
        Get-AzureVM -Name $VMName[0] -ServiceName $ServiceName[0] `
            |Set-AzureIPForwarding -Enable
    
    # Configure NSG
        Write-Host "Configuring the Network Security Group (NSG)" -ForegroundColor Cyan
    
      # Build the NSG
        Write-Host "Building the NSG" -ForegroundColor Cyan
        New-AzureNetworkSecurityGroup -Name $NSGName -Location $DeploymentLocation -Label "Security group for $VNetName subnets in $DeploymentLocation"
    
      # Add NSG Rule
        Write-Host "Writing rules into the NSG" -ForegroundColor Cyan
        Get-AzureNetworkSecurityGroup -Name $NSGName | Set-AzureNetworkSecurityRule -Name "Isolate the $VNetName VNet from the Internet" -Type Inbound -Priority 100 -Action Deny `
            -SourceAddressPrefix INTERNET -SourcePortRange '*' `
            -DestinationAddressPrefix VIRTUAL_NETWORK -DestinationPortRange '*' `
            -Protocol *
    
      # Assign the NSG to two Subnets
        # The NSG is *not* bound to the Security Subnet. The result
        # is that internet traffic flows only to the Security subnet
        # since the NSG bound to the Frontend and Backback subnets
        # will Deny internet traffic to those subnets.
        Write-Host "Binding the NSG to two subnets" -ForegroundColor Cyan
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $FESubnet -VirtualNetworkName $VNetName
        Set-AzureNetworkSecurityGroupToSubnet -Name $NSGName -SubnetName $BESubnet -VirtualNetworkName $VNetName
    
    # Optional Post-script Manual Configuration
      # Configure Firewall
      # Install Test Web App (Run Post-Build Script on the IIS Server)
      # Install Backend resource (Run Post-Build Script on the AppVM01)
      Write-Host
      Write-Host "Build Complete!" -ForegroundColor Green
      Write-Host
      Write-Host "Optional Post-script Manual Configuration Steps" -ForegroundColor Gray
      Write-Host " - Configure Firewall" -ForegroundColor Gray
      Write-Host " - Install Test Web App (Run Post-Build Script on the IIS Server)" -ForegroundColor Gray
      Write-Host " - Install Backend resource (Run Post-Build Script on the AppVM01)" -ForegroundColor Gray
      Write-Host
    

#### <a name="network-config-file"></a>Hálózati konfigurációs fájl
Mentse az XML-fájl frissített helyét, és a hivatkozás elhelyezése a $NetworkConfigFile változó a fenti parancsfájl ezt a fájlt.

    <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <Dns>
          <DnsServers>
            <DnsServer name="DNS01" IPAddress="10.0.2.4" />
            <DnsServer name="Level3" IPAddress="209.244.0.3" />
          </DnsServers>
        </Dns>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="CorpNetwork" Location="Central US">
            <AddressSpace>
              <AddressPrefix>10.0.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="SecNet">
                <AddressPrefix>10.0.0.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="FrontEnd">
                <AddressPrefix>10.0.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>10.0.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
            <DnsServersRef>
              <DnsServerRef name="DNS01" />
              <DnsServerRef name="Level3" />
            </DnsServersRef>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>

#### <a name="sample-application-scripts"></a>Minta alkalmazás parancsfájlok
Ha szeretne egy minta alkalmazás telepítése a jelenlegi és más DMZ példákat, egyet az alábbi hivatkozásra kapott: [Alkalmazás mintaparancsfájl][SampleApp]

<!--Image References-->
[1]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3design.png "Kétirányú DMZ NVA, NSG és UDR"
[2]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/example3firewalllogical.png "A tűzfal szabályok logikai megtekintése"
[3]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectfrontend.png "Hálózati FrontEnd-objektum létrehozása"
[4]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectdns.png "A DNS Server-objektum létrehozása"
[5]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpa.png "Alapértelmezett RDP szabály másolata"
[6]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/createnetworkobjectrdpb.png "AppVM01 szabály"
[7]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconapplicationredirect.png "Az átirányítási Alkalmazásikon"
[8]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/icondestinationnat.png "Cél hálózati Címfordítást ikon"
[9]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/iconpass.png "A sikeres ikon"
[10]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulefirewall.png "Adatkezelési szabály"
[11]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/rulerdp.png "Tűzfal RDP szabály"
[12]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleweb.png "Webes szabály"
[13]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleappvm01.png "Tűzfal AppVM01 szabály"
[14]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleoutbound.png "Kimenő szabály"
[15]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledns.png "Tűzfal DNS szabály"
[16]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruleintravnet.png "Tűzfal belüli-VNet szabály"
[17]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/ruledeny.png "Tűzfal szabály elutasítása"
[18]: ./media/virtual-networks-dmz-nsg-fw-udr-asm/firewallruleactivate.png "Tűzfal szabály aktiválása"

<!--Link References-->
[HOME]: ../best-practices-network-security.md
[SampleApp]: ./virtual-networks-sample-app.md
