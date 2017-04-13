#### <a name="prerequisites"></a>Előfeltételek
- Azure fiók; egy [ingyenes fiókra](https://azure.microsoft.com/free) hozhat létre
- Az [Office 365](https://office365.com) -fiókba  

Az Office 365-fiókjába a összefüggés-at használ, mielőtt engedélyezheti a logika alkalmazást az Office 365-fiókhoz való csatlakozáshoz. Ezt megteheti egyszerűen a logika alkalmazásból, az Azure portálon.  

Engedélyezze az logika alkalmazást az Office 365-fiókjába, az alábbi lépésekkel csatlakozhat:

1. Hozzon létre egy logikai alkalmazást. A logikai alkalmazások tervezőben a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-hoz** , és a Keresés mezőbe írja be "az office 365". Az eseményindító vagy műveletek közül választhat:  
    ![Az Office 365-kapcsolat létrehozása lépés](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  

2. Ha még nem korábban létrehozott bármely kapcsolatokat az Office 365-be, a program kéri a jelentkezzen be az Office 365 hitelesítő adataival:  
    ![Az Office 365-kapcsolat létrehozása lépés](./media/connectors-create-api-office365-outlook/office365-signin.png)  

3. Válassza a **Bejelentkezés**, és írja be a felhasználónevét és jelszavát. Válassza a **Bejelentkezés**:  
    ![Az Office 365-kapcsolat létrehozása lépés](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)

    Ezek a hitelesítő adatok használatával engedélyezheti a logikájának alkalmazás csatlakozni kíván, és az Office 365-fiókját. 

4. Értesítés a kapcsolat létrejött. Ezután folytassa a lépéseket a logikájának alkalmazásban:   
    ![Az Office 365-kapcsolat létrehozása lépés](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  
  