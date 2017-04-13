<properties 
   pageTitle="Az Azure klasszikus portálon VPN átjáró beállítása |} Microsoft Azure"
   description="Ebben a cikkben megismerkedhet a virtuális hálózat virtuális Magánhálózati átjáró beállítása és módosítása az átjárók VPN útválasztási típusa keresztül."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/11/2016"
   ms.author="cherylmc" />

# <a name="configure-a-vpn-gateway-for-the-classic-deployment-model"></a>A klasszikus telepítési modell VPN átjáró beállítása


Ha szeretné az Azure és a helyszíni helye közötti biztonságos határokon helyszíni kapcsolat létrehozása, meg kell átjáró virtuális Magánhálózati kapcsolat beállítása. A klasszikus telepítési modell az átjárók egyike lehet VPN útválasztási kétféle: statikus vagy dinamikus. A típus, kiválaszthatja, hogy a csomagja hálózaton tervezés és a helyszíni VPN eszköz használni kívánt függ. 

Néhány csatlakozási beállításokat, például a webhely-pont kapcsolat, például a dinamikus útválasztási átjáró szükség. Ha azt szeretné, a kapcsolatok pont webhely (P2S) és a webhely (S2S) kapcsolat támogatási az átjáró beállítása, dinamikus útválasztási átjáró beállítása, annak ellenére, hogy a webhely konfigurálható mindkét átjáró VPN útválasztási típusa van. 

Ezenkívül kell győződnie arról, hogy az eszköz a kapcsolat használni kívánt támogatja-e az VPN útválasztási típust szeretne létrehozni. Című témakörben [olvashat a virtuális Magánhálózati eszközöket](vpn-gateway-about-vpn-devices.md).


**Erről a cikkről** 

Ez a cikk a klasszikus telepítési modell [Klasszikus portal](https://manage.windowsazure.com) (nem az Azure-portálra) készült. 

**Azure környezetben modellek**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="configuration-overview"></a>Beállítások áttekintése

Az alábbi lépésekkel végigvezetik Önt a virtuális Magánhálózati átjáró beállítása az Azure klasszikus portálon. Ezeket a lépéseket a klasszikus telepítési modell használatával létrehozott virtuális hálózatok átjárókat vonatkozik. Jelenleg elérhető az Azure-portálon közül nem mindegyik átjárók beállításokat. Vannak, ha azt fogja hozzon létre egy új az Azure-portálra vonatkozó útmutatást.


1. [A VNet a virtuális Magánhálózati átjáró létrehozása](#create-a-vpn-gateway)

1. [A virtuális Magánhálózati eszköz konfigurálásával kapcsolatos információk összegyűjtése](#gather-information-for-your-vpn-device-configuration)

1. [A virtuális Magánhálózati eszköz beállítása](#configure-your-vpn-device)

1. [A helyi hálózati tartományok és a virtuális Magánhálózati gateway IP-cím ellenőrzése](#verify-your-local-network-ranges-and-vpn-gateway-ip-address)

### <a name="before-you-begin"></a>Első lépések

Mielőtt beállítaná az átjáró, akkor először meg kell létrehoznia a virtuális hálózat. Hozzon létre egy virtuális hálózati határokon helyszíni kapcsolat [beállítása egy virtuális hálózati hely közötti virtuális Magánhálózati kapcsolatot](vpn-gateway-site-to-site-create.md)vagy a [pont-webhely virtuális Magánhálózati kapcsolatot virtuális hálózat konfigurálása](vpn-gateway-point-to-site-create.md)című lépést. Ezután kövesse az alábbi lépéseket a virtuális Magánhálózati átjáró beállítása, és állítsa be a VPN eszköz szükséges információk összegyűjtése. 

Ha már van egy virtuális Magánhálózati átjárót, és meg szeretné változtatni a virtuális Magánhálózati útválasztási típusa, megtudhatja, [hogy miként az átjáró a virtuális Magánhálózati útválasztási típusának módosítása](#how-to-change-the-vpn-routing-type-for-your-gateway).

## <a name="create-a-vpn-gateway"></a>Virtuális Magánhálózati átjáró létrehozása

1. Az [Azure klasszikus portál](https://manage.windowsazure.com) **hálózatok** lapon ellenőrizze, hogy az Állapot oszlopban a virtuális hálózat **létrehozott**.

1. Kattintson a **név** oszlopban a virtuális hálózat nevét.

1. Az **Irányítópult** lapon figyelje meg, hogy a VNet nincs konfigurálva még az átjárók. Ezt az állapotot a műveletek elvégzéséhez az átjáró beállítása menet közben megjelenik.

![Az átjáró nem létrehozva](./media/vpn-gateway-configure-vpn-gateway-mp/IC717025.png)


Ezután a lap alján kattintson az **Átjáró létrehozása**gombra. *Statikus útválasztás* , illetve *a dinamikus útválasztás*kijelölhet. A virtuális Magánhálózati útválasztási típustól néhány tényezőktől függ. Például VPN eszköze támogatja, és hogy szüksége támogatja a webhely pont-kapcsolatokat. Jelölje be a [VPN eszközök kapcsolatos virtuális a hálózati kapcsolat](vpn-gateway-about-vpn-devices.md) : Ellenőrizze a virtuális Magánhálózati útválasztási típus van szüksége. Az átjáró létrehozása után közötti átjáró VPN-típusokat útválasztási nélkül törlése, és hozza létre újra az átjáró nem módosítható. Amikor a rendszer kéri, hogy erősítse meg, hogy az átjáró létrehozott, kattintson az **Igen**gombra.

![Átjáró VPN útválasztási típusa](./media/vpn-gateway-configure-vpn-gateway-mp/IC717026.png)

Amikor az átjáró hoz létre, és figyelje meg a lapon az átjáró ábra sárga változik, és megjelenik a *Átjáró létrehozása*. Az átjáró létrehozásához 45 perc is eltarthat. Várja meg, amíg az átjáró befejeződött, a továbblépés előtt előre az egyéb beállításait.

![Átjáró létrehozása](./media/vpn-gateway-configure-vpn-gateway-mp/IC717027.png)

Az átjáró *Csatlakozás*változik, amikor Ön is összegyűjtött szüksége van a virtuális Magánhálózati eszköz.

![Csatlakozás átjáró](./media/vpn-gateway-configure-vpn-gateway-mp/IC717028.png)

## <a name="gather-information-for-your-vpn-device-configuration"></a>A virtuális Magánhálózati eszköz konfigurálásával kapcsolatos információk összegyűjtése

Az átjáró létrehozása után gyűjtse össze a virtuális Magánhálózati eszköz konfigurálásával kapcsolatos információk. Ezt az információt a virtuális hálózathoz az **Irányítópult** lapon található:

1. **Gateway IP-cím –** Az IP-címet az **Irányítópult** lapon találhatók. Nem láthatja addig, amíg az átjáró befejezte a létrehozása után.

1. **A megosztott kulcs –** Kattintson **A kulcs kezelése** a képernyő alján. Kattintson a billentyű lenyomásával másolja a vágólapra, majd illessze be és mentse a kulcsot melletti ikonra. Ez a gomb csak akkor működik, ha egy egyetlen S2S VPN-csatorna. Ha több S2S VPN bújtatás, használja az *Első virtuális hálózati átjáró megosztott kulcs* API vagy PowerShell-parancsmag.

![Kulcs kezelése](./media/vpn-gateway-configure-vpn-gateway-mp/IC717029.png)


## <a name="configure-your-vpn-device"></a>A virtuális Magánhálózati eszköz beállítása

Miután elkészült az előző lépéseket, vagy a hálózati rendszergazda kell a virtuális Magánhálózati eszköz beállítása a kapcsolat létrehozásához. További információt a virtuális Magánhálózati eszközök, lásd: [A virtuális Magánhálózati eszközök virtuális a hálózati kapcsolat](vpn-gateway-about-vpn-devices.md) .

A virtuális Magánhálózati eszköz beállítása után a VNet az irányítópult lapon megtekintheti a frissített kapcsolat adatait.

Futtathatja a kapcsolat tesztelése az alábbi parancsok egyikére:

|                      | Cisco ASA             | Cisco ISR/automatikus rendszer-Helyreállítás         | Boróka SSG/ISG | Boróka SRX-Leírásban/J                            |
|----------------------|-----------------------|-----------------------|-----------------|------------------------------------------|
| **Jelölje be a fő mód társítások**  | titkosítási isakmp megjelenítése nyelvű | titkosítási isakmp megjelenítése nyelvű | ike cookie beolvasása  | biztonsági megjelenítése ike biztonsági-hozzárendelés   |
| **Társítások ellenőrzése** | titkosítási ipsec megjelenítése nyelvű  | titkosítási ipsec megjelenítése nyelvű  | rendszergazdai beszerzése          | biztonsági ipsec biztonsági-hozzárendelés megjelenítése |


## <a name="verify-your-local-network-ranges-and-vpn-gateway-ip-address"></a>A helyi hálózati tartományok és a virtuális Magánhálózati gateway IP-cím ellenőrzése

### <a name="verify-your-vpn-gateway-ip-address"></a>A virtuális Magánhálózati gateway IP-cím ellenőrzése

Csatlakozás megfelelő átjáró a virtuális Magánhálózati eszköze IP-címe kell megfelelően beállítania a helyi hálózaton a határokon helyszíni konfigurációban megadott. Általában ez úgy van beállítva a webhelyen a webhely-beállítási folyamat során. Ha egy másik eszközt a korábban használt a helyi hálózaton, vagy az IP-cím módosítását, a helyi hálózat, azonban a helyes Gateway IP-cím megadásához beállításainak szerkesztése.

1. Ellenőrizze az átjáró IP-címének, a portál bal oldali ablaktáblában kattintson a **hálózatok** , és válassza a **Helyi hálózatok** elemre a lap tetején. Láthatja, hogy az egyes helyi hálózaton, Ön által létrehozott VPN-átjáró címét. Az IP-cím módosításához jelölje ki a VNet, és kattintson a **szerkesztése** elemre a lap alján.

1. **A helyi hálózaton részletek megadása** lapon az IP-cím módosítása lehetőséget, és kattintson a lap alján a következő nyílra.

1. **Adja meg a címterületet** lapján kattintson a jobb alsó sarkában a beállítások mentéséhez a bejelölést.

### <a name="verify-the-address-ranges-for-your-local-networks"></a>A helyi hálózat a címtartományokat ellenőrzése

A helyes forgalmat keresztül az átjáró a helyszíni helyre frissítenie kell ellenőrizze, hogy minden IP-címtartományokat meg van adva. Minden olyan tartomány szerepelnie kell a **Helyi hálózat** Azure konfigurációban. Attól függően, hogy a helyszíni helyre a hálózati konfigurálásról Ez lehet egy kicsit nagy feladatot. Az IP-címet, amely tartalmazza a felsorolt tartományok kötött forgalmat a virtuális hálózati VPN átjáró keresztül küld. A tartomány, amely a listában, nem kell lenniük saját tartományokat, annak ellenére is szükség van ellenőrizze, hogy a helyszíni konfigurációs kaphatnak a bejövő forgalmat.

Szeretne felvenni, vagy a helyi hálózaton a tartományok szerkesztését, kövesse az alábbi lépéseket.

1. A helyi hálózat IP-címtartományok szerkesztéséhez a portál bal oldali ablaktáblában kattintson a **hálózatok** , és válassza a **Helyi hálózatok** elemre a lap tetején. A portálon a megtekintheti a tartományokat, amely már szerepel a felsorolásban legegyszerűbben a **Szerkesztés** lapon. A tartományok, jelölje be a VNet és című a lap alján a **Szerkesztés** gombra.

1. **A helyi hálózaton részletek megadása** lapon nem végezze el a módosításokat. Kattintson a lap alján a következő nyílra.

1. **A címterület megadása** lapon a kívánt módosításokat hálózati cím helyet. Válassza a be van jelölve a konfiguráció mentéséhez.

## <a name="how-to-view-gateway-traffic"></a>Átjáró forgalom megtekintése

Megtekintheti az átjáró és az átjáró forgalmat a hálózati virtuális **Irányítópult** lapon.

Az **Irányítópult** lapon tekintheti meg a következőket:

- Lépés van, az átjárót, mind az adatokat, majd az adatok ki keresztül adatok mennyiségét.

- A DNS-kiszolgálók virtuális hálózatához megadott nevét.

- Az átjáró és a virtuális Magánhálózati eszköz közötti kapcsolat.

- A megosztott kulcs, amely a virtuális Magánhálózati eszköze átjáró kapcsolatot beállítani.


## <a name="how-to-change-the-vpn-routing-type-for-your-gateway"></a>Az átjáró a virtuális Magánhálózati útválasztási típusának módosítása

Néhány csatlakozási beállítások csak az átjáró útválasztási bizonyos érhetők el, mivel előfordulhat, hogy az átjáró egy meglévő VPN átjáró útválasztási típusú VPN módosítani szeretné. Például érdemes hozzáadása csatlakozási pont-webhely statikus az átjárók tartalmazó már létező webhely kapcsolat. Webhely-pont kapcsolatok dinamikus az átjárók szükség. Ez azt jelenti, hogy P2S kapcsolat beállítása, módosítása az átjáró VPN útválasztási írja be a statikus dinamikus van.

Az átjárók VPN útválasztási típusa módosítani szeretné, ha, fogja a meglévő átjáró törlése, és kattintson az új átjáró létrehozása új útválasztási típusú. Nem kell a teljes virtuális hálózat módosítása az átjáró útválasztási törlése.

Mielőtt módosítja a átjáró VPN útválasztási típusa, feltétlenül ellenőrizze, hogy a virtuális Magánhálózati eszköz támogatja a használni kívánt útválasztási típusát. Új útválasztási konfigurációs minták letöltése, és jelölje be a VPN eszköz követelmények, akkor olvassa el a [Virtuális Magánhálózati eszközök kapcsolatos virtuális a hálózati kapcsolat](vpn-gateway-about-vpn-devices.md).

>[AZURE.IMPORTANT] Amikor töröl egy virtuális hálózati virtuális Magánhálózati átjárót, az átjáró rendelt a virtuális csomagot. Amikor meg újra az átjáró, egy új virtuális van hozzárendelve.

1. **Törölje a meglévő virtuális Magánhálózati átjárót.**

    A virtuális hálózatához **Irányítópult** lapon keresse meg az oldal aljára, és kattintson az **Átjáró törlése**gombra. Várja meg, hogy az átjáró törölve lett-e értesítést. Amikor megjelenik az értesítés, hogy az átjáró törlése után a képernyőn, új átjárókat készíthet.

1. **Hozzon létre egy új virtuális Magánhálózati átjárót.**

    Az eljárás használatával az oldal tetején hozzon létre egy új átjáró: [a virtuális Magánhálózati átjáró létrehozása](#create-a-vpn-gateway).


## <a name="next-steps"></a>Következő lépések

A virtuális hálózat virtuális gépeken futó vehet. Megtudhatja, [hogy miként hozhat létre egy egyéni virtuális számítógépre](../virtual-machines/virtual-machines-windows-classic-createportal.md).

A webhely-pont virtuális Magánhálózati kapcsolat konfigurálásához, című témakörben olvashat [a pont-webhely virtuális Magánhálózati kapcsolat beállítása](vpn-gateway-point-to-site-create.md).

 
