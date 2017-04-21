#<a name="how-to-create-a-custom-virtual-machine"></a>Hogyan hozhat létre egy egyéni virtuális számítógépre

*Egyéni* virtuális gép hoz létre, mivel lehetővé teszi, hogy a **Gyors létrehozása** módszernél további beállításainak **Gyűjtemény a** módszerrel virtuális gép hivatkozik. A beállítások között:

- További lehetőségek szeretné, hogy a kép a virtuális gép (virtuális) létrehozása
- A virtuális a virtuális hálózatokhoz történő csatlakozás
- A virtuális hozzáadása egy meglévő felhőalapú szolgáltatást
- A virtuális hozzáadása egy elérhetőségének beállítása

> [AZURE.IMPORTANT] Ha azt szeretné, hogy a virtuális gépen virtuális hálózatot használ, így csatlakozhat, közvetlenül a hostname (állomásnév) vagy a szolgáltatás keresztté helyszíni kapcsolatok beállítása úgy, győződjön meg arról, hogy a virtuális gép létrehozásakor adja meg a virtuális hálózat. A virtuális gép beállítható úgy, hogy egy virtuális hálózati csatlakozás, csak a virtuális gép létrehozásakor. Virtuális hálózatok kapcsolatos további tudnivalókért olvassa el az [Azure virtuális hálózati – áttekintés](http://go.microsoft.com/fwlink/p/?LinkID=294063)című témakört.

1. Jelentkezzen be az [Azure-portálon](http://manage.windowsazure.com).

2. A parancssávon kattintson az **Új**gombra.

3. Kattintson a **kiszámítania** **virtuális gép**kattintson, és válassza **A gyűjtemény**.

4. Válassza ki a használni kívánt képet, és kattintson a nyílra kattintva továbbra is.

5. Ha a kép több verziójának érhetők el, a **Verzió megjelenési dátuma**, válassza ki a használni kívánt verziót.

6. A **Virtuális gép neve**mezőbe írja be a nevet, amelyet a virtuális gép használni kívánt.

7. **Réteg** és a **méret** használatával jelölje ki a virtuális gép megfelelő méretet. A méretét, jelölje ki a virtuális gép, valamint az ár maximális konfigurációja hatással van. Konfigurációs részletekért olvassa el [virtuális gép és Azure felhőalapú szolgáltatás méretét](http://go.microsoft.com/fwlink/p/?LinkID=389844).

8. Az **Új felhasználó nevét**írja be egy nevet, amelyet a kiszolgáló kezelése a rendszergazdai fiók.

9. **Új jelszó**mezőbe írja be a rendszergazdai fiók erős jelszó. A **Jelszó megerősítése**mezőbe írja be ismét ugyanazt a jelszót.

10. Kattintson a nyílra kattintva továbbra is.

11. A **Felhőalapú szolgáltatásba**tegye a következők valamelyikét:

    - A felhőalapú szolgáltatást első vagy csak virtuális gép esetén válassza az **Új felhőalapú szolgáltatás létrehozása**. A **Felhőalapú szolgáltatás DNS neve**mezőbe írjon be egy nevet használó 3 24 kisbetűket és számokat között. Ez a név, amely a virtuális gép a felhőben szolgáltatáson keresztül lépjen kapcsolatba a URI részévé válik.
    - Ha a virtuális számítógéphez hozzáadott egy felhőalapú szolgáltatásba, jelölje ki a listában.

    > [AZURE.NOTE] További információt a virtuális gépeken futó elhelyezése ugyanazt a felhőalapú szolgáltatást megtudhatja, [hogy miként csatlakozás egy felhőalapú szolgáltatásba a virtuális gépeken futó](https://azure.microsoft.com/manage/windows/how-to-guides/connect-to-a-cloud-service/).

12. **Régió/affinitás csoport/virtuális hálózati**jelölje ki a régió, affinitás csoport vagy a virtuális gép használni kívánt virtuális hálózat. További információt a affinitás csoportok [kapcsolatos affinitás csoport virtuális hálózat](../virtual-network/virtual-networks-migrate-to-regional-vnet.md)megjelenítéséhez.

13. **Tárterület-fiókot**jelöljön ki egy meglévő tárterület-fiókot a virtuális fájlt, vagy automatikusan generált tárterület-fiókot használ. Csak egy tároló / régió automatikusan létrejön fiók. Minden más virtuális gépeken futó által létrehozott, ezzel a beállítással a tárhely fiókba találhatók. Korlátozódik 20 tárterület-fiókokhoz.

14. Ha azt szeretné, hogy a virtuális gépen tartoznak az elérhetőség, **Elérhetőségének beállítása**, válassza a **Create elérhetőségének beállítása** , vagy vegye fel egy meglévő elérhetőségének beállítása.

    **Megjegyzés**: az elérhetőség beállítása virtuális gépeken futó különböző hiba tartományok van telepítve. Több virtuális gépeken futó elhelyezése egy elérhetőségének beállítása segít győződjön meg róla, hogy az alkalmazás elérhető hálózati hibák, a merevlemezre hardver hibák és a bármilyen tervezett legrövidebb leállás során.

15.  Az **Végpontok**tekintse át az új végpontjait a virtuális gép, például a keresztül távoli asztal vagy egy biztonságos rendszerhéj (SSH) ügyfél kapcsolatok engedélyezése létrehozott. Is felvehet végpontok gombra, és létrehozhatja őket később. Őket később létrehozása című témakö [állítsa be végpontok virtuális géphez](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md).

16.  A **Virtuális ügynök**döntés a virtuális ügynököt. Ez az ügynök, amelyek segíthetnek kapcsolatba léphet a virtuális gépen bővítmények telepítését a környezetet biztosít. A részletekért olvassa [Bővítmények kezelése](http://go.microsoft.com/FWLink/p/?LinkID=390493).

17. Kattintson a nyílra a virtuális gép létrehozásához.

    ![A sikeres egyéni virtuális gép létrehozása](./media/howto-custom-create-vm/VMSuccessWindows.png)

##<a name="next-steps"></a>Következő lépések##
A virtuális gép létrehozását követően bekövetkezésekor automatikusan. A portál állapotjelzője futtatásával, ha akkor is jelentkezzen be a virtuális gépen. További tudnivalókért olvassa el az alábbi cikkekben:

- [Jelentkezzen be a Linux operációs rendszert futtató virtuális gép hogyan](../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md)
- [Jelentkezzen be a Windows Server operációs rendszert futtató virtuális gép hogyan](../articles/virtual-machines/virtual-machines-windows-classic-connect-logon.md)

