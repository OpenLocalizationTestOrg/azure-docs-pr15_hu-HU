<properties 
   pageTitle="Útvonalak - portálon elhárítása |} Microsoft Azure"
   description="Ismerje meg, hogy az erőforrás-kezelő Azure telepítési modell az Azure-portálon útvonalak hibák elhárítása."
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

# <a name="troubleshoot-routes-using-the-azure-portal"></a>Az Azure-portálon útvonalak – problémamegoldás

> [AZURE.SELECTOR]
- [Azure portál](virtual-network-routes-troubleshoot-portal.md)
- [A PowerShell](virtual-network-routes-troubleshoot-powershell.md)

Ha a hálózati kapcsolat problémákat szeretné vagy át az Azure virtuális gép (virtuális) tapasztal, akkor útvonalak érintő a virtuális forgalom. Ez a cikk áttekintést nyújt további hibaelhárítási útvonalak diagnosztika funkciók.

Útvonal alhálózat társított és a hálózati adaptereken (hálózati) az adott alhálózat érvényesek. A következő típusú útvonalak is alkalmazható egyes hálózati kapcsolat:

- **Rendszer útvonalak:** Alapértelmezés szerint minden Azure virtuális hálózatban (VNet) létrehozott alhálózat rendelkezik a rendszer útvonal táblákat, amelyek lehetővé teszik a helyi VNet forgalom, a helyszíni forgalom e VPN átjárók és internetes forgalmat. Rendszer útvonalak is léteznek peered VNets.
- **BGP útvonalak:** Propagált hálózati kapcsolatok készült ExpressRoute vagy a webhely VPN kapcsolaton keresztül. További tudnivalók a [BGP a virtuális Magánhálózati átjárók](../vpn-gateway/vpn-gateway-bgp-overview.md) és [készült ExpressRoute áttekintése](../expressroute/expressroute-introduction.md) cikkek olvasásával útválasztás BGP.
- **Felhasználói útvonalak (UDR):** Hálózati virtuális készülékek vagy a rendszer használata esetén kényszerített tunneling forgalmat a webhely virtuális Magánhálózaton keresztül a helyszíni hálózaton, előfordulhat, hogy van útvonalak felhasználói (UDRs) a alhálózat útvonal táblázat társított. Ha nem ismeri a UDRs, olvassa el a [felhasználó által definiált útvonalak](virtual-networks-udr-overview.md#user-defined-routes) .

A különböző útvonal, amelyet a hálózati kapcsolat alkalmazhatók Ez lehet nehezen állapíthatók meg, mely összesítő útvonalak érvényesek. Az erőforrás-kezelő Azure telepítési modell problémák megoldására virtuális a hálózati kapcsolat, megtekintheti a hatékony útvonalak hálózati kapcsolat.

## <a name="using-effective-routes-to-troubleshoot-vm-traffic-flow"></a>Hatékony útvonalak használatáról virtuális forgalom folyamat kapcsolatos hibák elhárítása

Ez a cikk a következő esetben példaként bemutatják, hogyan lehet a hatékony útvonalak hálózati kapcsolat hibaelhárítása használja:

A VNet csatlakozik egy virtuális (*VM1*) (*VNet1*, előtag: 10.9.0.0/16) nem sikerül kapcsolódni a VM(VM3) egy újonnan peered VNet (*VNet3*, 10.10.0.0/16 előtag) szereplő. Vannak olyan UDRs vagy BGP nem irányítja VM1-NIC1 hálózati kapcsolat csatlakozik a virtuális alkalmazott, akkor csak a rendszer útvonalak lesz érvényes.

Ez a cikk ismerteti, hogyan okának a kapcsolati hiba Azure az erőforrás-kezelés telepítési modell használata hatékony útvonalak lehetőséget.
A példában a csak a rendszer utakat, miközben ezeket a lépéseket használható határozza meg a bejövő és kimenő csatlakozási hibák egy útvonal fölé.

>[AZURE.NOTE] Ha a virtuális egynél több hálózati csatlakoztatva van, jelölje be az egyes diagnosztizálása és onnan egy virtuális hálózati csatlakozási problémákat tapasztal a NICS hatékony útvonalak.

### <a name="view-effective-routes-for-a-virtual-machine"></a>A virtuális gép hatékony útvonalak megtekintése

Az összesítő útvonalak egy virtuális alkalmazott megtekintéséhez tegye a következőket:

1. Jelentkezzen be az Azure-portált a https://portal.azure.com.
2. Kattintson a **További szolgáltatások**elemre, majd a megjelenő listában kattintson a **virtuális gépeken futó** .
3. Jelölje ki a virtuális kapcsolatos hibák elhárítása a megjelenő listából, és egy virtuális, a beállítások lap jelenik meg.
4. Kattintson a **diagnosztizálása & problémamegoldó** , és válassza az általános probléma. Ebben a példában **nem sikerül megnyitni a Windows virtuális** be van jelölve. 

    ![](./media/virtual-network-routes-troubleshoot-portal/image1.png)

5. A problémát, a lépések jelennek meg, az alábbi képen látható módon: 

    ![](./media/virtual-network-routes-troubleshoot-portal/image2.png)

    Kattintson a *hatékony útvonalak* ajánlott lépéseket listájában.

6. A **hatékony útvonalak** lap jelenik meg, az alábbi képen látható módon:

    ![](./media/virtual-network-routes-troubleshoot-portal/image3.png)

    Ha a virtuális csak egy hálózati, alapértelmezés szerint van jelölve. Ha egynél több hálózati, jelölje ki a hálózati kártya, amelynek meg szeretné tekinteni a hatékony útvonalak.

    >[AZURE.NOTE] Ha a virtuális a hálózati kártya társított nem fut, hatékony útvonalak nem jelenik meg. Csak az első 200 hatékony útvonalak jelennek meg a portálon. A teljes lista kattintson a **Letöltés**gombra. A letöltött .csv fájlból eredmények további is szűrheti.

    Figyelje meg az eredményben a következőket:
    - **Forrás**: az útvonal típusáról. Rendszer útvonalak ábrán *alapértelmezett*, UDRs ábrázolva *felhasználói* és az átjáró útvonalak (statikus vagy BGP) jelennek meg *VPNGateway*.
    - **Állapot**: a hatékony útvonal állapotát jelzi. Lehetséges értékei *aktív* vagy *Érvénytelen*.
    - **AddressPrefixes**: Itt adhatja meg a cím előtagját hatékony útvonal CIDR formában. 
    - **nextHopType**: azt jelzi, a következő Ugrás az adott útvonal. Lehetséges értékei *VirtualAppliance*, *Internet*, *VNetLocal*, *VNetPeering*vagy *Null*. A *Null* értéket **nextHopType** egy UDR az érvénytelen útvonal jelezheti. Például **nextHopType** *VirtualAppliance* és a hálózat virtuális esetén készülék virtuális nem szerepel egy kiépítéstől/futó állam. **NextHopType** *VPNGateway* , és nincs kiépítve/futtatása a megadott VNet az átjáró nem, az útvonal előfordulhat, hogy érvénytelenné válnak.
    
7. Nem látható a *WestUS-VNET3* VNet (előtag 10.10.0.0/16) a *WestUS-VNet1* (előtag 10.9.0.0/16) a képen az előző lépésben az útvonal nem. Az alábbi képen a peering hivatkozás *kapcsolat* állapotban van:
    
    ![](./media/virtual-network-routes-troubleshoot-portal/image4.png)

    A kétirányú kapcsolat számára a peering megszakad, amely megtudhatja, miért VM1 nem lehet kapcsolódni a *WestUS-VNet3* VNet a VM3.

8. A következő képen a leveleket a kétirányú peering hivatkozás létrehozása után:

    ![](./media/virtual-network-routes-troubleshoot-portal/image5.png)

További hibaelhárítási esetek kényszerített tunneling és útvonal kiértékelési olvassa el a Ez a cikk a [szempontok](virtual-network-routes-troubleshoot-portal.md#Considerations) című szakaszát.

### <a name="view-effective-routes-for-a-network-interface"></a>A hálózati kapcsolat hatékony útvonalak megtekintése

Ha egy adott hálózati kapcsolat (hálózati) hatással van a hálózati forgalmat folyamat, is megtekintheti egy teljes listát az hatékony útvonalak egy hálózati kártya közvetlenül. A hálózati kártya alkalmazott összesítő útvonalak megtekintéséhez tegye a következőket:

1. Jelentkezzen be az Azure-portált a https://portal.azure.com.
2. Kattintson a **További szolgáltatások**elemre, majd kattintson a **hálózati kapcsolatok**
3. Keresse meg a hálózati kártya nevére a listában, vagy jelölje ki a megjelenő listából. Ebben a példában **VM1-NIC1** be van jelölve.
4. **Hatékony útvonalak** válassza a **hálózati kapcsolat** lap a következő képen látható módon:
   
    ![](./media/virtual-network-routes-troubleshoot-portal/image6.png)

    A **hatókör** alapértelmezés szerint a kijelölt kapcsolat.

    ![](./media/virtual-network-routes-troubleshoot-portal/image7.png)


### <a name="view-effective-routes-for-a-route-table"></a>Az útvonal táblázat hatékony útvonalak megtekintése

Felhasználó által definiált útvonalak (UDRs) útvonal táblázatban módosításakor érdemes, tekintse át az egy adott virtuális a hozzáadott útvonalak hatását. Tetszőleges számú alhálózat útvonal táblázat társítható. Megjelenítheti a tényleges útvonalak az adott útvonalon táblázat alkalmazott, az összes NIC anélkül, hogy az adott útvonalon a táblázat lap a helyi váltás.

Ebben a példában egy UDR (*UDRoute*) útvonal táblázat (*UDRouteTable*) van megadva. Ez az útvonal *Alhalozat_1* a *WestUS-VNet1* VNet az összes internetes forgalmat a hálózati virtuális készülék (NVA), *Alhalozat_2* , az azonos VNet keresztül elküldi. Az útvonal az alábbi képen látható:

![](./media/virtual-network-routes-troubleshoot-portal/image8.png)

Az összesítő útvonalak útvonal tábla látható, hajtsa végre az alábbi lépéseket:

1. Jelentkezzen be az Azure-portált a https://portal.azure.com.
2. Kattintson a **További szolgáltatások**elemre, majd kattintson a **táblák útvonal**
3. Keresse meg a listában az összesítő útvonalak megjelenítéséhez, és jelölje ki a kívánt útvonalat táblázat. Ebben a példában **UDRouteTable** be van jelölve. Egy lap a kijelölt útvonal tábla jelenik meg, az alábbi képen látható módon:

    ![](./media/virtual-network-routes-troubleshoot-portal/image9.png)

4. Jelölje ki az **útvonal tábla** lap **Hatékony útvonalak** . A **hatókör** az útvonal táblázat, kiválasztott értékre van állítva.
5. Több alhálózat útvonal táblázat alkalmazhatók. Jelölje ki az **alhálózathoz** meg szeretné tekinteni a listából. Ebben a példában **Alhalozat_1** be van jelölve.
6. Jelölje ki a **hálózati kapcsolat**. A kijelölt alhálózat kapcsolt összes NIC sorolja fel. Ebben a példában **VM1-NIC1** be van jelölve.

    ![](./media/virtual-network-routes-troubleshoot-portal/image10.png)

    >[AZURE.NOTE] Ha a hálózati kártya nem egy futó virtuális társítva, nem hatékony útvonalak jelennek meg.

## <a name="considerations"></a>Megfontolandó szempontok

Néhány dolog, amit érdemes észben tartania, amikor útvonalak listájának áttekintése vissza.

- A leghosszabb előtag hol.van (LPM) között UDRs, továbbítás alapul BGP és a rendszer útvonalak. Ha egynél több útvonalat az azonos LPM egyezést, akkor útvonal lesz kijelölve alapján az origin a következő sorrendben:
    - Felhasználó által definiált továbbítására
    - BGP továbbítására
    - A rendszer (alapértelmezett) továbbítására

    Hatékony útvonalak csak megtekintheti az összes lehetséges útvonal alapján LPM egyező hatékony útvonalak. Hogyan útvonalak ténylegesen kiértékelt, üres egy adott hálózati kártya megjelenítésével így adott utakat, előfordulhat, hogy a virtuális/közötti kapcsolatot kell érintő hibák elhárítása a műveletet egyszerűbbé.

- Ha UDRs és forgalmat a hálózati virtuális a készülék (NVA), a *VirtualAppliance* **nextHopType**, küld, győződjön meg arról, hogy IP-továbbítás engedélyezve van-e a NVA a forgalom fogadása vagy csomagok megszakadnak. 
- Kényszerített tunneling engedélyezve van, ha az összes kimenő internetes forgalmat a helyszíni továbbítja. Ezt a beállítást, attól függően, hogy a forgalmat a helyszíni kezelésének RDP/SSH az internetről szeretne a virtuális nem működik. 
  Kényszerített tunneling engedélyezése:
    - Webhely VPN, használata nextHopType VPN-átjárót, mint a felhasználó által definiált útvonal (UDR) beállításával
    - Ha az alapértelmezett útvonal van közzététel BGP keresztül
- VNet peering forgalmának megfelelően működjön a rendszer útvonal **nextHopType** *VNetPeering* peered VNet előtag tartomány léteznie kell. Ha például egy útvonal nem létezik, és a VNet peering hivatkozás néz ki az OK gombra:
    - Várjon néhány másodpercet, és próbálkozzon újra, ha meg-e egy újonnan létrehozott peering hivatkozást. Időnként hosszabb ideig tart propagálása útvonalak alhálózat a hálózati kapcsolatokat.
    - Hálózati biztonsági csoport (NSG) szabályok a forgalom érintő is lehet. További információt a [Hálózat biztonsági csoportok – Problémamegoldás](virtual-network-nsg-troubleshoot-portal.md) témakört is.
