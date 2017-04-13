## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>A klasszikus VNet létrehozása az Azure-portálon

A klasszikus VNet alapján a forgatókönyv a fenti létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://portal.azure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson az **Új** > **hálózati** > **virtuális hálózati**, akkor **Válassza ki a telepítési modell** listában már látható a **Klasszikus**, és kattintson a **Létrehozás**, az alábbi ábrán látható módon.

    ![VNet létrehozása az Azure-portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)

3. A **virtuális hálózati** lap írja be a VNet **nevét** , és kattintson a **Címterület**gombra. A VNet és az első alhálózat helyet címét beállításainak konfigurálása, majd kattintson az **OK gombra**. Az alábbi ábrán a CIDR továbbfejlesztett fájlblokkolás beállításai a forgatókönyvet.

    ![A lap címét terület](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)

4. **Erőforráscsoport** kattintson, és jelöljön ki egy erőforrás csoportot a VNet szeretne hozzáadni, kattintson **az erőforrás új csoport létrehozása** a VNet hozzáadása egy új erőforráscsoport. Az alábbi ábrán az erőforrás csoportbeállítások **TestRG**nevű új erőforráscsoport. Az erőforrás csoportok kapcsolatos további tudnivalókért látogassa meg [Azure erőforrás szolgáltatásának áttekintése](../articles/virtual-network/resource-group-overview.md#resource-groups).

    ![Erőforrás-csoport lap létrehozása](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)

5. Ha szükséges, a VNet **helyét** és az **előfizetés** beállításainak módosítása. 

6. Ha nem szeretné a VNet látható a **Startboard**a mozaik, tiltsa le a **Startboard PIN-kódot**. 

7. Kattintson a **Létrehozás** gombra, és figyelje meg, a mozaik nevű **virtuális létrehozása hálózati** , az alábbi ábrán látható módon.

    ![Hozzon létre VNet portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)

8. Várja meg a létrehozandó VNet, és amikor megjelennek a lenti csempére, kattintson rá, további alhálózat hozzáadása.

    ![Hozzon létre VNet portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)

9. Meg kell jelennie a **konfigurációs** a VNet az alább látható módon. 

    ![Hozzon létre VNet portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)

10. Kattintson a **alhálózat** > **hozzáadása**, írjon be egy **nevet** , és adjon meg egy alhálózathoz **címtartományokat (CIDR blokk)** , és kattintson **az OK**gombra. Az alábbi ábrán az aktuális forgatókönyv a beállításait.

    ![VNet létrehozása az Azure-portálon](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)