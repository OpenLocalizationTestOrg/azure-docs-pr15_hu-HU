<properties
   pageTitle="Az Azure virtuális hálózati az Azure portálon átjáró pont-webhely virtuális Magánhálózati kapcsolat beállítása |} Microsoft Azure"
   description="Az Azure portálon pont-webhely VPN átjáró kapcsolat létrehozásával, biztonságosan csatlakoztathassa a Azure virtuális hálózathoz."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Egy az Azure portálon VNet pont a webhely-kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasszikus - Azure portál](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Ez a cikk végigvezeti a VNet létrehozása az Azure portálon a klasszikus telepítési modellben pont a webhely-kapcsolatot. A pont-webhely (P2S) konfiguráció segítségével biztonságos kapcsolatot az egyes ügyfélszámítógép virtuális hálózathoz. P2S kapcsolat akkor hasznos, ha a Kapcsolódás a VNet távoli helyről, például a kezdőlap vagy konferencia szeretne. Vagy ha csak van néhány ügyfélprogramokat sorolja fel kell egy virtuális hálózathoz csatlakozik.

A kapcsolatok pont-webhely nem szükséges VPN-eszközhöz vagy egy nyilvános IP-cím használata. A virtuális Magánhálózati kapcsolat által a kapcsolat kezdve az ügyfélszámítógép létrejön. További információt a webhely-pont kapcsolatok című [VPN átjáró gyakran ismételt kérdések](vpn-gateway-vpn-faq.md#point-to-site-connections) [Virtuális Magánhálózati átjárót](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Telepítési modellek és P2S kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a két környezetben modell és elérhető telepítési módszerek P2S konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Egyszerű munkafolyamat 

![Pont-a-webhely-diagram] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "pont-webhely")


Az alábbi szakaszok haladjon végig, és hozhat létre virtuális hálózathoz biztonságos webhely pont-kapcsolatot. 

1. Hozzon létre egy virtuális hálózati és a virtuális Magánhálózati átjáró
2. Tanúsítványok létrehozása
3. A .cer fájl feltöltése
4. A virtuális Magánhálózati ügyfél konfigurációs csomag készítése
5. Az ügyfélszámítógép konfigurálása
6. Azure csatlakoztatása

### <a name="example-settings"></a>Példa beállításai

Az alábbi példa beállításokat használhatja:

- **Név: VNet1**
- **Hely címe: 192.168.0.0/16**
- **Alhálózat neve: FrontEnd**
- **Alhálózat címtartományokat: 192.168.1.0/24**
- **Előfizetés:** Ha egynél több előfizetése van, ellenőrizze, hogy a megfelelő fiókot használ.
- **Erőforráscsoport: TestRG**
- **Hely: Kelet-Amerikai Egyesült Államok**
- **Kapcsolat típusa: pont-webhely**
- **Ügyfél címterület: 172.16.201.0/24**. Az a pont-webhely kapcsolattal VNet csatlakozó VPN-ügyfelek IP-címet a megadott készletből fogadása
- **GatewaySubnet: 192.168.200.0/24**. Az átjáró alhálózat "GatewaySubnet" nevét kell használnia.
- **Méret:** Jelölje ki a használni kívánt Termékváltozat átjáró.
- **Útválasztási típusa: dinamikus**

## <a name="vnetvpn"></a>Szakasz 1 - virtuális hálózat és a virtuális Magánhálózati átjáró létrehozása

### <a name="createvnet"></a>1 rész: Hozzon létre egy virtuális hálózaton

Ha még nem rendelkezik egy virtuális hálózaton, hozzon létre egyet. Képernyőképek példaként szolgálnak. Ügyeljen arra, hogy az értékek lecserélése saját. Az Azure portál használatával egy VNet létrehozásához kövesse az alábbi lépéseket: 

1. Böngészőben, nyissa meg az [Azure portálra](http://portal.azure.com) , és ha szükséges, jelentkezzen be a Azure-fiókjával.

2. Kattintson az **Új**gombra. A **Keresés a piactér** mezőbe írja be a "Virtuális hálózat". Keresse meg a **Virtuális hálózati** a visszaadott listából, és a gombra kattintva nyissa meg a **Virtuális hálózati** lap.

    ![Virtuális hálózati lap keresése] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Virtuális hálózati lap keresése")

3. A virtuális hálózati lap, a listából **Válasszon ki egy telepítési modellt** , alján jelölje be a **Klasszikus**, és kattintson a **Létrehozás**gombra.

    ![Válassza a telepítési modellje] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Válassza a telepítési modellje")

4. A **virtuális hálózat létrehozása** lap a VNet beállításainak. Ez a lap fog felvétele az első Címterület és a tartomány egyetlen alhálózat címet. A VNet létrehozása után térjen vissza, és további alhálózat és a cím szóközöket.

    ![Virtuális hálózati a Létrehozás lap] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Virtuális hálózati a Létrehozás lap")

5. Győződjön meg arról, hogy az **előfizetése** -e a megfelelő fiókot. Az előfizetések módosíthatja a legördülő lista használatával.

6. Kattintson az **erőforrás csoportot** , és válassza a meglévő erőforráscsoport, vagy hozzon létre egy újat, írjon be egy nevet az új erőforráscsoport. Ha egy új csoportot szeretne létrehozni, nevezze el az erőforráscsoport a tervezett konfigurációs értékek szerint. Az erőforrás csoportok kapcsolatos további tudnivalókért látogassa meg [Azure erőforrás szolgáltatásának áttekintése](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Ezután jelölje ki a **helyet** a VNet beállításait. A hely határozza meg az erőforrásokat, ez VNet rendszerbe tároló.

8. Jelölje be a **PIN-kód irányítópult** , ha szeretné engedélyezni szeretné a VNet az irányítópulton megtalálását, és kattintson a **Létrehozás**gombra.
    
    ![PIN-kód irányítópult] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "PIN-kód irányítópult")


9. Létrehozása gombra kattint, látni fogja a csempe az irányítópult, amely a VNet végrehajtásának tükrözni fogja. A mozaik változik a VNet készül.

    ![Virtuális hálózati létrehozása csempe] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Virtuális hálózati Creating csempe")

10. Miután létrehozta a virtuális hálózat, annak érdekében, hogy a névfeloldás kezelje DNS-kiszolgáló IP-címét is hozzáadhat. Nyissa meg a virtuális hálózat beállításainak, kattintson a DNS-kiszolgálók, és adja hozzá a DNS-kiszolgáló, amely a használni kívánt IP-címét. Ez a beállítás nem hoz létre egy új DNS-kiszolgálót. Ne felejtse el a DNS-kiszolgáló, amely kommunikálni az erőforrások hozzáadása.

A virtuális hálózat létrehozása után jelenik meg **létrehozva** megjelenik **az állapot** csoportban a hálózatok lapon az Azure klasszikus portálon.

### <a name="gateway"></a>2 rész: Átjáró alhálózat és dinamikus útválasztási átjáró létrehozása

Ebben a lépésben létrehoz egy átjáró alhálózat és dinamikus útválasztási átjárót. Az Azure-portálon a klasszikus telepítési modell az átjáró alhálózat és az átjáró létrehozásához történik azonos konfigurációs rögzítéséhez keresztül.

1. Az portálon nyissa meg a virtuális hálózat, amelyhez hozzá szeretne létrehozni egy átjáró.

2. Kattintson a virtuális hálózatához, az **Áttekintés** lap a virtuális Magánhálózati kapcsolatot csoportban kattintson a lap **átjáró**.

    ![Ide kattintva átjáró létrehozása] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Ide kattintva átjáró létrehozása")


3. Az **Új virtuális Magánhálózati kapcsolatot** a lap, jelölje be a **pont webhely**.

    ![P2S kapcsolat típusa] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S kapcsolat típusa")

4. Adja hozzá az IP-címtartományokat **Ügyfél-címterület használatára**. Ez az a tartomány, amelyből a VPN-ügyfelek fog kapni az IP-cím csatlakozáskor. Az automatikus kitöltve tartomány törlése és felvétele a saját.

    ![Ügyfél-címterület használatára] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Ügyfél-címterület használatára")

5. Jelölje be a **Létrehozás átjáró azonnal** jelölőnégyzetet.

    ![Létrehozás átjáró azonnal] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Létrehozás átjáró azonnal")

6. Kattintson a **választható átjáró konfigurációs** az **átjáró konfigurációs** lap megnyitásához.

    ![Kattintson a választható átjáró konfigurációja] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Kattintson a választható átjáró konfigurációja")

7. Kattintson a **beállítások konfigurálása alhálózat szükséges** felvenni az **átjáró alhálózat**. Lehet létrehozni egy átjáró alhálózat minél kisebb méretű /29 lép, azt javasoljuk, hogy legalább /28 vagy /27 bejelölésével több címet tartalmazó nagyobb alhálózat hozza létre. Ez az esetleges további beállításokat, hogy szükség lehet a jövőben igazodik elég címek lehetővé teszi.

    >[AZURE.IMPORTANT] Átjáró alhálózat használatakor ne hálózati biztonsági csoport (NSG) az átjáró alhálózathoz társítása. Az alhálózathoz hálózati biztonsági csoport társítása, előfordulhat, hogy le szeretné állítani a várt módon működik a virtuális Magánhálózati átjárót. Lásd: további információt a hálózat biztonsági csoportok [Mi az hálózati biztonsági csoport?](../articles/virtual-network/virtual-networks-nsg.md)

    ![GatewaySubnet hozzáadása] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "GatewaySubnet hozzáadása")

8. Jelölje ki az átjáró **méretét**. Ez az, hogy az átjáró Termékváltozat a virtuális hálózati átjáró létrehozásához használt. A portálon az alapértelmezett Termékváltozat **egyszerű**. Az átjáró termékváltozatok kapcsolatos további tudnivalókért tekintse meg [A virtuális Magánhálózati átjáró beállításait](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![átjáró mérete](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Válassza a **Továbbítás írja be** az átjáró. P2S konfigurációk szükség egy **dinamikus** útválasztási típust. Ha befejezte a lap konfigurálása, kattintson az **OK gombra** .

    ![Útválasztási típusának beállítása] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Útválasztási típusának beállítása")

10. Kattintson az **Új virtuális Magánhálózati kapcsolatot** a lap, **az OK gombra** a lap alján a virtuális hálózati átjáró létrehozása a kezdéshez. Ez lehet legfeljebb 45 percet igénybe vehet. 


## <a name="generatecerts"></a>Szakasz 2 - tanúsítványok létrehozása

Tanúsítványok Azure által használt VPN-ügyfelek pont-webhely VPN hitelesítést végezni. Nyilvános tanúsítvány-adatok (nem a titkos kulcs) szerint az alap-64 encoded egy vállalati tanúsítvány megoldás által generált legfelső szintű tanúsítvány, vagy a legfelső önaláírt tanúsítvány X.509 .cer fájl exportálhatja. Kattintson a nyilvános tanúsítvány adatokat importál a legfelső szintű tanúsítvány az Azure. Ezenkívül kell ügyfél-tanúsítvány létrehozása a legfelső szintű tanúsítvány az ügyfelek számára. Minden ügyfél, hogy csatlakozzon a virtuális hálózathoz P2S kapcsolaton keresztül szeretne egy ügyfél tanúsítvány telepítve van a legfelső szintű tanúsítvány alapján jött létre kell rendelkeznie.

### <a name="cer"></a>1 rész: A .cer fájl a legfelső szintű tanúsítvány beszerzése


Ha egy vállalati megoldás használja, a meglévő tanúsítvány-lánc is használhatja. Ha nem használ egy vállalati hitelesítésszolgáltató megoldást, létrehozhat egy önaláírt legfelső szintű tanúsítvány. Makecert megoldásának gyakori módja a önaláírt tanúsítvány létrehozása.

- Ha egy vállalati tanúsítvány rendszert használ, szerezze be a .cer fájl a legfelső szintű tanúsítvány, amely a használni kívánt. 

- Ha nem használ egy vállalati tanúsítvány megoldás, meg kell legfelső önaláírt tanúsítvány létrehozása. A Windows 10-es lépéseket is hivatkozhat [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md).

1. .Cer fájl szerezni egy tanúsítványt, nyissa meg a **certmgr.msc parancsot** , és keresse meg a legfelső szintű tanúsítvány. Kattintson a jobb gombbal a legfelső önaláírt tanúsítványt, válassza a **Minden tevékenység**, és kattintson az **Exportálás**parancsra. Ekkor megnyílik az **Exportálás varázsló tanúsítvány**.

2. A varázslóban kattintson a **Tovább**gombra, válassza a **nem, a titkos kulcs exportálni**, és kattintson a **Tovább gombra**.

3. A **Fájlformátumban exportálhatja** lapon jelölje ki az alap-64 **kódolású X.509 (. CER).** Kattintson a **Tovább gombra**. 

4. Kattintson az **exportálandó fájl**, **tallózással keresse meg** a tanúsítvány exportálása pontjára. A **fájl nevét**a tanúsítvány fájl nevét. Kattintson a **Tovább gombra**.

5. A tanúsítvány exportálása a **Befejezés** gombra.


### <a name="genclientcert"></a>2 rész: Ügyfél-tanúsítvány létrehozása

Egyes fog csatlakozni ügyfél vagy hozhat létre egy egyedi tanúsítványt, vagy a tanúsítvány felhasználhatja több ügyfél. Az egyedi ügyfél tanúsítványok létrehozása előnye, hogy az azt jelenti, hogy egyetlen tanúsítvány visszavonni, ha szükséges. Ha a ügyfél tanúsítvány mindenki használ, és megtalálta, amelyet fel kell-e a tanúsítványt egy ügyfél visszavonni, szüksége lesz hozhat létre, és telepítse az új tanúsítványokat összes az ügyfelek, amelyek tanúsítvány hitelesítést végezni.

- Egy vállalati tanúsítvány megoldás használatakor készítése a közös értéket névformátum ügyfél tanúsítvány 'name@yourdomain.com', helyett a "tartománynév\felhasználónév" formátum. 

- Ha önaláírt tanúsítványt használja, megnézheti, hogy [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md) ügyfél-tanúsítvány létrehozása.

### <a name="exportclientcert"></a>Rész 3: Az ügyfél-tanúsítvány exportálás

Ügyfél-tanúsítvány telepítse az összes olyan számítógépen, amely a virtuális hálózat csatlakozni szeretne. Ügyfél-tanúsítvány szükség a hitelesítést. Az ügyfél-tanúsítvány telepítése automatizálható, vagy manuálisan telepíteni. Az alábbi lépésekkel haladjon végig exportálása és az ügyfél-tanúsítvány manuális telepítése.

1. Ügyfél-tanúsítvány exportálásához *certmgr.msc parancsot*is használhatja. Kattintson a jobb gombbal az ügyfél tanúsítvány, amelyet exportálni, válassza a **Minden tevékenység**és majd az **Exportálás**gombra.
2. Az ügyfél-tanúsítvány exportálása a titkos kulccsal. Ez a *.pfx* fájl. Ellenőrizze, hogy ne feledje, hogy a jelszó (kulcs) ebben a tanúsítványban meg és rögzítése.

## <a name="upload"></a>Szakasz 3 – a legfelső szintű tanúsítvány .cer fájl feltöltése

Az átjáró létrehozása után az Azure feltöltheti a .cer fájl megbízható legfelső szintű tanúsítvány. Legfeljebb 20 legfelső szintű tanúsítványok fájlokat is feltölthet. Azure nem töltse fel a titkos kulcs a legfelső szintű tanúsítvány. Miután a .cer fájl feltöltésekor, Azure használja, amely a virtuális hálózati csatlakozás ügyfelek hitelesítést végezni.

1. A lap a VNet a **virtuális Magánhálózati kapcsolatot** szakaszban kattintson a Megnyitás a **pont-pont virtuális Magánhálózati kapcsolatot a **ügyfelek** képre** lap.

    ![Ügyfelek] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Ügyfelek")

2. A **pont webhely kapcsolaton** lap, kattintson a **kezelés tanúsítványok** a **tanúsítványok** lap megnyitásához.<br>

    ![Tanúsítványok lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Kattintson a **tanúsítványok** lap **Töltse fel** a **tanúsítvány feltöltése** lap megnyitásához.<br>

    ![Töltse fel a tanúsítványok lap](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Kattintson a mappa képre a .cer fájl tallózásához. Jelölje ki a fájlt, majd kattintson az **OK gombra**. Frissítse a lapot a feltöltött tanúsítvány megtekintéséhez kattintson a **tanúsítványok** lap.

    ![Töltse fel a tanúsítvány](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Szakasz 4 – a virtuális Magánhálózati ügyfél konfigurációs csomag készítése

A virtuális hálózathoz csatlakozik, akkor is kell állítsa be a VPN-ügyfél. Az ügyfélszámítógép való csatlakozás szükséges az ügyfél-tanúsítvány és a TNÉV VPN konfigurációs ügyfélcsomag is.

A virtuális Magánhálózati ügyfélcsomag beállítása a Windows rendszerbe épített VPN-ügyfélszoftver konfigurációs adatait tartalmazza. A csomag nem külön szoftver telepítése. A beállítások egyediek a virtuális hálózat, amelyhez csatlakozni szeretne. Ügyfél által támogatott operációs rendszerek listájáért lásd: a a [pont webhely kapcsolatok](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN átjáró gyakori kérdések szakaszt. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>A virtuális Magánhálózati ügyfélcsomag konfigurációs létrehozásához

1. Az Azure portált a **áttekintése** lap az a VNet a **virtuális Magánhálózati kapcsolatot**, kattintson a ügyfél képre kattintva nyissa meg a **pont-pont virtuális Magánhálózati kapcsolat** lap.
2. A képernyő tetején, a **pont-pont virtuális Magánhálózati kapcsolat** lap, kattintson a letöltés csomagra, amely megfelel az ügyfél operációs rendszere, amelyeken telepítendő:

 - A 64 bites ügyfelek jelölje be a **VPN-ügyfél (64 bites)**.
 - A 32 bites ügyfelek jelölje be a **VPN-ügyfél (32 bites)**.

    ![Töltse le a virtuális Magánhálózati ügyfélcsomag konfigurálása](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Ekkor megjelenik egy üzenet, hogy Azure virtuális a hálózati VPN ügyfél csomagja hoz létre. Néhány perc múlva a csomag jön létre, és egy üzenet jelenik meg, hogy a csomag letöltődött, a helyi számítógépen. Mentse a konfigurációs csomagfájlt. Minden ügyfélszámítógépen P2S használatáról virtuális hálózati csatlakozó telepíti.


## <a name="clientconfiguration"></a>Szakasz 5 - az ügyfélszámítógép konfigurálása

### <a name="part-1-install-the-client-certificate"></a>1 rész: Telepítse az ügyfél-tanúsítvány

Az egyes ügyfélszámítógépekkel annak érdekében, hogy a hitelesítő kell rendelkeznie az ügyfél-tanúsítványt. Ha telepíti az ügyfél-tanúsítvány, szüksége lesz az ügyfél-tanúsítvány exportálásakor létrehozott jelszót.

1. Az ügyfélszámítógép a .pfx fájl másolása
2. Kattintson duplán a .pfx fájl kattintva telepítheti. Ne módosítsa a telepítés helyének.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>2 rész: A virtuális Magánhálózati ügyfélcsomag konfigurációs telepítése

Az azonos VPN konfigurációs ügyfélcsomag az egyes ügyfélszámítógépekkel használhatja, feltéve, hogy a verzió architektúrája megegyezik az ügyfél számára.

1. Másolja a vágólapra a konfigurációs fájl helyileg virtuális hálózatához csatlakozzanak, és kattintson duplán az .exe fájl, amelyet a számítógépre. 

2. A csomag magában telepítése után elindíthatja a virtuális Magánhálózati kapcsolatot. A Microsoft által a konfigurációs csomag nincs aláírva. Előfordulhat, hogy a kívánja a csomagot, a szervezet aláírási szolgáltatás használatával jelentkezzen be, vagy jelentkezzen [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)segítségével saját. Célszerű használni a csomag bejelentkezés nélkül az OK gombra. Jó helyen jár, ha a csomag nem be van jelentkezve, egy figyelmeztetés jelenik meg a csomag telepítésekor.

3. Kattintson az ügyfélszámítógépen nyissa meg azt a **Hálózati beállításokat** , majd kattintson a **virtuális Magánhálózati**. Ekkor megjelenik a kapcsolat szerepel a listában. A virtuális hálózat nevét mutatja, hogy fog csatlakozni, és ehhez hasonlóan fog kinézni: 

    ![VPN-ügyfél] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN-ügyfél")

## <a name="connect"></a>Szakasz 6 - Azure csatlakoztatása

### <a name="connect-to-your-vnet"></a>Csatlakozás a VNet

1. A VNet az ügyfélszámítógépen csatlakozhat nyissa meg azt a virtuális Magánhálózati kapcsolatot, és keresse meg a az Ön által létrehozott virtuális Magánhálózati kapcsolatot. A virtuális hálózat nevével megegyező nevű azt. Kattintson a **Csatlakozás**gombra. Előugró üzenet jelenhet meg, amelyre hivatkozik a tanúsítvány használatával. Ez történik, ha kattintson a **Folytatás** magasabb szintű jogosultságokkal használni. 

2. A **kapcsolat** állapota lapon kattintson a **Csatlakozás** a kapcsolat indítása elemre. Ha **Tanúsítvány kiválasztása** képernyő jelenik meg, ellenőrizze, hogy az ügyfél tanúsítványát megjelenítő azt, amelyiket csatlakozni szeretne. Ha nem, jelölje ki a megfelelő tanúsítványt használja a lefelé mutató nyílra, és kattintson **az OK**gombra.

    ![Csatlakozás] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "Virtuális Magánhálózati kapcsolat-ügyfél")

3. Most már kell létrehozni a kapcsolatot.

    ![Létrehozott kapcsolat] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Kapcsolat létrehozása")

### <a name="verify-the-vpn-connection"></a>A virtuális Magánhálózati kapcsolat ellenőrzése

1. Győződjön meg róla, hogy a virtuális Magánhálózati kapcsolat aktív, nyissa meg a rendszergazda jogú parancssort, és futtassa az *ipconfig/minden*.
2. Az eredmények megtekintéséhez. Figyelje meg, hogy a kapott IP-címét a webhely pont-kapcsolat létrehozásakor a VNet megadott cím tartományon belül címek valamelyikét. Az eredmény rendre valami ehhez hasonló:

Példa:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Következő lépések

A virtuális hálózat virtuális gépeken futó vehet. Megtudhatja, [hogy miként hozhat létre egy egyéni virtuális számítógépre](../virtual-machines/virtual-machines-windows-classic-createportal.md).