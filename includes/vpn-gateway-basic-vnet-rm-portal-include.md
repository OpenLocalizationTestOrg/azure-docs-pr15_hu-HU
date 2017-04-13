Az erőforrás-kezelő telepítési modell egy VNet létrehozása az Azure portál használatával, kövesse az alábbi lépéseket. A számítógépen készített képernyőképeket példaként szolgálnak. Ügyeljen arra, hogy az értékek lecserélése saját. Virtuális hálózatok használatával kapcsolatos további tudnivalókért olvassa el a a [Virtuális hálózati – áttekintés](../articles/virtual-network/virtual-networks-overview.md)című témakört.

1. Böngészőben, nyissa meg az [Azure portálra](http://portal.azure.com) , és ha szükséges, jelentkezzen be a Azure-fiókjával.

2. Kattintson az **Új**gombra. A **Keresés a piactér** mezőbe írja be a "Virtuális hálózat". Keresse meg a **Virtuális hálózati** a visszaadott listából, és a gombra kattintva nyissa meg a **Virtuális hálózati** lap.

    ![Virtuális hálózati keresse meg az erőforrás lap] (./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Keresse meg a virtuális hálózati erőforrás lap")

3. A virtuális hálózati lap, a listából **Válasszon ki egy telepítési modellt** , alján jelölje ki az **Erőforrás-kezelő**, és kattintson a **Létrehozás**gombra.


    ![Jelölje ki az erőforrás-kezelő] (./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Jelölje ki az erőforrás-kezelő")

4. A **virtuális hálózat létrehozása** lap a VNet beállításainak. A mezők kitöltése a piros felkiáltójel jelenik meg zöld pipa lesz, amikor a mezőben megadott karakterek érvényesek.

    ![A mező érvényesítése] (./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "A mező érvényesítése")

5. A **virtuális hálózat létrehozása** lap hasonlít az alábbi példa. Előfordulhat, hogy automatikusan kitöltött értékeket. Ha igen, cserélje ki az értékeket a saját.

    ![Virtuális hálózati a Létrehozás lap] (./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Virtuális hálózati a Létrehozás lap")

6. **Név**: írja be a nevet a virtuális hálózathoz.

7. **Címterület**: Itt adhatja meg a cím térközt. Ha több címet szóközt hozzáadása, adja hozzá az első címterület használatára. További cím szóközöket később, hozzáadhatja a VNet létrehozása után.
 
8. **Alhálózat neve**: Adja hozzá a alhálózat nevét és alhálózat címtartományokat. További alhálózat később, hozzáadhatja a VNet létrehozása után.

10. **Előfizetés**: Ellenőrizze, hogy az előfizetés szerepel a listában a megfelelő fiókot. Az előfizetések módosíthatja a legördülő lista használatával.

11. **Erőforráscsoport**: jelölje be a meglévő erőforráscsoport, vagy hozzon létre egy újat, írjon be egy nevet az új erőforráscsoport. Ha egy új csoportot szeretne létrehozni, nevezze el az erőforráscsoport a tervezett konfigurációs értékek szerint. Az erőforrás csoportok kapcsolatos további tudnivalókért látogassa meg [Azure erőforrás szolgáltatásának áttekintése](resource-group-overview.md#resource-groups).

12. **Hely**: válassza ki a VNet helyét. A hely határozza meg, hogy az erőforrásokat, ez VNet rendszerbe tároló.

13. Jelölje be a **PIN-kód irányítópult** , ha szeretné engedélyezni szeretné a VNet az irányítópulton megtalálását, és kattintson a **Létrehozás**gombra.
    
    ![PIN-kód irányítópult] (./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "PIN-kód irányítópult")

14. **Létrehozása**gombra kattint, látni fogja a csempe az irányítópult, amely a VNet végrehajtásának tükrözni fogja. A mozaik változik a VNet készül.

    ![Virtuális hálózati létrehozása csempe] (./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Virtuális hálózati létrehozása csempe")