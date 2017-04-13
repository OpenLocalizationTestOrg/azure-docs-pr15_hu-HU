<properties
   pageTitle="Több virtuális Magánhálózati átjáró hely közötti kapcsolatok hozzáadása egy virtuális hálózaton, az erőforrás-kezelő telepítési modell az Azure portálon |} Microsoft Azure"
   description="Több elem webhely S2S kapcsolatok hozzáadása egy virtuális Magánhálózati átjárót, amely tartalmaz egy meglévő kapcsolat"
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>



# <a name="add-a-site-to-site-connection-to-a-vnet-with-an-existing-vpn-gateway-connection"></a>Webhely kapcsolat hozzáadása egy VNet VPN-átjáró meglévő kapcsolattal

> [AZURE.SELECTOR]
- [Erőforrás-kezelő - portál](vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)
- [Klasszikus - PowerShell](vpn-gateway-multi-site.md)

Ez a cikk végigvezeti webhely (S2S) kapcsolatok felvétele egy virtuális Magánhálózati átjárót, amely tartalmaz egy meglévő kapcsolat az Azure portálon keresztül. Az ilyen típusú kapcsolat gyakran a "több webhelyen" konfiguráció nevezik. 

Ez a cikk S2S kapcsolat hozzáadása egy VNet, amely már tartalmaz egy S2S kapcsolat, a csatlakozási pont-webhely vagy a VNet-VNet kapcsolat is használhatja. Vannak bizonyos korlátozások fel a kapcsolatot. Jelölje be az [első lépések](#before) a jelen cikk című a konfigurációban megkezdése előtt ellenőrizze. 

Ez a cikk az erőforrás-kezelő telepítési modell használatával létrehozott VNets RouteBased VPN az átjárók rendelkező vonatkozik. Ezeket a lépéseket nem vonatkoznak készült ExpressRoute /-webhelyek coexisting csatlakozási beállításokat. Lásd: a [készült ExpressRoute/S2S coexisting kapcsolatok](../expressroute/expressroute-howto-coexist-resource-manager.md) coexisting kapcsolatok információt.

### <a name="deployment-models-and-methods"></a>Telepítési modellek és módszerek

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

Az alábbi táblázat frissítjük új cikkekben, és további eszközök lesznek elérhetők, ebben a konfigurációban. Ha egy cikk áll rendelkezésre, azt hivatkozás közvetlenül az ebből a táblából.

[AZURE.INCLUDE [vpn-gateway-table-multi-site](../../includes/vpn-gateway-table-multisite-include.md)] 


## <a name="before"></a>Első lépések

Ellenőrizze az alábbiakat:

- Nem készít egy készült ExpressRoute/S2S coexisting kapcsolatot.
- Ha létrehozott az erőforrás-kezelő telepítési modell használata egy meglévő kapcsolat virtuális hálózat.
- A virtuális hálózati átjáró esetében a VNet RouteBased. Ha egy PolicyBased virtuális Magánhálózati átjárót, kell a virtuális hálózati átjáró törlése és RoutBased szerint új VPN átjáró létrehozása.
- A címtartományokat közül egyik sem a VNets e VNet csatlakozó valamelyike átfedik egymást.
- Ha kompatibilis VPN eszköz és olyannak, aki tudja állítani. Című témakörben [olvashat a virtuális Magánhálózati eszközöket](vpn-gateway-about-vpn-devices.md). Ha nem ismeri a virtuális Magánhálózati eszköze konfigurálása, vagy nem ismeri a tartományokat található IP-címét a helyszíni hálózati konfigurációja, a koordinálása másokkal, akik nyújtanak a adatai jelennek meg a szükséges.
- A virtuális Magánhálózati eszköz van külső felekkel szemben lévő nyilvános IP-címet. Az IP-cím nem található meg az mögött egy hálózati címfordítást.


## <a name="part1"></a>1 rész - kapcsolat beállítása

1. Böngészőben, nyissa meg az [Azure portálra](http://portal.azure.com) , és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson az **összes erőforrás** , és keresse meg a **virtuális hálózati átjáró** erőforrások a listából, és kattintson rá.
3. A **virtuális hálózati átjáró** lap kattintson a **kapcsolatok**gombra.

    ![Kapcsolatok lap](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/connectionsblade.png "Connections blade")<br>

4. Kattintson a **kapcsolatok** lap **+ Hozzáadás gombra**.

    ![Kapcsolat gomb felvétele](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addbutton.png "Add connection button")<br>

5. A **kapcsolat hozzáadása** lap töltse ki az alábbi mezőket:
    - **Neve:** Meg szeretné adni a webhely létrehozásakor a kapcsolat neve.
    - **Kapcsolat típusa:** Válassza a **webhely (IPsec)**.

    ![Kapcsolat a lap hozzáadása](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/addconnectionblade.png "Add connection blade")<br>

## <a name="part2"></a>2 rész – a helyi hálózati átjáró hozzáadása

1. Kattintson a **helyi hálózati átjáró** ***választható a helyi hálózati átjáró***. Ekkor megnyílik a **választható a helyi hálózaton átjáró** lap.

    ![Válassza a helyi hálózati átjáró](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/chooselng.png "Choose local network gateway")<br>
2. Kattintson a **Új létrehozása** a **Létrehozás helyi hálózaton átjáró** lap megnyitásához.

    ![Hozzon létre helyi hálózaton átjárók lap](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/createlngblade.png "Create local network gateway")<br>

3. Kattintson a **Létrehozás helyi hálózaton átjáró** lap töltse ki az alábbi mezőket:
    - **Neve:** Meg szeretné adni a helyi hálózati átjáró erőforrás neve.
    - **IP-címe:** A nyilvános IP-címe a virtuális Magánhálózati eszközt, a webhelyet, amely, amelyhez csatlakozni szeretne.
    - **Terület cím:** A címterület, amelyet az új helyi hálózati hely átirányítását.
4. Kattintson a **Létrehozás helyi hálózaton átjáró** lap, a módosítások mentéséhez kattintson az **OK gombra** .

## <a name="part3"></a>3 rész - hozzáadása a megosztott billentyűt, és a kapcsolat létrehozása

1. A **kapcsolat hozzáadása** a lap adja hozzá a kapcsolat létrehozásához használni kívánt megosztott kulcsot. A megosztott kulcs első VPN mobileszközről vagy egyik be ide és konfigurálnia VPN eszközét, hogy ugyanazt a megosztott kulcsot. Figyeljen arra, hogy a billentyűk megegyeznek pontosan.

    ![Megosztott kulcs](./media/vpn-gateway-howto-multi-site-to-site-resource-manager-portal/sharedkey.png "Shared key")<br>
2. A lap alján kattintson az **OK gombra** a kapcsolat létrehozása lehetőséget.

## <a name="part4"></a>Rész 4 – a virtuális Magánhálózati kapcsolat ellenőrzése

Ellenőrizheti, hogy a virtuális Magánhálózati kapcsolatot a portálon vagy PowerShell használatával.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]


## <a name="next-steps"></a>Következő lépések

- A kapcsolat elkészülte virtuális gépeken futó hozzáadhatja a virtuális hálózatok. Lásd: a virtuális gépeken futó [tanulási javaslat](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) további információt.
