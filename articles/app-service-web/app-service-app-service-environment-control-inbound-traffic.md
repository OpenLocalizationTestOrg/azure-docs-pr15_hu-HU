<properties 
    pageTitle="Hogyan szabályozható a bejövő forgalom az alkalmazás-szolgáltatási környezetben" 
    description="További tudnivalók arról, hogy miként szabályozhatja az alkalmazás-szolgáltatási környezetben bejövő forgalmat a hálózati biztonsági szabályok konfigurálása." 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Hogyan szabályozható a bejövő forgalom az alkalmazás-szolgáltatási környezetben

## <a name="overview"></a>– Áttekintés ##
Az alkalmazás-szolgáltatási környezetben hozható létre a **vagy** az erőforrás-kezelő Azure virtuális hálózati **vagy** klasszikus környezetet alakítana modell [virtuális hálózati][virtualnetwork].  Kezdésének az alkalmazás-szolgáltatási környezetben hoz létre egy új virtuális hálózati és az új alhálózat definiálható.  Azt is megteheti az alkalmazás-szolgáltatási környezetben hozhat létre egy már meglévő virtuális hálózati és a már meglévő alhálózat.  A legutóbbi módosítás június 2016-ban készült ASEs most telepíthető virtuális használó hálózatok nyilvános címtartományok vagy RFC1918 cím szóközt (azaz be a magánjellegű címeket).  Az alkalmazás-szolgáltatási környezetben létrehozásával kapcsolatban további részleteket lásd [létrehozása az alkalmazás-szolgáltatási környezetben][HowToCreateAnAppServiceEnvironment].

Az alkalmazás-szolgáltatási környezetben mindig létre kell hozni alhálózat mivel alhálózat hálózati oszlopazonosító jobboldali zárolásához bejövő forgalom mögött felsőbb szintű eszközök és szolgáltatások, például, hogy a http- és HTTPS-forgalom csak az adott felsőbb szintű IP-címek fogadta a partner használható.

Bejövő és kimenő hálózati forgalmának engedélyezésére alhálózat szabályozza [hálózati biztonsági csoport]használata[NetworkSecurityGroups]. Bejövő szabályozása hálózati biztonsági szabályok létrehozása a hálózati biztonsági csoport, és ezután hozzárendelése a hálózati biztonsági csoport a az alkalmazás-szolgáltatási környezetben tartalmazó alhálózat szükséges.

Hálózati biztonsági csoport alhálózat van rendelve, az alkalmazás-szolgáltatási környezetben alkalmazások bejövő forgalmat van engedélyezve, illetve tiltott tartományok engedélyezve alapján, és a megtagadási a hálózati biztonsági csoport definiált szabályokat.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>Az alkalmazás-szolgáltatási környezetben használt hálózati portok ##
Mielőtt bejövő hálózati forgalmat a hálózati biztonsági csoport zárolása, fontos, hogy az alkalmazás-szolgáltatási környezetben által használt kötelező és választható hálózati portok csoportja.  Ki az egyes portok forgalom véletlenül bezárása eredményezhet funkcióvesztés az alkalmazás-szolgáltatási környezetben.

Az alábbiakban megtalálja az alkalmazás-szolgáltatási környezetben által használt portok listáját. Az összes legyen **TCP**, példáiban egyértelműen:

- 454:, **port szükséges** kezelésére és az alkalmazás szolgáltatási környezetben SSL keresztül fenntartása Azure infrastruktúra által használt.  Nem blokkolja a forgalmat a port.  A port mindig van kötve a nyilvános virtuális egy ASE a.
- 455:, **port szükséges** kezelésére és az alkalmazás szolgáltatási környezetben SSL keresztül fenntartása Azure infrastruktúra által használt.  Nem blokkolja a forgalmat a port.  A port mindig van kötve a nyilvános virtuális egy hajlamosnak.
- 80: alapértelmezett port tartalmaz a bejövő HTTP-alkalmazás futó alkalmazás szolgáltatás tervek az alkalmazás-szolgáltatási környezetben.  A ILB engedélyező ASE a port kötve a ASE ILB címét.
- 443-as: alapértelmezett port tartalmaz a bejövő SSL-alkalmazás futó alkalmazás szolgáltatás tervek az alkalmazás-szolgáltatási környezetben.  A port a ILB engedélyező ASE kötődik a ASE ILB címét.
- 21: FTP-vezérlő csatorna.  A port is biztonságosan blokkolhatja Ha FTP nem használja-e.  A ILB engedélyező ASE a port is kötni ILB címre az egy ASE.
- 990: FTPS vezérlő csatornát.  A port is biztonságosan blokkolhatja Ha FTPS nem használja-e.  A ILB engedélyező ASE a port is kötni ILB címre az egy ASE.
- 10001-10020: FTP-adatcsatornát.  A vezérlő csatorna az alábbi portokat is biztonságosan blokkolhatja Ha FTP nem használja-e.  A ILB engedélyező ASE a ASE ILB címet kell kötni a port.
- 4016: hibakereséshez távoli a Visual Studio 2012.  A port is biztonságosan letilt, ha a szolgáltatás nem használja-e.  A ILB engedélyező ASE a port kötve a ASE ILB címét.
- 4018: hibakereséshez távoli Visual Studio 2013.  A port is biztonságosan letilt, ha a szolgáltatás nem használja-e.  A ILB engedélyező ASE a port kötve a ASE ILB címét.
- 4020: hibakereséshez távoli a Visual Studio 2015.  A port is biztonságosan letilt, ha a szolgáltatás nem használja-e.  A ILB engedélyező ASE a port kötve a ASE ILB címét.

## <a name="outbound-connectivity-and-dns-requirements"></a>Kimenő kapcsolatok és a DNS-követelményei ##
Az alkalmazás szolgáltatás csak akkor működik megfelelően környezetben szükséges különböző végpontok kimenő elérését. A külső, egy ASE által használt végpontokat teljes listáját a "Hálózati kapcsolat szükséges" szakasz a [Hálózati konfigurálásról számára készült ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) cikk szerepel.

Alkalmazás szolgáltatási környezetben konfigurálva a virtuális hálózat érvényes DNS-infrastruktúrát szükség.  Ha a DNS-konfigurációs módosítják az alkalmazás-szolgáltatási környezetben létrehozását követően bármilyen okból fejlesztők kényszerítheti az alkalmazás-környezetből felveszi az új DNS-konfigurációs.  Az [Azure portál] az alkalmazás szolgáltatási környezetben kezelése lap tetején található a működés közben környezet újra kell indítani az "Indítsa újra a" ikonnal kiváltó[ NewPortal] a környezet felveszi az új DNS-konfigurációja miatt.

Ajánlott is, hogy minden olyan egyéni DNS-kiszolgálók a vnet telepíthető az alkalmazás-szolgáltatási környezetben létrehozása előtt időszakokra.  Ha egy virtuális hálózat DNS konfigurálása az alkalmazás-szolgáltatási környezetben létrehozása közben megváltozik, hogy az alkalmazás-szolgáltatási környezetben létrehozási folyamat meghibásodása eredményezi.  Egy hasonló tekintettel az egyéni DNS-kiszolgáló megtalálható-e VPN az átjárók másik végét, és a DNS-kiszolgáló nem érhető el vagy nem érhető el, ha az alkalmazás-szolgáltatási környezetben létrehozásának folyamatát is sikertelen lesz.

## <a name="creating-a-network-security-group"></a>Hálózati biztonsági csoport létrehozása ##
Teljes a hálózati biztonsági csoportok, hogyan munka című témakör tartalmaz az alábbi [információk][NetworkSecurityGroups].  A részletek érintésvezérlési alatt kiemeli a hálózati biztonsági csoportokat, és konfigurálása, amely tartalmazza az alkalmazás-szolgáltatási környezetben alhálózat hálózati biztonsági csoport alkalmazása egy kiemelve.

**Megjegyzés:** Hálózati biztonsági csoportok konfigurálhatók grafikusan az [Azure-portál](https://portal.azure.com) és Azure Powershellen keresztül.

Hálózati biztonsági csoportok első olyan előfizetéséhez társított önálló egységként jönnek létre. Mivel hálózati biztonsági csoportok készült Azure területen, győződjön meg arról, hogy a hálózati biztonsági csoport ugyanabban a régióban, mint az alkalmazás-szolgáltatási környezetben van-e létre.

A következő szemlélteti a hálózati biztonsági csoport létrehozása:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Hálózati biztonsági csoport létrehozását követően egy vagy több hálózati biztonsági szabályok adódik hozzá  Mivel a szabálykészlet idő után megváltoznak, ki a számozási sorrendben használt szabály prioritásának megkönnyítheti a többi szabályt beszúrása adott idő alatt térköz ajánlott.

Az alábbi példában egy szabályt, amely az adatkezelési portokat szükség szerint az Azure infrastruktúra kezeléséhez, és az alkalmazás-szolgáltatási környezetben karbantartása hozzáférést biztosít a kifejezetten mutatja.  Ne feledje, hogy az összes felügyeleti forgalom átfolyása az SSL védett ügyfél tanúsítványok, így akkor is, ha a portokat nyitnak meg azok bármely személy Azure kezelési infrastruktúra nem érhető el.


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

Amikor zárolása port 80 és 443-as "elrejtése" mögött felsőbb szintű eszközök és szolgáltatások alkalmazás szolgáltatási környezetben való hozzáférést, fog kell tudnia a felsőbb szintű IP-cím.  Például a webes alkalmazás tűzfal (WAF) használ, a WAF van a saját IP-címe (vagy), amely akkor használja, ha egy alkalmazás későbbi szolgáltatási környezetben proxyként forgalmat.  Meg kell az IP-cím használata a hálózati biztonsági szabály *SourceAddressPrefix* paramétere.

Az alábbi példában egy adott felsőbb szintű IP-címet a bejövő forgalom kifejezetten engedélyezett.  A cím *1.2.3.4* helyőrzőként szolgál egy felsőbb szintű WAF IP-címét.  Módosítsa a megfelelő a címet a felsőbb szintű eszköz vagy a szolgáltatás által használt értékét.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
FTP-támogatás tetszés szerint a következő szabályok használható sablonként való hozzáférést a FTP vezérlő port és az adatok csatorna portokat.  Mivel FTP állapot-nyilvántartó protokoll, nem lehet tudni a forgalmat FTP hagyományos HTTP-/ HTTPS tűzfal vagy proxykiszolgáló eszközön keresztül.  Ebben az esetben szüksége lesz a *SourceAddressPrefix* beállítása egy másik értékre – például az IP-címtartományokat Fejlesztőeszközök vagy üzembe gépek mely FTP ügyfelek futtatnak. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Megjegyzés:** csatorna port adattartomány megváltozhat az előnézeti időszakban.)

A Visual Studio távoli hibakeresés használata esetén a következő szabályok szemléltetik hozzáférést.  Mivel minden verzió egy másik portot használja a távoli hibakereséshez, van egy külön szabályt minden egyes Visual Studio támogatott verziójába.  FTP access, az távoli hibakeresési forgalom előfordulhat, hogy nem megfelelő sorrendben hagyományos WAF vagy a proxykiszolgáló eszköz keresztül.  A *SourceAddressPrefix* inkább adhatja meg a Visual Studio futó Fejlesztőeszközök gépek IP-címtartományokat.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Hálózati biztonsági csoport alhálózat rendelése ##
Hálózati biztonsági csoport rendelkezik egy alapértelmezett biztonsági szabályt, amely letiltja az összes külső forgalmat a hozzáférést.  A fent leírt hálózati biztonsági szabályok és az alapértelmezett biztonsági szabály blokkolja a bejövő forgalmat, a eredménye ezt csak a forgalmat a forrás címről *engedélyezése* művelet társított tartományok tudják forgalom küldésére alkalmazások alkalmazás szolgáltatási környezetben működik.

Hálózati biztonsági csoport biztonsági szabályok kitölti, miután kell az alkalmazás-szolgáltatási környezetben tartalmazó az alhálózathoz-e kiosztani.  A hozzárendelés parancsot egyaránt az alkalmazás-szolgáltatási környezetben helye a virtuális hálózat nevét, valamint a nevét az alhálózathoz, ha az alkalmazás-szolgáltatási környezetben jött létre hivatkozik.  

Az alábbi példában látható van hozzárendelve egy alhálózat és virtuális hálózati hálózati biztonsági csoport:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Miután létrejött a hálózati csoport-hozzárendelés (hozzárendelés egy hosszan futó műveleteket és is eltarthat néhány percig), csak a megfelelő *Engedélyezés* szabályok bejövő forgalom sikeresen eléri apps az alkalmazás-szolgáltatási környezetben.

A teljesség a következő példa bemutatja a eltávolítása, és így a hálózati biztonsági csoport a alhálózat dis-társítani:


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Közvetlen IP-SSL megfontolandó különleges szempontok ##
Ha be van állítva az alkalmazás közvetlen IP-SSL a cím (vonatkozik, amelyek csak egy nyilvános virtuális ASEs), az alkalmazás-szolgáltatási környezetben alapértelmezett IP-címe helyett, HTTP és a HTTPS forgalom az alhálózathoz folytatódjon keresztül nem 80-as és 443-as port más-más szabálykészletet.

Az egyes pár minden IP-SSL cím által használt portok az alkalmazás szolgáltatási környezetben részletek UX lap portál felhasználói felületén található.  Jelölje be "az összes beállítások" a "IP-címek"-->.  A "IP-címek" lap összes explicit módon beállított IP-SSL címek táblázatát környezethez az alkalmazás szolgáltatás, a speciális port pár, amellyel a forgalmat a HTTP- és HTTPS minden IP-SSL cím társított együtt jeleníti meg.  A port párt használatakor a DestinationPortRange paraméterekkel szabályok beállítása a hálózati biztonsági csoport kell, hogy.

Ha egy ASE lévő IP-SSL protokoll használatára van beállítva, külső ügyfeleket nem fogja látni, és nem kell aggódnia amiatt, hogy a speciális port pár leképezést.  Alkalmazások-alapú forgalmat a szokásos módon flow fog konfigurált IP-SSL címére.  A speciális port párosítás a fordítás automatikusan történik belső során a végleges láb adatforgalom, az a ASE tartalmazó alhálózat be. 

## <a name="getting-started"></a>Első lépések

Első lépések az alkalmazás-szolgáltatási környezetben, lásd: [alkalmazás szolgáltatási környezetben – bevezetés][IntroToAppServiceEnvironment]

Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Alkalmazások biztonságosan kapcsolódás kódmentes erőforrás alkalmazás szolgáltatás környezetből köré, című témakör tartalmaz részletes [biztonságosan csatlakoztatása az alkalmazás szolgáltatás környezetből Kódmentes erőforrások][SecurelyConnecttoBackend]

Az Azure alkalmazás szolgáltatás platform kapcsolatos további tudnivalókért lásd a [Azure alkalmazás szolgáltatás][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
