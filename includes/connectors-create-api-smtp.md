### <a name="prerequisites"></a>Előfeltételek

- Egy [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol) -fiókkal  


SMTP-fiókja egy logikai alkalmazásban használata előtt engedélyeznie kell a logika alkalmazást, az SMTP-fiókhoz való csatlakozáshoz. Szerencsére ezt megteheti a egyszerűen a logika alkalmazásból, az Azure-portálon.  

Az összefüggés-alkalmazást, az SMTP-fiókhoz való csatlakozáshoz engedélyezése a lépések a következők:  
1. A logikai alkalmazás Tervező SMTP, kapcsolat létrehozásához a legördülő listában válassza a **Microsoft megjelenítése felügyelt API-k** , majd adja meg az *SMTP* a Keresés mezőbe. Jelölje ki az eseményindító vagy művelet fogja használni kívánt:  
![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Ha még nem hozott létre bármely előtt SMTP-kapcsolatot, fog kiírja az SMTP-hitelesítő adatait. Ezek a hitelesítő adatok engedélyezheti a logika alkalmazás való csatlakozáshoz használt, és az SMTP-fiók adatait elérni:  
![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Értesítés a kapcsolat létrejött, és most szabadon folytassa a többi a logika alkalmazásban:  
 ![](./media/connectors-create-api-smtp/smtp-3.png)  

