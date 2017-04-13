<properties 
   pageTitle="Az erőforrás-kezelő telepítési modell és az Azure portal segítségével virtuális hálózati átjáró pont-webhely virtuális Magánhálózati kapcsolat beállítása |} Microsoft Azure"
   description="Az erőforrás-kezelő és az Azure portal pont-webhely VPN átjáró kapcsolat létrehozásával, biztonságosan csatlakoztathassa a Azure virtuális hálózathoz."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Egy az Azure-portálon VNet pont a webhely-kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasszikus - Azure portál](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

A pont-webhely (P2S) konfiguráció segítségével biztonságos kapcsolatot az egyes ügyfélszámítógép virtuális hálózathoz. P2S kapcsolat akkor lehet hasznos, amikor szeretne csatlakozni a VNet távoli helyről, például a kezdőlap vagy a konferenciához, vagy csak néhány ügyfelek, amely még nem csatlakozott a virtuális hálózathoz van. 

A kapcsolatok pont-webhely nem szükséges VPN-eszközhöz vagy egy nyilvános IP-cím használata. A virtuális Magánhálózati kapcsolat által a kapcsolat kezdve az ügyfélszámítógép létrejön. További információt a webhely-pont kapcsolatok című [VPN átjáró gyakran ismételt kérdések](vpn-gateway-vpn-faq.md#point-to-site-connections) [tervezése és kialakítása](vpn-gateway-plan-design.md).

Ez a cikk végigvezeti a VNet létrehozása az Azure portálon, az erőforrás-kezelő telepítési modell pont a webhely-kapcsolatot.

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Telepítési modellek és P2S kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a két környezetben modell és elérhető telepítési módszerek P2S konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Egyszerű munkafolyamat 

![Pont-a-webhely-diagram] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "pont-webhely")

### <a name="example"></a>Példa értékek

- **Név: VNet1**
- **Hely címe: 192.168.0.0/16**<br>Ez a példa azt csak egy címterület használja. Egynél több címterület a VNet számára is.
- **Alhálózat neve: FrontEnd**
- **Alhálózat címtartományokat: 192.168.1.0/24**
- **Előfizetés:** Ha egynél több előfizetése van, ellenőrizze, hogy a megfelelő fiókot használ.
- **Erőforráscsoport: TestRG**
- **Hely: Kelet-Amerikai Egyesült Államok**
- **GatewaySubnet: 192.168.200.0/24**
- **Virtuális hálózati átjáró neve: VNet1GW**
- **Átjáró típusa: VPN**
- **VPN típusa: útvonal-alapú**
- **Nyilvános IP-cím: VNet1GWpip**
- **Kapcsolat típusa: pont-webhely**
- **Ügyfél cím készlet: 172.16.201.0/24**<br>Az a pont-webhely kapcsolattal VNet csatlakozó VPN-ügyfelek kapnia az ügyfelek címét készlet IP-címet.

## <a name="before-beginning"></a>Mielőtt elkezdené

- Ellenőrizze, hogy rendelkezik-e egy Azure-előfizetést. Ha még nem rendelkezik az Azure előfizetéssel, is aktiválhatja az [MSDN előfizetői előnyökkel jár](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) , azaz a bejelentkezési felfelé [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
    
## <a name="createvnet"></a>1 rész - virtuális hálózat létrehozása

Ha ez a beállítás, mint egy torna hoz létre, akkor is hivatkozhat [példaértékeket](#example).

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

### <a name="2-add-additional-address-space-and-subnets"></a>2. további Címterület és alhálózat hozzáadása

Akkor is adhat további Címterület és alhálózat a VNet létrehozása után.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

### <a name="3-create-a-gateway-subnet"></a>3. a átjáró alhálózat létrehozása

Mielőtt az átjárók a virtuális hálózathoz csatlakozik, először kell a virtuális hálózat, amelyhez csatlakozni az átjárót alhálózat létrehozása. Ha lehetséges érdemes létrehozni egy átjáró alhálózat alapcím CIDR /28 vagy /27 elég IP-címek további jövőbeli konfigurációs követelmények igazodik biztosítása érdekében.

Ebben a szakaszban a képernyőképek egy hivatkozás példaként állnak rendelkezésre. Ne felejtse el a megfelelő GatewaySubnet címtartományokat használata a konfigurációban a szükséges értékeket.

#### <a name="to-create-a-gateway-subnet"></a>Átjáró alhálózat létrehozása

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

### <a name="dns"></a>4. Adja meg a DNS-kiszolgáló (nem kötelező)

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## <a name="creategw"></a>2 rész – ezek olyan virtuális hálózati átjáró létrehozása

Webhely-pont kapcsolatok csak az alábbi beállításokat:

- Átjáró típusa: VPN
- VPN típusa: útvonal-alapú

### <a name="to-create-a-virtual-network-gateway"></a>A virtuális hálózati átjáró létrehozása

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## <a name="generatecert"></a>Része a 3 - tanúsítványok létrehozása

Tanúsítványok Azure által használt VPN-ügyfelek pont-webhely VPN hitelesítést végezni. Nyilvános tanúsítvány-adatok (nem a titkos kulcs) szerint az alap-64 encoded egy vállalati tanúsítvány megoldás által generált legfelső szintű tanúsítvány, vagy a legfelső önaláírt tanúsítvány X.509 .cer fájl exportálhatja. Kattintson a nyilvános tanúsítvány adatokat importál a legfelső szintű tanúsítvány az Azure. Ezenkívül kell ügyfél-tanúsítvány létrehozása a legfelső szintű tanúsítvány az ügyfelek számára. Minden ügyfél, hogy csatlakozzon a virtuális hálózathoz P2S kapcsolaton keresztül szeretne egy ügyfél tanúsítvány telepítve van a legfelső szintű tanúsítvány alapján jött létre kell rendelkeznie.

### <a name="getcer"></a>1. a .cer fájl a legfelső szintű tanúsítvány beszerzése

Ha egy vállalati megoldás használja, a meglévő tanúsítvány-lánc is használhatja. Ha nem használ egy vállalati hitelesítésszolgáltató megoldást, létrehozhat egy önaláírt legfelső szintű tanúsítvány. Makecert megoldásának gyakori módja a önaláírt tanúsítvány létrehozása.

- Ha egy vállalati tanúsítvány rendszert használ, szerezze be a .cer fájl a legfelső szintű tanúsítvány, amely a használni kívánt. 

- Ha nem használ egy vállalati tanúsítvány megoldás, meg kell legfelső önaláírt tanúsítvány létrehozása. A Windows 10-es lépéseket is hivatkozhat [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md).

1. .Cer fájl szerezni egy tanúsítványt, nyissa meg a **certmgr.msc parancsot** , és keresse meg a legfelső szintű tanúsítvány. Kattintson a jobb gombbal a legfelső önaláírt tanúsítványt, válassza a **Minden tevékenység**, és kattintson az **Exportálás**parancsra. Ekkor megnyílik az **Exportálás varázsló tanúsítvány**.

2. A varázslóban kattintson a **Tovább**gombra, válassza a **nem, a titkos kulcs exportálni**, és kattintson a **Tovább gombra**.

3. A **Fájlformátumban exportálhatja** lapon jelölje ki az alap-64 **kódolású X.509 (. CER).** Kattintson a **Tovább gombra**. 

4. Kattintson az **exportálandó fájl**, **tallózással keresse meg** a tanúsítvány exportálása pontjára. A **fájl nevét**a tanúsítvány fájl nevét. Kattintson a **Tovább gombra**.

5. A tanúsítvány exportálása a **Befejezés** gombra.

### <a name="generateclientcert"></a>2. a ügyfél-tanúsítvány létrehozása

Egyes fog csatlakozni ügyfél vagy hozhat létre egy egyedi tanúsítványt, vagy a tanúsítvány felhasználhatja több ügyfél. Az egyedi ügyfél tanúsítványok létrehozása előnye, hogy az azt jelenti, hogy egyetlen tanúsítvány visszavonni, ha szükséges. Ha a ügyfél tanúsítvány mindenki használ, és megtalálta, amelyet fel kell-e a tanúsítványt egy ügyfél visszavonni, szüksége lesz hozhat létre, és telepítse az új tanúsítványokat összes az ügyfelek, amelyek tanúsítvány hitelesítést végezni.

- Egy vállalati tanúsítvány megoldás használatakor készítése a közös értéket névformátum ügyfél tanúsítvány 'name@yourdomain.com', helyett a "tartománynév\felhasználónév" formátum. 

- Ha önaláírt tanúsítványt használja, megnézheti, hogy [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md) ügyfél-tanúsítvány létrehozása.

### <a name="exportclientcert"></a>3. a ügyfél-tanúsítvány exportálása

Ügyfél-tanúsítvány szükség a hitelesítést. Miután az ügyfél-tanúsítvány létrehozása, exportálhatja. Az ügyfél-tanúsítvány exportálásának később telepíti minden ügyfélszámítógépen.

1. Ügyfél-tanúsítvány exportálásához *certmgr.msc parancsot*is használhatja. Kattintson a jobb gombbal az ügyfél tanúsítvány, amelyet exportálni, válassza a **Minden tevékenység**és majd az **Exportálás**gombra.
2. Az ügyfél-tanúsítvány exportálása a titkos kulccsal. Ez a *.pfx* fájl. Ellenőrizze, hogy ne feledje, hogy a jelszó (kulcs) ebben a tanúsítványban meg és rögzítése.

## <a name="addresspool"></a>Rész 4 – az ügyfél cím-készlet hozzáadása

1. A virtuális hálózati átjáró létrehozása után nyissa meg a virtuális hálózati átjárók lap **Beállítások** szakaszában. A **Beállítások** csoportban kattintson a **pont webhely konfiguráció** a **konfigurációs** lap megnyitásához.

    ![mutasson a webhely lap] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/configuration.png "mutasson a webhely lap")

2. **Cím készlet** az IP-címek, amelyből csatlakozó ügyfelek fog kapni az IP-cím készlete. A cím-készlet hozzáadása, és kattintson a **Mentés**gombra.

    ![ügyfél cím készlet] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/addresspool.png "ügyfél cím készlet")

## <a name="uploadfile"></a>Kijelző 5 – a legfelső szintű tanúsítvány .cer fájl feltöltése

Az átjáró létrehozása után az Azure feltöltheti a .cer fájl megbízható legfelső szintű tanúsítvány. Legfeljebb 20 legfelső szintű tanúsítványok fájlokat is feltölthet. Azure nem töltse fel a titkos kulcs a legfelső szintű tanúsítvány. Miután a .cer fájl feltöltésekor, Azure használja, amely a virtuális hálózati csatlakozás ügyfelek hitelesítést végezni.

1. Nyissa meg azt a **pont webhely konfiguráció** lap. A .cer fájl ad hozzá, ez a lap **legfelső szintű tanúsítvány** szakaszában.

    ![mutasson a webhely lap] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/rootcert.png "mutasson a webhely lap")

2. Győződjön meg arról, hogy a legfelső szintű tanúsítvány exportált, alap-64 kódolású X.509 (.cer) fájl. Exportálja a következő formátumban, így azok, megnyithatja a tanúsítványt és szövegszerkesztőben szüksége.
3. Nyissa meg a tanúsítvány szövegszerkesztőben, például a Jegyzettömbben. Másolja a következő szakaszban.

    ![tanúsítvány-adatok] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/copycert.png "tanúsítvány-adatok")

4. A tanúsítvány adatainak illessze be a **Nyilvános tanúsítvány-adatok** a portálon című szakaszát. Helyezi el a tanúsítvány nevét a **név** szóköz, és kattintson a **Mentés**gombra. Legfeljebb 20 megbízható legfelső szintű tanúsítványok is hozzáadhat.

    ![tanúsítvány feltöltése] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/uploadcert.png "tanúsítvány feltöltése")

## <a name="clientconfig"></a>Töltse le és telepítse a virtuális Magánhálózati ügyfélcsomag konfigurációs 6 - kijelző

Azure P2S használatával csatlakozik ügyfelek ügyfél-tanúsítvány, mind a virtuális Magánhálózati konfigurációs ügyfélcsomag telepítése kell rendelkeznie. Windows-ügyfelek VPN ügyfél konfigurációs csomagok érhetők el. 

A virtuális Magánhálózati ügyfélcsomag beállítása a Windows rendszerbe épített VPN-ügyfélszoftver információkat tartalmaz. A beállítás a virtuális Magánhálózati csatlakoztatása kívánt alkalmazásra. A csomag nem külön szoftver telepítése. Lásd: további információt a [Virtuális Magánhálózati átjáró – gyakori kérdések](vpn-gateway-vpn-faq.md#point-to-site-connections) .

1. **Pont webhely konfigurációja** lap, kattintson a **Letöltés VPN-ügyfél** a **Letöltés VPN-ügyfél** lap megnyitásához.

    ![Töltse le a VPN-ügyfél] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/downloadclient.png "Töltse le a VPN-ügyfél")

2. Jelölje ki a megfelelő csomagot az ügyfél, majd kattintson a **Letöltés**gombra. Jelölje ki a 64 bites ügyfelek esetén **AMD64**. A 32 bites ügyfelek esetén válassza a **x86**.

3. Telepítse a csomagot az ügyfélgépen. Ha egy SmartScreen előugró, kattintson a **További információt a**, majd a **Futtatás mégis** annak érdekében, hogy a csomag telepítéséhez.

4. Kattintson az ügyfélszámítógépen nyissa meg azt a **Hálózati beállításokat** , majd kattintson a **virtuális Magánhálózati**. Ekkor megjelenik a kapcsolat szerepel a listában. Akkor jelennek meg a virtuális hálózathoz kapcsolódik, és így néz Ez a példa hasonló: 

    ![VPN-ügyfél] (./media/vpn-gateway-howto-point-to-site-resource-manager-portal/vpn.png "VPN-ügyfél")

## <a name="installclientcert"></a>Az ügyfél-tanúsítvány telepítés 7 - kijelző

Az egyes ügyfélszámítógépekkel annak érdekében, hogy a hitelesítő kell rendelkeznie az ügyfél-tanúsítványt. Ha telepíti az ügyfél-tanúsítvány, szüksége lesz az ügyfél-tanúsítvány exportálásakor létrehozott jelszót.

1. Az ügyfélszámítógép a .pfx fájl másolása
2. Kattintson duplán a .pfx fájl kattintva telepítheti. Ne módosítsa a telepítés helyének.

## <a name="connect"></a>8 - rész Azure csatlakoztatása

1. A VNet az ügyfélszámítógépen csatlakozhat nyissa meg azt a virtuális Magánhálózati kapcsolatot, és keresse meg a az Ön által létrehozott virtuális Magánhálózati kapcsolatot. A virtuális hálózat nevével megegyező nevű azt. Kattintson a **Csatlakozás**gombra. Előugró üzenet jelenhet meg, amelyre hivatkozik a tanúsítvány használatával. Ez történik, ha kattintson a **Folytatás** magasabb szintű jogosultságokkal használni. 

2. A **kapcsolat** állapota lapon kattintson a **Csatlakozás** a kapcsolat indítása elemre. Ha **Tanúsítvány kiválasztása** képernyő jelenik meg, ellenőrizze, hogy az ügyfél tanúsítványát megjelenítő azt, amelyiket csatlakozni szeretne. Ha nem, jelölje ki a megfelelő tanúsítványt használja a lefelé mutató nyílra, és kattintson **az OK**gombra.

    ![2 VPN-ügyfél] (./media/vpn-gateway-howto-point-to-site-rm-ps/clientconnect.png "Virtuális Magánhálózati kapcsolat-ügyfél")

3. Most már kell létrehozni a kapcsolatot.

    ![3 VPN-ügyfél] (./media/vpn-gateway-howto-point-to-site-rm-ps/connected.png "Virtuális Magánhálózati ügyfél kapcsolat 2")

## <a name="verify"></a>Része a 9 - ellenőrizze a kapcsolat

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

## <a name="add"></a>Hozzáadása és eltávolítása a megbízható legfelső szintű tanúsítványok

Azure megbízható legfelső szintű tanúsítvány eltávolítható. Ha eltávolít egy megbízható tanúsítványt, az ügyfél-tanúsítványok a legfelső szintű tanúsítvány előállított már nem tud csatlakozni az Azure pont-webhelyen keresztül. Ha azt szeretné, hogy ügyfeleknek való csatlakozást, telepíteniük kell a tanúsítvány megbízható Azure-ban létrehozott új ügyfél-tanúsítványt.

Kezelheti a visszavont ügyfél tanúsítványok **pont webhely konfigurációja listáját** lap. Ez az a, a [megbízható legfelső szintű tanúsítvány feltöltéséhez](#uploadfile)használt lap.

## <a name="revokeclient"></a>A visszavont ügyfél tanúsítványok listájának kezelése

Ügyfél-tanúsítványok vonhatja meg. A tanúsítvány-visszavonási lista lehetővé teszi a szelektív megtagadja az egyéni ügyfél tanúsítványok alapuló webhely pont-kapcsolatot. Ha eltávolítja a legfelső szintű tanúsítvány .cer Azure, akkor az access összes ügyfél tanúsítványok generált/aláírt a visszavont legfelső szintű tanúsítvány által visszavonása. Ha szeretne visszavonni egy adott ügyfél tanúsítványt, nem a legfelső szintű, megteheti. Úgy, hogy a tanúsítványok, a legfelső szintű tanúsítvány előállított továbbra is érvényes. 

A közös gyakorlat a legfelső szintű tanúsítvány használatával csoport- vagy szervezeti szintű hozzáférés kezelése a külön hozzáférés vezérléséhez az egyéni felhasználók a visszavont ügyfél tanúsítványok használatakor.

Kezelheti a visszavont ügyfél tanúsítványok **pont webhely konfigurációja listáját** lap. Ez az a, a [megbízható legfelső szintű tanúsítvány feltöltéséhez](#uploadfile)használt lap.


## <a name="next-steps"></a>Következő lépések

A virtuális hálózat virtuális gép vehet. A lépések [létrehozása egy virtuális gép](../virtual-machines/virtual-machines-windows-hero-tutorial.md) témakörben találhat.