<properties 
   pageTitle="Hálózati biztonsági csoportok – portál elhárítása |} Microsoft Azure"
   description="Megtudhatja, hogy miként kapcsolatos hibák elhárítása az Azure erőforrás-kezelő telepítési modell az Azure-portálon hálózati biztonsági csoportokat."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Az Azure-portálon hálózati biztonsági csoportok – problémamegoldás

> [AZURE.SELECTOR]
- [Azure portál](virtual-network-nsg-troubleshoot-portal.md)
- [A PowerShell](virtual-network-nsg-troubleshoot-powershell.md)

Ha megadott hálózati biztonsági csoportok (NSGs) a virtuális gépen (virtuális), és virtuális csatlakozási problémákat tapasztal, ez a cikk áttekintést nyújt további hibaelhárítási NSGs diagnosztika funkciókhoz.

NSGs lehetővé teszi a forgalom típusú szabályozhatja, hogy a munkafolyamat és kijelentkezés a virtuális gépeken futó (VMs). Az Azure virtuális hálózati (VNet) vagy hálózati kapcsolatok (hálózati) alhálózat NSGs alkalmazhatók. A hatékony szabályokat alkalmazza a hálózati kártya is megtalálható a NSGs egy hálózati kártya alkalmazott szabályok és az alhálózathoz csatlakoztatva van egy összesítést. Szabályok át ezeket a NSGs időnként ütköznek egymással, és hatással lehet a virtuális a hálózati kapcsolat.  

A virtuális NIC alkalmazva az összes hatékony biztonsági szabály megtekintheti a NSGs, a. Ez a cikk bemutatja, hogyan szabályok használatával a Azure erőforrás-kezelő telepítési modell virtuális kapcsolódási problémák elhárítása. Ha nem ismeri a VNet és NSG fogalmak, olvassa el a [virtuális hálózati](virtual-networks-overview.md) és a [hálózati biztonsági csoportok](virtual-networks-nsg.md) – áttekintés cikkeket.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Hatékony biztonsági szabályok segítségével virtuális forgalom folyamat kapcsolatos hibák elhárítása

Az alkalmazási példát, amely követi a leggyakoribb csatlakozási probléma példája:

A virtuális *VM1* nevű *Alhalozat_1* belül egy névvel ellátott *WestUS-VNet1*VNet nevű alhálózat része. A virtuális portot 3389 felett RDP használatával csatlakozhat kísérlet sikertelen lesz. A hálózati kártya *VM1-NIC1* , mind az alhálózathoz *Alhalozat_1*NSGs alkalmazzák. TCP-port 3389 forgalom engedélyezett a NSG társított a hálózati kapcsolaton *VM1-NIC1*, azonban a TCP VM1 a port 3389 sikertelen a ping.

Ez a példa olyan portot 3389, miközben az alábbi lépéseket minden olyan portot határozza meg a bejövő és kimenő csatlakozási hibák használható.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Virtuális gép hatékony biztonsági szabályok megtekintése

Hajtsa végre az alábbi lépések végrehajtásával kapcsolatos hibák elhárítása NSGs egy virtuális:

A hálózati a virtuális magát a is megtekintheti a hatékony biztonsági szabályok teljes listáját. Hozzáadása, módosítása, és hálózati kártya és a alhálózat NSG szabályok törlése a hatékony szabályok lap, ha van jogosultsága a következő műveletek hajthatók végre.

1. Jelentkezzen be az Azure-portált a https://portal.azure.com.
2. Kattintson a **További szolgáltatások**elemre, majd a megjelenő listában kattintson a **virtuális gépeken futó** .
3. Jelölje ki a virtuális kapcsolatos hibák elhárítása a megjelenő listából, és egy virtuális, a beállítások lap jelenik meg.
4. Kattintson a **diagnosztizálása & problémamegoldó** , és válassza az általános probléma. Ebben a példában **nem sikerül megnyitni a Windows virtuális** be van jelölve. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. A problémát, a lépések jelennek meg, az alábbi képen látható módon: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Kattintson a *hatékony biztonsági csoport szabályok* a javasolt lépéseket listájában.

6. Az **első hatékony biztonsági szabályok** lap jelenik meg, az alábbi képen látható módon:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Az alábbi szakaszok a kép láthat:

    - **Hatókör:** A virtuális 3 lépésben kiválasztott *VM1*beállításához.
    - **Hálózati kapcsolat:** *VM1-NIC1* be van jelölve. A virtuális beállíthatja, hogy több hálózati kapcsolaton (hálózati). Minden egyes hálózati beállíthatja, hogy egyedi hatékony biztonsági szabályokat. Ha a hiba elhárításához, szüksége lehet hatékony biztonsági szabályokat meg az egyes adaptert
    - **NSGs tartozó:** A hálózati kártya és a az alhálózathoz csatlakozik a hálózati kártya NSGs alkalmazhatók. A képen egy NSG alkalmazása a hálózati kártya és a hozzá kapcsolódó alhálózat. Kattinthat a NSG neveket közvetlenül a NSGs szabályok módosításához szükséges engedélyekkel.
    - **VM1-nsg lap:** A képen látható, a szabályok listáját a NSG a adaptert alkalmazott szolgál. Amikor létrejön egy NSG több alapértelmezett szabályok Azure hozza létre. Nem távolítható el az alapértelmezett szabályokat, de magasabb prioritású szabályokkal felülbírálhatja őket. Alapértelmezett szabályok kapcsolatos további információért olvassa el a [NSG áttekintése](virtual-networks-nsg.md#default-rules) -cikket.
    - **Cél oszlop:** Néhányat a szabályok formázni a szöveget az a oszlopban, míg mások rendelkeznek a cím előtagot. A szöveg pedig a szabály létrehozásakor jött létre automatikusan alkalmazott alapértelmezett címkék neve. A címkék e-mailek több prefixumokban megjelenítő rendszer által biztosított azonosítóját. A **cím prefixumokban** lap előtagját címkével ellátott *AllowInternetOutBound*, például a szabály kiválasztásával sorolja fel.
    - **Letöltése:** A szabályok listáját hosszú lehet. Offline elemzéshez a szabályok .csv fájl letöltheti úgy **Töltse le** a fájlt.
    - **AllowRDP** A bejövő szabályok: Ez a szabály lehetővé teszi, hogy a virtuális RDP-kapcsolatot.
7. Kattintson a **Alhalozat_1-NSG** lapján megtekintheti a NSG az alhálózathoz, alkalmazza a hatékony szabályok, az alábbi képen látható módon: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Figyelje meg a *denyRDP* **Bejövő** szabályt. A bejövő szabályok az alhálózathoz alkalmazott szabályokat alkalmazza a hálózati kapcsolaton, mielőtt értékeli ki. A Megtagadás szabályt alkalmazza a az alhálózathoz, mivel TCP 3389 csatlakozás az értekezlet-összehívást sikertelen lesz, mert van, a hálózati kártya engedélyezése szabály soha nem értékeli ki. 

    A *denyRDP* szabályt az oka Miért hibás a RDP-kapcsolatot. Eltávolítást, célszerű a probléma megoldásához.

    >[AZURE.NOTE]Ha a virtuális a hálózati kártya társított nem fut, vagy a hálózati vagy alhálózat NSGs még nem alkalmazott, nincs szabály jelennek meg.

8. NSG szabályok szerkesztéséhez kattintson a *Alhalozat_1-NSG* **Tartozó NSGs** szakaszában.
   Ekkor megnyílik a **Alhalozat_1-NSG** lap. A szabályok szerkesztheti közvetlenül a **Bejövő szabályok**gombra kattintva.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. A **Alhalozat_1-NSG** eltávolítása a *denyRDP* bejövő szabályt, és egy *allowRDP* szabály hozzáadása, után a hatékony szabályok lista néz ki a következő képen:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Győződjön meg arról, hogy a TCP port 3389 meg nyitva egy RDP-kapcsolat a virtuális megnyitása vagy a PsPing eszközzel. További információ a PsPing olvasásával [PsPing letöltési oldalon](https://technet.microsoft.com/sysinternals/psping.aspx)talál.

### <a name="view-effective-security-rules-for-a-network-interface"></a>A hálózati illesztő hatékony biztonsági szabályok megtekintése

A virtuális forgalom folyamat hatással van az egy adott hálózati, ha az alábbi lépéseket követve megtekintheti a hatékony szabályok teljes listáját a hálózati a hálózati kapcsolatok környezetből:

1. Jelentkezzen be az Azure-portált a https://portal.azure.com.
2. Kattintson a **További szolgáltatások**elemre, majd a megjelenő listában kattintson a **hálózati kapcsolatok** .
3. Jelölje ki a hálózati kapcsolat. Az alábbi képen egy hálózati kártya elnevezett *VM1-NIC1* be van jelölve.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Figyelje meg, hogy a **hatókör** a kijelölt kapcsolat értéke. Többet szeretne tudni a további információt a látható, olvassa el a lépés 6: Ez a cikk **NSGs verzióval – hibaelhárítás egy virtuális** szakasza.

    >[AZURE.NOTE] Egy NSG törlődik a hálózati kapcsolat, a alhálózat NSG esetén továbbra is hatékony adott hálózati adapteren Ebben az esetben a kimenet volna csak akkor látható a alhálózat NSG szabályokat. Szabályok csak akkor jelenik meg, ha a hálózati kártya egy virtuális van társítva.

4. A hálózati és alhálózat kapcsolódó NSGs közvetlenül szerkesztése szabályokat. Megtudhatja, hogyan, olvassa el a lépés: Ez a cikk a **hatékony biztonsági szabályok megtekintése virtuális géphez** szakaszának 8.

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>A hálózati biztonsági csoport (NSG) hatékony biztonsági szabályok megtekintése

Amikor NSG szabályok módosításával, érdemes, tekintse át a milyen következményekkel járnak a felvételről szóló egy adott virtuális szabályokat. A hatékony biztonsági szabályok teljes listáját megtekintheti az egy adott NSG alkalmazott, az összes NIC anélkül, hogy a környezetben a megadott NSG lap a válthat. Hatékony szabályok belül egy NSG kapcsolatos hibák elhárítása, hajtsa végre az alábbi lépéseket:

1. Jelentkezzen be az Azure-portált a https://portal.azure.com.
2. Kattintson a **További szolgáltatások**, majd a megjelenő listában kattintson a **hálózati biztonsági csoportokat** .
3. Jelölje ki az NSG. Az alábbi képen egy névvel ellátott VM1-nsg NSG van kijelölve.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Az alábbi szakaszok a korábbi kép láthat:

    - **Hatókör:** Állítsa be a kijelölt NSG.
    - **Virtuális gépen:** Egy NSG alhálózat történő alkalmazásakor a alhálózathoz csatlakoztatott összes VMs csatolt hálózati adaptereken érvényes. Ebben a listában látható e NSG alkalmazott összes VMs. Választhat a minden virtuális a listából.

    >[AZURE.NOTE] Ha egy NSG csak egy üres alhálózathoz alkalmazza, VMs nem szerepel a listában. Ha egy NSG alkalmazva van egy hálózati, amely nem egy virtuális társítva, ezek NIC is nem szerepelnek. 
    - **Hálózati kapcsolat:** A virtuális beállíthatja, hogy több hálózati kapcsolaton. A kijelölt virtuális csatlakoztatott hálózati kártya kijelölése
    - **AssociatedNSGs:** A hálózati bármikor, akár két hatékony NSGs, egy alkalmazott a hálózati kártya és a másik alhálózathoz van. Bár a hatókör VM1-nsg, ha a hálózati kártya van egy hatékony alhálózat NSG van kiválasztva, akkor a kimenet mindkét NSGs jelennek meg.
4. A hálózati vagy alhálózat társított NSGs közvetlenül szerkesztése szabályokat. Megtudhatja, hogyan, olvassa el a lépés: Ez a cikk a **hatékony biztonsági szabályok megtekintése virtuális géphez** szakaszának 8.

Többet szeretne tudni a további információt a látható, olvassa el a lépés 6: Ez a cikk **hatékony biztonsági szabályok megtekintése virtuális géphez** szakasza.

>[AZURE.NOTE] Bár egy alhálózat és a hálózati kártya minden lehet csak az egyik NSG alkalmazza őket, egy NSG lehet több és több alhálózat van társítva.

## <a name="considerations"></a>Megfontolandó szempontok

Kapcsolódási problémák elhárítása során, vegye figyelembe az alábbiakat:

- Alapértelmezett NSG szabályok tiltható le a bejövő hozzáférése az internetről, és azt csak akkor VNet engedélyezik bejövő forgalom. Szabályok kell kifejezetten hozzá bejövő elérésének engedélyezése az internetről, szükség szerint.
- Nincsenek egy virtuális a hálózati kapcsolat lefagyását okozza NSG biztonsági szabályok, ha a probléma oka az, hogy lehet meg:
    - A virtuális operációs rendszerben futó tűzfal szoftver
    - Virtuális készülékek vagy a forgalmat a helyszíni konfigurálva útvonalak. Internetes forgalmat a helyszíni keresztül kényszerített tunneling átirányítható. Ezt a beállítást, attól függően, hogy a forgalmat a helyszíni hálózati eszközök kapacitása kezelésének nem működik a virtuális egy RDP/SSH kapcsolat az internetről. Olvassa el a [Hibaelhárítási útvonalak](virtual-network-routes-troubleshoot-powershell.md) a cikkből megtudhatja, hogy miként problémák útvonal, előfordulhat, hogy a forgalmat és kijelentkezés a virtuális kell késleltetése. 
- Ha VNets, alapértelmezés szerint van peered, a a VIRTUAL_NETWORK címke automatikusan kiterjesztése peered VNets prefixumokban tartalmazzák. Megtekintheti a prefixumokban **ExpandedAddressPrefix** listájában VNet peering kapcsolódási kapcsolatos problémák megoldásához. 
- Hatékony biztonsági szabályok csak van-e egy társított a virtuális hálózati kártya és vagy alhálózat NSG jelennek meg. 
- Ha nem a hálózati kártya társított NSGs vagy alhálózat és rendelkezik egy nyilvános IP-címet a virtuális rendelve, az összes olyan portot nyissa meg a bejövő és kimenő hozzáférés lesz. Ha a virtuális van egy nyilvános IP-címet, a hálózati és alhálózat NSGs alkalmazásával ajánlott.