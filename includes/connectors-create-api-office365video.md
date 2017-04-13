### <a name="prerequisites"></a>Előfeltételek

- Egy [Office 365 videó](https://support.office.com/article/Meet-Office-365-Video-ca1cc1a9-a615-46e1-b6a3-40dbd99939a6) -fiókkal  


Az Office 365 videó fiók egy logikai alkalmazásban használata előtt engedélyeznie kell a logika alkalmazást az Office 365 videó fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon.  

A lépések az Office 365 videó fiókhoz való csatlakozáshoz a logikájának app engedélyezése a következők:  
1. Az Office 365 videó kapcsolat létrehozása a logika alkalmazás tervezőben, a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be *Az Office 365 videó* . Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
![Az Office 365 videó kapcsolat létrehozása lépés](./media/connectors-create-api-office365video/office365video-1.png)  
2. Ha minden olyan kapcsolatot az Office 365 videó előtt eddig nem hozott létre, fog kiírja az Office 365 videó hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logikájának alkalmazás való csatlakozáshoz használt, és az Office 365 videó partneradatokat elérése:  
![Az Office 365 videó kapcsolat létrehozása lépés](./media/connectors-create-api-office365video/office365video-2.png)  
3. Adja meg a hitelesítő adatok csatlakoztatása az Office 365 videó:  
 ![Az Office 365 videó kapcsolat létrehozása lépés](./media/connectors-create-api-office365video/office365video-3.png)  
4. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logikájának alkalmazásban:  
![Az Office 365 videó kapcsolat létrehozása lépés](./media/connectors-create-api-office365video/office365video-4.png)  
