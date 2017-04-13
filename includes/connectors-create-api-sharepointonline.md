

Annak érdekében, hogy a **SharePoint online-ban**szeretne csatlakozni, meg kell adnia az identitás (felhasználónév és jelszó, intelligens kártya hitelesítő adatainak stb.) a SharePoint online-bA. Amelyeket már hitelesítését követően folytathatja is használja a SharePoint Online-összekötő a logika alkalmazásban. 

A tervező a logika alkalmazás, a hajtsa végre ezeket a lépéseket követve jelentkezzen be az SharePoint használata **kapcsolat** létrehozása a logika alkalmazásban:

1. A Keresés mezőbe írja be a SharePoint, és várja meg a találatok eseményindítók és a SharePoint Online kapcsolódó műveletek:   
![A SharePoint beállítása][1]  
2. Jelölje ki a **SharePoint online-ban - fájlok létrehozásakor** eseményindító  
3. Válassza a **Bejelentkezés a SharePoint online-ban**:   
![A SharePoint beállítása][2]    
4. Adja meg a SharePoint hitelesítő adatait, és jelentkezzen be a hitelesíteni a SharePointtal   
![A SharePoint beállítása][3]     
5. A hitelesítés befejeződése után a logika alkalmazásba átirányítjuk. Ennyi az egész, a kapcsolat létrejött. Figyelje meg az üzenetet, amely azt jelzi, hogy most csatlakozik SharePoint alján.  
![A SharePoint beállítása][4]  
6. Más eseményindítók és a műveleteket, amelyeket a logika alkalmazás szükséges majd hozzá.   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png
