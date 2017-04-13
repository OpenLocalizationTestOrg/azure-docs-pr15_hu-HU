## <a name="how-to-create-a-vnet-using-a-network-config-file-in-the-azure-portal"></a>Hogyan hozhat létre egy VNet az Azure-portálon hálózati konfigurációs fájl használatával

Azure xml-fájlhoz használja az összes rendelkezésre álló VNets definiálásához előfizetéshez. Töltse le ezt a fájlt, és a szerkesztési engedélyeit, módosíthatja vagy törölheti a meglévő VNets, és hozzon létre új dokumentumokat. A dokumentum megtudhatja, hogy miként töltheti le a fájlt, network configuration (vagy netcgf) fájlt, a továbbiakban, és hozzon létre egy új VNet szerkesztheti. Jelölje be az [Azure virtuális hálózati konfigurációja séma](https://msdn.microsoft.com/library/azure/jj157100.aspx) további információt a hálózat konfigurációs fájl.

Az Azure portálon keresztül netcfg fájl használatával egy VNet létrehozásához kövesse az alábbi lépéseket.

1. Nyissa meg a böngészőjében nyissa meg azt a http://manage.windowsazure.com, és ha szükséges, jelentkezzen be a Azure-fiókjával.
2. Görgessen le a szolgáltatások listában, majd kattintson a **HÁLÓZATOKON** , alább látható módon.

    ![Azure virtuális hálózatok](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure1.gif)

3. A lap alján kattintson az **EXPORTÁLÁS** gombra alább látható módon.

    ![Exportálás gomb](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure2.png)

4. A **hálózati konfigurálásról exportálása** lapon jelölje ki a virtuális hálózati beállításait az exportálni kívánt előfizetést, és kattintson az oldal bal alsó sarkában kattintson a pipára.
5. A böngésző útmutatás a **NetworkConfig.xml** fájl mentéséhez. Győződjön meg arról, hogy, vegye figyelembe, ahová a fájlt menteni.
6. Nyissa meg a fájlt mentette az 5 feletti bármely XML- vagy szöveges képszerkesztő alkalmazásban, és keresse meg a **<VirtualNetworkSites>** elemet. Ha már létrehozott hálózatot, az összes olyan hálózat jelenik meg, a saját **<VirtualNetworkSite>** elemet.
7. Szeretne létrehozni a virtuális hálózat leírt ebben az esetben, adja hozzá a következő XML csak csoportban a **<VirtualNetworkSites>** elem:

        <VirtualNetworkSite name="TestVNet" Location="Central US">
          <AddressSpace>
            <AddressPrefix>192.168.0.0/16</AddressPrefix>
          </AddressSpace>
          <Subnets>
            <Subnet name="FrontEnd">
              <AddressPrefix>192.168.1.0/24</AddressPrefix>
            </Subnet>
            <Subnet name="BackEnd">
              <AddressPrefix>192.168.2.0/24</AddressPrefix>
            </Subnet>
          </Subnets>
        </VirtualNetworkSite>

8.  A hálózati konfigurációs fájl mentése
9.  Az Azure-portálon kattintson a lap bal alsó sarkában kattintson az **Új**, majd kattintson a **Hálózati szolgáltatások**, majd **Virtuális hálózati**, gombmenü **Konfiguráció IMPORTÁLÁSA** az alábbi ábrán látható módon.

    ![Konfiguráció importálása](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure3.gif)

10.  **A hálózati konfigurációs fájl importálása** lapon kattintson a **Tallózás gombra a fájl...**, majd keresse meg azt a mappát, ahová a fájlt mentette 8 a fenti lépésben, jelölje ki a fájlt, és kattintson a **Megnyitás**gombra. Az weblapon az alábbi ábrán láthatóhoz hasonlóan kell kinéznie. Jobb alsó részén a lapra kattintson a nyílra az Ugrás a következő lépéssel.

    ![Importálás hálózati konfigurációs fájl lap](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure4.png)

11.   **A hálózat létrehozása** lapon figyelje meg, a szöveg az új VNet esetében az alábbi ábrán látható módon.

    ![A hálózat lap létrehozása](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure5.png)

12.   Kattintson a jobb alsó szélén látható a lapon a VNet létrehozása a pipa gombra. Néhány másodperc után a VNet jelenik meg a listában a rendelkezésre álló VNets, az alábbi ábrán látható módon.

    ![Új virtuális hálózati](./media/virtual-networks-create-vnet-classic-portal-xml-include/vnet-create-portal-netcfg-figure6.png)