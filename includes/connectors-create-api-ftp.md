### <a name="prerequisites"></a>Előfeltételek

- Az [FTP-](https://wikipedia.org/wiki/File_Transfer_Protocol) fiókhoz  


Mielőtt használatba vehetné FTP-fiókhoz összefüggés-alkalmazásban, engedélyeznie kell a logika alkalmazást az FTP-fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logikájának alkalmazásból, az Azure-portálon.  

Az FTP-fiókhoz csatlakozhat összefüggés-alkalmazás engedélyezése a lépések a következők:  
1. A logikai alkalmazás Tervező FTP-kapcsolat létrehozásához a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd adja meg az *FTP* a Keresés mezőbe. Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
![FTP-kapcsolat létrehozása lépés](./media/connectors-create-api-ftp/ftp-1.png)  
2. Ha még nem hozott létre bármely előtt FTP-kapcsolatot, fog kiírja az FTP hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logikájának alkalmazás való csatlakozáshoz használt, és elérni a FTP-fiók adatait:  
![FTP-kapcsolat létrehozása lépés](./media/connectors-create-api-ftp/ftp-2.png)  
3. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logikájának alkalmazásban:  
 ![FTP-kapcsolat létrehozása lépés](./media/connectors-create-api-ftp/ftp-3.png)  
