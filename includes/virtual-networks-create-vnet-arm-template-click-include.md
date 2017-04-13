## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>ARM sablon üzembe telepítéséhez kattintson használatával

Előre definiált ARM sablonok feltöltése a Microsoft által fenntartott github tárházba újrafelhasználása, és nyissa meg a Közösség. Ezek a sablonok rendszerbe egyenes kívül github, vagy letölthető és módosíthatók úgy, hogy felel meg az igényeinek. Egy sablont, amely a két alhálózat létrehoz egy VNet telepítéséhez kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Görgessen lefelé a listában, sablonok, majd kattintson a **101-vnet-két-alhálózat**. Jelölje be a **README.md** fájlt, alább látható módon.

    ![Github READEME.md fájl](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Kattintson a **telepítse az Azure**. Ha szükséges, adja meg a Azure bejelentkezési hitelesítő adatait. 
4. A **Paraméterek** lap adja meg az új VNet létrehozásához használni kívánt értékeket, és kattintson **az OK**gombra. Az alábbi ábrán a forgatókönyvet az értékeket.

    ![ARM-sablon paraméterei](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

4. **Erőforráscsoport** gombra, és jelöljön ki egy erőforrás csoportot a VNet szeretne hozzáadni, vagy **Új létrehozása** új erőforráscsoport a VNet hozzáadása gombra. Az alábbi ábrán az erőforrás csoportbeállítások **TestRG**nevű új erőforráscsoport.

    ![Erőforráscsoport](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

5. Ha szükséges, a VNet **helyét** és az **előfizetés** beállításainak módosítása.
6. Ha nem szeretné a VNet látható a **Startboard**a mozaik, tiltsa le a **Startboard PIN-kódot**.
5. Kattintson **Leagl kifejezéseket**, olvassa el a feltételeket, majd a **vásárlása** azzal. 
6. Kattintson a **Create** a VNet létrehozásához.

    ![Az előnézet portálon küldő telepítési csempe](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

7. Miután a telepítés befejeződött, kattintson a **TestVNet** > **minden elérhető beállítás** > **alhálózat** alhálózat tulajdonságainak megjelenítéséhez az alább látható módon.

    ![Hozzon létre VNet megtekintése a portálon](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.gif)