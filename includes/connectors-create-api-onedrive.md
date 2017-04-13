#### <a name="prerequisites"></a>Előfeltételek
- Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre
- A [OneDrive](https://www.microsoft.com/store/apps/onedrive/9wzdncrfj1p3) -fiók 

Mielőtt használatba vehetné a OneDrive-fiók összefüggés-alkalmazásban, engedélyezheti a logika alkalmazást a OneDrive-fiókhoz való csatlakozáshoz.  Ezt megteheti egyszerűen a logika alkalmazásból, az Azure portálon. 

A OneDrive-fiók, az alábbi lépésekkel csatlakozhat az összefüggés-alkalmazás engedélyezése:

1. Hozzon létre egy logikai alkalmazást. A logikai alkalmazások tervezőben a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-hoz** , és írja be a keresőmezőbe a "onedrive". Az eseményindító vagy műveletek közül választhat:  
  ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Ha még nem korábban létrehozott minden olyan OneDrive-kapcsolatot, a program kéri a jelentkezzen be a onedrive-on hitelesítő adataival:  
  ![](./media/connectors-create-api-onedrive/onedrive-2.png)
3. Válassza a **Bejelentkezés**, és írja be a felhasználónevét és jelszavát. Válassza a **Bejelentkezés**:  
  ![](./media/connectors-create-api-onedrive/onedrive-3.png)   

    Ezek a hitelesítő adatok használatával engedélyezheti a logika alkalmazás kapcsolódni, és a OneDrive-fiók adatait. 
4. Válassza az **Igen gombra** kattintva engedélyezheti a logika alkalmazás OneDrive-fiók:  
  ![](./media/connectors-create-api-onedrive/onedrive-4.png)   
5. Értesítés a kapcsolat létrejött. Most folytassa a lépéseket a logika alkalmazásban:  
  ![](./media/connectors-create-api-onedrive/onedrive-5.png)
