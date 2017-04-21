A Microsoft Azure piactéren elérhetővé teszi a népszerű web Apps alkalmazások Microsoft, külső cégek és Megnyitás szoftver kezdeményezések által fejlesztett széles köre. A Microsoft Azure piactéren lévő létrehozott Web Apps alkalmazások nem szükséges azon szoftvereket, csak az [Azure előzetes portálon](http://go.microsoft.com/fwlink/?LinkId=529715)használja a webböngésző, telepítési. 

Ebből az oktatóanyagból megismerheti:

- Bemutatja, hogyan hozhat létre egy új web App alkalmazásban, a Microsoft Azure piactéren keresztül.

- Telepítéséről a web app az Azure előzetes portálon keresztül.
 
Alapértelmezett sablon használó WordPress blog fogja összeállítása. Az alábbi ábrán látható a teljes alkalmazást:


![WordPress blog][13]

>[AZURE.NOTE] Ha azt szeretné, mielőtt feliratkozna az Azure-fiók használatbavételéhez Azure alkalmazás szolgáltatás, [Próbálja meg alkalmazás szolgáltatás](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid életű starter web app alkalmazás szolgáltatásban megnyitásához. Nem kötelező, bankkártyás fizetés Nincs nyilatkozatát.

## <a name="create-a-web-app-in-the-portal"></a>Egy webalkalmazás létrehozása a portálon

1. Jelentkezzen be az Azure előzetes portálra.

2. Nyissa meg a Microsoft Azure piactéren vagy a **piactér** ikonra kattintva:

    ![Piactér ikon][marketplace]

    Illetve az **Új** ikonra a képernyő jobb felső sarkában az irányítópulton, majd a **piactér** a bottow a lista elemre.
    
    ![Új gyorsművelet létrehozása][5]
    
3. Jelölje be a **webes + Mobile**. Keresse meg a **WordPress** , és kattintson a **WordPress** ikonra.

    ![WordPress listából][7]
    
5. Válassza a leírás WordPress alkalmazás elolvasása, után **létrehozása**lehetőséget.

6. Kattintson a **Web App alkalmazásban**, és adja meg a szükséges értékeket konfigurációs a web App alkalmazásban.
    
    ![az alkalmazás beállítása][8]

7. Kattintson a **adatbázis**, és adja meg a szükséges értékeket a MySQL-adatbázis konfigurációs. 

    ![adatbázis konfigurálása][database]

8. Nevezze el az új erőforráscsoport.

    ![Erőforrás-csoport beállítása][groupname]

8. Ha szükséges, kattintson az **ELŐFIZETÉS**elemre, és adja meg az előfizetést. 

7. Ha befejezte a web app definiálásával, kattintson a **Létrehozás**gombra, és várja meg, amíg az új webalkalmazás jön létre.

   Az alkalmazás létrehozásakor a web app és az adatbázis tartalmazó erőforráscsoport jelenik meg.

   ![Megjelenítés csoport][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>Indítsa el, és kezelheti a WordPress web App alkalmazásban
    
1. Kattintson az új webalkalmazást szeretne tudni az alkalmazás.

    ![Indítsa el az irányítópult][10]

2. A **Essentials** lapon kattintson a **Tallózás** vagy a hivatkozás nyissa meg az Üdvözöljük lapon a web app **URL-címe** alatt.

    ![webhely URL-címe][browse]

3. Ha még nem telepítette WordPress, adja meg a megfelelő konfigurációs WordPress által igényelt, és kattintson a **Telepítés WordPress** konfigurációs véglegesítése, és nyissa meg a web app bejelentkezési lapjára.

4. Kattintson a **Bejelentkezés** gombra, és írja be hitelesítő adatait.  

5. Egy új WordPress web App alkalmazásban, amely hasonlít az alábbi webalkalmazásban is.    

    ![a WordPress webhelyen][13]






[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png
