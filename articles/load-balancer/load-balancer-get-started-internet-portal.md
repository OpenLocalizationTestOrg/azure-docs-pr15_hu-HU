<properties
   pageTitle="Hozzon létre egy internetes terheléselosztó az erőforrás-kezelő az Azure portálon |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó az erőforrás-kezelő az Azure portálon"
   services="load-balancer"
   documentationCenter="na"
   authors="anavinahar"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/14/2016"
   ms.author="annahar" />

# <a name="creating-an-internet-facing-load-balancer-using-the-azure-portal"></a>Az internetes terheléselosztó az Azure portálon létrehozása

[AZURE.INCLUDE [load-balancer-get-started-internet-arm-selectors-include.md](../../includes/load-balancer-get-started-internet-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja az erőforrás-kezelő telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó klasszikus telepítési használatával](load-balancer-get-started-internet-classic-portal.md)

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

Ez lefedi való hozzon létre egy terheléselosztó és részletesen ismertetik, mit kell végrehajtania a cél elvégezheti rajta az egyedi tevékenységekhez sorozata.

## <a name="what-is-required-to-create-an-internet-facing-load-balancer"></a>Mi az hozhat létre egy internetes terheléselosztó szükséges?

Hozhat létre, és állítsa be a következő objektumoknak üzembe egy terheléselosztó szükséges.

- Előtér-IP-konfiguráció – a bejövő hálózati forgalmat a nyilvános IP-címeket tartalmazza.

- Háttéradatbázis cím alkalmazáskészlet - a virtuális gépeken futó hálózati forgalmának engedélyezésére fogadjon a terheléselosztó hálózati kapcsolatok (NIC) tartalmaz.

- Egy nyilvános portjához a terheléselosztó hozzárendelése a háttéradatbázist cím készletben port szabályok terheléselosztás szabályok - tartalmazza.

- Bejövő hálózati Címfordítást szabályok – a terheléselosztó egy nyilvános portjához hozzárendelése egy adott virtuális gép a háttéradatbázist cím készletben port szabályokat tartalmaz.

- Ellenőrzi,-állapot szondákat elérhetőségének megállapítása a háttéradatbázist cím készletben virtuális gépeken futó példányok használt tartalmazza.

Kaphat további információt a betöltés terheléselosztó összetevők Azure erőforrás-kezelővel [Azure erőforrás-kezelő támogatási terheléselosztó betöltése](load-balancer-arm.md)a.


## <a name="set-up-a-load-balancer-in-azure-portal"></a>Az Azure-portálon egy terheléselosztó beállítása

> [AZURE.IMPORTANT] Ez a példa feltételezi, hogy van-e **myVNet**nevű virtuális hálózat. Keresse meg a [virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) művelet. Azt is feltételezi, hogy nincs belüli **myVNet** nevű alhálózat **LB alhálózat lehet** és a **weben 1** és a **weben 2** témakörén belül elérhetősége mindazon című két VMs **myAvailSet** nevű **myVNet**. Olvassa el [ezt a hivatkozást](../virtual-machines/virtual-machines-windows-hero-tutorial.md) VMs létrehozásához.


1. A böngészőben nyissa meg az Azure portált: [http://portal.azure.com](http://portal.azure.com) , és jelentkezzen be az Azure-fiók.

2. A képernyő bal felső oldalán válassza az **Új** > **hálózati** > **terheléselosztó.**

3. Írja be egy nevet a terheléselosztó **létrehozása terheléselosztó** lap. Itt **myLoadBalancer**nevezik.

4. A **típus**résznél válassza a **nyilvános**lehetőséget.

5. A **nyilvános IP-cím**hozzon létre egy **myPublicIP**nevű új nyilvános IP.

6. Erőforrás csoportban jelölje be a **myRG**. Válassza ki a megfelelő **helyre**, és kattintson **az OK**gombra. A terheléselosztó majd elkezdenek bevezetését tervezi, és sikeresen végrehajtása telepítésének néhány percet fog igénybe venni.

![A terheléselosztó erőforráscsoport frissítése](./media/load-balancer-get-started-internet-portal/1-load-balancer.png)


## <a name="create-a-back-end-address-pool"></a>Háttéradatbázis cím készlet létrehozása

1. Miután sikeresen elkezdte a terheléselosztó, jelölje ki a az erőforrások belül. A beállítások csoportban jelölje be a Kódmentes készletek. Írja be a kódmentes készlet nevét. Kattintson a **Hozzáadás** gombra, amely megjelenik a lap tetejénél.

2. Kattintson **egy virtuális gép** hozzáadni a **kódmentes-készlet hozzáadása** lap.  Jelölje ki a **Válasszon egy elérhetősége** **elérhetőségének beállítása** csoportban, és válassza ki a **myAvailSet**. Ezután jelölje ki a **Válassza ki a virtuális gépeken futó** virtuális gépeken futó csoportjában kattintson a lap, és a **weben 1** és a **weben 2**, a terheléselosztás létrehozott két VMs gombra. Gondoskodhat arról, hogy mindkét kék pipák balra az alábbi képen látható módon. Kattintson a **Jelölje ki** a, majd az OK gombra a **Válassza ki a virtuális gépeken futó** lap a, majd **az OK gombra** a **kódmentes-készlet hozzáadása** lap a lap.

    ![A cím kódmentes alkalmazáskészlet - hozzáadása ](./media/load-balancer-get-started-internet-portal/3-load-balancer-backend-02.png)

3. Győződjön meg arról, hogy az értesítések legördülő lista frissítésre van mentése a betöltés terheléselosztó kódmentes készlet mellett a **weben 1** VMs és a **weben 2**a hálózati kapcsolat frissítése kapcsolatban.


## <a name="create-a-probe-lb-rule-and-nat-rules"></a>Egy vizsgálati, LB szabály és hálózati Címfordítást szabályok létrehozása

1. Hozzon létre egy állapot vizsgálati.

    A terheléselosztó a beállítások csoportban jelölje be a szondákat. Kattintson a **Hozzáadás** elemre a lap tetején található gombra.

    Állítsa be a vizsgálati kétféleképpen: HTTP vagy TCP. Ez a példa bemutatja, hogy HTTP, de a TCP hasonló módon beállíthatók.
    Frissítse a szükséges információkat. Említett, a **myLoadBalancer** fogja tölteni a 80-as Port egyenleg forgalmat. A kiválasztott elérési út HealthProbe.aspx, intervallum 15 másodperc, pedig sérült küszöb 2. Miután elkészült, kattintson az **OK gombra** a vizsgálati létrehozásához.

    Vigye az egérmutatót fölé "i" ikonra, ha többet szeretne tudni az egyes őket, és hogyan azokat módosítani lehet gondoskodjon arról, hogy a követelményeknek.

    ![Egy vizsgálati hozzáadása](./media/load-balancer-get-started-internet-portal/4-load-balancer-probes.png)

2. Hozzon létre egy betöltés terheléselosztó szabályt.

    Kattintson a terheléselosztás szabályok a terheléselosztó beállítások szakaszában. Az új lap kattintson a **Hozzáadás gombra**. A szabály neve. Ebben az esetben célszerű a HTTP. Válassza ki a frontend port és Kódmentes port. Ebben az esetben 80 mindkét választja. A Kódmentes készlet és a korábban létrehozott **HealthProbe** , mint a vizsgálati válassza a **LB-kódmentes** lehetőséget. Más konfigurációk beállítható, hogy igényeinek megfelelően. Kattintson az OK gombra a terheléselosztási szabály mentéséhez.

    ![A terheléselosztás szabály hozzáadása](./media/load-balancer-get-started-internet-portal/5-load-balancing-rules.png)

3. Hálózati Címfordítást a bejövő szabályok létrehozása

    Kattintson a hálózati címfordító Eszközig bejövő szabályok a terheléselosztó beállítások szakaszban. Az új lap, amely, kattintson a **Hozzáadás**gombra. Adja meg a bejövő hálózati Címfordítást szabály nevét. Itt **inboundNATrule1**nevezik. A cél a korábban létrehozott nyilvános IP kell lennie. Válassza az egyéni szolgáltatás csoportban, és válassza ki a használni kívánt protokollt. Az alábbi TCP be van jelölve. Írja be a port 3441, és a cél port ebben az esetben 3389. Kattintson az OK gombra kattintva mentheti a szabályt.

    Amikor az első szabály létrejött, ismételje meg ezt a második bejövő hálózati Címfordítást szabály port 3442 cél porthoz 3389 inboundNATrule2 hívását.

    ![Egy bejövő hálózati Címfordítást szabály hozzáadása](./media/load-balancer-get-started-internet-portal/6-load-balancer-inbound-nat-rules.png)

## <a name="remove-a-load-balancer"></a>Egy terheléselosztó eltávolítása

Ha törölni szeretne egy terheléselosztó, jelölje ki az eltávolítani kívánt terheléselosztó. Kattintson a *Terheléselosztó* lap, **törölje** a lap tetején található. Válassza a **Igen** .

## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-cli.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
