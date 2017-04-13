<properties
   pageTitle="Figyelő AlwaysOn availabilty csoport létrehozása az Azure virtuális gépeken futó SQL Server"
   description="Lépésenkénti útmutatást adunk figyelő egy AlwaysOn availabilty csoport létrehozása az Azure virtuális gépeken futó SQL Server"
   services="virtual-machines"
   documentationCenter="na"
   authors="MikeRayMSFT"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows-sql-server"
   ms.workload="infrastructure-services"
   ms.date="07/12/2016"
   ms.author="MikeRayMSFT"/>

# <a name="configure-an-internal-load-balancer-for-an-alwayson-availability-group-in-azure"></a>Egy belső terheléselosztó egy AlwaysOn elérhetősége csoport konfigurálása Azure-ban

Ebből a témakörből megtudhatja, egy belső terheléselosztó egy SQL Server AlwaysOn elérhetősége csoport létrehozása az Azure virtuális gépeken futó operációs rendszert futtató erőforrás-kezelő modell. Egy AlwaysOn rendelkezésre állási csoport egy terheléselosztó szükséges, ha az SQL Server-példányok Azure virtuális gépeken. A terheléselosztó IP-címét az elérhetőség csoport figyelő tárolja. Az elérhetőség csoportban található régiók átnyúlik, ha az egyes régiókra egy terheléselosztó szükséges.

A feladat telepíthető az erőforrás-kezelő modell Azure virtuális gépeken futó SQL Server AlwaysOn elérhetősége csoport van szükség. Mindkét SQL Server virtuális gépeken futó elérhetősége mindazon kell tartoznia. A [Microsoft-sablon](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) segítségével a AlwaysOn rendelkezésre állási csoport létrehozása az Azure erőforrás-kezelő automatikusan. Ez a sablon automatikusan létrehozza a belső terheléselosztó. 

Ha inkább, azt is megteheti [kézi megadása egy AlwaysOn rendelkezésre állási csoport](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Ez a témakör igényel, hogy a availablity csoportok már vannak-e beállítva.  

Kapcsolódó témakörök tartalmaznak:

 - [Azure virtuális (grafikus) AlwaysOn rendelkezésre állási csoportok beállítása](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)   
 
 - [VNet-VNet kapcsolat beállítása erőforrás-kezelő Azure és a PowerShell használatával](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## <a name="steps"></a>Lépések

Útmutató a dokumentum alapján által, fog létrehozása és konfigurálása a terheléselosztó az Azure-portálon. Amely befejeződése után, a fürt a terheléselosztó az IP-cím használata a AlwaysOn elérhetősége csoport figyelő fog konfigurálni.

## <a name="create-and-configure-the-load-balancer-in-the-azure-portal"></a>Létrehozása és konfigurálása a terheléselosztó az Azure-portálon

Ez a tevékenység részén el fog az Azure-portálon az alábbi lépéseket:

1. A terheléselosztó létrehozása és konfigurálása az IP-cím

1. A kódmentes készlet konfigurálása

1. A vizsgálati létrehozása 

1. A terheléselosztási szabályok beállítása

>[AZURE.NOTE] Ha az SQL-kiszolgálók más erőforrás-csoportok és régióban találhatók, fog el ezeket a lépéseket minden kétszer egyszer az egyes erőforráscsoport.

## <a name="1-create-the-load-balancer-and-configure-the-ip-address"></a>1. a terheléselosztó létrehozása és konfigurálása az IP-cím

Első lépésként a terheléselosztó létrehozásához. Az Azure-portálon nyissa meg az erőforráscsoport, amely tartalmazza az SQL Server virtuális gépeken futó. Erőforrás csoportjában kattintson a **Hozzáadás**gombra.

- Keresse meg a **terheléselosztó**. A keresési eredmények közül válassza ki a **Terheléselosztó**, **a Microsoft**által kiadott.

- A **Terheléselosztó** lap kattintson a **Létrehozás**gombra.

- A **Létrehozás terheléselosztó**, állítsa be a a terheléselosztó az alábbiak szerint:

| A beállítás | Érték |
| ----- | ----- |
| **név** | A terheléselosztó jelző szöveg nevét. Ha például **sqlLB**. |
| **Séma** | **Belső** |
| **Virtuális hálózati** | Válassza ki a virtuális hálózat, amelyek az SQL-kiszolgálók.   |
| **Alhálózat**  | Válassza ki az alhálózathoz, amelyhez a rendszer az SQL-kiszolgálók. |
| **Előfizetés** | Ha több előfizetéssel rendelkeznek, ezt a mezőt jelenhet meg. Jelölje ki azt az előfizetést, amelyhez az erőforráshoz társított. Érdemes általában az azonos előfizetési mind az erőforrások elérhetősége csoport.  |
| **Erőforráscsoport** | Válassza az erőforráscsoport, amelyek az SQL-kiszolgálók. | 
| **Hely** | Válassza ki, amelyek az SQL Server Azure helyét. |

- Kattintson a **létrehozása**gombra. 

Azure hoz létre a fenti konfigurált terheléselosztó. A terheléselosztó tartozik egy adott hálózati, alhálózat, erőforráscsoport és helyét. Azure befejeződése után ellenőrizze az Azure-ban betöltés terheléselosztó beállításait. 

Most állítsa be a betöltés terheléselosztó IP-címet.  

- Kattintson a betöltés terheléselosztó a **Beállítások** lap, **IP-címet**. Az **IP-címét** a lap azt mutatja, hogy ez egy privát terheléselosztó azonos virtuális hálózaton, mint az SQL-kiszolgálókon. 

- Az alábbi beállítások megadása: 

| A beállítás | Érték |
| ----- | ----- |
| **Alhálózat** | Válassza ki az alhálózathoz, amelyhez a rendszer az SQL-kiszolgálók. |
| **Hozzárendelés** | **Statikus** |
| **IP-cím** | Írja be a alhálózat még nem használt virtuális IP-címet.  |

- A beállítások mentéséhez.

A terheléselosztó összegyűjtötte IP-címet. Rekord IP-cím. A fürt figyelő létrehozásakor, de ez IP-címet kell használni. Egy PowerShell-parancsprogramot a jelen cikk, használja a címét a `$ILBIP` változó.



## <a name="2-configure-the-backend-pool"></a>2. a kódmentes készlet beállítása

A következő lépésként kódmentes cím készlet létrehozása. Azure hívja fel a kódmentes készlet *kódmentes készlet*. Ebben az esetben a kódmentes készlet az elérhetőség csoportban a két SQL-kiszolgáló címét. 

- Az erőforrás csoportjában kattintson a létrehozott terheléselosztó. 

- A **Beállítások**csoportban kattintson a **Kódmentes készletek**.

- **Kódmentes cím készletek**kattintson a **Hozzáadás** kódmentes cím készlet létrehozása gombra. 

- **Hozzáadás kódmentes készlet** **neve**alatt írja be a kódmentes készlet nevét.

- A **virtuális gépeken futó** kattintson a **+ Hozzáadás virtuális gép**elemre. 

- Csoportban **Válassza ki a virtuális gépeken futó** kattintson **egy elérhetőségének beállítása gombot** , és adja meg, a availablity beállítása, hogy az SQL Server virtuális gépeken futó tartozik.

- Miután kiválasztotta a elérhetőségének beállítása, kattintson a **Válassza ki a virtuális gépeken futó**. Kattintson a két virtuális gépeken futó, amely a szolgáltató az SQL Server-példányok az elérhetőség csoportban. Kattintson a **Kijelölés**gombra. 

- **Az OK gombra** kattintva zárja be a rögzítéséhez **Válassza ki a virtuális gépeken futó**és **kódmentes-készlet hozzáadása**gombra. 

Azure frissíti a kódmentes cím gyűjtő beállításokat. Az elérhetőség beállítása ekkor megjelenik a két SQL-kiszolgálók erőforráskészlethez tartozik.

## <a name="3-create-a-probe"></a>3. a vizsgálati létrehozása

A következő lépésként hozzon létre egy vizsgálati. A vizsgálati határozza meg, hogyan Azure ellenőrzi az SQL-kiszolgálók mely jelenleg tulajdonosa a elérhetősége csoport figyelő. Azure fog probe a szolgáltatás IP-címet, amely csak a vizsgálati létrehozásakor port alapján.

- Kattintson a betöltés terheléselosztó **Beállítások** lap **ellenőrzi**. 

- **Ellenőrzi** lap kattintson a **Hozzáadás**gombra.

- Állítsa be a vizsgálati a **Hozzáadás vizsgálati** lap. A vizsgálati konfigurálása a következő értékeket használja:

| A beállítás | Érték |
| ----- | ----- |
| **név** | A vizsgálati jelző szöveg nevét. Ha például **SQLAlwaysOnEndPointProbe**. |
| **Protocol (protokoll)** | **TCP** |
| **Port** | Minden olyan elérhető port használhatók. Ha például *59999*.    |
| **Intervallum**  | *5* | 
| **Sérült küszöbérték**  | *2* | 

- Kattintson az **OK gombra**. 

>[AZURE.NOTE] Győződjön meg róla, hogy a megadott port nyissa meg a mindkét SQL Server tűzfalon. Mindkét kiszolgáló megkövetelése a olyan portot használt egy bejövő szabályt. [Hozzáadás vagy a tűzfalat szabály szerkesztése](http://technet.microsoft.com/library/cc753558.aspx) talál további információt. 

Azure a vizsgálati hoz létre. Azure fogja használni a vizsgálati tesztelése mely SQL Server rendelkezik a figyelő az elérhetőség csoport.

## <a name="4-set-the-load-balancing-rules"></a>4. a terheléselosztási szabályok beállítása

A terheléselosztási szabályok beállítása A betöltés terheléselosztó szabályok beállítása, hogy hogyan a terheléselosztó a forgalom átirányítása az SQL-kiszolgálók. Az e terheléselosztó közvetlen kiszolgáló visszatérési fog engedélyezése, mivel a két SQL-kiszolgálók csak az egyik minden eddiginél saját figyelő az Erőforrás elérhetősége csoport egyszerre.

- Kattintson a betöltés terheléselosztó **Beállítások** lap **terheléselosztó szabályok betöltése**. 

- A **terheléselosztás szabályok** lap kattintson a **Hozzáadás**gombra.

- A **Hozzáadás betöltése terheléselosztó szabályok** lap használatával állítsa be a szabály terheléselosztási. Használja az alábbi beállításokat: 

| A beállítás | Érték |
| ----- | ----- |
| **név** | A terheléselosztási szabályok jelző szöveg nevét. Ha például **SQLAlwaysOnEndPointListener**. |
| **Protocol (protokoll)** | **TCP** |
| **Port** | *1433*   |
| **Kódmentes Port** | *1433*. figyelje meg, hogy ez le lesz tiltva, mert ez a szabály **Lebegő IP (visszatérési közvetlen kiszolgáló)**használ.   |
| **Vizsgálati** | Használata ezt terheléselosztó az Ön által létrehozott vizsgálati nevét. |
| **Munkamenet persistance**  | **Nincs lehetőség** | 
| **Üresjárati időtúllépése (perc)**  | *4* | 
| **Lebegő IP (visszatérési közvetlen server)**  | **Engedélyezve van** | 

 >[AZURE.NOTE] Előfordulhat, hogy görgetnie kell ahhoz a a lap minden beállítás megtekintéséhez.

- Kattintson az **OK gombra**. 

- Azure konfigurálja a terheléselosztási szabályt. Most már a terheléselosztó útvonal forgalom az SQL Server az elérhetőség csoport a figyelő üzemeltető van beállítva. 

Ezen a ponton az erőforráscsoport rendelkezik egy terheléselosztó csatlakoztatása az egyik SQL Server-gépen. A terheléselosztó is tartalmaz az SQL Server AlwaysOn availablity csoport figyelő IP-címet, úgy, hogy akár gép reagálni tud a elérhetősége csoportok kérések.

>[AZURE.NOTE] Ha az SQL Server két külön régióban, ismételje meg a többi régióban. Egyes régiókra egy terheléselosztó szükséges. 

## <a name="configure-the-cluster-to-use-the-load-balancer-ip-address"></a>Állítsa be a betöltés terheléselosztó IP-címet használja a fürthöz 

A következő lépésként a figyelő konfigurálása a fürt, valamint a figyelő online állapotba. Ehhez tegye a következőket: 

1. A Feladatátvevőfürt a availablity csoport figyelő létrehozása 

1. Jelenítse meg a figyelő online

## <a name="1-create-the-availablity-group-listener-on-the-failover-cluster"></a>1. a availablity csoport figyelő létrehozása a Feladatátvevőfürt

Ebben a lépésben meg manuálisan hozzák létre az elérhetőség csoport figyelő Feladatátvevőfürt-kezelő és az SQL Server Management Studio (SSMS).

- RDP segítségével a Azure virtuális gépen, amelyen az elsődleges replika csatlakozhat. 

- Nyissa meg a Feladatátvevőfürt-kezelő.

- Jelölje ki a **hálózatok** csomópontot, és jegyezze fel a fürt hálózat nevét. Ez a név szerepel a `$ClusterNetworkName` változó a PowerShell-parancsprogramot.

- Bontsa ki a csoport nevét, és kattintson a **szerepköröket**.

- A **szerepkörök** ablakban kattintson a jobb gombbal a elérhetősége csoport nevét, és válassza a **Erőforrás hozzáadása** > **Ügyfél-hozzáférési ponthoz**.

- A **név** mezőbe hozzon létre egy nevet a új figyelő, majd kattintson kétszer a **Tovább gombra** , és kattintson a **Befejezés gombra**. Nem hozza a figyelő vagy az erőforrás online ezen a ponton.

 >[AZURE.NOTE] Az új értesülnie neve nevet a hálózatnak alkalmazások által használt SQL Server elérhetősége csoportjának adatbázisokhoz való csatlakozáshoz.

- Kattintson az **erőforrások** fülre, majd bontsa ki az imént létrehozott ügyfél-hozzáférési pont. Kattintson a jobb gombbal az IP-erőforráshoz, és válassza a Tulajdonságok parancsot. Jegyezze fel az IP-címet. Ez a név, a használandó a `$IPResourceName` változó a PowerShell-parancsprogramot.

- **IP-cím** csoportban kattintson a **Statikus IP-címet** , majd állítsa be a statikus IP-cím ugyanazt a címet használta az Azure-portálra a betöltés terheléselosztó IP-cím beállításakor. NetBIOS engedélyezése erre a címre, és kattintson az **OK gombra**. Ha a megoldás több Azure VNets fogja át, ismételje meg ezt a lépést az egyes IP-erőforrásokhoz. 

- Az elsődleges kópia jelenleg üzemeltető csomóponton nyissa meg a egy jogú PowerShell ISE, és illessze be az alábbi parancsok új parancsfájl.

        $ClusterNetworkName = "<MyClusterNetworkName>" # the cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher to find the name)
        $IPResourceName = "<IPResourceName>" # the IP Address resource name
        $ILBIP = “<X.X.X.X>” # the IP Address of the Internal Load Balancer (ILB). This is the static IP address for the load balancer you configured in the Azure portal.
    
        Import-Module FailoverClusters
    
        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

- Frissítse a változók, és futtassa az IP-cím és a port konfigurálása az új értesülnie PowerShell-parancsprogramot.

 >[AZURE.NOTE] Ha az SQL Server külön régióban, a PowerShell-parancsprogramot kétszer futtatásához szükséges. Az első alkalommal a fürt hálózat nevét, fürt IP erőforrás nevét, és a terheléselosztó IP-címe az első erőforráscsoport betöltéséhez. A második alkalommal, amikor a fürt hálózat nevét, fürt IP erőforrás neve és a terheléselosztó IP-címet a második erőforráscsoport betöltéséhez.

Ekkor a fürt megjelenik egy elérhetősége csoport figyelő erőforrás.

## <a name="2-bring-the-listener-online"></a>2. a figyelő online állapotba

Az elérhetőség csoport figyelő erőforrással beállítva áttelepítheti a figyelő online, hogy az alkalmazás elérhetősége csoportjában a figyelő az adatbázisok csatlakozhat.

- Nyissa meg újra a Feladatátvevőfürt-kezelő. Bontsa ki a **szerepkörök** , és jelölje ki az elérhetőség csoportban. Az **erőforrások** lapon kattintson a jobb gombbal a figyelő nevét, és válassza a **Tulajdonságok parancsot**.

- Kattintson a **függőségek** fülre. Több felsorolt erőforrások esetén ellenőrizze, hogy az IP-címek van-e és, függőségek. Kattintson az **OK gombra**.

- Kattintson a jobb gombbal a figyelő nevét, majd kattintson **Az Online állapotba**.


- Miután a figyelő online, az **erőforrások** fülre, kattintson a jobb gombbal az elérhetőség csoportban, és válassza a **Tulajdonságok parancsot**.

- Hozzon létre egy függőség a figyelő név erőforrás (nem az IP-címhez erőforrások mezőben). Kattintson az **OK gombra**.


- Indítsa el az SQL Server Management Studio, és csatlakoztassa az elsődleges replika.


- Nyissa meg azt a **AlwaysOn magas elérhetőség** | **rendelkezésre állási csoportok** | **rendelkezésre állási csoport hallgatók**. 


- Ekkor megjelennek a Feladatátvevőfürt-kezelő létrehozott figyelő nevét. Kattintson a jobb gombbal a figyelő nevét, és válassza a **Tulajdonságok parancsot**.


- A **Port** mezőbe írja be a portszámot az elérhetőség csoport értesülnie a korábban használt $EndpointPort használatával (1433 volt az alapértelmezett), majd kattintson az **OK gombra**.

Most már erőforrás-kezelő üzemmód futásának Azure virtuális gépeken futó SQL Server AlwaysOn elérhetőség csoport. 

## <a name="test-the-connection-to-the-listener"></a>Tesztelje a kapcsolatot a figyelő

A kapcsolat tesztelése:

1. SQL Server virtuális ugyanabba a hálózatba, de nem rendelkezik a replika RDP. Ez lehet az egyéb SQL Server a fürt.

1. **Sqlcmd** segédprogrammal tesztelje a kapcsolatot. Például a következőt egy **sqlcmd** kapcsolatot létesít az elsődleges replika a Windows-hitelesítés figyelő keresztül:

        sqlcmd -S <listenerName> -E

A SQLCMD kapcsolat automatikus csatlakozás SQL Server-példányt tartson bármelyik tárolja az elsődleges replika. 

## <a name="guidelines-and-limitations"></a>Útmutatások és korlátai

Vegye figyelembe az alábbi útmutatásokat availablity csoport figyelő használatával belső Azure-ban a terheléselosztó:

- Csak egy belső availablity csoport figyelő egy felhőalapú szolgáltatásba funkció használható, mert a figyelő úgy van konfigurálva, hogy a terheléselosztó, és csak egy belső terheléselosztó van. Azonban célszerű lehet létrehozni entitáshoz külső hallgatók. 

- Az egy belső terheléselosztó csak érheti el a figyelő a virtuális hálózaton belül.
 
