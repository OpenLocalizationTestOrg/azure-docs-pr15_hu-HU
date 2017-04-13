<properties 
   pageTitle="Egy virtuális hálózati csatlakozás virtuális Magánhálózati átjáró és a PowerShell használatával több webhelyen |} Microsoft Azure"
   description="Ez a cikk azt ismerteti, hogy több helyi a helyszíni webhely virtuális hálózathoz csatlakozik egy virtuális Magánhálózati átjárót használata a klasszikus telepítési modell keresztül."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/11/2016"
   ms.author="yushwang" />

# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Webhely kapcsolat hozzáadása egy VNet VPN-átjáró meglévő kapcsolattal

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - portál](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasszikus - PowerShell](vpn-gateway-multi-site.md)

Ez a cikk végigvezeti webhely (S2S) kapcsolatok hozzáadása egy virtuális Magánhálózati átjárót, amely tartalmaz egy meglévő kapcsolat a PowerShell használatával. Az ilyen típusú kapcsolat gyakran a "több webhelyen" konfiguráció nevezik. 

Ez a cikk a klasszikus telepítési modell (más néven Szolgáltatáskezelés) alapján létre virtuális hálózatok vonatkozik. Ezeket a lépéseket nem vonatkoznak készült ExpressRoute /-webhelyek coexisting csatlakozási beállításokat. Lásd: a [készült ExpressRoute/S2S coexisting kapcsolatok](../expressroute/expressroute-howto-coexist-classic.md) coexisting kapcsolatok információt.

### <a name="deployment-models-and-methods"></a>Telepítési modellek és módszerek

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Az alábbi táblázat frissítjük új cikkekben, és további eszközök lesznek elérhetők, ebben a konfigurációban. Ha egy cikk áll rendelkezésre, azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 



## <a name="about-connecting"></a>Kapcsolódás

Több a helyszíni webhely csatlakoztathatja virtuális egyetlen hálózat. Ez a különösen vonzó a hibrid felhő megoldások fejlesztésére. Az Azure virtuális hálózati átjáró több webhelyen kapcsolat létrehozása nagyon hasonlít más hely közötti kapcsolatok létrehozását. Erre valójában használhat egy meglévő Azure virtuális Magánhálózati átjáró mindaddig, amíg az átjáró dinamikus (útvonal-alapú).

Ha már van egy statikus átjáró a virtuális hálózathoz csatlakozik, módosíthatja az átjáró típusú dinamikus anélkül, hogy a virtuális hálózat újraépítéséhez annak érdekében, hogy beleférjenek több webhelyen. Mielőtt módosítja a útválasztási típusa, győződjön meg arról, hogy a helyszíni virtuális Magánhálózati átjáró támogatja-e VPN konfigurációk útvonal-alapú. 

![több elem webhely diagramja] (./media/vpn-gateway-multi-site/multisite.png "több webhelyen")

## <a name="points-to-consider"></a>Megfontolandó szempontok

**Nem használhatja az Azure klasszikus portál virtuális hálózat szeretne változtatásokat végezni.** Ebben a kiadásban kell a hálózati konfigurációs fájl helyett az Azure klasszikus portálon végezze el a módosításokat. Ha Ön az Azure klasszikus portálon, fog felülírja a virtuális hálózat több webhelyre hivatkozási beállítások. 

Úgy érzi meglehetősen kényelmes a hálózati konfigurációs fájl használatával az időt, a több elem webhely eljárás végrehajtása után. Jó helyen jár Ha több személy dolgozik, a hálózati konfigurálásról, kell győződjön meg arról, hogy mindenki tud arról, hogy ezt a korlátozást. Ez nem jelenti azt, hogy egyáltalán nem használható az Azure klasszikus portálon. Használhatja az, hogy milyen egyéb, kivéve a konfigurációs módosítást hajtana végre adott virtuális hálózathoz.

## <a name="before-you-begin"></a>Első lépések

Konfigurációs megkezdése előtt ellenőrizze, hogy az alábbi:

- Egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési be egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).

- Kompatibilis VPN hardver minden egyes helyszíni helyét. Jelölje be a [VPN eszközök kapcsolatos virtuális a hálózati kapcsolat](vpn-gateway-about-vpn-devices.md) ellenőrizze a használni kívánt eszköz-e egy üzenetet úgy, hogy kompatibilis ismert.

- Külső felhasználókkal szemben lévő nyilvános IPv4 IP-címének minden VPN eszközt. Az IP-cím nem található meg az mögött egy hálózati címfordítást. Ez a követelmény.

- Telepítse az Azure PowerShell-parancsmagok legújabb verzióját kell. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt.

- Személyt jártassággal a virtuális Magánhálózati hardver konfigurálása elemre. A virtuális Magánhálózati automatikusan létrehozott parancsfájlok az Azure klasszikus portálról használhatja a virtuális Magánhálózati eszközök beállítása nem. Ez azt jelenti, hogy a virtuális Magánhálózati eszköz beállítása és használata olyannak, aki jelent erős megértése.

- Az IP-címtartományok (Ha még nem már létrehozott egy) a virtuális hálózathoz használni kívánt. 

- Az IP-címtartományok az egyes a helyi hálózati helyek, amely meg fog csatlakozni. Győződjön meg arról, hogy az IP-cím csak jelszóval módosítható tartományok az egyes a helyi hálózati helyek nem áll átfedésben csatlakozni, amelyet kell. Az Azure klasszikus portálon vagy a REST API-t, elutasítja az feltöltése folyamatban konfigurációját. 

    Ha például ha két helyi hálózati helyek is tartalmazza az IP-cím tartomány 10.2.3.0/24, és a célként megadott címmel 10.2.3.3 csomaggal rendelkezik, Azure nem tudja, milyen el szeretné küldeni a csomag, mivel a címtartományokat átfedi webhelyet. Útválasztási problémák megelőzése érdekében Azure nem engedélyezi átfedő tartományokat tartalmazó konfigurációs fájl feltöltése.



## <a name="1-create-a-site-to-site-vpn"></a>1. a webhely VPN létrehozása

Ha már van egy webhely VPN dinamikus útválasztási átjáró nagy! [A virtuális hálózati kereséskonfigurációs beállítások](#export)exportálása folytathatja. Ha nem, akkor tegye a következőket:

### <a name="if-you-already-have-a-site-to-site-virtual-network-but-it-has-a-static-policy-based-routing-gateway"></a>Ha már rendelkezik webhely virtuális hálózatot, de statikus (Csoportházirend-alapú) útválasztási az átjárók van:

1. A dinamikus útválasztás módosíthatja az átjáró típusát. Több elem webhely virtuális Magánhálózattal dinamikus (más néven útvonal-alapú) útválasztási az átjárók szükséges. Ha módosítani szeretné az átjáró, kell először törölje a meglévő átjáró, majd hozzon létre egy újat. Útmutatásért lásd: [a virtuális Magánhálózati útválasztási írja be az átjáró módosítása](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md#how-to-change-the-vpn-routing-type-for-your-gateway).  

2. Az új átjáró beállítása és a VPN-csatorna létrehozása. Útmutatásért lásd: a [Configure egy virtuális Magánhálózati átjárót, az Azure klasszikus portálon](vpn-gateway-configure-vpn-gateway-mp.md). Első lépésként az átjáró típusának módosítása dinamikus útválasztás. 

### <a name="if-you-dont-have-a-site-to-site-virtual-network"></a>Ha nem rendelkezik webhely virtuális hálózat:

1. Hálózatot hozhat létre, a webhely virtuális ezeket az utasításokat: [létrehozása egy virtuális hálózati hely közötti virtuális Magánhálózati kapcsolatot az Azure klasszikus portálon](vpn-gateway-site-to-site-create.md).  

2. Adjon meg egy dinamikus útválasztási átjárót, használja az alábbi útmutatást: [beállítása egy virtuális Magánhálózati átjárót](vpn-gateway-configure-vpn-gateway-mp.md). Győződjön meg arról, jelölje be a **dinamikus útválasztás** az átjáró típusát.

## <a name="export"></a>2. a hálózati konfigurációs fájl exportálása 

A hálózati konfigurációs fájl exportálása A fájlt, exportálásának használandó az új több vonatkozó beállítások megadása. Ha egy fájl exportálására vonatkozó útmutatást, a című témakört a: [hogyan hozhat létre egy VNet az Azure-portálon hálózati konfigurációs fájl használatával](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="3-open-the-network-configuration-file"></a>3. a hálózati konfigurációs fájl megnyitása

Nyissa meg a hálózati konfigurációs fájl letöltött az utolsó lépésben. Bármely, például az XML-szerkesztő használata. A fájl a következőhöz hasonlóan kell kinéznie:

        <NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <LocalNetworkSites>
              <LocalNetworkSite name="Site1">
                <AddressSpace>
                  <AddressPrefix>10.0.0.0/16</AddressPrefix>
                  <AddressPrefix>10.1.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.2.3.4</VPNGatewayAddress>
              </LocalNetworkSite>
              <LocalNetworkSite name="Site2">
                <AddressSpace>
                  <AddressPrefix>10.2.0.0/16</AddressPrefix>
                  <AddressPrefix>10.3.0.0/16</AddressPrefix>
                </AddressSpace>
                <VPNGatewayAddress>131.4.5.6</VPNGatewayAddress>
              </LocalNetworkSite>
            </LocalNetworkSites>
            <VirtualNetworkSites>
              <VirtualNetworkSite name="VNet1" AffinityGroup="USWest">
                <AddressSpace>
                  <AddressPrefix>10.20.0.0/16</AddressPrefix>
                  <AddressPrefix>10.21.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="FE">
                    <AddressPrefix>10.20.0.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="BE">
                    <AddressPrefix>10.20.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="GatewaySubnet">
                    <AddressPrefix>10.20.2.0/29</AddressPrefix>
                  </Subnet>
                </Subnets>
                <Gateway>
                  <ConnectionsToLocalNetwork>
                    <LocalNetworkSiteRef name="Site1">
                      <Connection type="IPsec" />
                    </LocalNetworkSiteRef>
                  </ConnectionsToLocalNetwork>
                </Gateway>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

## <a name="4-add-multiple-site-references"></a>4. a több mutató hivatkozások hozzáadása

Hozzáadásakor vagy webhelyre hivatkozási adatok eltávolítása, akkor fogja módosíthatja konfigurációs ConnectionsToLocalNetwork/LocalNetworkSiteRef. Egy új helyi hivatkozás indítók Azure hozhat létre egy új alagutas hozzáadása. Az alábbi példában a hálózati konfigurálásról egyetlen helyet kapcsolat szolgál. Amikor befejezte a módosítások elvégzése, mentse a fájlt.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

    To add additional site references (create a multi-site configuration), simply add additional "LocalNetworkSiteRef" lines, as shown in the example below: 

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="Site1"><Connection type="IPsec" /></LocalNetworkSiteRef>
            <LocalNetworkSiteRef name="Site2"><Connection type="IPsec" /></LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

## <a name="5-import-the-network-configuration-file"></a>5. a hálózati konfigurációs fájl importálása

A hálózati konfigurációs fájl importálása. Importálja a fájlt a módosításokat, az új alagutakat kerüljön. A alagutak a dinamikus átjáró, amely a korábban létrehozott fogja használni. Útmutatást a fájl importálása van szüksége, ha a című témakört: [hogyan hozhat létre egy VNet az Azure-portálon hálózati konfigurációs fájl használatával](../virtual-network/virtual-networks-create-vnet-classic-portal.md#how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal). 

## <a name="6-download-keys"></a>6. billentyűk letöltése

Miután felvette a új alagutak, használja a PowerShell-parancsmag `Get-AzureVNetGatewayKey` minden alagutas előre megosztott IPsec/IKE billentyűparancsok eléréséhez.

Példa:

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site1"

    Get-AzureVNetGatewayKey –VNetName "VNet1" –LocalNetworkSiteName "Site2"

Ha inkább, az *Első virtuális hálózati átjáró megosztott kulcs* REST API-t is használhatja az előre megosztott kulcsok első.

## <a name="7-verify-your-connections"></a>7. a kapcsolatok ellenőrzése

Több elem webhely alagutas állapotának ellenőrzéséhez. Minden egyes alagutas billentyűparancsai a letöltés után célszerű kapcsolatok ellenőrzése. Használat `Get-AzureVnetConnection` szeretne lista beszerzése az virtuális hálózati alagutak, az alábbi példában látható módon. VNet1 pedig a VNet neve.

    Get-AzureVnetConnection -VNetName VNET1
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 661530
    IngressBytesTransferred   : 519207
    LastConnectionEstablished : 5/2/2014 2:51:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site1' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site1
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7f68a8e6-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded
        
    ConnectivityState         : Connected
    EgressBytesTransferred    : 789398
    IngressBytesTransferred   : 143908
    LastConnectionEstablished : 5/2/2014 3:20:40 PM
    LastEventID               : 23401
    LastEventMessage          : The connectivity state for the local network site 'Site2' changed from Not Connected to Connected.
    LastEventTimeStamp        : 5/2/2014 2:51:40 PM
    LocalNetworkSiteName      : Site2
    OperationDescription      : Get-AzureVNetConnection
    OperationId               : 7893b329-51e9-9db4-88c2-16b8067fed7f
    OperationStatus           : Succeeded

## <a name="next-steps"></a>Következő lépések

VPN átjárók kapcsolatos további információért olvassa el a [VPN átjárók](../vpn-gateway/vpn-gateway-about-vpngateways.md)című témakört.

