### <a name="prerequisites"></a>Előfeltételek

- [Az Office 365-felhasználóknak](https://office365.com) fiók  


Az Office 365-felhasználóknak fiók egy logikai alkalmazásban használata előtt engedélyeznie kell a logika alkalmazást az Office 365-felhasználóknak fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon.  

A lépések az Office 365-felhasználóknak fiókhoz való csatlakozáshoz az összefüggés-alkalmazás engedélyezése a következők:  
1. Az Office 365-felhasználóknak kapcsolat létrehozása a logika alkalmazás tervezőben, a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be *Az Office 365-felhasználóknak* . Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
![Az Office 365 felhasználói kapcsolat létrehozása lépés](./media/connectors-create-api-office365users/office365users-1.png)  
2. Ha az Office 365-felhasználók előtt bármely kapcsolatok eddig nem hozott létre, fog kiírja az Office 365-felhasználók hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és az Office 365-felhasználóknak partneradatokat elérése:  
![Az Office 365 felhasználói kapcsolat létrehozása lépés](./media/connectors-create-api-office365users/office365users-2.png)  
3. Adja meg az Office 365-felhasználóknak felhasználónevét és jelszavát a logikájának app engedélyezése:  
 ![Az Office 365 felhasználói kapcsolat létrehozása lépés](./media/connectors-create-api-office365users/office365users-3.png)  
4. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logikájának alkalmazásban:  
![Az Office 365 felhasználói kapcsolat létrehozása lépés](./media/connectors-create-api-office365users/office365users-4.png)  
  