<properties
   pageTitle="Első lépések megtételében egy belső terheléselosztó az erőforrás-kezelő az Azure portálon |} Microsoft Azure"
   description="Ismerje meg, hogy miként hozhat létre egy belső terheléselosztó az erőforrás-kezelő az Azure portálon"
   services="load-balancer"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags
   ms.service="load-balancer"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/24/2016"
   ms.author="sewhee" />

# <a name="create-an-internal-load-balancer-in-the-azure-portal"></a>Hozzon létre egy belső terheléselosztó az Azure-portálon

[AZURE.INCLUDE [load-balancer-get-started-ilb-arm-selectors-include.md](../../includes/load-balancer-get-started-ilb-arm-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][Klasszikus telepítési modell](load-balancer-get-started-ilb-classic-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Első lépések megtételében egy belső terheléselosztó Azure portál használatával

Az Azure portálról egy belső terheléselosztó létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőt, nyissa meg az [Azure-portálra](http://portal.azure.com), és jelentkezzen be az Azure-fiók.
2. Kattintson a képernyő bal felső szélén, az **Új** > **hálózati** > **terheléselosztó**.
3. **Létrehozás terheléselosztó** lap írja be egy **nevet** a terheléselosztó.
4. A **rendszer**kattintson a **belső**.
5. Kattintson a **virtuális hálózat**, és válassza ki a virtuális hálózat, amelyhez a terheléselosztó létrehozásához.

    >[AZURE.NOTE] Ha nem látható a virtuális hálózat szeretné használni, jelölje be a **hely** a terheléselosztó használ, és ennek megfelelően változik.

6. Kattintson a **alhálózat**, és válassza ki a kívánt helyre a terheléselosztó létrehozása alhálózat.
7. Az **IP-cím hozzárendelése**kattintson a **dinamikus** vagy **statikus**, attól függően, hogy szeretné-e a rögzítendő (statikus) vagy nem terheléselosztó IP-címét.

    >[AZURE.NOTE] Ha bejelöli a statikus IP-címet használja, akkor a terheléselosztó szükséges címre.

8. **Erőforráscsoport** csoportban adja meg a terheléselosztó új erőforráscsoport a nevét vagy kattintson **Jelölje ki a meglévő** és jelölje ki a meglévő erőforráscsoport.
9. Kattintson a **létrehozása**gombra.

## <a name="configure-load-balancing-rules"></a>Terheléselosztási szabályok konfigurálása

A betöltés terheléselosztó létrehozása után nyissa meg a betöltés terheléselosztó erőforrást állítja be.
Állítsa be először erőforráskészlethez tartozik háttéradatbázist címet és egy vizsgálati egy terheléselosztási szabály beállítása előtt szükség.

### <a name="step-1-configure-a-back-end-pool"></a>Lépés: 1: Állítsa be a háttéradatbázist készletben

1. Az Azure-portálon kattintson a **Tallózás** > **terheléselosztóként**, és kattintson a fentebb létrehozott terheléselosztó.
2. Kattintson a **Beállítások** lap **Kódmentes készletek**.
3. A **cím-készletek Kódmentes** lap kattintson a **Hozzáadás**gombra.
4. Kattintson a **Hozzáadás kódmentes készlet** lap adja meg az kódmentes csoport **nevét** , és kattintson **az OK**gombra.

### <a name="step-2-configure-a-probe"></a>Lépés: 2: A vizsgálati konfigurálása

1. Az Azure-portálon kattintson a **Tallózás** > **terheléselosztóként**, és kattintson a fentebb létrehozott terheléselosztó.
2. Kattintson a **Beállítások** lap **ellenőrzi**.
3. **Ellenőrzi** lap kattintson a **Hozzáadás**gombra.
4. Írja be egy **nevet** a **Hozzáadás vizsgálati** lap a vizsgálati.
5. **Protocol (protokoll)**csoportban jelölje be a **HTTP** (a webhelyek) vagy a **TCP** (más TCP-alapú alkalmazások számára) lehetőséget.
6. **Port**csoportban adja meg a port a vizsgálati elérésekor használni kívánt.
7. Az **elérési utat** (csak HTTP szondákat) adja meg az elérési útját egy vizsgálati használni.
8. **Csoportban** adja meg, hogy milyen gyakran probe az alkalmazást.
9. A **sérült küszöbérték**adja meg, hány kísérletek fejeződjön be a kódmentes virtuális gépre van-e jelölve a sérült előtt.
10. Kattintson az **OK** vizsgálati létrehozásához.

### <a name="step-3-configure-load-balancing-rules"></a>3 lépés: A terheléselosztási szabályok konfigurálása

1. Az Azure-portálon kattintson a **Tallózás** > **terheléselosztóként**, és kattintson a fentebb létrehozott terheléselosztó.
2. Kattintson a **Beállítások** lap, a **terheléselosztás szabályokat**.
3. A **terheléselosztás szabályok** lap kattintson a **Hozzáadás**gombra.
4. **Hozzáadás betöltése terheléselosztó szabály** lap írja be a szabály **nevét** .
5. **Protocol (protokoll)**csoportban jelölje be a **HTTP** (a webhelyek) vagy a **TCP** (más TCP-alapú alkalmazások számára) lehetőséget.
6. **Port**csoportban adja meg, a terheléselosztó csatlakoztatása a port ügyfelek.
7. **Kódmentes port**csoportban adja meg a port kódmentes készletben használt (a betöltés terheléselosztó port és a kódmentes port általában ugyanaz).
8. A **Kódmentes készlet**válassza a a fentebb létrehozott kódmentes készletre.
9. A **munkamenet adatmegőrzési**adja meg, hogyan tanfolyamainkat továbbra is fennáll.
10. **Üresjárati időtúllépése (perc)**csoportban adja meg, hogy az üresjárati időkorlátot.
11. Kattintson a **Szövegen kívüli IP (visszatérési közvetlen kiszolgáló)**, a **tiltott** vagy **engedélyezett**.
12. Kattintson az **OK gombra**.

## <a name="next-steps"></a>Következő lépések

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)