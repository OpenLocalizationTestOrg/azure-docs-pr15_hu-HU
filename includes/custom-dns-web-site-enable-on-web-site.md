Miután a tartománynév rekordjainak propagált van, társítania kell őket a Web App alkalmazásban. Ahhoz, hogy a tartománynevek webböngészőn keresztül kövesse az alábbi lépéseket.

> [AZURE.NOTE] Eltarthat egy kis időt megtörtént a DNS-rendszerben terjesztése az előző lépésekben létrehozott TXT-rekord esetében. Mindaddig, amíg a TXT rekord propagálása tartományneve nem lehet hozzáadni a web App alkalmazásban. Ha egy rekord használata esetén nem adhat rögzítése A tartománynév a web App mindaddig, amíg az előző lépésben létrehozott TXT rekord propagálása.
>
> A TXT-rekord elérhetőségének ellenőrzéséhez egy szolgáltatásba, például <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> is használhatja.

1. A böngészőben nyissa meg az [Azure-portálon](https://portal.azure.com).

2. A **Web Apps alkalmazások** lapon kattintson a nevére a web App alkalmazásban, és válassza az **egyéni tartományok**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. Kattintson az **egyéni tartományok** lap **hozzáadása hostname (állomásnév)**.
    
4. A **Hostname (állomásnév)** mezőbe írja be a tartomány nevét a web app társítása használata.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Kattintson az **érvényesítés**gombra.

7.  **Érvényesítése** Azure kattintás után a program szolgáltatás tartomány hitelesítése a munkafolyamat elindításához. Ez ellenőrzi, hogy a tartomány tulajdonjogát, valamint a hostname (állomásnév) rendelkezésre állásának és a jelentés sikeres vagy a elfogadott guidence olvashat a hiba kijavításához a hiba részletes.    

Ezen a ponton látnia kell a böngészőben adja meg az egyéni tartománynevet, és látható, hogy sikeresen szükséges időt, a web App alkalmazásban.
