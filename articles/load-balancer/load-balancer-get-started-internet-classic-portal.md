
<properties
   pageTitle="Első lépések megtételében szemben lévő az Azure klasszikus portálon klasszikus telepítési modell terheléselosztó internetes |} Microsoft Azure"
   description="Megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó klasszikus telepítési modell az Azure klasszikus portál használatával"
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
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="sewhee" />

# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-classic-portal"></a>Első lépések megtételében egy internetes terheléselosztó (klasszikus) az Azure klasszikus portálon

[AZURE.INCLUDE [load-balancer-get-started-internet-classic-selectors-include.md](../../includes/load-balancer-get-started-internet-classic-selectors-include.md)]

[AZURE.INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]Ez a cikk bemutatja a klasszikus telepítési modell. Azt is [megtudhatja, hogy miként hozhat létre egy internetes terheléselosztó Azure erőforrás-kezelő használatával](load-balancer-get-started-internet-arm-ps.md).

[AZURE.INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]


## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Az internetes terheléselosztó a virtuális gépeken futó beállítása

Annak érdekében, hogy az interneten keresztül egy felhőalapú szolgáltatás virtuális gépeken futó egyenleg hálózati forgalmának engedélyezésére betöltéséhez terheléselosztás meg kell létrehoznia. Ez az eljárás feltételezi, hogy a virtuális gépeken futó már létrehozott és, hogy azok minden belül ugyanazt a felhőalapú szolgáltatást.

**A virtuális gépeken futó terheléselosztás meg konfigurálása**

1. Az Azure klasszikus portálon kattintson a **virtuális gépeken futó**, és kattintson a nevére a virtuális gép terheléselosztás megadása.

2. Kattintson a **Végpontok pontra**, és kattintson a **Hozzáadás**gombra.

3. A **virtuális géphez végpont hozzáadása** lapon kattintson a jobbra mutató nyílra.

4. A lapon **Adja meg a végpont adatait** :

    * A **név**mezőbe írjon be egy nevet a végpontot, vagy válassza ki az előre definiált végpontok közös protokollok listájából.
    * **Protocol (protokoll)**válassza ki a végpontot, TCP vagy UDP, típusú által igényelt protokollt, szükség szerint.
    * A **nyilvános és titkos portot**mezőbe írja be a portszámokat, amelyet a virtuális gépen szeretné használni, szükség szerint. Használhatja a magánjellegű portokkal és tűzfalszabályokat a virtuális gépen irányítja a forgalmat, ha szükséges, az alkalmazás. A magánjellegű port lehet ugyanaz, mint a nyilvános portot. Például a webes (HTTP) forgalmához zárólap sikerült hozzárendeli 80-as port a nyilvános és titkos portot.

5. Jelölje be **a terheléselosztás készlet létrehozása**, és kattintson a jobbra mutató nyílra.

6. A **terheléselosztás megadása** lap nevezze el a terheléselosztás beállítás, és rendelje az Azure betöltése terheléselosztó vizsgálati viselkedését értékeit. A betöltés terheléselosztó szondákat alapján határozza meg, ha elérhetők-e a virtuális gépeken futó terheléselosztás megadása bejövő forgalmat fogadni.

7. Kattintson a jelölőnégyzet be van jelölve a terheléselosztás végpont létrehozásához. Ekkor megjelenik **Igen** a **beállítása terheléselosztás név** oszlopban a virtuális gép **Végpontok** lap.

8. Az-portálon kattintson a **virtuális gépeken futó**, kattintson a nevére, egy további virtuális gép terheléselosztás megadása, kattintson a **Végpontok**, és kattintson a **Hozzáadás**gombra.

9. A **virtuális géphez végpont hozzáadása** lapon kattintson a **Hozzáadás egy meglévő terheléselosztás beállítása végpontot**, jelölje ki a nevét, a terheléselosztás készlet, és kattintson a jobbra mutató nyílra.

10. **Adja meg a végpont adatait** lapon írjon be egy nevet a végpontot, és kattintson a pipára.

A további virtuális gépeken futó terheléselosztás megadása ismételje meg az 8 – 10.



## <a name="next-steps"></a>Következő lépések

[Első lépések egy belső terheléselosztó konfigurálása](load-balancer-get-started-ilb-arm-ps.md)

[A betöltés terheléselosztó terjesztési mód konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP időtúllépés beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

