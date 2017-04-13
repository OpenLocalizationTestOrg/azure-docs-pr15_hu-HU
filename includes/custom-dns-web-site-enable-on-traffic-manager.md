A tartománynév rekordjainak propagált van, miután kell annak ellenőrzése, hogy használható-e az egyéni tartománynevet a web app alkalmazás Azure szolgáltatásban elérhető a böngésző használatával.

> [AZURE.NOTE] Eltarthat egy kis időt, a szülőtől megtörtént a DNS-rendszerben CNAME. Ellenőrizze, hogy a CNAME elérhető egy szolgáltatásba, például <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> is használhatja.

Ha nem már hozzáadta a web App alkalmazásban a forgalom Manager végpontjának, el kell végeznie ezt névfeloldás működnek, mint az egyéni tartomány neve útvonalak forgalom Manager előtt. Adatforgalom Manager majd irányítja a web App alkalmazásban. Az információk segítségével [hozzáadása vagy törlése a végpontok](../articles/traffic-manager/traffic-manager-endpoints.md) a forgalom Manager profilhoz zárólap adja hozzá a web App alkalmazásban.

> [AZURE.NOTE] A web App alkalmazásban nem szerepel a végpont hozzáadása, ha ellenőrizze, hogy van-e beállítva a **szabványos** alkalmazás szolgáltatás terv üzemmód-e. Annak érdekében, hogy a forgalmat Manager használata a webalkalmazás **szabványos** üzemmódban kell használnia.

1. A böngészőben nyissa meg az [Azure-portálon](https://portal.azure.com).

1. A **Web Apps alkalmazások** lapon kattintson a nevére a web App alkalmazásban, válassza a **Beállítások**, és válassza az **egyéni tartományok**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. Kattintson az **egyéni tartományok** lap **hozzáadása hostname (állomásnév)**.
    
1. A **Hostname (állomásnév)** mezőbe írja be a forgalom Manager tartománynevet szeretne társítani a web app használatával

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Kattintson a **érvényesítése** gombjára kattintva mentse a tartomány beállítását.

7.  **Érvényesítése** Azure kattintás után a program szolgáltatás tartomány hitelesítése a munkafolyamat elindításához. Ez ellenőrzi, hogy a tartomány tulajdonjogát, valamint a hostname (állomásnév) rendelkezésre állásának és a jelentés sikeres vagy a elfogadott guidence olvashat a hiba kijavításához a hiba részletes.    

8.  Sikeres érvényességi **hozzáadása hostname (állomásnév)** után gomb aktívvá válik, és meg tudják a hozzárendelése hostname (állomásnév). Most már az egyéni tartománynevet a böngészőben nyissa meg. Ekkor megjelenik az alkalmazás megkezdése a saját tartománynév. 

    Konfigurációs befejeződése után az egyéni tartomány neve megjelenik a webalkalmazás **tartománynevek** szakaszában.

Ezen a ponton látnia kell a böngészőben adja meg a forgalom Manager tartománynevet, és látható, hogy sikeresen szükséges időt, a web App alkalmazásban.
