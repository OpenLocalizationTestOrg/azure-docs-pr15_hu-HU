<properties 
    pageTitle="Az alkalmazás környezetek réteges biztonsági architektúrája" 
    description="A végrehajtási egy réteges biztonsági architektúrája, az alkalmazás-szolgáltatási környezetben." 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Végrehajtási egy réteges biztonsági architektúrája, az alkalmazás környezetek

## <a name="overview"></a>– Áttekintés ##
 
Alkalmazás környezetek egy virtuális hálózatba rendszerbe elszigetelt futtatókörnyezet környezetet biztosít, mivel a fejlesztők számára a különböző hálózati hozzáférési szinteket kezeléséről minden fizikai alkalmazás réteg réteges biztonsági architektúrája hozhat létre.

Közös óhaját API vissza részek Internet általános hozzáféréstől elrejtése lehetőséget, és csak a felsőbb szintű web Apps alkalmazások által hívandó API-k engedélyezése is.  [Biztonsági csoportok (NSGs) hálózati] [ NetworkSecurityGroups] használható nyilvános elérésének korlátozására API-alkalmazások alhálózat tartalmazó alkalmazás szolgáltatási környezetben.

Az az alábbi ábrán egy példa architektúra egy alapú WebAPI alkalmazással az alkalmazás-szolgáltatási környezetben rendszerre telepíthető.  Három külön web app-példányok, a három külön alkalmazás szolgáltatási környezetben üzembe hívásokat háttéradatbázist az azonos WebAPI alkalmazásba.

![Magyarázó rész architektúra][ConceptualArchitecture] 

A zöld pluszjelre azt jelzik, hogy a hálózati biztonsági csoport az "apiase" tartalmazó alhálózat lehetővé teszi, hogy a felsőbb szintű webalkalmazásokat, a bejövő hívások jól hívások belőle, mint.  Azonban a az azonos hálózati biztonsági csoport kifejezetten megtagadja a hozzáférést az általános bejövő forgalom az internetről. 

Ez a témakör a többi végigvezeti a hálózati biztonsági csoport konfigurálása az "apiase" tartalmazó alhálózat szükséges lépéseket.

## <a name="determining-the-network-behavior"></a>Annak megállapítása, a kapcsolati működésének beállítása ##
Annak érdekében, hogy tudja, hogy milyen hálózati biztonsági szabályokra van szükség, meg kell határoznia, mely hálózati ügyfelek lesz jogosult az alkalmazás-szolgáltatási környezetben, az API-alkalmazás tartalmazó eléréséhez, és mely ügyfelek, azzal blokkolhatja.

[Hálózati biztonsági csoportok (NSGs)] óta[ NetworkSecurityGroups] alkalmazva vannak a alhálózat, és az alkalmazás szolgáltatási környezetben van telepítve, az alhálózathoz, egy NSG foglalt szabályok alkalmazása az **összes** alkalmazás futó alkalmazás szolgáltatási környezetben.  A minta architektúra ebben a cikkben használata után a "apiase" tartalmazó alhálózat alkalmazott hálózati biztonsági csoport esetén minden alkalmazás "apiase" alkalmazás szolgáltatási környezetben futó fogja védeni ugyanazokat a szabályokat. 

- **Felsőbb szintű hívók kimenő IP-címének megállapításához:**  Mi az a IP-címe vagy a felsőbb szintű hívók?  Ezek a címek kifejezetten az access az használható a NSG lesz szüksége.  Alkalmazás környezetek közötti hívások ilyenként "Internetes" hívásokat, mivel ez azt jelenti, a kimenő IP-cím hozzárendelt a három felsőbb szintű alkalmazás szolgáltatási környezetben megengedettek az access a NSG a "apiase" alhálózat szükséges.   További információt a kimenő IP-címe meghatározásában, az alkalmazás-szolgáltatási környezetben futó alkalmazások című témakör tartalmaz a [Hálózat architektúrája] [ NetworkArchitecture] áttekintő cikk.
- **A háttéradatbázist API-alkalmazás kell hívni magát?**  Időnként kihagyott és finom pont az alkalmazási példát, ahol a háttéradatbázist alkalmazást kell hívhatja saját magát.  Ha egy alkalmazás szolgáltatási környezetben a háttéradatbázist API alkalmazás hívhatja saját magát, ezt is van-e kezelni egy "Internet" hívásként.  A minta architektúra ehhez a "apiase" alkalmazás szolgáltatási környezetben, valamint kimenő IP-címének a hozzáférést.

## <a name="setting-up-the-network-security-group"></a>A hálózati biztonsági csoport beállítása ##
Miután a kimenő IP-címek készlete ismertek, a következő lépésként hálózati biztonsági csoport létrehozása.  Az erőforrás-kezelő mindkét virtuális hálózatok, valamint a klasszikus virtuális hálózatok-alapú hálózati biztonsági csoportokat hozhat létre.  Az alábbi példákat megjelenítése létrehozása és konfigurálása az NSG a hálózaton klasszikus virtuális Powershell használatával.

A minta architektúra a környezetben található Dél központi megfelel, így egy üres NSG hoz létre az adott régióban:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

Először explicit engedélyezése szabály ad hozzá az Azure kezelési infrastruktúra [bejövő forgalmat] a cikkben leírt módon[ InboundTraffic] alkalmazás szolgáltatási környezetben.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
Ezután két szabályok hozzáadva lehetővé teszi a az első felsőbb szintű alkalmazás szolgáltatás-környezetből (a "fe1ase") a HTTP és HTTPS-hívások.

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Le, és ismételje meg a második és harmadik felsőbb szintű alkalmazás szolgáltatási környezetben ("fe2ase" és "fe3ase").

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Végezetül hozzáférést a háttéradatbázist API alkalmazás szolgáltatási környezetben kimenő IP-címét, hogy akkor is visszahívhatja be magát.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Nincs más hálózati biztonsági szabályok kell beállítania mivel minden NSG rendelkezik, amely az internetről bejövő hozzáférésének letiltása az alapértelmezett szabályhalmaz alapértelmezés szerint.

A teljes listát az szabályok a hálózati biztonsági csoport alatt jelennek meg.  Fontos tudni, hogyan a utolsó szabályt, amelyen ki van emelve, blokkolja a bejövő eltérő azokat, amelyeket kifejezetten hozzáférést kapott valamennyi hívók hozzáférést.

![NSG konfigurálása][NSGConfiguration] 

Az utolsó lépésként a NSG alkalmazása a alhálózat, amely tartalmazza az "apiase" alkalmazás szolgáltatási környezetben.  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Az alhálózathoz alkalmazott NSG, a csak a három felsőbb szintű alkalmazás szolgáltatási környezetben, és az alkalmazás-szolgáltatási környezetben, a API háttéradatbázist tartalmazó engedélyezett hívja fel a "apiase" környezetbe.


## <a name="additional-links-and-information"></a>További hivatkozások és információk ##
Az összes cikk, és hogyan – a hangpostáján az alkalmazás-szolgáltatási környezetben érhetők el az [alkalmazás-szolgáltatási környezetben – fontos fájl](../app-service/app-service-app-service-environments-readme.md).

Információk a [hálózati biztonsági csoportokat](../virtual-network/virtual-networks-nsg.md). 

[Kimenő IP-címek] ismertetése[ NetworkArchitecture] és alkalmazás szolgáltatási környezetben.

[Hálózati portok] [ InboundTraffic] alkalmazás szolgáltatási környezetben használja.

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
