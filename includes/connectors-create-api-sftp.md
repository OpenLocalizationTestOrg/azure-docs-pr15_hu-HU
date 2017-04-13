### <a name="prerequisites"></a>Előfeltételek

- [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol) fiók  


Mielőtt használatba vehetné a SFTP fiók összefüggés-alkalmazásban, engedélyeznie kell a logika alkalmazást a SFTP fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon.  

Az a SFTP fiókhoz való csatlakozáshoz összefüggés-alkalmazás engedélyezése a lépések a következők:  
1. A logika alkalmazás Tervező SFTP, kapcsolatot létrehozni, a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd a Keresés mezőbe írja be a *SFTP* . Jelölje ki a **SFTP – Ha a fájl hozzáadása vagy módosítása** az eseményindító:  
![SFTP online kapcsolat kép 1](./media/connectors-create-api-sftp/sftp-1.png)  
2. Minden kapcsolat SFTP előtt eddig nem hozott létre, ha fogja kiírja a SFTP hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és elérni a SFTP fiók adatait:  
![SFTP online kapcsolat kép 2](./media/connectors-create-api-sftp/sftp-2.png)  
3. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logika alkalmazásban:   
 ![SFTP online kapcsolat kép 3](./media/connectors-create-api-sftp/sftp-3.png) 
