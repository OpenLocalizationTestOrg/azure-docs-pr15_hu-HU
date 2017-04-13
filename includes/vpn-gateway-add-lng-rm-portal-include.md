1. Az az **összes erőforrás**-portálon kattintson a **+ Hozzáadás gombra**. A **minden** lap Keresés mezőbe írja be a **helyi hálózati átjáró**, majd kattintson a Keresés gombra. Ez ad vissza egy listában. Kattintson a **helyi hálózati átjáró** kattintva nyissa meg a lap, majd kattintson a **Create** a **Létrehozás helyi hálózaton átjáró** lap megnyitásához.

    ![helyi hálózaton átjáró létrehozása](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)

2. Kattintson a **Create a helyi hálózaton átjáró lap**adja meg a helyi hálózaton átjáró objektum **neve** .
 
3. Adjon meg egy érvényes nyilvános **IP-címet** a virtuális Magánhálózati eszköz vagy a virtuális hálózati átjáró, amelyhez szeretne csatlakozni.<br>Ha a helyi hálózat helyszíni szöggel képvisel, az a nyilvános IP-címe, amelyhez csatlakozni szeretne VPN eszköz. Nem lehet hálózati Címfordítást mögött és a működnie kell az Azure megközelíthető.<br>Ha a helyi hálózat egy másik VNet képvisel, a nyilvános IP-címet, amely a virtuális hálózati átjáró van beállítva, hogy VNet az határozza meg.<br>

4. **Címterület** hivatkozik, a hálózat, amely a helyi hálózat címtartományokhoz. Megadhatja, hogy több hely címtartományok. Győződjön meg arról, hogy az itt megadott tartományokat nem áll átfedésben más hálózatok csatlakoztatása kívánt tartománnyal.
 
5. **Előfizetés**győződjön meg arról, hogy látható-e a megfelelő előfizetést.

6. **Erőforráscsoport**jelölje ki a használni kívánt erőforráscsoport. Hozzon létre egy új erőforráscsoport, vagy jelöljön ki egy már korábban elkészült.

7. **Helyre**válassza ki azt a helyet, hogy az objektum jön létre. Jelölje ki az adott helyen található a VNet érdemes, de ezt nem kötelező.

8. Kattintson a **Create** a helyi hálózaton átjáró létrehozása.
