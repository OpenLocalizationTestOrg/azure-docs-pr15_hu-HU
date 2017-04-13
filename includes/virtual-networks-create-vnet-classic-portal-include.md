## <a name="how-to-create-a-vnet-in-the-azure-portal"></a>Egy VNet létrehozása az Azure-portálon

Egy alapján a forgatókönyv a fenti VNet létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://manage.windowsazure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Kattintson az **Új** > **Hálózati szolgáltatások** > **Virtuális hálózati** > **Egyéni létrehozása** , az alábbi ábrán látható módon.

    ![Hozzon létre VNet portálon](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure1.gif)

3. A **Virtuális hálózati részletei** lapon írja be a **nevét** a VNet, jelölje ki azt a **HELYET**, és kattintson a nyílra kattintva jelenítheti meg lépés: 2 a lap jobb alsó gombjára. Az alábbi ábrán a forgatókönyv a beállításait.

    ![Virtuális hálózati Részletek lap](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure2.png)

4. A **DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat** lapon adja meg a nevét és IP-cím használata a 9-es DNS-kiszolgálók felfelé. Ha nem adja meg a DNS-kiszolgálót, a VNet az Azure által biztosított belső elnevezési felbontás felbontás fogja használni. A példában azt nem konfigurálja a DNS-kiszolgálók.
5. Ahhoz, hogy virtuális magánhálózat pont-webhely a VNet, engedélyezze a **virtuális Magánhálózattal pont-webhely konfigurálása** jelölőnégyzetet. Ha nem adja meg a pont-webhely VPN, felveheti a VNet a létrehozása után bármikor. A példában azt nem konfigurálja a pont-webhely virtuális Magánhálózattal.
6. Ha azt szeretné, webhely virtuális Magánhálózati kapcsolatot a VNet és egy másik VNet vagy a helyszíni hálózaton közötti biztosít, engedélyezése a **konfigurálása webhely VPN** jelölőnégyzet, és megadhatja, hogy **készült ExpressRoute** vagy megjegyzést, és a hálózat nevét használatával csatlakozhat. Ha nem adja meg a webhely VPN, felveheti a VNet a létrehozása után bármikor. A példában azt nem konfigurálja a webhely virtuális Magánhálózattal.
7. Kattintson a nyílra kattintva jelenítheti meg az alábbi ábra a forgatókönyv beállításainak 3 az oldal jobb alsó.

    ![A DNS-kiszolgálók és a virtuális Magánhálózati kapcsolat lapon](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure3.png)

8. A **Virtuális hálózati cím szóközöket** lapon a **KEZDŐ IP-** *10.0.0.0* az VNet Címterület módosítása parancsát, és írja be a használni kívánt kezdő címterület használatára. Írja be a esetben *192.168.0.0*. 
9. **CIDR (cím száma)** csoportban jelölje be a alhálózat maszk eltolással. Jelölje ki az esetben a *16 (65536)*.
10. A **ALHÁLÓZAT**kattintson a *alhálózat-1* , és nevezze át az alhálózathoz, ha szükséges. A példában átnevezése *FrontEnd*.

    >[AZURE.NOTE] A név mezőben lévő értéket alhálózathoz kívülre kattint nem fogja tudni szerkeszteni a nevét, ha ismét az alhálózathoz. A javítás, hogy el kell távolítania az alhálózathoz a az attól jobbra látható X gombra kattintva, majd adja meg egy új alhálózat a 13 alatt.

11. A **KEZDŐ IP-** az első alhálózat adja meg a kezdő IP-címét az alhálózathoz. Írja be az esetben, *192.168.1.0*.
12. **CIDR (cím száma)** csoportban jelölje be az első alhálózat az alhálózathoz maszk eltolással. Jelölje ki az esetben a *24 (256)*.
13. Ha szükséges, kattintson a **alhálózat hozzáadása** egy új alhálózat hozzáadása gombra. A példában alhálózat hozzáadása, és ismételje meg a 10 – 12 konfigurálása a VNet, az alábbi ábrán látható módon.

    ![Virtuális hálózati cím szóközöket lapon](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure4.png)

14. Kattintson a jobb alsó szélén látható a lapon a VNet létrehozása a pipa gombra. Néhány másodperc után a VNet jelenik meg a listában a rendelkezésre álló VNets, az alábbi ábrán látható módon.

    ![Új virtuális hálózati](./media/virtual-networks-create-vnet-classic-portal-include/vnet-create-portal-figure5.png)