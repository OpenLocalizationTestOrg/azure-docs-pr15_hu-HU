<properties
   pageTitle="A klasszikus portálon Azure virtuális hálózathoz pont-webhely VPN átjáró kapcsolat beállítása |} Microsoft Azure"
   description="Átjáró pont-webhely virtuális Magánhálózati kapcsolat létrehozásával, biztonságosan csatlakoztathassa a Azure virtuális hálózathoz."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>A klasszikus portálon egy VNet pont a webhely-kapcsolat beállítása

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - Azure portál](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Erőforrás-kezelő - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klasszikus - Azure portál](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klasszikus – klasszikus portál](vpn-gateway-point-to-site-create.md)

A pont-webhely (P2S) konfiguráció segítségével biztonságos kapcsolatot az egyes ügyfélszámítógép virtuális hálózathoz. P2S kapcsolat akkor lehet hasznos, amikor szeretne csatlakozni a VNet távoli helyről, például a kezdőlap vagy a konferenciához, vagy csak néhány ügyfelek, amely még nem csatlakozott a virtuális hálózathoz van.

Ez a cikk végigvezeti a VNet létrehozása a **Klasszikus portál** **Klasszikus telepítési modell** pont a webhely-kapcsolatot.

A kapcsolatok pont-webhely nem szükséges VPN-eszközhöz vagy egy nyilvános IP-cím használata. A virtuális Magánhálózati kapcsolat által a kapcsolat kezdve az ügyfélszámítógép létrejön. További információt a webhely-pont kapcsolatok című [VPN átjáró gyakran ismételt kérdések](vpn-gateway-vpn-faq.md#point-to-site-connections) [tervezése és kialakítása](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Telepítési modellek és P2S kapcsolatok módszerei

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

A következő táblázat mutatja a két környezetben modell és elérhető telepítési módszerek P2S konfigurációk. Egy konfigurálási lépéseket ez a cikk érhető el, ha azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Egyszerű munkafolyamat

![Pont-a-webhely-diagram] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "pont-webhely")
 
Az alábbi lépésekkel haladjon végig, és hozhat létre virtuális hálózathoz biztonságos webhely pont-kapcsolatot. 

Az a pont a webhely-kapcsolat beállításai négy részre oszlik. A sorrendben, amelyben beállíthatja az egyes szakaszokhoz fontos. Nem ugorja át, vagy a következő ugorhat.

- **Szakasz 1** Hozzon létre egy virtuális hálózati és a virtuális Magánhálózati átjárót.
- **Szakasz 2** Hozzon létre a hitelesítéshez használt tanúsítványokat, és töltse fel őket.
- **Szakasz 3** Exportálhatja, és telepítse az ügyfél-tanúsítványokat.
- **Szakasz 4** Állítsa be a VPN-ügyfél.

## <a name="vnetvpn"></a>Szakasz 1 - virtuális hálózat és a virtuális Magánhálózati átjáró létrehozása

### <a name="part-1-create-a-virtual-network"></a>1 rész: Hozzon létre egy virtuális hálózaton

1. Jelentkezzen be az [Azure klasszikus portálon](https://manage.windowsazure.com/). Ezeket a lépéseket a klasszikus portál az Azure portálon használja. Jelenleg nem hozható létre az Azure portálon P2S kapcsolatot.

2. A képernyő bal alsó sarkában kattintson az **Új**gombra. A navigációs ablakban kattintson a **Hálózati szolgáltatásokkal**, és válassza a **Virtuális hálózat**. Kattintson az **Egyéni létrehozása** a konfigurálása varázsló elindításához.

3. A **Virtuális hálózati részletei** lapon írja be az alábbi adatokat, és kattintson a jobb alsó sarkában lévő következő nyílra.
    - **Név**: nevezze el a virtuális hálózatot. Ha például "VNet1". Ez a fogja a hivatkozott amikor rendszerbe állítják VMs e VNet nevét.
    - **Hely**: A hely, amelyre az erőforrások (VMs) tárolnak fizikai helyét (régió) közvetlenül kapcsolódó. Például ha azt szeretné, hogy a VMs, amely a virtuális hálózati fizikailag kelet-Amerikai Egyesült Államokban található rendszerbe, jelölje ki azt a helyet. A régió virtuális hálózatához kapcsolódó létrehozásuk után nem módosítható.

4. A **DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat** lapon írja be az alábbi adatokat, és kattintson a jobb alsó sarkában lévő következő nyílra.
    - **A DNS-kiszolgálók**: írja be a DNS-kiszolgáló neve és IP-cím vagy a helyi menüből válassza ki a korábban már regisztrált a DNS-kiszolgáló. Ez a beállítás nem hoz létre a DNS-kiszolgáló. Lehetővé teszi, hogy adja meg a DNS-kiszolgálók névfeloldás a virtuális hálózathoz használni kívánt. Ha szeretne használni az Azure alapértelmezett név felbontás szolgáltatás, hagyja üresen ebben a szakaszban.
    - **Webhely-konfigurálása pont VPN**: jelölje be a jelölőnégyzetet.

5. A **Csatlakozási pont-webhely** lapon adja meg az IP-címtartományokat, amelyből a VPN-ügyfelek kap egy IP-címet, amikor a csatlakoztatott. Megadhatja a címtartományokat kapcsolatos néhány szabályok vonatkoznak. Fontos, hogy ellenőrizze, hogy a megadott feltételeknek nem áll átfedésben a helyszíni hálózaton található tartományok bármelyikével.

6. Írja be az alábbi adatokat, és kattintson a következő mutató nyílra.
 - **Címterület**: a kezdő IP és CIDR (cím száma).
 - **Hozzáadás címterület**: Adja hozzá a címterület csak akkor, ha szükséges, a hálózattervezés.

7. A **Virtuális hálózati cím szóközöket** lapon adja meg a virtuális hálózathoz használni kívánt címet tartományt. Ezek a dinamikus IP-címek (IMMERZIÓBAN), amely a VMs és a többi szerepkör példányát, amely a virtuális hálózati rendszerbe fog kiosztani.<br><br>Különösen fontos, hogy a helyszíni hálózaton használt tartományok bármelyikével nem áll átfedésben tartomány kijelölése. Meg kell koordinálása a hálózati rendszergazda, aki carve a helyszíni hálózaton cím adhatja meg a virtuális hálózathoz használni egy az IP-címeket ki kell.

8. Írja be az alábbi adatokat, és kattintson a a jelet a virtuális hálózat létrehozásának megkezdéséhez.
 - **Címterület**: Adja hozzá a belső IP-címtartományokat a virtuális, többek között a darab és a kezdő IP-hálózathoz használni kívánt. Fontos, hogy a helyszíni hálózaton használt tartományok bármelyikével nem áll átfedésben tartomány kijelölése. 
 - **Hozzáadás alhálózat**: további alhálózathoz nem szükséges, de érdemes hozzon létre külön alhálózathoz VMs, akihez statikus IMMERZIÓBAN. Vagy van-e a VMs alhálózat különböző nézeten a többi szerepkör futó példányát, érdemes lehet.
 - **Hozzáadás átjáró alhálózat**: a pont-webhely VPN szükség-e az átjáró alhálózat. Az átjáró alhálózat hozzáadásához kattintson ide. Az átjáró alhálózat csak a virtuális hálózati átjáró használatos.

9. A virtuális hálózat létrehozása után jelenik meg **létrehozva** megjelenik **az állapot** csoportban a hálózatok lapon az Azure klasszikus portálon. A virtuális hálózat létrehozása után a dinamikus útválasztási átjáró hozhat létre.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>2 rész: Dinamikus útválasztási átjáró létrehozása

Az átjáró típus szerint dinamikus kell beállítania. Statikus útválasztási átjárók Ez a funkció nem működik.

1. Az Azure klasszikus portálon **hálózatok** lapon kattintson a virtuális hálózat létrehozott, és nyissa meg az **Irányítópult** oldalát.

2. Kattintson az **Átjárók létrehozása**, az **Irányítópult** lap alján található. Megjelenik egy üzenet **meg szeretné virtuális hálózati "VNet1" az átjáró létrehozása**. Kattintson az **Igen gombra** a kezdéshez az átjáró létrehozása. Eltarthat körülbelül 15 percet, hogy az átjáró létrehozásához.

## <a name="generate"></a>Szakasz 2 - készítése, és töltse fel a tanúsítványok

Tanúsítványok használatával VPN-ügyfelek pont-webhely VPN hitelesítést végezni. Használhatja a legfelső szintű tanúsítvány generálja a vállalati tanúsítvány megoldás, vagy önaláírt tanúsítványt is használhatja. Legfeljebb 20 legfelső szintű tanúsítványok Azure is feltölthet. Miután a .cer fájl feltöltésekor, Azure segítségével azt tárolt adatok hitelesítést végezni az ügyfelek, amelyek a telepített ügyfél-tanúsítványt. Az ügyfél tanúsítványát a ugyanaz a tanúsítvány, amely a .cer fájl kell létrehozni.

Ebben a részben meg fog tegye a következőket:

- Szerezze be a legfelső szintű tanúsítvány .cer fájl. Ez lehet önaláírt tanúsítványt, vagy használhatja a vállalati tanúsítvány rendszerhez.
- Töltse fel a .cer fájl Azure.
- Ügyfél-tanúsítványok létrehozása.

### <a name="root"></a>1 rész: A .cer fájl a legfelső szintű tanúsítvány beszerzése

Ha egy vállalati tanúsítvány rendszert használ, szerezze be a .cer fájl a legfelső szintű tanúsítvány, amely a használni kívánt. A [kijelző 3](#createclientcert)az ügyfél tanúsítványok a legfelső szintű tanúsítvány a készítése.

Ha nem használ egy vállalati tanúsítvány megoldás, kell legfelső önaláírt tanúsítvány létrehozása. A Windows 10-es lépéseket is hivatkozhat [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md). A cikk bemutatja az önaláírt tanúsítvány létrehozása, majd exportálja a .cer fájl makecert használatával.

### <a name="upload"></a>2 rész: Feltöltése a legfelső szintű tanúsítvány .cer fájl az Azure klasszikus portálra

Megbízható tanúsítvány hozzáadása Azure. Base64 kódolású X.509 (.cer) fájl Azure beállításakor, a legfelső szintű tanúsítvány, amely a fájl megbízhatónak Azure is jelzi.

1. Az Azure klasszikus portálon virtuális hálózatához, a **tanúsítványok** lapon kattintson a **legfelső szintű tanúsítvány feltöltése**elemre.

2. A **Tanúsítvány feltöltése** lapon keresse meg a .cer legfelső szintű tanúsítvány, és válassza a be van jelölve.

### <a name="createclientcert"></a>3 rész: Ügyfél-tanúsítvány létrehozása

Ezután készítése az ügyfél tanúsítványok. Egyes fog csatlakozni ügyfél vagy hozhat létre egy egyedi tanúsítványt, vagy a tanúsítvány felhasználhatja több ügyfél. Az egyedi ügyfél tanúsítványok létrehozása előnye, hogy az azt jelenti, hogy egyetlen tanúsítvány visszavonni, ha szükséges. Egyéb esetben ha ugyanaz a ügyfél tanúsítvány mindenki használ, és megtalálta, amelyet fel kell-e a tanúsítványt egy ügyfél visszavonni, szüksége lesz hozhat létre, és telepítse az új tanúsítványokat összes az ügyfelek, amelyek a tanúsítvány hitelesítést végezni.

- Egy vállalati tanúsítvány megoldás használatakor készítése a közös értéket névformátum ügyfél tanúsítvány 'name@yourdomain.com', helyett a "Tartomány\felhasználónév" NetBIOS formázása. 

- Ha önaláírt tanúsítványt használja, megnézheti, hogy [önaláírt legfelső szintű tanúsítványok pont a webhely-konfiguráció használata](vpn-gateway-certificates-point-to-site.md) ügyfél-tanúsítvány létrehozása.

## <a name="installclientcert"></a>Szakasz 3 – exportálás és telepítése az ügyfél-tanúsítvány

Ügyfél-tanúsítvány telepítse az összes olyan számítógépen, amely a virtuális hálózat csatlakozni szeretne. Ügyfél-tanúsítvány szükség a hitelesítést. Az ügyfél-tanúsítvány telepítése automatizálható, vagy manuálisan telepíteni. Az alábbi lépésekkel haladjon végig exportálása és az ügyfél-tanúsítvány manuális telepítése.

1. Ügyfél-tanúsítvány exportálásához *certmgr.msc parancsot*is használhatja. Kattintson a jobb gombbal az ügyfél tanúsítvány, amelyet exportálni, válassza a **Minden tevékenység**és majd az **Exportálás**gombra.
2. Az ügyfél-tanúsítvány exportálása a titkos kulccsal. Ez a *.pfx* fájl. Ellenőrizze, hogy ne feledje, hogy a jelszó (kulcs) ebben a tanúsítványban meg és rögzítése.
3. Az ügyfélszámítógép a *.pfx* fájl másolása Az ügyfélgépen kattintson duplán a *.pfx* fájl kattintva telepítheti. Írja be a jelszót a kért. Ne módosítsa a telepítés helyének.

## <a name="vpnclientconfig"></a>Szakasz 4 - állítsa be a VPN-ügyfél

A virtuális hálózathoz csatlakozik, akkor is kell állítsa be a VPN-ügyfél. Az ügyfél való csatlakozás szükséges az ügyfél-tanúsítvány és a megfelelő VPN ügyfél konfigurációja is. Állítsa be a VPN-ügyfél, hajtsa végre sorrendben az alábbi lépéseket.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>1 rész: A virtuális Magánhálózati ügyfélcsomag konfigurációs létrehozása

1. Az Azure klasszikus portálon virtuális hálózatához, az **Irányítópult** lapon keresse meg a felső sarokban a fontos menüjére. Ügyfél által támogatott operációs rendszerek listájáért lásd: a a [pont webhely kapcsolatok](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN átjáró gyakori kérdések szakaszt. A virtuális Magánhálózati ügyfélcsomag beállítása a Windows rendszerbe épített VPN-ügyfélszoftver konfigurációs adatait tartalmazza. A csomag nem külön szoftver telepítése. A beállítások egyediek a virtuális hálózat, amelyhez csatlakozni szeretne.<br><br>Jelölje ki a letöltési csomagot, amely megfelel az ügyfél operációs rendszere, amelyeken telepítendő:
 - A 32 bites ügyfelek esetén válassza a **Töltse le a 32 bites ügyfélcsomag VPN**.
 - 64 bites ügyfelek jelölje ki **a 64 bites ügyfél VPN csomag letöltése**.

2. Az ügyfél csomag létrehozásához néhány percet vesz igénybe. Miután befejeződött a csomagot, letöltheti a fájlt. A letöltött *.exe* fájl biztonságosan tárolhatók a helyi számítógépre.

3. Miután hoz létre, és töltse le a virtuális Magánhálózati ügyfélcsomag az Azure klasszikus portálról, a ügyfélcsomag telepíthető az ügyfélszámítógép virtuális hálózatához csatlakozni szeretne. Ha a virtuális Magánhálózati ügyfélcsomag telepítése több ügyfélszámítógépek, győződjön meg arról, hogy azok minden is telepítve van az ügyfél-tanúsítványt.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>2 rész: Telepítse a virtuális Magánhálózati konfigurációs csomagot az ügyfél

1. Másolja a vágólapra a konfigurációs fájl helyileg virtuális hálózatához csatlakozzanak, és kattintson duplán az .exe fájl, amelyet a számítógépre. 

2. A csomag magában telepítése után elindíthatja a virtuális Magánhálózati kapcsolatot. A Microsoft által a konfigurációs csomag nincs aláírva. Előfordulhat, hogy a kívánja a csomagot, a szervezet aláírási szolgáltatás használatával jelentkezzen be, vagy jelentkezzen [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)segítségével saját. Célszerű használni a csomag bejelentkezés nélkül az OK gombra. Jó helyen jár, ha a csomag nem be van jelentkezve, egy figyelmeztetés jelenik meg a csomag telepítésekor.

3. Kattintson az ügyfélszámítógépen nyissa meg azt a **Hálózati beállításokat** , majd kattintson a **virtuális Magánhálózati**. Ekkor megjelenik a kapcsolat szerepel a listában. A virtuális hálózat nevét, jelennek meg, hogy fog csatlakozni, és ehhez hasonlóan fog kinézni: 

    ![VPN-ügyfél] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN-ügyfél")


### <a name="part-3-connect-to-azure"></a>3 rész: Azure csatlakoztatása

1. A VNet az ügyfélszámítógépen csatlakozhat nyissa meg azt a virtuális Magánhálózati kapcsolatot, és keresse meg a az Ön által létrehozott virtuális Magánhálózati kapcsolatot. A virtuális hálózat nevével megegyező nevű azt. Kattintson a **Csatlakozás**gombra. Előugró üzenet jelenhet meg, amelyre hivatkozik a tanúsítvány használatával. Ez történik, ha kattintson a **Folytatás** magasabb szintű jogosultságokkal használni. 

2. A **kapcsolat** állapota lapon kattintson a **Csatlakozás** a kapcsolat indítása elemre. Ha **Tanúsítvány kiválasztása** képernyő jelenik meg, ellenőrizze, hogy az ügyfél tanúsítványát megjelenítő azt, amelyiket csatlakozni szeretne. Ha nem, jelölje ki a megfelelő tanúsítványt használja a lefelé mutató nyílra, és kattintson **az OK**gombra.

    ![2 VPN-ügyfél] (./media/vpn-gateway-point-to-site-create/clientconnect.png "Virtuális Magánhálózati kapcsolat-ügyfél")

3. Most már kell létrehozni a kapcsolatot.

    ![3 VPN-ügyfél] (./media/vpn-gateway-point-to-site-create/connected.png "Virtuális Magánhálózati ügyfél kapcsolat 2")

### <a name="part-4-verify-the-vpn-connection"></a>4 rész: A virtuális Magánhálózati kapcsolat ellenőrzése

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

Ha azt szeretné, hogy virtuális hálózatokról további információt, lásd: a [Virtuális hálózati dokumentáció](https://azure.microsoft.com/documentation/services/virtual-network/) lapon.
