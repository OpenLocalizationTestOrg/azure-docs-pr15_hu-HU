## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Egy VNet létrehozása az Azure-portálon

Az Azure előzetes portálon az alkalmazási példát, fent alapján egy VNet létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson az **Új** > **hálózati** > **virtuális hálózati**, a **telepítési modell kiválasztása** listában kattintson az **Erőforrás-kezelő** , és kattintson a **Létrehozás**, az alábbi ábrán látható módon.

    ![VNet létrehozása az Azure-portálon](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. Kattintson a **virtuális hálózat létrehozása** lap beállításainak VNet az alábbi ábrán látható módon.

    ![Virtuális hálózati lap létrehozása](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. **Erőforráscsoport** gombra, és jelöljön ki egy erőforrás csoportot a VNet szeretne hozzáadni, vagy **Új létrehozása** új erőforráscsoport a VNet hozzáadása gombra. Az alábbi ábrán az erőforrás csoportbeállítások **TestRG**nevű új erőforráscsoport. Az erőforrás csoportok kapcsolatos további tudnivalókért látogassa meg [Azure erőforrás szolgáltatásának áttekintése](../articles/resource-group-overview.md#resource-groups).

    ![Erőforráscsoport](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. Ha szükséges, a VNet **helyét** és az **előfizetés** beállításainak módosítása. 

6. Ha nem szeretné a VNet látható a **Startboard**a mozaik, tiltsa le a **Startboard PIN-kódot**. 

7. Kattintson a **Létrehozás** gombra, és figyelje meg, a mozaik nevű **virtuális létrehozása hálózati** , az alábbi ábrán látható módon.

    ![Virtuális hálózati csempe létrehozása](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Várja meg a létrehozandó VNet, majd kattintson a **virtuális hálózati** lap az **összes beállítások** > **alhálózat** > alább látható módon a**Hozzáadás gombra** .

    ![Alhálózat hozzáadása az Azure-portálon](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Adja meg a *Kódmentes* alhálózat alhálózat beállításait, alább látható módon, és kattintson **az OK**gombra. 

    ![Alhálózat beállításai](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Figyelje meg a lista alhálózat, az alábbi ábrán látható módon.

    ![A VNet alhálózat listája](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)
