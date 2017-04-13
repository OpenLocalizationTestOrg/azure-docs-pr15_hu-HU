Helytelen vagy nem következetes konfigurációs beállítások a legtöbb az idő hitelesítési hibák eredménye. Az alábbiakban hasznos tanácsokat adott ellenőrizze.

* Győződjön meg arról, hogy nem kihagy bárhol a **Mentés** gombra. Ez gyakran egyszerűen hajtsa végre, és meg fog megtekintik helyes értékek egy portáloldalon, de azok ténylegesen az Azure környezetben vagy Azure AD-alkalmazás nem mentett eredménye.
* Konfigurált, az **Alkalmazás beállításai** lap a Azure portál beállításainak bejelölve, hogy a helyes API-alkalmazást vagy a web app volt a beállítások megadásakor volt.  Győződjön meg arról is, hogy a beállítások voltak kell megadni **alkalmazás beállításainak** és a nem **kapcsolati karakterláncot**, a két szakasz formátuma hasonló módon.
* JavaScript előtér hitelesítés, töltse le ismét annak ellenőrzésére, hogy a jegyzék `oauth2AllowImplicitFlow` sikeresen módosítva, `true`.
* Ellenőrizze, hogy HTTPS bárhol úgy állította be az URL-címeket:

    * A projekt kódot.
    * A CORS
    * Az egyes API-alkalmazás és a web app alkalmazás beállításainak Azure környezetben
    * Azure AD-alkalmazás beállításai.
    
    Ne feledje, hogy ha az API-alkalmazásokban URL-címet másolja a portálról, akkor gyakran `http://` és módosítania kell manuálisan, hogy `https://`.

* Győződjön meg arról, hogy kód módosításokat sikeres volt telepítve. Például a megoldás több projekttel is lehet módosítani szeretné a projekt kódot és véletlenül válasszon egyet a többi szeretné telepíteni a módosítás esetén.
* Győződjön meg arról, hogy fogja HTTPS URL-címek a böngészőben nem HTTP URL-ek. Alapértelmezés szerint a Visual Studio hoz profilok HTTP URL-közzététel, és mi megnyílik a böngészőben projekt telepítését követően.
* A JavaScript előtér hitelesítés győződjön meg arról, hogy CORS helyesen van beállítva az API-alkalmazás, amely a JavaScript-kód hívja meg. Ha kétségei vannak arról, hogy a probléma CORS kapcsolatos, próbálja meg a "*" engedélyezett origin URL-címével. 
* Nyissa meg a böngésző fejlesztői eszközökkel konzol fülre kattintva további hibainformációk JavaScript előtér, és vizsgálja meg a HTTP-kérelmek a hálózaton. Konzol hibaüzenetek azonban félrevezető lehet. Ha egy CORS hibát jelző üzenet jelenik meg, a tényleges probléma hitelesítési lehetnek. Ellenőrizheti, ha ez így az alkalmazás futtatásával hitelesítéssel ideiglenes átmenetileg le van tiltva.
* A .NET API-at győződjön meg arról, vannak első minél több információt az hibaüzenetek jelennek meg a lehető [customErrors mód kikapcsolása](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview)megadásával.
* .NET API-alkalmazásokból [Távoli hibakeresési munkamenet](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)indítása, és a változók szerezheti be a bearer jogkivonathoz ADAL használó kódot vagy kódot, amely ellenőrzi a szemben a várt szolgáltatás fő azonosító követelések átadott értékeinek vizsgálata Figyelje meg, hogy a kód Kiválaszthatom konfigurációs értékek számos különböző forrásból, ezért keresése érheti ezzel a módszerrel lehetséges. Tegyük fel például, hogy elgépelt `ida:ClientId` mint `ida:ClientID` Azure alkalmazás szolgáltatási környezetben beállítások megadásakor jelenhet meg a kódot a `ida:ClientId` a fájlt, az Azure alkalmazás szolgáltatás beállítást figyelmen kívül a keresett értéket. 
* Normál Internet Explorer-ablak dolog, amit nem működnek, ha egy meglévő napló beépülő modul is gátolhatják; Próbálja ki az InPrivate, és próbálja meg a Chrome vagy a Firefox böngészőt.
