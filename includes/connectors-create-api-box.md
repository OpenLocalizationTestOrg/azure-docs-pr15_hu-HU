### <a name="prerequisites"></a>Előfeltételek

- A [párbeszédpanel](http://box.com) -fiók  


A mező fiók egy logikai alkalmazásban használata előtt engedélyeznie kell a logika alkalmazás, a mező fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon.  

A mező fiókhoz való csatlakozáshoz az összefüggés-alkalmazás engedélyezése a lépések a következők:  
1. A logika alkalmazás Tervező mezőbe kapcsolat létrehozásához a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be a *jelölőnégyzetet* . Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
![mező a kapcsolat létrehozása lépés](./media/connectors-create-api-box/box-1.png)  
2. Ha még nem hozott létre bármely kapcsolatok előtt mezőben, fog kiírja a be hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és be fiókja adatok eléréséhez:  
![mező a kapcsolat létrehozása lépés](./media/connectors-create-api-box/box-2.png)  
3. Adja meg a felhasználónév mezőbe, és a jelszavát az logikájának-alkalmazás engedélyezése:  
 ![mező a kapcsolat létrehozása lépés](./media/connectors-create-api-box/box-3.png)  
4. Kapcsolatfelvételi csatlakozhat jelölőnégyzetet engedélyezése:  
![mező a kapcsolat létrehozása lépés](./media/connectors-create-api-box/box-4.png)  
5. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logikájának alkalmazásban:  
![mező a kapcsolat létrehozása lépés](./media/connectors-create-api-box/box-5.png)  
