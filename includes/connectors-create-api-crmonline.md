#### <a name="prerequisites"></a>Előfeltételek
- Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre
- A [Dynamics CRM Online](https://www.microsoft.com/en-us/dynamics/crm-free-trial-overview.aspx) számla 

A Dynamics fiókkal összefüggés-alkalmazásban, mielőtt engedélyezheti a logika alkalmazást a CRM Online fiókhoz való csatlakozáshoz. Ezt megteheti egyszerűen a logika alkalmazásból, az Azure portálon. 

Engedélyezze a logika alkalmazás CRM Online fiókját az alábbi lépésekkel csatlakozhat:

1. Hozzon létre egy logikai alkalmazást. A logikai alkalmazások tervezőben a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-hoz** , és írja be a keresőmezőbe a "dynamics". Az eseményindító vagy műveletek közül választhat:  
  ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Ha még nem korábban létrehozott minden kapcsolatok Dynamics, a program kéri a jelentkezzen be a Dynamics hitelesítő adataival:  
  ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Válassza a **Bejelentkezés**, és írja be a felhasználónevét és jelszavát. Jelölje be, **Jelentkezzen be**. 

    Ezek a hitelesítő adatok használatával engedélyezheti a logika alkalmazás csatlakozni kíván, és elérni az adatokat a Dynamics-fiókjában. 
4. Értesítés a kapcsolat létrejött. Ezután folytassa a lépéseket a logikájának alkalmazásban:  
  ![](./media/connectors-create-api-crmonline/dynamics-properties.png)
