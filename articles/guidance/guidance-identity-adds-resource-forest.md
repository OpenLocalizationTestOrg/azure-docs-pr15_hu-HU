<properties
   pageTitle="Azure architektúra hivatkozás - IaaS: Azure Active Directory erőforráserdők létrehozása |} Microsoft Azure"
   description="Bemutatja, hogyan hozhat létre egy megbízható Active Directory tartományi Azure-ban."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="telmos"/>

# <a name="creating-a-active-directory-directory-services-adds-resource-forest-in-azure"></a>Azure Active Directory címtár szolgáltatások (ÖSSZEADJA) erőforráserdők létrehozása

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Ez a cikk ismerteti az Active Directory tartományi létrehozása az Azure elkülönülnek, de megbízható, a helyszíni erdő tartománya.

> [AZURE.NOTE] Azure van két különböző telepítési modellek: [Erőforrás-kezelő] [ resource-manager-overview] és klasszikus. A hivatkozás architektúra erőforrás-kezelő Microsoft javasolja új telepítési használja.

Előfordulhat, hogy egy szervezet helyszíni Active Directory (AD) futtató számos különböző tartományok alkotó erdő. Például a különféle részlegek vagy suborganizations egyéni tartományok is létrehozhat, vagy új tartományok előfordulhat, hogy hozzáadta a megszerzése vagy más szervezetek egyesülés eredményeként. Elkülönítési fokát, esetleg biztonsági okokból külön kell tartani funkcionális területen adja meg a tartományok is használhatja, de megoszthatja bizalmi kapcsolatok létrehozásával tartományok között.

Egy szervezet, amelyik a külön tartományok kihasználhatja Azure szerint egy vagy több ezeket a tartományokat az áthelyezés a felhőbe. Azt is megteheti egy szervezet előfordulhat, hogy szeretne összes felhő erőforrás adott tárolt helyszíni logikailag elkülönülő megtartani, és a felhő erőforrások adatainak tárolására is a felhőben tárolt tartomány a saját Directoryban.

Alkalmazhat az Active Directory Azure-ban számos különböző módon az [Azure Active Directory bővítése] cikkekben ismertetett módon[ extending-ad-to-azure] és [Azure Active Directory végrehajtási][implementing-aad]. A dokumentum egy adott esetben koncentrál: tartomány létrehozása, amelyek eltérnek a helyszíni tartott tartományaira, de, beállíthatja, hogy a helyszíni tartományokkal megbízhatósági kapcsolat a felhőben. 

Ez a felépítés esettel jellemző használata a következők:

- Objektumok és a felhőben tárolt identitások biztonsági elkülönítésének fenntartása.

- Egyéni tartományok áttelepítése a helyszíni a felhőbe.

## <a name="architecture-diagram"></a>Architektúra diagramja

Az alábbi ábra kiemeli a fontos e (*kattintson a Nagyítás*) architektúra összetevőket. A szürkén jelenik meg elemeket kapcsolatos további tudnivalókért olvassa el a [végrehajtási egy biztonságos hibrid hálózat architektúrája Azure] [ implementing-a-secure-hybrid-network-architecture] és [a biztonságos hibrid hálózat architektúrája Azure-ban Internet-hozzáféréssel rendelkező végrehajtásához][implementing-a-secure-hybrid-network-architecture-with-internet-access]:

[! [0]][0]

- **A helyszíni hálózaton.** A helyszíni hálózaton saját AD erdőt és a tartományokat tartalmazza.

- **Active Directory-kiszolgálók.** Ezek a címtár-szolgáltatások (AD DS) fut, mint a felhőben VMs végrehajtási tartomány vezérlők. Ezeket a kiszolgálókat az azokat tároló helyszíni külön egy vagy több tartományt tartalmazó erdő szolgáltató.

- **Egyirányú kapcsolatot.** A példában a diagram a tartományból egyirányú megbízhatósági kapcsolat a felhőben, hogy a helyszíni tartomány jeleníti meg. Ez a kapcsolat lehetővé teszi, hogy a helyszíni felhasználóknak, hogy a tartományban a felhőben, de nem fordítva forrásokat. Ez a gyakori eset. Azonban kétirányú is létrehozhat, ha a felhőben felhasználók is szükséges a helyszíni erőforrások elérését.

- **Az Active Directory-alhálózat.** Az Active Directory tartományi szolgáltatások kiszolgálón lévő külön alhálózathoz van közzétéve. Az Active Directory tartományi szolgáltatások kiszolgálók védelme és szolgáltatja a forgalmat a váratlan források ellen tűzfalat NSG szabályokat.

- **Webes réteg alhálózat**, **üzleti réteg alhálózat**és **adatokat az első csoportba tartozó alhálózat**. Ezek alhálózat szolgáltató a kiszolgálókat és összetevők szerepelnek, amelyek az alkalmazások futtatásához a felhőben. További tudnivalókért lásd: az [Operációs rendszert futtató VMs az Azure-N szintű architektúrája][running-VMs-for-an-N-tier-architecture-on-Azure]. Az erőforrások és a VMs az alhálózathoz az a felhő tartományon belül tartalmazzák.

- **Azure átjáró**. Az Azure átjáró a helyszíni hálózaton és az Azure VNet közötti kapcsolatot biztosít. Ez lehet a [virtuális Magánhálózati kapcsolat] [ azure-vpn-gateway] vagy [a készült Azure ExpressRoute][azure-expressroute]. További tudnivalókért lásd: a [végrehajtási egy biztonságos hibrid hálózat architektúrája Azure][implementing-a-secure-hybrid-network-architecture].

## <a name="recommendations"></a>Javaslatok

Ez a témakör az alapvető alkatrészek az alap építészet végrehajtásához szükséges alapján javaslatok listáját. Ezek a javaslatok terjed ki:

- ÖSSZEADJA a beállítások és

- Megbízhatósági kapcsolat konfigurációs.

Lehet, hogy az itt leírtaktól további vagy eltérő követelményei. Használhatja az elemek ebben a részben kiindulási pontként, hogy miként szabhatja testre a saját rendszer architektúra.

### <a name="adds-recommendations"></a>ÖSSZEADJA javaslatok

Adott arra vonatkozó javaslatokat Active Directorynak a felhőben, tekintse át a dokumentumot [Az Azure Active Directory bővítése][extending-ad-to-azure]. A következő cikket: [Útmutató telepítése a Windows Server Active Directory a Azure virtuális gépeken futó] [ ad-azure-guidelines] további részletes információkat tartalmaz.

### <a name="trust-recommendations"></a>A megbízható javaslatok

A helyszíni tartomány belül egy másik erdő a felhőben tartományokból találhatók. Ahhoz, hogy a helyszíni felhasználók a felhőben hitelesítés, a tartományok a felhőben kell bíznia a bejelentkezési tartomány a helyszíni erdőben. Hasonlóképpen a felhőben bejelentkezési tartomány nyújt a külső felhasználók, ha szükségessé válhat az a felhő a tartományt a helyszíni erdőben.

Futtatva létesíthet erdő szintre bizalmi kapcsolatok létrehozásával [erdőszintűek][creating-forest-trusts], vagy [külső bizalmi kapcsolatok]létrehozásával, a tartomány szintjén[creating-external-trusts]. Egy erdő szintű adatvédelmi létre két rendszerű tartományai között. Egy külső szintű tartománymeghatalmazási csak két megadott tartomány közötti kapcsolatot hoz létre. A külső tartománynév tartományok különböző erdőkben közötti szintű bizalmi kapcsolatok csak úgy kell létrehozni.

Lehet, hogy meghatalmazások egyirányú (egyirányú), illetve a kétirányú (kétirányú):

- Egyirányú lehetővé teszi a felhasználóknak egy tartomány vagy erdő (más néven a *bejövő* tartomány vagy erdő), az erőforrások tartani egy másik eléréséhez (a *kimenő* tartomány vagy erdő). 

- Kétirányú bizalmi lehetővé teszi a felhasználóknak a tartomány vagy a erdő tartani a másik forrásokat.

Az alábbi táblázat összefoglalja az adatvédelmi beállítások néhány egyszerű felhasználási területei:

| Eset | A helyszíni adatvédelmi | Felhőalapú adatvédelmi |
|----------|-------------------|-------------|
| A helyszíni erőforrásokhoz a felhőben van szükségük a felhasználóknak, de nem fordítva | Egy, a bejövő | Egyirányú, kimenő |
| Az access van szükségük a felhasználóknak a felhőben erőforrásait a helyszíni, de nem fordítva | Egyirányú, kimenő | Egy, a bejövő |
| Az a felhő és a helyszíni tárolt erőforrások hozzáférésre van szüksége a felhasználók a felhőben, és a helyszíni mindkét | Kétirányú, a bejövő és kimenő | Kétirányú, a bejövő és kimenő |

## <a name="security-considerations"></a>Biztonsági megfontolások

Sávos erdőszintűek tranzitívak. Ha erdőszintű szintű megbízhatósági kapcsolat a helyszíni erdőn és a felhőben erdő között létrehozni, vagy erdőben létrehozott új tartományához terjeszteni a ezt az adatvédelmi. Ha a tartományok használatával adja meg a biztonsági okokból elkülönítésének, fontolja meg, csak a tartomány szintjén bizalmi kapcsolatok létrehozásával. A tartományok szintű megbízhatósági intranzitív.

Active Directory-specifikus biztonsági megfontolások, című *biztonsági megfontolások* az [Azure Active Directory bővítése][extending-ad-to-azure].

## <a name="availability-considerations"></a>Elérhetőség kapcsolatos szempontok

Minden egyes tartományhoz legalább két tartomány vezérlők végrehajtása. Kiszolgálók között automatikus replikáció lehetővé teszi. Hozzon létre egy elérhetősége az egyes tartományok kezelése Active Directory-kiszolgálók eljáró VMs beállítása. Győződjön meg arról, hogy nincsenek-e legalább két kiszolgálók megadása. 

Célszerű minden egyes tartományhoz egy vagy több kiszolgálókon kijelölő [készenléti műveleti mesteralakzatok] szerint is[ standby-operations-masters] abban az esetben, ha FSMO szerepet eljáró kiszolgálóra való kapcsolódási sikertelen lesz.

## <a name="scalability-considerations"></a>Méretezhetőség kapcsolatos szempontok

Active Directory tartomány vezérlők tartománynevek részét képező automatikusan méretezhető. Kérelmek egy tartományon belül az összes vezérlő vannak elosztva. Egy másik tartományvezérlőnek is felvehet, és automatikusan szinkronizálja a tartományt. Irányítsa át a vezérlők alapú forgalmat a tartományon belül egy külön terheléselosztó nem állítja be. Gondoskodhat arról, hogy minden tartomány vezérlők elegendő memória- és erőforrások, a tartomány-adatbázis kezelése. Az összes tartományvezérlőnek VMs az azonos méretűre.

## <a name="management-and-monitoring-considerations"></a>Kezelés és előtt megfontolandó kérdések figyelése

Kezelés és előtt megfontolandó kérdések figyelése információkért egyenértékű foglalása című szakaszban találhatók az [Azure Active Directory bővítése][extending-ad-to-azure]. 

További tudnivalókért lásd: [Az Active Directory figyelése][monitoring_ad]. Eszközök, például [Microsoft rendszerek Center] telepítheti[ microsoft_systems_center] az adatkezelési alhálózat érdekében ezeket a műveleteket a megfigyeléssel kiszolgálón. 

## <a name="solution-components"></a>Megoldás-összetevők

Megoldás mintaparancsfájl, [Deploy-ReferenceArchitecture.ps1][solution-script], érhető el, hogy is használhatja, amely követi a jelen cikkben ismertetett javaslatok architektúrája végrehajtása. Ez a parancsfájl Azure erőforrás-kezelő sablonok használja. A sablonok állnak rendelkezésre, alapvető építőelemek csoportja, amelyek hajt végre egy adott műveletet, például egy VNet létrehozásához, vagy egy NSG konfigurálása. A parancsprogram célja téve a sablon telepítésének.

A sablonok vannak paraméteres, külön JSON-fájlokban tárolt paraméterekkel. A paraméterek ezeket a fájlokat a saját igényeinek kielégítéséhez telepítésének konfigurálásához módosíthatók. Nem kell módosítani kell a sablonok saját maguk. A sémák az objektumok paraméter fájlokban nem módosítható.

A sablonok szerkesztésekor létrehozása objektumok [Elnevezési szabályai ajánlott Azure erőforrások]ismertetett elnevezési szabályokat követő[naming-conventions].

A minta megoldást hoz létre, és olyan környezetben beállítja a felhőben, amely *treyresearch.com*megnevezett tartományt. A környezet magában foglalja a ÖSSZEADJA alhálózat és kiszolgálók, DMZ, webes réteg, üzleti réteg és adat-hozzáférési szint összetevőket virtuális Magánhálózati átjárót, és a kezelés réteg. A minta megoldást is hozhat létre egy szimulált a helyszíni környezet saját tartománnyal, *akkor a contoso.com*választható konfiguráció. A megoldás tartalmaz megbízhatósági kapcsolatot létesíteni át ezeket a tartományokat, amely lehetővé teszi a helyszíni elérése a felhőben *treyresearch.com* tartományban objektumok parancsfájlokat.

Az alábbi szakaszok ismertetik a helyszíni elemei, és a felhő konfigurációk.

### <a name="on-premises-components"></a>A helyszíni összetevők

>[AZURE.NOTE] Ezek az összetevők nem a fő fókusz a architektúra, a jelen dokumentum ismerteti, és állnak rendelkezésére egyszerűen ad tesztkörnyezetben a felhőben biztonságosan, nem oszlophivatkozás használatával valós munkakörnyezetben lehetőséget. Emiatt a szakasz csak a kulcs paraméter fájlokat foglalja össze. Módosíthatja a beállításokat, például az IP-címek vagy a VMs méretű, de célszerű változatlan marad a további paramétereket számos hagyja.

Ez a környezet Active Directory erdőn a *contoso.com* tartomány magába foglalja. A tartomány két ÖSSZEADJA kiszolgáló IP-címek 192.168.0.4 és 192.168.0.5 tartalmazza. Ezek a két kiszolgálók futtatása a DNS-szolgáltatás is. A helyi rendszergazdafiók mindkét VMs a meghívott `testuser` jelszóval `AweS0me@PW`. Emellett a konfigurációs hoz létre egy virtuális Magánhálózati átjárót a felhőbeli VNet csatlakozhat. A következő JSON-fájlokat a [**Paraméterek/helyi**] található szerkesztésével módosíthatja a konfigurációs[ on-premises-folder] mappa:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Ez a fájl határozza meg, hogy a hálózati címterületet a helyszíni környezetben.

- ** [virtualMachines-adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. A fájl tartalmaz a helyszíni VMs szolgáltatója szolgáltatások ÖSSZEADJA az adatokat. Alapértelmezés szerint két *Standard-DS3-v2* VMs jönnek létre.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** és ** [connection.parameters.json][on-premises-connection-parameters]**. Ezeket a fájlokat a felhőben, beleértve az átjáró áthaladó forgalom védelmére használandó megosztott kulcs tartsa lenyomva a virtuális Magánhálózati kapcsolatot az Azure virtuális Magánhálózati átjáró beállításait.

A hátralévő a mappában lévő fájlok a a helyszíni tartomány (*contoso.com*), az infrastruktúrát használó létrehozásához használt konfigurációs adatokat tartalmazzák. ÖSSZEADJA, a telepítő DNS, telepítheti őket, és hozzon létre a helyszíni erdőt.

A megoldást is használja a következő parancsfájl, [a bejövő-trust.ps1]nevű[incoming-trust], amely a helyszíni tartomány egy gépen fut:

```Powershell
# Run the following powershell script in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)

$TrustedDomainName = "contoso.com"
#$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

$TrustingDomainName = "treyresearch.com"
$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustingDomainDnsIpAddresses
$ForwardDomainName = $TrustingDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

A parancsprogram hozzáadása az Active Directory tartományi szolgáltatások kiszolgálók az IP-címek a felhőben (lásd a következő szakaszt) a helyi DNS-szolgáltatás, és kattintson a [netdom] [ netdom] bejövő egyirányú megbízhatósági kapcsolat létrehozása a tartomány a felhőben (*treyresearch.com*) parancsot.

### <a name="cloud-components"></a>Felhőalapú összetevők

Ezek az összetevők e architektúra alapvető alkotnak. Ezeket a infrastruktúra *treyresearch.com* tartomány beállítása és a megbízhatósági kapcsolatok létrehozása a helyszíni *contoso.com* tartománnyal. A [**Paraméterek/azure**] [ azure-folder] mappa tartalmazza a következő paraméterek fájlok konfigurálásához az alábbi összetevőket:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Ez a fájl a VNet felépítése a VMs és más összetevőket a felhőben határozza meg. Beállítások, például a nevet, címterület, alhálózat és minden szükséges DNS-kiszolgáló címét tartalmazza. A DNS-címek jelenik meg ez a példa hivatkozás a helyszíni DNS-kiszolgálók, valamint az alapértelmezett Azure DNS-kiszolgáló IP-címét. Ezeket a címeket, ha nem használ a minta a helyszíni környezetben hivatkozni saját DNS-beállításainak módosítása:
    
    ```json
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg",
        "addressPrefixes": [
          "10.0.0.0/16"
        ],
        "subnets": [
          {
            "name": "dmz-private-in",
            "addressPrefix": "10.0.0.0/27"
          },
          {
            "name": "dmz-private-out",
            "addressPrefix": "10.0.0.32/27"
          },
          {
            "name": "dmz-public-in",
            "addressPrefix": "10.0.0.64/27"
          },
          {
            "name": "dmz-public-out",
            "addressPrefix": "10.0.0.96/27"
          },
          {
            "name": "mgmt",
            "addressPrefix": "10.0.0.128/25"
          },
          {
            "name": "GatewaySubnet",
            "addressPrefix": "10.0.255.224/27"
          },
          {
            "name": "web",
            "addressPrefix": "10.0.1.0/24"
          },
          {
            "name": "biz",
            "addressPrefix": "10.0.2.0/24"
          },
          {
            "name": "data",
            "addressPrefix": "10.0.3.0/24"
          },
          {
            "name": "adds",
            "addressPrefix": "10.0.4.0/27"
          }
        ],
        "dnsServers": [
          "10.0.4.4",
          "10.0.4.5",
          "168.63.129.16"
        ]
      }
    }
    ```

- ** [virtualMachines-adds.parameters.json ] [ virtualmachines-adds-parameters] **. Ez a fájl konfigurálja a VMs operációs rendszert futtató ÖSSZEADJA a felhőben. A konfiguráció két VMs áll. A rendszergazdai felhasználónevével és a jelszó módosítása a `virtualMachineSettings` szakaszra, és tetszés szerint módosíthatja a virtuális méretét annak a tartománynak az igényeknek megfelelően alakíthatja:

    További tudnivalókért lásd: az [Azure Active Directory bővítése][extending-ad-to-azure].
    
    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-ad",
        "computerNamePrefix": "aad",
        "size": "Standard_DS3_v2",
        "osType": "Windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "adds",
            "privateIPAllocationMethod": "Static",
            "startingIPAddress": "10.0.4.4",
            "enableIPForwarding": false,
            "dnsServers": [
            ],
            "isPrimary": "true"
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 127,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [
        ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-as"
        }
      }
    },
    "virtualNetworkSettings": {
      "value": {
        "name": "ra-adtrust-vnet",
        "resourceGroup": "ra-adtrust-network-rg"
      }
    },
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ```

- ** [hozzáadása – ad-tartomány-controller.parameters.json][add-adds-domain-controller-parameters]**. Ezt a fájlt tartalmazza a beállításait a *treyresearch.com* tartomány felvétele kiszolgálókon tartó hozhat létre. A tartomány létrehozása és hozzáadása kiszolgálókon hozzáadása egyéni bővítmények használ. Amíg nem hoz létre a további hozzáadása kiszolgálók (ebben az esetben meg kell adhat hozzá a `vms` tömb), a nevük módosíthatja az alapértelmezett, vagy hozzon létre egy tartományt, nem kell ezzel a fájllal módosítása más névvel szeretné.

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. A fájl tartalmaz a beállításokat, amelyek az Azure virtuális Magánhálózati átjáró létrehozása a felhőben, használja a helyszíni hálózaton. Módosítania kell a `sharedKey` az az érték a `connectionsSettings` megfelelően, hogy a helyszíni VPN eszköz szakaszban. További tudnivalókért lásd: a [végrehajtási egy hibrid hálózat architektúrája Azure és a helyszíni VPN][hybrid-azure-on-prem-vpn].

- ** [dmz-private.parameters.json] [ dmz-private-parameters] ** és ** [dmz-public.parameters.json ] [ dmz-public-parameters] **. Ezeket a fájlokat konfigurálása (nyilvános) bejövő és kimenő (személyes) oldalán a VMs, amely tartalmazza a DMZ védelme a kiszolgálókat, a felhőben. Ezeket az elemeket és a konfigurációs kapcsolatos további tudnivalókért lásd: a [végrehajtási Azure és az Internet között a DMZ][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer-web.parameters.json][loadBalancer-web-parameters]**, ** [loadBalancer-biz.parameters.json][loadBalancer-biz-parameters]**, és ** [loadBalancer-data.parameters.json][loadBalancer-data-parameters]**. A paraméterek fájlok az adatok, üzleti és web access rétegek virtuális specifikációi, és állítsa be az egyes réteg terheléselosztókkal. Ezek a VMs a web Apps alkalmazások és az adatbázisok, és végezze el a vállalati feladatok a szervezet számára. A webes réteg a VMs bekerülnek a tartományhoz a felhőben, megadott beállításokkal a ** [web-virtuális-tartomány-join.parameters.json] [ web-vm-domain-join-parameters] ** fájlt.

    Fájlonként konfigurációs paramétereket két csoportját tartalmazza. A `virtualMachineSettings` szakaszban határozza meg a VMs, amely a felhőben az ADFS-szolgáltatás üzemelteti. Alapértelmezés szerint a parancsfájl hoz létre két az alábbiak VMs elérhetősége mindazon. A következő töredékek megjelenítése a *loadBalancer-web.parameters.json* fájl vonatkozó részei:

    ```json
    "virtualMachinesSettings": {
      "value": {
        "namePrefix": "ra-adtrust-web",
        "computerNamePrefix": "web",
        "size": "Standard_DS1_v2",
        "osType": "windows",
        "adminUsername": "testuser",
        "adminPassword": "AweS0me@PW",
        "osAuthenticationType": "password",
        "nics": [
          {
            "isPublic": "false",
            "subnetName": "web",
            "privateIPAllocationMethod": "Dynamic",
            "isPrimary": "true",
            "enableIPForwarding": false,
            "dnsServers": [ ]
          }
        ],
        "imageReference": {
          "publisher": "MicrosoftWindowsServer",
          "offer": "WindowsServer",
          "sku": "2012-R2-Datacenter",
          "version": "latest"
        },
        "dataDisks": {
          "count": 1,
          "properties": {
            "diskSizeGB": 128,
            "caching": "None",
            "createOption": "Empty"
          }
        },
        "osDisk": {
          "caching": "ReadWrite"
        },
        "extensions": [ ],
        "availabilitySet": {
          "useExistingAvailabilitySet": "No",
          "name": "ra-adtrust-web-vm-as"
        }
      }
    },
    ...
    "buildingBlockSettings": {
      "value": {
        "storageAccountsCount": 2,
        "vmCount": 2,
        "vmStartIndex": 1
      }
    }
    ````
    A méret és a VMs minden réteg igényeinek megfelelően módosíthatja.

    A `loadBalancerSettings` a témakör a leírását a terheléselosztó ezek VMs. A terheléselosztó átadja megjelenő forgalmat a (HTTP) 80 és 443-as (HTTPS) portot egy vagy több a VMs. 

    >[AZURE.NOTE] 80-as port szabálya HTTP helyett egy TCP-kapcsolatot használ. Ennek oka az, a telepítés az IIS webes rétegben csak a Windows-hitelesítés támogatására van beállítva. Névtelen hitelesítés le van tiltva. *Ping* kísérel meg 80-as port HTTP-kapcsolaton keresztül nem sikerül egy 401 (nem hitelesített) hiba, mivel csak a TCP-kapcsolat használatával észleli, hogy az aktív-e a port:

    ```json
    "loadBalancerSettings": {
      "value": {
        "name": "ra-adtrust-web-lb",
        "frontendIPConfigurations": [
          {
            "name": "ra-adtrust-web-lb-fe",
            "loadBalancerType": "internal",
            "internalLoadBalancerSettings": {
              "privateIPAddress": "10.0.1.254",
              "subnetName": "web"
            }
          }
        ],
        "backendPools": [
          {
            "name": "ra-adtrust-web-lb-bep",
            "nicIndex": 0
          }
        ],
        "loadBalancingRules": [
          {
            "name": "http-rule",
            "frontendPort": 80,
            "backendPort": 80,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "http-probe",
            "enableFloatingIP": false
          },
          {
            "name": "https-rule",
            "frontendPort": 443,
            "backendPort": 443,
            "protocol": "Tcp",
            "backendPoolName": "ra-adtrust-web-lb-bep",
            "frontendIPConfigurationName": "ra-adtrust-web-lb-fe",
            "probeName": "https-probe",
            "enableFloatingIP": false
          }
        ],
        "probes": [
          {
            "name": "http-probe",
            "port": 80,
            "protocol": "Tcp",
            "requestPath": null
          },
          {
            "name": "https-probe",
            "port": 443,
            "protocol": "Tcp",
            "requestPath": null
          }
        ],
        "inboundNatRules": [ ]
      }
    }
    ```

- ** [virtualMachines-mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Ez a fájl az Ugrás párbeszédpanel/adatkezelési VMs adatokat tartalmazza. Csak akkor lehet bejelentkezési és felügyeleti hozzáférés szükséges a webes, üzleti és adatok rétegek VMs való az Ugrás mezőből. Alapértelmezés szerint a parancsfájl hoz létre egy egyetlen *Standard_DS1_v2* virtuális, de a fájl méretét, illetve további VMs létrehozása: Ha a terhelést a kezelés valószínűleg jelentős módosíthatja.

A konfiguráció is használja a [kimenő trust.ps1] [ outgoing-trust] parancsfájl egyirányú kimenő megbízhatósági kapcsolat létrehozása a *contoso.com* tartománnyal alább látható módon:

```powershell
# prerequiste: 
# You need to first run incoming-trust.ps1 in ra-adtrust-onpremise-ad-vm1 (ip 192.168.0.4)
# Then,
# Run the following powershell script in ra-adtrust-ad-vm1 (ip 10.0.4.4)

$TrustedDomainName = "contoso.com"
$TrustedDomainDnsIpAddresses = "192.168.0.4,192.168.0.5"

#$TrustingDomainName = "treyresearch.com"
#$TrustingDomainDnsIpAddresses = "10.0.4.4,10.0.4.5"

$ForwardIpAddress  = $TrustedDomainDnsIpAddresses
$ForwardDomainName = $TrustedDomainName

$IpAddresses = @()
foreach($address in $ForwardIpAddress.Split(',')){
    $IpAddresses += [IPAddress]$address.Trim()
}
Add-DnsServerConditionalForwarderZone -Name "$ForwardDomainName" -ReplicationScope "Domain" -MasterServers $IpAddresses

#netdom trust $TrustingDomainName /d:$TrustedDomainName /add
```

Ez a parancsfájl hasonlít a *Bejövő levelek-trust.ps1* parancsfájl ismertetett. Ad hozzá az IP-címek a helyszíni Active Directory tartományi szolgáltatások kiszolgálók a helyi DNS-szolgáltatás, és kattintson a [netdom] [ netdom] parancsot a Kimenő megbízhatósági kapcsolat létrehozásához.

## <a name="solution-deployment"></a>Megoldás üzembe helyezése

A megoldás feltételezi, hogy az alábbi előfeltételek:

- Ha egy meglévő Azure előfizetésbe erőforrás csoportokat hozhat létre.

- Letöltött és Azure Powershell legújabb változatát telepítette. Lásd: [Itt] [ azure-powershell-download] utasításokat.

Futtatása, amely a megoldást üzembe helyezése:

1. A helyi számítógépen kényelmes mappa áthelyezése és a következő almappák létrehozása:

    - Parancsfájlok

    - Parancsfájlok és paraméterek

    - Parancsfájlok/paraméterek/helyi

    - Parancsfájlok/paramétereket és Azure

2. Töltse le a [Központi telepítés-ReferenceArchitecture.ps1] [ solution-script] fájlt a parancsfájlok mappába

3. Töltse le a [Paraméterek/helyi] tartalmának[ on-premises-folder] parancsfájlok/paraméterek/helyi mappa:

4. Töltse le a [Paraméterek/azure] tartalmának[ azure-folder] parancsfájlok/paramétereket és Azure mappa.

5. A parancsfájlok mappát a Deploy-ReferenceArchitecture.ps1 fájl, és a következő sorokat megadhatja az erőforrás-csoportokat kell létrehozott vagy az erőforrások hozta létre a parancsfájl tárolására szolgáló módosítása:

    ```powershell
    # Azure Onpremise Deployments
    $onpremiseNetworkResourceGroupName = "ra-adtrust-onpremise-rg"

    # Azure ADDS Deployments
    $azureNetworkResourceGroupName = "ra-adtrust-network-rg"
    $workloadResourceGroupName = "ra-adtrust-workload-rg"
    $securityResourceGroupName = "ra-adtrust-security-rg"
    $addsResourceGroupName = "ra-adtrust-adds-rg"
    ```

6. A parancsfájlok/paraméterek/helyi rendszerekhez és parancsfájlok/paramétereket és Azure mappák a paraméter fájlok szerkesztése. Frissítse az erőforrás csoport hivatkozások ezeket a fájlokat a központi telepítés-ReferenceArchitecture.ps1 fájlban a változók hozzárendelt erőforrás csoportokat azoknak megfelelően. Az alábbi táblázat bemutatja, hogy mely paraméter fájlok melyik erőforráscsoport hivatkozást. Az *TS-adfs-terhelés-rg*, *TS-adfs-biztonsági-rg*, *TS-adfs-felveszi-rg*, *TS-adfs-adfs-rg*és *TS-adfs-proxy-rg* erőforrás csak a PowerShell-parancsprogramot használt és fordul elő a paraméter fájlokat.

  	|Erőforráscsoport|Paraméter fájlok|
  	|--------------|--------------|
  	|TS-adtrust-helyi-rg|parameters\onpremise\connection.Parameters.JSON<br /> parameters\onpremise\virtualMachines-ADDS.Parameters.JSON<br />parameters\onpremise\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\onpremise\virtualNetwork.Parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON
  	|TS-adtrust-hálózaton-rg|parameters\onpremise\connection.Parameters.JSON<br />parameters\azure\dmz-private.Parameters.JSON<br />parameters\azure\dmz-public.Parameters.JSON<br />parameters\azure\loadBalancer-biz.Parameters.JSON<br />parameters\azure\loadBalancer-Data.Parameters.JSON<br />parameters\azure\loadBalancer-Web.Parameters.JSON<br />parameters\azure\virtualMachines-ADDS.Parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.Parameters.JSON<br />parameters\azure\virtualNetwork-ADDS-DNS.Parameters.JSON<br />parameters\azure\virtualNetwork.Parameters.JSON<br />parameters\azure\virtualNetworkGateway.Parameters.JSON (*két előfordulás*)

    Ezenkívül állítsa át a konfigurációt a helyszíni és felhőbeli-összetevők, a [Megoldás-összetevők] ismertetett módon[ solution-components] szakaszban.

7. Nyisson meg egy Azure PowerShell ablakot, helyezze át a parancsfájlok mappába, és futtassa a következő parancsot:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Csere `<subscription id>` az Azure előfizetés azonosítójával.

    A `<location>`, például: Adja meg, az Azure régió `eastus` vagy `westus`.

    A `<mode>` paraméter a következő értékek egyike lehet:

    - `Onpremise`, és a szimulált a helyszíni környezet létrehozása.

    - `Infrastructure`, a VNet infrastruktúra létrehozása és Ugrás a mező a felhőben.

    - `CreateVpn`, Azure virtuális hálózati átjáró létre, és csatlakoztassa a helyszíni hálózaton.

    - `AzureADDS`, a ÖSSZEADJA kiszolgálók eljáró VMs Egyenletszerkesztővel, telepítse az Active Directory ezek VMs, és hozza létre a tartományt a felhőben.

    - `WebTier`, a webes hoz létre, amely az első csoportba tartozó VMs és terheléselosztó.

    - `Prepare`, amely hajt végre, a megelőző tevékenységek. **Az ajánlott a lehetőség, ha egy teljesen új telepítési készítésekor, és nem rendelkezik egy meglévő helyszíni infrastruktúra.**

    - `Workload`a vállalati verzió és az adatok réteg VMs létrehozása és terheléselosztóként. Ezek a VMs amelyek nem része a `Prepare` lehetőséget.

    >[AZURE.NOTE] Ha a `Prepare` beállítást választja, a parancsfájl több óráig tart befejezéséhez.

8.  Ha a minta helyszíni konfiguráció használata esetén:

    1. Az Ugrás párbeszédpanel (*TS-adtrust-kezelése-vm1* *TS-adtrust-biztonsági-rg* erőforrás csoportjának) csatlakozzon. Jelentkezzen be azzal *tesztfelhasználó nevet* jelszóval *AweS0me@PW*.

    2.  Az Ugrás párbeszédpanel nyisson meg egy RDP-munkamenetet a az első virtuális a *contoso.com* tartományban (a helyszíni tartomány). A virtuális IP-címe 192.168.0.4 rendelkezik. A felhasználónév szó *contoso\testuser* jelszóval *AweS0me@PW*.

    3. Töltse le a [Bejövő levelek-trust.ps1] [ incoming-trust] parancsfájl, és indítsa el a bejövő megbízhatósági kapcsolat létrehozása a *treyresearch.com* tartományból.

9. Ha a saját helyszíni infrastruktúra használja:

    1. Töltse le a [Bejövő levelek-trust.ps1] [ incoming-trust] parancsfájl.

    2. Szerkessze a parancsfájlt, és az értéket a `$TrustedDomainName` változó, írja be az egyéni tartomány nevét.

    3. Futtassa a.

10. Az Ugrás mezőből csatlakozás a első virtuális *treyresearch.com* tartományban (a tartomány a felhőben). A virtuális IP-címe 10.0.4.4 rendelkezik. A felhasználónév szó *treyresearch\testuser* jelszóval *AweS0me@PW*.

11. Töltse le a [kimenő trust.ps1] [ outgoing-trust] parancsfájl, és indítsa el a bejövő megbízhatósági kapcsolat létrehozása a *treyresearch.com* tartományból. Ha a saját helyszíni gépek használ, majd a Szerkesztés a parancsfájl először. Állítsa a `$TrustedDomainName` változó a helyszíni tartomány nevére, és adja meg a tartományhoz, az Active Directory tartományi szolgáltatások kiszolgálók az IP-címek a `$TrustedDomainDnsIpAddresses` változó.

12. Helyi számítógépen, hajtsa végre a [Meghatalmazás ellenőrzése] című témakörben ismertetett[ verify-a-trust] határozza meg, hogy a meghatalmazás megfelelően van beállítva a *contoso.com* és *treyresearch.com* tartományok között. Előfordulhat, hogy várnia befejezése az előző lépéseket, mielőtt a meghatalmazás tudja érvényesíteni után néhány perccel.

A hátralévő választható lépések bemutatják, hogyan határozza meg, hogy a tartománymeghatalmazási meg a várt módon működik-e. Ezeket a lépéseket kell, hogy egy fejlesztés géphez fér hozzá a Visual Studio telepítve.

1.  Az Azure PowerShell ablakból, futtassa a következő parancsot a webes réteg létrehozása: Ha nem állított azt be a korábban (használatával a `Prepare` beállítás):

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> WebTier
    ```

    Ez a parancs a webes réteg hoz létre, és felveszi a VMs *treyresearch.com* tartományt.

2. Visual Studio fejlesztési számítógépen, hoz létre elnevezett *TreyResearchWebApp*ASP.NET webalkalmazást. A .NET-keretrendszer 4.5.2 használja.

3. Jelölje ki a *MVC* sablont, és módosítsa a hitelesítés *Windows-hitelesítés használatával*. Az alkalmazás szolgáltatásainak nem hoz létre a felhőben.

3. Építse fel, és futtassa az alkalmazást, tesztelje a hitelesítés. Ellenőrizze, hogy a Windows aktuális felhasználónevét a menüsávon felé jobbra a lap tetején jelenik.

4. Zárja be az Internet Explorer.

5. A *Megoldás-kezelő* ablakban kattintson a jobb gombbal a TreyResearchWebApp projektet, kattintson a *Közzététel*gombra.

6. A *Webhely közzététele* ablakában kattintson az *egyéni*elemre. Névvel ellátott *TreyResearchWebApp*egyéni profil létrehozása.

7. A *kapcsolat* lapon állítsa a *Közzététel módszer* *Fájlrendszerben* , és adja meg a *TreyResearchWebApp*, egy helyen fejlesztési a számítógépen található mappába.

8. A *Beállítások* lapon állítsa a *konfigurációs* *kiadásra*.

9. A *kép* lapon kattintson a *Közzététel*gombra.

10. Minden egyes virtuális a webes réteg viszont (az Ugrás mező) keresztül csatlakozhat, és végezze el az alábbi műveleteket. Az IP-címek a Web VMs rétegben a következők: 10.0.1.4 és 10.0.1.5. Mindkét VMs a felhasználónév *treyresearch\testuser* jelszóval *AweS0me@PW*:

    1. Másolja a *TreyResearchWebApp* mappát és tartalmát a fejlesztői számítógép *C:\inetpub* mappába.

    2. Az Internet Information Services (IIS) Manager konzolon nyissa meg *Sites\Default webhely* azon a számítógépen.

    3. A *Műveletek* ablaktáblában *Alapvető beállításai*gombra, és a webhely fizikai elérési útjának *%SystemDrive%\inetpub\TreyResearchWebApp*módosítása.

    4. Kattintson duplán a *hitelesítési* *Szolgáltatások* nézete. Győződjön meg arról, hogy a *Windows-hitelesítés* van engedélyezve, és *Névtelen hitelesítés* le van tiltva.

11. egy helyi gépről nyissa meg az Internet Explorerben, és keresse meg a webhelyen http://10.0.1.254 (Ez az a cím, a webes réteg terheléselosztó).

12. A *Windows biztonsági* párbeszédpanelen adja meg a helyszíni tartomány felhasználói hitelesítő adatait. Adja meg a felhasználónevet, amely nem létezik a *fortuna* tartományban. Ha a szimulált helyszíni környezetben használja először a *contoso* tartományban található hozzon létre egy felhasználót, és adja meg a felhasználó hitelesítő adatait.

13. Amikor megjelenik a Kezdőlap lap, győződjön meg arról, hogy a megfelelő tartományt, és a felhasználónevet jelennek meg a menüsor felé jobbra a lap tetején.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók a gyakorlati tanácsok [a helyszíni ÖSSZEADJA a tartomány Azure bővítése][adds-extend-domain]

- További tudnivalók a gyakorlati tanácsok [az ADFS-infrastruktúrát létrehozásához] [ adfs] Azure-ban.

<!-- links -->

[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[adfs]: ./guidance-identity-adfs.md
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[implementing-aad]: ./guidance-identity-aad.md
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-hybrid-network-architecture-with-vpn]: ./guidance-hybrid-network-vpn.md
[running-VMs-for-an-N-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[azure-vpn-gateway]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-vpngateways/
[azure-expressroute]: https://azure.microsoft.com/documentation/articles/expressroute-introduction/
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[standby-operations-masters]: https://technet.microsoft.com/library/cc794737(v=ws.10).aspx
[best_practices_ad_password_policy]: https://technet.microsoft.com/magazine/ff741764.aspx
[monitoring_ad]: https://msdn.microsoft.com/library/bb727046.aspx
[microsoft_systems_center]: https://www.microsoft.com/server-cloud/products/system-center-2016/
[creating-forest-trusts]: https://technet.microsoft.com/library/cc816810(v=ws.10).aspx
[creating-external-trusts]: https://technet.microsoft.com/library/cc816837(v=ws.10).aspx
[naming-conventions]: ./guidance-naming-conventions.md
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[verify-a-trust]: https://technet.microsoft.com/library/cc753821.aspx
[netdom]: https://technet.microsoft.com/library/cc835085.aspx
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://github.com/mspnp/reference-architectures/blob/master/guidance-identity-adds-trust/parameters/onpremise/connection.parameters.json
[incoming-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/incoming-trust.ps1
[outgoing-trust]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/extensions/outgoing-trust.ps1
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adds-trust/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-adds.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/add-adds-domain-controller.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/dmz-public.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/virtualMachines-mgmt.parameters.json
[web-vm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adds-trust/parameters/azure/web-vm-domain-join.parameters.json
[solution-components]: [#solution_components]
[0]: ./media/guidance-identity-aad-resource-forest/figure1.png "Hibrid hálózat architektúrája külön Active Directory – tartományok és biztonságos"