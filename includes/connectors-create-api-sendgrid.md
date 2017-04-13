### <a name="prerequisites"></a>Előfeltételek
- Egy [SendGrid](https://www.SendGrid.com/) fiók 

Mielőtt használatba vehetné a SendGrid fiók összefüggés-alkalmazásban, engedélyeznie kell a logika alkalmazást a SendGrid fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon. 

Az a SendGrid fiókhoz való csatlakozáshoz összefüggés-alkalmazás engedélyezése a lépések a következők:

1. A logika alkalmazás Tervező SendGrid, kapcsolatot létrehozni, a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be a *SendGrid* . Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
  ![SendGrid lépés: 1](./media/connectors-create-api-sendgrid/sendgrid-1.png)
2. Minden kapcsolat SendGrid előtt eddig nem hozott létre, ha fogja kiírja a SendGrid hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és elérni a SendGrid fiók adatait:  
  ![SendGrid lépés: 2](./media/connectors-create-api-sendgrid/sendgrid-2.png)
3. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logika alkalmazásban:  
  ![3 SendGrid lépés](./media/connectors-create-api-sendgrid/sendgrid-3.png)   
