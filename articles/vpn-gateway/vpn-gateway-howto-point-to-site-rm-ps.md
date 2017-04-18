<properties 
   pageTitle="Az erőforrás-kezelő telepítési modell használatáról virtuális hálózati átjáró pont-webhely virtuális Magánhálózati kapcsolat beállítása |} Microsoft Azure"
   description="Átjáró pont-webhely virtuális Magánhálózati kapcsolat létrehozásával, biztonságosan csatlakoztathassa a Azure virtuális hálózathoz."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc" />

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-powershell"></a>A PowerShell használatá VNet pont a webhely-kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasszikus - Azure portál](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

A pont-webhely (P2S) konfiguráció segítségével biztonságos kapcsolatot az egyes ügyfélszámítógép virtuális hálózathoz. P2S kapcsolat akkor lehet hasznos, amikor szeretne csatlakozni a VNet távoli helyről, például a kezdőlap vagy a konferenciához, vagy csak néhány ügyfelek, amely még nem csatlakozott a virtuális hálózathoz van. 

A kapcsolatok pont-webhely nem szükséges VPN-eszközhöz vagy egy nyilvános IP-cím használata. A virtuális Magánhálózati kapcsolat által a kapcsolat kezdve az ügyfélszámítógép létrejön. További információt a webhely-pont kapcsolatok című [VPN átjáró gyakran ismételt kérdések](vpn-gateway-vpn-faq.md#point-to-site-connections) [tervezése és kialakítása](vpn-gateway-plan-design.md). 

Ez a cikk végigvezeti elkészítésén egy VNet az erőforrás-kezelő telepítési modell PowerShell használatával pont a webhely-kapcsolatot.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Telepítési modellek és P2S kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a két környezetben modell és elérhető telepítési módszerek P2S konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Egyszerű munkafolyamat 

![Pont-a-webhely-diagram] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "pont-webhely")

Ebben az esetben létrehoz egy virtuális hálózati pont a webhely-kapcsolatot. Az útmutatást segítséget is a fenti konfiguráció szükséges tanúsítványokról készítése. Az alábbi elemek P2S kapcsolat áll: A VNet egy virtuális Magánhálózati átjárót, legfelső szintű tanúsítvány .cer fájl (nyilvános kulcs), ügyfél-tanúsítvány és a virtuális Magánhálózati konfiguráció az ügyfélgépen. 

Ebben a konfigurációban fogjuk kiszámolni az alábbi értékeket. Be van állítva a változók szakasz [1](#declare) olvashatók. Egy segédlet, hajtsa végre a lépéseket, és az használja az értékeket figyelmen kívül hagyásával őket, vagy módosíthatja a környezet megfelelően. 

### <a name="example"></a>Példa értékek

- **Név: VNet1**
- **Terület cím: 192.168.0.0/16** és **10.254.0.0/16**<br>Ebben a példában a egynél több címterület szemléltetéséhez, hogy ez a beállítás működjön, több címet szóközökkel használjuk. Jó helyen jár sem a fenti konfiguráció szükséges szóközöket címet.
- **Alhálózat neve: FrontEnd**
    - **Alhálózat címtartományokat: 192.168.1.0/24**
- **Alhálózat neve: Kódmentes**
    - **Alhálózat címtartományokat: 10.254.1.0/24**
- **Alhálózat neve: GatewaySubnet**<br>Az alhálózathoz *GatewaySubnet* neve kötelező, a virtuális Magánhálózati átjáró működik.
    - **Alhálózat címtartományokat: 192.168.200.0/24** 
- **Virtuális Magánhálózati ügyfél cím készlet: 172.16.201.0/24**<br>Az a pont-webhely kapcsolattal VNet csatlakozó VPN-ügyfelek IP-címet a virtuális Magánhálózati ügyfél cím készletből fogadása
- **Előfizetés:** Ha egynél több előfizetése van, ellenőrizze, hogy a megfelelő fiókot használ.
- **Erőforráscsoport: TestRG**
- **Hely: Kelet-Amerikai Egyesült Államok**
- **DNS-kiszolgáló: IP-cím** a DNS-kiszolgáló névfeloldás használni kívánt.
- **GW neve: Vnet1GW**
- **Nyilvános IP-neve: VNet1GWPIP**
- **VpnType: RouteBased**

## <a name="before-beginning"></a>Mielőtt elkezdené

- Ellenőrizze, hogy rendelkezik-e egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
    
- Telepítse az Azure erőforrás-kezelő PowerShell-parancsmagok legújabb verzióját. Megtudhatja, [hogy miként telepítheti, állíthatja Azure PowerShell](../powershell-install-configure.md) PowerShell-parancsmagok telepítésével kapcsolatos további információt. Amikor a PowerShell használata az ebben a konfigurációban, ellenőrizze, hogy futtatása rendszergazdaként. 

## <a name="declare"></a>1 - napló részt és változók beállítása

Ebben a részben jelentkezzen be, és ebben a konfigurációban használt értékek deklarálhatnak. A minta parancsfájlok használt feltüntetett értékeket. Változtassa meg a saját környezetben megfelelően. Vagy használja a megadott értéket, és mint gyakorlat végig, és lépjen.

1. A PowerShell konzolban jelentkezzen be az Azure-fiókjába. Ezzel a parancsmaggal, a bejelentkezési hitelesítő adatokat kér az Azure-fiók. Naplózás után letöltés a Fiókbeállítások úgy, hogy Azure PowerShell elérhetők legyenek.

        Login-AzureRmAccount 

2. Ismerkedés az Azure-előfizetések listáját.

        Get-AzureRmSubscription

3. Adja meg az előfizetést, szeretné használni. 

        Select-AzureRmSubscription -SubscriptionName "Name of subscription"

4. A használni kívánt változók az elemeket rekordként. Használja az alábbi példa a helyettesítése a saját, szükség esetén az értékeket.

        $VNetName  = "VNet1"
        $FESubName = "FrontEnd"
        $BESubName = "Backend"
        $GWSubName = "GatewaySubnet"
        $VNetPrefix1 = "192.168.0.0/16"
        $VNetPrefix2 = "10.254.0.0/16"
        $FESubPrefix = "192.168.1.0/24"
        $BESubPrefix = "10.254.1.0/24"
        $GWSubPrefix = "192.168.200.0/26"
        $VPNClientAddressPool = "172.16.201.0/24"
        $RG = "TestRG"
        $Location = "East US"
        $DNS = "8.8.8.8"
        $GWName = "VNet1GW"
        $GWIPName = "VNet1GWPIP"
        $GWIPconfName = "gwipconf"
        
## <a name="ConfigureVNet"></a>Része a 2 - egy VNet konfigurálása 

1. Hozzon létre egy erőforrás csoportot.

        New-AzureRmResourceGroup -Name $RG -Location $Location

2. Hozzon létre az alhálózathoz konfigurációk virtuális hálózatok, *FrontEnd*, *Kódmentes*és *GatewaySubnet*elnevezési őket. Ezek a prefixumokban a VNet címterületet, amely akkor deklarálva tagjának kell lennie.

        $fesub = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName -AddressPrefix $FESubPrefix
        $besub = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName -AddressPrefix $BESubPrefix
        $gwsub = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName -AddressPrefix $GWSubPrefix

3. A virtuális hálózat létrehozása. A megadott DNS-kiszolgáló, amely a csatlakozik az erőforrások nevei megoldhatja a DNS-kiszolgáló kell lennie. Ez a példa azt egy nyilvános IP-címet használja. Ne felejtse el a saját értékek használata.
    
        New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG -Location $Location -AddressPrefix $VNetPrefix1,$VNetPrefix2 -Subnet $fesub, $besub, $gwsub -DnsServer $DNS

4. Adja meg a virtuális hálózat létrehozott változói.

        $vnet = Get-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $RG
        $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet

5. Kérése egy dinamikusan hozzárendelt nyilvános IP-címet. Az IP-cím szükség az átjáró működik megfelelően. Később csatlakozni az átjárót a gateway IP-konfigurációja.
        
        $pip = New-AzureRmPublicIpAddress -Name $GWIPName -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
        $ipconf = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip

## <a name="Certificates"></a>3 - tanúsítványok kijelző

Tanúsítványok Azure által használt VPN-ügyfelek pont-webhely VPN hitelesítést végezni. Nyilvános tanúsítvány-adatok (nem a titkos kulcs) szerint az alap-64 kódolású X.509 .cer fájl egy vállalati tanúsítvány megoldás által generált legfelső szintű tanúsítvány vagy önaláírt legfelső szintű tanúsítvány exportálhatja. Kattintson a nyilvános tanúsítvány adatokat importál a legfelső szintű tanúsítvány az Azure. Ezenkívül kell ügyfél-tanúsítvány létrehozása a legfelső szintű tanúsítvány az ügyfelek számára. Minden ügyfél, hogy csatlakozzon a virtuális hálózathoz P2S kapcsolaton keresztül szeretne egy ügyfél tanúsítvány telepítve van a legfelső szintű tanúsítvány alapján jött létre kell rendelkeznie.

### <a name="cer"></a>1. a .cer fájl a legfelső szintű tanúsítvány beszerzése

Meg kell a nyilvános tanúsítvány adatok beolvasása a legfelső szintű tanúsítvány, amely a használni kívánt.

- Ha egy vállalati tanúsítvány rendszert használ, szerezze be a .cer fájl a legfelső szintű tanúsítvány, amely a használni kívánt. 

- Ha nem használ egy vállalati tanúsítvány megoldás, meg kell legfelső önaláírt tanúsítvány létrehozása. A Windows 10-es lépéseket is hivatkozhat [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md).


1. .Cer fájl szerezni egy tanúsítványt, nyissa meg a **certmgr.msc parancsot** , és keresse meg a legfelső szintű tanúsítvány. Kattintson a jobb gombbal a legfelső önaláírt tanúsítványt, válassza a **Minden tevékenység**, és kattintson az **Exportálás**parancsra. Ekkor megnyílik az **Exportálás varázsló tanúsítvány**.

2. A varázslóban kattintson a **Tovább**gombra, válassza a **nem, a titkos kulcs exportálni**, és kattintson a **Tovább gombra**.

3. A **Fájlformátumban exportálhatja** lapon jelölje ki az alap-64 **kódolású X.509 (. CER).** Kattintson a **Tovább gombra**. 

4. Kattintson az **exportálandó fájl**, **tallózással keresse meg** a tanúsítvány exportálása pontjára. A **fájl nevét**a tanúsítvány fájl nevét. Kattintson a **Tovább gombra**.

5. A tanúsítvány exportálása a **Befejezés** gombra.

### <a name="generate"></a>2. a ügyfél-tanúsítvány létrehozása

Ezután készítése az ügyfél tanúsítványok. Egyes fog csatlakozni ügyfél vagy hozhat létre egy egyedi tanúsítványt, vagy a tanúsítvány felhasználhatja több ügyfél. Az egyedi ügyfél tanúsítványok létrehozása előnye, hogy az azt jelenti, hogy egyetlen tanúsítvány visszavonni, ha szükséges. Egyéb esetben ha ugyanaz a ügyfél tanúsítvány mindenki használ, és megtalálta, amelyet fel kell-e a tanúsítványt egy ügyfél visszavonni, szüksége lesz hozhat létre, és telepítse az új tanúsítványokat összes az ügyfelek, amelyek a tanúsítvány hitelesítést végezni. Az ügyfél tanúsítványok minden ügyfélszámítógépen ebben a gyakorlatban a telepített.

- Egy vállalati tanúsítvány megoldás használatakor készítése a közös értéket névformátum ügyfél tanúsítvány 'name@yourdomain.com', helyett a "Tartomány\felhasználónév" NetBIOS formázása. 

- Ha önaláírt tanúsítványt használja, megnézheti, hogy [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md) ügyfél-tanúsítvány létrehozása.

### <a name="exportclientcert"></a>3. a ügyfél-tanúsítvány exportálása

Ügyfél-tanúsítvány szükség a hitelesítést. Miután az ügyfél-tanúsítvány létrehozása, exportálhatja. Az ügyfél-tanúsítvány exportálásának később telepíti minden ügyfélszámítógépen.

1. Ügyfél-tanúsítvány exportálásához *certmgr.msc parancsot*is használhatja. Kattintson a jobb gombbal az ügyfél tanúsítvány, amelyet exportálni, válassza a **Minden tevékenység**és majd az **Exportálás**gombra.
2. Az ügyfél-tanúsítvány exportálása a titkos kulccsal. Ez a *.pfx* fájl. Ellenőrizze, hogy ne feledje, hogy a jelszó (kulcs) ebben a tanúsítványban meg és rögzítése.

### <a name="upload"></a>4. a legfelső szintű tanúsítvány .cer fájl feltöltése

A tanúsítvány lecserélése az érték a saját neve a változó deklarálása:

        $P2SRootCertName = "Mycertificatename.cer"

Azure hozzáadása a legfelső szintű tanúsítvány nyilvános tanúsítvány adatait. Legfeljebb 20 legfelső szintű tanúsítványok fájlokat is feltölthet. Azure nem töltse fel a titkos kulcs a legfelső szintű tanúsítvány. Miután a .cer fájl feltöltésekor, Azure használja, amely a virtuális hálózati csatlakozás ügyfelek hitelesítést végezni. 

Cserélje ki a fájl elérési útját a saját, és futtassa a parancsmagok.
    
        $filePathForCert = "C:\cert\Mycertificatename.cer"
        $cert = new-object System.Security.Cryptography.X509Certificates.X509Certificate2($filePathForCert)
        $CertBase64 = [system.convert]::ToBase64String($cert.RawData)
        $p2srootcert = New-AzureRmVpnClientRootCertificate -Name $P2SRootCertName -PublicCertData $CertBase64

## <a name="creategateway"></a>Rész 4 – a virtuális Magánhálózati átjáró létrehozása

Állítsa be, és a virtuális hálózati átjáró létrehozása a VNet. A *-GatewayType* **Vpn** kell lennie, és a *-VpnType* **RouteBased**kell lennie. Ez lehet legfeljebb 45 percet igénybe vehet.

        New-AzureRmVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG `
        -Location $Location -IpConfigurations $ipconf -GatewayType Vpn `
        -VpnType RouteBased -EnableBgp $false -GatewaySku Standard `
        -VpnClientAddressPool $VPNClientAddressPool -VpnClientRootCertificates $p2srootcert

## <a name="clientconfig"></a>Kijelző 5 – a virtuális Magánhálózati ügyfél konfigurációs csomag letöltése

Azure P2S használatával csatlakozik ügyfelek ügyfél-tanúsítvány, mind a virtuális Magánhálózati konfigurációs ügyfélcsomag telepítése kell rendelkeznie. Windows-ügyfelek VPN ügyfél konfigurációs csomagok érhetők el. A virtuális Magánhálózati ügyfélcsomag konfigurálása a virtuális Magánhálózati ügyfélszoftver, amely a Windows rendszerbe épített és a VPN csatlakozni, amelyet meghatározott információkat tartalmaz. A csomag nem külön szoftver telepítése. Lásd: további információt a [Virtuális Magánhálózati átjáró – gyakori kérdések](vpn-gateway-vpn-faq.md#point-to-site-connections) .

1. Az átjáró létrehozása után letöltheti a konfigurációs ügyfélcsomag. Ebben a példában a csomag letölti a 64 bites ügyfelek számára. Ha szeretne a 32 bites ügyfélalkalmazás letöltése, a "Amd64" cserélje "x86". A virtuális Magánhálózati ügyfél letöltheti az Azure portál használatával is.

        Get-AzureRmVpnClientPackage -ResourceGroupName $RG `
        -VirtualNetworkGatewayName $GWName -ProcessorArchitecture Amd64

2. A PowerShell-parancsmag a hivatkozás URL-címet adja eredményül. Ez a példa milyen a visszaadott URL-cím néz ki:

        "https://mdsbrketwprodsn1prod.blob.core.windows.net/cmakexe/4a431aa7-b5c2-45d9-97a0-859940069d3f/amd64/4a431aa7-b5c2-45d9-97a0-859940069d3f.exe?sv=2014-02-14&sr=b&sig=jSNCNQ9aUKkCiEokdo%2BqvfjAfyhSXGnRG0vYAv4efg0%3D&st=2016-01-08T07%3A10%3A08Z&se=2016-01-08T08%3A10%3A08Z&sp=r&fileExtension=.exe"

3. Másolja és illessze be a hivatkozást, a függvény által visszaadott a csomag letöltése webböngészőben. Ezután telepítse a csomagot az ügyfélgépen. Ha egy SmartScreen előugró, kattintson a **További információt a**, majd a **Futtatás mégis** annak érdekében, hogy a csomag telepítéséhez.

4. Kattintson az ügyfélszámítógépen nyissa meg azt a **Hálózati beállításokat** , majd kattintson a **virtuális Magánhálózati**. Ekkor megjelenik a kapcsolat szerepel a listában. Akkor jelennek meg a virtuális hálózathoz kapcsolódik, és így néz Ez a példa hasonló: 

    ![VPN-ügyfél] (./media/vpn-gateway-howto-point-to-site-rm-ps/vpn.png "VPN-ügyfél")


## <a name="clientcertificate"></a>Az ügyfél-tanúsítvány telepítés 6 - kijelző

Az egyes ügyfélszámítógépekkel annak érdekében, hogy a hitelesítő kell rendelkeznie az ügyfél-tanúsítványt. Ha telepíti az ügyfél-tanúsítvány, szüksége lesz az ügyfél-tanúsítvány exportálásakor létrehozott jelszót.

1. Az ügyfélszámítógép a .pfx fájl másolása
2. Kattintson duplán a .pfx fájl kattintva telepítheti. Ne módosítsa a telepítés helyének.

## <a name="connect"></a>7 - rész Azure csatlakoztatása

1. A VNet az ügyfélszámítógépen csatlakozhat nyissa meg azt a virtuális Magánhálózati kapcsolatot, és keresse meg a az Ön által létrehozott virtuális Magánhálózati kapcsolatot. A virtuális hálózat nevével megegyező nevű azt. Kattintson a **Csatlakozás**gombra. Előugró üzenet jelenhet meg, amelyre hivatkozik a tanúsítvány használatával. Ez történik, ha kattintson a **Folytatás** magasabb szintű jogosultságokkal használni. 

2. A **kapcsolat** állapota lapon kattintson a **Csatlakozás** a kapcsolat indítása elemre. Ha **Tanúsítvány kiválasztása** képernyő jelenik meg, ellenőrizze, hogy az ügyfél tanúsítványát megjelenítő azt, amelyiket csatlakozni szeretne. Ha nem, jelölje ki a megfelelő tanúsítványt használja a lefelé mutató nyílra, és kattintson **az OK**gombra.

    ![Virtuális Magánhálózati kapcsolat-ügyfél] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Virtuális Magánhálózati kapcsolat-ügyfél")

3. Most már kell létrehozni a kapcsolatot.

    ![Kapcsolat létrehozása] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Kapcsolat létrehozása")

## <a name="verify"></a>8 rész – ellenőrizze a kapcsolat

1. Győződjön meg róla, hogy a virtuális Magánhálózati kapcsolat aktív, nyissa meg a rendszergazda jogú parancssort, és futtassa az *ipconfig/minden*.

2. Az eredmények megtekintéséhez. Figyelje meg, hogy a kapott IP-cím valamelyik belül a pont-webhely VPN ügyfél címet, amely a konfigurációban megadott címet. Az eredmény rendre valami ehhez hasonló:
    
        PPP adapter VNet1:
            Connection-specific DNS Suffix .:
            Description.....................: VNet1
            Physical Address................:
            DHCP Enabled....................: No
            Autoconfiguration Enabled.......: Yes
            IPv4 Address....................: 172.16.201.3(Preferred)
            Subnet Mask.....................: 255.255.255.255
            Default Gateway.................:
            NetBIOS over Tcpip..............: Enabled

## <a name="addremovecert"></a>Hozzáadása és eltávolítása a megbízható legfelső szintű tanúsítvány

Tanúsítványok használatával VPN-ügyfelek pont-webhely VPN hitelesítést végezni. Az alábbi lépésekkel haladjon végig hozzáadása és eltávolítása a legfelső szintű tanúsítványok. Base64 kódolású X.509 (.cer) fájl Azure beállításakor, a legfelső szintű tanúsítvány, amely a fájl megbízhatónak Azure is jelzi. 

Felvehet, és eltávolítása a megbízható legfelső szintű tanúsítványok PowerShell használatával, vagy az Azure-portálon. Ha meg szeretne ehhez az Azure portálon, nyissa meg a **virtuális hálózati átjáró > Beállítások > webhely pont-konfigurációs > legfelső szintű tanúsítványok**. Az alábbi lépésekkel ismerteti, hogy ezek a feladatok PowerShell használatával. 

### <a name="add-a-trusted-root-certificate"></a>A megbízható legfelső szintű tanúsítvány felvétele

Azure legfeljebb 20 megbízható legfelső szintű tanúsítvány .cer fájlokat adhat hozzá. A legfelső szintű tanúsítvány felvétele az alábbi lépésekkel.

1. Hozzon létre, és Felkészülés az új legfelső szintű tanúsítvány, amely az Azure szeretne hozzáadni. A nyilvános kulcs exportálását, alap-64 kódolású X.509 (. CER), és nyissa meg a szövegszerkesztőben. Csak az alább látható módon szakaszban Ezután másolja. 
 
    Másolja az értékeket, az alábbi példában látható módon:

    ![tanúsítvány] (./media/vpn-gateway-howto-point-to-site-rm-ps/copycert.png "tanúsítvány")
    
2. Adja meg a tanúsítvány neve és a főbb információk változóként. Az adatok cserélje ki a saját, ahogy az alábbi példában:

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"


3. Adja hozzá az új legfelső szintű tanúsítvány. Egyszerre csak egy tanúsítványt adhat hozzá.

        Add-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayname "VNet1GW" -ResourceGroupName "TestRG" -PublicCertData $MyP2SCertPubKeyBase64_2


4. Ellenőrizheti, hogy az új tanúsítvány hozzá lett adva megfelelően az alábbi parancsmag használatával.

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

### <a name="to-remove-a-trusted-root-certificate"></a>Ha el szeretne távolítani egy megbízható legfelső szintű tanúsítvány

Azure megbízható legfelső szintű tanúsítvány eltávolítható. Ha eltávolít egy megbízható tanúsítványt, az ügyfél-tanúsítványok a legfelső szintű tanúsítvány előállított már nem tud csatlakozni az Azure pont-webhelyen keresztül. Ha azt szeretné, hogy ügyfeleknek való csatlakozást, telepíteniük kell a tanúsítvány megbízható Azure-ban létrehozott új ügyfél-tanúsítványt.

1. Ha el szeretne távolítani egy megbízható legfelső szintű tanúsítvány, az alábbi példa módosítása:

    A változók az elemeket rekordként.

        $P2SRootCertName2 = "ARMP2SRootCert2.cer"
        $MyP2SCertPubKeyBase64_2 = "MIIC/zCCAeugAwIBAgIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAMBgxFjAUBgNVBAMTDU15UDJTUm9vdENlcnQwHhcNMTUxMjE5MDI1MTIxWhcNMzkxMjMxMjM1OTU5WjAYMRYwFAYDVQQDEw1NeVAyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAyjIXoWy8xE/GF1OSIvUaA0bxBjZ1PJfcXkMWsHPzvhWc2esOKrVQtgFgDz4ggAnOUFEkFaszjiHdnXv3mjzE2SpmAVIZPf2/yPWqkoHwkmrp6BpOvNVOpKxaGPOuK8+dql1xcL0eCkt69g4lxy0FGRFkBcSIgVTViS9wjuuS7LPo5+OXgyFkAY3pSDiMzQCkRGNFgw5WGMHRDAiruDQF1ciLNojAQCsDdLnI3pDYsvRW73HZEhmOqRRnJQe6VekvBYKLvnKaxUTKhFIYwuymHBB96nMFdRUKCZIiWRIy8Hc8+sQEsAML2EItAjQv4+fqgYiFdSWqnQCPf/7IZbotgQIDAQABo00wSzBJBgNVHQEEQjBAgBAkuVrWvFsCJAdK5pb/eoCNoRowGDEWMBQGA1UEAxMNTXlQMlNSb290Q2VydIIQKazxzFjMkp9JRiX+tkTfSzAJBgUrDgMCHQUAA4IBAQA223veAZEIar9N12ubNH2+HwZASNzDVNqspkPKD97TXfKHlPlIcS43TaYkTz38eVrwI6E0yDk4jAuPaKnPuPYFRj9w540SvY6PdOUwDoEqpIcAVp+b4VYwxPL6oyEQ8wnOYuoAK1hhh20lCbo8h9mMy9ofU+RP6HJ7lTqupLfXdID/XevI8tW6Dm+C/wCeV3EmIlO9KUoblD/e24zlo3YzOtbyXwTIh34T0fO/zQvUuBqZMcIPfM1cDvqcqiEFLWvWKoAnxbzckye2uk1gHO52d8AVL3mGiX8wBJkjc/pMdxrEvvCzJkltBmqxTM6XjDJALuVh16qFlqgTWCIcb7ju"

2. A tanúsítvány eltávolítása

        Remove-AzureRmVpnClientRootCertificate -VpnClientRootCertificateName $P2SRootCertName2 -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -PublicCertData $MyP2SCertPubKeyBase64_2
 
3. Az alábbi parancsmag használatával ellenőrizze, hogy a tanúsítvány sikeresen eltávolította. 

        Get-AzureRmVpnClientRootCertificate -ResourceGroupName "TestRG" `
        -VirtualNetworkGatewayName "VNet1GW"

## <a name="revoke"></a>A visszavont ügyfél tanúsítványok listájának kezelése

Ügyfél-tanúsítványok vonhatja meg. A tanúsítvány-visszavonási lista lehetővé teszi a szelektív megtagadja az egyéni ügyfél tanúsítványok alapuló webhely pont-kapcsolatot. Ha eltávolítja a legfelső szintű tanúsítvány .cer Azure, akkor az access összes ügyfél tanúsítványok generált/aláírt a visszavont legfelső szintű tanúsítvány által visszavonása. Ha szeretne visszavonni egy adott ügyfél tanúsítványt, nem a legfelső szintű, megteheti. Úgy, hogy a tanúsítványok, a legfelső szintű tanúsítvány előállított továbbra is érvényes.

A közös gyakorlat a legfelső szintű tanúsítvány használatával csoport- vagy szervezeti szintű hozzáférés kezelése a külön hozzáférés vezérléséhez az egyéni felhasználók a visszavont ügyfél tanúsítványok használatakor.

### <a name="revoke-a-client-certificate"></a>Ügyfél-tanúsítvány visszavonása

1. Ismerkedés a ujjlenyomat vissza szeretné vonni az ügyfél tanúsítvány.

        $RevokedClientCert1 = "ClientCert1"
        $RevokedThumbprint1 = "‎ef2af033d0686820f5a3c74804d167b88b69982f"

2. Az ujjlenyomatot hozzáadása a visszavont ujjlenyomat listáját.

        Add-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

3. Győződjön meg arról, hogy az ujjlenyomatot hozzáadva a tanúsítvány-visszavonási lista. Egyszerre egy ujjlenyomat fel kell vennie.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

### <a name="reinstate-a-client-certificate"></a>Ügyfél-tanúsítvány visszaállítása

Ügyfél-tanúsítvány visszaállíthatja az ujjlenyomatot eltávolításával visszavont ügyfél tanúsítványok a listából.

1.  Távolítsa el az ujjlenyomatot visszavont ügyfél tanúsítvány ujjlenyomat listáját.

        Remove-AzureRmVpnClientRevokedCertificate -VpnClientRevokedCertificateName $RevokedClientCert1 `
        -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG -Thumbprint $RevokedThumbprint1

2. Annak ellenőrzése, ha az ujjlenyomatot eltávolítja a visszavont listában.

        Get-AzureRmVpnClientRevokedCertificate -VirtualNetworkGatewayName $GWName -ResourceGroupName $RG

## <a name="next-steps"></a>Következő lépések

A virtuális hálózat virtuális gép vehet. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.