
1. A helyszíni gépen jelentkezzen be az [Azure Kezelőportálja](http://manager.windowsazure.com) (Ez a a régi portál).

2. A navigációs ablak alján válassza az **+ Új** > **Alkalmazás szolgáltatások** > **BizTalk szolgáltatás** > **Egyéni létrehozása**.

3. Adjon meg egy **BizTalk szolgáltatás nevét** , és válassza ki a egy **Edition**. 

    Ebben az oktatóanyagban **mobile1**használja. Meg kell adni egy egyedi nevet az új BizTalk szolgáltatás.

4. A BizTalk szolgáltatás létrehozását követően jelölje ki a **Hibrid kapcsolatok** fülre, majd kattintson a **Hozzáadás**gombra.

    ![Hibrid kapcsolat](./media/hybrid-connections-create-new/3.png)

    Ez a hibrid új kapcsolatot hoz létre.

5. A hibrid kapcsolat a szükséges **nevét** és **Host Name mezőbe** , és állítsa **Port** `1433`. 
  
    ![Hibrid kapcsolat beállítása](./media/hybrid-connections-create-new/4.png)

    Az állomásnév a helyszíni kiszolgáló neve. Ez beállítja a hibrid kapcsolat port 1433 futó SQL Server eléréséhez. Elnevezett SQL Server-példányt használja, ha inkább a korábban megadott statikus portot használja.

6. Után az új kapcsolat jön létre, állapotát az új kapcsolat megjelenítése a **helyszíni beállítása hiányos**.

7. Lépjen vissza a mobilszolgáltatás, kattintson a **Konfigurálás**gombra, görgessen le a **hibrid kapcsolatok** **hibrid kapcsolat hozzáadása**gombra, majd válassza ki a hibrid kapcsolatot éppen létrehozott és kattintson az **OK gombra**.

    Ezzel a mobilszolgáltatás, ha új hibrid kapcsolaton.

Ezután szüksége lesz a hibrid kapcsolatkezelő telepítse a helyi számítógépre.