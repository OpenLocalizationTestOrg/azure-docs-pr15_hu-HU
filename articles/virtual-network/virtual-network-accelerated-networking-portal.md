<properties 
   pageTitle="Virtuális géphez - portálon hálózati gyorsított |} Microsoft Azure"
   description="Megtudhatja, hogy miként gyorsított hálózat beállítása az Azure virtuális gép az Azure-portálon."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/26/2016"
   ms.author="jdial" />

# <a name="accelerated-networking-for-a-virtual-machine"></a>Virtuális gép gyorsított hálózatok

> [AZURE.SELECTOR]
- [Azure portál](virtual-network-accelerated-networking-portal.md)
- [A PowerShell](virtual-network-accelerated-networking-powershell.md)

Gyorsított hálózat lehetővé teszi, hogy egyetlen legfelső szintű I/O virtualizációs (SR-IOV) virtuális géphez (virtuális), nagyban javítása a hálózati teljesítményét. A nagy teljesítményű elérési út figyelmen kívül hagyja a késés, vibrálás és a legtöbb nagy hálózati a támogatott virtuális típusok terhelések kezelésére használható Processzor kihasználtsági csökkentése datapath a tároló. Ez a cikk ismerteti, hogyan gyorsított hálózat beállítása az Azure erőforrás-kezelő telepítési modell az Azure Portal segítségével. A virtuális a gyorsított hálózat Azure PowerShell használatával is létrehozhat. Megtudhatja, hogyan, kattintson a tetején látható ez a cikk a PowerShell mezőbe.

Az alábbi képen látható két virtuális gépeken futó (virtuális) és hálózati gyorsított nélkül közötti kommunikáció:

![Összehasonlítás](./media/virtual-network-accelerated-networking-portal/image1.png)

Hálózati gyorsított nélkül teljes hálózatokból forgalom és kijelentkezés a virtuális kell bejárása az állomás és a virtuális kapcsolót. A virtuális Váltás az összes házirend végrehajtás, például hálózati biztonsági csoportokat, hozzáférés-listákat, elkülönítési és más hálózati forgalmat a hálózati virtualizálva szolgáltatások biztosít. További tudnivalókért olvassa el a [Hyper-V hálózati virtualizációs és a virtuális váltás](https://technet.microsoft.com/library/jj945275.aspx) cikket.

A hálózat gyorsított hálózati forgalmat a hálózati kártya megérkezik, és továbbítja a virtuális. Az összes hálózati házirendek nélkül gyorsított hálózati alkalmazott a virtuális váltás kiürített, és alkalmazza a hardver. A hardver házirend alkalmazása lehetővé teszi, hogy közvetlenül a virtuális kihagyásával az állomás és a virtuális kapcsoló akkor alkalmazza a fogadó összes házirend megőrzésével az előre hálózati forgalmat a hálózati.

A hálózat gyorsított előnyei csak a virtuális, hogy engedélyezve van a vonatkoznak. A legjobb eredményt akkor hasznos, ahhoz, hogy kapcsolódik az azonos VNet legalább két VMs ezt a szolgáltatást. Kommunikáció több VNets és összekötő a helyszíni, ha ez a funkció a teljes időtartam a minimális hatása van.

[AZURE.INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

##<a name="benefits"></a>Legfontosabb előny

- **Alsó késés / egy újabb csomagok második (pps):** A virtuális váltás eltávolítása a datapath eltávolítja a csomagok töltött idő a fogadó házirend feldolgozásra, és megnövelheti a csomagot, amely a virtuális belül feldolgozhatók számát.
- **Vibrálás csökkentett:** Virtuális kapcsoló feldolgozás vonatkozni kell, hogy a házirend összegét és a Processzor, amely a feldolgozás tesz a terhelését függ. A szabály végrehajtására a hardver mert megszüntetéséhez adott adattartományról csomagok kézbesítése közvetlenül a virtuális az állomás virtuális kommunikáció és az összes szoftver megszakítás és a helyi kapcsolók eltávolítása.
- **Csökkent Processzor kihasználtsági:** A virtuális Váltás a fogadó kihagyásával vezet kevesebb Processzor kihasználtsági feldolgozásához hálózati forgalmának engedélyezésére.

## <a name="limitations"></a>Korlátozások

Az alábbi korlátozások vonatkoznak a használja ezt a lehetőséget, ha létezik:
 
- **Hálózati kapcsolat létrehozása:** Gyorsított hálózati csak engedélyezhető az új hálózati kapcsolat.  Egy meglévő hálózati kapcsolaton nem engedélyezett.
- **Virtuális létrehozása:** A hálózati illesztő engedélyezett gyorsított hálózattal csak kapcsolható egy virtuális a virtuális létrehozásakor. A hálózati kapcsolaton nem lehet egy meglévő virtuális csatolt.
- **Régiók:** Csak a nyugati központi US és a nyugati Europe Azure régió kínált. A régiók készlete kibonthatja a jövőben.
- **Támogatott operációs rendszer:** Microsoft Windows Server 2012 R2 és a Windows Server 2016 technikai előzetes verzió 5. Linux és a Windows Server 2012-támogatási, amint hozzáadódik.
- **Virtuális méret:** Standard_D15_v2 és Standard_DS15_v2 az egyetlen támogatott virtuális példány méretét. További információ a [Windows virtuális méretű](../virtual-machines/virtual-machines-windows-sizes.md) témakört is. A támogatott virtuális példány méretű megadása kibonthatja a jövőben.

Ezek a korlátozások módosításai keresztül az [Azure virtuális hálózati frissítések](https://azure.microsoft.com/updates/accelerated-networking-in-preview) lap jelenti be a rendszer.

## <a name="create-a-windows-vm-with-accelerated-networking"></a>Virtuális Windows gyorsított hálózattal létrehozása

1. Regisztráljon az előzetes verzió e-mailt küld a [Hálózat előfizetések gyorsított](mailto:axnpreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e) előfizetés azonosítója és rendeltetését. A hátralévő lépéseket, amíg nem teljes, miután az, hogy Ön már elfogadták az Előnézet be értesítő e-mailt kap.
2. Jelentkezzen be az Azure-portált a http://portal.azure.com.
3. Hozzon létre egy virtuális az alábbi beállítások [létrehozása a Windows virtuális](../virtual-machines/virtual-machines-windows-hero-tutorial.md) cikk lépéseinek elvégzésével:
    - Jelölje ki a korlátozások szakaszban, a jelen cikk operációs rendszer.
    - Válasszon helyet (régió) Ez a cikk a korlátozások szakaszban.
    - Válassza ki a virtuális a korlátozások szakaszban, a jelen cikk. Ha nem szerepel a támogatott méretek közül, kattintson **az összes megtekintése** a jelölje ki a méretet egy kibontott listából **Válassza ki a méret** lap.
    - A **Beállítások** lap kattintson *engedélyezve* **gyorsított hálózat**, az alábbi képen látható módon:

        ![Beállítások](./media/virtual-network-accelerated-networking-portal/image3.png)

    >[AZURE.NOTE] A hálózat gyorsított lehetőség csak akkor látható, ha az alábbiakat végezte el:
    >
    >- Az Előnézet be elfogadták
    >- Támogatott operációs rendszerre, helyét és a jelen cikk vonatkozó korlátozások című részben leírt virtuális méretű kijelölve.

5. Amikor létrejött a virtuális, töltse le a [hálózati gyorsított illesztőprogram](https://gallery.technet.microsoft.com/Azure-Accelerated-471b5d84), csatlakozhat, és jelentkezzen be a virtuális, és a Futtatás a virtuális belül az illesztőprogram telepítőt.
6. Kattintson a jobb gombbal a Windows-gombot, és kattintson az **Eszközkezelő**elemre. Ellenőrizze, hogy a **Mellanox ConnectX-3-as virtuális függvény Ethernet kártya** jelenik meg a **hálózati** megadására, ha ki van bontva, az alábbi képen látható módon:

    ![Az Eszközkezelő](./media/virtual-network-accelerated-networking-portal/image2.png)