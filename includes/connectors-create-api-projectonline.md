### <a name="prerequisites"></a>Előfeltételek
- A [Project Online](https://products.office.com/Project/project-online-with-project-for-office-365) -fiók 

A Project online-fiók egy logikai alkalmazásban használata előtt engedélyeznie kell a logika app a Project Online-fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon. 

A logikai app a Project Online-fiókhoz való csatlakozáshoz engedélyezése a lépések a következők:

1. Project Online, kapcsolat létrehozása a logika alkalmazás tervezőben, a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be a *Project Online* . Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
  ![Project Online lépés: 1](./media/connectors-create-api-projectonline/projectonline-1.png)
2. Ha még nem hozott létre bármely előtt a Project online-kapcsolatot, fog kiírja a Project online hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és elérni a Project online-fiók adatait:  
  ![Project Online lépés: 2](./media/connectors-create-api-projectonline/projectonline-2.png)
3. Adja meg a Project Online felhasználónevét és jelszavát az összefüggés-alkalmazás engedélyezése:  
  ![Project Online lépés 3](./media/connectors-create-api-projectonline/projectonline-3.png)   
4. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logika alkalmazásban:  
  ![Project Online lépés: 4](./media/connectors-create-api-projectonline/projectonline-4.png)   
